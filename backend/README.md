# Backend Introduction

A Spring Boot backend application for a RAG (Retrieval-Augmented Generation) based intelligent Q&A system. This system enables users to upload documents and engage in intelligent conversations based on the document content using OpenAI's GPT models and vector embeddings.

Local full-stack demo: run the root `docker-compose.yml` and open `http://localhost:8081`.

## Table of Contents

- [Technology Stack](#technology-stack)
- [System Architecture & Features](#system-architecture--features)
- [Local Development](#local-development)
- [Docker Deployment](#docker-deployment)

## Technology Stack

### Core Framework
- **Java 17** - Programming language
- **Spring Boot 3.3.3** - Application framework
- **Spring Security** - Authentication and authorization
- **MyBatis 3.0.4** - ORM framework (SSM architecture)

### Database
- **MySQL 8.4.0** - Relational database
- **Flyway** - Database migration tool

### AI & ML
- **LangChain4j 0.33.0** - Java framework for LLM applications
- **OpenAI API** - GPT-4o-mini for chat, text-embedding-3-small for embeddings
- **InMemoryEmbeddingStore** - Vector storage for document embeddings

### Security
- **JWT (jjwt 0.11.5)** - Token-based authentication

### Document Processing
- **Apache PDFBox** - PDF document parsing
- **Apache POI 5.2.5 & 4.1.2** - Word document parsing (.doc, .docx)

### Build Tools
- **Maven** - Dependency management and build tool

## System Architecture & Features

### Architecture Overview

The system follows a layered architecture:

```
Controller Layer (REST API)
    ↓
Service Layer (Business Logic)
    ↓
Mapper Layer (Data Access)
    ↓
Database (MySQL)
```

### Core Features

#### 1. User Authentication & Authorization
- User registration with role-based access (Admin/User)
- JWT token-based authentication
- Password encryption using BCrypt
- Token expiration and refresh handling

#### 2. Document Management
- **Multi-format Support**: PDF, TXT, MD, HTML, DOC, DOCX
- **Document Parsing**: Automatic text extraction from various formats
- **Vector Indexing**: Documents are split into chunks and converted to embeddings
- **Document Storage**: File upload and metadata management
- **Index Rebuilding**: Automatic re-indexing on application startup

#### 3. RAG (Retrieval-Augmented Generation) System
- **Semantic Search**: Vector similarity search using OpenAI embeddings
- **Context Retrieval**: Top-K retrieval (K=4) of relevant document chunks
- **Intelligent Q&A**: GPT-4o-mini generates answers based on retrieved context
- **Reference Tracking**: Tracks and displays source documents for each answer
- **Context Awareness**: Maintains conversation history (last 10 messages)

#### 4. Chat System
- **Multi-session Management**: Users can create multiple chat sessions
- **Message History**: Persistent storage of chat messages
- **Streaming Response**: Server-Sent Events (SSE) for real-time token streaming
- **Non-streaming Support**: Traditional request-response mode

### API Endpoints

#### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login

#### Health
- `GET /api/health` - Service health check

#### Document Management (Admin only)
- `POST /api/docs/upload` - Upload document
- `GET /api/docs` - List all documents
- `GET /api/docs/{id}` - Get document metadata
- `GET /api/docs/{id}/content` - Preview parsed document content
- `DELETE /api/docs/{id}` - Delete document

#### Chat
- `POST /api/chat/create` - Create new chat session
- `GET /api/chat/list` - List user's chat sessions
- `GET /api/chat/{chatId}/history` - Get chat history
- `POST /api/chat/{chatId}/send` - Send message (non-streaming)
- `POST /api/chat/{chatId}/stream` - Send message (streaming, SSE)

## Local Development

### Prerequisites

- Java 17 or higher
- Maven 3.6+
- MySQL 8.0+
- OpenAI API Key

### Setup Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd capstone
   ```

2. **Configure MySQL Database**
   
   Create a MySQL database:
   ```sql
   CREATE DATABASE rag_chat CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
   ```
   
   Configure database credentials through environment variables:
   ```bash
   export SPRING_DATASOURCE_URL='jdbc:mysql://localhost:3306/rag_chat?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=Asia/Shanghai&characterEncoding=utf8'
   export SPRING_DATASOURCE_USERNAME='root'
   read -s SPRING_DATASOURCE_PASSWORD
   export SPRING_DATASOURCE_PASSWORD
   ```

3. **Set Environment Variables**
   
   Set your OpenAI API key:
   ```bash
   read -s OPENAI_API_KEY
   export OPENAI_API_KEY
   ```
   
   Keep the key out of source control. The application reads `OPENAI_API_KEY` at startup.

4. **Configure File Directories**
   
   Configure runtime directories if you do not want to use the defaults:
   ```bash
   export APP_RAG_INDEX_DIR='./data/vector-index'
   export APP_RAG_UPLOAD_DIR='./data/uploads'
   ```

5. **Build the Project**
   ```bash
   mvn clean install
   ```

6. **Run the Application**
   ```bash
   mvn spring-boot:run
   ```
   
   Or run the JAR file:
   ```bash
   java -jar target/rag-chat-backend-0.0.1-SNAPSHOT.jar
   ```

7. **Verify the Application**
   
   The application will start on `http://localhost:8080`
   
   Check if the API is running:
   ```bash
   curl http://localhost:8080/api/health
   ```

### Database Migration

Flyway will automatically run database migrations on application startup. Migration scripts are located in `src/main/resources/db/migration/`.

### Development Notes

- The application uses Flyway for database schema management
- Document indexing happens automatically on upload
- Vector index is rebuilt on application startup via `RagBootstrap`
- Logs are configured in `logback-spring.xml` and output to `logs/app.log`

## Docker Deployment

For Docker deployment instructions, use the root-level [README.md](../README.md) and [docker-compose.yml](../docker-compose.yml).

The deployment guide includes:
- Docker and Docker Compose setup
- Environment configuration
- Service orchestration
- Data persistence
- Logging and monitoring
- Troubleshooting

### Quick Docker Deployment

1. Create `.env` file with required environment variables
2. Build and start services:
   ```bash
   docker compose up -d --build
   ```
3. Check service status:
   ```bash
   docker compose ps
   ```

For detailed deployment steps, see the root-level [README.md](../README.md).

---

## License

This project is part of a capstone project for DSS5105.
