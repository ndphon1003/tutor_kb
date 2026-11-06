# Database Schema

## Status

**TUTOR-4 migrations implemented in local backend source and verified on PostgreSQL 17. Live Supabase deployment is not yet verified.**

The accepted design is recorded in [TUTOR-4](https://nguyenphon1003.atlassian.net/browse/TUTOR-4?focusedCommentId=10002). These migrations establish persistence; authentication, authorization, approval, discovery and connection feature services remain subsequent work.

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
- `auth_session`: user FK, unique hashed session identifier, issue/expiry/revocation timestamps.

Authentication is application-managed. There is no Supabase Auth dependency or separate administrator identity table. Java roles are `TUTOR`, `TUTOR_SEEKER`, `ADMIN`. The backend must enforce role/profile compatibility, prevent dual customer profiles and public ADMIN registration, enforce activation/password/OTP rules, and consult revocation state. Those feature services are not delivered by the migrations. Never store plaintext passwords, OTPs or session tokens.

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

The backend's `scripts/verify-migrations.ps1` starts a fresh loopback PostgreSQL 17 cluster, uses real Flyway/JDBC tests and stops the server afterward. Ignored logs/data are retained under `target`.

Verification passed all 17 tests (16 database integration tests plus the existing health test). Checks cover clean migration, simulated Supabase roles, valid checksums/no pending versions, repeat application, enum string persistence, relational/numeric/time constraints, concurrent pending/connection uniqueness, pair-consistent reviews and runtime privileges. These persistence tests do not establish correctness of unimplemented feature services.

See `tutor_backend/docs/database-migrations.md` for the procedure and deployment preflight. Before live application, inspect existing schemas/history (including `public.database_change_log`), verify migration-owner permissions, provision runtime membership, approve catalog seed data and verify in an isolated Supabase project. No live Supabase deployment or reference seeding has been performed.

## Explicit exclusions

No jobs, lesson bookings, sessions, payments, credits, AI conversation/recommendation history, AI review structures, enum catalogs or default administrator credentials.

