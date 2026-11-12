> GitHub: #42 (Room Booking API)
(Milestones and the work board. Every Shared or multi-file change gets a board row; a Small one-file change needs none. Track smaller tasks in the tracker.)

## Definition of ready
A Shared change, a change that touches several files, or a change whose approach is unclear starts only after the developer approves this card (classes: AGENTS.md Making changes):

    Change: (name, one line)
    Class: (Small / Shared / Release, and why)
    ACs: (added or changed; each written now as a PROPOSED block in SPEC.md or DESIGN.md, named by file and AC ID or heading)
    Files: (paths, with their areas from the area map in ARCHITECTURE.md)
    Tests: (to add or run, and the affected areas)
    Docs: (to update)
    Out of scope: (what this change will not do)

    Do you approve this ready card? Reply "yes", or tell me what to change.

## Definition of done
Before calling a task done:
- Tests added/updated; targeted tests and the docs check pass, plus affected areas if other code uses the changed code (see the AGENTS.md Testing section; no full regression unless it calls for one)
- Shared change: fresh-context review of the diff against the ACs done; findings fixed or listed
- Area map in ARCHITECTURE.md updated if code, tests, or areas changed
- Task summary in this shape, every line present ("none" if empty), written for a reader who did not see the session (full paths, Glossary terms, no terms invented during the work):

      Changed: (files)
      Class: (Small / Shared / Release)
      Tested: (command -> pass/fail counts; results file; a check you could not run is "Not run: reason", never passed)
      Reviewed: (fresh-context result for a Shared change, or n/a; a dismissed finding cites the file and line that shows it is wrong)
      Docs updated: (files) | Docs checked, no change: (files; search terms used)
      Proposed: (PROPOSED blocks by file and AC ID or heading)
      Decided without asking: (one line each)
      Skipped: (step and why)
      Next: (one action)

- Result matches the ready card, if there was one; anything outside it is reported, not done
- Code is readable and documented per the AGENTS.md Conventions
- Affected docs updated (SPEC and DESIGN changes written as PROPOSED blocks; all other docs updated directly)
- RELEASE-CHECKLIST.md updated if acceptance criteria changed
- `.env.example` updated if environment variables changed
- AGENTS.md Docs and Skills lists updated if files or skills changed
- Work board row updated, if the change has one (Status; Done when finished)

## Milestones
- [x] M1: Bookings and email (Dana; released as 1.0.0 on 2026-10-30)
- [ ] M2: Booking limits and room search (Dana; in progress)

## Work board
(One row per work item in the current milestone. Claim an item by setting Owner and Status before you start. Two In progress rows may not list the same file; if another row holds a file you need, do not start: add the dependency and pick another item or ask. Mark Done; delete nothing. When open rows pass about 15, move this table to docs/plans/M<n>.md and keep only the milestones here. A solo developer keeps the board too; it stays short.)
| Item | Owner | Areas | Files | Status | Branch or ticket | Started |
|---|---|---|---|---|---|---|
| Maximum booking length (AC-7, PROPOSED) | Dana / Claude Code | Bookings | src/routes/bookings.js, tests/bookings.test.js | Blocked: waiting for AC-7 acceptance | #57 | 2026-11-05 |
| Room search by capacity | Dana / Claude Code | Bookings | src/routes/rooms.js, tests/rooms.test.js | Ready | #58 | |
| Audit the email retry table for stuck rows | Dana / Codex | Notifications | src/notify/retry.js | Ready | #59 | |
