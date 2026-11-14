# 16 — How to implement this with agents

Status: plan. **Implementation has not started and must not start until the owner says so.**

## 1. Roles

| Role | Who | Does |
|---|---|---|
| **Owner** | The human designer | Makes design calls; playtests at named gates; approves milestone exits |
| **Spec session** | One long-lived orchestrating agent, on the most capable model available | Keeps the specs current; turns a milestone into task briefs; reviews and verifies every result; merges; records decisions. Writes little code itself |
| **Implementers** | Sub-agents, one per task, each in its own git worktree | Build one brief; write tests; report |
| **Verifiers** | Fresh sub-agents with no view of the implementer's reasoning | Review a diff against the brief and the spec; try to break it |
| **Specialists** | Humans | Art, audio, security audit, legal, operations |

The owner designs; agents implement; the owner tests until it feels right. Agent output is treated as untrusted until verified.

## 2. Principles

1. **Specs lead.** Code follows `docs/`. If code needs to differ, the spec changes first, with a decision record.
2. **Contracts before bodies.** `proto` messages, content schemas, service interfaces, and database migrations are written and reviewed before the code behind them. Parallel tracks meet only at contracts.
3. **Make rules mechanical.** Any design rule that can be checked by a validator, a type, or a test is. "No empty ranks" is a validator error, not a guideline.
4. **Everything is testable without a screen.** The headless `botclient` can do anything a player can. Simulations are deterministic and replayable.
5. **Small tasks.** A brief is one to three days of human-equivalent work, touches one crate where possible, and has an explicit "done" check.
6. **Least code that meets the spec.** No speculative abstraction. Every report states lines added and removed.
7. **Security-critical crates get human eyes.** `ledger`, `auth`, `gateway`, and anything touching money.

## 3. Milestone loop

```
1. Spec session reads the milestone in docs/15 and the specs it references.
2. It writes or updates contracts (proto, schemas, migrations) and lands them on main.
3. It splits the milestone into briefs and groups them into tracks that do not share files.
4. Implementers run in parallel, one worktree each.
5. Each result goes to a verifier, then to the spec session, which runs the gate checks itself.
6. Merged work updates docs/STATUS.md (what exists, what is verified, known gaps).
7. At the exit gate, the owner playtests or approves. Findings become briefs or decisions.
```

Only the spec session merges to `main`.

## 4. Brief template

```markdown
# Brief <milestone>-<n>: <title>
Track: <S server | C client | D data | E economy | A avatar>
Spec: docs/<file>#<section> (read these first)
Contracts: <proto messages, schemas, tables this task uses — already on main>

## Goal
<two or three sentences: what exists when this is done>

## In scope
- ...
## Out of scope
- ...

## Done when
- [ ] <observable check, e.g. "botclient test `two_players_see_each_other` passes">
- [ ] cargo fmt, clippy with warnings denied, and tests pass
- [ ] no new dependency (or: dependency X approved in DECISIONS)
- [ ] report written

## Report back
What you built, what you did not, how you verified it, lines added and removed,
anything in the spec that was unclear or wrong.
```

A brief names files and "done" in the spec session's own words. It never says "as discussed".

## 5. Tracks

| Track | Crates | Runs alongside |
|---|---|---|
| S — simulation | `sim-core`, `zone`, `gateway` | C, D |
| E — economy | `ledger`, `auth`, migrations | S once contracts exist |
| C — client | `client-core`, `client/` | S, against `botclient`-recorded sessions |
| D — data | `content/`, `content` crate, validator | all |
| A — avatar | rig tools, baker, garment validator | C |
| Q — quality | `botclient`, harnesses, fuzzers, load tests | all |

Two writers never touch the same files. Each track has one worktree at a time.

## 6. Verification

| Layer | Tool | Catches |
|---|---|---|
| Types and lints | Compiler; clippy with warnings denied; `unsafe` forbidden | Whole classes of agent error |
| Unit and property tests | Standard test runner; property-testing crate | Rule bugs; ledger invariants |
| Deterministic replay | Recorded inputs → state hash | Simulation regressions |
| Fuzzing | Wire decoder, content loader, placement validator | Crashes, bounds |
| Integration | `botclient` against a local stack | Protocol and flow bugs |
| Red team | Hostile `botclient` profile: replays, races, malformed and out-of-order messages | Trust failures, dupes |
| Chaos | Kill services mid-transaction; assert conservation | Hand-off and persistence bugs |
| Balance harness | Bot parties × builds × encounters | Outliers; unclearable content |
| Economy simulation | Bot agents with simple strategies over simulated weeks | Inflation, dead markets |
| Client | Browser-driven tests; screenshot comparison in CI; performance profile | UI regressions; budget breaches |
| Content | Validator | Design-rule violations |
| Human | Owner playtests; specialist reviews | Feel; art; security |

A task is not done because its author says so. The spec session re-runs the checks.

## 7. Guard rails for agents

- Every test run has a memory limit and a timeout. Every retry loop is bounded.
- Each agent gets a private scratch directory. No agent deletes outside its own worktree and scratch directory.
- Heavy tests — load, soak, anything with large persistent files — run on a dedicated test host, not on the development workstation. Only results come back.
- No secrets in the repository, in prompts, or in logs. Local development uses generated throwaway credentials.
- No new dependency without a line in `DECISIONS.md` (purpose, licence, maintenance state).
- No design decisions by implementers. An ambiguity is reported, and work continues on what is unambiguous.
- No network access to production systems, ever.
- Commits are small, describe why, and reference the brief.

## 8. Repository conventions (to be created at M0)

```
AGENTS.md            rules every agent reads first
docs/                specs (this folder)
docs/STATUS.md       what is built and verified
docs/DECISIONS.md    owner and technical decisions
briefs/              one file per task; kept after completion
crates/ client/ content/ tools/
```

CI on every push: format, lint, tests, validator, dependency advisories and licences, short fuzz, red-team smoke.

## 9. What agents should not be asked to do

- Decide what is fun.
- Produce final art or music.
- Sign off security for `ledger`, `auth`, or payments.
- Tune live economies without human review.
- Moderate people.

## 10. Starting M0

When the owner gives the go-ahead, the first session's instructions are:

> Read `AGENTS.md`, `docs/00`, `docs/11`, `docs/12`, `docs/15`, and `docs/16`. Resolve the open technical decisions needed for M0 (ECS crate, codec, message bus) by checking current versions and maintenance status, and record them in `DECISIONS.md`. Create the workspace and CI. Write `proto` and the content schema as contracts. Split the remainder of M0 into briefs. Do not begin M1.

## 11. Cost control

- Use the most capable model for the spec session and for verification of security-critical work; use a cheaper capable model for routine implementation.
- Prefer one well-written brief over several agents exploring.
- Stop and re-scope when a task's diff exceeds twice its expected size.
