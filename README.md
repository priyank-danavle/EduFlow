# EduFlow — Full-stack learning platform

A portfolio project scaffold for the L&T EduTech Engineer Trainee role. It includes a React + TypeScript frontend, Node.js/Express + TypeScript backend, MySQL persistence, MongoDB document/chat storage, REST APIs, GraphQL, and a Python/FastAPI document Q&A service.

> This is a runnable MVP foundation, not a claim that every production concern is solved. Add tests, authorization hardening, deployment configuration, and a proper vector database before presenting it as production-ready.

## Architecture

- `frontend/`: React, TypeScript, Vite
- `backend/`: Express, JWT authentication, REST, GraphQL, MySQL, MongoDB
- `ai-service/`: FastAPI PDF/text ingestion and retrieval-based Q&A
- `docker-compose.yml`: MySQL and MongoDB local services

## Prerequisites

- Node.js 20+
- Python 3.11+
- Docker + Docker Compose

## 1. Start databases

From the project root:

```bash
docker compose up -d mysql mongo
```

MySQL creates the `eduflow` database. The backend creates its tables on startup.

## 2. Start backend

```bash
cd backend
cp .env.example .env
npm install
npm run dev
```

Backend: `http://localhost:4000`
- REST health: `GET /api/health`
- REST API: `/api/auth`, `/api/courses`, `/api/assignments`
- GraphQL: `POST /graphql`
- External learning resource search: `GET /api/resources/search?q=javascript`

Register using `POST /api/auth/register` with `{ "name": "Priyank", "email": "priyank@example.com", "password": "ChangeMe123!" }`.

## 3. Start frontend

In another terminal:

```bash
cd frontend
npm install
npm run dev
```

Open `http://localhost:5173`.

## 4. Start AI service

In another terminal:

```bash
cd ai-service
python -m venv .venv
# Linux/macOS: source .venv/bin/activate
# Windows: .venv\\Scripts\\activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Open `http://localhost:8000/docs`. Upload a `.txt` or `.pdf` document to `/documents`, then ask questions at `/ask`. Retrieval uses a transparent token-overlap baseline so the service works locally without a paid API key. It is a learning MVP, not semantic/vector retrieval yet.

## Useful API examples

```bash
# Register
curl -X POST http://localhost:4000/api/auth/register \\
  -H 'Content-Type: application/json' \\
  -d '{"name":"Priyank","email":"priyank@example.com","password":"ChangeMe123!"}'

# Login
curl -X POST http://localhost:4000/api/auth/login \\
  -H 'Content-Type: application/json' \\
  -d '{"email":"priyank@example.com","password":"ChangeMe123!"}'

# List courses (public)
curl http://localhost:4000/api/courses
```

## Before publishing to GitHub

1. Change all secrets in `.env`; never commit `.env` files.
2. Add tests for authentication, course CRUD, GraphQL and AI retrieval.
3. Add role-based authorization and rate limiting.
4. Replace baseline keyword retrieval with embeddings + a vector store.
5. Add CI/CD, deployment manifests, screenshots, and a truthful project demo.
6. Only add technologies/features to your resume after you have run and understood them.
