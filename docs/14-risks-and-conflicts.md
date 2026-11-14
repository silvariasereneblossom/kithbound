# 14 — Blockers, design conflicts, resolutions, and workarounds

Status: living register. Each entry has an ID so decisions and tasks can reference it. "Fallback" is what to do if the resolution fails in testing.

## A. Blockers

These can stop the project. None is solved by writing code.

| ID | Blocker | Why it blocks | Resolution or path | Fallback |
|---|---|---|---|---|
| B1 | **Art volume** | Hundreds of garments in two views, plus hair, faces, furniture, tiles, enemies, effects. Agents cannot produce shippable illustration | Cutout rig and warp fitting cut per-garment cost ([13](13-client-and-art-pipeline.md)); pattern × fabric × dye multiplies each drawing; measure true cost on the 60-pattern slice before committing to launch scope | Smaller launch wardrobe; fewer frames; licensed base assets for non-wardrobe art |
| B2 | **Scope against team size** | An MMO is among the largest things in software. Design and code can be agent-driven; operations, art, community, and law cannot | Stage it: a vertical slice for ~50 friends, then decide. The slice is useful on its own as a proof of every risky system | Ship the slice as a small co-op game with persistent homes, without the "massively" |
| B3 | **Live operations** | A persistent world needs on-call response, backups, support, moderation, and billing, every day | Automate what can be (alerts, kill switches, restores); budget for at least part-time operations and moderation before any public test | Scheduled-hours test servers until staffing exists |
| B4 | **Population bootstrap** | A game that needs parties needs people online together. Launching thin is self-reinforcing | One megaserver per area with layers; Echoes; sync-down; a full non-combat game; scheduled community nights during tests | Allow two Echoes during measured low-population hours (C1) |
| B5 | **Legal and compliance** | Privacy law, children's data, payments and tax, terms of service, moderation duties, age ratings | Minimal data by design ([12](12-security.md) §7); engage counsel before public sign-ups; age gate decision (D14) | Invite-only adult testers under a simple agreement |
| B6 | **Feel cannot be automated** | Combat and minigames live or die on feel. Tests prove correctness, not fun | Named owner playtest gates in the roadmap; build feel-critical pieces first | Reference-game comparison sessions; outside playtesters |
| B7 | **Funding and business model** | Servers, art, and operations cost money before revenue exists. The model constrains design (C21) | Owner decision D3 before M5 | Keep the slice self-hostable at hobby cost |
| B8 | **Unmeasured capacity** | No published per-core figures for this kind of game. Cost and zone sizes are estimates | Load test with bots at M1 and M3; treat 150 per core as a hypothesis | Lower zone caps, more layers; reduce tick to 15 Hz outside combat zones |
| B9 | **Browser performance on low-end hardware** | Unproven for 40 animated layered characters plus effects | Budgets with automated measurement at M2 | Switch client to Godot (D2); the server and protocol are unaffected |
| B10 | **Content volume** | Zones, encounters, Trials (4 per discipline and vocation), recipes | Data-driven content with validators; agents draft, humans curate; small launch scope | Fewer disciplines and vocations at launch |
| B11 | **Transport maturity** | The main Rust WebTransport crate calls itself not production-ready | WebSocket fallback is first-class and tested equally; transport is behind one interface | Launch on WebSocket only; add WebTransport later |
| B12 | **Audio** | Music and sound need composers and designers; Music as a vocation needs an instrument system | Small commissioned score; notation-driven instruments synthesised on the client | Licensed library audio for the slice |
| B13 | **Name** | "Kithbound" has had a web search only | Trademark search before any public marketing | Rename; nothing in code depends on the name |
| B14 | **Bots can never be fully stopped** | Any client-visible game can be automated | Make it unprofitable and detectable ([07](07-economy.md) §6, [12](12-security.md) §6) | Tighter new-account limits; manual review capacity |

## B. Design conflicts

### Grouping

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C1 | Party-required combat **vs** players online at quiet hours, in small time zones, or with unpredictable schedules | Echoes; solo fringe; duo viability; everything non-combat is solo-friendly; party of four | Two Echoes allowed when zone population is below a threshold |
| C2 | Party-required **vs** quality of life | Party Board, Rally stones, Open Bands, layer priority. Friction stays in travel and preparation, not in finding people | Add role-flex auto-fill to the Board |
| C3 | Party-required **vs** players who are anxious about strangers | Open Bands need no conversation; phrase wheel and pings; Echoes of friends; commendations reward patience; mentors are rewarded for teaching | Tutorial-run Delves with a staff-authored Echo party |
| C4 | Echoes fill gaps **vs** Echoes replace people (the Guild Wars heroes outcome) | Echoes are capped in number, weaker at Concerts, barred from Strongholds | Lower Echo Concert strength |
| C5 | Soft roles **vs** role identity | Every discipline has a strongest role and can flex; encounters need role *functions*, not classes | Add role-specific Tenets |
| C6 | Shared credit and Open Bands **vs** leeching | Contribution threshold for credit; Practice requires the character's own skill events | Idle detection removes Band membership |
| C7 | Sync-down mentors **vs** veterans trivialising content | Ranks and gear are clamped; techniques kept; balance harness tests synced builds | Stricter clamp |
| C8 | Kith bonds **vs** cliques that exclude newcomers | Bonds give convenience only; Teaching progress rewards playing with new people | Remove bond perks |
| C9 | Trials as gates **vs** frustration at a skill wall | Free retries; Trials test the discipline's own lessons; mentors rewarded | Assist tier with reduced rewards |

### Progression

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C10 | Time-consuming **vs** not feeling like a grind | Variety pays (Freshness, Insight); no empty ranks; Trials as milestones; long arcs made of short, distinct steps | Shorten Expert and Master ranks |
| C11 | Freshness prevents repetition **vs** feeling punished for replaying what you enjoy | Floor of 0.4; presented as a bonus for fresh contexts, not a penalty; recovers within a day; a dungeon contains many contexts | Raise the floor |
| C12 | Use-based growth **vs** macros and AFK loops | Challenge is zero on trivial content; all Practice comes from server-observed events | Per-session soft caps on a single context |
| C13 | Use-based growth **vs** playing badly on purpose to train weak skills | Encounter share spreads Practice across the loadout | Raise the share |
| C14 | No vertical power growth **vs** long-term goals for veterans | Breadth: second Callings, vocations, Teaching, housing, wardrobe, Company projects, mastery of making | Occasional announced tier additions |
| C15 | Rested bonus **vs** "no time pressure" | Pool caps at about a week and never expires; no daily shape | Remove Rested |
| C16 | No dailies or expiring rewards **vs** retention and revenue norms | Accepted on purpose. Riot's 2025 data suggests the long-term trade is favourable ([01](01-research.md) §5) | None; this is a pillar |
| C17 | Deep customisation **vs** balance | Attunement and tag budgets; global stacking rules; nightly balance harness | Narrow augment forks |
| C18 | Seamless subs **vs** N² authored pairings | Sub Aspects reference tags and the resource interface only | — |
| C19 | Multiclassing possible **vs** pressure to do it | Twin Calling adds options within the same Attunement budget, not power | Remove Twin Calling; keep Second Calling |
| C20 | Attributes from skills **vs** "train everything for stats" | Only active Main, Sub, and committed vocations count | — |

### Crafting and gathering

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C21 | Deep minigames **vs** the 500th repetition | Routine crafting after a hand-made proof; batches | Raise routine quality |
| C22 | Player skill **vs** character skill | Rank sets ceiling and handling; hands fill it | Tune ceilings |
| C23 | Dexterity-based minigames **vs** accessibility | Assists trade time for dexterity without lowering ceilings; planning-heavy phases (layout, alloy, draft) reward thought over reflex | Fully turn-based mode for every phase |
| C24 | Responsive minigames **vs** server authority | Local simulation for feedback; server replays the trace; chunked reveal of hidden state | Shorter phases with a server round-trip between them |
| C25 | Analog minigames **vs** bots that play them well | Hidden server state, plausibility scoring, low payoff | Rate caps on top-grade output per account |
| C26 | Vocation commitment **vs** wanting one character to do everything | Everything is learnable to Apprentice; commitments can change on a cooldown; alts are allowed | Add a third Secondary slot |
| C27 | Interdependence **vs** a solo crafter being stuck | Markets and commissions supply other vocations' parts; NPC vendors sell floor-quality basics | Raise NPC floor quality |
| C28 | Automation **vs** gatherers' livelihood | Automated grade is capped; Prime specimens are hand-only; top tier is never automated | Lower the caps |
| C29 | Automation **vs** AFK play, inflation, bots | Upkeep, rent, field pressure, service visits, re-surveys; automation has no client input to bot | Storage caps, higher rent |
| C30 | High barrier to automate **vs** new players feeling locked out | Novice bulk automates early; new players' hands are worth the most on Prime runs | Starter rig from the tutorial |
| C31 | Automation needs many skillsets **vs** unsociable players | A1–A3 can be bought in parts on the market; A4 needs a group | Hired NPC specialists at a premium |
| C32 | Unlimited claims **vs** scarcity giving land value | Scarcity moves into yield and rent instead of space | Finite premium sites with auctions |
| C33 | Shifting resource fields **vs** stability | Shifts are announced; stored materials keep their properties | Longer shift period |

### Economy

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C34 | Gear wears out **vs** loss aversion | Slow wear; reclaiming; looks are permanent | Repair without maximum loss for low tiers |
| C35 | No gear drops **vs** the thrill of loot | Bosses drop technique fragments, Prime materials, named components, and earned looks; the crafted result carries the boss's name and the maker's mark | Rare finished "relic" drops that still need a crafter to restore |
| C36 | Regional markets **vs** convenience and thin populations | Global search; freight; regions can be merged by configuration | One market |
| C37 | Player shopfronts **vs** towns as hubs (SWG's isolated cities) | Town stalls; search shows both; Party Board, Trials, and markets are in towns | — |
| C38 | Permanent wardrobe unlocks **vs** lasting demand for tailors | Fabric properties from shifting fields; colourways; new patterns; armour-grade clothing wears | Looks tradeable after unlock |
| C39 | Crafted cosmetics **vs** selling cosmetics | Pattern books: paid looks still pass through tailors | Subscription or box price only |
| C40 | New-account trade limits **vs** honest new players | Limits lift quickly with passkey sign-in and play time; gifts from Kith are exempt up to a value | Shorter limits |

### Identity and housing

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C41 | Every garment on every body **vs** art cost | Shared world rig; warp-fitted atelier art; validator contact sheets | Fewer frames |
| C42 | Looks separate from gear **vs** reading other players' roles | Role and discipline icons on nameplates; signature skill effects are not reskinnable | — |
| C43 | Player-voted styling **vs** bias against certain bodies or skin tones | Tag score as a baseline; anonymous randomised pairs; monitor score gaps by frame and tone | Judge outfits on a neutral mannequin |
| C44 | Self-expression **vs** harassment | Staffed moderation, symmetrical block, identity-based harassment as a named offence | Friends-only visibility options |
| C45 | Unlimited housing **vs** neighbourhood life | Hamlets, tours, shopfronts, commons projects | Public hamlet squares in towns |
| C46 | Building freedom **vs** low-spec clients and griefing | Render-cost budget; server-validated placement; visitors can always leave | Detail scaling |
| C47 | Never demolishing homes **vs** dead hamlets | Shuttered lots can have their marker moved; the home is untouched | Auto-suggest hamlet merges |
| C48 | No user-uploaded art **vs** creative players | Motif libraries, dye channels, blueprints, composing music | Moderated uploads post-launch |

### Technical

| ID | Conflict | Resolution | Fallback |
|---|---|---|---|
| C49 | Action combat **vs** server authority and latency | Telegraph minimums; defender-favoured dodge window; prediction limited to own movement | Slower base combat pace |
| C50 | Action combat **vs** crowds on low-spec machines | 40 full characters, the rest simplified; baked atlases | Lower cap |
| C51 | One shared world **vs** layers splitting people | Placement by party, Kith, Company, hamlet; switch at waystones | Larger zone caps for towns |
| C52 | Minimal client trust **vs** responsiveness | Predict what is safe; never predict outcomes | — |
| C53 | Rust strictness **vs** content iteration speed | Content is data with hot reload; rules are code | Sandboxed scripting |
| C54 | Bot scoring **vs** players with atypical input (assistive devices, motor differences) | Scores trigger review, not bans; declared assist modes are factored in; appeal path | Raise thresholds |
| C55 | Web client **vs** native performance | Wasm for hot paths; pooled objects; measured budgets | Godot client |
| C56 | Agent-written code **vs** security-critical correctness | Property tests, fuzzing, red-team bot, human review on `ledger`, `auth`, `gateway` ([16](16-agentic-implementation.md)) | Outside audit before any public test |
| C57 | Single `ledger` writer **vs** scale | Per-region ledgers; read replicas | Per-zone inventory shards with two-phase transfers |
| C58 | Shared craft simulation in wasm and native **vs** identical results | Integer maths in `craft-sim`; the server's replay is final | — |

## C. Assumptions to test first

1. A cutout-rig garment costs a small fraction of a frame-by-frame one. (Slice, M5)
2. A browser client holds 30 fps on the reference machine in a busy town. (M2)
3. One core simulates 150 players in combat at 20 Hz. (M1, M3)
4. Analog minigames stay fun and resist trivial scripting. (M4, owner playtest)
5. Parties of four form within ten minutes at test population. (M6 onward)
6. Freshness reads as a bonus. (M2 playtest)
