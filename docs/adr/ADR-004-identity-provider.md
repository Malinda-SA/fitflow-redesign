# ADR-004: Delegate Identity to Auth0

| Field | Value |
|-------|-------|
| **Status** | Accepted |
| **Date** | 2026-09-18 |
| **Deciders** | FitFlow Engineering Team |
| **Category** | Security & Identity |

---

## Context

FitFlow handles sensitive health and wellness data, placing it firmly in the category of applications that must meet HIPAA (Health Insurance Portability and Accountability Act) and GDPR (General Data Protection Regulation) requirements. Authentication is a high-risk surface area: vulnerabilities in a custom auth system can expose all user health data.

Candidates evaluated:
- **Auth0** — managed identity platform (Okta product)
- **Firebase Authentication** — Google's managed auth service
- **AWS Cognito** — Amazon's managed identity service
- **Custom JWT** — in-house implementation using NestJS + bcrypt + JWT

---

## Decision

**Use Auth0 as the identity provider for all user authentication, MFA, and social login.**

NestJS will validate Auth0-issued JWTs rather than managing credentials itself. Machine-to-machine (M2M) Auth0 tokens will secure internal service-to-service communication between NestJS and FastAPI.

---

## Rationale

1. **HIPAA-eligible BAA** — Auth0 offers a Business Associate Agreement (BAA), which is required for any service that processes Protected Health Information (PHI) on behalf of a covered entity. This is a hard requirement.
2. **GDPR compliance** — Auth0 provides data residency controls and processes personal data in accordance with GDPR.
3. **Security depth without custom code** — MFA, brute-force protection, anomaly detection, password breach detection, and bot detection are all included and maintained by Auth0's security team.
4. **Protocol standards** — OAuth 2.0, OIDC, and SAML are fully supported, enabling future enterprise SSO integrations.
5. **Social login** — Apple Sign-In, Google, and Facebook can be enabled with minimal configuration — important for fitness app user acquisition.
6. **Audit logs** — Auth0's tenant logs provide a ready-made audit trail for compliance evidence.
7. **M2M tokens** — Auth0 issues machine-to-machine tokens to secure NestJS-to-FastAPI internal calls without a custom token mechanism.

---

## Implementation Notes

- Flutter uses the `auth0_flutter` SDK for the universal login flow.
- NestJS uses `passport-jwt` with `jwks-rsa` to validate Auth0 tokens using Auth0's public JWKS endpoint.
- FastAPI validates the same JWTs using `python-jose` and the JWKS endpoint.
- Auth0 Actions (serverless functions) will enforce custom claims (e.g., `fitflow_role`) into the JWT for RBAC.

---

## Consequences

### Positive
- Fastest path to HIPAA/GDPR-compliant, secure authentication with minimal custom code.
- Security patches, protocol updates, and compliance certifications maintained by Auth0 professionals.
- Reduces engineering time spent on auth by an estimated 4–6 weeks vs. a custom solution.

### Negative
- **Cost** — Auth0 free tier supports 7,500 MAU; at scale, per-active-user pricing applies and can become significant.
- **Vendor lock-in** — migrating away from Auth0 would require re-implementing auth flows and migrating user credential records.
- **Uptime dependency** — if Auth0 experiences an outage, login will be unavailable; mitigated by Auth0's 99.99% SLA.

---

## Alternatives Considered

| Option | Reason Rejected |
|--------|----------------|
| Firebase Authentication | No HIPAA-eligible BAA available without Google Cloud Healthcare API; less enterprise-grade audit logging |
| AWS Cognito | HIPAA-eligible but complex configuration, inferior developer experience, limited UI customisation |
| Custom JWT | Fastest at scale cost-wise, but introduces significant security risk, requires dedicated security expertise, and delays time-to-compliance by weeks |
