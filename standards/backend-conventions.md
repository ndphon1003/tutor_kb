# Backend Conventions

## Status

**Confirmed:** `tutor-backend` uses Spring Boot 4.1.1, Java 21, and Maven. The Maven Wrapper is configured for Maven 3.10.0, and the base Java package is `com.tutormarketplace`.

The repository uses a layered scaffold with Controller, Service, Repository, Entity, DTO, Mapper, Security, Exception, Config, and Util areas. Some of these areas are placeholders and must not be described as implemented behavior until corresponding source code exists.

The backend is connected to Supabase PostgreSQL, and Spring Data JPA plus Flyway are configured. The presence of Repository and Entity areas does not mean the official MVP schema or persistence model has already been defined; that work belongs to the next planned schema ticket.

## Layer responsibilities

| Area | Responsibility currently documented |
| --- | --- |
| Controller | HTTP boundary |
| Service | Application/service logic |
| Repository | Repository concern; MVP persistence behavior awaits the official schema ticket |
| Entity | Entity concern; official database mappings have not yet been defined |
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

