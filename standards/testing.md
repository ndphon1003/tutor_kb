# Testing Standards

## Status

Testing tools, required test levels, quality gates, and coverage targets are **TBD**.

Do not assume framework defaults are accepted project standards. Verify the current test setup in `tutor-frontend` and `tutor-backend`.

## To be defined

| Area | Open decision |
| --- | --- |
| Backend | Unit, integration, repository, API, and security testing approach |
| Frontend | Unit, component, SSR/SSG, accessibility, and end-to-end testing approach |
| Database | Testing approach after database design and initialization are defined |
| API | Contract and compatibility testing |
| Payments | Provider sandbox and failure-path testing |
| AI matching | Evaluation data, quality measures, and regression testing |
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

