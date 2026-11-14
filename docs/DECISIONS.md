# Decisions

Open owner decisions are listed as choices, with the spec's working assumption first. The specs are written as if option (a) were chosen; picking another option means updating the named files.

## Open — owner

| ID | Question | Options (a = current assumption) | Affects |
|---|---|---|---|
| D1 | Name | (a) Keep "Kithbound" as a working title · (b) choose another now | README |
| D2 | Client stack | (a) Web-first: PixiJS + HTML UI + Rust wasm · (b) Godot 4 with Rust bindings · (c) Bevy | 13, 11 |
| D3 | Business model | (a) One-time purchase plus optional pattern books · (b) optional subscription · (c) free with pattern books only · (d) decide after the slice | 07, 08 |
| D4 | PvP | (a) Duels and a small arena only · (b) none at all · (c) opt-in contested regions later | 04 |
| D5 | Art direction and sourcing | (a) Commissioned artists to a style guide, soft painterly atelier with stylised world sprites · (b) pixel art throughout · (c) other | 08, 13 |
| D6 | Homestead ownership | (a) One per account, shared by characters · (b) one per character | 09 |
| D7 | Vocation commitments | (a) 1 Primary, 2 Secondary, rest Apprentice · (b) 1 + 3 · (c) no caps | 05 |
| D8 | Launch discipline count | (a) 8 · (b) 6 · (c) 10 | 03 |
| D9 | Weapons and disciplines | (a) Each discipline has its own weapon family · (b) weapons are free choice with discipline skills adapting | 03 |
| D10 | Echo rule at quiet hours | (a) One Echo per two humans, always · (b) two Echoes when zone population is low | 04 |
| D11 | Perspective | (a) Top-down three-quarter · (b) side-scrolling · (c) isometric | 13 |
| D12 | Licence for this repository | (a) All rights reserved for now · (b) CC BY-NC-SA for docs · (c) CC BY for docs | README |
| D13 | Free-form player art (embroidery, signs) | (a) Not at launch · (b) moderated uploads at launch | 05, 09 |
| D14 | Age policy | (a) Adults only for tests; decide for launch with counsel · (b) teen rating target from the start | 12 |
| D15 | Megaserver areas at launch | (a) One · (b) three | 10, 11 |
| D16 | Generative tools in the art pipeline | (a) Not assumed · (b) allowed with human clean-up · (c) prohibited | 13 |

## Open — technical (resolved at M0 by the spec session, then recorded here)

| ID | Question | Leaning |
|---|---|---|
| T1 | ECS crate | Standalone `bevy_ecs`, pinned; `hecs` if churn is a problem |
| T2 | Wire codec | A compact binary codec with derive support and wasm compatibility |
| T3 | Message bus | NATS |
| T4 | WebTransport implementation | Evaluate current crates; WebSocket fallback regardless |
| T5 | UI component library | Smallest that gives typed components and good test tooling |
| T6 | Hosting | A provider with UDP-aware DDoS protection included |
| T7 | Rank count | 15 (5 × 3) |
| T8 | Tick rate | 20 Hz |

## Decided

| Date | Decision | Reason |
|---|---|---|
| 2026-11-14 | Rust for all authoritative code | Memory safety, performance, compile-time checking, wasm sharing ([11](11-architecture.md) §1) |
| 2026-11-14 | No SpacetimeDB, no dynamic spatial partitioning | Licence and prediction gaps; operational cost ([11](11-architecture.md) §2) |
| 2026-11-14 | All item and currency moves through one transactional ledger | Dupes become structurally impossible ([11](11-architecture.md) §6) |
| 2026-11-14 | No dailies, login rewards, gacha, or expiring content | Pillar 4 ([00](00-vision.md)) |
| 2026-11-14 | Housing supply is never finite | Pillar 5 ([09](09-housing.md)) |
| 2026-11-14 | Design specs only; no implementation until the owner says so | Owner instruction |
