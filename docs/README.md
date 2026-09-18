# Documentation Area

This directory contains all architecture documentation, decision records, and technology evaluation artifacts for the FitFlow redesign project.

## Contents

| File / Folder | Description |
|---------------|-------------|
| `tech-stack-summary.md` | Recommended tech stack with justifications |
| `comparison-matrix.md` | Weighted technology decision matrix |
| `architecture-diagram.md` | High-level system architecture (Mermaid diagram) |
| `adr/` | Architecture Decision Records |

## Architecture Decision Records (ADRs)

| ADR | Title |
|-----|-------|
| [ADR-001](adr/ADR-001-frontend-framework.md) | Single cross-platform frontend with Flutter |
| [ADR-002](adr/ADR-002-backend-split.md) | Split backend: NestJS for product API, FastAPI for AI |
| [ADR-003](adr/ADR-003-data-layer.md) | Polyglot persistence: PostgreSQL + MongoDB + Redis |
| [ADR-004](adr/ADR-004-identity-provider.md) | Delegate identity to Auth0 |
