# Project rules
AI working rules for every AI tool on this project; CLAUDE.md and .github/copilot-instructions.md only point here. At most 120 lines and 1,200 words.

## Project documentation standard
- Aligned to KEEL 2.2.0 (https://github.com/rickflashman/keel).
- Authority for the doc structure: `KEEL.md` in the project root, else the KEEL skill, else the existing docs; read it before you change that structure. Never change, move, or delete `KEEL.md` unasked.
- Precedence, highest first: direct developer instruction > docs/decisions/ > SPEC.md and DESIGN.md (accepted text, not PROPOSED blocks) > ARCHITECTURE.md and PLAN.md > RUNBOOK.md, USER-GUIDE.md, and MOCKUP.md (mockups only) > HANDOFF.md and RESEARCH.md > CHANGELOG.md (history only) > the code (evidence only) > docs/archive/ (never a source).

## Commands
- In README.md (from the ticket) and docs/ARCHITECTURE.md. Record new commands in ARCHITECTURE.md only.

## Conventions
- Never: clever or compressed code; if performance needs it, comment why. Why: the next reader, human or AI, must understand it without the original session.
- Must: a header comment on every file and a doc comment on every function or component (what, inputs, outputs); inside, comment only the why of non-obvious logic; update comments with the code. Why: sessions start without memory, so the code must explain itself.
- (Add a project rule when a real mistake happens: `Must | Never | Prefer: the rule. Why: the reason. Unless: the exception.`)
- Ask before touching: (paths that always need a ready card, such as auth, migrations, CI; or "(none)")
- Area owners: (optional: area, owner, review required; delete if unused)

## Making changes
- A question gets an answer, not a change; offer a small, clear change and wait.
- Classify first, then act by class:
  - Small (1 area, no shared code): change directly; targeted tests.
  - Shared (shared code, data model, auth, build config, a dependency upgrade, a cross-area refactor, or an ask-before-touching path): an approved ready card (docs/PLAN.md Definition of ready) first; affected tests; then a fresh-context review of the diff against the ACs.
  - Release: docs/RELEASE-CHECKLIST.md, with a full regression.
- A multi-file change or an unclear approach also needs a ready card. Claim a work board row before a Shared or multi-file change.
- Bug fix: first reproduce it with a saved failing test or captured error output; no repro, no fix. Never guess a version pin. If the same fix fails twice, stop and ask.
- Never work around a deliberate blocker (an ask-before-touching path, withheld access): report it.
- Update the affected docs in the same task: search docs/ for each path, name, label, and behavior you changed (names: area map, SPEC Glossary). Edit docs directly, except SPEC and DESIGN: write a PROPOSED block next to the text, never only in chat (`> PROPOSED YYYY-MM-DD (tool): the edit. Reason: ... Code: path.`).
- Doc files outside this standard (notes, Word files, a second README): offer to absorb each one into its target docs and archive the original; do not link to it as a source.

## Testing
- Read the area map in docs/ARCHITECTURE.md before you search the code; use it for what is not near your change.
- Targeted: new and updated tests, the changed area's tests, and the docs check. Affected: targeted plus the areas that use the changed code, or any area a failure points to. Never retest unchanged areas. A full regression outside a release needs the developer's approval.
- Run only saved commands from the area map; save each new test and add its command; each test names the AC it covers. Each command writes test-results/latest.md; check the exit code and the counts. Test interactively only to debug, to explore a new check, or for visual checks.
- Never weaken, skip, or delete a test to make it pass; report the failure and its cause. After 3 test-fix rounds, stop and report.

## Reporting
- Lead with the result. No preamble, recap, or effort estimates; lists of at most 5 items. Show errors and failing tests in full.
- Before reporting done, re-read the Definition of done in docs/PLAN.md and your ready card, and end with the task summary it defines, every line present. A check you could not run is "Not run: reason", never passed.

## Decisions
- Before a structural or technology choice, check docs/decisions/. Never reverse a decision: write a new note, then set the old note's Status to "Superseded by NNNN", the only edit allowed.
- Record a choice that is hard to reverse or affects several files: docs/decisions/NNNN-short-title.md, the next number, the format of the newest note or the KEEL template, no ticket ID.

## Session handoff
- Handoff file: docs/HANDOFF.md (teams: docs/handoff/<developer>.md). Start routine: read it and the work board in docs/PLAN.md; state the next action in 1 line.
- Close routine, when the developer closes the session, before any compact or clear, or when most of the context is used: follow the steps and routes at the top of the handoff file (if it is missing, use the KEEL handoff template).
- If something important would be lost before the close, record it now. On a conflict, the other docs win: flag it.

## Always / Never
- Always: mention the ticket ID when describing work.
- Never: put secrets in code or docs; edit docs/archive/.
- Ask first: a destructive or irreversible action the developer did not request, such as deleting files or data.

## Docs (read the one that matches your task)
- docs/SPEC.md: requirements, glossary, constraints.
- docs/ARCHITECTURE.md: area map, structure, commands, environment facts, operations. Read before structural changes.
- docs/DESIGN.md: before touching UI.
- docs/USER-GUIDE.md: end-user guide in STE, for current behavior.
- docs/RUNBOOK.md: operations for support staff.
- docs/MOCKUP.md: before mockups or a large UI change.
- docs/RESEARCH.md: before researching again; save research worth keeping.
- Not used (developer decision; do not recreate or report missing): (none)
- Kept as is (developer decision; do not offer to absorb): (none)

## Skills (.claude/skills/<skill-name>/SKILL.md)
- When a task matches a skill listed here, follow its SKILL.md.
- (`<skill-name>`: one-line purpose.)
