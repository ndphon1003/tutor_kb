# Tutor Connection and Learning History Domain

## Status

**Confirmed product capability:** A Tutor Seeker account keeps a history of tutor relationships.

No session or learning-history implementation is confirmed by this page.

Formal lesson booking, scheduling, attendance, completion, cancellation, and dispute handling are explicitly post-MVP. The MVP history represents accepted Tutor connections, not verified lessons.

## Confirmed scope

- Learning history belongs to an authenticated Tutor Seeker account.
- The history records Tutor relationships associated with the Tutor Seeker account.
- Authentication is required so the history can be retained for the account.
- A Tutor Seeker sends a connection request and the Tutor accepts or rejects it.
- A history record is created when the Tutor accepts and the connection becomes `ACCEPTED`.

The product does not need to identify whether the Tutor Seeker studied personally or arranged the tutoring for someone else.

This is connection/relationship history. A record alone does not prove that a tutoring session occurred or was completed.

## To be defined

- Whether the same tutor can be selected more than once
- Removal, cancellation, and reselection behavior
- Post-MVP session fields, statuses, and lifecycle
- Post-MVP attendance and completion confirmation
- History fields, sorting, filtering, and retention
- Whether tutors can view corresponding history
- Whether administrators can access history and under what conditions
- Cancellation, rescheduling, absence, and dispute behavior
- AI access to connection history follows the explicit-request rule in [AI Tutor Matching](ai-matching.md)
- Relationship to payments and reviews

## Related domains

- [User](user.md)
- [Tutor](tutor.md)
- [Payment](payment.md)
- [Review](review.md)
- [Connection request](application.md)

