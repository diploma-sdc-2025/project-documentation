# Criterion: Backend

## Architecture Decision Record

### Status

**Status:** Accepted

**Date:** 01-03-2026

### Context

The diploma project requires a backend system demonstrating:
- RESTful API design
- Clear separation of concerns
- Persistent data storage
- Maintainability
- Integration with deployment, analytics, and testing criteria
The system must support a turn-based auto-battler game flow including authentication, matchmaking, game state management, battle execution, and analytics.
Constraints include:
- Solo developer
- Limited time
- Knowledge of Spring Boot, PostgreSQL, Docker, and Azure
- ≥70% automated test coverage

### Decision

A microservices-based backend architecture was implemented using Spring Boot, where each core part is represented by an independent service with its own database:
Auth Service - user registration, login, JWT handling
Matchmaking Service - queue management and match creation
Game Service - match state, shop mechanics, economy, and board state
Battle Service - battle execution and outcome calculation (Stockfish integration)
Analytics Service - gameplay events and service health metrics
Services communicate using REST APIs for commands and WebSockets for real-time updates.
Each service owns its data using a database-per-service pattern with PostgreSQL, and Redis is used for in-memory operations.

### Alternatives Considered

| Alternative | Pros | Cons | Why Not Chosen |
|-------------|------|------|----------------|
| Monolithic Spring Boot API| Simple to build and deploy| Poor separation of concerns, less depth| Does not demonstrate backend complexity|
| Shared Database Across Services | Easier queries and joins| schema conflicts, not scalable | Violates microservices principles |
| Using NoSql Db | Alternative to postgres | Not suitable to the project | Never used before, harder to use |

### Consequences

**Positive:**
- Clear separation of responsibilities and services ownership
- Real-world backend architecture patterns
- Independent testing, containerization, and deployment per service

**Negative:**
- Higher complexity in service coordination
- Requires careful API and error-handling design

**Neutral:**
- Increased boilerplate code compared to monolith (in general harder to make then monolith)
- Need knowledge of azure deployement

## Implementation Details

### Project Structure

Each microservice follows the following
 structure:
```
src/
├── controller/        # REST & WebSocket endpoints
├── service/           # Business logic
├── repository/        # Data access (Spring Data JPA)
├── entity/            # JPA entities
├── dto/               # Request/response DTOs
├── config/            # Security, WebSocket, Redis, Swagger config
├── exception/         # Centralized error handling
└── Application.java   # Service entry point

Each service is packaged and deployed independently as a Docker container.

```

### Key Implementation Decisions

| Decision | Rationale |
|----------|-----------|
| Spring Boot for all services| Fast development, strong testing and security support |
| Database-per-service (PostgreSQL) | Ensures loose coupling and data ownership for each service |
| Redis for game state & queues| High performance for frequently updated, transient data |
| REST for commands, WebSockets for updates| Separation between actions and real-time notifications|
| JWT-based authentication | Stateless, scalable, and suitable for distributed services|

### Code Examples

**Auth-service**
```
    // Login endpoint for user authentication.
    // Validates credentials and returns a JWT-based AuthResponse.
    // Demonstrates REST endpoint, validation, and service delegation.

    @PostMapping("/login")
    public ResponseEntity<AuthResponse> login(
            @Valid @RequestBody LoginRequest req) {
        return ResponseEntity.ok(auth.login(req));
    }
```

**Matchmaking service**
```
    // Allows an authenticated player to join the matchmaking queue.
    // The user identity is extracted from the JWT via Spring Security's Authentication object.
    // Returns a response indicating whether the player was added to the queue or was already waiting.

    @PostMapping("/join")
    public ResponseEntity<QueueJoinResponse> joinQueue(
            Authentication authentication) {

        return ResponseEntity.ok(
                service.joinQueue(authentication)
        );
    }
```

**Game-service**
```
    // Endpoint called by the Matchmaking Service when a match is formed.
    // Creates a new match instance and initializes game state.
    // Demonstrates inter-service REST communication.

    @PostMapping("/matches")
    public MatchResponse createMatch(@Valid @RequestBody CreateMatchRequest req) {
        return game.createMatch(req);
    }
```

**Battle-service**
```
    // Executes an automated battle simulation.
    // Called by the Game Service once players are ready.
    // Calls stockfish, which calculates the outcome

    @PostMapping("/simulate")
    public BattleResultResponse simulate(@Valid @RequestBody BattleRequest req) {
        return battle.simulateBattle(req);
    }
```

**Analytics-service**
```
    // Exposes live gameplay and system metrics aggregated from Redis.
    // Demonstrates real-time analytics and monitoring without impacting core gameplay.
    // Metrics include queue size, event counts, and last update timestamp.
    @GetMapping
    public Map<String, Object> getLiveMetrics() {
        Map<String, Object> metrics = new HashMap<>();

        Object queueSize = redisTemplate.opsForValue().get("analytics:current_queue_size");
        metrics.put("currentQueueSize", queueSize != null ? queueSize : 0);

        Object totalJoins = redisTemplate.opsForValue().get("analytics:total_queue_joins");
        metrics.put("totalQueueJoins", totalJoins != null ? totalJoins : 0);

        Long eventsLastMinute = redisTemplate.opsForZSet().zCard("analytics:events:last_minute");
        metrics.put("eventsLastMinute", eventsLastMinute != null ? eventsLastMinute : 0);

        Object lastUpdated = redisTemplate.opsForValue().get("analytics:last_updated");
        metrics.put("lastUpdated", lastUpdated);

        Object playerJoins = redisTemplate.opsForValue().get("analytics:event_type:player_join");
        Object playerLeaves = redisTemplate.opsForValue().get("analytics:event_type:player_leave");

        Map<String, Object> eventsByType = new HashMap<>();
        eventsByType.put("player_join", playerJoins != null ? playerJoins : 0);
        eventsByType.put("player_leave", playerLeaves != null ? playerLeaves : 0);
        metrics.put("eventsByType", eventsByType);

        return metrics;
    }
```


### Diagrams

[High level architecture](../assets/diagrams/High-Level-Architecture.png)

[Use case diagram](../assets/diagrams/use-case-diagram.png)
                        

## Requirements Checklist

| # | Requirement                  | Status | Evidence / Notes                        |
| - | ---------------------------- | ------ | --------------------------------------- |
| 1 | RESTful backend API          | ✅      | Controllers implemented in all services |
| 2 | Persistent data storage      | ✅      | PostgreSQL used per service             |
| 3 | Clear separation of concerns | ✅      | Microservices + layered architecture    |
| 4 | Real-time communication      | ⚠️      | WebSockets implemented for core updates |
| 5 | Scalable architecture        | ✅      | Dockerized services, Azure deployment   |


**Legend:**
- ✅ Fully implemented
- ⚠️ Partially implemented
- ❌ Not implemented

## Known Limitations

| Limitation | Impact | Potential Solution |
|------------|--------|-------------------|
| Increased complexity from microservices | More problems occur, harder to deploy | Test everything before pushing, read materials and documentations on the matter |
| Limited frontend interaction | UX validated only via prototype | Implement real frontend in future |
| Time limit | Some parts of the system might not be done fully | Fully structured plan of what will be done each point, finish in the future|

## References

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Redis Documentation](https://redis.io/docs)
- [Project source code](https://github.com/orgs/diploma-sdc-2025/repositories)
- 

