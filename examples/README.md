# KEEL examples

This folder shows what a project looks like after KEEL is set up and has run for a few sessions.

| Example | What it shows |
|---|---|
| [`room-booking-api/`](room-booking-api/) | A small API service with no user interface: the 8 required files, a decision note, a session handoff, a pending PROPOSED block, the work board, and a release record |

The project, its code, and its people are invented. The example holds only the documentation, not the code. The docs check script (`scripts/check-docs.mjs`) is also left out: KEEL writes that script for each project in the project's own language. The code paths named in the docs, such as `src/routes/bookings.js`, are absent too, so docs check 4 would report them in this example.

Read the files in this order:

1. `AGENTS.md`: the rules that every AI tool loads at the start of a session.
2. `docs/HANDOFF.md`: where the last session stopped.
3. `docs/PLAN.md`: the work board and the Definition of done.
4. `docs/SPEC.md`: the requirements, with a pending PROPOSED block.
5. `docs/ARCHITECTURE.md`: the area map, the environment facts, and the testing commands.
6. `docs/RELEASE-CHECKLIST.md`, `docs/decisions/`, and `docs/CHANGELOG.md`.
