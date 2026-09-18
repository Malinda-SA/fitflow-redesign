# ADR-002: Split backend: NestJS for product API, FastAPI for AI

**Status:** Accepted

**Context:**
The product needs a robust, real-time-capable API layer and a separate AI/ML workload for personalized recommendations; no single language ecosystem excels at both.

**Decision:**
Run NestJS as the primary backend for auth delegation, workouts, social, and real-time features, and a dedicated FastAPI microservice for all model inference, communicating over an internal REST/gRPC API.

**Consequences:**
Gains: each service uses the best ecosystem for its job, and the AI service can be scaled, deployed, or replaced independently. Trade-offs: two runtimes to operate, and a network hop between product API and AI service that must be monitored for latency.
