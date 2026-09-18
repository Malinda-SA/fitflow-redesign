# ADR-001: Single Cross-Platform Frontend with Flutter

| Field | Value |
|-------|-------|
| **Status** | Accepted |
| **Date** | 2026-09-18 |
| **Deciders** | FitFlow Engineering Team |
| **Category** | Frontend Architecture |

---

## Context

FitFlow must deliver a seamless, high-performance fitness experience on iOS, Android, and Web. The team cannot maintain three separate native codebases (Swift/SwiftUI for iOS, Kotlin for Android, React for Web) without unacceptable duplication of business logic, inconsistency in UI behaviour, and a prohibitive maintenance burden.

The four candidates evaluated were:
- **Flutter** — Google's UI toolkit using Dart and its own rendering engine
- **React Native** — JavaScript/TypeScript, renders to native components
- **Kotlin Multiplatform (KMP)** — Shared Kotlin logic with native UIs per platform
- **Swift/SwiftUI** — Apple-first, iOS/macOS only

---

## Decision

**Adopt Flutter as the single frontend framework for iOS, Android, and Web.**

Native platform channels will be used only for direct sensor access (HealthKit on iOS, Google Fit on Android) where Flutter's plugin ecosystem does not suffice.

---

## Rationale

1. **True multi-platform from one codebase** — Flutter is the only candidate that genuinely targets all three required platforms (iOS, Android, Web) without separate native UI layers.
2. **Performance** — Flutter renders via its own Impeller/Skia engine, bypassing native component bridges. This delivers consistent 60/120fps for animated workout tracking screens.
3. **Dart maturity** — Dart 3 with sound null safety eliminates a large class of runtime errors that plague JavaScript-based frameworks.
4. **HealthKit/Google Fit bridges** — Mature Flutter plugins (`health`, `flutter_health_connect`) provide the sensor access required for FitFlow's core features.
5. **Hot reload** — Significantly accelerates UI iteration during development.

---

## Consequences

### Positive
- One codebase to build, test, and deploy across all three platforms.
- Consistent UI/UX across platforms — no platform-specific design drift.
- Smaller team can ship faster without platform specialisation.

### Negative
- Dart is less common than JavaScript or Kotlin; new hires may require ramp-up time.
- Flutter Web has known limitations for SEO-heavy marketing pages (consider a static landing page in React for marketing only).
- Deep OS-specific features (e.g., custom Bluetooth stacks, ARKit) require small native bridge implementations.

---

## Alternatives Considered

| Option | Reason Rejected |
|--------|----------------|
| React Native | Bridge-based rendering causes frame drops during complex animations; no first-class Web support |
| Kotlin Multiplatform | Only shares business logic; still requires native UIs per platform — doesn't reduce frontend effort |
| Swift/SwiftUI | iOS/macOS only; Android and Web would need entirely separate solutions |
