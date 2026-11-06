> GitHub: #42 (Room Booking API)
Verified: 2026-10-30 (last developer review against the code; the only date to trust)

## Goals
- Staff can book a meeting room for a time slot without a double booking.
- Each booking gets a confirmation email within 1 minute.

## Users
- Staff, through the front desk client app (not part of this project).
- The front desk team, which can cancel any booking.

## Glossary
- Room: a bookable meeting room, with a name and a capacity.
- Booking: 1 room for 1 time slot, held by 1 staff member.
- Slot: a start time and an end time, in whole 15-minute steps, stored in UTC.
- Overlap: 2 bookings for the same room whose slots share any minute.

## Features & user stories
### Bookings - Area: Bookings
- As a staff member, I book a room for a slot, so that the room is held for my meeting. Files: src/routes/bookings.js, src/booking/overlap.js.
- As the front desk, I cancel any booking. Files: src/routes/bookings.js.

### Notifications - Area: Notifications
- As a staff member, I get an email when my booking is made or cancelled. Files: src/notify/email.js.

## Constraints
- Performance: a booking request answers in under 300 ms. (No AC yet: see Open questions.)
- Security: only signed-in staff can book; only the front desk role can cancel another person's booking (AC-4).

## Acceptance criteria & test cases
### Bookings - Area: Bookings
- **AC-1:** A booking for a free slot is created.
  - Given: room R1 has no booking from 10:00 to 11:00 UTC
  - When: a staff member books R1 from 10:00 to 11:00
  - Then: the API answers 201 with the booking ID
- **AC-2:** A booking that overlaps another booking is refused.
  - Given: R1 is booked from 10:00 to 11:00
  - When: a staff member books R1 from 10:30 to 11:30
  - Then: the API answers 409, and no booking is created
- **AC-3:** A slot that is not in whole 15-minute steps is refused with 400.

> PROPOSED 2026-11-05 (Claude Code): add AC-7: a booking can last at most 4 hours; a longer slot is refused with 400.
> Reason: the client app now lets staff pick any end time, and a test booking held R1 for 3 days. Code: src/routes/bookings.js.

- **AC-4:** A staff member who is not front desk cannot cancel another person's booking (403).
- **AC-5 (end to end):** Book, read back, and cancel a booking through the API; the room is free again.

### Notifications - Area: Notifications
- **AC-6:** A confirmation email is sent within 1 minute of a booking, and within 1 minute of a cancellation.

## Non-goals
- Recurring bookings.
- Rooms in more than 1 building.

## Open questions / assumptions
- The performance constraint has no acceptance criterion yet. Dana decides the target load before release 1.1.
