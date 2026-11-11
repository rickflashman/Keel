---
name: keel-init
description: "KEEL 2.2.0 new setup: creates the baseline docs for a project that has no documentation yet (README, docs/SPEC, ARCHITECTURE, PLAN, RELEASE-CHECKLIST, AGENTS.md, CLAUDE.md, copilot-instructions) from the KEEL templates, then runs the initialization checklist. Use when the developer runs keel-init (/keel:keel-init or /keel-init) or asks to initialize a new project with KEEL. For a project that already has docs, use keel-upgrade."
disable-model-invocation: true
argument-hint: "[ticket ID or project description]"
metadata:
  version: "2.2.0"
  source: "https://github.com/rickflashman/keel"
---
# KEEL 2.2.0: new setup

Create the baseline documentation for a project that has no documentation yet. Version 2.2.0 (November 11, 2026), https://github.com/rickflashman/keel.

If the developer gave the ticket ID, the project description, or other instructions with the command, use them: $ARGUMENTS

## Steps

1. **Check first.** Follow the Check first rule below. If the path is not *New setup*, stop and tell the developer to run `keel-upgrade` (migration or upgrade) or `keel` (any path). Do not continue with a project that already has docs.
2. **Ask before you create any file.** Send this message, with all 3 questions, as plain chat text, and wait for the answer. Do not use a question tool or form: the answers are free text. Skip only a question the developer already answered with the command. Never take the project name from the folder name. If the developer does not answer an item, leave its placeholder (see *Linking to the project tracker*).

       Before I create the files, 3 questions:
       1. Tracker and ticket ID, with the ticket link if you have one (or "none"):
       2. Project name and a short description:
       3. Does the project have a user interface (yes or no)?

3. **Create the required files** in numbered order from `templates/` (guidance for each file: `templates/README.md`). Create optional files only where the table says they are needed now, such as `docs/DESIGN.md` for a project with a UI. Follow the folder layout exactly.
4. **Write the docs check script** and add its row to the area map (see *Docs check and drift report*). When you fill the Testing section of `docs/ARCHITECTURE.md`, ask the stronger checks question at the end of this file.
5. **Run the Initialization checklist** below and report any item that fails.
6. **Offer** the as-built review (`keel-as-built`) if the project has code, then the SPEC interview (`keel-interview`).

Never overwrite a file that already exists; merge or ask. The rest of this file is the part of the standard that a new setup needs. The working rules (rule 1 to rule 11), the risk classes, and project skills are in `references/rules.md`. Read it only when a step or a template names a rule and you need its full text.

## Initialization rules

- **Check first:** Before you create anything, check the project and choose 1 path:
  - **Upgrade:** `AGENTS.md` (or `CLAUDE.md` before 1.2.0) has a version stamp older than this skill's version. A stamp that names *AI-PROJECT-DOCS-STANDARD* is an older version of this standard (KEEL 1.x). Follow the upgrade-only path in `references/migration.md`.
  - **Migration:** the project has any file that the Inventory step would find, even a single `README.md` or rules file. A project with a stamp is never a Migration: it is an Upgrade, or it is up to date. A file outside the standard in such a project, such as a package README, is a candidate to absorb (see *The documentation files*). Follow `references/migration.md`.
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

## Folder layout

```
project/
├── KEEL.md                 (the standard as a file, or this skill in its place; retained at the developer's discretion)
├── README.md
├── AGENTS.md
├── CLAUDE.md
├── .github/
│   └── copilot-instructions.md
├── .claude/                (optional)
│   └── skills/
│       └── <skill-name>/
│           └── SKILL.md
├── .env.example            (optional)
├── test-results/           (written by test runs: latest.md, docs.md, handoff-previous.md; excluded from version control)
└── docs/
    ├── SPEC.md
    ├── ARCHITECTURE.md
    ├── PLAN.md
    ├── RELEASE-CHECKLIST.md
    ├── decisions/             (recommended)
    ├── HANDOFF.md             (recommended; created when first needed)
    ├── handoff/               (teams only; one <developer>.md per developer, in place of HANDOFF.md)
    ├── plans/                 (optional; one M<n>.md per milestone when the work board grows)
    ├── CHANGELOG.md           (recommended; created at the first release)
    ├── DESIGN.md              (optional; required if the project has a UI)
    ├── DESIGN-GUIDE.html      (optional; required if the project has a UI)
    ├── USER-GUIDE.md          (optional; for projects with end users)
    ├── RUNBOOK.md             (optional; split from the ARCHITECTURE.md Operations section)
    ├── images/                (optional; screenshots and images for the docs)
    ├── MOCKUP.md              (optional)
    ├── RESEARCH.md            (optional)
    └── archive/               (optional; created by a migration or an absorb)
        └── INDEX.md           (lists each archived file and its status)
```

The docs check script (see *Docs check and drift report*) lives where the project keeps its tests, or in `scripts/` if the project has no test folder.

## The documentation files

| # | File | Status | Purpose | Seeded from the ticket |
|---|---|---|---|---|
| 1 | `README.md` | Required | For humans: what the project is, how to run it, owners and support, links | Full description + link |
| 2 | `docs/SPEC.md` | Required | What and why: goals, users, glossary, features, user stories, constraints, acceptance criteria, non-goals | Requirements |
| 3 | `docs/ARCHITECTURE.md` | Required | How: stack, folders, commands, data model, APIs, auth, hosting, deployment, operations, testing, the area map, diagrams | Approach, deployment notes |
| 4 | `docs/PLAN.md` | Required | Execution: milestones, the work board, Definition of ready, Definition of done | Rarely |
| 5 | `AGENTS.md` | Required | AI working rules, precedence, and lists of the docs and skills, shared by all AI coding tools | n/a |
| 6 | `CLAUDE.md` | Required | Loads `AGENTS.md` into Claude Code with an import line, plus optional Claude-only notes | n/a |
| 7 | `.github/copilot-instructions.md` | Required | Points GitHub Copilot to `AGENTS.md` and its listed skills, plus optional Copilot-only notes | n/a |
| 8 | `docs/RELEASE-CHECKLIST.md` | Required | Release gate: one check per SPEC acceptance criterion, the non-test release steps, and the latest release result | n/a |
| - | `docs/decisions/` | Recommended | One short note per major decision, numbered in order. Create the folder with the first decision. | n/a |
| - | `docs/HANDOFF.md` | Recommended | Short handoff to the next session: next action, open items, traps. Dated bullets, within the rule 8 cap. Create it at the first session close, or earlier to record something important. | n/a |
| - | `docs/handoff/<developer>.md` | Teams only | One handoff per developer, same template and rules as `HANDOFF.md`. Used in place of `HANDOFF.md` when more than 1 developer works on the project. | n/a |
| - | `docs/CHANGELOG.md` | Recommended | Release history, newest first: what was added, changed, and fixed in each release. Create it at the first release. | n/a |
| - | `docs/plans/M<n>.md` | Optional | One work board per milestone, used when the board in `PLAN.md` passes about 15 open rows | n/a |
| - | `docs/DESIGN.md` | Optional (required if the project has a UI) | UI/UX: colors, type, spacing, components, layout, accessibility, wireframe links | UI/UX or design notes |
| - | `docs/DESIGN-GUIDE.html` | Optional (required if the project has a UI) | Human-readable, visual version of `DESIGN.md`, so graphic designers have an easy reference. Regenerated from `DESIGN.md` every time `DESIGN.md` changes; never edited by hand. | n/a |
| - | `docs/USER-GUIDE.md` | Optional (for projects with end users) | Task-based guide for the people who use the product but do not develop or maintain it | n/a |
| - | `docs/RUNBOOK.md` | Optional | Operations guide for support staff: services, logs, common failures and fixes, backups, contacts. Split from the `ARCHITECTURE.md` Operations section when it passes a page. | n/a |
| - | `docs/images/` | Optional | Screenshots and images used by the docs, such as the user guide | n/a |
| - | `docs/MOCKUP.md` | Optional | Mockup-only instructions, temporary sample data, and project-specific mockup decisions | n/a |
| - | `docs/RESEARCH.md` | Optional | Results of research and comparisons, including options not chosen, kept for later sessions | n/a |
| - | `docs/archive/` | Optional | Original documentation files moved by a migration or an absorb, kept for history only, with `INDEX.md` giving each file's status. Never delete them, and never use them as a source of current information. | n/a |
| - | `.claude/skills/<skill-name>/SKILL.md` | Optional | Project-specific skills, one folder per skill | n/a |
| - | `.env.example` | Optional | Names of the required environment variables, with no real values. Required if the project uses environment variables. | n/a |

**Sections that do not apply:** Create every required file and keep all its headings. If a section does not apply, say why in the file. Example: the `ARCHITECTURE.md` Auth & security section says the project has no authentication.

**Files a developer removes:** A developer may decide not to use a file from this standard, even a required one. That decision overrides this standard. List each such file on the "Not used" line of the `AGENTS.md` Docs list. Never recreate a file on that line, and never report it as missing. If a file from this standard is missing and is not on that line, ask the developer once whether to recreate it, and record the answer there.

**Doc files outside the standard:** A migration brings existing docs into the standard at setup. After setup, new doc files can still appear: a notes file, a Word or PDF file, a wiki export, or a second README. When the AI finds such a file, inside or outside `docs/`, it offers to absorb it. Do not link to the file as a source instead. To absorb a file, follow migration steps 3 to 7 for that file only (see `references/migration.md`): resolve currency, map the current content to its target docs, get approval, move the content, and archive the original in `docs/archive/` with a row in `INDEX.md`. Then update every link to the file. If the developer declines, list the file on the "Kept as is" line of the `AGENTS.md` Docs list, and do not offer again. The docs check flags these files (check 5).

**Projects with no UI:** Do not create `DESIGN.md` or `DESIGN-GUIDE.html`, and leave them out of the `AGENTS.md` Docs list. If the project later gains a UI, create both from the templates.

**Projects with end users:** Create `USER-GUIDE.md` once end users have a working feature to use. Write it in ASD-STE100 Simplified Technical English (STE): short sentences, active voice, and approved words. Describe current behavior as tasks, and put screenshots in `docs/images/`.

**Projects with more than 1 developer:** Count developers, not tools: a solo developer who uses 2 AI tools is still 1 developer. The work board in `PLAN.md` holds the shared state: who holds which item, which files it touches, and its status. Each developer has a handoff file `docs/handoff/<developer>.md` (the developer's name in lowercase with no spaces, such as `rick.md`), rewritten only by that developer's sessions, and `docs/HANDOFF.md` is not used. The Handoff file line in `AGENTS.md` names the form the project uses. A board Owner is a developer name, with the tool after it when useful ("Rick / Claude Code").

Create other optional files only when needed, from their templates. If the `AGENTS.md` template has a Docs entry for the file, add that entry.

**Research versus decisions:** Use `docs/RESEARCH.md` for research results or data that do not belong in the plan, specs, architecture, or decisions.

- **Decision note:** records *what* we chose and *why*, in a sentence or two. Example: "We chose A over B because it was faster." The AI must not reverse it.
- **Research entry:** records the *evidence* behind a choice, such as the actual test results for A and B. If A later fails, the entry shows whether B is a viable fallback. Research entries may be updated.

When a decision rests on research, the decision note links to the research entry.

**Acceptance criteria versus the release checklist:** `SPEC.md` defines correct behavior. Each acceptance criterion has an ID (`AC-1`, `AC-2`, and so on) and its test cases, grouped by feature or milestone, so development can test one part at a time. `RELEASE-CHECKLIST.md` proves a release meets all of them: one check per ID, plus non-test release steps such as updating the version and the docs (rule 6).

**Proposed changes to SPEC and DESIGN:** The AI never edits accepted text in `SPEC.md` or `DESIGN.md` (rule 1). It writes each proposal in the file, next to the text it changes, as a PROPOSED block, never only in chat:

```md
> PROPOSED 2026-10-31 (Claude Code): change AC-3 to allow an empty email.
> Reason: the form now has a guest path. Code: src/forms/signup.js.
```

A PROPOSED block is not a requirement and not a design. The developer accepts it by applying the edit and deleting the marker, rejects it by deleting the block, or tells the AI to do either. The developer may decide on several blocks at once, for example "accept all PROPOSED blocks in SPEC.md". The developer may also accept the blocks of a ready card together with the card, but only by saying so; approval of the card alone does not accept its blocks. A block is named by its file and the AC ID or heading it changes (`SPEC.md AC-3`). The AI lists pending blocks in its task summary and, at the next close, in `HANDOFF.md` under Waiting. No block may remain at a release (rule 6).

**Session handoff versus lasting records:** `docs/HANDOFF.md` is a short, temporary handoff between sessions, tools, or developers, not a general record of what the AI remembers. Before writing to it, the AI routes each item to its proper home and prints the routing (rule 5):

| Content | Goes in |
|---|---|
| A choice and why it was made | `docs/decisions/` |
| Test results, options compared, fallbacks | `docs/RESEARCH.md` |
| Milestones and milestone-level next work | `docs/PLAN.md` |
| Unfinished work that needs its own task | A work board row in `docs/PLAN.md` |
| Commands, structure, stack, environment facts, lasting traps | `docs/ARCHITECTURE.md` |
| Release check results | `docs/RELEASE-CHECKLIST.md` |
| Requirement or UI changes | PROPOSED block in `SPEC.md` or `DESIGN.md`; listed in `HANDOFF.md` under Waiting until accepted |

Only leftovers go in `HANDOFF.md`: the immediate next action, questions waiting on a person, unconfirmed assumptions, decisions made without asking, and traps found this session that have not yet moved. It never overrides the other docs. The close routine steps and the same routes sit at the top of the `HANDOFF.md` template, so the AI sees them each time it edits the file, and `AGENTS.md` stays short.

**AI instruction files:** `AGENTS.md` is a shared format that many AI coding tools read, including Claude Code and GitHub Copilot. It holds all the AI working rules, so a project keeps working as tools change. The other two files only point to it, plus optional notes for their own tool. Do not copy rules from `AGENTS.md` into either one.

- `CLAUDE.md` starts with the line `@AGENTS.md`, which imports `AGENTS.md` into Claude Code. The import works in every current Claude Code setup, including sessions that cannot read `AGENTS.md` directly, and never loads the rules twice. Do not replace it with a symbolic link (links need extra permissions on Windows).
- `.github/copilot-instructions.md` points GitHub Copilot to `AGENTS.md` for setups that do not read `AGENTS.md` directly. Only GitHub Copilot reads it; *Microsoft Copilot* and *Microsoft 365 Copilot* do not. Note the leading dot in `.github`, and use the exact file name. It needs no GitHub repository or version control. Create it even if you only use Claude Code, so others can use GitHub Copilot with the same rules.

**Personal rules files:** A developer may also have a personal rules file that the tool loads in every project, such as `~/.claude/CLAUDE.md`. It is outside the project, and this standard does not manage it. Keep personal preferences there: how the AI writes to you, and which tools you prefer. Keep everything the project needs in `AGENTS.md`, so the next developer and the next tool get the same rules. The AI never copies a personal rules file into the project docs, and a migration or an absorb does not read it. A personal rule is not a direct developer instruction. If it conflicts with a project doc on something that changes the project (code, tests, or docs), the project doc wins, because other developers and tools do not see the personal file. Tell the developer once. An instruction that the developer gives in the session still overrides, as the Precedence line says.

## Linking to the project tracker

Most projects have a ticket, work item, or issue in a tracker such as Jira, Azure DevOps, GitHub Issues, or Linear, with a description of the project. Where the table lists a "Seeded from the ticket" section, copy ("seed") the file's first draft from the matching part of that description (for example, a Requirements or Approach section). Which tracker, and what its sections are called, is the project's choice; this standard does not depend on any product.

- `README.md` and every Markdown file in `docs/`, except decision notes and files in `docs/archive/`, start with a header line that names the tracker and the ticket: `> Tracker: TICKET-# (name of project)`, with the tracker's name in place of `Tracker` and the ticket ID in place of `TICKET-#`, for example `> Jira: BOOK-12 (Booking Portal)` or `> GitHub: #42 (Booking Portal)`. The README also links to the ticket and starts with the full ticket description. Projects set up under 1.x keep their `> Jira:` headers; they already follow this form.
- The ticket seeds the first drafts only. After setup, the docs are the source of truth, and the AI never copies the ticket over them. When the ticket changes later, the AI writes the change into the docs like any other change: a PROPOSED block for SPEC or DESIGN, a direct update for the others, and the new description in the README.
- Include the ticket ID in the names of work items and in any notes or messages about the work. Decision notes carry no ticket ID.
- If no ticket ID has been assigned yet, leave the `TICKET-#` placeholder. The developer fills it in later, or gives it to the AI to replace every placeholder.
- If the project has no tracker, replace each header with `> Project: (name of project)`, omit the ticket link and ticket IDs, and ask the developer for a project description to use instead.

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
5. Every file in `docs/` and its subfolders is a file from this standard (a name in the table of *The documentation files*, or a file inside `docs/decisions/`, `docs/images/`, `docs/plans/`, `docs/handoff/`, or `docs/archive/`), or is on the Kept as is line of `AGENTS.md`. A file from this standard needs no entry in the Docs list. Every other doc file (`.md`, `.txt`, `.rst`, `.adoc`, `.docx`, `.pdf`) in the project root is a file from this standard, `KEEL.md` or one of its `UPGRADING` files, a common repository file (`LICENSE`, `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, with any extension), an older `AI-PROJECT-DOCS-STANDARD.md`, or on the Kept as is line. These `.txt` files are for tools, and are not doc files: `requirements*.txt`, `constraints*.txt`, `robots.txt`, `llms.txt`, and `CMakeLists.txt`. A file that fails is a candidate to absorb (see *The documentation files*). The check does not scan subfolders such as package READMEs; the AI finds those while it works.
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

## Initialization checklist

After a new setup, a migration, or an upgrade, confirm each item and report any that fail. An item that names a path applies only after that path.

- [ ] `README.md`, `docs/SPEC.md`, `docs/ARCHITECTURE.md`, `docs/PLAN.md`, and `docs/RELEASE-CHECKLIST.md` exist, from the starter templates, unless the developer chose not to use one and it is on the `AGENTS.md` "Not used" line.
- [ ] Every SPEC acceptance criterion has an ID and a matching check in `RELEASE-CHECKLIST.md`. `SPEC.md` has the Glossary and Constraints headings.
- [ ] `ARCHITECTURE.md` has the Environment facts and Operations sections and the area map with a Docs row. The docs check script exists, is listed in the map, and passes.
- [ ] `PLAN.md` has the Definition of ready, the Definition of done, and the work board.
- [ ] A project with a UI has `docs/DESIGN.md` and `docs/DESIGN-GUIDE.html`. A project without one has neither, and `AGENTS.md` does not list them.
- [ ] `AGENTS.md` is at most 120 lines and 1,200 words, and has the version stamp, the precedence line, and the Project documentation standard, Making changes, Testing, Reporting, Session handoff, and Docs sections, and the Skills section if the project has a skill.
- [ ] `CLAUDE.md` starts with `@AGENTS.md`, and `.github/copilot-instructions.md` (exact name) points to `AGENTS.md`. Neither copies rules from `AGENTS.md`.
- [ ] Every tracker header has the tracker name and `TICKET-# (name of project)` filled in, or keeps the placeholder for the developer.
- [ ] Every required section that does not apply says why (for example, no authentication in `ARCHITECTURE.md`).
- [ ] Optional files exist only where needed, and each one has its Docs entry if the `AGENTS.md` template has an entry for it. After a new setup or a migration: `docs/HANDOFF.md` was not created, unless a migration or the as-built review wrote to it, and `docs/CHANGELOG.md` was not created, unless a migration moved release history into it.
- [ ] After a new setup or a migration: no PROPOSED blocks exist, unless a migration or the as-built review created them. On every path, each pending PROPOSED block is listed in the report and in `HANDOFF.md` under Waiting.
- [ ] No file contains secrets or customer data, and `.gitignore` excludes `.env` and `test-results/` if the project uses version control.
- [ ] After a migration: no Superseded content was migrated, and `docs/archive/INDEX.md` lists every archived file.
- [ ] No existing file was overwritten without the developer's approval, and `KEEL.md` was not deleted, renamed, moved, or modified, except by a switch to the skill on the developer's explicit instruction.

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
