# ADR-003: Polyglot Persistence — PostgreSQL + MongoDB + Redis

| Field | Value |
|-------|-------|
| **Status** | Accepted |
| **Date** | 2026-09-18 |
| **Deciders** | FitFlow Engineering Team |
| **Category** | Data Layer Architecture |

---

## Context

FitFlow handles three fundamentally different types of data:

1. **Transactional health data** — user profiles, workout records, nutrition logs, health metrics. Requires strict integrity, compliance controls (HIPAA/GDPR), and relational querying.
2. **Social / activity feed data** — posts, comments, likes, milestone notifications. Requires high write throughput, flexible schemas (varied content types), and fast reads for infinite-scroll feeds.
3. **Ephemeral/real-time data** — session tokens, API cache, real-time pub/sub events for live workout sessions. Requires sub-millisecond access and TTL-based expiry.

No single database excels at all three workloads simultaneously.

---

## Decision

**Use three specialised datastores:**
- **PostgreSQL** — primary system of record for all health, user, and transactional data.
- **MongoDB** — secondary store for the social activity feed and flexible event documents.
- **Redis** — cache, session store, and pub/sub bus for real-time features.

---

## Rationale

### PostgreSQL (Primary)
1. **ACID transactions** — non-negotiable for health data where partial writes (e.g., incomplete workout records) would corrupt analytics.
2. **Row-Level Security (RLS)** — enforces data isolation at the database level, supporting HIPAA requirements.
3. **JSONB columns** — flexible enough to store semi-structured sensor payloads without a full document DB.
4. **pgcrypto extension** — enables column-level encryption for sensitive health fields.
5. **TimescaleDB compatibility** — future-proofs time-series queries over health metrics.

### MongoDB (Secondary)
1. **Document model** — the activity feed contains heterogeneous content (workout completions, social posts, achievement badges); a flexible document schema avoids complex polymorphic relational tables.
2. **High write throughput** — MongoDB's write scaling handles burst social events from a large concurrent user base more easily than PostgreSQL for this use case.
3. **Aggregation pipeline** — efficient for feed ranking and analytics queries over large document collections.

### Redis (Cache & Pub/Sub)
1. **Sub-millisecond reads** — API response caching dramatically reduces load on PostgreSQL for frequently read data (e.g., leaderboards, user stats).
2. **Pub/Sub** — Redis Pub/Sub powers the real-time WebSocket notifications in NestJS (workout session updates, social notifications).
3. **Session storage** — short-lived JWT refresh token rotation can be tracked in Redis with automatic TTL expiry.
4. **Simplicity** — Redis's in-memory model is operationally simpler than running a dedicated message broker (Kafka) at this stage.

---

## Data Ownership Map

| Data Type | Store | Justification |
|-----------|-------|---------------|
| User profiles | PostgreSQL | Relational, HIPAA-sensitive |
| Workout sessions | PostgreSQL | ACID, time-series potential |
| Nutrition logs | PostgreSQL | Relational, compliance |
| Health metrics (HR, steps) | PostgreSQL | Structured, encrypted |
| Activity feed posts | MongoDB | Flexible schema, high write |
| Social comments/likes | MongoDB | Document model |
| Achievement events | MongoDB | Heterogeneous event shapes |
| API response cache | Redis | Fast read, TTL-based |
| WebSocket pub/sub | Redis | Real-time event bus |
| Session tokens | Redis | Short-lived, auto-expiry |

---

## Consequences

### Positive
- Each data type sits on the store best suited to its access patterns.
- PostgreSQL RLS provides a compliance-ready data isolation layer.
- Redis eliminates expensive repeated queries for hot data.

### Negative
- Three datastores to operate, monitor, back up, and maintain.
- Cross-store consistency (e.g., a social post referencing a workout record in PostgreSQL) is eventual, not ACID.
- Developers must understand which store to use for each new data type.

---

## Alternatives Considered

| Option | Reason Rejected |
|--------|----------------|
| PostgreSQL only | Feed write throughput and flexible social schemas degrade relational model; JSONB workarounds add complexity |
| MongoDB only | Lacks ACID guarantees needed for health data; not HIPAA-eligible without significant additional controls |
| Firebase/Firestore | Proprietary, vendor lock-in, not HIPAA-eligible without BAA, limited SQL-style querying |
