# FitFlow AI Service

FastAPI-based microservice for ML inference, personalized workout recommendations, and nutrition suggestions.

## Overview

The AI service is a dedicated Python microservice that provides ML model inference for FitFlow. It is called internally by the NestJS backend via REST and returns personalized recommendations based on user workout history and health metrics.

## Prerequisites

- Python 3.12+
- pip or [uv](https://github.com/astral-sh/uv) (recommended)
- Access to PostgreSQL (for reading training data)
- Redis (for caching predictions)

## Setup

```bash
# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate      # Linux / macOS
venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt

# Copy environment template
cp .env.example .env
# Fill in your secrets

# Start development server (with hot reload)
uvicorn main:app --reload --port 8000

# Run tests
pytest
```

## Environment Variables

```
# App
PORT=8000
ENVIRONMENT=development

# Database (read-only replica preferred)
DATABASE_URL=postgresql://user:password@localhost:5432/fitflow

# Redis
REDIS_URL=redis://localhost:6379

# Auth0 M2M (to validate inbound tokens from NestJS)
AUTH0_DOMAIN=your-tenant.auth0.com
AUTH0_AUDIENCE=https://ai.fitflow.internal
```

## Folder Structure (planned)

```
ai-service/
├── main.py                    # FastAPI app entry point
├── routers/
│   ├── recommendations.py     # Workout recommendation endpoints
│   ├── nutrition.py           # Meal suggestion endpoints
│   └── analytics.py          # Progress analytics endpoints
├── models/
│   ├── workout_model.pkl      # Trained recommendation model
│   └── nutrition_model.pkl    # Trained nutrition model
├── schemas/
│   ├── request_schemas.py     # Pydantic input models
│   └── response_schemas.py    # Pydantic output models
├── services/
│   ├── recommendation_service.py
│   └── cache_service.py
├── tests/
├── requirements.txt
└── .env.example
```

## API Endpoints (planned)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/health` | Health check |
| POST | `/recommend/workout` | Get personalized workout plan |
| POST | `/recommend/meal` | Get personalized meal suggestions |
| GET | `/analytics/progress/{user_id}` | Get progress analytics |

## Auto-Generated Docs

When running locally, interactive API docs are available at:
- Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)
- ReDoc: [http://localhost:8000/redoc](http://localhost:8000/redoc)
