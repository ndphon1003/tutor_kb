# API Error Handling

## Status

The general API error contract is **TBD**.

No global exception handler, custom error envelope, stable application error code, or documented error-to-HTTP mapping is currently implemented in the backend source.

## Verified current frontend behavior

- The frontend error interceptor clears its local authentication session when it receives HTTP `401`.
- After that action, the interceptor rethrows the original error.
- Other HTTP and network errors are rethrown without a shared display, recovery, or translation policy.

This is current client behavior only. It does not establish the backend authentication-failure response contract, and it does not guarantee that every unauthorized backend condition currently returns `401`.

## To be defined

- Error response shape
- Stable error identifiers or codes
- HTTP status-code mapping
- Validation-error representation
- Authentication and authorization failures
- Not-found and conflict behavior
- Unexpected server-error behavior
- Safe client-facing messages
- Logging and correlation identifiers
- Localization
- Frontend display and recovery behavior

## Documentation requirements

Once decided, this page should contain:

- The canonical error envelope, if one is adopted
- A mapping from error categories to HTTP behavior
- Rules for separating safe public details from internal diagnostics
- Verified examples
- Compatibility expectations for clients

Do not expose stack traces, secrets, or internal implementation details in documentation examples.

