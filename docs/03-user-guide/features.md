# Feature Walkthrough

## Feature 1: User Authentication & Authorization

### Overview

This feature provides secure user authentication using JSON Web Tokens (JWT).
It allows users to register, log in, refresh tokens, and log out securely across all backend services.

### How to Use

**Step 1:** Register a user
- Endpoint: POST /api/auth/register
- Provide user credentials (username, email, password)

**Step 2:** Log in
- Endpoint: POST /api/auth/login
- Receive an access token and refresh token

**Step 3:** Authorize requests
- add as header (e.g in postman) : Authorization: Bearer <accessToken>

**Expected Result:** 
The user is authenticated and can access protected endpoints in other services.

### Tips

- Tokens expire; use the refresh endpoint when needed
- Swagger UI provides built-in authorization support

---

## Feature 2: Matchmaking System

### Overview

The matchmaking service pairs players together if in queue.
Redis is used to ensure fast matching and low latency.

### How to Use

![Screenshot: `![Feature 2](../assets/images/feature-2.png)`]

**Step 1:** Authenticate using the Auth Service

**Step 2:** Send a queue join request
- Endpoint: POST /api/matchmaking/join
- Requires valid JWT token

**Step 3:** : Retrieve match result
- Endpoint: GET /api/matchmaking/status

**Expected Result:** 
The user is matched with another player and assigned to a game session.
---

## Feature 3: Game State Management

### Overview

The game service manages game sessions, player resources, and match state persistence.

### How to Use

**Step 1:** Authenticate and obtain JWT

**Step 2:** Create or join a game session
- Endpoint: POST /api/game/create-match

**Step 3:** Query game state
- Endpoint: GET /api/game/matches/{matchId}/state


**Expected Result:** 

Game state is stored and retrievable from the database.

---

## Feature 4: Battle Simulation 

### Overview

The battle service handles combat logic between players and determines battle outcomes.

### How to Use

**Step 1:** Authenticate

**Step 2:** Create or join a game session
- Endpoint: POST /api/game/create-match

**Step 3:** Trigger battle execution
- Endpoint: POST /api/battle/simulate

**Expected Result:** 
Battle results are calculated and health and etc are changed



## Feature 5: Battle Simulation 

### Overview

The analytics service collects gameplay events and provides aggregated statistics.
It supports both REST and WebSocket communication.

### How to Use

**Step 1:** Authenticate with the admin login and password

**Step 2:** Retrieve analytics data
- Endpoint: GET /api/analytics/live

**Expected Result:** 
Aggregated statistics and insights are returned. (an html page with a dashboard is possible)

---

## Feature Comparison

| Feature                | Available |
| ---------------------- | --------- |
| User Authentication    | ✅         |
| Matchmaking            | ✅         |
| Game State Management  | ✅         |
| Battle Simulation      | ✅         |
| Analytics & Statistics | ✅         |
