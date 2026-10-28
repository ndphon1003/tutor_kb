# Product Scope

## Status

The product direction and MVP scope below are **Confirmed** product requirements. They do not imply that the corresponding features have been implemented.

## Market

- Tutor Marketplace serves Vietnam only.
- The initial product is not intended for markets outside Vietnam.
- Product behavior, content, and future operational decisions should be designed for the Vietnamese market.

## Customers and account types

The product serves two customer groups using separate account types:

- Tutor
- Tutor Seeker

A customer account cannot be both types. There is no separate Parent role or Parent account type. A student may register directly as a Tutor Seeker without being attached to a parent account.

Parents may use the product as Tutor Seekers, but parenthood does not create a distinct system role. Administrators support platform operation but are not currently identified as a customer group; the administrator identity model remains **TBD**.

A Tutor Seeker may look for a tutor for any tutoring need, regardless of who will receive the tutoring. The MVP does not require the intended learner or the relationship between the searcher and learner to be modeled.

A Tutor Seeker account requires a username, verified email address, full name, and phone number. A profile photo is optional. Phone numbers are not SMS-verified in the MVP. Contact information remains private until a Tutor accepts the connection request.

## Learning modes

The product supports both:

- Online tutoring
- In-person tutoring

The detailed location, travel-distance, meeting-place, and online-session rules remain **TBD**.

## Confirmed MVP scope

The MVP includes:

1. Registration and sign-in for the separate Tutor and Tutor Seeker account types
2. Tutor profile creation
3. AI-assisted CV pre-validation and final administrator approval
4. Tutor search
5. Tutor list/search-results view
6. Tutor detail view, including the administrator-approved sanitized CV
7. Conversational AI-assisted tutor recommendations
8. Free connection requests from Tutor Seekers to Tutors
9. Tutor acceptance or rejection of connection requests
10. Contact-information unlock after acceptance
11. Tutor connection/relationship history
12. Tutor reviews and ratings submitted by connected Tutor Seekers

Registration, profile approval, connection requests, accepted connections, and contact-information unlock are free in the MVP. Connection credits are planned post-MVP. See [Monetization](monetization.md).

## Explicitly outside the MVP

The following capabilities are post-MVP:

- Job postings
- Lesson booking and scheduling
- Formal tutoring-session management
- Attendance and lesson-completion tracking
- Rescheduling, cancellation, and lesson disputes
- Tutoring-fee payment, payout, refund, or platform commission
- Connection credits and paid connection plans

Online and in-person teaching remain supported discovery attributes in the MVP; excluding booking and session management does not remove either teaching mode.

Confirmed tutor-profile fields, search filters, list content, and detail content are documented in [Tutor](../domain/tutor.md). AI recommendations use a conversational experience and use subject, current level, learning goal, teaching mode, conditional location, budget, availability, professional background, and published reviews to rank 5 suitable Tutors. Recommendations optimize overall suitability and do not need to match every preference completely. The priority order is documented in [AI Tutor Matching](../domain/ai-matching.md); tie-breaking and evaluation criteria remain **TBD**.

AI conversation history and past recommendation results are not stored in the MVP. Only temporary context for the active conversation is maintained.

## Authentication boundary

Tutor Seekers must sign in to use tutor discovery in the MVP, including tutor search, tutor list/search-results, tutor details, and AI-assisted tutor recommendations. Authentication also enables learning history to be retained for the account.

The confirmed MVP credential model uses a unique username and password. Registration requires an email address and a one-time email OTP verification before the account becomes active. Routine sign-in requires only the username and password; activation OTP is not a recurring sign-in factor.

Passwords must be 8–128 characters, contain at least one letter and one digit, and must not equal the username or email address. Password recovery uses an email OTP, and a successful reset revokes all existing sessions. Activation and recovery OTPs use 6 digits, expire after 5 minutes, allow 5 incorrect attempts, enforce a 60-second resend delay, and invalidate older OTPs when replaced. Five failed attempts invalidate the OTP and block new OTP requests for 15 minutes. The MVP does not define an account lockout based on failed password attempts. Broader OTP rate limits, usage for other sensitive changes, and normal session duration remain **TBD**.

## Related domains

- [User](../domain/user.md)
- [Tutor](../domain/tutor.md)
- [AI tutor matching](../domain/ai-matching.md)
- [Connection request](../domain/application.md)
- [Tutor connection and learning history](../domain/session.md)
- [Review](../domain/review.md)
- [Monetization](monetization.md)

