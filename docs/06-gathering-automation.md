# 06 — Gathering and automation

Status: draft spec. Numbers are initial tuning targets.

## 1. Goals

- Bulk materials stop needing hands as the player masters the work.
- Automation is earned through rank and through combining vocations, usually across several players.
- The harder the material, the higher the bar to automate it. The best materials are never automated.
- Automation cannot become an AFK money printer, a land rush, or a server-load problem.

## 2. Resource fields and deposits

Each region has **resource fields**: continuous maps of concentration for each material family. Fields are generated from a seed and **shift every one to three weeks**. When a field shifts, its material gets new property values (hardness, drape, and so on), and the old batch never returns. This follows Star Wars Galaxies' unique resource spawns and keeps prospecting and material knowledge valuable.

**Surveying** is a gathering skill. A surveyor samples the field and gets a reading of concentration and properties at that spot. Better rank gives finer resolution and reveals more properties.

## 3. Manual gathering

Every node is a short minigame (10–40 seconds):

| Vocation | Minigame | Skilled play yields |
|---|---|---|
| Mining | Read the vein's direction; place strikes to free ore without shattering it | More ore, intact crystals |
| Foraging | Identify the plant; choose where to cut; leave the root for regrowth | Higher property values, regrowing patches |
| Forestry | Pick the tree by grain; plan the fell | Long clear boards |
| Fishing | Read water and weather; play the line | Rare species |

Hand-gathered material has the **full** property values of the field. It also has a chance at **Prime specimens**: rare variants with a special trait (a trace element, an unusual fibre) that the best recipes require. Prime specimens come only from hands, and mostly from dangerous sites where a gatherer wants an escort ([04](04-combat-and-party.md) §4).

## 4. Automation ladder

| Tier | Name | What it is | Output grade | Works offline |
|---|---|---|---|---|
| A0 | Hand | Minigame per node | Full, plus Prime chance | — |
| A1 | Aids | Portable tools used on the spot: sluices, snares, drying racks. Multiply what you gather while present | Full | No |
| A2 | Works | Stationary extractors on a claim: mine rigs, sawpits, retting ponds | Common (60% of field peak) | Yes |
| A3 | Lines | Workshop machines at the homestead that process materials and run routine recipes in batches; carriers move goods from claims | As input | Yes |
| A4 | Networks | Works and lines pooled by a hamlet or guild, with shared logistics | Fine (80% of field peak) | Yes |

### Gates

To automate a material of tier *T*:

| Requirement | Source |
|---|---|
| Gathering rank one tier above *T* | The gatherer |
| A survey of the claim, at that rank | The gatherer |
| A mechanism blueprint of tier *T* | Tinkering |
| Frames, fittings, and wear parts | Smithing, Woodworking |
| For A4: all of the above from at least three different characters | A group |

Consequences:

- Novice-tier bulk automates early. It is the first thing a new player's friends help them build.
- Master-tier materials would need a rank above Master, so they are **never** automated.
- Each step up the material tiers needs higher rank in three or four vocations at once. One character cannot hold those ranks ([05](05-life-skills.md) §2), so automation is a joint project by design.
- The Tinkering minigame matters: a better mechanism design gives more throughput per unit of upkeep.

## 5. Claims without a land rush

Works sit on **claims**. A claim is a small private instance anchored to a spot in a resource field. Any number of players can claim the same spot; they do not see or block each other.

Scarcity is handled by price and yield instead of by space:

- **Field pressure.** A field's yield per rig falls as total extraction in that field rises. Crowded fields pay less; quiet ones pay more.
- **Ground rent.** Each claim pays a recurring fee to the regional treasury. The fee is set by the field's richness and current pressure, so the best ground costs the most. This is a currency sink that tunes itself.
- **Access.** Claims in high-tier regions must be installed and serviced in person. Getting there is party content.

## 6. Upkeep

| Cost | Paid in | Why |
|---|---|---|
| Fuel | Materials from other vocations | Ties automation to the wider economy |
| Wear parts | Smithed and tinkered components | Recurring demand for crafters |
| Ground rent | Currency | Sink; prices good ground |
| Service visit | A trip, every 7–14 days by tier | Automation needs tending |
| Re-survey | After every field shift | No rig runs forever untouched |

A works that runs out of fuel or parts stops. It does not decay or vanish. Stored output is capped; a full hopper stops production.

## 7. Server cost

Works do not tick. Each stores its start time, rate, fuel, and wear. Output is computed in closed form when the owner collects, when a field shifts, or when an input runs out. Ten thousand idle rigs cost a few rows in the database and no CPU. Lines and carriers use the same lazy model with a small event queue per homestead.

## 8. Failure modes and counters

| Failure | Counter |
|---|---|
| Bulk materials flood and become worthless | Field pressure; upkeep; consumption by fuel and wear parts; routine crafting consumes bulk at scale |
| Hand gatherers have nothing to sell | Full-grade and Prime materials only come from hands; top recipes require them |
| Automation is indistinguishable from a bot farm | Automation is a server-side system with no client input, so there is nothing to bot. Rent and upkeep make mass idle accounts unprofitable |
| New players cannot compete | Novice bulk is meant to be cheap; newcomers earn from hand-gathered full-grade material and from escort and service work |
| AFK play replaces playing | Works need service visits, re-surveys, and supply. Output grade is capped below hand quality |
| One rich guild locks up the best ground | Claims are not exclusive; rent rises with demand |
| Persistent machines load the servers | Lazy closed-form accrual (§7) |

## 9. Open questions

See [DECISIONS.md](DECISIONS.md): field shift period, whether A4 Fine grade is too generous, and service-visit intervals.
