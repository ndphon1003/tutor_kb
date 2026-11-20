# API Error Handling

## Status

TUTOR-5 implements a shared error envelope for controller exceptions, JWT/session validation, CSRF and rejected CORS requests in local source. Feature-specific errors beyond authentication remain **TBD**.

## Verified current frontend behavior

- `ACCESS_TOKEN_EXPIRED` coordinates one refresh and at most one request replay. Authentication/bootstrap calls never recursively refresh.
- Invalid refresh or expired/revoked sessions clear memory and redirect to Login. Transient restoration failure exposes manual retry.
- Ordinary `403` does not clear authentication or refresh. Forms branch on stable codes, display field errors and preserve non-sensitive input.
- `RATE_LIMITED` can display server-provided `retryAfterSeconds`; ambiguous network failures never automatically replay business writes.

The envelope contains `code`, safe `message`, ISO `timestamp`, request `path`, server-generated `requestId`, and `fieldErrors` (an empty object when inapplicable). `X-Request-ID` repeats the identifier; errors use `Cache-Control: no-store`. Optional `retryAfterSeconds` is understood by Angular for future rate-limit responses.

| HTTP | Stable codes |
| --- | --- |
| 400 | `VALIDATION_FAILED` |
| 401 | `INVALID_CREDENTIALS`, `ACCESS_TOKEN_INVALID`, `ACCESS_TOKEN_EXPIRED`, `REFRESH_INVALID`, `SESSION_EXPIRED`, `SESSION_REVOKED` |
| 403 | `FORBIDDEN`, `ADMIN_REGISTRATION_FORBIDDEN`, `CSRF_REJECTED` |
| 409 | `IDENTITY_CONFLICT` |
| 500 | `INTERNAL_ERROR` |
| 503 | `SERVICE_UNAVAILABLE` |

No new account lockout or rate-limit business rule is implemented. Error bodies exclude parser/database diagnostics, stack traces, passwords and tokens. Login failures are generic. Refresh failure never changes the refresh cookie. See [authentication](authentication.md).

## To be defined

- Feature-specific error codes and not-found semantics beyond authentication
- Infrastructure rate-limit policy and producers of 429 responses
- Application-wide error reporting and observability beyond the shared request identifier
- Localization
- Recovery behavior for future feature operations

## Documentation requirements

Once decided, this page should contain:

- The canonical error envelope, if one is adopted
- A mapping from error categories to HTTP behavior
- Rules for separating safe public details from internal diagnostics
- Verified examples
- Compatibility expectations for clients

Do not expose stack traces, secrets, or internal implementation details in documentation examples.

