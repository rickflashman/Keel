---
name: keel
description: "KEEL 2.2.0, the documentation and working standard for AI-developed projects. Sets up, migrates, or upgrades a project's docs (README, SPEC, ARCHITECTURE, PLAN, RELEASE-CHECKLIST, AGENTS.md) and chooses the right path on its own. Use when the developer runs the keel skill (/keel:keel or /keel), says 'apply KEEL', 'set up KEEL', or 'upgrade to the latest KEEL', or is not sure which KEEL task they need. Task skills: keel-init, keel-upgrade, keel-check, keel-as-built, keel-interview."
disable-model-invocation: true
argument-hint: "[init | upgrade | switch | check | as-built | interview | free text]"
metadata:
  version: "2.2.0"
  source: "https://github.com/rickflashman/keel"
---
# KEEL 2.2.0

A documentation and working standard for AI-developed projects. Version 2.2.0 (November 11, 2026), https://github.com/rickflashman/keel.

Like the keel of a ship, it keeps a project steady and on course when the AI tool, the AI model, or the developer changes. It keeps the AI's work organized, supportable, and token-efficient. This is the general skill: it looks at the project, chooses the right path, and does it. The developer may also call 1 task directly:

| Skill | Task |
|---|---|
| `keel-init` | New setup from the templates, for a project with no docs yet |
| `keel-upgrade` | Migrate existing docs, upgrade an older KEEL version, or switch a project from the `KEEL.md` file to this installed skill (the skill then replaces the file as the reference) |
| `keel-check` | Write or run the docs check script, write a drift report, and, on request, run a cross-compare review or a docs cleanup |
| `keel-as-built` | The as-built review: area map, SPEC from code, differences and health |
| `keel-interview` | The SPEC interview: fill the gaps in `SPEC.md` as PROPOSED blocks |

If the developer gave extra instructions with the command, follow them: $ARGUMENTS

## How to use this skill

1. Read *Initialization rules* below and choose 1 path: Upgrade, Migration, New setup, or Switch to the skill. Tell the developer which path you chose and why before you continue.
2. For a Migration or an Upgrade, read `references/migration.md` and follow it. `references/changelog.md` lists the changes in each version. For a Migration, first read `references/files.md`.
3. For a New setup, first read `references/files.md`: the folder layout, the documentation files, and the tracker link. Then create the files from `templates/` (guidance: `templates/README.md`), in the numbered order of the documentation files table.
4. When the path is complete, run `references/checklist.md` and report. Then make the offers that the After the path rule names: the as-built review and the SPEC interview.
5. The developer approves before any existing file changes. Never delete, rename, move, or modify `KEEL.md` without the developer's explicit instruction.

The rest of this file is the part of the standard that every path needs: authority, the initialization rules, the docs check, and the stronger checks question. The working rules (rule 1 to rule 11), the risk classes, and project skills are in `references/rules.md`. Read it only when a step or a template names a rule and you need its full text.

## Authority and file handling

When `KEEL.md` is in the project root, it is the authoritative reference for project structure, documentation standards, naming conventions, required, recommended, and optional artifacts, and templates and seed content, including for documentation created later. Direct developer instructions and explicit project-specific requirements override it. When project docs disagree with each other, the Precedence list in `AGENTS.md` says which one wins.

AI assistants may read and use the file, but must not delete, rename, move, modify, archive, or otherwise manage it without direct, explicit developer instruction. The developer controls its location and lifecycle, and may ask an AI assistant to update it.

When `KEEL.md` is absent and the KEEL skill is installed, the skill is the reference for the same things. When both are present, `KEEL.md` wins; if their versions differ, tell the developer before you continue.

## Initialization rules

- **Check first:** Before you create anything, check the project and choose 1 path:
  - **Upgrade:** `AGENTS.md` (or `CLAUDE.md` before 1.2.0) has a version stamp older than this skill's version. A stamp that names *AI-PROJECT-DOCS-STANDARD* is an older version of this standard (KEEL 1.x). Follow the upgrade-only path in `references/migration.md`.
  - **Migration:** the project has any file that the Inventory step would find, even a single `README.md` or rules file. A project with a stamp is never a Migration: it is an Upgrade, or it is up to date. A file outside the standard in such a project, such as a package README, is a candidate to absorb (see `references/files.md`). Follow `references/migration.md`.
  - **New setup:** the project has none of these. Create the files from the templates in `templates/` (guidance: `templates/README.md`).
  - **Switch to the skill:** the KEEL skill is running, `KEEL.md` is in the root folder, and the developer asked to switch. Run the Upgrade path first if the stamp is older than the skill's version. Then set the stamp to the skill's version and, only on the developer's explicit instruction, delete `KEEL.md` or move it where the developer says. Report what changed.

  If the root folder also holds `AI-PROJECT-DOCS-STANDARD.md`, it is an older version of this standard: follow this skill, and tell the developer. When the upgrade is complete, ask the developer once whether to delete the old file; do not delete, move, or archive it without that answer. Tell the developer which path you chose and why before you continue. If the stamp is this skill's version and there is nothing to bring in, say that the project is up to date, and stop.
- **Missing information:** Before you create any file, send the developer this message, with all 3 questions, as plain chat text, and wait for the answer. Do not use a question tool or form: the answers are free text. Skip only a question the developer already answered. Never take the project name from the folder name. If the developer does not answer an item, leave its placeholder.

      Before I create the files, 3 questions:
      1. Tracker and ticket ID, with the ticket link if you have one (or "none"):
      2. Project name and a short description:
      3. Does the project have a user interface (yes or no)?
- **Existing files:** Never overwrite a file or folder from this standard that already exists. Merge the new content in, or propose the change, and ask the developer first.
- **After the path:** When the path is complete, run the Initialization checklist and report. Then, if the project has code, offer the as-built review. After an Upgrade, offer it only when `ARCHITECTURE.md` has no Verified date. After you seed or migrate `SPEC.md`, offer the SPEC interview; when you also offer the as-built review, offer the interview after it.
- **As-built review:** A review of existing code, offered after the path for a project with code (see After the path). The developer may decline or choose only some areas. On approval, run the `keel-as-built` skill, which holds the steps. If that skill is not installed, follow the As-built review steps in the Initialization rules of `KEEL.md` (https://github.com/rickflashman/keel).
- **SPEC interview:** Interview the developer about the gaps in `SPEC.md`: edge cases, non-goals, trade-offs, constraints, glossary terms, and acceptance criteria. Ask a few questions at a time, and skip questions the docs already answer. Write the resulting SPEC changes as PROPOSED blocks for acceptance (rule 1). The developer may decline.

## Docs check and drift report

**Docs check** is a saved script in the project's own language, with no new runtime or dependency. When the project has no language yet, check which runtimes are installed on the developer's machine and use one of them; if none fits, write a shell script (`sh` on macOS and Linux, PowerShell on Windows). Port the script when the stack is chosen, and record that in `ARCHITECTURE.md`. It is the one script this standard asks for. It is listed in the area map as area *Docs* and runs with the targeted tests of every change; a red check is a failing test. The AI writes it at setup or upgrade and keeps the list of checks, with these definitions, in the script's header comment, so the list survives without this skill. The first line of that comment names the version that the list comes from, in the form `Docs check for KEEL X.Y.Z`, with this skill's version. It writes its results to `test-results/docs.md`, so the area test results in `test-results/latest.md` survive the same task.

These rules apply to every check:

- **Result:** A check passes, passes with warnings, or fails. The script ends with exit code 0 when no check fails, and with exit code 1 when a check fails. `test-results/docs.md` starts with the first line of the header comment, the date, the result (PASS or FAIL), and the counts. Then it has 1 line for each check, and 1 line for each warning and each failure, with the file and the line number. A finding about a whole file gives line 1.
- **The docs:** `README.md` and every Markdown file in `docs/`. Checks 4, 6, and 7 do not read `docs/archive/`, because archived files are never edited.
- **Lines in `AGENTS.md`:** On the Not used line and the Kept as is line, the items come after the colon, with commas between them. A note in parentheses after an item is not part of its name, and "(none)" means no items.
- **PROPOSED block:** a group of lines that start with `>`, in `SPEC.md` or `DESIGN.md`. Its first line starts with `> PROPOSED`, then a date in the form `YYYY-MM-DD`. A block with no date is still a block (check 8).
- **A missing file:** If `AGENTS.md` does not exist, check 1 fails. If `SPEC.md` or `RELEASE-CHECKLIST.md` does not exist, check 3 fails. In every other check, a file that does not exist gives no finding, and without `AGENTS.md` its lines have no items.

It checks:

1. `AGENTS.md` is within its caps (rule 8: a warning above 120 lines or 1,200 words, a failure above 140 lines or 1,500 words) and has the version stamp, `Aligned to KEEL X.Y.Z`. If it has no stamp, check 1 fails and the other checks run. The version in the stamp is the same as the version in the first line of the script's header comment. If they differ, the script reports only this failure, "The docs check is from KEEL X.Y.Z; update it to the version in AGENTS.md", because an older check list can give wrong findings.
2. `docs/HANDOFF.md` and each `docs/handoff/*.md` is within its cap (rule 8: a warning above 40 lines, a failure above 50), and every bullet starts with a date; a bullet with no date is a failure. A bullet is a line that starts with `- ` below a `##` heading, and its date has the form `(YYYY-MM-DD)`. No bullet is carried forward: compare each bullet, without its date, with the earlier versions of the same file. A bullet that was also in the previous version is carried once (a warning). A bullet that was also in the 2 previous versions is carried twice (a failure). A bullet below the Waiting heading that has the word PROPOSED is not counted (check 8 covers it). Compare only when the bullets differ from the last version the check saw, so repeated runs in one session do not count as carries. A version that changes only other lines, such as an upgrade of the lines at the top of the file, is not a version for this check. A version that only adds bullets and keeps the Last updated line is not a new version: its bullets belong to the version before it. An upgrade adds its Waiting bullets in that way. With version control (the project root has a `.git` folder, and the `git` command answers), read the earlier versions from the history. Without it, the check keeps its own snapshot in `test-results/handoff-previous.md` (on a team, `test-results/handoff-previous-<developer>.md` for each handoff file): the bullets of the last version it saw, each with its carry count. The check updates the snapshot only when the bullets changed, and the script chooses its format. The AI never edits the snapshot.
3. Every `AC-n` in `SPEC.md` outside PROPOSED blocks has a row in the Requirement checks table of `RELEASE-CHECKLIST.md`, and every row has such an AC. A row is a table row whose first cell is an `AC-n` ID.
4. Every link target, every path in the area map, and every command exists. A link target is the target of a Markdown link or image; a URL and an anchor (`#...`) are not checked. In the rows of the area map in `ARCHITECTURE.md`, a path is a word that has a `/` and ends with `/` or with a file extension. A word is the text between spaces or table bars, without its quotes, backticks, brackets, bold and italic marks, and end punctuation. A file extension is a dot, then 1 to 5 letters or digits with at least 1 letter. A pattern (a word with `<`, `>`, `*`, `{`, or `...`) is not a path. A path that starts with `/`, `~`, or a drive letter (`C:`) is outside the project, and the check does not look for it. Look for the path from the project root, then from the folder of the doc that names it. The check does not read other path words in the docs: folder trees, build output, and history text name paths that correctly do not exist. The reviewer covers those. These missing paths are not failures: a file or folder from this standard that the project has not made yet (the exact name, such as `docs/RESEARCH.md` or `docs/decisions/`; a file inside such a folder is still checked), a path in `test-results/`, and an item on the Not used line. A command is the first code span in a cell of the Command column of the area map, or a line of a code block in `ARCHITECTURE.md` Commands. If the command names a path, that path exists; if it names none, its first word is a program on the PATH of the machine, or a file in the project. The check does not run a command.
5. Every file in `docs/` and its subfolders is a file from this standard (a name in the table of *The documentation files*, or a file inside `docs/decisions/`, `docs/images/`, `docs/plans/`, `docs/handoff/`, or `docs/archive/`), or is on the Kept as is line of `AGENTS.md`. A file from this standard needs no entry in the Docs list. Every other doc file (`.md`, `.txt`, `.rst`, `.adoc`, `.docx`, `.pdf`) in the project root is a file from this standard, `KEEL.md` or one of its `UPGRADING` files, a common repository file (`LICENSE`, `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, with any extension), an older `AI-PROJECT-DOCS-STANDARD.md`, or on the Kept as is line. These `.txt` files are for tools, and are not doc files: `requirements*.txt`, `constraints*.txt`, `robots.txt`, `llms.txt`, and `CMakeLists.txt`. A file that fails is a candidate to absorb (see `references/files.md`). The check does not scan subfolders such as package READMEs; the AI finds those while it works.
6. The first line of every doc is a header line of the form `> Name: text`. Decision notes and the files on the Kept as is line are not checked.
7. No file at the top level of `docs/` has the name of a file from this standard plus a suffix: the name without its extension, then `-`, `_`, a space, or `(`, then more text, in upper or lower case. Examples: `SPEC-v2.md`, `PLAN_old.md`, and `ARCHITECTURE copy.md`. A name that is a file from this standard, such as `DESIGN-GUIDE.html`, is not a failure.
8. Each PROPOSED block older than 14 days, by the date of the machine, is listed (a warning, not a failure). A PROPOSED block with no date is also a warning.
9. No assignment-shaped secret in the docs, in `docs/archive/`, or in `AGENTS.md`, `CLAUDE.md`, and `.github/copilot-instructions.md`: the word `password`, `secret`, `api_key`, or `token`, in upper or lower case, then `=` or `:`, then a value of 8 or more characters with no space, on the same line; or a private key header (`-----BEGIN ... PRIVATE KEY-----`). The word can be part of a longer name, such as `DB_PASSWORD`, and spaces and quotes around the `=` or `:` are allowed. A placeholder in angle brackets or in parentheses is not a value.

**Drift report** is a read-only report the AI writes before every release (rule 6) and whenever the developer asks ("run a drift report"). It never blocks and changes nothing. Each finding carries a certainty, and findings are never averaged. *Certain*: a mechanical check proves it. *Evidence*: the AI found it by reading, and the developer confirms it. *Inference*: a guess from dates, not from behavior.

| Finding | Certainty | How found |
|---|---|---|
| A SPEC feature or AC with no code behind it | Evidence | Area map ACs column against the code |
| Code with no SPEC coverage | Evidence | Folders not in any area map row, or rows with no ACs |
| An AC with no test, or a test with no AC | Certain | Area map ACs column against tests that cite `AC-n` in a name or comment |
| A command, folder, file, or link in the docs that does not exist | Certain | The docs check (paths, links, commands); the reviewer for anything else |
| A doc section older than the code it describes | Inference | The Verified date against the change history: version-control history if any; else the work board Started dates and `CHANGELOG.md`; else "Not run: no change history". Compares dates, not behavior. |

The report goes in the task summary. Lasting findings go to `RESEARCH.md`. At a release, each finding is fixed or accepted by the developer.

## The stronger checks question (rule 9)

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

---

This skill is generated from `KEEL.md` in https://github.com/rickflashman/keel. Do not edit it by hand; change `KEEL.md` and run `python3 build/build.py`.
