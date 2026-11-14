# 03 — Disciplines, sub-disciplines, and multiclassing

Status: draft spec. Names are placeholders.

## 1. Goals

- A lot of build freedom, expressed through several independent layers.
- A sub-discipline always fits its main, including mains that do not exist yet.
- Multiclassing is possible, takes real commitment, and is never mandatory.
- The combination space stays testable.

## 2. Customization layers

| # | Layer | Choices | Changeable |
|---|---|---|---|
| 1 | Main discipline | 1 of N Called disciplines | Town |
| 2 | Sub-discipline | 1 of any discipline at Apprentice+ | Town |
| 3 | Loadout | 6 actives + 1 mobility + 1 signature, within 10 Attunement | Town or camp |
| 4 | Augments | One of 2–3 forks at ranks 3, 6, 9, 12, 15 of each skill | Town or camp |
| 5 | Tenets | 4 passive slots from discipline tenet pool | Town or camp |
| 6 | Gear | Crafted; stats come from materials and how it was made ([05](05-life-skills.md)) | Any time |
| 7 | Vocation perks | Field perks from committed vocations (a smith repairs in the field, a cook's meals last longer) | With vocation |

Appearance is fully separate from all of this ([08](08-wardrobe.md)).

## 3. Roles

Six soft roles. Every discipline can cover at least two depending on loadout, so no party is ever stuck waiting for "the" healer.

| Role | Job |
|---|---|
| Hold | Take and keep enemy attention; protect positions |
| Strike | Sustained damage |
| Break | Deplete enemy Guard to open stagger windows |
| Mend | Restore health, clear ailments, revive |
| Ward | Prevent damage: barriers, mitigation, interrupts |
| Control | Move, slow, bind, and group enemies |

## 4. Launch disciplines (target: 8; vertical slice: 3)

| Discipline | Kit | Core resource | Roles | Slice |
|---|---|---|---|---|
| Bulwark | Shield and one-hander | Poise | Hold, Ward, Break | yes |
| Reaver | Two-handed weapons | Momentum | Strike, Break | |
| Duelist | Light blades | Tempo | Strike, Control | |
| Ranger | Bow and traps | Focus | Strike, Control | yes |
| Arcanist | Elemental casting | Charge | Strike, Control, Break | |
| Mender | Restorative arts | Grace | Mend, Ward | yes |
| Chanter | Combat music | Cadence | Ward, Mend, Control | |
| Artificer | Deployables and gadgets | Parts | Control, Ward, Strike | |

Chanter draws on the Music vocation and Artificer on Tinkering, so those vocations have a combat outlet.

Each discipline ships with about 12 skills: 8 active-capable, 1 mobility, 1 signature, and a tenet pool of 8.

## 5. Sub-disciplines

Any discipline the character has at Apprentice tier can be slotted as Sub. A sub grants:

1. Its **sub-available skills** (about half its actives; never the signature), usable up to rank 12.
2. Its **Sub Aspect**: one passive that plugs into whatever the main is.

### How "seamless" is guaranteed

Synergy is written against abstractions, never against a named discipline.

- Every skill carries **tags** from a fixed vocabulary: delivery (`melee`, `shot`, `cast`, `deploy`, `song`), effect (`strike`, `break`, `mend`, `ward`, `bind`, `move`), and element.
- Every main has a **core resource** with a common interface: it is *built* by some events and *spent* by others.
- A Sub Aspect may reference only tags and the build/spend interface.

Examples:

| Sub | Sub Aspect |
|---|---|
| Bulwark | Spending your core resource grants a brief barrier scaled to the amount spent |
| Ranger | Your `strike` skills mark the target; marked targets take extra Break from everyone |
| Mender | Building your core resource above half heals the lowest-health ally nearby for a small amount |
| Chanter | Each distinct tag you use in sequence extends a party-wide tempo buff |
| Artificer | Your mobility skill leaves a deployable matching your last used element |

Because an Aspect cannot name a discipline, it works with all eight launch mains and with any main added later. The content validator enforces this.

Sub skills cost 1 Attunement each, the same as main skills. The limit is the rank-12 cap and the missing signature.

## 6. Multiclassing

A character begins with one **Calling** (chosen after the tutorial, once they have tried several kits). Only a Called discipline can be Main.

| Step | Requirement | Result |
|---|---|---|
| Dabble | None | Any discipline can be trained to Journeyman and used as Sub |
| Second Calling | First Calling at Master tier; a long questline with party Trials in the new discipline | The second discipline can be Main and can pass Journeyman |
| Twin Calling | Both Callings at Master tier; a capstone Trial | Up to 2 skills from the other Calling may be slotted while it is not Main or Sub, at 2 Attunement each |
| Third Calling and beyond | Same as Second, with requirements scaled up | As above |

Skills outside the current Main and Sub gain Practice at 50%.

Why this shape:

- **Possible**: nothing is locked permanently, and one character can eventually do everything in combat.
- **Difficult**: a second Calling needs roughly 400+ hours in the first.
- **Not mandatory**: Twin Calling adds flexibility, not raw power. The Attunement budget stays at 10, so two cross-calling skills displace three ordinary ones.
- **Complementary subs stay relevant**: a Sub costs nothing extra, so Main + Sub remains the efficient baseline and Twin Calling is a specialist's tool.

## 7. Keeping the combination space sane

8 mains × 7 subs × loadouts × augments is far too many to hand-test. Controls:

1. **Budgets, not exceptions.** Attunement, tenet slots, and the tag vocabulary bound what any build can stack.
2. **Stacking rules are global.** Same-named effects do not stack; same-tag buffs stack additively within a category and multiply across at most three categories.
3. **Automated balance harness.** Headless bot parties run every main/sub pair with heuristic loadouts against a fixed encounter suite each night. Outliers beyond ±15% of the median clear time are flagged ([16](16-agentic-implementation.md)).
4. **Augments are forks, not upgrades.** Each fork trades something; none is a strict improvement.
5. **New disciplines add tags rarely.** Adding a tag requires a spec change, because every Aspect must be re-checked against it.

## 8. Open questions

See [DECISIONS.md](DECISIONS.md): launch discipline count, whether weapons are tied to disciplines, and the Attunement budget.
