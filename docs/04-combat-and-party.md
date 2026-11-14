# 04 — Combat and party play

Status: draft spec. Numbers are initial tuning targets.

## 1. Goals

- Action combat in 2D that reads clearly and tolerates 150 ms of latency.
- Combat progression is a group activity. Soloing exists, and is the slow road.
- Grouping is easy to start and has no role bottleneck.
- A player who logs in at 4 a.m. always has something worthwhile to do.
- Everything is decided on the server ([12](12-security.md)).

## 2. Core combat model

- Top-down three-quarter view, free 8-direction movement, aimed skills (direction or ground target). No tab-target lock; a soft aim-assist for gamepad and touch.
- **Loadout**: 6 actives, 1 mobility skill (dodge, dash, block-step), 1 signature. All skills have cooldowns and most build or spend the discipline's core resource.
- **Enemy attacks are telegraphed** on the ground or by wind-up animation. Minimum telegraph is 400 ms; dangerous attacks are 700 ms or more. Difficulty comes from overlapping patterns and positioning, not reaction speed.
- **Guard and stagger**: elites and bosses have a Guard gauge. `break` skills deplete it. At zero the enemy is staggered and takes increased damage for a few seconds.
- **Ailments** are few and visible: Burning, Chilled, Bound, Exposed, Sundered.
- **Downed state**: a player at zero health is downed for 30 s and can be raised by any ally. A fully solo player respawns at the last camp.
- **Death cost**: gear durability and the walk back. No Practice loss.

### Latency-driven design rules

These are constraints on content, enforced by the content validator where possible:

1. No enemy telegraph shorter than 400 ms.
2. Player dodges are validated with a 150 ms grace window in the defender's favour.
3. No mechanic depends on two players acting within less than 300 ms of each other.
4. Projectiles and areas are simulated on the server; clients predict only their own movement and the start of their own skill animations.
5. A zone shows at most 40 fully animated player characters to one client; the rest are simplified ([13](13-client-and-art-pipeline.md)).

## 3. Why you want other people

Grouping is made valuable by four mechanics that a lone character cannot reproduce.

### 3.1 Concert (cross-player chains)

Some skills are **Openers**: they apply a Primed state (Sundered, Exposed, Bound, Burning, Chilled). Others are **Linkers**: striking a Primed target with a matching Linker detonates a **Concert** — a large burst of damage, Break, healing, or control depending on the pair.

- A Concert between two different players is at full strength.
- A character linking their own Opener gets 40%.
- An Echo (§5) linking gets 70%.
- Three different players chaining in sequence produces a Grand Concert.

Concert pairs are defined on tags, so every discipline pair has some available.

### 3.2 Guard needs variety

Elite Guard gauges resist repeated Break from the same tag. Two or three different Break sources empty a gauge several times faster than one.

### 3.3 Encounter shape

Field enemies above the starter areas come in packs built for two to four players: a front line that punishes being flanked, a caster that must be interrupted, a healer that must be controlled. A solo player can pull single stragglers from the fringe of a camp. This is deliberate "solo fringe" content, worth about a third of the Practice per hour of a party in the camp.

### 3.4 Progression gates

- Company multiplier on Practice (up to 1.35).
- Tier Trials for combat disciplines are party content ([02](02-progression.md) §5).
- The best crafting materials from combat come from Delves and Strongholds.

A solo-only player can reach the top of Journeyman-equivalent in field play, see the whole world map, and enjoy every non-combat system. They cannot pass combat tier Trials alone.

## 4. Content types

| Type | Size | Length | Notes |
|---|---|---|---|
| Field camps | 2–4 (solo fringe) | Open-ended | Open world; shared credit |
| Delves | 4 | 20–30 min | Instanced dungeons |
| Trials | 2–4 | 10–20 min | Tier gates; retry freely |
| Strongholds | 8 (two parties) | 45–60 min | Instanced; weekly-ish cadence by design of drops, not lockout |
| World Hunts | 12–40 | 15–30 min | Open world; join by walking up |
| Escorts and expeditions | 2–6 | Varies | Protect gatherers, caravans, and installation crews ([06](06-gathering-automation.md)) |

Party size is four. Small parties form faster and make role shortages rarer.

## 5. Echoes: grouping when nobody is on

An **Echo** is an AI-driven copy of a real character's build, lent by a friend or guildmate (or by the character's own second Calling build).

- A party may contain at most one Echo per two human players, with a minimum allowance of one. A solo player can bring one Echo; a party of three can bring one.
- Echoes fight competently and predictably. They link Concerts at 70% and never perform Grand Concerts.
- The lender earns a small share of materials and Teaching progress when their Echo is used. This is an asynchronous social tie: your friend helped you while they were asleep.
- Echoes cannot be used in Strongholds and count against Trial contribution checks.

This is the answer to low-population hours. It is deliberately weaker than a person so it never replaces one.

## 6. Forming groups: what is easy and what is kept

| Kept (friction with a purpose) | Removed (friction with no purpose) |
|---|---|
| You travel to the place where the content is | Standing in a city spamming chat for a group |
| A party is people you can talk to, on one shared world where reputation follows you | Anonymous cross-server strangers who never meet again |
| Roles matter inside the fight | Waiting an hour for one specific class |
| Preparing matters: food, repairs, a plan | Loot arguments, kill stealing, tag races |

Tools:

- **Party Board**: a world-wide listing. Post or browse by activity; sign up asynchronously; get notified when a group is ready. Role-flex is shown per applicant.
- **Rally stones**: once a party exists, members can travel to the leader's nearest town or camp from any town. The last stretch is on foot.
- **Open Bands**: in the field, players near each other fighting the same camp can merge credit with one button. Bands convert to parties.
- **Shared credit**: everyone who contributes gets full personal loot and Practice. There is nothing to steal.
- **Layer priority**: the world service places party members, friends, and guildmates on the same zone layer ([11](11-architecture.md)).
- **Commendations**: after group content, players can commend each other. Commendations feed cosmetic titles and Party Board sorting, never power.
- **Kith bonds**: time spent grouped with the same people builds a bond that grants convenience (a shared Rested bonus, a longer-range Rally). Bonds do not decay.

## 7. Threat, healing, and roles

- Threat is visible. Hold skills generate it; positional and guard mechanics also let non-Hold builds kite or control instead of tanking.
- Healing is partly positional (zones, cones, linked Concerts), so Mend is an active role and not health-bar whack-a-mole.
- Every encounter must be clearable by at least three different role mixes. This is checked by the balance harness.

## 8. PvP

Out of scope for the first release, apart from consensual rated duels and a small arena that are balanced separately. No open-world PvP and no full loot. See [DECISIONS.md](DECISIONS.md).

## 9. Activities that never need a group

Crafting, gathering, automation upkeep, housing, wardrobe and styling contests, music performance, market trading, exploration and lore, solo-fringe hunting, and duo play with an Echo.
