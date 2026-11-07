# KEEL 2.1.1: reviews

Reference for the `keel` skill, generated from `KEEL.md` (https://github.com/rickflashman/keel). Do not edit by hand.

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
