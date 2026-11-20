# Frontend Architecture

## Status

**Confirmed:** The frontend lockfile currently resolves Angular Core, CLI, and SSR to 22.2.2; TypeScript to 6.0.3; RxJS to 7.8.2; and Vitest to 5.0.4. The project uses SCSS, Angular server output, client hydration, and a hybrid feature-based structure.

The project declares npm 11.12.1 as its package manager and Node.js 24.15.0 in `.nvmrc`. These versions describe the current source baseline; the upgrade and browser-support policies remain **TBD**.

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

The current source tree contains `core`, `shared`, `layouts`, and `features` under `src/app`. Many feature and shared subdirectories are placeholders and must not be described as implemented capabilities merely because their directories exist.

## Current implementation baseline

The frontend implements:

- The root application and router configuration
- Main and authentication layout shells
- Header, footer, and home page
- A home-page health check against `GET /api/health`
- Core authentication and role guards
- Authentication and 401 error interceptors
- Signals-based memory-only `AuthStore`, browser restoration, single-flight refresh and one-time recovery/replay
- Registration/login forms, protected account page, Access Denied, manual restoration retry and logout controls

The `auth` routes are implemented by TUTOR-5. `jobs` and `tutors` routes and other feature directories remain scaffolded.

## Feature locality

Feature-specific services, models, and state should remain inside their feature where appropriate. Move code to `shared` or `core` only when its responsibility is genuinely cross-feature.

## Rendering

- Angular server output is configured with an Express server entry point.
- Client hydration is enabled through `provideClientHydration()`.
- `/account` uses `RenderMode.Client`; public/auth screens use prerendering. Authentication restoration runs in the browser after rendering (or on client navigation when a guard awaits it); it never loads credentials during prerendering.
- The home-page backend health check runs only in the browser and is skipped during server rendering.
- The long-term route-by-route SSR, prerendering, or client-rendering policy and deployment topology remain **TBD**.

## Current application patterns

- Components and layouts use standalone Angular configuration and direct dependency imports.
- Top-level layouts and the home page are loaded with `loadComponent`.
- Authentication state currently uses Angular Signals and a computed authentication flag.
- Guards and HTTP interceptors use Angular's functional APIs.
- These are verified implementation facts, not yet an approved project-wide convention for every future feature.

## Backend communication

- The frontend communicates with the Spring Boot backend using REST/HTTP.
- The configured API base URL is `/api` in both development and production environment files.
- Local development proxies `/api` to the backend.
- The implemented health call uses `GET /api/health`.
- The auth interceptor sends a memory-only access JWT only to intended relative backend requests. Expired-token errors coordinate refresh; ordinary forbidden errors preserve authentication. Neither token is persisted in Web Storage.
- See the [authentication contract](../api/authentication.md) and [errors](../api/error-handling.md) for payloads, recovery, cookie/XSRF integration and pending browser verification.

## To be defined

- Project-wide state-management convention beyond the current Signals-based auth scaffold
- Routing conventions beyond the current lazy standalone-component pattern
- Component and service naming rules
- Form and validation conventions
- Accessibility targets
- Styling and design-system rules
- Long-term SSR, prerendering, and client-rendering eligibility by route
- Frontend test-level requirements, coverage expectations, and CI gates
- Upgrade and browser-support policy

See [frontend conventions](../standards/frontend-conventions.md).

