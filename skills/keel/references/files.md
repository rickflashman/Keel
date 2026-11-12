# KEEL 2.2.1: files

Reference for the `keel` skill, generated from `KEEL.md` (https://github.com/rickflashman/keel). Do not edit by hand.

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
