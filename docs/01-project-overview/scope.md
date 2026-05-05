# Project Scope

## In Scope

| Feature | Description | Priority |
| ------- | ----------- | -------- |
| **Web client** | React SPA: auth, matchmaking, shop/board, battle flow, tutorial route | **Must** |
| User authentication | Register, login, guest where implemented; JWT | **Must** |
| Matchmaking | Queue and pairing into matches | **Must** |
| Core game logic | Shop, economy, placement, king, automated battles | **Must** |
| Multiple Spring Boot services | Auth, Matchmaking, Game, Battle, Analytics | **Must** |
| Persistence | PostgreSQL + Flyway per service patterns | **Must** |
| Redis | Queue + caches + analytics event bus | **Must** |
| API documentation | OpenAPI + Swagger/springdoc | **Must** |
| Containerization | Dockerfiles per service | **Must** |
| Cloud demo | **Azure VM** (typical): nginx, HTTPS, services — *document your actual topology* | **Must** |
| Analytics | Events, leaderboard, **live admin SSE**; optional Actuator health per service | **Must** |
| UX | Implemented UI + Figma reference | **Must** |

## Out of Scope

| Feature | Reason |
| -------- | ------ |
| Native mobile apps | Web-first diploma scope |
| Full tournament / esports systems | Beyond time box |
| Social graph, chat, monetisation | Not required academically |
| Heavy ML analytics | Optional future |

## Assumptions & Constraints

### Assumptions

| # | Assumption |
|---|--------------|
| A1 | Players use a **modern browser** and stable internet; no unsupported legacy browsers. |
| A2 | **PostgreSQL** and **Redis** are reachable from all services that need them (same VPC/network or configured firewall). |
| A3 | **JWT signing secret** and DB credentials are provided via environment/config — same secret wherever tokens are validated. |
| A4 | **Stockfish** (or equivalent) is available to **Battle Service** in each runtime you deploy. |
| A5 | Demo traffic is **low** (committee, testers); no global-scale load or HA cluster is implied. |

### Constraints

| # | Constraint | Impact |
|---|------------|--------|
| C1 | **Single developer / thesis deadline** | Scope must stay cut to demonstrable vertical slices. |
| C2 | **One main demo environment** (e.g. one Azure VM + domain) | No multi-region failover or 24/7 SLO. |
| C3 | **Budget** | Free/student tiers and open-source tools where possible. |
