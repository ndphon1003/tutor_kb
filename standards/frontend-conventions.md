# Frontend Conventions

## Status

**Confirmed:** `tutor-frontend` uses Angular 22, TypeScript, SCSS, SSR/SSG, and a hybrid feature-based structure of `core`, `shared`, `layouts`, and `features`.

## Placement rules

- Keep feature-specific services, models, and state inside their feature where appropriate.
- Use `shared` for genuinely reusable cross-feature code.
- Use `core` for application-wide infrastructure and singleton concerns.
- Use `layouts` for page shells and layout composition.
- Verify the current source tree before choosing a concrete path.

## Working rules

- Preserve SSR/SSG compatibility when changing browser-dependent behavior.
- Do not infer API payloads or business rules; use verified contracts and domain documentation.
- Update this KB when a new cross-feature convention becomes accepted.

## To be defined

- Angular workspace and folder details below the top-level areas
- File and symbol naming conventions
- Standalone component and dependency-injection conventions
- State-management pattern
- RxJS and async patterns
- Routing, guards, and resolvers
- Forms and validation
- Styling, theming, and responsive design
- Accessibility and browser support targets
- SSR/SSG route and hydration rules
- Formatting, linting, and testing tools

