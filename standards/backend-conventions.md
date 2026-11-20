# Backend Conventions

## Status

**Confirmed:** `tutor-backend` uses Spring Boot 4.1.1, Java 21, and Maven. The Maven Wrapper is configured for Maven 3.10.0, and the base Java package is `com.tutormarketplace`.

The repository uses a layered scaffold with Controller, Service, Repository, Entity, DTO, Mapper, Security, Exception, Config, and Util areas. Some of these areas are placeholders and must not be described as implemented behavior until corresponding source code exists.

Spring Data JPA and Flyway are configured for PostgreSQL. TUTOR-4 implements the persistence foundation; TUTOR-5 uses mapped JPA entities, Spring Data JPA repositories and JPA service transactions. Preserve user-before-session pessimistic lock ordering and explicitly refresh entities after locking to avoid stale persistence-context state. Database-generated normalization/audit fields are read-only mappings; shared user/profile/credential keys use `@MapsId`. Other feature repositories remain pending. See the [database](../database/schema.md) and [authentication](../api/authentication.md) contracts.

## Layer responsibilities

| Area | Responsibility currently documented |
| --- | --- |
| Controller | HTTP boundary |
| Service | Application/service logic |
| Repository | Spring Data JPA queries/locks and aggregate persistence coordination |
| Entity | JPA mappings of the existing private-schema tables, with enums persisted as strings |
| DTO | Boundary data transfer |
| Mapper | Representation mapping |
| Security | Cross-cutting security concerns |
| Exception | Exception concerns |
| Config | Configuration |
| Util | Shared utility code |

The confirmed base package is `com.tutormarketplace`. This table does not yet define subpackage naming rules or a strict dependency policy.

## Working rules

- Preserve the documented layered structure unless an accepted decision changes it.
- Verify current naming, package layout, and framework usage in `tutor-backend` before modifying code.
- Use the repository Maven Wrapper for project builds unless an accepted convention explicitly changes the build workflow.
- Do not place an unconfirmed business rule into code based only on a domain label.
- Update this KB when a new convention becomes accepted.

## To be defined

- Supported upgrade policy for Java, Spring Boot, Maven, and dependencies
- Standard build, test, and local-run commands
- Subpackage and symbol naming conventions
- Dependency-direction rules
- DTO/entity exposure policy
- Mapping approach
- Validation and transaction conventions
- Security conventions
- Exception and logging conventions
- Configuration and secret-management rules
- Static analysis and formatting tools

