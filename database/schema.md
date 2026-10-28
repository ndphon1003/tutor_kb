# Database Schema

## Status

**Database connected / Official MVP schema not yet defined**

The backend is connected to Supabase PostgreSQL. Spring Data JPA, the PostgreSQL driver, and Flyway are configured in `tutor-backend`.

The official application schema required to build the MVP has not yet been designed or created. No approved schema details or MVP Flyway migrations are recorded in this KB.

After the current KB grooming is completed, a dedicated development ticket will define and create the official MVP schema. This document will become the schema source of truth after that ticket is completed and verified.

## Current confirmed state

- Supabase PostgreSQL is the confirmed database technology.
- The backend datasource connection is configured and has been connected successfully.
- Flyway is configured to manage database migrations.
- No official application schema has been approved or documented yet.
- No MVP tables, relationships, constraints, indexes, or seed/reference data are defined by this KB.
- No approved MVP Flyway migration is present yet.
- Jira is not configured yet.

## Next planned work

After the current KB grooming is completed, the next planned development ticket is **MVP Database Schema Design & Creation**.

- This is roadmap information only.
- The ticket will define its detailed requirements and acceptance criteria before implementation begins.
- The ticket will design and create the schema required to build the confirmed MVP scope.
- The ticket will define application tables, relationships, constraints, indexes, migration ordering, and any required seed/reference data.
- The ticket will implement the accepted schema through versioned Flyway migrations.
- The current KB grooming does not make or record those schema-design decisions.

After the schema ticket is completed and verified, update this page and all affected architecture, domain, API, and engineering documents with the accepted and implemented state.

## Documentation rule

Do not treat proposed tables or relationships as implemented before the schema ticket establishes and verifies them. Until then, distinguish the confirmed database connection from the still-pending official application schema. After the ticket is completed, update this page to reflect only the accepted and implemented state.

