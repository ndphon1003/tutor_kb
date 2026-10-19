# Backend Conventions

## Status

**Confirmed:** `tutor-backend` is a layered Spring Boot backend with Controller, Service, Repository, Entity, DTO, Mapper, Security, Exception, Config, and Util areas.

The presence of Repository and Entity areas does not mean a database, schema, or persistence implementation exists. Database work is **TBD / Not implemented**.

## Layer responsibilities

| Area | Responsibility currently documented |
| --- | --- |
| Controller | HTTP boundary |
| Service | Application/service logic |
| Repository | Repository concern; persistence behavior is **TBD** |
| Entity | Entity concern; database mapping is **TBD** |
| DTO | Boundary data transfer |
| Mapper | Representation mapping |
| Security | Cross-cutting security concerns |
| Exception | Exception concerns |
| Config | Configuration |
| Util | Shared utility code |

This table does not define exact package names or a strict dependency policy.

## Working rules

- Preserve the documented layered structure unless an accepted decision changes it.
- Verify current naming, package layout, and framework usage in `tutor-backend` before modifying code.
- Do not place an unconfirmed business rule into code based only on a domain label.
- Update this KB when a new convention becomes accepted.

## To be defined

- Java and Spring Boot versions
- Build tool and commands
- Package and naming conventions
- Dependency-direction rules
- DTO/entity exposure policy
- Mapping approach
- Validation and transaction conventions
- Security conventions
- Exception and logging conventions
- Configuration and secret-management rules
- Static analysis and formatting tools

