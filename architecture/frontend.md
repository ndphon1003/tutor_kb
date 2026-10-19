# Frontend Architecture

## Status

**Confirmed:** The frontend uses Angular 22, TypeScript, SCSS, SSR/SSG, and a hybrid feature-based architecture.

## Repository

`tutor-frontend`

## Current high-level structure

```text
src/
├── core/
├── shared/
├── layouts/
└── features/
```

| Area | Boundary |
| --- | --- |
| `core` | Application-wide infrastructure and singleton concerns |
| `shared` | Reusable UI or code shared across features |
| `layouts` | Page shells and layout composition |
| `features` | Product capabilities grouped by feature |

Exact directory names below these areas must be verified in `tutor-frontend`.

## Feature locality

Feature-specific services, models, and state should remain inside their feature where appropriate. Move code to `shared` or `core` only when its responsibility is genuinely cross-feature.

## Rendering

- Server-side rendering (SSR) and static site generation (SSG) are part of the frontend technology profile.
- Route-level rendering choices, hydration behavior, and deployment configuration are **TBD** in this KB.

## Backend communication

- The frontend communicates with the Spring Boot backend using REST/HTTP.
- Endpoint paths, payload formats, authentication transport, and client error mapping are **TBD**.

## To be defined

- State-management approach
- Routing conventions
- Component and service naming rules
- Form and validation conventions
- Accessibility targets
- Styling and design-system rules
- SSR/SSG eligibility by route
- Frontend testing tools and coverage expectations

See [frontend conventions](../standards/frontend-conventions.md).

