# 12 — Security

Status: draft spec.

## 1. Stance

The client is a remote control and a display. It is assumed to be modified, scripted, replayed, and hostile. Nothing it says is believed without a check, and it is never sent anything the player should not know.

## 2. Trust boundaries

| Boundary | Rule |
|---|---|
| Internet → `gateway` | TLS only. Authenticated session ticket required before any game message. Per-connection and per-account rate limits |
| `gateway` → internal services | Mutual TLS; services accept traffic only from the bus and from each other |
| `zone` → `ledger` | Signed service identity, character lease number, idempotency key |
| Staff → `admin` | Passkey sign-in, role-based access, just-in-time elevation for sensitive actions, full audit log |
| Services → PostgreSQL | Per-service database roles with least privilege. Only `ledger` can write ownership tables |

## 3. Threats and controls

| Threat | Control |
|---|---|
| Speed, teleport, and wall hacks | Server simulates movement; inputs are move vectors, not positions. Collision is server-side |
| Cooldown and resource hacks | Cooldowns, resources, and ranges live only on the server |
| Fabricated hits or damage | The client never reports outcomes |
| Map and wallhack-style information leaks | Area-of-interest filtering; hidden state (stealthed enemies, loot rolls, craft flaws) is not sent |
| Packet replay and injection | Sequence numbers, session-bound keys, idempotency keys |
| Malformed packets | Bounded, fuzzed decoder; length and rate limits; disconnect on violation |
| Item and currency duplication | Single-transaction moves, leases with fencing, append-only ledger, conservation checks ([11](11-architecture.md) §6) |
| Race conditions in trade, mail, market | All in `ledger`, serialised per container |
| Integer overflow in quantities and prices | Checked arithmetic everywhere; bounded types in `proto` |
| Scripted crafting | Server-held seeds, trace re-simulation, plausibility scoring (§5) |
| Combat bots and farming bots | Behaviour scoring (§6); economy design ([07](07-economy.md) §6) |
| Account takeover | Passkeys first; OIDC sign-in; no password-only accounts; new-device trade holds; session revocation |
| Real-money trading and laundering | Ledger analytics; new-account trade limits; flagged off-index trades |
| Denial of service | Protected `gateway` hosting; zone addresses never exposed; connection puzzles under load; time dilation instead of collapse |
| Chat abuse, scams, phishing links | Filter, link stripping, rate limits, block and report |
| Malicious text in names and messages | Text is rendered as text. Strict Content Security Policy in the client; no HTML from the server |
| Insider abuse | Role separation, audit log, two-person rule for item grants, alerts on staff actions touching their own or linked accounts |
| Supply-chain compromise | Locked dependencies, licence and advisory checks in CI, dependency review for new crates, reproducible builds, signed releases |
| Secrets exposure | No secrets in the repository or the client. Short-lived service credentials from a secrets manager |
| Data breach | Minimal personal data; encryption at rest; separate database for identity; breach response plan |

## 4. Validation rules for every client message

1. Is the session valid and is this message type allowed in the session's current state?
2. Is it within rate limits for its type?
3. Does it parse within bounds?
4. Does the character own, see, and reach what it refers to — as known by the server?
5. Is the action legal now (cooldown, resource, state)?
6. Only then: apply it on the next tick.

A message that fails is dropped and counted. Repeated failures raise a risk score; they do not instantly ban.

## 5. Verifying skill minigames

1. On session start, the server generates the hidden state (flaws, grain, traits) from a secret seed and stores it.
2. The client receives only what the character can perceive, plus the public parameters.
3. The client runs `craft-sim` locally for immediate feedback and records a timestamped input trace.
4. The client sends the trace. The server replays it in `craft-sim` against the true hidden state. Integer maths makes the replay exact.
5. The server's result is the result. A client-claimed result is never read.
6. Session duration must be consistent with the trace and with wall-clock time on the server.
7. Traces are kept for a period for review and model training.

For phases where hidden information is revealed by interaction (striking near a flaw), the trace is sent in chunks and the server answers each with the newly perceivable state, so the client never holds the full hidden state.

## 6. Bots

Kernel anti-cheat is not used. Detection is server-side and layered:

- Input timing variance, path regularity, reaction-time distributions, and session length patterns, scored per play style.
- Ledger patterns: one-way transfers, account clusters, mule chains.
- Occasional soft challenges that are ordinary gameplay (an unusual node, a patrol that needs a response).
- Scores lead to review queues. Jagex's 2012 mass false-ban incident is the cautionary example ([01](01-research.md) §9); automated action is limited to trade holds and rate limits.
- The strongest control is economic: legitimate automation and cheap bulk remove most of what bots sell.

## 7. Accounts and privacy

- Sign-in: passkeys (WebAuthn) and OIDC providers. Recovery uses verified email plus a waiting period, with all sessions revoked.
- Personal data collected: sign-in identifier, email, country for legal purposes, payment references held by the payment provider. No real names, no birth dates beyond an age gate.
- Player-facing privacy controls: appear offline, hide from directory, restrict whispers and visits.
- Data export and deletion are built before the first public test.
- Children: an age gate and a policy decision are required before launch ([14](14-risks-and-conflicts.md)).

## 8. Moderation and staff tooling

- Every staff action is logged with actor, reason, and before/after state.
- Item restoration goes through `ledger` as a recorded grant and requires a second approver above a value threshold.
- Players can see enforcement actions on their own account and appeal.

## 9. Engineering practice

- `unsafe` Rust is forbidden outside a reviewed allow-list.
- Fuzzing for the wire decoder and content loader.
- Property tests for ledger invariants: no sequence of valid or invalid requests, crashes, and retries changes the total of anything.
- A red-team bot client that sends hostile input runs in CI against every build.
- Third-party penetration test before any public launch.
- Emergency switches: per-region trade freeze, per-feature kill switch, forced client version.

## 10. Client hardening

The client is not trusted, and it still should not be a liability to its user: strict Content Security Policy, no third-party scripts, integrity-checked assets, sign-in in a separate origin, and a minimal permission set for the desktop wrapper.
