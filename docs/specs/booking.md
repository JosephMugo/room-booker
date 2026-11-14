# Spec: Room booking (v1)

## Purpose
Staff book a meeting room for today without double-booking. The app shows
today's bookings and lets a person add a booking or cancel their own.

## Users
- Staff member: sees all of today's bookings, creates bookings, cancels their
  own (see R9–R11, R20–R21).

## Business rules
- R1. The start minute and the end minute of a booking are each 0, 15, 30, or 45.
- R2. When end is after start, a booking's duration is at least 15 minutes and at most 2 hours.
- R3. When end is not after start, the booking is rejected as too short.
- R4. A booking starts at or after 08:00 and ends at or before 18:00.
- R5. A booking may start at a time already past today when that start satisfies R4.
- R6. Two bookings for the same room must not overlap in time.
- R7. A booking may start at the minute another booking for the same room ends.
- R8. Overlap is checked only against bookings already saved for that room before the new booking is saved.
- R9. Only the person who made a booking can cancel it.
- R10. Every new booking is owned by the current user.
- R11. The owner may cancel their booking at any time today, including after it has started or ended.
- R12. When creating a booking breaks more than one of R1, R3, R2, R4, and R6, the app reports the first broken rule in the order R1, R3, R2, R4, R6.
- R13. When a cancel breaks R9, the app reports: not the owner.
- R14. The list of today's bookings shows room, start, end, and owner for each booking.
- R15. The list of today's bookings is ordered by start time ascending, then by room name ascending.
- R16. The available rooms are exactly Oak and Birch.
- R17. Entered start and end are shown as the same venue-local wall-clock times the person entered.
- R18. Today is the device's current calendar date.
- R19. Any seconds on start or end are discarded before rules run.
- R20. The app has a fixed list of people that includes Sam and Alex.
- R21. The current user is selected from that list without authentication.

## Acceptance criteria
Unless an AC says otherwise, each scenario starts with exactly one existing
booking: Sam, Oak, 10:00–11:00. Rejection messages are exactly the phrases
given in the matching AC (see AC34).

- AC1. When the current user is Alex and Alex books Oak from 09:00 to 09:30, the
  booking is accepted.
- AC2 (R1). When the current user is Alex and Alex books Oak from 09:10 to
  09:40, it is rejected: not on a 15-minute boundary.
- AC3 (R1). When the current user is Alex and Alex books Oak from 09:15 to
  09:45, the booking is accepted.
- AC4 (R1). When the current user is Alex and Alex books Oak from 09:00 to
  09:20, it is rejected: not on a 15-minute boundary.
- AC5 (R2). When the current user is Alex and Alex books Oak from 09:00 to
  09:15, the booking is accepted.
- AC6 (R2). When the current user is Alex and Alex books Oak from 12:00 to
  14:00, the booking is accepted. When Alex books Oak from 12:00 to 14:15, it
  is rejected: too long.
- AC7 (R3). When the current user is Alex and Alex books Oak from 09:00 to
  09:00, it is rejected: too short.
- AC8 (R3). When the current user is Alex and Alex books Oak from 09:30 to
  09:00, it is rejected: too short.
- AC9 (R4). When the current user is Alex and Alex books Oak from 07:45 to
  08:15 or from 17:30 to 18:15, it is rejected: outside opening hours.
- AC10 (R4). When the current user is Alex and Alex books Oak from 08:00 to
  08:15, the booking is accepted. When Alex books Oak from 17:00 to 18:00, the
  booking is accepted.
- AC11 (R5). Given the current time is 15:00, when the current user is Alex and
  Alex books Oak from 09:00 to 09:30, the booking is accepted.
- AC12 (R6). When the current user is Alex and Alex books Oak from 10:30 to
  11:30, it is rejected: the room is taken.
- AC13 (R7). When the current user is Alex and Alex books Oak from 11:00 to
  12:00, the booking is accepted.
- AC14 (R7). When the current user is Alex and Alex books Oak from 09:00 to
  10:00, the booking is accepted.
- AC15 (R6). When the current user is Alex and Alex books Birch from 10:00 to
  11:00, the booking is accepted.
- AC16 (R8). After the current user is Alex and Alex books Oak from 11:00 to
  12:00 and that booking is saved, when Alex books Oak from 11:30 to 12:30, it
  is rejected: the room is taken.
- AC17 (R9, R13). When the current user is Sam and Sam cancels Sam's booking,
  the booking is removed. When the current user is Alex and Alex tries to
  cancel Sam's booking, it is rejected: not the owner, and the booking remains.
- AC18 (R10). When the current user is Alex and Alex books Oak from 09:00 to
  09:30, the saved booking's owner is Alex.
- AC19 (R11). Given the current time is 10:30, when the current user is Sam and
  Sam cancels Sam's booking (10:00–11:00), the booking is removed.
- AC20 (R11). Given the current time is 12:00, when the current user is Sam and
  Sam cancels Sam's booking (10:00–11:00), the booking is removed.
- AC21 (R12). When the current user is Alex and Alex books Oak from 09:00 to
  12:00, it is rejected: too long (R2 before R6; the slot also overlaps Sam's
  booking).
- AC22 (R12). When the current user is Alex and Alex books Oak from 09:10 to
  16:00, it is rejected: not on a 15-minute boundary (R1 before R2).
- AC23 (R12). When the current user is Alex and Alex books Oak from 09:10 to
  09:10, it is rejected: not on a 15-minute boundary (R1 before R3).
- AC24 (R12). When the current user is Alex and Alex books Oak from 07:30 to
  07:00, it is rejected: too short (R3 before R4).
- AC25 (R12). This scenario starts with Sam, Oak, 17:00–18:00. When the current
  user is Alex and Alex books Oak from 17:30 to 18:15, it is rejected: outside
  opening hours (R4 before R6).
- AC26 (R14). The booking list shows Sam's booking with room Oak, start 10:00,
  end 11:00, and owner Sam.
- AC27 (R15). After the current user is Alex and Alex books Oak from 09:00 to
  09:30 and Birch from 09:00 to 09:30, the list order is: Birch 09:00–09:30
  (Alex), Oak 09:00–09:30 (Alex), then Oak 10:00–11:00 (Sam).
- AC28 (R16). The rooms offered for booking are only Oak and Birch.
- AC29 (R17). When the current user is Alex and Alex books Oak from 09:00 to
  09:30, the list shows start 09:00 and end 09:30 (same clock times entered,
  not shifted for display).
- AC30 (R18). A booking created while the device date is D is stored and shown
  as a booking for day D (today).
- AC31 (R19). When the current user is Alex and Alex books Oak from 09:00:30 to
  09:30:45, seconds are discarded first, the booking is accepted, and it is
  stored as 09:00–09:30.
- AC32 (R20). The fixed people list includes Sam and Alex.
- AC33 (R21). The current user can be set to Sam or to Alex without
  authentication.
- AC34. When a create or cancel is rejected, the person sees the rejection
  phrase from the matching criterion above, and nothing is saved / the booking
  is unchanged.

## Out of scope for this version
- Sign-in / authentication (current user is selected without auth; see R21).
- Saving bookings across a full page reload or between visits. Bookings are
  held only for the lifetime of the running app session; a full reload starts
  with no bookings except what the scenario seeds.
- Days other than today.
- Recurring bookings.

## Open questions
- None. Resolved in review: back-to-back bookings are allowed (R7), and the
  order of error messages is fixed (R12).
