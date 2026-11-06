# Testing Standards

## Status

The backend now has a local GitHub Actions workflow and JaCoCo configuration. The requested policy is: every executed test must pass, migration integration tests must execute without skips, and coverage of added/modified executable Java lines must be strictly greater than 85%. GitHub workflow activation and required branch protection remain pending until the workflow is pushed and the check is registered. Other project-wide testing policies remain **TBD**.

Backend CI uses Java 21, the Maven Wrapper `clean verify`, and a fresh PostgreSQL 17 service. SQL migrations are verified through Flyway/JDBC tests, not a Java coverage percentage. The changed-code metric is weighted by executable line count; exactly 85% fails, missing coverage for changed Java files fails, and no changed executable lines are reported as N/A. JaCoCo HTML/XML, changed-code summaries and Surefire reports are retained as workflow artifacts.

See `tutor_backend/docs/ci.md` and `.github/workflows/backend-ci.yml` for implementation, comparison-base rules, local verification and merge-protection setup. This configuration has no Supabase/production credentials and does not deploy database changes.

Do not assume framework defaults are accepted project standards. Verify the current test setup in `tutor-frontend` and `tutor-backend`.

## To be defined

| Area | Open decision |
| --- | --- |
| Backend | Unit, integration, repository, API, and security testing approach |
| Frontend | Unit, component, SSR/SSG, accessibility, and end-to-end testing approach |
| Database | Migration, schema, repository, and integration testing approach to be defined with the planned MVP schema ticket |
| API | Contract and compatibility testing |
| Payments | Provider sandbox and failure-path testing |
| AI matching | Evaluation data, quality measures, and regression testing |
| AI CV review | Contact-data detection, obfuscation, OCR/QR extraction, false positives, fail-safe behavior, and sanitized-copy review |
| CI | Required checks and merge gates |
| Coverage | Measurement tools and thresholds |

## Minimum documentation expectation

When testing standards are accepted, record:

- Which behaviors require tests
- Approved tools and commands
- Test naming and placement
- Required CI checks
- Test-data and environment handling
- Ownership of flaky-test resolution

