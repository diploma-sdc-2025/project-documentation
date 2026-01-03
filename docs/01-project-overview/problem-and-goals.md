# Problem Statement & Goals

## Context

[AutoChess Classic is a diploma project that reimagines traditional chess as a short-session strategy auto-battler. Instead of manual move-by-move play, the player focuses on drafting pieces, managing economy, positioning on an 8×8 board, and upgrading units, while battles execute automatically.
The system is implemented as a Spring Boot microservices backend (Auth, Game, Battle, Matchmaking), each with its own PostgreSQL database, containerized with Docker, deployed to Azure, documented via Swagger, monitored using Spring Boot Actuator + Micrometer metrics, and validated by automated tests with ≥70% coverage. UX is delivered as Figma wireframes(no full front-end implementation).]

## Problem Statement

**Who:** [Chess-aware players who want strategic depth without memorizing openings]
[Auto-battler fans who want familiar, iconic units rather than learning new rosters]
[Mobile/short-session players who prefer 5–10 minute matches]

**What:** [Traditional chess has barriers for casual engagement (high skill floor, long matches, punishing mistakes, low variety), while popular auto-battlers lack the universally understood mechanics and units of chess. There is no clear bridge between these audiences.]

**Why:** [This limits chess accessibility for casual/mobile play and leaves a market gap for a strategy game that is both familiar and replayable. For the diploma, it also provides a suitable domain to demonstrate backend architecture skills, persistence, deployment, analytics, documentation, and testing.]

### Pain Points

| # | Pain Point | Severity | Current Workaround |
|---|------------|----------|-------------------|
| 1 | [High skill floor in classical chess (openings/endgames/tactics memorization)] | High | [Players avoid chess or play only simplified modes] |
| 2 | [Long time commitment (30–60 minutes typical)] | High | [Players choose faster casual games instead] |
| 3 | [Punishing mistakes and pressure of perfect play] | Medium | [Players play against weaker opponents or stop playing] |
| 4 | [Limited variety (same pieces, same starting state, predictable early game)] | Medium | [Players seek variants or different strategy games] |


## Business Goals

| Goal | Description | Success Indicator |
|------|-------------|-------------------|
| [Academic achievement] | [Demonstrate required diploma complexity (backend, UX, DB, analytics, Docker, API docs, testing)] | [All criteria demonstrated in documentation + successful defense] |
| [Technical excellence] | [Implement a clean microservices REST backend with game logic and core services] | [Working multi-service system with stable endpoints] |
| [Functional completeness] | [Deliver a playable multiplayer flow via APIs (auth → matchmaking → shop/board → battle → results)] | [End-to-end scenario demonstrable through Swagger/Postman] |

## Objectives & Metrics

| Objective | Metric | Current Value | Target Value | Timeline |
|-----------|--------|---------------|--------------|----------|
| [Provide fully functional and working backend behavior] | [End-to-end flow works + API response time] | [All done] | [<500ms average for main game actions] | [Before defense] |
| [Meet diploma testing requirements] | [Automated test coverage] | [All done] | [≥70% line coverage (JaCoCo)] | [Before defense] |
| [Ensure containerized deployment] | [Services running as Docker containers in cloud] | [Not deployed] | [5 services deployed on Azure App Service for Containers] | [Before defense] |
| [Provide complete API documentation] | [Swagger coverage] | [All done] | [100% endpoint documentation (5 Swagger UIs)] | [Before defense] |
| [Deliver analytics/monitoring] | [Actuator + custom gameplay metrics] | [Partially done] | [≥5 gameplay metrics exposed as JSON across services] [Before defense] |
| [Validate UX quality] | [Usability tests on Figma prototype] | []

## Success Criteria

### Must Have

- [Done] [Authentication works end-to-end: users can register/login, receive JWT, and access protected endpoints]
- [Done] [Matchmaking works: authenticated players can join queue and matches are created successfully.]
- [Partially Done] [Core game loop works via APIs: shop offers pieces, purchase consumes gold, board placement is persisted and match state is retrievable.]
- [Partially Done] [Battle execution works: Game Service triggers Battle Service, battle result stored and returned, win/loss recorded.]
- [Done] [Microservices architecture demonstrated: 5 Spring Boot services, REST communication, database-per-service.]
- [Done] [Containerization + cloud deployment: all services Dockerized and deployed to Azure.]
- [Done] [Documentation + testing compliance: Swagger docs for all services + ≥70% test coverage with report.]

### Nice to Have

- [ ] [Unified analytics view: clearer aggregation of metrics across services (beyond basic endpoints).]
- [ ] [Improved balancing insights: additional gameplay metrics (e.g., win rates by piece type) and simple reporting.]

## Non-Goals

What this project explicitly does NOT aim to achieve:

- [Full web/mobile front-end implementation (React/Angular/Vue/native apps)]
- [Ranked mode, ELO/MMR, leaderboards, tournaments]
- [Social features (friends list, chat), cosmetics, monetization, progression systems]
- [Advanced analytics (ML/predictive modeling), alerting systems]  
- [AI single-player opponent, spectator mode, replays]
