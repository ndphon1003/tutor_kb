# Review Domain

## Status

**Confirmed MVP domain:** Connected Tutor Seekers can review and rate Tutors.

No review feature implementation is confirmed by this page.

## Scope

The review author is a Tutor Seeker and the review subject is a Tutor. A Tutor Seeker becomes eligible after a connection request reaches `ACCEPTED`.

Eligibility is based on an accepted connection, not verified attendance or lesson completion, because formal session tracking is outside the MVP.

Published ratings are displayed on Tutor list and detail views and can be used as a tutor-search filter.

## Confirmed review content

- A rating is required.
- Ratings use a whole-star scale from 1 to 5.
- Written review text is optional.
- A review cannot be submitted unless the Tutor Seeker and Tutor have an `ACCEPTED` connection.

## Confirmed review uniqueness

- Each Tutor Seeker–Tutor pair may have at most one review.
- The Tutor Seeker edits the existing review instead of creating an additional review for the same Tutor.
- Updating a review changes its rating and optional text without increasing the Tutor's review count.

## To be defined

- Relationship to completed sessions or payments
- Editing, deletion, and response behavior
- Visibility, moderation, and reporting
- Aggregation and display rules
- Abuse prevention and audit requirements

No rating aggregation formula or moderation rule is currently documented.

## Related domains

- [User](user.md)
- [Tutor](tutor.md)
- [Tutor connection and learning history](session.md)

