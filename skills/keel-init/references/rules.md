# KEEL 2.2.0: rules

Reference for the `keel-init` skill, generated from `KEEL.md` (https://github.com/rickflashman/keel). Do not edit by hand.

## Project skills

Skills specific to one project live with that project, not in a developer's personal `~/.claude/skills/` folder, where they would apply to every project on that machine.

- **Location:** `.claude/skills/<skill-name>/SKILL.md`, one folder per skill. Create `.claude/skills/` if it does not exist (note the leading dot).
- **Naming:** Name the file exactly `SKILL.md`. The folder name must match the `name:` line inside it.
- **Using a skill:** Claude Code or GitHub Copilot can pick one up automatically when it is relevant, or a developer can call it by typing `/<skill-name>`.
- **Listing:** When you add, rename, or remove a skill, update the Skills list in `AGENTS.md`. Some tools do not scan `.claude/skills/` on their own, so that list points them there. Add the Skills section from the template with the first skill, and delete it with the last one.

Starting point for a new skill:

```md
---
name: <skill-name>
description: One sentence on what it does and when to use it.
---

# Skill Name
Instructions for the AI go here.
```

## Risk classes

Every change gets 1 class before work starts. The class is defined here once; rules 6, 9, and 10 and the `AGENTS.md` template cite it instead of repeating it. The AI names the class in the ready card or the task summary, so the developer can correct it.

| Class | Trigger | Tests (rule 9) | Review (rule 6) | Plan (rule 10) |
|---|---|---|---|---|
| **Small** | 1 area, no shared code, no ask-before-touching path | Targeted | None | Direct; a ready card if the change touches several files or its approach is unclear |
| **Shared** | Shared code (such as core utilities), the data model, auth, build configuration, a dependency upgrade, a refactor across areas, or a path on the ask-before-touching line in `AGENTS.md` | Affected; ask before a full regression | Fresh-context review of the diff against the acceptance criteria, before the task is done | Ready card, approved first |
| **Release** | A release | Full regression | Fresh-context review and drift report | Release checklist |

## Rules

1. **Humans own SPEC and DESIGN (when it exists),** including the acceptance criteria and their IDs. After the first draft is seeded at setup, the AI may only *propose* edits, as PROPOSED blocks in the file, and must wait for acceptance. Otherwise requirements quietly drift to match whatever the code does.
2. **The AI keeps the docs current.** Every time the application changes, the AI updates the affected files in place, in the same task, not afterward. To find them, it searches `docs/` for each path, name, label, and behavior it changed; the area map and the Glossary give the names. It writes SPEC and DESIGN edits as PROPOSED blocks in the same task, never skips them silently, and never defers doc updates to `HANDOFF.md`. Never create a new version of a doc (such as `SPEC-v2.md`) beside the old one; that leaves several conflicting versions. To change a decision, follow rule 3. How to handle each file:
   - **Update directly:** ARCHITECTURE (including the area map), PLAN (including the work board), the `AGENTS.md` Docs and Skills lists, `.env.example` (when environment variables change), and, when used, MOCKUP, RESEARCH, RUNBOOK, and USER-GUIDE (current user-facing behavior only).
   - **Propose:** SPEC and DESIGN, as PROPOSED blocks (rule 1).
   - **Regenerate:** `DESIGN-GUIDE.html` (when used) every time `DESIGN.md` changes.
   - **`RELEASE-CHECKLIST.md`:** keep one check per accepted acceptance criterion; record results only by running the checks (rule 6).
   - **`HANDOFF.md`** (when used): rewrite at session close through the close routine, or to record something important right away (rule 5), not after every change.
   - **`CHANGELOG.md`** (when used): add one entry at the top at each release (rule 6). Do not rewrite old entries.
3. **Decisions are never rewritten.** To change one, write a new note that supersedes it, then mark the old note's Status as Superseded (the only edit allowed). If 2 notes get the same number, the later merge renumbers its note and fixes the Superseded references.
4. **A task is not done until it meets the Definition of done in PLAN.md.** Test the task as rule 9 says; a task does not need a full regression unless rule 9 calls for one. The result must match the ready card, when there was one; anything outside the card is reported, not done. A deliberate blocker is not an obstacle to solve: a path on the ask-before-touching line, access that was withheld, or a rule in `AGENTS.md`. Report it, and never work around it.
5. **The session handoff is not a record.** A session runs from the start routine to the next close routine.

   **Start routine**, at the start of every session: read the handoff file if it exists, and the work board in `PLAN.md`. Then state the next action in 1 line before you start work. If the handoff file conflicts with another doc, the other doc wins: say so.

   **Close routine**, when the developer says the session is closing, before any compact or clear (for example, `/compact` or `/clear`), or when the session has used most of its context:
   1. **Route.** For each item worth keeping, print 1 line: `item -> target doc`, or `item -> HANDOFF (Next | Waiting | Assumed | Decided | Trap)`. Use the routing table. An item that is already in the current `HANDOFF.md` cannot go back to HANDOFF: route it to its doc, to a work board row, or delete it. The one exception is a Waiting bullet for a PROPOSED block that is still pending; check 8 tracks how old those blocks are.
   2. **Write** the routed items into their docs (PROPOSED blocks for SPEC and DESIGN).
   3. **Rewrite** `HANDOFF.md` (never append) with only what step 1 routed there, and print the line count.

   A close with no routing report is incomplete. `HANDOFF.md` stays within its cap (rule 8). Every bullet is dated; an item older than 1 session is moved or deleted, never copied forward. A copy with a new date is still a copy: the docs check compares the bullet text without its date and counts the carries (check 2). After a close at the context limit, tell the developer to start a new session from `HANDOFF.md`; do not continue on a compacted context. If something important would be lost sooner, record it right away in its proper doc or in `HANDOFF.md`, creating the file if needed. On a team, each developer's `docs/handoff/<developer>.md` follows the same rules.
6. **A release is not approved until the release checklist passes.** Before every release, in this order:
   1. A reviewer in a fresh context (a new session or a subagent, not the one that wrote the code) checks the code against the SPEC acceptance criteria, runs each command in `README.md` Run it and `ARCHITECTURE.md` Commands, and reads `README.md` and `USER-GUIDE.md` (if used) against the code. It reports only gaps that affect correctness, the requirements, or the docs. Fix each gap or list it for the developer.
   2. Write the drift report (see *Docs check and drift report*). The developer fixes or accepts each finding.
   3. Run a full regression (rule 9): the whole test suite, the docs check, and every check in `RELEASE-CHECKLIST.md`. Never mark a check passed without running it.
   4. Confirm that no PROPOSED block remains in `SPEC.md` or `DESIGN.md`. Review new or upgraded dependencies (license, maintenance, known vulnerabilities).
   5. Record how each check was verified, replace the Latest release section with the result, set the Verified date in `SPEC.md` and `ARCHITECTURE.md`, and add the release entry to `CHANGELOG.md` (create the file at the first release).
   6. A human gives the final approval.
7. **Never put secrets in code or docs.** If the project uses environment variables, list their names in `.env.example`. Do not paste secrets or customer data into docs or the tracker. If the project uses version control, make sure `.gitignore` excludes `.env` and the `test-results/` folder (rule 9), creating or updating it if needed. A project uses version control when it has a `.git` folder, or when its docs name a repository or a branch. If you cannot tell, ask the developer once. This is the only version-control step this standard asks for; docs check 2 also reads the history of the handoff file.
8. **Keep docs skimmable.** These are the caps, and the docs check uses the same numbers:
   - `AGENTS.md`: 120 lines and 1,200 words (a warning above either, a failure above 140 lines or 1,500 words), because the AI loads it every session. The word cap catches long lines that a line count misses. The template is a maximum, so delete the optional lines that do not apply. The template in this standard has a budget of 1,100 words, so each project keeps at least 100 words for its own rules; `build/build.py` in the KEEL repository fails above the budget.
   - Each handoff file: 40 lines (a warning above 40, a failure above 50).
   - `CLAUDE.md` and `copilot-instructions.md`: a few lines.

   Other docs have no line limit: if one is too long to skim, trim it or split it by topic, leaving a link in the original. `RESEARCH.md` grows by design; split it by topic if it becomes unwieldy. In `AGENTS.md`, for each line, ask whether removing it would cause mistakes; if not, cut it. Do not include what the AI can learn from the code or already does by default. Give each project rule a weight (Must, Never, or Prefer) and a short reason, because the AI follows a rule it understands: `Never: edit files in vendor/. Why: they are overwritten at each update.` When a rule has an exception, write the exception and its reason into the rule (`Unless: ...`), not into a second rule. An optional rule that many projects can use: `Prefer: a fix that removes code over a fix that adds a layer, a flag, or a special case. Why: less code is less to keep correct. Unless: the removal changes accepted behavior.` Add a rule when a real mistake happens, not in advance. Mark at most 1 or 2 lines "IMPORTANT"; if many lines are marked, none stands out. If the AI keeps breaking a rule, or asks a question that `AGENTS.md` already answers, flag to the developer that the file may be too long or unclear.
9. **Test progressively.** Run only the tests a change needs, as the risk class says. The area map in `ARCHITECTURE.md` lists each area, its code, its tests, its command, its acceptance criteria, and the areas it depends on. An area that a change does not touch is not tested again; its last passing result still holds.
   - **Reading code:** read the map before you search the code. Reading the files near the code you change is normal and keeps your view current. The map is for what is not near: the areas that use the changed code, and long-range dependencies such as shared libraries or code in other folders. Do not search the whole project, or the developer's computer, for something the map already answers.
   - **Targeted (the default):** new and updated tests, plus the existing tests for the changed area, plus the docs check.
   - **Affected:** targeted, plus the tests for each area that uses the changed code. Run them for a Shared change. If a targeted test fails in a way that points outside the changed area, run the affected areas without asking.
   - **Full regression:** the whole test suite, the docs check, and every release check. Run it before every release (rule 6) and when the developer asks. For any other reason, ask the developer first and explain why; a Shared change with a wide reach is the typical reason.
   - **Scripted first:** run the saved test commands from the area map. Each run writes a short results file, `test-results/latest.md`: what ran, the pass and fail counts, and details for failures only. The docs check writes `test-results/docs.md` instead, so both results survive the same task. Attach screenshots only for failures or visual checks. Check the exit code and the counts, not only a "passed" line. Test interactively only to debug a failure, to explore a new check before you script it, or for visual checks.
   - **Saved scripts only:** use the project's test framework when it has one. Save each new test script in the project and add its command to the area map. Each test names the acceptance criterion it covers (`AC-n`) in its name or a comment. Do not write throwaway test scripts. When an acceptance criterion is removed, remove its tests and its `RELEASE-CHECKLIST.md` row in the same task; do not keep tests for behavior that is gone.
   - **Stronger checks (optional):** for a Shared change or a release, the fresh-context reviewer may also test "what if" cases at the edges of the changed logic. Where the project has a mutation testing tool, it may run it on the changed code, to confirm that the new tests have real assertions. The developer decides whether the project uses these checks. Ask once, when you first fill the `ARCHITECTURE.md` Testing section (at setup or upgrade), with this message as plain chat text, and record the answer on its Stronger checks line:

         Optional stronger checks, for Shared changes and releases only:
         1. Not used (most common): the normal tests only.
         2. What-if cases (recommended): the reviewer also tests the edges
            of the changed logic, such as limits, empty values, and times
            that cross midnight. No tool to install; a little more review time.
         3. What-if cases and mutation testing (rare): a tool (such as Stryker for
            JavaScript or mutmut for Python) changes the code on purpose to
            prove that the tests catch it. Needs the tool; much slower.
         Which one (1, 2, or 3)? You can change it later.

     If the developer does not answer, record option 2 and say so in the task summary.
10. **Plan before code.** Classify the change (Risk classes). For a Shared change, a change that touches several files, or a change whose approach is unclear, write the ready card from the `PLAN.md` Definition of ready and wait for approval. Approval of the card approves the plan, not its PROPOSED blocks (see *Proposed changes to SPEC and DESIGN*). A Small change with a clear approach goes ahead directly. Before a bug fix, reproduce the bug with a failing test or captured error output and save the test; no repro, no fix: ask. Never guess a version pin; read the lock file or ask. If the same fix fails twice, stop, report what you tried, and ask. A message that asks a question ("should we", "how hard is it", "why does") asks for an answer, not a change: answer it, and if the change is small and clear, offer it and wait. Before a destructive or irreversible action that the developer did not request, such as deleting files or data, ask first. Routine reversible steps (edits, installs, test runs) need no approval.
11. **Every claim carries evidence.** End every task with the task summary from the `PLAN.md` Definition of done, every line present ("none" if empty); the `AGENTS.md` Reporting section points to it. Write the task summary and the handoff file for a reader who did not see the session: give full paths and names, use the Glossary terms, and do not use a term that the session invented. A test passed only if the results file shows it. A check the AI could not run is reported as "Not run: reason", never as passed. A Shared change is not done until its fresh-context review ran and its findings are fixed or listed. Never weaken, skip, or delete a test to make it pass; report the failure and its cause. After 3 test-fix rounds, stop and report. To dismiss a review finding, cite the file and line that shows it is wrong. Before reporting a task done, re-read the Definition of done and the ready card; this is the cheapest defense against a long context.
