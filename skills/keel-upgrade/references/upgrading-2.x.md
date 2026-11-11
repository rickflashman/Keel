# KEEL upgrades from 1.5.1 to 2.1.2

The upgrade steps ("version blocks") for versions 2.0.0 to 2.1.2 of KEEL. They moved here from `KEEL.md` in 2.2.0, when 1 general upgrade rule replaced the steps for each version. Nothing was changed, and no new block is added here.

Use this file only for a project whose stamp is older than 2.1.2. Apply the blocks in order, from the project's version, as step 8 of *Migrating or upgrading an existing project* in `KEEL.md` says. Then continue with the general rule in that step. For a stamp older than 1.5.1, apply the blocks in `UPGRADING-1.x.md` first.

Rule numbers and section names in the blocks refer to `KEEL.md`. In a skill, they are in `SKILL.md` or in `references/`. The changelog entries for these versions are at the end of `KEEL.md`.

## Upgrade blocks

- **1.5.1 to 2.0.0:** The standard is now KEEL. New headings and template lines are structure: add them directly once the developer approves the plan, with "(Not documented yet.)" as content. Only changes to accepted SPEC or DESIGN text go through PROPOSED blocks. Headers keep their `> Jira:` form; the standard now accepts any tracker name.
  1. Root: during this version the root may hold `AI-PROJECT-DOCS-STANDARD.md` beside or instead of `KEEL.md`; follow `KEEL.md`. Set the stamp to `Aligned to KEEL 2.0.0 (https://github.com/rickflashman/keel)`. An existing stamp `Aligned to KEEL v2.0.0` is the same version in its earlier form: update it to the new form, and the `KEEL.md` line after it, the next time you edit `AGENTS.md`.
  2. `AGENTS.md`: replace every template section with its 2.0.0 version (all changed; Reporting and the precedence line are new; the cap is about 120 lines), keep the project's own lines, and add the Conventions lines for ask-before-touching paths and area owners. Propose cuts under rule 8 and wait for approval.
  3. `ARCHITECTURE.md`: convert the test map to the area map (add Code, Entry points, ACs, and Confidence; mark rows Inferred); add `Verified: (not yet verified)` under the header; add the Operations section with Known traps, the data lifecycle line, and the Testing lines for the docs check, the AC citation, and the results files; write the docs check script and add its row.
  4. `SPEC.md`: add `Verified: (not yet verified)`, the Glossary and Constraints headings, and the Area name on each feature group. Convert each proposed SPEC or DESIGN change waiting in the old `HANDOFF.md` Open questions into a PROPOSED block.
  5. `PLAN.md`: add the Definition of ready, replace the Definition of done, and convert Current / Next to the work board.
  6. `RELEASE-CHECKLIST.md`: replace the Release steps and the Result values.
  7. `README.md`: add Owners and support.
  8. `CLAUDE.md`: extend the compaction note.
  9. `HANDOFF.md`: run the close routine on it: route each item, move lasting gotchas to Known traps or `RESEARCH.md`, and rewrite it with the dated sections. If more than 1 developer works on the project, create `docs/handoff/<developer>.md` per developer and retire `docs/HANDOFF.md`.
  10. Offer the as-built review; it sets the Verified dates.
- **2.0.0 to 2.1.0:** Add the new headings and template lines directly once the developer approves the plan. Change no accepted SPEC or DESIGN text.
  1. `AGENTS.md`: replace every template section with its 2.1.0 version: most sections are shorter, the close routine steps moved to the handoff file, Testing is shorter, and the Code section is gone. Keep the project's own lines: Conventions, Ask before touching, Area owners, the Docs entries for files that exist, and the Skills list. Move the old Code lines into Conventions, each with a weight and a reason, and offer to add a weight and a reason to each other Conventions rule (rule 8). Set the stamp to `Aligned to KEEL 2.1.0 (https://github.com/rickflashman/keel)`, and add the "Kept as is" line.
  2. `ARCHITECTURE.md`: add the Environment facts section, with "(Not documented yet.)" as content. Add the Stronger checks line to Testing, and ask the developer the stronger checks question in rule 9 (word for word; it explains each option and recommends the simpler one). Delete the "How to run tests" and "Results file" lines from Testing; those rules are now in `AGENTS.md`. Replace the guidance line of the area map.
  3. Decision template: new notes have a Date line. Do not edit existing notes.
  4. `HANDOFF.md`: in the header, change "about 40 lines" to "at most 40 lines", and add the Close routine line from the template. In the Routing line, add "and unfinished tasks" to the PLAN.md route, and "environment facts" to the ARCHITECTURE.md route.
  5. Docs check: use the caps of rule 8 in checks 1 and 2 (`AGENTS.md`: a warning above 120 lines or 1,200 words, a failure above 140 lines or 1,500 words; handoff: a warning above 40 lines, a failure above 50). Add the carry count to check 2, and the root folder files and the Kept as is line to check 5. Update the script's header comment. Run it. Report any root doc files it finds, and offer to absorb them.
  6. Tell the developer that the cross-compare review and the docs cleanup are available on request.
- **2.1.0 to 2.1.1:** In `AGENTS.md` Conventions, add the 2 default code rules from the template (no clever or compressed code; header and doc comments), unless the project already has rules that cover them or the developer removed them on purpose. Set the stamp to `Aligned to KEEL 2.1.1 (https://github.com/rickflashman/keel)`.
- **2.1.1 to 2.1.2:** Add the new lines directly once the developer approves the plan. Change no accepted SPEC or DESIGN text. Where a step says "its 2.1.2 version", use the text of the current template, because the standard has only the current templates.
  1. `AGENTS.md`: in Making changes, add the question line as the first bullet. In Always / Never, add the Ask first line. In Conventions, add "update comments with the code" to the comment rule, if the project still has that rule, and `Unless:` to the rule form line. In Session handoff, replace the last bullet with its 2.1.2 version; the cap and the bullet rules are now only in the handoff file header. Set the stamp to `Aligned to KEEL 2.1.2 (https://github.com/rickflashman/keel)`.
  2. Each handoff file: replace the "Session handoff only" line and the "Close routine" line in the header with their 2.1.2 versions.
  3. `PLAN.md`: in the Definition of done, replace the line that introduces the task summary with its 2.1.2 version.
  4. `MOCKUP.md` (when used): no change. The Baseline is an example, and the project's own text stays.
