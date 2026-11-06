# 0001: Use SQLite for bookings
Date: 2026-09-22
Status: Accepted
Context: The API needs a database for rooms and bookings. Load is low: 1 office, under 50 bookings a day.
Decision: Use SQLite in a single file on the container's data volume.
Why: No database server to run or pay for, and the overlap check fits in 1 transaction. Rejected: PostgreSQL, because the platform team charges for a managed instance and the load does not need it.
