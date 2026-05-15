# Housing Rental AI Assistant

This repository preserves a team capstone project for a document-grounded rental consultation assistant. The system lets users upload rental documents, policies, house rules, or other knowledge-base files, then ask questions and receive answers grounded in retrieved source context.

The repository combines the original frontend and backend code into one reviewable project so the product flow, API design, RAG logic, and deployment path can be inspected together.

## What This Project Shows

- A practical RAG workflow: document parsing, chunking, embeddings, retrieval, answer generation, and source references.
- A full user flow: authentication, role-based document management, chat sessions, streaming responses, and bilingual UI.
- Backend persistence for users, documents, chat sessions, messages, and document metadata in MySQL.
- Engineering structure with Spring Boot, React, Flyway migrations, Docker Compose, and Nginx reverse proxying.
- Clear project boundary: this is a team course project and should not be presented as a single-person production system.

## Architecture

```mermaid
flowchart LR
  U[\"User\"] --> F[\"React + TypeScript frontend\"]
  F -->|REST / SSE| B[\"Spring Boot backend\"]
  B --> A[\"JWT auth + role control\"]
  B --> D[\"Document parser<br/>PDFBox + Apache POI\"]
  D --> E[\"OpenAI embeddings\"]
  E --> V[\"In-memory vector store\"]
  B --> R[\"RAG retrieval + GPT answer\"]
  B --> M[\"MySQL<br/>users, documents, chats, messages\"]
```

## Tech Stack

| Layer | Main Tools |
| --- | --- |
| Frontend | React 19, TypeScript, Vite, Tailwind CSS, React Router, Axios, React Markdown, i18next |
| Backend | Java 17, Spring Boot 3.3, Spring Security, MyBatis, Flyway |
| AI/RAG | LangChain4j, OpenAI GPT-4o-mini, text-embedding-3-small, in-memory embedding store |
| Data | MySQL 8.x |
| Deployment | Docker Compose, Nginx reverse proxy |

## Repository Layout

```text
.
|-- backend/              # Spring Boot RAG API
|-- frontend/             # React/Vite web app
|-- docs/                 # Course deliverables and project evidence
|-- docker-compose.yml    # Local full-stack deployment
`-- .env.example          # Environment template with blank secrets
```

## Main API Surface

| Area | Endpoint |
| --- | --- |
| Health | `GET /api/health` |
| Auth | `POST /api/auth/register`, `POST /api/auth/login` |
| Documents | `POST /api/docs/upload`, `GET /api/docs`, `GET /api/docs/{id}`, `GET /api/docs/{id}/content`, `DELETE /api/docs/{id}` |
| Chat | `POST /api/chat/create`, `GET /api/chat/list`, `GET /api/chat/{chatId}/history`, `POST /api/chat/{chatId}/send`, `POST /api/chat/{chatId}/stream` |

## Run Locally With Docker

Prerequisites: Docker, Docker Compose, and an OpenAI API key.

```bash
cp .env.example .env
# Fill OPENAI_API_KEY, database passwords, and JWT secret in .env.

cd frontend
npm ci
npm run build

cd ..
docker compose up -d --build
```

Open the app:

```text
http://localhost:8081
```

Backend health check:

```bash
curl http://localhost:8080/api/health
```

## Demo Flow

1. Register an admin account.
2. Upload a rental policy, tenancy agreement, or house-rules document.
3. Create a chat session.
4. Ask contract, deposit, repair, or policy questions.
5. Review the streamed answer and referenced source documents.

## Project Evidence

- Original backend repository: <https://github.com/M-Downey/5105-Capstone-backend>
- Original frontend repository: <https://github.com/M-Downey/5105-Capstone-frontend>
- Course deliverables are preserved under `docs/`.

## Scope and Limits

- The current vector store is in memory. Uploaded files are re-indexed on backend startup from the configured upload directory.
- A production deployment should use a persistent vector database and stricter document-level permissions.
- GitHub Pages can host only the static frontend. The full app needs a backend, MySQL, and OpenAI API access.
- Do not commit `.env` or any API keys.

## Interview Angle

For finance, risk, or fintech roles, this project is best used to show product thinking, document-grounded AI workflow design, source traceability, and system-level understanding. It should not be framed as a pure software-engineering role claim.
