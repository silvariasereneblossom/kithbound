# 00 — Vision and pillars

Working title: **Kithbound** ("kith": the people you know and rely on). The name is a placeholder; a web search found no game using it, and no trademark database has been checked.

## 1. One paragraph

A 2D online world where you get stronger by doing things with other people. Combat is action-based and built for parties of four. Crafts are real games in their own right, and everything worth wearing or wielding was made by a player. Everyone gets a home and a wardrobe with no scarcity and no gender locks. The whole thing runs in a browser tab on an old laptop, and the server believes nothing the client tells it.

## 2. Pillars

| # | Pillar | Means | Spec |
|---|---|---|---|
| 1 | **Stronger together** | Combat progression needs a party; crafts need each other | [04](04-combat-and-party.md), [05](05-life-skills.md) |
| 2 | **Doing is learning** | Skills rise through use, with one visible bar and no checklists | [02](02-progression.md) |
| 3 | **Hands on the work** | Smithing feels like smithing; tailoring like tailoring | [05](05-life-skills.md) |
| 4 | **Every step counts** | No empty ranks, no gear resets, no chores | [02](02-progression.md) |
| 5 | **Yourself, at home** | Deep wardrobe for every body; a guaranteed house | [08](08-wardrobe.md), [09](09-housing.md) |
| 6 | **Mastery builds machines** | Bulk gathering automates as skills combine; rare things stay hand-won | [06](06-gathering-automation.md) |
| 7 | **The server is the truth** | Clients send intentions; servers decide outcomes | [12](12-security.md) |
| 8 | **Small footprint, wide reach** | Low-spec client, cheap servers, scale by adding zones | [11](11-architecture.md), [13](13-client-and-art-pipeline.md) |

When two pillars conflict, the lower number wins unless [14](14-risks-and-conflicts.md) records a different resolution.

## 3. Modern MMO pitfalls and the answer to each

| Pitfall | Answer |
|---|---|
| Endgame treadmill; each patch invalidates gear (mudflation) | Fixed tier cap; growth is sideways; gear is crafted and wears slowly |
| Daily and weekly chore lists, login rewards, expiring battle passes | None of these exist. The only time-based bonus is a Rested pool that fills while away |
| "Alone together": players solo side by side | Concert chains, pack encounters, and party Trials make other people mechanically valuable |
| Anonymous matchmaking strips out community | One shared world per region, a Party Board, and travel to the content |
| Role shortages stall groups | Parties of four, every discipline flexes two or more roles, Echoes fill gaps |
| Crafting is a progress bar, or is solved by macros | Analog, seeded minigames; routine crafting once you have proven a recipe by hand |
| Crafted gear is outclassed by drops | Bosses drop materials and techniques. All finished gear is made by players |
| Opaque, wiki-driven grinding | One Practice formula with its inputs shown before you act |
| Housing scarcity, lotteries, demolition | Every character has an instanced homestead, forever |
| Cosmetics are gender-locked or sold by gacha | Every garment fits every body; garments are crafted by tailors |
| Pay-to-win | Nothing that affects power, progression speed, or inventory capacity is sold |
| Bots and real-money trading | Server authority, an audited ledger, and an economy where bulk goods are cheap by design |
| Duplication exploits | Every item and coin moves in one database transaction ([11](11-architecture.md) §6) |
| Content droughts | Long-lived systems (economy, crafts, styling, housing, music) generate goals without new authored content |
| Low-population death spiral | Zone layers on a regional megaserver, sync-down, Echoes, and a full non-combat game |

## 4. Who it is for

- Players who miss needing other people: Mabinogi, Final Fantasy XI, classic EverQuest, Star Wars Galaxies.
- Crafters and traders who want their work to matter.
- Dress-up and housing players who have been asked to accept gender locks, gacha, or a lottery.
- Anyone on modest hardware.

## 5. Non-goals for the first release

- 3D graphics.
- Open-world PvP, full loot, territory warfare.
- A scripted main-story campaign with voiced cutscenes.
- Player-uploaded images or free-text signs in the world (moderation cost).
- Console ports.

## 6. Business model stance

Recorded in [DECISIONS.md](DECISIONS.md) as an owner call. The design assumes: no power for sale, no gacha, nothing that expires, and no paid cosmetics that bypass player tailors.
