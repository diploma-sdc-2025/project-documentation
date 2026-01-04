# Technology Stack

## Stack Overview

| Layer | Technology | Version | Justification |
| ----- | ---------- | ------- | ------------- |
| **Frontend**| No implemented UI, Figma prototype| - | Scope focuses on backend architecture; UX is delivered via Figma wireframes and usability testing |
| **UI Framework**| N/A | - | No front-end implementation in this diploma project |
| **Backend** | Java | 21+| Modern LTS Java version; strong ecosystem for microservices and testing|
| **Framework** | Spring Boot (microservices)| 3.x (or latest stable used) | Rapid REST development, strong security/testing tooling, production-ready patterns |
| **Realtime**          | Spring WebSocket | latest stable | Enables server push updates (match/queue/battle events) |
| **Database**          | PostgreSQL (database-per-service)| 17+ | Relational consistency, mature tooling, fits JPA well; supports clear service ownership |
| **ORM** | Spring Data JPA (Hibernate)| bundled with Spring Boot    | Fast persistence layer development, entity mapping, repository pattern |
| **Cache / In-memory** | Redis | latest stable | Fast state storage for in-memory board matrix and matchmaking queue state |
| **Deployment** | Docker + Docker Compose + Azure App Service for Containers | - | Reproducible environments, simple local setup; Azure is required by diploma criteria |
| **API Documentation** | Swagger / OpenAPI (springdoc)| latest stable | Required for clear endpoint documentation and demo via Swagger UI |
| **Testing** | JUnit 5 + Mockito + JaCoCo | latest stable | Meets ≥70% coverage requirement; supports unit + integration style testing |
| **CI/CD** | GitHub Actions (might be done) | - | Automates build/test and improves reliability                                     |


## Key Technology Decisions

### Decision 1: Spring Boot Microservices + Database-per-Service

**Context:** The project must demonstrate backend complexity, clean architecture, and multiple evaluation criteria (REST API, DB design, deployment, testing, analytics).

**Decision:** Implement the backend as multiple Spring Boot microservices (Auth, Matchmaking, Game, Battle, Analytics) with a separate PostgreSQL database per service.

**Rationale:**
- Clear separation of responsibilities and easier reasoning about each domain area
- Matches real-world distributed systems patterns 
- Supports independent containerization and deployment

**Trade-offs:**
- Pros: scalable architecture, clean separation, complex
- Cons: harder to implement, unknown errors might occur

### Decision 2: Redis for Real-Time State (Game + Matchmaking)

**Context:** Some data is short-lived and performance-sensitive (board matrix, game state, matchmaking queue).

**Decision:** Use Redis namespaces to store:
- game:* for game:board:{matchId} and game:state:{matchId}
- queue:* for queue:active, queue:user:{userId}, queue:stats

**Rationale:**
- Very fast access for active match state and queue operations
- Avoids constant relational writes for frequently changing in-memory state
- Natural fit for queue structures (sorted sets) and ephemeral session data

**Trade-offs:**
- Pros: performance, simpler real-time operations, reduces DB write load
- Cons: harder to implement, need to sync with db

## Development Tools

| Tool | Purpose | Notes |
|------|---------|-------|
| **IDE** | VS Code, IntelliJ | VS Code for documentation; IntelliJ used for Spring Boot |
| **Version Control** | Git + GitHub | All in one main branch (separated repos) |
| **Build Tool** | | Maven or Gradle | Consistent builds across services |
| **Containerization** | Docker + Docker Compose | Local orchestration of services + dependencies|
| **Testing** | JUnit 5 + Mockito + JaCoCo | Coverage target: ≥70% |
| **API Testing** | Postman + Swagger UI | Swagger for documentation + quick manual testing; Postman for flows across services |
| **Documentation** | Markdown + Swagger/OpenAPI | Markdown for docs; Swagger for endpoint docs |

## External Services & APIs

| Service | Purpose | Pricing Model |
| --------| --------| --------------|
| **Azure App Service for Containers** | Deploy and host the containerized microservices | Free tier / Student account |
| **GitHub** | Source control + collaboration | Free tier   |
| **Stockfish** | Chess analysis / move generation used by Battle Service | Free / open source |
| **Figma** | UX wireframes and prototype | Free tier |
