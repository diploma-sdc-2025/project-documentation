# Criterion: Backend

## Architecture Decision Record

### Status

**Status:** Accepted  

**Date:** 2026-05-01 (revised 2026-05-04)

### Context

The diploma backend must show:

- **REST** HTTP APIs with clear boundaries  
- **Persistence** (PostgreSQL + Flyway) where each service owns its data  
- **Maintainability** (layered packages, tests)  
- Integration with **deployment**, **analytics**, and **automated testing** (including **JaCoCo ≥70%** on `analytics-service`)

The domain is a turn-based auto-battler flow: **authentication → matchmaking → game (shop/board) → battle evaluation → results/ratings → analytics**.

Constraints: solo developer, fixed timeline, stack centred on **Spring Boot**, **PostgreSQL**, **Redis**, **Docker**, **Azure VM–style** deployment.

### Decision

Use **multiple Spring Boot applications** (not a single monolith):

| Service | Responsibility |
|---------|----------------|
| **Auth** | Registration, login, guest session, JWT issuance, user/rating integration |
| **Matchmaking** | Redis-backed queue; pairs players; triggers match creation in Game |
| **Game** | Match lifecycle, shop/inventory/board/king, orchestrates battle rounds and rating callbacks |
| **Battle** | Stockfish-backed evaluation and battle simulation |
| **Analytics** | Event ingestion (e.g. Redis channel), persistence, leaderboard, **SSE** admin stream |

**Integration style:**

- **Synchronous HTTP (REST)** between browser and services, and between services where designed (e.g. game ↔ auth for ratings).  
- **Redis** for volatile structures (queue, game caches, analytics fan-in).  
- **Real-time toward browsers:** the **game loop** does **not** rely on a mandatory WebSocket channel — the SPA uses **REST** with **polling/resync** where needed; **admin analytics** uses **Server-Sent Events** from `analytics-service`. Optional STOMP/WebSocket helpers may exist for analytics but are **not** the primary player transport.

**Data:** PostgreSQL per service pattern with Flyway; **Redis** alongside for ephemeral state.

### Alternatives Considered

| Alternative | Pros | Cons | Why not chosen |
|-------------|------|------|----------------|
| Monolithic Spring Boot | Simpler deploy | Weak separation story for thesis | Rejected for clarity of domains |
| Shared single PostgreSQL schema for all | Easier joins | Couples services | Rejected |
| WebSockets for every game update | Push everywhere | More infra/reconnect logic for diploma scope | Not primary choice; REST + polling sufficient |

### Consequences

**Positive:** Clear ownership per service; battle CPU isolated; analytics separated.

**Negative:** More moving parts, ports, and env vars than one JAR.

**Neutral:** Cross-service calls must handle failures and consistent JWT/internal secrets.

## Implementation Details

### Typical package layout (per service)

```
src/main/java/.../
├── controller/     # REST controllers (@RestController)
├── service/        # Business logic
├── repository/     # Spring Data JPA where used
├── entity/         # JPA entities
├── dto/            # Request/response objects
├── config/         # Security, Redis, OpenAPI, etc.
├── exception/      # Global handlers where present
└── *Application.java
```

*(Exact packages vary by service.)*

### Key implementation decisions

| Decision | Rationale |
|----------|-----------|
| Spring Boot 3.x + Java 21 | Required stack; good test/monitoring ecosystem |
| REST as default integration | Browser and Postman-friendly; nginx-friendly |
| Redis for queues/caches | Latency and simplicity for matchmaking and hot keys |
| JWT | Stateless auth across services with shared signing configuration |
| Stockfish in Battle Service | Strong evaluation without building a chess engine |

### Illustrative patterns (not exhaustive)

**Auth — login returns tokens**

```java
@PostMapping("/login")
public ResponseEntity<AuthResponse> login(@Valid @RequestBody LoginRequest req) {
    return ResponseEntity.ok(auth.login(req));
}
```

**Matchmaking — authenticated queue join**

```java
@PostMapping("/join")
public ResponseEntity<QueueJoinResponse> joinQueue(Authentication authentication) {
    return ResponseEntity.ok(service.joinQueue(authentication));
}
```

**Game — match creation from matchmaking (internal/partner API)**

Match creation is invoked from matchmaking via HTTP to game-service; exact path and DTO names match your OpenAPI (`api-docs/reference/openapi.yaml`).

**Battle — evaluation driven by engine**

Battle endpoints accept FEN/state from game flows and delegate to **Stockfish** via `BattleService`; exact mappings follow `battle-service` controllers in the repo.

**Analytics — operator-facing metrics**

Live snapshots and **SSE** (`SseEmitter`) are exposed from **analytics-service**; ingestion often combines **Redis** listeners (`RedisSubscriber`) with persistence in **AnalyticsService** — see source rather than duplicating full Redis key layouts here (they evolve with implementation).

### Diagrams

![High level architecture](../../assets/diagrams/High-Level-Architecture.png)

![Use case diagram](../../assets/diagrams/use-case-diagram.png)

## Requirements checklist

| # | Requirement | Status | Evidence |
|---|-------------|--------|----------|
| 1 | REST APIs for core flows | ✅   | Controllers per service |
| 2 | Persistent storage | ✅   | PostgreSQL + Flyway |
| 3 | Separation of concerns | ✅ | Service + layer boundaries |
| 4 | Live behaviour | ✅ |  **SSE** for admin analytics |
| 5 | Testability | ✅ | `src/test/java` + JaCoCo where configured |

## Known limitations

| Topic | Note |
|--------|------|
| Distributed debugging | Issues span services — correlate logs and request IDs in demos. |

## References

- [Spring Boot](https://spring.io/projects/spring-boot)
- [PostgreSQL](https://www.postgresql.org/docs/)
- [Redis](https://redis.io/docs/)
- OpenAPI: `api-docs/reference/openapi.yaml` in the monorepo
