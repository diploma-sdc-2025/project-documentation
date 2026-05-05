# Technology Stack

## Stack Overview

| Layer | Technology | Version / notes | Justification |
| ----- | ---------- | --------------- | ------------- |
| **Client** | React + TypeScript + Vite | React 19, Vite 7 | SPA for matchmaking, shop/board, battle UI; `Vite` dev proxy for `/api/*` |
| **Client tests** | Vitest + Testing Library | Vitest 3.x | Unit/UI tests aligned with Vite |
| **Backend** | Java 21, Spring Boot | 3.5.x (BOM) | REST services, JPA, security, test support |
| **Game sync** | HTTP (REST) + polling in client | — | Shop/match resync and matchmaking status; |
| **Real-time (admin)** | Server-Sent Events (SSE) + Redis pub/sub | Spring `SseEmitter` | Live operator metrics; not the same as full-duplex WebSockets |
| **Database** | PostgreSQL + Flyway | Azure-compatible JDBC | Migrations per service; `ddl-auto=none` |
| **Cache / queue** | Redis | 6379 | Matchmaking queue; game/cache keys; analytics event channel |
| **Battle engine** | Stockfish (UCI) | configurable depth | Objective evaluation in Battle Service |
| **API contracts** | OpenAPI 3.1 YAML + springdoc (per service) | `api-docs/reference/openapi.yaml` | Unified reference + Swagger UI in development |
| **Reverse proxy** | nginx | TLS 443 | SPA + `/api/*`; SSE-friendly buffering settings |
| **Containers** | Docker (multi-stage Dockerfiles per service) | Eclipse Temurin 21 JRE | Reproducible JAR deployment |
| **Coverage gate** | JaCoCo | plugin on `analytics-service` | `mvn verify` enforces ≥70% bundle instructions |

## Key Technology Decisions

### Multiple Spring Boot services + PostgreSQL

**Decision:** Auth, Matchmaking, Game, Battle, Analytics as separate deployables, each with Flyway-managed schema where applicable.

**Rationale:** Clear boundaries for the diploma; independent scaling/restart of battle vs auth.

**Trade-off:** More integration work than a monolith; HTTP between services must stay consistent (timeouts, internal secrets).

### Redis for ephemeral state

**Decision:** Redis for matchmaking queue and hot game keys; analytics ingestion channel (`analytics:events`) for fan-out to SSE.

**Rationale:** Low latency; avoids over-writing Postgres on every tick.

### REST-first client + SSE for admin analytics

**Decision:** Player flows use **REST** (with intentional polling where push was not required); operators use **SSE** for live dashboards.

**Rationale:** Simpler to deploy behind nginx than upgrading every route to WebSockets; SSE fits server→browser push.

## Development Tools

| Tool | Purpose |
|------|---------|
| IntelliJ / VS Code | Backend / frontend |
| Maven | Java builds |
| npm | Frontend build |
| Git + GitHub | Version control |
