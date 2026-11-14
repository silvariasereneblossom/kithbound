# 11 — Technical architecture

Status: draft spec. Crate versions are deliberately not pinned here; they are checked and pinned at milestone M0. Capacity figures are estimates until load-tested.

## 1. Language

**Rust for everything authoritative.** TypeScript for presentation only.

| Requirement | Rust | C# / .NET | Go | Elixir | C++ |
|---|---|---|---|---|---|
| Performance per dollar | Best; no GC pauses | Close; GC tuning at high tick rates | Good; GC pauses | Weak in simulation loops | Best |
| Memory safety | Yes, at compile time | Yes | Yes | Yes | No |
| Compile-time checking (matters most for agent-written code) | Strongest | Strong | Weaker | Weak | Moderate |
| Shares code with a browser client | Yes, via wasm | Possible, heavier | Poor | No | Possible |
| Proven at scale | BitCraft; Discord hot paths | Albion; Halo services (Orleans) | Many backends | Discord, chat-shaped loads | Most MMOs |

C++ is ruled out on memory safety: 66–75% of CVEs are memory-safety bugs ([01](01-research.md) §10). C# is the credible alternative and would be chosen if the client were Unity or Godot C#. Elixir was considered for chat and presence; one language is worth more to a small team than the best tool per service.

Rust's strictness is an asset for agentic implementation: an agent's mistake is far more likely to fail the build than to reach a test.

## 2. What is not used, and why

| Option | Verdict |
|---|---|
| SpacetimeDB | No. BSL licence limits production use to one instance until 2031, there is no client prediction, and action combat needs control of the tick loop |
| Full Bevy on the server | No. Breaking releases every few months. Use the standalone `bevy_ecs` crate, pinned, or `hecs` |
| Dynamic spatial partitioning (SpatialOS-style) | No. Operational cost sank several games. Fixed zones with layers instead |
| Cassandra/Scylla | Not at first. PostgreSQL holds everything until measurements say otherwise |
| Kubernetes | Not at first. Containers under a simple orchestrator on a few machines |
| Kernel anti-cheat | No. The server decides everything; there is nothing on the client worth protecting that way |

## 3. Services

```
                      ┌────────────┐
 players ──TLS────▶   │  gateway   │  WebTransport (QUIC) + WebSocket fallback
                      └─────┬──────┘  auth ticket check, rate limits, hides zone addresses
              ┌─────────────┼──────────────────┐
        ┌─────▼────┐   ┌────▼─────┐      ┌─────▼─────┐
        │   auth   │   │  world   │◀────▶│ zone × N  │  one thread per zone/instance
        └─────┬────┘   └────┬─────┘      └─────┬─────┘
              │             │    message bus   │
              │        ┌────▼──────────────────▼────┐
              └───────▶│           ledger           │  only writer of items, currency, markets
                       └────────────┬───────────────┘
                              ┌─────▼─────┐
                              │ PostgreSQL│
                              └───────────┘
```

| Service | Responsibility | State |
|---|---|---|
| `gateway` | Terminates connections; validates session tickets; per-connection rate limits; routes to zones; the only public address | None |
| `auth` | Accounts, passkeys, OIDC sign-in, device records, short-lived signed session tickets | PostgreSQL |
| `world` | Presence, parties, Companies, chat routing, Party Board, layer assignment, instance start/stop | In memory + PostgreSQL |
| `zone` | Simulation. One OS process hosts many zone and instance simulations, each on its own thread | In memory; checkpoints via ledger |
| `ledger` | Inventory, currency, markets, commissions, escrow, works accrual, housing containers | PostgreSQL |
| `admin` | Support and moderation tools; role-based, audited | PostgreSQL |

Crafting verification runs inside `zone` at first and is written as a pure library so it can move to its own stateless workers.

Messaging between services uses a bus (NATS is the working choice) with request/reply for commands and streams for events.

## 4. Workspace layout

```
crates/
  proto/        wire types, codec, versioning          (server + client wasm)
  content/      data schemas, loader, validator        (server + tools)
  sim-core/     deterministic rules: movement, skills, effects, practice
  craft-sim/    crafting minigame simulations, integer maths   (server + client wasm)
  zone/  gateway/  world/  ledger/  auth/  admin/
  client-core/  wasm: codec, prediction, interpolation, craft-sim bindings
  botclient/    headless client for integration, load, and balance tests
  xtask/        build, content compile, checks
client/         TypeScript: PixiJS world view, HTML UI
content/        data files: skills, items, recipes, zones, encounters, garments
docs/
```

## 5. Simulation and netcode

- **Tick**: 20 Hz fixed per zone, single-threaded per zone. Zones do not share memory.
- **ECS**: entities and components; systems in a fixed order. Deterministic given the same inputs and seed, which enables replay tests.
- **Client input**: intentions only — move vector, use skill with aim, interact, craft input events. Sequence-numbered.
- **Prediction**: the client predicts only its own movement and the start of its own skill animations, using the same `sim-core` movement code compiled to wasm. The server corrects; the client reconciles.
- **Other entities**: interpolated about 100 ms in the past.
- **Hit resolution**: on the server. Dodges are validated with a 150 ms grace window in the defender's favour ([04](04-combat-and-party.md)). No client-reported hits.
- **Interest management**: a grid of 16 m cells with separate enter and leave radii. A client receives only entities in its area of interest. This is also an anti-cheat measure: the client cannot reveal what it was never sent.
- **Snapshots**: delta-compressed against the last acknowledged state; unreliable datagrams. Per-client send budget with priority (party, target, nearest, then others). Reliable streams for inventory, chat, and results.
- **Overload**: a zone that cannot keep 20 Hz slows its clock (time dilation) before it drops players.

### Transport

WebTransport over QUIC gives unreliable datagrams and reliable streams from a browser. A WebSocket fallback is mandatory for networks and browsers where QUIC fails. Both carry the same `proto` messages.

### Protocol

Wire types are Rust structs in `proto`, encoded with a compact binary codec. The browser client uses the same crate compiled to wasm, so there is no hand-written second implementation to drift. Every message has a version; the gateway rejects unknown versions. The decoder is fuzzed.

## 6. Persistence and the no-dupe rule

**Rule: an item or coin exists in exactly one place, and moves only inside one database transaction performed by `ledger`.**

- Every item instance has a unique ID and exactly one container (character bag, storage, market escrow, homestead, mail).
- Every mutating request carries an **idempotency key**. Replays return the original result.
- Every change appends to a **ledger table** (who, what, from, to, why, request key). Balances are derivable from it.
- **Conservation checks** run continuously: per item class and for currency, created − destroyed = in existence. A mismatch pages a human and can trigger a trade freeze.
- **Single writer per character.** One zone holds a lease on a character, with a fencing number that increases on every hand-off. `ledger` rejects requests carrying an old number. A character cannot be live in two places.
- **Zone hand-off** is: release lease → persist → acquire lease, as one transaction. A crash at any point leaves the character in exactly one zone or in none (and then loads from the last committed state).
- **Economy state is written through**, never cached behind. Position, cooldowns, and Practice progress are written behind every few seconds and on hand-off. Losing a few seconds of position is acceptable; losing or doubling an item is not.
- **Loot** is minted by `ledger` on request from a zone, keyed by (zone, encounter, drop sequence), so a retried request cannot mint twice.

The 2021 New World gold dupe came from a transfer that committed for the receiver while the sender's state was rolled back separately ([01](01-research.md) §10). The rule above makes that class of bug structurally impossible, because there is no separate character snapshot containing wealth.

PostgreSQL holds everything at first. The ledger table is partitioned by time. Read replicas serve market search and analytics.

## 7. Content and extensibility

- All game content is **data**: skills, effects, augments, items, recipes, garments, encounters, zones. Files are text, schema-validated, and compiled into a content pack at build time.
- Skills are compositions of a fixed set of **effect primitives** (damage, apply state, move, spawn area, modify resource…). New content rarely needs new code.
- The **validator** enforces design rules mechanically: no empty ranks, telegraph minimums, Sub Aspects reference only tags, every encounter clearable by three role mixes (via the balance harness), faucet/sink annotations on every currency source.
- New features are ECS plugins behind crate boundaries. Encounter scripting starts as data-driven state machines; sandboxed scripting (wasm) is deferred until data proves too limiting.
- Content packs are versioned and hot-swappable per zone at restart.

## 8. Scaling

- Scale out by adding zone processes. `world` assigns zones and instances to hosts by load.
- Reference point: Albion reported about 15,000 concurrent players on eight ten-core servers in 2016, roughly 190 per core including all zones. Planning figure here: **150 players per core**, to be replaced by measurement at M3.
- At that figure, 1,000 concurrent players need about 8 simulation cores plus the database: one or two dedicated machines. 10,000 need a small rack's worth. These are estimates.
- Instances hibernate when empty. Automation is computed lazily ([06](06-gathering-automation.md) §7).
- `gateway`, `auth`, and `world` are horizontally scalable; `ledger` scales with PostgreSQL and is the eventual bottleneck. Per-region ledgers are the planned split.

## 9. Operations

- Structured logs, metrics, and traces from the first service. Dashboards for tick time, snapshot size, ledger latency, and conservation.
- Every zone can record inputs for deterministic replay of a bug.
- Blue/green zone restarts: drain a zone to a new layer, then stop it.
- DDoS: only `gateway` is exposed. Host it with a provider that includes UDP-aware protection ([01](01-research.md) §10).
- Backups: continuous archiving with point-in-time recovery; restores are rehearsed.

## 10. Open questions

See [DECISIONS.md](DECISIONS.md): ECS crate, message bus, hosting provider, and codec.
