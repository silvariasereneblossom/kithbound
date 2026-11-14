# 07 — Economy

Status: draft spec.

## 1. Principles

1. **Players make everything worth having.** Enemies drop materials, components, and technique fragments. They do not drop finished gear.
2. **Things wear out, slowly.** Demand for crafters never ends, and no patch makes gear obsolete.
3. **Bulk is cheap; excellence is scarce.** Automation floods the low end on purpose. Value sits in full-grade materials, Prime specimens, Workmanship, and reputation.
4. **Every coin and item is accounted for.** One service owns all ownership changes, in transactions, with an append-only ledger ([11](11-architecture.md) §6).
5. **Nothing is sold for real money that the economy produces.**

## 2. Gear life cycle

- Gear has durability. Use wears it; repair restores it and lowers the maximum by a little each time. A well-made item lasts months of regular play.
- Higher Workmanship and tougher materials wear more slowly, so quality has a lasting price premium.
- Worn-out gear can be **reclaimed** by a crafter for part of its materials and, rarely, a technique insight.
- Appearance is separate. A look added to the wardrobe is permanent ([08](08-wardrobe.md)).

## 3. Faucets and sinks

| Faucets (currency in) | Sinks (currency out) |
|---|---|
| Regional contracts (deliver goods, clear camps, escort) | Ground rent on claims ([06](06-gathering-automation.md) §5) |
| Sale of surplus to NPC buyers at floor prices | Freight and waystone travel fees |
| Trial and Hunt purses | Market listing fee and sales tax |
| | Homestead expansion, hamlet commons projects |
| | Repair fees at NPC smiths (worse than player repair) |

| Item faucets | Item sinks |
|---|---|
| Gathering, automation, combat drops | Crafting inputs, fuel, wear parts |
| Crafting | Durability loss, reclaiming |
| | Wardrobe unlocks (the item is consumed) |
| | Consumables: food, tonics, dyes, scrolls |

Telemetry tracks total currency, velocity, and price indices per region from the ledger. Rent, fees, and contract payouts are tuning knobs, adjusted by data and announced.

## 4. Trade

| Channel | How it works |
|---|---|
| **Shopfronts** | A shelf at your homestead or a rented stall in town. Goods are searchable region-wide; buying means visiting or paying freight |
| **Regional markets** | One order book per region. Goods sit in that region's depot. Prices differ between regions |
| **Freight** | NPC carriers move goods between depots for a fee and a delay. Hauling it yourself is faster and free, and escorted caravans are party content |
| **Commissions** | A buyer posts a work order: recipe, minimum Workmanship, materials supplied or not, price. Currency and materials are held in escrow. The crafter gets a Practice bonus |
| **Direct trade** | Two-sided confirm window; both sides see exact items, and any change resets confirmation |

Regional prices and freight are deliberate friction: they give traders a role and make geography matter. Search is global, so nobody walks between stalls comparing prices.

## 5. Crafter reputation

Maker's marks, a public record of commissions fulfilled, and shop ratings. Provenance Practice ([02](02-progression.md) §3) pays crafters when their goods are used. Star Wars Galaxies' market eventually collapsed into a perfect-information race to the bottom; the counter here is that items are not fungible. Two swords of one recipe differ in shape and materials, and buyers follow makers.

## 6. Against bots and real-money trading

| Threat | Design counter |
|---|---|
| Farming bots selling bulk | Bulk is cheap already; automation is a legitimate in-game system |
| Gold selling | Few raw currency faucets; most wealth is in goods with makers' marks. Ledger analytics flag one-way transfers and price-anomalous trades |
| Mule accounts | New accounts have trade limits that lift with played time and verified sign-in |
| Laundering via market | Listings far from the regional index are delayed and flagged |
| Stolen accounts | Passkeys; trade holds after a new device signs in ([12](12-security.md)) |

An emergency **trade freeze** switch exists per region and per item class, so a suspected dupe can be contained while it is investigated.

## 7. Business model

An owner decision, listed in [DECISIONS.md](DECISIONS.md). The constraints from this spec:

- Never sold: power, Practice speed, Rested pool, inventory or housing capacity, automation capacity, finished cosmetics.
- No gacha, no loot boxes, no items that expire.
- If cosmetics are monetised, they are sold as **pattern books** that a tailor must still make, so paid looks pass through the player economy.

## 8. Known tensions

- Permanent wardrobe unlocks mean demand for any one garment saturates. New patterns, fabrics from shifting fields, and colourways keep tailors busy; this needs watching.
- Durability must be slow enough not to feel like a tax and fast enough to matter. Start slow.
- Regional markets split liquidity when population is low. Regions can be merged into one market by configuration.
