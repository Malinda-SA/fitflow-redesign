# FitFlow Redesign

> Technology evaluation, architecture, and documentation for the FitFlow fitness app redesign.
> IT3060 - Human Computer Interaction, Lab Exercise 05.

## Tech Stack

| Layer            | Technology                     |
|------------------|--------------------------------|
| Frontend         | Flutter (iOS, Android, Web)    |
| Backend (API)    | NestJS (Node.js / TypeScript)  |
| Backend (AI)     | FastAPI (Python)               |
| Primary Database | PostgreSQL                     |
| Document Store   | MongoDB                        |
| Cache / Pub-Sub  | Redis                          |
| Authentication   | Auth0                          |

## Repository Structure

```
fitflow-redesign/
├── frontend/          # Flutter client (iOS, Android, Web)
├── backend/           # NestJS API and real-time gateway
├── ai-service/        # FastAPI AI microservice
├── docs/              # Architecture docs, ADRs, matrices
│   ├── adr/           # Architecture Decision Records
│   ├── tech-stack-summary.md
│   ├── comparison-matrix.md
│   └── architecture-diagram.md
├── .github/
│   └── workflows/     # CI/CD pipeline definitions
├── .gitignore
└── README.md
```

## Documentation

| Document | Description |
|----------|-------------|
| [Tech Stack Summary](docs/tech-stack-summary.md) | Full justification for every technology choice |
| [Comparison Matrix](docs/comparison-matrix.md) | Weighted decision matrix (Activities 1-3) |
| [Architecture Diagram](docs/architecture-diagram.md) | High-level system architecture (Mermaid) |
| [ADR-001](docs/adr/ADR-001-frontend-framework.md) | Frontend framework selection |
| [ADR-002](docs/adr/ADR-002-backend-split.md) | Backend split: NestJS + FastAPI |
| [ADR-003](docs/adr/ADR-003-data-layer.md) | Polyglot persistence strategy |
| [ADR-004](docs/adr/ADR-004-identity-provider.md) | Identity provider selection |

## Getting Started

```bash
# Clone the repository
git clone https://github.com/<your-username>/fitflow-redesign.git
cd fitflow-redesign

# Frontend
cd frontend && flutter pub get && flutter run

# Backend
cd backend && npm install && npm run start:dev

# AI Service
cd ai-service && pip install -r requirements.txt && uvicorn main:app --reload
```

## Branch Strategy

| Branch   | Purpose                                   |
|----------|-------------------------------------------|
| `main`   | Production-ready code (protected)         |
| `develop`| Integration branch for feature work       |
| `feature/*` | Individual feature branches            |

> **Branch protection** is enabled on `main`: pull-request reviews are required before merging.

## Contributing

1. Create a feature branch from `develop`.
2. Make your changes and commit with clear messages.
3. Open a pull request targeting `develop`.
4. Request a review before merging.

## License

This project is part of the IT3060 coursework at SLIIT.
