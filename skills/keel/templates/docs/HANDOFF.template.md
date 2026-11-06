> Tracker: TICKET-# (name of project)

# Session handoff
Last updated: YYYY-MM-DD by (tool or developer)

Session handoff only; rewritten at each session close, at most 40 lines. If anything here conflicts with another doc, the other doc wins.
Close routine: (1) Route: print 1 line per item worth keeping, `item -> target`, using the routes below. An item already in this file cannot route back here, except a Waiting bullet for a pending PROPOSED block. (2) Write the routed items into their docs (PROPOSED blocks for SPEC and DESIGN). (3) Rewrite this file, never append, with only the leftovers; print the line count.
Routing: a choice -> docs/decisions/ | results or evidence -> docs/RESEARCH.md | milestones and unfinished tasks -> docs/PLAN.md | commands, structure, environment facts, lasting traps -> docs/ARCHITECTURE.md | release results -> docs/RELEASE-CHECKLIST.md | requirement or UI changes -> PROPOSED block in SPEC.md or DESIGN.md. Only leftovers stay here, in the 5 sections below (Decided = Decided without asking; Traps = Traps found this session). Date every bullet; move or delete anything older than one session.

## Where we left off
(Two sentences: what was being worked on and its state, naming any file partly changed. History goes in CHANGELOG.md or PLAN.md, not here.)

## Next
- (YYYY-MM-DD) (The immediate next action within the current task, with the file it touches; 1 to 3 bullets. Milestone-level work goes in PLAN.md.)

## Waiting
- (YYYY-MM-DD) (A question waiting on a named person, or a pending PROPOSED block: file and AC ID or heading.)

## Assumed
- (YYYY-MM-DD) (An unconfirmed assumption, and what would confirm it.)

## Decided without asking
- (YYYY-MM-DD) (A choice the AI made on its own, with the evidence. Target: docs/decisions/ or a PROPOSED block.)

## Traps found this session
- (YYYY-MM-DD) (A surprise or workaround.) Move to: (ARCHITECTURE Known traps / RESEARCH / decisions)
