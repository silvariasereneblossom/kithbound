# 05 — Life skills (vocations)

Status: draft spec. Smithing and Tailoring are specified in depth as the reference pair; the others follow the same framework and get their own spec files before they are built.

## 1. Goals

- Each craft is built on the real decisions of the real craft.
- Output depends on the player's hands, the character's rank, the materials, and the tools — in that order of feel.
- The 500th item is not a chore.
- Crafts need each other, so crafters need each other.
- The server can verify every result.

## 2. Vocations

| Group | Vocation | Phase |
|---|---|---|
| Gathering | Mining | Slice |
| Gathering | Foraging (herbs, fibre) | Slice |
| Gathering | Forestry | Launch |
| Gathering | Fishing | Launch |
| Gathering | Husbandry (crops, animals, silk, wool) | Post-launch |
| Crafting | Smithing | Slice |
| Crafting | Tailoring | Slice |
| Crafting | Tinkering (mechanisms, automation) | Slice (basic) |
| Crafting | Woodworking | Launch |
| Crafting | Cooking | Launch |
| Crafting | Alchemy (tonics, dyes) | Launch |
| Crafting | Jewelcraft | Post-launch |
| Crafting | Scribing (technique scrolls, maps) | Post-launch |
| Performance | Music | Launch |
| Performance | Styling | Launch |

### Commitment

A character can learn every vocation, and commits to a few:

| Commitment | Count | Rank cap |
|---|---|---|
| Primary | 1 | Master (15) |
| Secondary | 2 | Expert (12) |
| Uncommitted | all others | Apprentice (6) |

Apprentice covers the basics: field repairs, plain meals, common materials. Anything better comes from someone who committed. Changing commitments has a 14-day cooldown, and ranks above the new cap go dormant, not lost. Gathering vocations share this pool on purpose: a master smith who wants master-grade ore needs a miner.

## 3. The quality model

Every crafted item records how it was made.

- **Material properties.** Each batch of material has its own values (metals: hardness, toughness, density, conductivity; fabrics: strength, drape, warmth, sheen). Properties come from where and how it was gathered ([06](06-gathering-automation.md)).
- **Workmanship**, 0–100, measured from the crafting session.
- **Recipe weights** map properties and workmanship onto item stats. Different recipes weight properties differently, so there is no single best material.
- **Maker's mark.** The crafter's name is on the item permanently.

Character rank does not add quality. It does three things:

| Rank effect | Example |
|---|---|
| Sets the Workmanship ceiling | Novice 40, Apprentice 55, Journeyman 70, Expert 85, Master 100 |
| Widens Handling | Larger timing windows, steadier cursor, an extra heat before grain damage |
| Unlocks techniques | Differential hardening, French seams, pattern-welding |

Tool quality works the same way as rank at a smaller scale. A skilled player with a low rank hits their ceiling quickly and reliably; a careless player with a high rank makes mediocre goods. This follows Mabinogi's shrinking tailoring cursor and avoids the Fantasy Life outcome where the stat is all that matters.

## 4. Smithing

A weapon takes three to six minutes by hand.

| Phase | What the player does | What it decides |
|---|---|---|
| 1. Charge | Choose metals and ratios for the crucible | Alloy properties (shown as ranges until assayed) |
| 2. Heat | Work the bellows; read temperature from the metal's colour | Workability. Too cold: strikes do little and risk cracks. Too hot: the alloy burns and loses toughness |
| 3. Forge | Strike the billet. Choose hammer (flat face spreads, cross-peen draws, fuller grooves), position, and force (hold to charge) | The actual shape. The billet is a small height grid; strikes move volume, and volume is conserved |
| 4. Quench | Pick the medium (water, oil, brine) and the moment | Hardness against crack risk. Thin sections crack first, so shape matters here |
| 5. Temper and grind | Reheat gently; set edge angle | Trades hardness for toughness; sharpness against edge durability |
| 6. Fit | Assemble with a grip, guard, and wrap | Components come from Woodworking and Tailoring |

Details that make it smithing:

- **Heats.** Metal cools as you work. Each return to the fire is a heat, and every heat coarsens the grain a little. Good smiths plan their strikes and finish in fewer heats.
- **Shape is stats.** There is no separate "quality roll". Where the metal ends up sets weight, balance point, edge thickness, and durability. Two swords from one recipe handle differently.
- **Flaws.** Each billet has hidden inclusions, seeded by the server. They show as off-colour sparks and a dull ring when struck nearby. Work them toward the tang or cut them off.
- **Striker.** Large pieces need a second pair of hands. One player holds and directs with light taps; another swings the sledge where the taps land. An Echo or a hired hand can strike, less accurately.

Rank unlocks include: reading temperature as a number, a second quench medium, pattern-welding two alloys, differential hardening with clay, and fullers.

## 5. Tailoring

A garment takes four to eight minutes by hand.

| Phase | What the player does | What it decides |
|---|---|---|
| 1. Brief | Pick a pattern; for a commission, read the client's frame and request | Garment family; a made-to-measure bonus for the named wearer |
| 2. Draft | Adjust pattern pieces: ease, length, darts, collar and sleeve variants | Mobility against coverage and warmth; fabric needed; the silhouette |
| 3. Lay and cut | Arrange pieces on the bolt; follow the grain; match prints; avoid flaws in the cloth | Yield and waste. Off-grain pieces cap Workmanship |
| 4. Sew | Choose a seam type per seam (plain, French, flat-felled); steer the needle along the seam line, controlling speed and tension | Seam strength, bulk, and neatness |
| 5. Press and finish | Hems, closures, lining | Durability; closures come from Woodworking or Jewelcraft |
| 6. Embellish | Place and stitch embroidery motifs; apply dyes and trims | Appearance; dyes come from Alchemy |

Details that make it tailoring:

- **Fabric handles differently.** Silk slips under the needle and frays at cut edges; wool forgives; leather shows every misplaced hole. Fabric properties change the minigame, not just the result.
- **The layout is a puzzle.** Fitting pieces on a bolt with grain, nap, and print direction is a packing problem with a different answer for every bolt.
- **Seams are choices.** A French seam is strong and clean and costs time and fabric. A flat-felled seam takes hard wear. The wrong seam on the wrong fabric puckers.
- **Design space.** Pattern × draft variants × fabric × trim × colourway gives each tailor a recognisable hand. This is where the wardrobe's variety comes from ([08](08-wardrobe.md)).
- **Fit for everyone.** Appearance garments fit every body automatically. Made-to-measure only affects a small stat bonus on armour-grade clothing.

Embroidery uses an authored motif library at launch. Free-form player drawing is deferred because of moderation cost ([14](14-risks-and-conflicts.md)).

## 6. Other vocations in brief

| Vocation | Core of the minigame | Reference |
|---|---|---|
| Cooking | Heat zones, timing several components to finish together, balancing seasoning; party feasts are multi-dish services | Timing games |
| Alchemy | Steer a mixture across a property map using ingredients with partly hidden traits | Potion Craft |
| Woodworking | Read the grain, choose cuts, fit joinery to tolerance | Shape-fitting |
| Tinkering | Assemble a mechanism from parts under space, power, and wear limits; the design is saved as a blueprint | Logic and layout puzzles |
| Jewelcraft | Cut stones along cleavage planes; set them | Precision |
| Scribing | Compose technique scrolls from fragments won in combat | Pattern assembly |
| Music | Rhythm performance; ensembles with other players; composing in notation | Mabinogi's music |
| Styling | Themed contests judged by tags and by players | Nikki styling battles |
| Mining | Read the vein; place strikes to free ore without shattering it | [06](06-gathering-automation.md) |
| Foraging | Identify, choose the cut, time the season | [06](06-gathering-automation.md) |
| Fishing | Read water and weather; play the line | [06](06-gathering-automation.md) |
| Forestry | Choose the tree; plan the fell; season the timber | [06](06-gathering-automation.md) |

## 7. Keeping it fresh, and keeping it honest

### Routine crafting

Once a player has made a recipe by hand in a given material class, they can make it again by **routine**: a short confirmation in about a tenth of the time.

- Routine Workmanship is 85% of the crafter's personal best for that recipe and material class.
- Routine never produces the top grade band.
- Routine gives one tenth of the Practice.
- Batches are supported.

Hand-crafting is for learning, for pushing a personal best, for commissions, and for rare materials. Stocking a shop with nails is routine. This answers both the "tedious one at a time" problem and the urge to macro.

### Why it resists scripting

1. **Server-held seeds.** Flaws, grain, and hidden traits live on the server. The client is told only what the character could perceive.
2. **Analog input.** Positions, force, and timing are continuous, and the best answer differs for every billet and bolt.
3. **Re-simulation.** The client sends a timestamped input trace. The server replays it against the true state and computes the result itself ([12](12-security.md) §5).
4. **Plausibility.** Traces are scored for human timing variance. Outliers are queued for review, not auto-banned.
5. **Low payoff.** Routine crafting already gives legitimate players the convenience a bot would sell.

A sufficiently good bot can still play an analog minigame well. The design goal is to make that unprofitable and detectable, not impossible.

### Accessibility

Every minigame has assist options that trade time for dexterity: slower phases, hold-to-toggle, larger targets, and audio and colour-independent cues. Assists do not lower the Workmanship ceiling.

## 8. Interdependence map (slice)

```
Mining ──ore──▶ Smithing ──blades, tools, fittings──▶ everyone
   │                │  ▲
   │                │  └── grips ── Woodworking
   │                └──── needles, shears ──▶ Tailoring
Foraging ─fibre─▶ Tailoring ──wraps, bags, garments──▶ everyone
   │                                   ▲
   └──── dyestuffs ──▶ Alchemy ─dyes───┘
Smithing + Woodworking ─parts─▶ Tinkering ─machines─▶ gatherers (automation)
```

## 9. Open questions

See [DECISIONS.md](DECISIONS.md): commitment counts, whether alts share a homestead workshop, and free-form embroidery.
