# Features & Requirements

## Epics Overview

| Epic | Description | Stories | Status |
| ---- | ------------| ------- | ------ |
| **E1: Authentication** | Register/login users, issue JWT tokens, protect endpoints | 2 | ✅ |
| **E2: Matchmaking** | Queue players and create matches | 2 | ✅  |
| **E3: Game Management & Shop** | Match state, shop offers, purchases, gold economy, board management, upgrades | 2 | ⚠️  |
| **E4: Battle Simulation**  | Execute battles automatically and store results | 2 | ⚠️ |
| **E5: Analytics & Monitoring** | Actuator + Micrometer metrics for gameplay and health  | 2 | ⚠️ |
| **E6: UX Prototype (Figma)** | Wireframes | 1 | ✅  |
| **E7: Cloud Deployment & Containerization** | Dockerize and deploy services to Azure | 2 | ✅ |


## User Stories

### Epic 1: Authentication

User identity management and security for all services using JWT.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | ------------------- | -------- | ------ |
| **US-001** | As a **new player**, I want to **register**, so that I **can access multiplayer features.**  | - Valid email/username/password accepted<br>- Password hashed (BCrypt)<br>- User stored in PostgreSQL<br>- JWT returned on success | Must | ✅ |
| **US-002** | As a **registered player**, I want to **log in**, so that I **can securely use the system.** | - Valid credentials return JWT<br>- Invalid credentials return error<br>- Protected endpoints require JWT | Must | ✅ |


### Epic 2: Matchmaking

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | ------------------- | -------- | ------ |
| **US-003** | As a **player**, I want to **join matchmaking**, so that **I can be matched with an opponent.** | - Authenticated users can join queue<br>- Duplicate queue joins prevented<br>- Queue status retrievable | Must | ✅  |
| **US-004** | As the **matchmaking service**, I want to **create a match** when players are found, so that gameplay can begin. | - Two players are paired<br>- Game Service match created successfully<br>- Match ID returned | Must | ✅ |


### Epic 3: Game Management & Shop

Manages match state, shop offers, economy, board placement and upgrades.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | --------------------| -------- | ------ |
| **US-005** | As a **player**, I want to **view and purchase pieces**, so that I **can build my team.** | - Shop offers generated per round<br>- Purchase deducts gold<br>- Purchased piece saved to inventory<br>- Match state updated | Must | ⚠️ |
| **US-006** | As a **player**, I want to **upgrade pieces**, so that I can increase their strength. | - Valid combinations only<br>- Upgrade persists to match state<br>- Upgrade affects battle outcome | Should | ⚠️ |

### Epic 4: Battle Simulation

Runs automated battle and returns outcome.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | -----------| ------------------- | -------- | ------ |
| **US-007** | As a **player**, I want the game to  **trigger a battle**, so that **my team fights automatically.** | - Valid match/board state required<br>- Battle result computed<br>- Outcome stored in DB<br>- Game Service receives results | Must | ⚠️ |

### Epic 5: Analytics & Monitoring

Tracks system and gameplay metrics.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | ------------------- | -------- | ------ |
| **US-008** | As a **developer**, I want to **track metrics**, so that I **can monitor stability and gameplay behavior.** | - Actuator enabled for all services<br>- ≥5 custom gameplay metrics exposed<br>- `/actuator/health` works | Must | ⚠️ |

### Epic 6: UX Prototype (Figma)

UX deliverables for diploma requirement.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | ------------------- | -------- | ------ |
| **US-009** | As a **player**, I want a **clear UI flow prototype**, so that I **can understand how the game works.** | - Key screens designed (auth, matchmaking, shop, board, results) <br>- Iteration notes included | Must | ✅/⚠️ |

### Epic 7: Cloud Deployment & Containerization

Production-like packaging and deployment.

| ID | User Story | Acceptance Criteria | Priority | Status |
| -- | ---------- | ------------------- | -------- | ------ |
| **US-010** | As a **developer**, I want to **deploy services to Azure**, so that **the system is portable and demonstrable.** | - Dockerfiles for each service<br>- Docker Compose works locally<br>- Services reachable in Azure | Must| ✅/⚠️ |

## Use Case Diagram

[Use Case Diagram](assets/diagrams/use-case-diagram.png)

## Non-Functional Requirements

### Performance

| Requirement | Target | Measurement Method |
|-------------|--------|-------------------|
| API response time| <500ms average for game actions| Actuator metrics / logs|
| Concurrent users| 100 concurrent users(demo) | Simple load test |
| Battle calculation time | <2 seconds per battle (demo) | Battle service logs |

### Security

- JWT-based authentication (Bearer token)
- Password hashing using BCrypt
- HTTPS enforced in production (Azure)
- Input validation on all REST endpoints (DTO validation)
- Proper error responses (no sensitive data in error payloads)

### Accessibility

(UX prototype only – no implemented UI)
- Mobile-first layouts in Figma
- Clear labels and readable font sizes
- Contrast and spacing considered in wireframes

### Reliability

| Metric        | Target                           |
| ------------- | -------------------------------- |
| Uptime        | 99.5% (demo environment)         |
| Recovery time | <10 minutes (restart containers) |
| Data backup   | Not required  |


### Compatibility

| Platform/Client            | Minimum Version                 |
| -------------------------- | ------------------------------- |
| Postman                    | Latest                          |
| Swagger UI                 | Included with Springdoc/OpenAPI |
| Mobile UI                  | Prototype only (Figma)          |
