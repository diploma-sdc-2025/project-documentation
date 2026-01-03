# 2. Technical Implementation

This section covers the technical architecture, design decisions, and implementation details.

## Contents

- [Tech Stack](tech-stack.md)
- [Criteria Documentation](criteria/) - ADR for each evaluation criterion
- [Deployment](deployment.md)

## Solution Architecture

### High-Level Architecture

[High-Level Architecture](../assets/diagrams/High-Level-Architecture.png)

### System Components

| Component | Description | Technology |
| --------- | ----------- | ---------- |
| **Client (Interaction layer)** | API interaction via Swagger UI and Postman; UX flows shown in Figma | Swagger UI, Postman, Figma |
| **Auth Service** | Registration, login, JWT issuance/validation, user storage | Spring Boot, Spring Security, JWT, PostgreSQL |
| **Matchmaking Service** | Queue players, pair opponents, create match sessions | Spring Boot, PostgreSQL |
| **Game Service**| Core game state, shop, economy, board management, upgrades | Spring Boot, Spring Data JPA, PostgreSQL |
| **Battle Service** | Executes battle resolution; integrates with Stockfish for analysis/move selection | Spring Boot, Stockfish (UCI), PostgreSQL |
| **Analytics/Monitoring** | Health checks and gameplay/system metrics | Spring Boot Actuator, Micrometer |
| **Databases** | Database-per-service; isolated schemas per microservice | PostgreSQL × N |
| **Deployment**| Containerized services deployed to cloud | Docker, Docker Compose, Azure App Service |


### Data Flow

```
[Player Action via Postman/Swagger]
        │
        ▼
[Auth Service] → JWT issued
        │
        ▼
[Matchmaking Service] join queue
        │
        ▼
[Game Service] match created + initial state
        │
        ▼
[Game Service] shop/purchase/place/upgrade actions
        │
        ▼
[Battle Service] battle requested
        │
        ├─▶ [Stockfish] analyze position / choose moves (UCI)
        │
        ▼
[Battle result stored] → [Game state updated] → [Metrics emitted]
        │
        ▼
[Repeats until 1 player remains alive]
        │
        ▼

[Game ends] → [Final results shown]
```

## Key Technical Decisions

| Decision | Rationale | Alternatives Considered |
| -------- | --------- | ----------------------- |
| Microservices architecture (multiple Spring Boot services) | Demonstrates distributed systems skills; separation of concerns; aligns with diploma complexity goals | Monolith REST API (simpler but less effective)|
| Database-per-service (PostgreSQL per microservice) | Aligns with microservices best practice; isolates data ownership and schema changes | Shared database (simpler but couples services)|
| REST APIs + Swagger for primary interaction | Easy testing and documentation; fits diploma requirement; works without frontend | message queue (adds complexity) |
| WebSockets for real-time updates | Enables push-based updates for match/battle status;| Simple REST|
| Stockfish used by Battle Service | Provides strong chess analysis and move selection, avoids implementing a chess engine from scratch | Fully custom engine too complex|
| Docker + Azure App Service for Containers | Reproducible environments; mandatory diploma deployment requirement | AWS  |


## Security Overview

| Aspect | Implementation |
|--------|----------------|
| **Authentication** | JWT Bearer tokens issued by Auth Service (login/register) |
| **Authorization** | Role-based access at endpoint level |
| **Data Protection** | HTTPS in production (Azure); passwords hashed with BCrypt; no plain-text credentials stored |
| **Input Validation** | Spring validation annotations on DTOs; consistent error responses |
| **Secrets Management** | Environment variables for JWT secret, DB credentials;|
