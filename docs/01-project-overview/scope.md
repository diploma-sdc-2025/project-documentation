# Project Scope

## In Scope ✅

| Feature| Description| Priority |
| -------| -----------| -------- |
| User Authentication | User registration, login, JWT-based authentication and authorization | **Must** |
| Matchmaking System | Queue management and pairing players into matches | **Must** |
| Core Game Logic| Match state management, shop mechanics, gold economy, piece placement| **Must** |
| Automated Battle System | Server-side battle execution and result calculation | **Must** |
| Microservices Architecture | Separation into Auth, Game, Battle, Matchmaking, and Analytics services | **Must** |
| Database per Service | Independent PostgreSQL database for each microservice | **Must**   |
| API Documentation | Swagger/OpenAPI documentation for all services | **Must** |
| Containerization | Dockerized services with Docker Compose for local development | **Must** |
| Cloud Deployment | Deployment of containers to Azure App Service | **Must** |
| Analytics & Monitoring | Spring Boot Actuator and Micrometer metrics | **Must** |
| UX Design (Prototype)  | Figma wireframes | **Must** |


## Out of Scope ❌

| Feature | Reason | When Possible |
| --------| -------| ------------- |
| Front-End Application(Swift/ Kotlin) | Focus is backend architecture and UX design only | After diploma defense |
| Ranked / Competitive Mode (ELO/MMR) | Requires advanced balancing and long-term data | After publishing to store |
| AI Single-Player Mode | Adds significant complexity to game logic | One of the future updates (when in store)|
| Social Features (Chat, Friends)     | Not required for academic goals | Future update|
| Monetization / Payments | Not relevant for diploma objectives  | Future update |
| Advanced Analytics / ML | Beyond required analytics scope | Future update |


## Assumptions

| # | Assumption | Impact if Wrong | Probability |
| - | -----------| ----------------| ----------- |
| 1 | Users are familiar with basic chess rules | UX may require more onboarding | Medium |
| 2 | Azure free/student tier is sufficient for demo deployment | Deployment issues or service limits | Medium |
| 3 | Simplified auto-battle rules provide engaging gameplay | Game may feel shallow | Medium|

## Constraints

Limitations that affect the project:

| Constraint Type | Description | Mitigation |
| --------------- | ----------- | ---------- |
| **Time** | Fixed diploma deadline (10–12 weeks)| Strict scope control, prioritizing Must-have features |
| **Budget** | €0 budget; free tiers only | Use open-source tools and Azure free/student plan     |
| **Technology** | Mandatory use of Spring Boot, PostgreSQL, Docker, Azure | Early architecture decisions and tutorials |
| **Resources** | Solo developer | Simplified feature set, realistic planning |
| **Knowledge** | Limited prior experience with game logic and cloud deployment | Learning time |
| **Infrastructure** | Single-region deployment, no high availability requirement | Designed for demo-scale usage |


## Dependencies

| Dependency        | Type      | Owner                | Status |
| ----------------- | --------- | -------------------- | ------ |
| GitHub Repository | External  | GitHub               | ✅ |
| Azure App Service | External  | Microsoft Azure      | ✅ |
| PostgreSQL        | Technical | PostgreSQL Community | ✅ |
| Spring Boot       | Technical | Spring / VMware      | ✅ |
| Docker            | Technical | Docker Inc.          | ✅ |
| Swagger / OpenAPI | Technical | OpenAPI Initiative   | ✅ |
| Figma             | External  | Figma Inc.           | ✅ |
| Stockfish     	| Technical | Open source          | ✅ |
