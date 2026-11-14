# 09 — Housing

Status: draft spec.

## 1. What "FFXIV syndrome" is, and the rule that prevents it

In Final Fantasy XIV, houses sit on a fixed number of plots per world. Demand exceeds supply, so plots are awarded by lottery, reclaimed from absent players, and limited to a few hundred furnishings ([01](01-research.md) §2).

The rule here: **housing supply is never finite.** Everything below follows from it.

| FFXIV | Here |
|---|---|
| Fixed plots per world | Instanced homesteads, created on demand |
| Lottery | Every account has one from the first hour |
| Auto-demolition | Never. An untouched home is exactly as it was left |
| 150–600 furnishing slots | A point budget starting near 1,500 items and growing past 6,000 |
| Large houses are the scarcest | Size is earned by play, by anyone |
| Wards mix strangers at random | Hamlets you choose, with people you choose |

## 2. Homesteads

- One free **homestead** per account, shared by its characters. More can be bought with in-game currency; there is no upkeep on any of them.
- Each homestead has a **setting** chosen from a growing list: meadow cottage, town terrace, forest clearing, lakeside, cliff, canal boat, workshop loft. Settings can be changed without losing anything.
- The plot starts at a modest size and expands in steps through play.

### Building

- **Tile building** for structure: floors, walls, roofs, doors, windows, stairs, fences, paths, water. Rooms are whatever shape you draw.
- **Free placement** for furnishing: fine snapping or none, rotation where art allows, stacking on surfaces, wall hanging, layering order, and nudge by pixel.
- **Dye channels** on furniture, using the same dyes as clothing.
- **Blueprints**: save a room or a whole build; share or sell the plan. Materials are still needed to place it.
- **Budget** is in render-cost points, not a flat count, so many small items are cheap and a few large animated ones are not.

### Function

A home is also where a character's work lives:

| Feature | Purpose |
|---|---|
| Workshop stations | Crafting, with station quality from Woodworking and Smithing |
| Lines | Automation machinery ([06](06-gathering-automation.md)) |
| Storage | Sorted, searchable, shared across the account |
| Garden and pens | Foraging and Husbandry |
| Mannequins and display cases | Show outfits and made items |
| Shopfront | Shelves that sell to visitors and appear in regional search ([07](07-economy.md)) |
| Stage | Music performances |
| Guest book, mailbox, notice board | Asynchronous social contact |

Furniture is made by players: Woodworking, Tailoring (soft furnishings), Smithing, Tinkering (moving pieces).

## 3. Hamlets: presence without scarcity

Instanced housing isolates people unless something brings them together.

- A **hamlet** is a small instanced village: a commons surrounded by up to 24 homestead lots.
- Hamlets are created on demand; there is no limit on how many exist.
- Players found one, join a friend's, join a guild's, or are offered an open one with active neighbours.
- Standing in the commons, you see your neighbours' exteriors and can walk in if they allow it.
- Moving a homestead to another hamlet is free and keeps everything.
- **Commons projects** — a market square, a bathhouse, a shared workshop, a festival ground — are built jointly and unlock A4 automation networks.
- A homestead inactive for a long time is shown as shuttered. Hamlet stewards can ask the system to move a shuttered lot's marker to make room. The home itself is untouched and reattaches when its owner returns.

Guilds get a **hall** (a large shared build) and can anchor one or more hamlets.

## 4. Visiting

- **Directory**: browse by theme, setting, popularity, and "open house" flag.
- **Tours**: visit a home while its owner is offline. The visitor walks a read-only copy.
- **Shop search** leads to shopfronts, so trade brings foot traffic.
- **Permissions**: visitor, friend, housemate (can use stations and storage), steward (can build).
- Likes and bookmarks; no public dislike count.

## 5. Technical notes

- A homestead instance runs only while someone is inside, in the same zone-server process type as any other instance ([11](11-architecture.md)).
- Layout is stored as compact placement records. Item ownership stays in the ledger; placing an item moves it to the homestead's container in a transaction.
- All edits are validated on the server: budget, bounds, ownership, collision rules, and permission.
- Automation inside a homestead uses lazy accrual, so an empty home costs no CPU.

## 6. Risks

| Risk | Counter |
|---|---|
| Homes feel isolated | Hamlets, tours, shopfronts, stages |
| Offensive builds | Report tool; hide-from-directory moderation action; no free text or images on signs at launch |
| Builds that trap visitors | Visitors can always leave from a menu; doors cannot lock a visitor in |
| Heavy homes hurt low-spec clients | Budget is render cost; client-side detail scaling |
| Furniture art volume | Tile sets and dye channels multiply variety; most pieces need one or two facings |

## 7. Open questions

See [DECISIONS.md](DECISIONS.md): per-account or per-character homesteads, hamlet size, and starting budget.
