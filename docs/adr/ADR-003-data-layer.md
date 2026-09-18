# ADR-003: Polyglot persistence: PostgreSQL + MongoDB + Redis

**Status:** Accepted

**Context:**
Health/transactional data needs strict integrity and compliance controls, while the social feed needs high write throughput and flexible schemas, and real-time features need fast pub/sub.

**Decision:**
Use PostgreSQL as the system of record for users and health data, MongoDB for the activity feed and logs, and Redis for caching, session storage, and pub/sub.

**Consequences:**
Gains: each workload sits on the datastore best suited to it. Trade-offs: the team must operate three datastores instead of one, requiring clear ownership boundaries and backup/DR plans for each.
