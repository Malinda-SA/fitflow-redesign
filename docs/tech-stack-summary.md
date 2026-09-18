# Tech Stack Summary

**Frontend**: Flutter
**Backend (core API)**: NestJS (Node.js)
**Backend (AI microservice)**: FastAPI (Python)
**Primary Database**: PostgreSQL
**Authentication**: Auth0

## Justification
- **Frontend**: Flutter is the only option that satisfies iOS/Android/Web from one codebase while maintaining near-native performance for live workout tracking.
- **Backend**: NestJS gives a structured, TypeScript-based API layer with WebSocket support. FastAPI is the natural home for the AI microservice since Python owns the ML ecosystem.
- **Database**: PostgreSQL provides ACID guarantees for health metrics. MongoDB serves as a secondary store for high-volume social activity feeds.
- **Authentication**: Auth0 provides fastest time-to-compliance (HIPAA-eligible BAA, SOC2).
