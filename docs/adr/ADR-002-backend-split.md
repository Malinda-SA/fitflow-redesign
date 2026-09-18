# ADR-002: Split Backend — NestJS for Core API, FastAPI for AI Microservice

| Field | Value |
|-------|-------|
| **Status** | Accepted |
| **Date** | 2026-09-18 |
| **Deciders** | FitFlow Engineering Team |
| **Category** | Backend Architecture |

---

## Context

FitFlow requires two distinct types of backend work:

1. **Product API** — structured CRUD operations, user authentication, real-time workout tracking (WebSockets), social features, and notification delivery.
2. **AI/ML workload** — personalised workout recommendations, nutrition suggestions, and progress analytics powered by machine learning models.

No single backend framework excels equally at both structured REST/WebSocket API development and ML model serving. Forcing both workloads into one service would create an awkward hybrid that uses the wrong tool for at least one job.

---

## Decision

**Run two backend services:**
- **NestJS** (Node.js / TypeScript) as the primary API gateway handling all product logic, real-time features, and auth delegation.
- **FastAPI** (Python) as a dedicated AI microservice handling all ML inference, communicating with NestJS via internal REST or gRPC.

---

## Rationale

### NestJS for Core API
1. **TypeScript-first** — full type safety from the API layer down to the database ORM (TypeORM).
2. **Module-based architecture** — enforces clean domain separation (WorkoutModule, NutritionModule, SocialModule).
3. **Built-in WebSocket gateway** — real-time workout tracking sessions require persistent connections; NestJS handles this natively.
4. **Decorator-driven controllers** — reduces boilerplate and keeps endpoint definitions readable.
5. **Microservices support** — NestJS has a native transport layer for gRPC, Redis queues, and Kafka, making it easy to communicate with FastAPI.

### FastAPI for AI Microservice
1. **Python is the ML ecosystem** — TensorFlow, PyTorch, scikit-learn, and Hugging Face are all Python-native.
2. **Async request handling** — handles concurrent inference requests efficiently via Python's `asyncio`.
3. **Auto-generated OpenAPI docs** — the internal API contract between NestJS and FastAPI is self-documenting.
4. **Pydantic validation** — ensures type-safe inputs and outputs at the inference boundary.
5. **Independent scaling** — the AI service can be scaled horizontally (e.g., GPU instances) without touching the core API.

---

## Consequences

### Positive
- Each service uses the best language/framework for its specific workload.
- The AI service can be updated, re-trained, or replaced without affecting the product API.
- Services can be scaled independently based on their distinct load profiles.

### Negative
- Two runtimes to operate, monitor, and deploy (Node.js and Python).
- A network hop between NestJS and FastAPI adds latency (~5–20ms internally); must be accounted for in SLA budgets.
- Two CI/CD pipelines required.

---

## Communication Protocol

| Scenario | Protocol |
|----------|----------|
| NestJS requests ML recommendation | Internal REST (HTTP/2) |
| High-throughput batch predictions | gRPC (future) |
| Authentication of service-to-service calls | Auth0 Machine-to-Machine (M2M) tokens |

---

## Alternatives Considered

| Option | Reason Rejected |
|--------|----------------|
| Single Django REST backend | Python is great for ML but lacks NestJS's structured TypeScript module system and native WebSocket support |
| Single NestJS with TensorFlow.js | TensorFlow.js is significantly less capable than Python TensorFlow; model ecosystem is much smaller |
| Spring Boot | Strong framework but Java/Kotlin adds operational complexity; overkill for this team size |
