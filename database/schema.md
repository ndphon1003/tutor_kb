# Database Schema

## Status

**TUTOR-4 migrations implemented in local backend source and verified on PostgreSQL 17. Live Supabase deployment is not yet verified.**

The accepted design is recorded in [TUTOR-4](https://nguyenphon1003.atlassian.net/browse/TUTOR-4?focusedCommentId=10002). TUTOR-5 now implements [authentication](../api/authentication.md) against this foundation without changing the migrations. Approval, discovery and connection feature services remain subsequent work.

The [Confluence Database Schema Version 01](https://nguyenphon1003.atlassian.net/wiki/spaces/TUTOR/pages/950277/Tutor+Marketplace+Database+Schema+Version+01) confirms table relationships, shared user/profile/credential keys, generated unique identity normalization and string-valued Java enums. TUTOR-5's JPA mappings cover the five identity/credential/session/customer-profile tables it uses; deferred feature tables remain defined by Flyway.

## Migration chain

Tables and Flyway history live in the private `tutor` schema. Hibernate uses `ddl-auto=validate` and `hibernate.default_schema=tutor`; Flyway uses `tutor.database_change_log`.

| Backend migration | Scope |
| --- | --- |
| `V1__create_identity_foundation.sql` | Shared identities, credentials, current OTP challenges, sessions, timestamp helper |
| `V2__create_profiles_and_catalogs.sql` | Empty business catalogs, separate profiles, discovery associations, weekly availability, credentials, CV versions, approval history |
| `V3__create_connections_and_reviews.sql` | Requests, status history, repeat accepted connections and reviews |
| `V4__restrict_application_access.sql` | Private schema and explicit runtime privileges |

Paths are under `tutor_backend/src/main/resources/db/migration/`. Applied migrations must not be edited after deployment to a shared environment; use new versioned migrations.

## Identity and authentication

- `app_user`: UUID identity; username/email and generated unique normalized keys using `lower(btrim(...))`; one textual role; email-verification and audit timestamps.
- `password_credential`: user PK/FK, password hash and password-change time.
- `email_otp_challenge`: user/purpose composite PK, protected OTP verifier, issue/expiry/resend times, attempts, consumed/invalidated and block timestamps. Replacement retains blocking state in the same row.
- `auth_session`: stable UUID session identifier, user FK, unique SHA-256 refresh-token hash in `session_token_hash`, issue/absolute-expiry/revocation timestamps. Rotation changes only the hash; the original seven-day deadline never slides.

Authentication is application-managed. There is no Supabase Auth dependency or separate administrator identity table. Java roles are `TUTOR`, `TUTOR_SEEKER`, `ADMIN`. TUTOR-5 enforces compatible profiles, normalized uniqueness, password policy, API-only key-protected ADMIN registration, local-only unverified sign-in and persistent session/revocation checks. OTP functionality remains deferred. JPA entities use shared-key `@MapsId` relationships for credentials/profiles, a lazy user relationship for sessions, and read-only mappings for generated normalization/audit fields. JPA service transactions lock and refresh the user before session mutation; migrations supply the underlying constraints/privileges. Never store plaintext passwords, OTPs or session tokens.

## Profiles and discovery

- `tutor_seeker_profile`: user PK/FK, required full name and phone, optional photo, audit timestamps.
- `tutor_profile`: user PK/FK, profile/contact information, education/major, experience, exact rate range/unit, approval status, approved CV reference, audit timestamps. Incomplete drafts are allowed.
- `subject`, `grade_level`, `location_area`: evolving business catalogs with unique codes. Locations have a parent FK. They remain empty pending approved datasets and versions.
- `tutor_subject`, `tutor_grade_level`, `tutor_teaching_area`: unique FK-backed catalog associations with discovery indexes.
- `tutor_teaching_mode`: unique Tutor/mode pairs.
- `tutor_availability`: weekday, start/end time and Tutor FK. Several windows per day are supported; duplicate/reversed/zero-length windows are rejected.
- `tutor_credential`: optional certificate/achievement type, descriptive information and timestamps.

Rates and request budgets are exact numeric VND amounts per `HOUR`; nonnegative ordered ranges reject NaN. Weekly windows use `Asia/Ho_Chi_Minh` and Java `DayOfWeek` names. Overnight windows split at midnight; PostgreSQL's `24:00` end boundary requires explicit Java time handling. The backend normalizes overlaps. No lesson scheduling, learner profiles, parent-child relationships or SMS verification is introduced.

## CVs and administrator approval

- `tutor_cv_version`: Tutor/version identity, private original and optional sanitized object identifiers, file metadata, creation time. Sanitized metadata is supplied together. Files remain outside PostgreSQL.
- `tutor_profile.approved_cv_version_id`: composite ownership FK prevents selecting another Tutor's CV.
- `tutor_approval_event`: append-only previous/new status, acting user, optional version/reason and timestamp.

The foundation uses `DRAFT`, `PENDING_ADMIN_REVIEW`, `APPROVED`, `REJECTED`, `SUSPENDED`. Only approved profiles qualify for discovery. The backend must validate completeness, ADMIN decisions, safe sanitized publication and important professional changes requiring re-review; it must not overwrite approved CV contents in place.

AI pre-validation remains eventual product direction but is deferred from this foundation. No AI review, extraction, finding, model, prompt or confidence data is stored. The original CV stays private. The foundational manual-review lifecycle above supersedes earlier schema-planning references to an implemented `AI_REVIEW` stage; eventual AI integration is future work.

## Connections and reviews

- `connection_request`: seeker/Tutor profile FKs, subject, grade/current proficiency, learning goal, mode, conditional location, required availability text, budget range/unit, optional message, status and temporal fields.
- `connection_request_status_event`: append-only transitions and actor/system attribution.
- `tutor_connection`: append-only accepted relationship; unique source request and pair-consistent FK. Multiple accepted connections per pair are allowed.
- `review`: matching accepted-connection/pair reference, integer rating 1–5, optional text, audit timestamps. One review per pair across repeat connections. Valid reviews publish immediately and edits preserve row count. Arithmetic average/count are derived; no reviews means absent average.

There is no `connection_request_availability` table. The pending-pair partial unique index uses `PENDING`; the request deadline defaults to seven days. Backend services must lock/check deadlines and lifecycle, serialize pair cooldown checks, enforce the 30-day rejection cooldown, and accept/create history atomically. A connection FK proves pair ownership, not valid request acceptance. In-person location validation, contact-channel detection, contact unlock, expiry processing and email delivery remain backend feature responsibilities.

## Enum and integrity policy

Java enums under `com.tutormarketplace.constant` define role, OTP purpose, approval/request status, teaching mode, rate unit and credential type. Persist names, never ordinals. Weekdays use the standard Java enum. No PostgreSQL enum types, enum lookup tables or allowed-value checks are created. `location_area.area_type` is approved-dataset metadata, not an invented enum vocabulary.

PostgreSQL enforces PK/FKs, required fields, normalized uniqueness, one profile per table/user, catalog integrity, numeric/time bounds, unique pending pairs, one connection per request and one review per pair. Backend services enforce enum membership, profile exclusivity and lifecycle transitions. Renaming persisted enum strings may require data/index migrations.

## Permissions and retention

`tutor_app_runtime` is an unprivileged NOLOGIN group. Provision the backend login/membership separately; use `FLYWAY_USER` and `FLYWAY_PASSWORD` for the migration owner. Migrations reject a privileged pre-existing runtime group.

PUBLIC and standard Supabase API roles receive no application-schema access. Keep `tutor` out of exposed Data API schemas. Runtime manages mutable data and can replace associations/windows; catalogs are read-only and history/connections append-only. Runtime has no access to Flyway history or automatic grants to future tables. Identity/profile/CV/review/history deletion and cascading deletion are not enabled; retention remains TBD.

## Verification and deployment boundary

The backend's `scripts/verify-migrations.ps1` starts a fresh loopback PostgreSQL 17 cluster, runs independent Flyway/JDBC migration tests plus JPA/HTTP authentication integration tests, and stops the server afterward. Ignored cluster logs/data are retained under `.verification/`, outside Maven's `target/` so clean builds can run safely.

The current clean backend verification passed all 44 tests with zero skips: 16 independent migration tests, 13 JPA/HTTP authentication integration tests, 5 authentication-configuration tests, 9 coverage-gate tests and the health test. Checks cover migration history/constraints/privileges, mapped shared keys/generated columns, registration rollback, JWT/session behavior, concurrent rotation/revocation and stale JPA persistence-context refresh. Changed executable Java line coverage is 98.99% for the current TUTOR-5 working tree. These tests do not establish correctness of unimplemented feature services or live deployment.

See `tutor_backend/docs/database-migrations.md` for the procedure and deployment preflight. Before live application, inspect existing schemas/history (including `public.database_change_log`), verify migration-owner permissions, provision runtime membership, approve catalog seed data and verify in an isolated Supabase project. No live Supabase deployment or reference seeding has been performed.

## Explicit exclusions

No jobs, lesson bookings, sessions, payments, credits, AI conversation/recommendation history, AI review structures, enum catalogs or default administrator credentials.

