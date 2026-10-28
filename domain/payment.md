# Payment Domain

## Status

**Planned product domain:** Payments are part of the intended product scope.

No tutoring-fee payment feature or provider integration is confirmed by this page.

No payment provider, money-flow design, or payment business rule has been selected in this KB.

Tutoring-fee payment, payout, refund, and platform commission are explicitly outside the MVP.

## Confirmed MVP payment boundary

- Tutor Seekers are not charged a platform connection fee in the MVP.
- Tutor registration, profile approval, connection requests, accepted connections, and contact-information unlock are free in the MVP.
- Contact information is unlocked after acceptance without payment.
- Tutoring fees are not subject to a platform commission in the MVP.

Connection credits are planned post-MVP. Pricing, packages, payment integration, expiration, and refund rules are **TBD**. See [Monetization](../product/monetization.md).

## To be defined

- Payer and payee relationships
- Relationship to jobs, applications, or sessions
- Supported currencies and amount representation
- Payment provider and integration boundary
- Authorization, capture, settlement, payout, refund, and dispute behavior
- Fees, commissions, taxes, and invoicing
- Payment statuses and idempotency rules
- Financial audit and retention requirements
- Handling of provider webhooks and failures
- Credit purchase, balance, ledger, expiration, and refund implementation

Do not implement payment behavior based on assumptions. Payment decisions require explicit product, legal, security, and technical review.

## Related domains

- [User](user.md)
- [Tutor](tutor.md)
- [Application](application.md)
- [Tutor connection and learning history](session.md)

