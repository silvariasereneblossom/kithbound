# 02 — Progression

Status: draft spec. All numbers are initial tuning targets, to be replaced by playtest data.

## 1. Goals

- Skills grow by being used, the way they do in Mabinogi.
- No rank-up checklists. A player never has to look up "what do I need to do 40 times" or fail on purpose.
- The fastest way to progress is to play naturally: varied content, at your level, with other people.
- The game is long (hundreds of hours per build), and every step hands the player something they can feel.
- Repetition against trivial content, macros, and AFK loops earn nothing.

## 2. Shape

There is no character level. A character is described by:

| Layer | What it is | Grows by |
|---|---|---|
| **Skill** | One ability or craft technique family | Practice from use (§3) |
| **Discipline** | A combat class: a bundle of ~12 skills plus a core mechanic ([03](03-disciplines.md)) | Its skills' ranks, plus a Trial per tier |
| **Vocation** | A life skill: gathering, crafting, or performance ([05](05-life-skills.md)) | Its techniques' ranks, plus a Trial per tier |
| **Attributes** | Might, Finesse, Wit, Resolve, Vigor | Derived from active skills (§6) |
| **Renown** | Account-visible summary of everything above | Derived; cosmetic and social only |

Every skill has 15 ranks in five tiers of three:

| Tier | Ranks | Target time per rank (of varied, on-level use) | Feel |
|---|---|---|---|
| Novice | 1–3 | 20–40 min | Learn what the skill is |
| Apprentice | 4–6 | 1–2 h | First real choices |
| Journeyman | 7–9 | 3–5 h | Competent; party-ready everywhere |
| Expert | 10–12 | 8–12 h | Specialist |
| Master | 13–15 | 20–30 h | Rare; server-known |

One skill to rank 15 is roughly 130 hours of use. A discipline's skills train in parallel during play, so a discipline to full Master is a 400–600 hour project, and most players will live happily at Journeyman/Expert in several things.

## 3. Practice

Every meaningful use of a skill grants Practice toward its next rank:

```
practice = base(action) × Challenge × Freshness × Outcome × Company × Rested
```

All factors are computed on the server from events the server itself observed.

**Challenge** — how hard the content is relative to the skill's rank. Content has a tier; so does the skill.

| Content tier vs skill tier | Challenge |
|---|---|
| Two or more tiers below | 0 (no practice) |
| One tier below | 0.4 |
| Same tier | 1.0 |
| One tier above | 1.3 |
| Two or more above | 1.5 (cap) |

This is what removes low-level farming and most macroing: trivial targets pay nothing.

**Freshness** — per skill, per *context* (enemy family, recipe, node type, chain partner's discipline). It starts at 1.0, drops 4% per use of the same context, floors at 0.4, and recovers fully over 20 hours. The first-ever use of a skill in a new context also pays a lump of **Insight** worth 5–10% of the current rank. Running a dungeon with friends touches many contexts, so it stays fresh for a long time; hitting one training dummy does not.

**Outcome** — exceptional 1.25, success 1.0, partial 0.6, failure 0.3. Failure is never required and never optimal.

**Company** — party of two 1.15, three 1.25, four 1.35. Co-op crafting 1.25. Crafting to fill another player's commission 1.2. Mentoring while synced down (§8) 1.2 on top.

**Provenance** — a crafter also earns Practice when other players *use* what they made (a sword swung in on-level combat, a meal eaten before a Delve). This is Raph Koster's "reward results, not repetitions" fix for use-based systems; it was planned for Star Wars Galaxies and cut. It pays crafters for making things people want and pays nothing for vendor-trash spam.

**Encounter share** — a quarter of the Practice earned in an encounter is spread across every skill in the loadout, used or not. Nobody has to spam a weak skill, or avoid their best one, to train it. This is the answer to the Elder Scrolls problem where the optimal way to level is to play badly.

**Rested** — ×2 while the Rested pool lasts. The pool fills by one hour per eight hours offline and caps at 14 hours, so a once-a-week player gets about two long doubled sessions. There is no daily login reward and nothing expires on a calendar.

### Why this is not Mabinogi's training list

| Mabinogi | Here |
|---|---|
| Each rank has a fixed list of conditions with counts | One Practice bar, fed by any use |
| Conditions reward narrow outcomes, repeated hundreds or thousands of times | Any on-level use counts; failure always pays less than success |
| Conditions name enemy ratings ("Strong", "Awful") that shift as the character grows | Challenge is a continuous factor, shown in the UI before you engage |
| Optimal play is a wiki-guided routine | Optimal play is varied, on-level, grouped play |
| AP gates every rank | No second currency; Trials gate tiers only |

The UI shows the three numbers that matter on any target or recipe before the player commits: Challenge, Freshness, and whether Insight is available.

## 4. The "no empty ranks" rule

Every rank of every skill grants exactly one named, non-numeric unlock:

- a **Technique** (a new move, a new phase option in a craft minigame),
- an **Augment** choice (a fork that changes how the skill behaves),
- a **Recipe family** or material class,
- an **Automation capability** ([06](06-gathering-automation.md)),
- a **Handling** upgrade (a wider timing window, a better tool grip, an extra heat), or
- a **Tenet** point for the discipline or vocation.

Pure number growth is capped at 3% per rank. This is enforced mechanically: the content validator rejects any skill definition where a rank has no non-numeric unlock.

## 5. Tier Trials

A discipline or vocation moves up a tier when (a) the sum of its skill ranks passes a threshold and (b) the character passes that tier's **Trial**. Skills cannot rank past their discipline's current tier.

- A Trial is authored content, not a counter. It can be retried freely with no penalty.
- **Combat Trials are party content** (2–4 players, at most one Echo — see [04](04-combat-and-party.md)). This is the main lever that makes grouping near-mandatory for combat progression: a solo player can fill a tier, but crosses into the next one with other people.
- **Craft Trials** are masterpiece commissions judged on measured quality, using materials the crafter must source across vocations.
- **Gathering Trials** are expeditions into a hazardous site, usually with an escort.
- Trials are per discipline/vocation tier (four each), not per skill, to keep the authored count manageable.

## 6. Attributes

Attributes are derived, never allocated. Each skill rank contributes fixed small amounts to one or two attributes. Only the skills of the character's **current Main discipline, Sub-discipline, and committed Vocations** count.

This deliberately avoids the Mabinogi outcome where every character must rank every skill for its stat bonuses. Training a skill you do not use gives you that skill and nothing else.

## 7. Vertical power policy

- Tier cap is fixed at Master for the life of the game unless a deliberate, announced exception is made.
- New content adds breadth: disciplines, techniques, augments, regions at existing tiers, material properties that are sidegrades.
- Gear is crafted, wears out slowly, and is never invalidated by a patch ([07](07-economy.md)).

## 8. Sync and mentoring

Any character can sync down to a lower tier to play with friends. Synced characters keep their techniques and augments but have ranks and gear clamped. They earn Practice at Challenge 1.0 in **Teaching** (a per-discipline side track that unlocks cosmetic titles, Echo improvements, and mentor tools), plus normal material rewards. A veteran always has a reason to play with a newcomer.

## 9. Respec and switching

| Change | Cost |
|---|---|
| Swap loadout skills, augments, tenets | Free in any town or camp |
| Swap Main/Sub among disciplines already Called | Free in any town |
| Change committed Vocations | 14-day cooldown; ranks above the uncommitted cap go dormant, not lost |
| Take a second Calling | Long questline ([03](03-disciplines.md) §6) |

## 10. Anti-grind and anti-macro summary

| Abuse | Counter |
|---|---|
| Farming trivial enemies | Challenge = 0 |
| Repeating one action for hours | Freshness floor 0.4, no Insight |
| AFK macro on a dummy or a friend | Dummies have no tier; player-vs-player sparring pays Practice only inside a rated duel with a daily cap |
| Failing on purpose | Outcome 0.3 |
| Scripted crafting | Seeded, analog minigames re-simulated on the server ([05](05-life-skills.md) §7); routine crafting pays almost no Practice |
| Being carried | Practice needs the character's own skill events; Trials check individual contribution |

## 11. Open questions

See [DECISIONS.md](DECISIONS.md): rank count per tier, Rested pool size, and whether Trials may be attempted with two Echoes during low-population hours.
