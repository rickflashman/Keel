> GitHub: #42 (Room Booking API)

# Release checklist
Never mark a check passed without running it. Record how each one was verified. A human gives the final approval.

## Requirement checks
| ID | Check | Method | Result |
|---|---|---|---|
| AC-1 | A booking for a free slot is created | Automated | Pass |
| AC-2 | An overlapping booking is refused | Automated | Pass |
| AC-3 | A slot not in 15-minute steps is refused | Automated | Pass |
| AC-4 | Only front desk cancels another person's booking | Automated | Pass |
| AC-5 | Book, read, and cancel end to end | Automated | Pass |
| AC-6 | Confirmation email within 1 minute | Manual on staging | Pass |

## Release steps
- [ ] Fresh-context review done: code against the SPEC acceptance criteria, each command in README Run it and ARCHITECTURE Commands run, README read against the code; each gap fixed or accepted by the developer
- [ ] Drift report written; each finding fixed or accepted by the developer
- [ ] Full regression passes, using the full regression command in ARCHITECTURE.md (results in test-results/latest.md and test-results/docs.md)
- [ ] No PROPOSED blocks remain in SPEC.md
- [ ] New or upgraded dependencies reviewed: license, maintenance, known vulnerabilities
- [ ] CHANGELOG.md has an entry for this release
- [ ] Version number updated in package.json
- [ ] README.md and ARCHITECTURE.md match the release; Verified dates set in SPEC.md and ARCHITECTURE.md
- [ ] .env.example is current; no secrets or customer data in code or docs
- [ ] Open items in the handoff file resolved or accepted for this release
- [ ] Tag the release; the platform deploys the tag to production

## Latest release
- Release: 1.0.0, 2026-10-30
- Checked by: Claude Code (fresh session), then Dana
- Result: All checks passed. Drift report: 1 finding accepted (the Auth area map row is Inferred; Dana reviews it in M2).
- Approved by: Dana Ortiz
