# FitFlow Backend

NestJS-based API gateway with REST, WebSocket, and internal microservice communication.

## Overview

The backend is the core product API for FitFlow. It handles user management, workout tracking, nutrition logging, social features, and real-time updates. It delegates authentication to Auth0 and ML inference to the FastAPI AI microservice.

## Prerequisites

- Node.js 20 LTS ([download](https://nodejs.org/en/download/))
- npm 10.x
- PostgreSQL 16 (local or Docker)
- MongoDB 7 (local or Docker)
- Redis 7 (local or Docker)
- Auth0 tenant credentials

## Setup

```bash
# Install dependencies
npm install

# Copy environment template
cp .env.example .env
# Fill in your secrets in .env

# Start in development mode (hot reload)
npm run start:dev

# Start in production mode
npm run start:prod

# Run unit tests
npm run test

# Run end-to-end tests
npm run test:e2e
```

## Environment Variables

```
# App
PORT=3000
NODE_ENV=development

# Auth0
AUTH0_DOMAIN=your-tenant.auth0.com
AUTH0_AUDIENCE=https://api.fitflow.app

# Databases
DATABASE_URL=postgresql://user:password@localhost:5432/fitflow
MONGODB_URI=mongodb://localhost:27017/fitflow
REDIS_URL=redis://localhost:6379

# AI Service
AI_SERVICE_URL=http://localhost:8000
```

## Folder Structure (planned)

```
backend/
├── src/
│   ├── main.ts                  # App entry point
│   ├── app.module.ts            # Root module
│   ├── auth/                    # JWT validation, guards
│   ├── users/                   # User profile CRUD
│   ├── workouts/                # Workout session management
│   ├── nutrition/               # Nutrition log endpoints
│   ├── social/                  # Activity feed, posts, likes
│   ├── notifications/           # WebSocket gateway
│   └── ai/                      # Proxy to FastAPI AI service
├── test/
├── .env.example
├── package.json
└── tsconfig.json
```

## API Endpoints (planned)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/validate` | Validate Auth0 JWT |
| GET | `/users/me` | Get current user profile |
| POST | `/workouts` | Log a new workout |
| GET | `/workouts` | List user's workouts |
| POST | `/nutrition` | Log a meal |
| GET | `/feed` | Get activity feed |
| GET | `/recommendations` | Get AI recommendations |
