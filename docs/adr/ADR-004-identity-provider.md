# ADR-004: Delegate identity to Auth0

**Status:** Accepted

**Context:**
FitFlow must support HIPAA/GDPR-appropriate authentication without dedicating engineering time to building and auditing a custom auth system.

**Decision:**
Use Auth0 for authentication, MFA, and social login, with the NestJS backend validating issued JWTs rather than managing credentials directly.

**Consequences:**
Gains: fastest path to compliant, secure auth with minimal custom code. Trade-offs: per-active-user cost at scale, and a dependency on a third-party identity provider's uptime.
