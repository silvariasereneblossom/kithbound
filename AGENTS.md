# Rules for agents working in this repository

1. **Implementation has not been authorised.** Until `docs/DECISIONS.md` records a go-ahead from the owner, change documents only.
2. Read `docs/16-agentic-implementation.md` before doing anything else. It defines roles, briefs, verification, and guard rails.
3. Specs lead. If code must differ from a spec, change the spec first and record why in `docs/DECISIONS.md`.
4. Do not make design decisions. Report ambiguities and continue with what is unambiguous.
5. Write the least code that meets the spec. Report lines added and removed.
6. Bound every loop and every test run (time and memory). Use only your own worktree and scratch directory.
7. No secrets in files, prompts, or logs. No new dependencies without a decision record.
8. The client is never trusted. Any change that makes the server believe a client-supplied outcome is wrong by definition (`docs/12-security.md`).
9. Items and currency move only through the ledger, in one transaction (`docs/11-architecture.md` §6).
