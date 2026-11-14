# Spec: Room booking (v1)

## Purpose
Staff book a meeting room for today without double-booking. The app shows
today's bookings and lets a person add a booking or cancel their own.

## Users
- Staff member: sees all of today's bookings, creates bookings, cancels their own.

## Business rules
- R1. A booking starts and ends on a 15-minute boundary (:00, :15, :30, :45).
- R2. A booking lasts at least 15 minutes and at most 2 hours.
- R3. A booking starts no earlier than 08:00 and ends no later than 18:00.
- R4. Two bookings for the same room cannot overlap. One may start at the
  minute another ends.
- R5. Only the person who made a booking can cancel it.
- R6. When a request breaks more than one rule, the app reports the first one
  in the order R1, R2, R3, R4.

## Acceptance criteria
All criteria assume Sam has already booked the Oak room from 10:00 to 11:00.

- AC1. When Alex books Oak from 09:00 to 09:30, the booking is accepted.
- AC2 (R1). When Alex books Oak from 09:10 to 09:40, it is rejected: not on a
  15-minute boundary.
- AC3 (R2). When Alex books Oak from 09:00 to 09:00, it is rejected: too short.
- AC4 (R2). Oak from 12:00 to 14:00 is accepted. Oak from 12:00 to 14:15 is
  rejected: too long.
- AC5 (R3). Oak from 07:45 to 08:15 and from 17:30 to 18:15 are rejected:
  outside opening hours. Oak from 17:00 to 18:00 is accepted.
- AC6 (R4). When Alex books Oak from 10:30 to 11:30, it is rejected: the room
  is taken.
- AC7 (R4). Oak from 11:00 to 12:00 is accepted. Birch from 10:00 to 11:00 is
  accepted.
- AC8 (R5). Sam can cancel Sam's booking. Alex cannot.
- AC9. When a booking is rejected, the person sees a message naming the broken
  rule and nothing is saved.

## Out of scope for this version
- Sign-in. The app assumes one known user.
- Saving bookings between visits.
- Days other than today.
- Recurring bookings.

## Open questions
- None. Resolved in review: back-to-back bookings are allowed (R4), and the
  order of error messages is fixed (R6).
  