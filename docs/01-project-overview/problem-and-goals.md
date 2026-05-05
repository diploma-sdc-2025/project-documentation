# Problem Statement & Goals

## Context

AutoChess Classic is a diploma project that reimagines traditional chess as a short-session strategy auto-battler. Instead of manual move-by-move play, the player focuses on drafting pieces, managing economy, and positioning on an 8×8 board, while battles execute automatically.
The system is implemented as **Spring Boot** services (Auth, Matchmaking, Game, Battle, **Analytics**), with **PostgreSQL** and **Flyway**, **Redis** for queue/cache, a **React + TypeScript** web client, **OpenAPI** documentation, **admin real-time metrics (SSE)**, and **automated tests** (including a **≥70%** JaCoCo gate on `analytics-service`). **Figma** supports UX;

## Problem Statement

**Who:** 
- Chess-aware players who want strategic depth without memorizing openings
- Auto-battler fans who want familiar, iconic units rather than learning new rosters
- Mobile/short-session players who prefer 5–10 minute matches

**What:** Traditional chess has barriers for casual engagement (high skill floor, long matches, punishing mistakes, low variety), while popular auto-battlers lack the universally understood mechanics and units of chess. There is no clear bridge between these audiences.

**Why:** This limits chess accessibility for casual play and leaves a market gap for a strategy game that is both familiar and replayable. For the diploma, it also provides a suitable domain to demonstrate backend architecture skills, persistence, deployment, analytics, documentation, and testing.

### Pain Points

| # | Pain Point | Severity | Current Workaround |
|---|------------|----------|-------------------|
| 1 | High skill floor in classical chess (openings/endgames/tactics memorization) | High | Players avoid chess or play only simplified modes |
| 2 | Long time commitment (30–60 minutes typical) | High | Players choose faster casual games instead |
| 3 | Punishing mistakes and pressure of perfect play | Medium | Players play against weaker opponents or stop playing |
| 4 | Limited variety (same pieces, same starting state, predictable early game) | Medium | Players seek variants or different strategy games |


## Business Goals

| Goal | Description | Success Indicator |
|------|-------------|-------------------|
| Academic achievement | Demonstrate required diploma complexity (backend, UX, DB, analytics, Docker, API docs, testing) | All criteria demonstrated in documentation |
| Technical excellence | Implement a clean microservices REST backend with game logic and core services | Working multi-service system with stable endpoints |
| Functional completeness | Deliver a playable multiplayer flow (auth → matchmaking → shop/board → battle → results), including the web UI | End-to-end scenario demonstrable in the browser and via APIs |

## Objectives & Metrics

| Objective | Metric | Current Value | Target Value | Timeline |
|-----------|--------|---------------|--------------|----------|
| Provide fully functional and working backend behavior | End-to-end flow works + API response time | All Done | <500ms average for main game actions | Before defense |
| Meet diploma testing requirements | Automated test coverage | All done | ≥70% line coverage (JaCoCo) | Before defense |
| Ensure containerized deployment | Dockerfiles + runnable stack on **Azure VM** (nginx, HTTPS, services) | In progress / demo-ready | Stable demo URL + reproducible runbook | Before defense |
| Provide complete API documentation | OpenAPI + Swagger where enabled | Unified YAML in repo + per-service springdoc in dev | Same contracts referenced by SPA | Before defense |
| Deliver analytics | Gameplay events, leaderboard, **live admin stream (SSE)** + Actuator health | Implemented | Metrics visible in analytics service + optional actuator health per service | Before defense |

## Success Criteria

### Must Have

- [Done] Authentication works end-to-end: users can register/login, receive JWT, and access protected endpoints
- [Done] Matchmaking works: authenticated players can join queue and matches are created successfully.
- [Done] Core game loop works via APIs: shop offers pieces, purchase consumes gold, board placement is persisted and match state is retrievable.
- [Done] Battle execution works: Game Service triggers Battle Service, battle result stored and returned, win/loss recorded.
- [Done] Microservices architecture demonstrated: 5 Spring Boot services, REST communication, database-per-service.
- [Done] Containerization + cloud deployment: all services Dockerized and deployed to Azure.
- [Done] Documentation + testing compliance: OpenAPI/Swagger + ≥70% JaCoCo gate where configured (`analytics-service`).

### Nice to Have

- Unified analytics view: clearer aggregation of metrics across services (beyond basic endpoints).
- Improved balancing insights: additional gameplay metrics (e.g., win rates by piece type).

## Non-Goals

What this project explicitly does **not** aim to achieve (beyond the diploma scope):

- Native **mobile apps** (iOS/Android) — web-first only
- Full **tournament** infrastructure, seasons, or commercial **matchmaking** at scale
- **Social** layer (friends list, in-game chat), cosmetics shop, monetisation
- **ML**/predictive analytics pipelines or full **observability** stacks (Datadog-class)
- **Spectator mode**, full **replay** export, or **AI opponent** as a product pillar

*Note: **Ratings** and a **leaderboard** exist so the game feels rewarding
