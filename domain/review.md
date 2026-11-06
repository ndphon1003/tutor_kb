# Review Domain

## Status

**Confirmed MVP domain:** Connected Tutor Seekers can review and rate Tutors.

No review feature implementation is confirmed by this page.

**TUTOR-4 decisions:** Valid reviews publish immediately, including edits. Rating summaries use the arithmetic mean and count of stored reviews, with an absent average when no reviews exist; display rounding is deferred. Local migrations enforce integer stars and one review per pair, with a matching accepted-connection reference. Feature authorization/publication services and live deployment remain pending. See [Database schema](../database/schema.md).

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

The TUTOR-4 mean/count and immediate-publication decisions above supersede the former aggregation/visibility TBDs. Deletion, responses, reporting and later moderation behavior remain unresolved.

## Related domains

- [User](user.md)
- [Tutor](tutor.md)
- [Tutor connection and learning history](session.md)

