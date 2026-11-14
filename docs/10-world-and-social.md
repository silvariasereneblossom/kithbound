# 10 — World structure, quality of life, and social systems

Status: draft spec.

## 1. World structure

| Unit | Description | Player cap (target) |
|---|---|---|
| **Megaserver** | One shared world per geographic area (for example Americas, Europe, Asia-Pacific) | — |
| **Region** | A themed area with its own market, treasury, resource fields, and tier band | — |
| **Zone** | A map simulated by one zone instance | 150 soft, 250 hard |
| **Layer** | A parallel copy of a busy zone, created automatically | Same |
| **Town** | A zone with no combat and cheaper simulation | 400 |
| **Instance** | Delve, Trial, Stronghold, homestead, hamlet, claim | By content |

Layers are created and merged automatically. Parties, friends, guildmates, and hamlet neighbours are placed together; a player can switch to a friend's layer at any waystone.

Regions have tier bands. Higher-tier regions are farther from the starting towns, and the roads between them pass through places worth stopping.

## 2. Travel

- On foot and mounted along roads; mounts are earned and crafted (tack by tailors and smiths).
- **Waystones** in towns and camps link places a character has already reached, for a small fee.
- **Rally stones** bring a party member to the leader's nearest town or camp.
- A free return to the homestead from anywhere, on a cooldown.
- No flying and no instant teleport to dungeon entrances.

## 3. Friction ledger

Every inconvenience in the game should be on one side of this table. Anything on neither side is a bug in the design.

| Kept, because it creates play between people | Removed, because it only wastes time |
|---|---|
| Combat tiers gate on party Trials | Chat spam to find a group (Party Board) |
| Travel to content; regional distance | Re-walking known routes (waystones) |
| Regional markets and freight | Walking stall to stall to compare prices (global search) |
| Vocation commitment: you need other crafters | Inventory tetris: generous, typed, searchable storage |
| Gear wears; repairs need a smith | Corpse runs, Practice loss on death |
| Automation needs several skillsets and upkeep | Hand-gathering common bulk forever |
| Hand-crafting for bests, commissions, rare materials | Hand-crafting the hundredth identical nail (routine) |
| Preparing for a Delve: food, repairs, plan | Loot rolls, tag races, kill stealing (personal loot, shared credit) |
| Service visits to distant claims | Daily logins, expiring rewards |
| Reputation on one shared world | Paid appearance changes, paid respecs |

## 4. Social structures

| Structure | Size | Purpose |
|---|---|---|
| Party | 4 | Combat unit |
| Band | up to 40 | Open-world Hunts; several parties |
| Kith | personal list | Friends; bonds grow with shared play and never decay |
| Company (guild) | up to ~150 | Hall, hamlets, shared works, Company projects |
| Hamlet | up to 24 homes | Neighbours |
| Mentor and apprentice | pairs | Sync-down play; Teaching progress |

A player can belong to one Company and any number of informal **circles** (named chat groups) so that niche interests — a music ensemble, a tailors' guild, a styling club — are not forced into one roster.

## 5. Communication and safety

- Channels: local, party, band, Company, circles, hamlet, whispers. No world-wide shout channel.
- Text passes a filter the player can tighten for themselves. Blocking is account-wide and symmetrical: a blocked player cannot see, whisper, trade with, or visit the blocker.
- Reports attach recent context automatically. Moderation actions are logged and auditable ([12](12-security.md) §8).
- Harassment tied to identity is a named offence in the code of conduct, with enforcement staffed from the first public test. A game that advertises inclusive creation and then leaves players unprotected has failed at pillar 5.
- Quick emotes, pings, and a phrase wheel let players cooperate without typing or sharing a language.
- Voice chat is out of scope.

## 6. The first hours

| Time | Beat |
|---|---|
| 0–20 min | Make a character. Arrive. Move, fight, gather one thing, craft one thing |
| 20–60 min | Try three discipline kits. Receive the homestead. Make a first garment |
| 1–2 h | First camp with an Echo of a tutor character. First Concert |
| 2–4 h | First real party through the Party Board or an Open Band. Choose a Calling |
| 4–10 h | First Delve. First commission. Join or found a hamlet |

A new player should have grouped with a real person within the first three hours, by invitation and not by requirement.

## 7. Events

Seasonal festivals return every year with the same patterns available, plus new ones. World Hunts run on in-world cycles. There are no limited-time exclusive rewards.

## 8. Accessibility

- Full key and gamepad remapping; one-handed presets; hold-or-toggle everywhere.
- Colour-independent telegraphs (shape and pattern as well as colour); adjustable telegraph opacity.
- Scalable text and UI; screen-reader support for menus through native HTML.
- Assist modes in crafting minigames ([05](05-life-skills.md) §7).
- Photosensitivity setting that removes flashes.
- Subtitles and visual cues for all gameplay audio.

## 9. Localisation

All text is keyed from the first commit. Layouts allow 40% expansion. Right-to-left support is a launch-window goal.
