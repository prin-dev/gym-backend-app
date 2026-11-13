# Design System and Accessibility

## Shared foundations
The foundation repository owns design specifications:
- Semantic color roles and light/dark tokens.
- Typography scale.
- Spacing and sizing.
- Corner radii.
- Elevation/shadows.
- Icon sizing.
- Component behavior and interaction conventions.
- Owner versus Member experience guidelines.
- Platform-specific interaction guidance.

Actual Compose and SwiftUI components live in `gym-mobile`.

## Theme
- Light and Dark modes in V1.
- Follow system appearance by default.
- Manual System/Light/Dark preference.
- Store preference locally.
- Both themes must preserve readable contrast and component states.

## Localization
- English-only V1.
- Centralized strings; no hardcoded UI text in reusable components.
- Locale-aware date/time/currency formatting.
- Backend error codes are language-independent.
- Structure strings/templates for future localization.

## Accessibility baseline
- System font scaling.
- Screen-reader semantics and labels.
- Adequate contrast.
- Adequate touch target sizes.
- Accessible form validation and errors.
- Respect system appearance.
- Native Compose semantics and SwiftUI accessibility modifiers.

Formal WCAG certification/audit is not a V1 commitment.

## Component principles
- Share design tokens and behavior, not forced identical layouts.
- Owner and Member experiences may have distinct component sets.
- Android and iOS may use platform-native interaction patterns.
- Avoid decorative consistency that harms platform usability.
