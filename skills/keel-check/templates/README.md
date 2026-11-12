# KEEL 2.2.1: starter templates

Generated from `KEEL.md` (https://github.com/rickflashman/keel). Do not edit by hand. Each template is a file in this folder at the path it takes in the project, with `.template` before its extension: copy `docs/SPEC.template.md` to `docs/SPEC.md`.

Copy these into each new project. Replace the placeholders (`Tracker`, `TICKET-#`, name, and web link if available), and follow or delete the guidance in parentheses. The `AGENTS.md` template is a maximum: at setup, delete the optional lines that do not apply (teams, area owners, and the Skills section when the project has no skill). The 2 code rules in Conventions are defaults: keep, change, or delete them. The Docs list holds only the docs whose reading rule no other section gives. Keep the entries for the files that exist in the project, plus `docs/RESEARCH.md`, which stays listed before it exists so the AI knows to create and use it. The other files from this standard need no entry. Keep the "Not used" and "Kept as is" lines, with "(none)" until the developer adds a file to one.

## `docs/ARCHITECTURE.md`

Template: `templates/docs/ARCHITECTURE.template.md`

## `docs/RESEARCH.md` *(optional)*

Template: `templates/docs/RESEARCH.template.md`

Create this file only when development produces research results or data that do not belong in the plan, specs, architecture, or decisions. Record research worth keeping for a future session, such as options tried but not chosen, and lasting findings from the as-built review or a drift report. Skip trivial lookups. Record the choice itself in `docs/decisions/`, and keep the supporting results here.
