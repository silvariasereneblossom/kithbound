# 15 — Scope and roadmap

Status: plan. **No implementation has started.** Milestones are ordered by risk: the things most likely to change the design come first.

## 1. The vertical slice

The first goal is a slice that exercises every risky system at small scale, playable by about 50 invited people.

| Area | Slice scope |
|---|---|
| World | One region: a town, three field zones, one Delve, one Trial |
| Disciplines | Bulwark, Ranger, Mender, to Journeyman; subs working between all three |
| Vocations | Mining, Foraging, Smithing, Tailoring, basic Tinkering |
| Wardrobe | Three frames, 60 patterns, dyes, atelier view |
| Housing | Homestead with tile building, free placement, workshop, storage, shopfront; one hamlet type |
| Automation | A0–A2: aids and works on claims, with rent and upkeep |
| Economy | Ledger, direct trade, one regional market, commissions |
| Social | Party, Party Board, Echoes, Kith, chat, block and report |
| Platform | Browser client; sign-in with passkeys; one megaserver |

## 2. Milestones

| ID | Name | Delivers | Exit gate |
|---|---|---|---|
| M0 | Foundations | Workspace, CI, `proto`, content schema and validator, `botclient` skeleton, pinned dependencies | CI green; decoder fuzz smoke test; validator rejects seeded bad content |
| M1 | Walking skeleton | `gateway`, dev sign-in, `zone` with movement, prediction, interest management; browser client shows other players | 2 browsers + 200 bots in one zone at a steady 20 Hz; first capacity reading |
| M2 | Combat core | Effects, skills, enemies, telegraphs, Guard, Concert, Practice; three disciplines; balance harness; client performance measurement | Harness within tolerance; client budget met on reference profile; **owner playtest 1 (combat feel; confirm client stack)** |
| M3 | Ledger and persistence | PostgreSQL, inventory, leases, zone hand-off, trade, conservation checks; real sign-in | Chaos test: crashes and retries never change totals; red-team bot finds no dupe; capacity figure replaces estimate |
| M4 | Crafts | `craft-sim`, Smithing, Tailoring, Mining, Foraging, trace verification, routine crafting, quality model | Server replay matches client in all test traces; **owner playtest 2 (craft feel)** |
| M5 | Avatar and wardrobe | Rig, baker, atelier, garment pipeline and validator, dyes, wardrobe UI | 60 patterns pass the validator on all frames; 40 baked characters within budget; per-garment art cost measured |
| M6 | Party and social | `world` service, parties, Board, Echoes, chat, Kith, Delve and Trial instances, sync | A four-bot party clears the Delve; **owner playtest 3 (group play with testers)** |
| M7 | Housing | Homestead instance, build mode, storage, shopfront, hamlet, tours | Placement fuzz finds no invalid state; heavy home within client budget |
| M8 | Automation and economy | Fields, surveying, claims, works, rent, market, commissions, telemetry | Economy simulation with bot agents stays within inflation bounds for a simulated month |
| M9 | Slice hardening | Security pass, accessibility pass, onboarding, operations runbooks, backups rehearsed | Outside security review; restore drill; **friends test** |

Rough dependency order: M0 → M1 → (M2 ∥ M3) → (M4 ∥ M5) → (M6 ∥ M7) → M8 → M9. Parallel pairs touch different crates.

## 3. After the slice

A decision point, not a schedule. With slice data in hand (art cost, capacity, feel, group formation), decide between:

1. Building out to launch scope (8 disciplines, 12 vocations, 600 patterns, several regions) with a real team for art, operations, and community.
2. Releasing the slice as a smaller persistent co-op game.
3. Stopping, with the research and specs public.

Launch-scope work is content and operations far more than systems, and is not a purely agentic job.

## 4. What is deliberately late

| Item | Why |
|---|---|
| Strongholds, World Hunts | Need population |
| Second Calling, Twin Calling | Need hundreds of hours of base content |
| A3–A4 automation | Needs a working multi-vocation economy |
| Music, Styling, Cooking, Alchemy, Woodworking, Fishing, Forestry | Each is its own minigame spec; the framework is proven by the slice pair |
| Phones, desktop wrapper | After the browser client is solid |
| Payments | After the business model decision |

## 5. Size expectations

Order-of-magnitude only, for planning agent work: the slice is likely 80–150 thousand lines across Rust and TypeScript, plus content data. Every milestone report states the line delta, and growth beyond expectation triggers a simplification review.
