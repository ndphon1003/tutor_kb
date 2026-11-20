# Authentication contract

## Status

TUTOR-5 is implemented in the local backend/frontend checkouts and covered by automated HTTP/PostgreSQL and Angular tests. Live deployment and browser integration verification remain pending. The revised [TUTOR-5 plan](https://nguyenphon1003.atlassian.net/browse/TUTOR-5?focusedCommentId=10003) confirms API-only ADMIN registration protected by an external `admin-register-key`; this supersedes the ticket description's older rejection of every ADMIN registration request.

## Registration and eligibility

- Registration accepts `username`, `email`, `password`, scalar `role`, and optional `fullName`/`phoneNumber`.
- Customer roles are `TUTOR` and `TUTOR_SEEKER`. Seeker contact fields are required; Tutors begin with incomplete `DRAFT` profiles.
- ADMIN provisioning requires exactly one nonempty matching `admin-register-key` header. Missing server configuration disables it. ADMIN has no customer profile. Angular never contains this secret or offers this account type.
- Identity, salted Argon2id credential and customer profile are inserted atomically. PostgreSQL enforces normalized username/email uniqueness using `lower(btrim(...))`; races return controlled conflicts.
- Passwords preserve their submitted value, contain 8–128 characters with a letter/digit, and differ from the trimmed username/email.
- Registration leaves email unverified, creates no session and sends no OTP/email. Only the exclusive Spring `local` profile permits unverified login. Without it, verification is required; `local` combined with any other active profile fails startup.
- Unknown usernames, incorrect passwords and ineligible verification states share `INVALID_CREDENTIALS`. No failed-password lockout policy is introduced.

## Operations

| Operation | Required transport | Success |
| --- | --- | --- |
| `GET /api/auth/csrf` | Public bootstrap | 204, `XSRF-TOKEN` cookie |
| `POST /api/auth/register` | JSON registration, CSRF, provisioning key for ADMIN | 201, current user |
| `POST /api/auth/login` | JSON username/password, CSRF | 200, access response, refresh cookie |
| `POST /api/auth/refresh` | Current refresh cookie, CSRF | 200, access response, rotated cookie |
| `GET /api/auth/me` | Bearer JWT, active session | 200, current user |
| `POST /api/auth/logout` | Bearer JWT, active session, CSRF | 204, revoke current session, clear cookie |
| `POST /api/auth/logout-all` | Bearer JWT, active session, CSRF | 204, revoke all existing sessions, clear current cookie |

Current user fields are `id`, `username`, `email`, `role`. Access response fields are `accessToken`, `accessExpiresAt`, `user`. No response body contains a refresh token. Authentication responses use `Cache-Control: no-store`. Public health remains available. All other requests require authentication; backend role checks use database-confirmed roles.

## Tokens and sessions

RSA keys are externally supplied with no development fallback. RS256 JWTs contain `sub`, stable `sid`, `iat`, `exp`, configured `iss`/`aud`, and expire after fifteen minutes. Every protected request verifies signature/claims/time and reads session ownership, revocation, expiry and current identity from PostgreSQL. Database unavailability fails closed with 503.

Each session has an absolute seven-day deadline from login. Refresh tokens contain 256 cryptographically random bits; only their SHA-256 hashes enter `auth_session.session_token_hash`. Rotation changes only that hash and preserves the session ID, issue time and deadline. Cookie lifetime is capped by the remaining deadline. Session expiry overrides any remaining access-JWT lifetime.

Session writes use JPA service transactions with Spring Data JPA pessimistic user locks before session locks. Explicit entity refreshes reload managed state after locking, so a cached snapshot cannot conceal another transaction's rotation or revocation. Rotation rechecks the current hash, giving one successful consumption. Losing/reused/invalid refresh requests issue no credentials and never modify the refresh cookie. Reuse does not revoke the winning session. Logout and logout-all serialize with rotation and cannot resurrect revocation.

## Browser security

The refresh cookie is host-only, HttpOnly, `SameSite=Lax`, `Path=/api/auth`. Secure is required except exclusive `local`, loopback bind and HTTP loopback frontend origins. Deletion uses matching scope. Local binds to `127.0.0.1:8081`. CORS origins are explicitly configured; wildcard credentialed origins fail startup and unapproved origins are rejected.

Unsafe requests require a separate JavaScript-readable CSRF cookie and matching `X-XSRF-TOKEN` header. Login/logout renew CSRF state. Angular uses relative `/api` through its existing proxy and built-in XSRF integration. Direct cross-origin clients must supply credential/CSRF transport explicitly.

Angular's Signals `AuthStore` holds access JWT/current user only in memory. Legacy `access_token` storage is deleted without reading it. Browser restoration and expiry recovery share one in-flight refresh; each request replays at most once after explicit `ACCESS_TOKEN_EXPIRED`. Authentication operations, forbidden responses and ambiguous network failures do not recursively refresh/replay. Terminal session failures redirect to Login; transient restoration failures expose manual retry.

Logout invalidates late state updates and coordinates in-flight refresh before final cookie deletion. Failure clears memory and displays that server revocation is unconfirmed. Guards await restoration; role failures use Access Denied. Restoration does not run during prerendering and `/account` uses client rendering. Tokens/user state are not embedded in prerendered output.

## Verification boundary

The backend runbook at `tutor_backend/docs/authentication.md` contains environment setup, API details, test commands and the pending browser checklist. Automated tests use an isolated PostgreSQL 17 cluster, unchanged migrations and a login inheriting restricted `tutor_app_runtime` privileges. They do not inspect or modify live Supabase. Jira remains In Progress pending browser acceptance checks and normal review.

See [errors](error-handling.md), [database](../database/schema.md), and [user domain](../domain/user.md).
