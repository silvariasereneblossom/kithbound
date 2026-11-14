# Kithbound (working title)

Design specifications for a 2D cooperative MMO.

**Status: design only.** There is no code here yet, and implementation has not started. The name is a placeholder.

## The idea

- **Stronger together.** Action combat built for parties of four. You can explore and craft alone; you advance in combat with other people.
- **Doing is learning.** Skills rise through use, with one visible progress bar and no hidden checklists.
- **Hands on the work.** Smithing is heat, hammer, and quench. Tailoring is drafting, cutting on the grain, and sewing seams. What you make carries your name.
- **Mastery builds machines.** Bulk gathering automates as players combine skills. The best materials are always won by hand.
- **Yourself, at home.** A deep 2D wardrobe where every garment fits every body, and a house for everyone with no lottery and no demolition.
- **The server is the truth.** Clients send intentions. Servers decide outcomes.
- **Small footprint.** Runs in a browser on old hardware. Rust on the server.

## Read in this order

| # | Document | What it covers |
|---|---|---|
| 00 | [Vision and pillars](docs/00-vision.md) | What the game is, and its answer to each common MMO pitfall |
| 01 | [Research and prior art](docs/01-research.md) | Sourced findings behind the design and technology choices |
| 02 | [Progression](docs/02-progression.md) | Use-based skills without the grind |
| 03 | [Disciplines](docs/03-disciplines.md) | Classes, sub-classes, multiclassing |
| 04 | [Combat and party play](docs/04-combat-and-party.md) | Action combat; why and how players group |
| 05 | [Life skills](docs/05-life-skills.md) | Craft minigames, quality, interdependence |
| 06 | [Gathering and automation](docs/06-gathering-automation.md) | From hand-gathering to networks |
| 07 | [Economy](docs/07-economy.md) | Player-made goods, sinks, trade, anti-RMT |
| 08 | [Wardrobe](docs/08-wardrobe.md) | Inclusive character creation and dress-up |
| 09 | [Housing](docs/09-housing.md) | Homesteads and hamlets |
| 10 | [World and social](docs/10-world-and-social.md) | World structure, friction ledger, safety, accessibility |
| 11 | [Architecture](docs/11-architecture.md) | Language, services, netcode, persistence, scaling |
| 12 | [Security](docs/12-security.md) | Threat model and controls |
| 13 | [Client and art pipeline](docs/13-client-and-art-pipeline.md) | Client stack, performance budget, layered characters |
| 14 | [Blockers and conflicts](docs/14-risks-and-conflicts.md) | Every known blocker and design conflict, with resolutions and fallbacks |
| 15 | [Roadmap](docs/15-roadmap.md) | Vertical slice and milestones |
| 16 | [Agentic implementation](docs/16-agentic-implementation.md) | How AI agents would build this, and what they should not do |
| — | [Decisions](docs/DECISIONS.md) | Open and settled decisions |

## Licence

All rights reserved for now. A licence for these documents is an open decision (D12).
