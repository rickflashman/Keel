> GitHub: #42 (Room Booking API)
Verified: 2026-10-30 (last as-built review or release; the only date to trust)

## Stack
Node.js 20, Express 4, SQLite 3 (better-sqlite3), Nodemailer. Tests: Vitest.

## Folder layout
- src/routes/: 1 route file per resource
- src/booking/: the overlap and slot rules
- src/notify/: email
- src/auth/: sign-in check and roles
- db/migrations/: numbered SQL files
- tests/: 1 test file per area

## Commands
- Run migrations: `npm run migrate`
- Lint: `npm run lint`
- Generate the API client types: `npm run gen` (writes src/generated/)

## Data model & APIs
- Tables: rooms (id, name, capacity), bookings (id, room_id, staff_id, starts_at, ends_at, created_at). Times are UTC.
- Personal data: staff_id and the staff email address (from the sign-in token). Bookings are deleted 1 year after their end time.
- API: `POST /bookings`, `GET /bookings/:id`, `DELETE /bookings/:id`, `GET /rooms`.

## Auth & security
Every request carries the company sign-in token. src/auth/ checks it and reads the role (staff or front desk).

## Hosting & deployment
A Linux container on the internal platform. A merge to main deploys to staging; a release tag deploys to production.

## Environment facts
- Staging: https://booking.staging.example.internal
- Production: https://booking.example.internal
- Never touch: the production database file /data/booking.db by hand; use migrations.
- Email: the internal relay at smtp.example.internal, port 25. Variable names are in .env.example.

## Operations
Logs go to standard output, and the platform collects them. A nightly job copies /data/booking.db to backup storage and keeps 30 days. If emails stop, check the relay first: the API keeps unsent emails in a retry table.
### Known traps
- Daylight saving: a slot entered in local time moves by 1 hour twice a year. Convert to UTC at the API edge (src/routes/bookings.js), never later. Found 2026-10-08.

## Testing
- What must be tested: the overlap and slot rules, every route, the roles, and email sending.
- Tools: Vitest. Email tests use a fake transport.
- Minimum bar: new code ships with tests; targeted tests and the docs check pass for each change; a full regression passes before each release.
- Docs check: `node scripts/check-docs.mjs`. The list of checks is in the script's header comment. Runs with every change; writes `test-results/docs.md`.
- Full regression command: `npm test && node scripts/check-docs.mjs`
- Stronger checks: not used (decided by Dana, 2026-10-30).
### Area map
(One row per area. Use the feature groups from SPEC.md as the areas. Read this map before you search the code. Reading the files near your change is normal; use the map to find what is not near, such as the areas that use your code. In Depends on, name the shared libraries and other areas this area uses, most of all the ones that are not obvious from its folder. Update the map when code, tests, or areas change. Confidence: High = the developer reviewed the row; Inferred = from code, not yet reviewed; Low = incomplete read, and the row says where reading stopped.)
| Area | Code (folders or files) | Entry points | Tests | Command (this area only) | ACs | Depends on | Confidence |
|---|---|---|---|---|---|---|---|
| Bookings | src/routes/bookings.js, src/routes/rooms.js, src/booking/ | POST, GET, DELETE /bookings; GET /rooms | tests/bookings.test.js | `npx vitest run tests/bookings.test.js` | AC-1 to AC-5 | Auth, Notifications, db/migrations/ | High |
| Notifications | src/notify/ | sendBookingEmail() | tests/notify.test.js | `npx vitest run tests/notify.test.js` | AC-6 | Nodemailer; the retry table in db/ | High |
| Auth | src/auth/ | requireStaff(), requireFrontDesk() | tests/auth.test.js | `npx vitest run tests/auth.test.js` | AC-4 | the company sign-in library (a shared package, not in this repository) | Inferred |
| Docs | scripts/check-docs.mjs (not included in this example) | | | `node scripts/check-docs.mjs` | | | High |
## Links to diagrams
- (None yet.)
