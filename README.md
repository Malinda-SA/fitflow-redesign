# FitFlow Redesign

Technology evaluation, architecture, and documentation for the FitFlow fitness app redesign (IT3060 â€” Human Computer Interaction, Lab Exercise 05).

## Tech Stack
- Frontend: Flutter (iOS, Android, Web)
- Backend: NestJS (core API + real-time) and FastAPI (AI microservice)
- Databases: PostgreSQL (primary), MongoDB (feed/logs), Redis (cache/pub-sub)
- Authentication: Auth0

## Repository Structure
- rontend/ â€” Flutter client
- ackend/ â€” NestJS API and real-time gateway
- i-service/ â€” FastAPI AI microservice
- docs/ â€” comparison matrices, decision matrix, architecture diagram, and ADRs

## Documentation
See docs/tech-stack-summary.md for the full justification, docs/comparison-matrix.md for the weighted decision matrix, and docs/adr/ for architecture decision records.
