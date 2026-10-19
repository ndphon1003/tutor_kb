# API Error Handling

## Status

The API error contract is **TBD**.

The backend has an `Exception` concern in its layered structure, but no response schema or handling behavior is confirmed in this KB.

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

