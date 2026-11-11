> GitHub: #42 (Room Booking API)

# Session handoff
Last updated: 2026-11-05 by Claude Code

Session handoff only; rewritten at each session close, not after every change, at most 40 lines. If anything here conflicts with another doc, the other doc wins. Write for a reader who did not see the session: full paths and names, Glossary terms, no terms invented during the work.
Close routine: (1) Route: print 1 line per item worth keeping, `item -> target`, using the routes below. An item already in this file cannot route back here, even with a new date (the docs check counts carries), except a Waiting bullet for a pending PROPOSED block. (2) Write the routed items into their docs (PROPOSED blocks for SPEC and DESIGN). (3) Rewrite this file, never append, with only the leftovers; print the line count.
Routing: a choice -> docs/decisions/ | results or evidence -> docs/RESEARCH.md | milestones and unfinished tasks -> docs/PLAN.md | commands, structure, environment facts, lasting traps -> docs/ARCHITECTURE.md | release results -> docs/RELEASE-CHECKLIST.md | requirement or UI changes -> PROPOSED block in SPEC.md or DESIGN.md. Only leftovers stay here, in the 5 sections below (Decided = Decided without asking; Traps = Traps found this session). Date every bullet; move or delete anything older than one session.

## Where we left off
The maximum booking length (#57) is blocked until Dana accepts AC-7. No code file is partly changed.

## Next
- (2026-11-05) Start room search by capacity (#58): add `GET /rooms?min_capacity=` in src/routes/rooms.js.

## Waiting
- (2026-11-05) Dana: accept or reject the PROPOSED block SPEC.md AC-7 (4-hour maximum).

## Assumed
- (2026-11-05) The client app sends end times in UTC. Confirm with the client team before #57 starts.

## Decided without asking
(none)

## Traps found this session
(none)
