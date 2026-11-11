# KEEL

A documentation and working standard for AI-developed projects.

Version 2.2.0 (November 11, 2026) - Rick Flashman. Formerly the AI Project Documentation Standard (1.0.0 to 1.5.1).

## Overview

This document defines the baseline documentation files, folder structure, and working rules for any AI-developed project. Like the keel of a ship, it keeps a project steady and on course when the AI tool, the AI model, or the developer changes. It keeps the AI's work organized, supportable, and token-efficient.

This document has three uses:

- **Guide for developers:** It explains how to create and organize a new project.
- **Setup prompt for AI coding assistants:** Claude Code, GitHub Copilot, or another assistant that reads `AGENTS.md` (such as Codex or Cursor) can follow it to set up a new project, or to bring an existing project's documentation up to this standard.
- **Source of the KEEL skill:** the same standard is also installed once, as a set of skills, so it works in every project without this file (see *The KEEL skill*).

> [!IMPORTANT]
>
> **How to use it:** Put this file in the project's root folder and tell your AI coding agent: *"Read KEEL.md and do what it says."* It works for new and existing projects. The AI asks for your approval before it changes any existing file.

> [!NOTE]
>
> **Terms:** *Required* items (numbered 1-8 in the table) are created at initialization, in numbered order. *Recommended* items are created when first needed. *Optional* items are created only if needed, but `DESIGN.md` and `DESIGN-GUIDE.html` are required if the project has a user interface (UI). *Teams only* items are used when more than 1 developer works on the project.
>
> **Scope:** Version control (branching, merging, pull requests) is not covered. When the project uses branches, the work board in `PLAN.md` records the branch name, nothing more. The only 2 exceptions: rule 7 keeps `.gitignore` current, and docs check 2 reads earlier versions of the handoff file from the history.

## Workflow

1. A developer puts `KEEL.md` in the project's root folder.
2. The AI reads it, checks the project (see Check first), and then sets up, migrates, or upgrades the documentation.
3. Afterward, the developer may keep, move, archive, or delete the file. *Authority and file handling* says which reference the AI follows in each case.

## The KEEL skill

The standard is also packaged as 6 skills, generated from this file by `build/build.py` in the repository, with the same version. A developer installs them once at user level, and they work in every project without this file. Each skill is 1 task; `keel` is the general one for a developer who is not sure which task they need, or who wants to bring a project to the newest version.

| Skill | Task |
|---|---|
| `keel` | Looks at the project, chooses the path (new setup, migration, upgrade, or a switch from the `KEEL.md` file to the installed skill), and does it |
| `keel-init` | New setup from the templates, for a project with no docs yet |
| `keel-upgrade` | Migrate existing docs, upgrade an older version, or switch a project from the `KEEL.md` file to the installed skill (the skill then replaces the file as the reference) |
| `keel-check` | Write or run the docs check script, write a drift report, and, on request, run a cross-compare review or a docs cleanup |
| `keel-as-built` | The as-built review |
| `keel-interview` | The SPEC interview |

The skills install as a Claude Code plugin, with `npx skills add`, or with `install.sh`, for Claude Code, Codex, Cursor, and GitHub Copilot. The `README.md` of the repository has the install and update steps. A project that uses this file can switch to the skill at any time (see *Switch to the skill* under Initialization rules), and a project set up by the skill can add this file later; the one in the root folder is then the authority.

## Authority and file handling

When `KEEL.md` is in the project root, it is the authoritative reference for project structure, documentation standards, naming conventions, required, recommended, and optional artifacts, and templates and seed content, including for documentation created later. Direct developer instructions and explicit project-specific requirements override it. When project docs disagree with each other, the Precedence list in `AGENTS.md` says which one wins.

AI assistants may read and use the file, but must not delete, rename, move, modify, archive, or otherwise manage it without direct, explicit developer instruction. The developer controls its location and lifecycle, and may ask an AI assistant to update it.

When `KEEL.md` is absent and the KEEL skill is installed, the skill is the reference for the same things. When both are present, `KEEL.md` wins; if their versions differ, tell the developer before you continue.

## Initialization rules

- **Check first:** Before you create anything, check the project and choose 1 path:
  - **Upgrade:** `AGENTS.md` (or `CLAUDE.md` before 1.2.0) has a version stamp older than this document's version. A stamp that names *AI-PROJECT-DOCS-STANDARD* is an older version of this document (KEEL 1.x). Follow the upgrade-only path in *Migrating or upgrading an existing project* (near the end of this document).
  - **Migration:** the project has any file that the Inventory step would find, even a single `README.md` or rules file. A project with a stamp is never a Migration: it is an Upgrade, or it is up to date. A file outside the standard in such a project, such as a package README, is a candidate to absorb (see *The documentation files*). Follow *Migrating or upgrading an existing project*.
  - **New setup:** the project has none of these. Create the files from the templates.
  - **Switch to the skill:** the KEEL skill is running, `KEEL.md` is in the root folder, and the developer asked to switch. Run the Upgrade path first if the stamp is older than the skill's version. Then set the stamp to the skill's version and, only on the developer's explicit instruction, delete `KEEL.md` or move it where the developer says. Report what changed.

  If the root folder also holds `AI-PROJECT-DOCS-STANDARD.md`, it is an older version of this document: follow this file, and tell the developer. When the upgrade is complete, ask the developer once whether to delete the old file; do not delete, move, or archive it without that answer. Tell the developer which path you chose and why before you continue. If the stamp is this document's version and there is nothing to bring in, say that the project is up to date, and stop.
- **Missing information:** Before you create any file, send the developer this message, with all 3 questions, as plain chat text, and wait for the answer. Do not use a question tool or form: the answers are free text. Skip only a question the developer already answered. Never take the project name from the folder name. If the developer does not answer an item, leave its placeholder.

      Before I create the files, 3 questions:
      1. Tracker and ticket ID, with the ticket link if you have one (or "none"):
      2. Project name and a short description:
      3. Does the project have a user interface (yes or no)?
- **Existing files:** Never overwrite a file or folder from this document that already exists. Merge the new content in, or propose the change, and ask the developer first.
- **After the path:** When the path is complete, run the Initialization checklist and report. Then, if the project has code, offer the as-built review. After an Upgrade, offer it only when `ARCHITECTURE.md` has no Verified date. After you seed or migrate `SPEC.md`, offer the SPEC interview; when you also offer the as-built review, offer the interview after it.
- **As-built review:** A review of existing code, offered after the path for a project with code (see After the path). The developer may decline or choose only some areas. On approval, follow these steps in order:
  1. **Feature inventory.** List the features the code implements, each with its entry points (routes, commands, screens, jobs, public modules) and folders. Show the list. The developer confirms, renames, merges, or drops items. Do not read deeper until the list is confirmed.
  2. **Area map.** Fill the area map in `ARCHITECTURE.md` for the confirmed areas: code, entry points, tests, command, dependencies (from imports). Set Confidence to Inferred on every row, and mark the table "Inferred from code, needs developer review".
  3. **One area at a time, with a budget.** Read 1 area, write or update its `ARCHITECTURE.md` content (stack, folders, commands, data model, operations), then stop and report before the next. Use a subagent per area where the tool allows, so the main context stays small. If an area exceeds the budget, set its Confidence to Low and record where reading stopped.
  4. **SPEC from behavior.** For each area, write feature text and acceptance criteria that describe what the code does now. If `SPEC.md` is empty or holds only the template, seed it and mark the content "Inferred from code, needs developer review". If `SPEC.md` has content, write the differences as PROPOSED blocks (see *Proposed changes to SPEC and DESIGN*). The same applies to `DESIGN.md`. Never change accepted SPEC or DESIGN text from code.
  5. **Differences and health.** Report what is built but not specified, what is specified but not built, superseded approaches still in the code, areas with no tests, secrets found, and dependencies with known issues. Lasting findings go to `RESEARCH.md`; questions go to `HANDOFF.md` under Waiting. Set the Verified date in `ARCHITECTURE.md`, and in `SPEC.md` once the developer has reviewed it.
  6. **Stop conditions.** Change no code. Do not run tests unless the developer asks.
  7. **Offer the cross-compare review.** End the report with 1 line that says the cross-compare review is available (see *Cross-compare review and docs cleanup*). Inferred docs are where most conflicts come from. Run the review only if the developer asks for it.
- **SPEC interview:** Interview the developer about the gaps in `SPEC.md`: edge cases, non-goals, trade-offs, constraints, glossary terms, and acceptance criteria. Ask a few questions at a time, and skip questions the docs already answer. Write the resulting SPEC changes as PROPOSED blocks for acceptance (rule 1). The developer may decline.

## Folder layout

```
project/
├── KEEL.md                 (this file, or the KEEL skill in its place; retained at the developer's discretion)
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

**Files a developer removes:** A developer may decide not to use a file from this document, even a required one. That decision overrides this document. List each such file on the "Not used" line of the `AGENTS.md` Docs list. Never recreate a file on that line, and never report it as missing. If a file from this document is missing and is not on that line, ask the developer once whether to recreate it, and record the answer there.

**Doc files outside the standard:** A migration brings existing docs into the standard at setup. After setup, new doc files can still appear: a notes file, a Word or PDF file, a wiki export, or a second README. When the AI finds such a file, inside or outside `docs/`, it offers to absorb it. Do not link to the file as a source instead. To absorb a file, follow migration steps 3 to 7 for that file only (see *Migrating or upgrading an existing project*): resolve currency, map the current content to its target docs, get approval, move the content, and archive the original in `docs/archive/` with a row in `INDEX.md`. Then update every link to the file. If the developer declines, list the file on the "Kept as is" line of the `AGENTS.md` Docs list, and do not offer again. The docs check flags these files (check 5).

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

**Personal rules files:** A developer may also have a personal rules file that the tool loads in every project, such as `~/.claude/CLAUDE.md`. It is outside the project, and this document does not manage it. Keep personal preferences there: how the AI writes to you, and which tools you prefer. Keep everything the project needs in `AGENTS.md`, so the next developer and the next tool get the same rules. The AI never copies a personal rules file into the project docs, and a migration or an absorb does not read it. A personal rule is not a direct developer instruction. If it conflicts with a project doc on something that changes the project (code, tests, or docs), the project doc wins, because other developers and tools do not see the personal file. Tell the developer once. An instruction that the developer gives in the session still overrides, as the Precedence line says.

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

## Linking to the project tracker

Most projects have a ticket, work item, or issue in a tracker such as Jira, Azure DevOps, GitHub Issues, or Linear, with a description of the project. Where the table lists a "Seeded from the ticket" section, copy ("seed") the file's first draft from the matching part of that description (for example, a Requirements or Approach section). Which tracker, and what its sections are called, is the project's choice; this document does not depend on any product.

- `README.md` and every Markdown file in `docs/`, except decision notes and files in `docs/archive/`, start with a header line that names the tracker and the ticket: `> Tracker: TICKET-# (name of project)`, with the tracker's name in place of `Tracker` and the ticket ID in place of `TICKET-#`, for example `> Jira: BOOK-12 (Booking Portal)` or `> GitHub: #42 (Booking Portal)`. The README also links to the ticket and starts with the full ticket description. Projects set up under 1.x keep their `> Jira:` headers; they already follow this form.
- The ticket seeds the first drafts only. After setup, the docs are the source of truth, and the AI never copies the ticket over them. When the ticket changes later, the AI writes the change into the docs like any other change: a PROPOSED block for SPEC or DESIGN, a direct update for the others, and the new description in the README.
- Include the ticket ID in the names of work items and in any notes or messages about the work. Decision notes carry no ticket ID.
- If no ticket ID has been assigned yet, leave the `TICKET-#` placeholder. The developer fills it in later, or gives it to the AI to replace every placeholder.
- If the project has no tracker, replace each header with `> Project: (name of project)`, omit the ticket link and ticket IDs, and ask the developer for a project description to use instead.

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
7. **Never put secrets in code or docs.** If the project uses environment variables, list their names in `.env.example`. Do not paste secrets or customer data into docs or the tracker. If the project uses version control, make sure `.gitignore` excludes `.env` and the `test-results/` folder (rule 9), creating or updating it if needed. A project uses version control when it has a `.git` folder, or when its docs name a repository or a branch. If you cannot tell, ask the developer once. This is the only version-control step this document asks for; docs check 2 also reads the history of the handoff file.
8. **Keep docs skimmable.** These are the caps, and the docs check uses the same numbers:
   - `AGENTS.md`: 120 lines and 1,200 words (a warning above either, a failure above 140 lines or 1,500 words), because the AI loads it every session. The word cap catches long lines that a line count misses. The template is a maximum, so delete the optional lines that do not apply. The template in this document has a budget of 1,100 words, so each project keeps at least 100 words for its own rules; `build/build.py` in the KEEL repository fails above the budget.
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

## Docs check and drift report

**Docs check** is a saved script in the project's own language, with no new runtime or dependency. When the project has no language yet, check which runtimes are installed on the developer's machine and use one of them; if none fits, write a shell script (`sh` on macOS and Linux, PowerShell on Windows). Port the script when the stack is chosen, and record that in `ARCHITECTURE.md`. It is the one script this document asks for. It is listed in the area map as area *Docs* and runs with the targeted tests of every change; a red check is a failing test. The AI writes it at setup or upgrade and keeps the list of checks, with these definitions, in the script's header comment, so the list survives without this file. The first line of that comment names the version that the list comes from, in the form `Docs check for KEEL X.Y.Z`, with this document's version. It writes its results to `test-results/docs.md`, so the area test results in `test-results/latest.md` survive the same task.

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
4. Every link target, every path in the area map, and every command exists. A link target is the target of a Markdown link or image; a URL and an anchor (`#...`) are not checked. In the rows of the area map in `ARCHITECTURE.md`, a path is a word that has a `/` and ends with `/` or with a file extension. A word is the text between spaces or table bars, without its quotes, backticks, brackets, bold and italic marks, and end punctuation. A file extension is a dot, then 1 to 5 letters or digits with at least 1 letter. A pattern (a word with `<`, `>`, `*`, `{`, or `...`) is not a path. A path that starts with `/`, `~`, or a drive letter (`C:`) is outside the project, and the check does not look for it. Look for the path from the project root, then from the folder of the doc that names it. The check does not read other path words in the docs: folder trees, build output, and history text name paths that correctly do not exist. The reviewer covers those. These missing paths are not failures: a file or folder from this document that the project has not made yet (the exact name, such as `docs/RESEARCH.md` or `docs/decisions/`; a file inside such a folder is still checked), a path in `test-results/`, and an item on the Not used line. A command is the first code span in a cell of the Command column of the area map, or a line of a code block in `ARCHITECTURE.md` Commands. If the command names a path, that path exists; if it names none, its first word is a program on the PATH of the machine, or a file in the project. The check does not run a command.
5. Every file in `docs/` and its subfolders is a file from this document (a name in the table of *The documentation files*, or a file inside `docs/decisions/`, `docs/images/`, `docs/plans/`, `docs/handoff/`, or `docs/archive/`), or is on the Kept as is line of `AGENTS.md`. A file from this document needs no entry in the Docs list. Every other doc file (`.md`, `.txt`, `.rst`, `.adoc`, `.docx`, `.pdf`) in the project root is a file from this document, `KEEL.md` or one of its `UPGRADING` files, a common repository file (`LICENSE`, `CONTRIBUTING`, `SECURITY`, `CODE_OF_CONDUCT`, with any extension), an older `AI-PROJECT-DOCS-STANDARD.md`, or on the Kept as is line. These `.txt` files are for tools, and are not doc files: `requirements*.txt`, `constraints*.txt`, `robots.txt`, `llms.txt`, and `CMakeLists.txt`. A file that fails is a candidate to absorb (see *The documentation files*). The check does not scan subfolders such as package READMEs; the AI finds those while it works.
6. The first line of every doc is a header line of the form `> Name: text`. Decision notes and the files on the Kept as is line are not checked.
7. No file at the top level of `docs/` has the name of a file from this document plus a suffix: the name without its extension, then `-`, `_`, a space, or `(`, then more text, in upper or lower case. Examples: `SPEC-v2.md`, `PLAN_old.md`, and `ARCHITECTURE copy.md`. A name that is a file from this document, such as `DESIGN-GUIDE.html`, is not a failure.
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

## Cross-compare review and docs cleanup

These 2 reviews read every doc, and the cross-compare review also reads the code. They use many tokens, so they run only when the developer asks. Never start one on your own. Only 2 places mention them unasked: the 1 line at the end of an as-built review (Initialization rules), and 1 line in the report of an upgrade from a version before 2.1.0. Use a subagent per doc or per area where the tool allows, so the main context stays small.

**Cross-compare review** ("run a cross-compare review"): a full comparison of all the docs with each other and with the code. It is most useful after a migration or an as-built review, when much of the content is new or inferred.

1. Read every doc that the project uses (the files from this document that exist, and the files on the Kept as is line of `AGENTS.md`), and the code of each area in the area map.
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

## Starter templates

Copy these into each new project. Replace the placeholders (`Tracker`, `TICKET-#`, name, and web link if available), and follow or delete the guidance in parentheses. The `AGENTS.md` template is a maximum: at setup, delete the optional lines that do not apply (teams, area owners, and the Skills section when the project has no skill). The 2 code rules in Conventions are defaults: keep, change, or delete them. The Docs list holds only the docs whose reading rule no other section gives. Keep the entries for the files that exist in the project, plus `docs/RESEARCH.md`, which stays listed before it exists so the AI knows to create and use it. The other files from this document need no entry. Keep the "Not used" and "Kept as is" lines, with "(none)" until the developer adds a file to one.

**README.md**

```md
> Tracker: TICKET-# (name of project)
(Ticket web link)

## Description
(Full ticket description)

## Run it
(Commands from the ticket)
- Install: `...`
- Dev: `...`
- Test: `...`
- More commands added during development: see `docs/ARCHITECTURE.md`

## Owners and support
- Owner: (who owns the project)
- Report issues: (where, such as the tracker project or a contact)

## Docs
See the `docs/` folder: `SPEC.md` (what and why), `ARCHITECTURE.md` (how), and `PLAN.md` (the work).
```

**docs/SPEC.md**

```md
> Tracker: TICKET-# (name of project)
Verified: YYYY-MM-DD (last developer review against the code; the only date to trust)

## Goals
## Users
## Glossary
(One line per domain term: term, meaning. One term per concept. Use the same term in code names, the docs, and the user guide.)
## Features & user stories
(Group by area; the same areas appear in the area map in ARCHITECTURE.md. Name the files and interfaces involved, where known.)
## Constraints
(Non-functional requirements: performance, compatibility, security, accessibility, localization. Give each a testable acceptance criterion where possible.)
## Acceptance criteria & test cases
(Group by feature or milestone. Give each criterion an ID and its test cases. Each ID has a matching check in RELEASE-CHECKLIST.md. End each group with an end-to-end criterion that proves the feature works. The AI proposes changes as PROPOSED blocks next to the text they change; see AGENTS.md Making changes.)
### (Feature or milestone) - Area: (name)
- **AC-1:** (Testable: "Form shows an inline error for an invalid email.")
  - Given:
  - When:
  - Then:
## Non-goals
(What is out of scope.)
## Open questions / assumptions
```

**docs/ARCHITECTURE.md**

```md
> Tracker: TICKET-# (name of project)
Verified: YYYY-MM-DD (last as-built review or release; the only date to trust)

## Stack
## Folder layout
## Commands
(Build, run, test, and lint commands added during development. Commands from the ticket stay in README.md.)
## Data model & APIs
(Include migrations, personal data, and retention, or say the project has none.)
## Auth & security
## Hosting & deployment
## Environment facts
(Facts the AI cannot learn from the code, 1 line each: staging and production URLs, folders or services never to touch, ports, accounts by name, and where things run. Names only, never secrets; values go in .env.example as names.)
## Operations
(Logging, error handling, backups and retention, monitoring, and what to do when it fails. Split to docs/RUNBOOK.md when this passes a page.)
### Known traps
(Lasting surprises and workarounds, moved here from HANDOFF.md: what, why, and how to avoid it.)
## Testing
- What must be tested: (e.g., business logic, API endpoints, key user flows)
- Tools: (e.g., the project's test framework; a saved script only if there is no framework)
- Minimum bar: (e.g., new code ships with tests; targeted tests and the docs check pass for each change; a full regression passes before each release)
- Docs check: (`...`). The list of checks is in the script's header comment. Runs with every change; writes `test-results/docs.md`.
- Full regression command: (`...`)
- Stronger checks: (the developer's answer to the rule 9 question: "not used", "what-if cases", or "what-if cases and mutation testing" with the tool and command)
### Area map
(One row per area. Use the feature groups from SPEC.md as the areas. Read this map before you search the code. Reading the files near your change is normal; use the map to find what is not near, such as the areas that use your code. In Depends on, name the shared libraries and other areas this area uses, most of all the ones that are not obvious from its folder. Update the map when code, tests, or areas change. Confidence: High = the developer reviewed the row; Inferred = from code, not yet reviewed; Low = incomplete read, and the row says where reading stopped.)
| Area | Code (folders or files) | Entry points | Tests | Command (this area only) | ACs | Depends on | Confidence |
|---|---|---|---|---|---|---|---|
| (Login) | (src/auth/) | (src/auth/login.js) | (tests/login.test.js) | (`npm test -- login`) | (AC-1, AC-2) | (Data model, Auth) | (High) |
| Docs | (scripts/check-docs.js) | | | (`node scripts/check-docs.js`) | | | High |
## Links to diagrams
```

**docs/PLAN.md**

```md
> Tracker: TICKET-# (name of project)
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
- DESIGN-GUIDE.html, if it exists, regenerated if DESIGN.md changed
- USER-GUIDE.md, if it exists, updated if user-facing behavior changed
- RELEASE-CHECKLIST.md updated if acceptance criteria changed
- `.env.example` updated if environment variables changed
- AGENTS.md Docs and Skills lists updated if files or skills changed
- Work board row updated, if the change has one (Status; Done when finished)

## Milestones
- [ ] M1: (name; owner; status)
- [ ] M2:

## Work board
(One row per work item in the current milestone. Claim an item by setting Owner and Status before you start. Two In progress rows may not list the same file; if another row holds a file you need, do not start: add the dependency and pick another item or ask. Mark Done; delete nothing. When open rows pass about 15, move this table to docs/plans/M<n>.md and keep only the milestones here. A solo developer keeps the board too; it stays short.)
| Item | Owner | Areas | Files | Status | Branch or ticket | Started |
|---|---|---|---|---|---|---|
| (Guest signup) | (Rick / Claude Code) | (Signup) | (src/forms/signup.js) | (Ready / In progress / Blocked / Review / Done) | (TICKET-12) | (YYYY-MM-DD) |
```

**AGENTS.md**

```md
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
```

**CLAUDE.md**

Keep the `@AGENTS.md` line first and on its own line. Do not copy rules from `AGENTS.md` into this file. A session-start hook that prints `docs/HANDOFF.md` is allowed as a Claude-only convenience; the rules do not depend on it.

```md
@AGENTS.md

## Claude-only notes
- When compacting, keep the list of changed files, the test commands that ran, the ready card, pending PROPOSED blocks, and open questions.
- (Optional: give a subagent that only searches or reads a smaller model. Keep the session's model for changes and reviews. Delete if unused.)
(Add only rules that apply to Claude Code and no other tool. Shared rules belong in AGENTS.md.)
```

**.github/copilot-instructions.md**

Keep the pointer lines first. Do not copy rules from `AGENTS.md` into this file.

```md
Follow all rules in AGENTS.md in the project root, including its Docs list.
When AGENTS.md has a Skills list and a task matches a skill in it, follow the instructions in
.claude/skills/<skill-name>/SKILL.md.

## Copilot-only notes
(Optional. Add only rules that apply to GitHub Copilot and no other tool. Shared rules belong in AGENTS.md. Delete this section if unused.)
```

**docs/decisions/0001-short-title.md** *(recommended; create when the first decision is recorded)*

```md
# 0001: Short title
Date: YYYY-MM-DD (the day the decision was made)
Status: Accepted (or Superseded by 000X)
Context: What problem or choice we faced.
Decision: What we chose.
Why: Main reasons, and what we rejected. (Link to the matching docs/RESEARCH.md entry if detailed results exist.)
```

**docs/RELEASE-CHECKLIST.md**

The release gate. Create it at setup with one check per acceptance criterion in `SPEC.md`, and keep it in step as the criteria change. Before each release, have a fresh-context review against the acceptance criteria and the docs, write the drift report, run a full regression, including every check, and replace the Latest release section. Example prompt: *"Prepare release 1.0: run the release checklist and record the results."*

```md
> Tracker: TICKET-# (name of project)

# Release checklist
Never mark a check passed without running it. Record how each one was verified. A human gives the final approval.

## Requirement checks
(One line per acceptance criterion in SPEC.md. Method: automated test, manual check, or both. For automated checks, the results file from the full regression is the evidence.)
| ID | Check | Method | Result |
|---|---|---|---|
| AC-1 | (Short description from SPEC.md) | (Automated / Manual) | (Pass / Fail / Not run: reason) |

## Release steps
- [ ] Fresh-context review done: code against the SPEC acceptance criteria, each command in README Run it and ARCHITECTURE Commands run, README and USER-GUIDE read against the code; each gap fixed or accepted by the developer
- [ ] Drift report written (SPEC with no code: Evidence; code with no SPEC: Evidence; AC with no test or test with no AC: Certain; broken path, link, or command: Certain; doc older than its code: Inference); each finding fixed or accepted by the developer
- [ ] Full regression passes: the whole test suite and the docs check, using the full regression command in ARCHITECTURE.md (results in test-results/latest.md and test-results/docs.md)
- [ ] No PROPOSED blocks remain in SPEC.md or DESIGN.md
- [ ] New or upgraded dependencies reviewed: license, maintenance, known vulnerabilities
- [ ] CHANGELOG.md has an entry for this release (create the file at the first release)
- [ ] Version number updated (if the project uses one)
- [ ] README.md, ARCHITECTURE.md, and USER-GUIDE.md (if used) match the release; Verified dates set in SPEC.md and ARCHITECTURE.md
- [ ] DESIGN-GUIDE.html regenerated if DESIGN.md changed
- [ ] .env.example is current; no secrets or customer data in code or docs
- [ ] Open items in the handoff file(s) resolved or accepted for this release
- [ ] (Project-specific steps, such as deployment or accessibility checks)

## Latest release
(Replaced at each release.)
- Release: (version), YYYY-MM-DD
- Checked by: (tool or developer)
- Result: (All checks passed, or each failed, not-run, or skipped check and why)
- Approved by: (developer name)
```

**docs/HANDOFF.md** *(recommended; create when first needed)*

A short session handoff so the next session (same tool, a different tool, or a different developer) can pick up without guessing. It is not created at initialization. Create it at the first session close, or earlier if something important must be recorded right away. Every bullet is dated, and the file stays within its cap (rule 8). Example closing prompt: *"I'm closing this session. Run the close routine: route each item, write it to its doc, then rewrite docs/HANDOFF.md."* On a team, each developer keeps `docs/handoff/<developer>.md` from the same template, and `docs/HANDOFF.md` is not used.

```md
> Tracker: TICKET-# (name of project)

# Session handoff
Last updated: YYYY-MM-DD by (tool or developer)

Session handoff only; rewritten at each session close, not after every change, at most 40 lines. If anything here conflicts with another doc, the other doc wins. Write for a reader who did not see the session: full paths and names, Glossary terms, no terms invented during the work.
Close routine: (1) Route: print 1 line per item worth keeping, `item -> target`, using the routes below. An item already in this file cannot route back here, even with a new date (the docs check counts carries), except a Waiting bullet for a pending PROPOSED block. (2) Write the routed items into their docs (PROPOSED blocks for SPEC and DESIGN). (3) Rewrite this file, never append, with only the leftovers; print the line count. After a close at the context limit, tell the developer to start a new session from this file.
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
```

**docs/plans/M<n>.md** *(optional; create when the work board in `PLAN.md` passes about 15 open rows)*

```md
> Tracker: TICKET-# (name of project)

# M<n>: (milestone name)
Owner: (name). Status: (Planned / In progress / Done).

## Work board
(Same rules as the board in PLAN.md: claim before you start; no two In progress rows share a file; mark Done, delete nothing.)
| Item | Owner | Areas | Files | Status | Branch or ticket | Started |
|---|---|---|---|---|---|---|
```

**docs/CHANGELOG.md** *(recommended; create at the first release)*

The release history, for testers, support staff, and other readers. It is not created at initialization. At each release, add one entry at the top, so the AI reads only the newest entries. Keep each entry short. Do not rewrite old entries.

```md
> Tracker: TICKET-# (name of project)

# Changelog
Newest release first. Add one entry at the top for each release. Do not rewrite old entries.

## (Version) - YYYY-MM-DD
- Added: (New features)
- Changed: (Changes to existing behavior)
- Fixed: (Bugs fixed)
```

**docs/DESIGN.md** *(optional; required if the project has a UI)*

Create this file for every project with a UI. Skip it for a project with no UI. The AI proposes changes as PROPOSED blocks (rule 1).

`docs/DESIGN-GUIDE.html` has no template. It is a human-readable, visual version of `DESIGN.md`, so graphic designers have an easy reference. Regenerate it from `DESIGN.md` every time `DESIGN.md` changes, in the same task. Never edit it by hand. Make it one self-contained HTML file that opens in a browser with no other files or build step. Follow the `DESIGN.md` headings, and show the values visually, such as color swatches, type samples, and spacing examples. Start it with a comment saying it is generated from `DESIGN.md` and must not be edited by hand.

```md
> Tracker: TICKET-# (name of project)

## Colors & typography
## Spacing & components
## Layout & responsive behavior
## Accessibility
## Links to wireframes and brand design guidelines
```

**docs/USER-GUIDE.md** *(optional; for projects with end users)*

Create this file once the product has a working feature that end users will use. End users are people who use the product but do not develop or maintain it. Write it for them, not for developers, in ASD-STE100 Simplified Technical English (STE): short sentences, active voice, one instruction per step, and approved words. Use the terms from the SPEC Glossary. Describe how the product works now, not planned features. Put screenshots in `docs/images/` and link them with relative paths.

```md
> Tracker: TICKET-# (name of project)

# (Product name) user guide

## What it does
(One or two short sentences: what the user can do with it.)

## Getting started
(How to open, sign in to, or install it.)

## Tasks
### (Task, such as "Book a stay")
1. (Step)
2. (Step)

![(Description of the screenshot)](images/(file-name).png)

## Troubleshooting
- (Common problem): (What to do)

## Getting help
(Who to contact.)
```

**docs/RUNBOOK.md** *(optional; create when the `ARCHITECTURE.md` Operations section passes a page)*

For the people who run and support the product. Move the Operations content here and leave a link in `ARCHITECTURE.md`. Update it directly when operations change.

```md
> Tracker: TICKET-# (name of project)

# Runbook
## Services and where they run
## Logs and monitoring
## Common failures and fixes
## Backups and restore
## Contacts and escalation
```

**docs/MOCKUP.md** *(optional)*

Create this file only when the project will include mockups or prototypes. Use it for mockup guidance, temporary sample data, and mockup-specific decisions that do not belong in the production requirements or design documents.

```md
> Tracker: TICKET-# (name of project)

# Mockup instructions
This file applies only to mockups and prototypes. It does not override `SPEC.md` or `DESIGN.md`.

## When to make mockups
- For a UI, layout, or copy change that is not small, show 2 or 3 different static mockups for review before you edit real components.
- Mock only what does not exist yet. For a component that exists, show the real component, for example in the project's component viewer.

## Baseline (example)
- Create each mockup as a single-screen, 16:9 SVG.
- Start with a clean, uncluttered pencil-wireframe style using an off-white background and dark gray strokes.
- Light green may be used for callouts that are not part of the interface.
- Use specified colors when color is important to the design.
- Use realistic temporary content from the project's own domain, with the terms from the SPEC Glossary.

## Project-specific instructions
Add mockup-specific decisions and user feedback here as the mockups evolve.
(None yet.)

## Temporary sample data
List any invented data or examples that should be used consistently. Do not use real customer data, secrets, or other sensitive information.
(None yet.)
```

**docs/RESEARCH.md** *(optional)*

Create this file only when development produces research results or data that do not belong in the plan, specs, architecture, or decisions. Record research worth keeping for a future session, such as options tried but not chosen, and lasting findings from the as-built review or a drift report. Skip trivial lookups. Record the choice itself in `docs/decisions/`, and keep the supporting results here.

```md
> Tracker: TICKET-# (name of project)

# Research
Newest entries first. Do not delete old entries. If a finding is no longer true, mark it "Outdated (date)" and add the new finding.

## Short topic title (YYYY-MM-DD)
- **Question:** What we needed to find out.
- **Options tried:** Each option and its results (numbers, behavior, limits).
- **Outcome:** What we used, and the main reason.
- **Fallback notes:** Options not used that could replace it, and what to watch for.
- **Sources:** Links, files, or tests the findings came from.
- **Not verified:** Anything uncertain or untested.
```

**docs/archive/INDEX.md** *(optional; create during a migration or an absorb)*

Lists every file moved into `docs/archive/` and what replaced it, so no session mistakes an old file for a current one. If the index already exists, add new rows to it.

```md
# Archive index
Original files moved here by a migration or an absorb, kept for history only. Do not use them as a source of current information. Never delete them.

Status: Migrated (all content moved to the current docs), Partly migrated (current sections moved; the rest was superseded), or Superseded (not migrated; replaced by a newer source).

| Original path | Status | Replaced by | Archived |
|---|---|---|---|
| (notes/auth-v2.md) | (Superseded) | (docs/decisions/0003-login-method.md) | (YYYY-MM-DD) |
| (SPEC-old.md) | (Migrated) | (docs/SPEC.md) | (YYYY-MM-DD) |
```

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

## Migrating or upgrading an existing project

Use this section when the project already has documentation: another format, a single notes file, or an older version of this standard. The other Initialization rules (existing files, missing information) still apply. Whatever the prompt, change nothing until the developer approves the plan in step 5. Change only documentation, never code, configuration, or dependencies. The 2 exceptions are the docs check script and `.gitignore` (rule 7). For rules 10 and 11, a migration or an upgrade is 1 task with no risk class, because it changes no code: the approved plan is its ready card, the docs check is its test, and its report ends with the task summary, with "Class: n/a". It needs no work board row.

**Upgrade only:** If the project already follows an older version of this standard, do steps 1, 8, and 9. A version block for an older stamp can name other steps to run.

1. **Back up.** Confirm with the developer that a backup or version-control commit exists. If not, copy the existing documentation to a folder outside the project and tell the developer where it is. If you cannot write outside the project, say so, and continue only when the developer agrees.
2. **Inventory.** Scan the whole project, including subfolders, for `.md`, `.txt`, `.rst`, `.adoc`, and similar text files, plus `.docx` and `.pdf` files you can read. Include rules files (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `.github/copilot-instructions.md`, `.github/instructions/`), decision records, test plans, QA or release checklists, user manuals or help pages, runbooks, wiki exports, TODO lists, changelogs, and release notes. Skip `docs/archive/` and dependency, build, and version-control folders (such as `node_modules`, `vendor`, `dist`, `bin`, `obj`, and `.git`). Do not read or migrate a developer's personal rules file outside the project, such as `~/.claude/CLAUDE.md`. List any file you could not read and ask how to handle it.
3. **Resolve currency.** Group the inventory by topic. For each topic with two or more sources, find the current source. Use these signals, strongest first:
   1. A direct statement from the developer.
   2. An explicit marker: "superseded", "deprecated", "obsolete", "old", "replaced by", or a link from a newer file.
   3. Agreement with the current code (evidence only; code never changes SPEC or DESIGN by itself).
   4. Dates or version numbers in the content or the file name (such as `v2`, `final`, or `2025-03`).
   5. Version-control history, if it exists.

   Do not rely on file modification dates alone; copies and sync tools (such as OneDrive or SharePoint) change them. Give each file one status: **Current**, **Partly current** (list the current sections), or **Superseded** (name what replaced it). If the signals disagree or are weak, mark the topic **Unresolved**.
4. **Map.** Give each Current item, and each current section of a Partly current file, a target file and section (for example, "requirements in README → `docs/SPEC.md` Goals"). Superseded content gets no target; it stays in the archive. Flag content with no target, content that conflicts with this standard, and content you would have to infer. When a newer source changed a choice recorded in a Superseded file, propose 2 decision notes from the original wording: the old choice ("Superseded by NNNN") and the new one (Accepted). For evidence of the change, such as failed tests, propose a `docs/RESEARCH.md` entry with the old finding marked "Outdated (date)".
5. **Get approval.** Show the developer the inventory, the currency table below, the mapping, the proposed decision notes and research entries, and the draft archive index (step 7). Ask about every Unresolved topic and every Low-confidence row; do not guess. Wait for approval.

   | Topic | Sources | Current source | Evidence | Confidence |
   |---|---|---|---|---|
   | (Login method) | (`auth.md`, `auth-v2.md`, `new-login.md`) | (`new-login.md`) | (Matches the code; newest date) | (High / Medium / Low) |
6. **Migrate**, using the starter templates:
   - Migrate only the content mapped in step 4. Reorganize its original wording; do not rewrite or drop it. Never invent content: if a section has no source, keep the heading and write "(Not documented yet.)" Mark content inferred from code "Inferred, needs developer review" in SPEC and DESIGN.
   - Give each acceptance criterion an ID, and create `RELEASE-CHECKLIST.md` with a matching check for each, carrying in existing test plans and release checklists.
   - Build the `ARCHITECTURE.md` area map from the existing code and tests, as its template says. If no command can run one area alone, write "(Full suite only)" and report it. Do not run the tests during the migration unless the developer asks. Write the docs check script and add its row.
   - Put user manuals or help content in `docs/USER-GUIDE.md`, and their images in `docs/images/`. Offer to convert the guide to Simplified Technical English. Put runbooks and operations notes in the `ARCHITECTURE.md` Operations section, or `docs/RUNBOOK.md` if they pass a page.
   - Copy decision records into `docs/decisions/` word for word, keeping their status (Accepted if none). Add the approved decision notes, using "(Not documented yet.)" for any part with no source, and number all notes together in date order. Link each approved research entry from its decision note.
   - Merge the rules files into `AGENTS.md`. Put project-specific rules under Conventions, Claude-only rules under Claude-only notes in `CLAUDE.md`, and Copilot-only rules under Copilot-only notes in `.github/copilot-instructions.md`. Report conflicts with this standard or between files. Then replace `CLAUDE.md` and `.github/copilot-instructions.md` with their templates, keeping those notes.
   - Move lasting content from handoff, session-note, or TODO files to its proper doc, and leftovers to `docs/HANDOFF.md` as dated bullets, with the routing printed.
   - Move changelogs and release notes to `docs/CHANGELOG.md`, newest first.
   - Update links to moved content. Point links to Superseded files at their replacements, not at the archive. Set the `AGENTS.md` version stamp to this document's version.
7. **Archive.** Move the originals into `docs/archive/` under their original paths, so files with the same name do not collide (`notes/auth.md` becomes `docs/archive/notes/auth.md`). For files that keep their path, such as `README.md` and the rules files, save a copy there before editing. Create `docs/archive/INDEX.md` from its template, or add to it, with one row per archived file. Never delete the originals or the backup.
8. **Upgrade.** The version stamp is in `AGENTS.md` (in `CLAUDE.md` before 1.2.0). If it is older than this document's version, bring the project's docs to the current templates. One rule covers every version after 2.1.2; this document has no steps for each version.
   1. **Older stamps first.** For a stamp older than 2.1.2, first apply the version blocks, in order: `UPGRADING-1.x.md` for a stamp before 1.5.1, then `UPGRADING-2.x.md`. Both files are in the KEEL repository, at the address in the version stamp. The blocks stop at 2.1.2. After them, continue as for a stamp of 2.1.2.
   2. **Read the changes.** Read the Changelog entries newer than the stamp. Each one has a summary and, when they apply, a Templates changed line and a Docs check line.
   3. **Apply the changes to each file.** The newer Changelog entries say what changed, and the current starter template gives the exact new text. Do this for `AGENTS.md`, `CLAUDE.md`, `.github/copilot-instructions.md`, and each doc that the project uses and that has a starter template. *Template text* is the headings, the guidance in parentheses, the fixed lines at the top of a file (such as the top of the handoff file), the fixed lists (the Definition of ready, the Definition of done, and the Release steps), and any other line that a Templates changed line names. *Project text* is what the project wrote.
      - The Templates changed line of each newer entry names the files, with the headings or the lines that changed. At each place that it names, make the file agree with the current template: add, replace, or remove the template text there. Below a new heading, add its guidance, or "(Not documented yet.)" where content is needed.
      - A difference from the template at a place that no newer entry names is the project's own change. Keep it, and do not restore the template text. List each such difference 1 time in the report, so the developer can decide. You cannot see the older templates, so this is the only safe reading.
      - Guidance in parentheses is different, because the project can delete it at setup. If the file still has a guidance line that changed, replace it with the current version. If the file does not have it, do not put it back. A new line that the template marks as optional is an offer (step 5), not a change.
      - In a handoff file, change only the fixed lines at the top. Do not change the Last updated line or a bullet that is there: the file is rewritten only at a close (rule 5). Step 9 adds Waiting bullets for the open questions.
      - Keep all project text. If its heading is gone, move it to the heading that the Changelog names. If the Changelog names none, ask.
      - `AGENTS.md` is the exception, because the AI loads it in every session: replace every template section with its current version. Then apply the setup rule again (see *Starter templates*): delete the optional parts that it names, and keep only the Docs entries for files that exist, plus the `docs/RESEARCH.md` entry. The project text is the project's own Conventions rules, the Ask before touching and Area owners lines, the Not used and Kept as is lines, the order of the Conventions rules, and the Skills list when the project has a skill. If the project has no skill, delete the Skills section. A default Conventions rule that the project changed or deleted stays as the project has it, and a guidance line that the project deleted stays deleted. Keep a Docs entry for a file that exists and is not a file from this document, such as a package README. If the project uses another word for the developer, such as "owner", use it in the template lines. Other text in the old file can be old template text or a project line, and you cannot see the older templates. Show each such line in the plan (step 8.6) before you remove it, so the developer can keep it as a Conventions rule. If the new file is above a cap of rule 8, propose cuts in the plan; never cut a project rule without approval. Add a default rule only when a Changelog entry newer than the stamp introduces it.
      - Never change accepted `SPEC.md` or `DESIGN.md` text: only their headings and guidance lines are template text. Never edit a decision note, an old `CHANGELOG.md` entry, or a file in `docs/archive/`.
   4. **Docs check.** If the project has no docs check script, write it as *Docs check and drift report* says, at the path that the area map names, else where the project keeps its tests (see *Folder layout*). Add its row to the area map, or correct the row that is there. For a script that exists: a Changelog entry names the checks that its version changed on its Docs check line. Change those checks first. Then compare the header comment of the docs check script with the list in *Docs check and drift report*, and add, change, or remove checks to match. A check that the project added is not in that list: keep it, put its line after the 9 checks in the header comment, and name it in the report. Update the comment, and set the version in its first line.
   5. **Questions and offers.** Ask each question that this document asks at setup, if the docs do not hold the answer. The docs hold the answers in these places: the tracker and the project name in the header lines, the UI answer in `DESIGN.md` (the file exists or it does not), and the stronger checks answer (rule 9) on the Testing line of `ARCHITECTURE.md`. Tell the developer, in 1 line each, what the newer Changelog entries add that the developer must ask for or decide: on-request reviews, optional rules, and optional template lines.
   6. **Approve and apply.** Show the developer the planned changes and wait for approval, as in Get approval (step 5 of this section). Then apply them, check `.gitignore` (rule 7), and set the stamp to this document's version, in the form of the `AGENTS.md` template. Run the docs check last, after the stamp is set and the Waiting bullets of step 9 are written.
9. **Report** what moved where, what was inferred, which Superseded content was not migrated and what replaced it, what was left unmapped, and any conflicts. Also list, with file names, each superseded approach that the code still appears to use (possible dead code or an unfinished change). After an upgrade-only path, report instead each file that changed with its changes, each project difference that you kept (step 8), the questions and the offers, and the result of the docs check. On every path, add each question that is still open to the handoff file as a dated Waiting bullet (rule 5), and do not change its Last updated line. Run the Initialization checklist and mark any item that does not pass. Then continue with *After the path* (Initialization rules).

## Changelog

Add a new entry at the top for each update, with its version and date, and update the version stamp in the `AGENTS.md` template to match. Keep an entry short: a summary of 2 to 4 sentences. From 2.2.0, the upgrade rule reads 2 more lines (step 8 of *Migrating or upgrading an existing project*), so add each one when it applies. The Templates changed line names each template file, with the heading or the lines that changed; it does not repeat the new text, because the template has it. The Docs check line names each changed check.

- **2.2.0 (November 11, 2026):** One general upgrade rule replaces the steps for each version (step 8 of *Migrating or upgrading an existing project*); the version blocks up to 2.1.2 are in `UPGRADING-2.x.md`. The `AGENTS.md` template is about 14% smaller and has a budget of 1,100 words (rule 8). The skills are smaller. New: a *Personal rules files* paragraph, a rule on deliberate blockers (rule 4), and exact definitions for the docs checks.
  - **Templates changed:** `AGENTS.md` (every section; the Docs list is shorter, and the Skills section is optional). `PLAN.md` (Definition of done: the Tested, Reviewed, and Affected docs lines). The handoff file (the Close routine line at the top). `README.md` (the Docs line). `.github/copilot-instructions.md` (the pointer lines). `MOCKUP.md` (a new "When to make mockups" heading). `CLAUDE.md` (a new optional note).
  - **Docs check:** every check has a new definition. Replace the whole list in the header comment, and add its first line, `Docs check for KEEL 2.2.0`.
- **2.1.2 (November 11, 2026):** 4 small rules. A question gets an answer, not a change, and the AI asks before a destructive action that the developer did not request (rule 10). A project rule can carry its exception, `Unless:` (rule 8). The task summary and the handoff file are written for a reader who did not see the session (rule 11).
- **2.1.1 (November 7, 2026):** The `AGENTS.md` template ships 2 default code rules in Conventions again: no clever or compressed code, and header and doc comments. In 2.1.0 they had become only an example, so new projects did not get them.
- **2.1.0 (November 6, 2026):** Changes from the first public feedback. The docs check catches a handoff bullet that is carried forward with a new date. Doc files outside the standard can be absorbed and archived at any time. Project rules carry a weight and a reason, and decisions carry a date. New on request: a cross-compare review and a docs cleanup. The `AGENTS.md` template is about 13% shorter and has a word cap. The 1.x upgrade blocks and changelog moved to `UPGRADING-1.x.md`, and `examples/` has an example project.
- **2.0.0 (October 31, 2026):** Renamed to KEEL, for any tracker. New: a close routine and a 40-line cap for the handoff file; PROPOSED blocks for SPEC and DESIGN changes; the area map, the docs check, and the drift report; risk classes, the ready card (rule 10), and the task summary (rule 11); the as-built review; and a work board in `PLAN.md` for teams. `AGENTS.md` has a cap of about 120 lines. The standard is also packaged as 6 skills, generated from this file by `build/build.py`.
- **1.x:** the entries for 1.0.0 to 1.5.1 are in `UPGRADING-1.x.md`.
