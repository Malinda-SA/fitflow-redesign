# ADR-001: Single cross-platform frontend with Flutter

**Status:** Accepted

**Context:**
FitFlow requires a seamless iOS, Android, and Web experience with high performance, delivered by a mid-sized team that cannot maintain three separate native codebases.

**Decision:**
Adopt Flutter as the single frontend codebase for all three targets, using native platform channels only for HealthKit/Google Fit sensor access.

**Consequences:**
Gains: one codebase, faster iteration, near-native performance. Trade-offs: Dart is a new language for most hires, and any feature requiring deep OS-specific APIs needs a small native bridge.
