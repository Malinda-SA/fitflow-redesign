# Tech Stack Summary

## Recommended Technology Stack

| Layer | Technology | Version |
|-------|-----------|---------|
| Frontend | Flutter | 3.x (Dart 3.x) |
| Backend API | NestJS | 10.x (Node.js 20 LTS) |
| AI Microservice | FastAPI | 0.111.x (Python 3.12) |
| Primary Database | PostgreSQL | 16.x |
| Document Store | MongoDB | 7.x |
| Cache / Pub-Sub | Redis | 7.x |
| Authentication | Auth0 | - |

---

## Frontend: Flutter

**Decision:** Flutter over React Native, Kotlin Multiplatform, and Swift/SwiftUI.

**Justification:**
- Single Dart codebase targets iOS, Android, and Web — critical for FitFlow's multi-platform requirement.
- Renders via its own Skia/Impeller engine, providing near-native performance for smooth workout tracking animations.
- Strong native bridge support for HealthKit (iOS) and Google Fit (Android) via platform channels.
- Hot reload dramatically improves UI iteration speed.
- Google's backing ensures long-term stability and regular updates.

---

## Backend: NestJS (Core API)

**Decision:** NestJS over Express, Django REST Framework, and Spring Boot.

**Justification:**
- TypeScript-first: eliminates whole classes of runtime errors in API development.
- Module-based architecture mirrors domain-driven design, making the codebase scalable.
- Built-in WebSocket gateway enables real-time workout tracking and social notifications.
- Dependency injection and decorators reduce boilerplate and improve testability.
- Large ecosystem of official modules (Passport, TypeORM, GraphQL, Microservices).

---

## AI Microservice: FastAPI

**Decision:** FastAPI over Flask and Django for the AI workload.

**Justification:**
- Python is the dominant language of the ML ecosystem (TensorFlow, PyTorch, scikit-learn).
- FastAPI's async design handles concurrent ML inference requests efficiently.
- Auto-generated OpenAPI documentation simplifies the internal API contract.
- Pydantic models provide type-safe data validation at the ML inference layer.
- Deployed independently, so the ML model can be updated or swapped without impacting the core API.

---

## Primary Database: PostgreSQL

**Decision:** PostgreSQL over MySQL, Firebase Firestore, and DynamoDB.

**Justification:**
- ACID transactions ensure data integrity for health metrics — critical for HIPAA/GDPR compliance.
- Relational model suits structured data: users, workout plans, nutrition logs.
- JSON/JSONB columns provide flexibility for semi-structured data without leaving the relational model.
- Row-level security (RLS) enables fine-grained access control per user.
- Extensions (TimescaleDB, pgcrypto) unlock time-series and encryption capabilities.

**Secondary: MongoDB** — used for the high-volume activity feed and social posts where a flexible document schema and horizontal scalability are preferred over relational integrity.

**Cache/Pub-Sub: Redis** — used for session storage, API response caching, and real-time WebSocket pub/sub events.

---

## Authentication: Auth0

**Decision:** Auth0 over Firebase Authentication, AWS Cognito, and custom JWT.

**Justification:**
- HIPAA-eligible Business Associate Agreement (BAA) available — mandatory for handling health data.
- Supports OAuth 2.0, OIDC, MFA, social login, and Passwordless out of the box.
- Universal login hosted by Auth0 reduces surface area for auth vulnerabilities.
- Machine-to-machine tokens secure the internal NestJS-to-FastAPI communication.
- Dashboard provides audit logs for compliance evidence.
