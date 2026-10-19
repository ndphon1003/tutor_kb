# Tutor Marketplace Knowledge Base

`tutor-kb` is the documentation source of truth for the Tutor Marketplace project. It records confirmed architecture, shared domain language, project conventions, and accepted technical decisions for both developers and AI agents.

This repository contains documentation only. Application code belongs in the frontend or backend repositories.

## Repository ecosystem

| Repository / system | Responsibility | Current state |
| --- | --- | --- |
| `tutor-frontend` | Angular 22 client using TypeScript, SCSS, and SSR/SSG | Repository initialized |
| `tutor-backend` | Spring Boot service | Repository initialized |
| `tutor-kb` | Architecture, domain knowledge, and project rules | Active; this repository |
| Database | Future persistence layer | **TBD / Not implemented** |
| Jira | Future ticket and workflow system | Planned to be configured next |
| MCP integrations | Future agent integrations | Not configured |
| `tutor-agent` | Future Planner, Developer, and Reviewer agents | Not configured |

The local frontend and backend are confirmed to communicate successfully over REST/HTTP. No database has been designed, created, or initialized.

## How context is divided

The target context model, after Jira and agent integrations are configured, is:

```text
Jira       -> What needs to be done
tutor-kb   -> Architecture, domain knowledge, and project rules
Source     -> Current implementation
```

- Once Jira is configured, use it for ticket scope and acceptance criteria.
- Use this KB for agreed project context and constraints.
- Inspect source repositories for the current implementation.
- If these sources conflict, do not guess. Record the discrepancy, resolve it with the project team, and update the affected sources.

## Documentation status vocabulary

| Label | Meaning |
| --- | --- |
| **Confirmed** | Provided project fact or accepted decision |
| **Planned** | Intended future work, not current behavior |
| **TBD** / **To be defined** | No decision is recorded yet |

Unmarked detail must not be treated as an approved requirement.

## Knowledge base map

| Area | Contents |
| --- | --- |
| [`architecture/`](architecture/system-overview.md) | System boundaries and frontend/backend structure |
| [`domain/`](domain/user.md) | Domain vocabulary and known boundaries |
| [`api/`](api/conventions.md) | API and error-contract conventions |
| [`database/`](database/schema.md) | Database status now; schema source of truth after future database work |
| [`standards/`](standards/backend-conventions.md) | Engineering and collaboration conventions |
| [`decisions/`](decisions/README.md) | Architecture Decision Records (ADRs) |

## Product scope

Tutor Marketplace is intended to connect:

- Students / Parents
- Tutors
- Administrators

Planned product domains are users, tutor profiles, job postings, tutor discovery, applications, sessions, payments, reviews, and AI-assisted tutor matching. Listing a domain does not mean the feature is implemented. Detailed behavior and implementation status remain `TBD` unless a domain document says otherwise.

## Current architecture

```text
Angular Frontend
       |
       | REST/HTTP
       v
Spring Boot Backend

Database: TBD / Not implemented
```

See [System Overview](architecture/system-overview.md) for current and planned architecture boundaries.

## First planned development ticket

The first planned development ticket is **Database Design & Initial Setup**.

- This is roadmap information only.
- The ticket will define its requirements and acceptance criteria later.
- No database technology, schema, configuration, or implementation is defined by this KB update.

## How to use this KB

Before planning or implementation:

1. Read [`architecture/system-overview.md`](architecture/system-overview.md).
2. Read the relevant architecture, domain, API, database, and standards pages.
3. Confirm implementation details in `tutor-frontend` or `tutor-backend`.
4. Treat every `TBD` as an unresolved question, not permission to invent a rule.
5. Once Jira is configured, consult the relevant ticket for task-specific requirements.

The following agent responsibilities are planned. No agent or MCP integration is currently configured.

| Agent | Primary KB usage |
| --- | --- |
| Planner Agent | Identify constraints, dependencies, and unresolved decisions |
| Developer Agent | Implement against documented architecture and conventions |
| Reviewer Agent | Check changes against confirmed rules and decisions |

## Source-of-truth policy

> The Knowledge Base must reflect the actual confirmed state of the project. Planned features, architecture, infrastructure, or business rules must not be documented as implemented.

- Record only verified facts and accepted decisions as confirmed.
- Keep current architecture separate from planned architecture.
- Do not document proposed endpoints, tables, authorization behavior, payment rules, or matching logic as if they exist.
- Link an accepted cross-cutting decision to an ADR in [`decisions/`](decisions/README.md).
- Keep pages concise, searchable, and focused on one subject.
- Prefer Markdown headings, tables, lists, and small Mermaid diagrams over long prose.

## Updating documentation

- When a Jira ticket introduces or changes architecture, domain rules, API contracts, database design, or engineering conventions, update the relevant KB documentation as part of that ticket.
- Complete the KB update before completing the ticket whenever project knowledge changed.
- State whether new information is **Confirmed**, **Planned**, or **TBD**.
- Replace a `TBD` only after the decision has been made or the implementation has been verified.
- Update all affected pages and links; avoid duplicating the same rule across files.
- Add an ADR for decisions that are cross-cutting, costly to reverse, or need rationale.
- Review rendered Markdown and Mermaid diagrams before merging.

Expected future ticket workflow:

```mermaid
flowchart TD
    T[Jira Ticket] --> K[Read current KB]
    K --> P[Plan]
    P --> I[Implement]
    I --> R[Test / Review]
    R --> U[Update KB if knowledge changed]
    U --> C[Complete Ticket]
```

Jira is not configured yet, so this workflow is planned rather than active.

## Non-goals

- Application source code
- Jira task duplication
- Guessed implementation details
- Unapproved product or business rules
