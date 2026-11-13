# Mobile Architecture

## Repository and applications

One repository, `gym-mobile`, contains one Kotlin Multiplatform project and four independently released apps:
- Owner Android — Jetpack Compose.
- Member Android — Jetpack Compose.
- Owner iOS — SwiftUI.
- Member iOS — SwiftUI.

Do not create four separate mobile code repositories. Do not use Compose Multiplatform UI.

## Shared KMP responsibilities
- API clients generated from approved OpenAPI specifications.
- Ktor networking and authentication transport.
- Domain models/use cases.
- Repositories and data mapping.
- Shared business rules and validation where suitable.
- Shared ViewModels and StateFlow state.
- Session orchestration.
- Common error interpretation.

## Native responsibilities
- Android: Compose screens, Navigation Compose, Android permissions, Keystore integration, platform services.
- iOS: SwiftUI screens, NavigationStack, Core Location integration, Keychain, platform services.
- Platform-specific accessibility and system integrations.
- Native UI should feel appropriate on each platform.

## Architecture style
Pragmatic Clean Architecture with feature-based modularization. Avoid excessive abstractions and duplicated layers.

## State and DI
- Kotlin Coroutines and StateFlow.
- Shared ViewModels.
- Compose lifecycle-aware collection.
- SwiftUI observes shared state through Kotlin/Native interop.
- Koin for dependency injection.
- No Decompose in V1.
- Native navigation per platform.

## Persistence and offline
- No SQLDelight/local SQL database or offline sync in V1.
- Android DataStore for preferences; Keystore for sensitive credentials.
- iOS UserDefaults for non-sensitive preferences; Keychain for sensitive credentials.
- Server is source of truth.
- In-progress workout autosaves online; interrupted session is resumed from backend state.

## Environment configuration
Build-time environment selection:
- Development -> local backend.
- Staging -> staging API.
- Production -> production API.
No production runtime environment switcher. A developer-only debug convenience may be added if useful.

Android uses Gradle product flavors; iOS uses Xcode configurations/schemes. API base URL is exposed through shared KMP configuration.

## Releases
- Four apps have independent bundle/application IDs, signing, versioning and release cadence.
- Semantic version and platform build number are separate.
- Google Play Internal Testing/Closed Testing and TestFlight.
- CI builds/tests; store submission is manual in V1.
- Sentry used across all four apps.
