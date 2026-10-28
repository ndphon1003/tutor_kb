# Frontend Conventions

## Status

**Confirmed:** `tutor-frontend` currently resolves Angular Core, CLI, and SSR to 22.2.2; TypeScript to 6.0.3; RxJS to 7.8.2; and Vitest to 5.0.4. It uses SCSS and a hybrid feature-based structure of `core`, `shared`, `layouts`, and `features`.

The project declares npm 11.12.1 and Node.js 24.15.0. Angular server output, client hydration, and prerendering for all current routes are configured.

## Placement rules

- Keep feature-specific services, models, and state inside their feature where appropriate.
- Use `shared` for genuinely reusable cross-feature code.
- Use `core` for application-wide infrastructure and singleton concerns.
- Use `layouts` for page shells and layout composition.
- Verify the current source tree before choosing a concrete path.
- Treat empty feature, shared, or route directories as scaffolding, not implemented product functionality.

## Verified current patterns

- Current components and layouts use standalone Angular configuration.
- Current top-level page composition uses lazy `loadComponent` routes.
- Current authentication state uses Angular Signals.
- Current route guards and HTTP interceptors use functional APIs.
- Vitest is the configured unit-test dependency and Angular's unit-test builder is enabled.

These observations describe the current implementation. They do not establish a mandatory pattern for all future features until the corresponding convention is explicitly accepted.

## Working rules

- Preserve SSR/SSG compatibility when changing browser-dependent behavior.
- Keep browser-only APIs behind platform checks or browser-safe services.
- Do not infer API payloads or business rules; use verified contracts and domain documentation.
- Update this KB when a new cross-feature convention becomes accepted.

## To be defined

- File and symbol naming conventions
- Project-wide standalone-component and dependency-injection conventions
- Project-wide state-management convention beyond the current Signals-based auth scaffold
- RxJS and async patterns
- Routing, guard, and resolver conventions beyond the current scaffold
- Forms and validation
- Styling, theming, and responsive design
- Accessibility and browser support targets
- Long-term SSR, prerendering, client-rendering, and hydration rules
- Formatting and linting conventions
- Unit, component, SSR, accessibility, and end-to-end test requirements and quality gates
- Framework and dependency upgrade policy

