---
name: keel-check
description: "KEEL 2.1.0 docs check, drift report, and on-request reviews. Writes or runs the project's docs check script (AGENTS.md size and stamp, handoff size and carried bullets, AC coverage in the release checklist, broken paths and links, unlisted docs, headers, stray doc versions, stale PROPOSED blocks, secrets) and writes the read-only drift report (SPEC with no code, code with no SPEC, ACs with no test, stale doc sections). Only when the developer asks: a cross-compare review of all docs and the code, or a docs cleanup. Use in a KEEL project when the developer runs keel-check (/keel:keel-check or /keel-check), says 'run the docs check', 'run a drift report', 'run a cross-compare review', 'run a docs cleanup', or before a release."
argument-hint: "[docs | drift | both | cross-compare | cleanup]"
metadata:
  version: "2.1.0"
  source: "https://github.com/rickflashman/keel"
---
# KEEL 2.1.0: docs check, drift report, and on-request reviews

Run the documentation checks of KEEL version 2.1.0 (November 6, 2026), https://github.com/rickflashman/keel. All of them are defined below.

If the developer gave instructions with the command, follow them: $ARGUMENTS

## Which task

- **Docs check** and **drift report:** steps 1 to 4 below. With no other instruction, run both.
- **Cross-compare review** or **docs cleanup:** only when the developer asks for it by name. They use many tokens. Do step 1, then follow *Cross-compare review and docs cleanup* below.

## Steps

1. **Confirm the project uses KEEL.** `AGENTS.md` has an `Aligned to KEEL` stamp (or an older *AI-PROJECT-DOCS-STANDARD* stamp). If not, stop and tell the developer to run `keel` first.
2. **Docs check.** Find the script in the area map row *Docs* in `docs/ARCHITECTURE.md`. If it does not exist, write it as the definition below says (the project's own language, no new dependency, the list of checks in its header comment, results to `test-results/docs.md`), add its row to the area map and its command to the Testing section, then run it. If it exists, run it and compare its header comment with the list below; propose any missing check to the developer before you add it. Report the result from `test-results/docs.md`, with the exit code. Never weaken a check to make it pass.
3. **Drift report.** Write it as the definition below says: each finding with its certainty, never averaged. It changes nothing. Put it in the task summary; lasting findings go to `docs/RESEARCH.md` (create it from the standard if needed).
4. **Report** with the task summary from the `docs/PLAN.md` Definition of done. A check you could not run is "Not run: reason", never passed.

When the developer asked for only 1 of the 2 checks, do that one. Before a release, both run (rule 6 in `references/standard.md`).

## Docs check and drift report

**Docs check** is a saved script in the project's own language, with no new runtime or dependency. When the project has no language yet, check which runtimes are installed on the developer's machine and use one of them; if none fits, write a shell script (`sh` on macOS and Linux, PowerShell on Windows). Port the script when the stack is chosen, and record that in `ARCHITECTURE.md`. It is the one script this standard asks for. It is listed in the area map as area *Docs* and runs with the targeted tests of every change; a red check is a failing test. The AI writes it at setup or upgrade and keeps the list of checks, with these definitions, in the script's header comment, so the list survives without this skill. It writes its results to `test-results/docs.md`, so the area test results in `test-results/latest.md` survive the same task. It checks:

1. `AGENTS.md` is within its caps (rule 8: a warning above 120 lines or 1,200 words, a failure above 140 lines or 1,500 words) and has the version stamp.
2. `docs/HANDOFF.md` and each `docs/handoff/*.md` is within its cap (rule 8: a warning above 40 lines, a failure above 50), and every bullet starts with a date. No bullet is carried forward: compare each bullet, without its date, with the earlier versions of the same file. A bullet that was also in the previous version is carried once (a warning). A bullet that was also in the 2 previous versions is carried twice (a failure). Waiting bullets that name a pending PROPOSED block are not counted (check 8 covers them). Compare only when the file differs from the last version the check saw, so repeated runs in one session do not count as carries. With version control, read the earlier versions from the history. Without it, the check keeps its own snapshot in `test-results/handoff-previous.md` (on a team, `test-results/handoff-previous-<developer>.md` for each handoff file): the bullets of the last version it saw, each with its carry count. The check updates the snapshot only when the file changed. The AI never edits the snapshot.
3. Every `AC-n` in `SPEC.md` outside PROPOSED blocks (lines that start with `>`) has a row in `RELEASE-CHECKLIST.md`, and every row has such an AC.
4. Every relative path and link in `README.md` and `docs/` exists. For every command in the area map and in `ARCHITECTURE.md` Commands, the script file or first word it names exists; the check does not run it.
5. Every file in `docs/` is listed in `AGENTS.md`, or on its Not used or Kept as is line (contents of `docs/decisions/`, `docs/images/`, `docs/plans/`, `docs/handoff/`, and `docs/archive/` are covered by their folder's entry). Every other doc file (`.md`, `.txt`, `.rst`, `.adoc`, `.docx`, `.pdf`) in the project root is a file from this standard, a common repository file (`LICENSE`, `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, with any extension), an older `AI-PROJECT-DOCS-STANDARD.md`, or on the Kept as is line. A file that fails is a candidate to absorb (see `references/standard.md`). The check does not scan subfolders such as package READMEs; the AI finds those while it works.
6. Every doc, except decision notes and `docs/archive/`, starts with a header line of the form `> Name: text`.
7. No file in `docs/` has a standard doc name plus a suffix such as `-v2`, `-old`, `-new`, `-copy`, or `-final`.
8. PROPOSED blocks older than 14 days are listed (a warning, not a failure).
9. No assignment-shaped secret in docs: `password`, `secret`, `api_key`, or `token`, followed by `=` or `:` and a value, or a private key header.

**Drift report** is a read-only report the AI writes before every release (rule 6) and whenever the developer asks ("run a drift report"). It never blocks and changes nothing. Each finding carries a certainty, and findings are never averaged. *Certain*: a mechanical check proves it. *Evidence*: the AI found it by reading, and the developer confirms it. *Inference*: a guess from dates, not from behavior.

| Finding | Certainty | How found |
|---|---|---|
| A SPEC feature or AC with no code behind it | Evidence | Area map ACs column against the code |
| Code with no SPEC coverage | Evidence | Folders not in any area map row, or rows with no ACs |
| An AC with no test, or a test with no AC | Certain | Area map ACs column against tests that cite `AC-n` in a name or comment |
| A command, folder, file, or link in the docs that does not exist | Certain | The docs check (paths, links, commands); the reviewer for anything else |
| A doc section older than the code it describes | Inference | The Verified date against the change history: version-control history if any; else the work board Started dates and `CHANGELOG.md`; else "Not run: no change history". Compares dates, not behavior. |

The report goes in the task summary. Lasting findings go to `RESEARCH.md`. At a release, each finding is fixed or accepted by the developer.

## Cross-compare review and docs cleanup

These 2 reviews read every doc, and the cross-compare review also reads the code. They use many tokens, so they run only when the developer asks. Never start one on your own. Only 2 places mention them unasked: the 1 line at the end of an as-built review (Initialization rules), and the report of an upgrade to 2.1.0. Use a subagent per doc or per area where the tool allows, so the main context stays small.

**Cross-compare review** ("run a cross-compare review"): a full comparison of all the docs with each other and with the code. It is most useful after a migration or an as-built review, when much of the content is new or inferred.

1. Read every doc in the `AGENTS.md` Docs list, and the code of each area in the area map.
2. Report each finding in a table, with the files and sections involved:

   | Finding | Example |
   |---|---|
   | Docs that disagree | `SPEC.md` says sessions last 30 minutes; `ARCHITECTURE.md` says 60 |
   | A doc that disagrees with the code | `ARCHITECTURE.md` names a command that the code no longer has |
   | Duplicated content | The same setup steps are in `README.md` and `ARCHITECTURE.md` |
   | A link that is not needed | A link to a section that repeats what the reader just read |
   | Superseded content still present | A section that describes an approach a decision note replaced |

3. For each disagreement between docs, say which doc wins by the Precedence line in `AGENTS.md`. For a doc that disagrees with the code: `ARCHITECTURE.md` and `PLAN.md` are fixed to match the code; for `SPEC.md` or `DESIGN.md`, write a PROPOSED block or report a gap in the code, and ask which one. If neither rule decides it, ask.
4. For each duplicate, name its home by the routing table. The home keeps the content; the other places delete it or keep 1 link.
5. Wait for the developer to fix or accept each finding, or to approve the fixes as a set. Then apply the approved fixes: direct edits, except PROPOSED blocks for accepted SPEC and DESIGN text, and a superseding note for a decision.
6. Run the docs check, and end with the task summary.

**Docs cleanup** ("run a docs cleanup"): the AI removes what makes the docs longer without making them more useful. It makes the edits itself, without a separate approval for each one, and lists every change in the task summary.

- Remove history that no current reader needs, such as "we used to" text in current docs.
- Remove links that are not needed, and repeated content outside its home doc (the routing table names the home).
- Remove commentary that does not help a reader do a task or make a decision.
- Delete Known traps that are no longer true. In `RESEARCH.md`, mark such entries "Outdated (date)", as its template says.
- Limits: write PROPOSED blocks for accepted SPEC and DESIGN text. Never delete `RESEARCH.md` entries. Never edit decision notes, old `CHANGELOG.md` entries, `docs/archive/`, or `KEEL.md`.
- Run the docs check, and end with the task summary. List each removal under Docs updated.

---

This skill is generated from `KEEL.md` in https://github.com/rickflashman/keel. Do not edit it by hand; change `KEEL.md` and run `python3 build/build.py`.
