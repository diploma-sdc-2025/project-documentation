# Criterion: Database

## Architecture Decision Record

### Status

**Status:** Accepted 

**Date:** 2026-01-03

### Context

The project requires a properly designed and implemented database layer demonstrating persistence, schema design, and correct integration with the application logic.
AutoChess Classic is built using a microservices architecture, which introduces the following constraints:
- Each service must be independently deployable and scalable
- Services should not share database schemas to avoid tight coupling
- Game-related data includes both long-term persistent data (users, matches, history) and short-lived, high-frequency state using  (queues, board positions)
- Performance is important for matchmaking and game state handling
- The solution uses PostgreSQL and Redis for cache-in memory

### Decision

A database-per-service pattern was chosen, using PostgreSQL for persistent storage and Redis for real-time data.
Each microservice owns its database schema and is responsible for its own data. Redis is used where frequent updates are required (e.g. matchmaking queue).

### Alternatives Considered

| Alternative | Pros | Cons | Why Not Chosen |
| ------------| -----| -----| -------------- |
| Single shared database | Simple to implement, 1 database |poor scalability, breaks microservices principles| Violates service isolation |
| NoSQL database (MongoDB) | Flexible schema, fast writes | less suitable for structured game data | Postgres is better for keeping data for this type of project, also have used it before unlike NoSQL dbs |
| In-memory only  | Very fast, simple  | Data loss on restart, no data at all    | Persistent storage is mandatory |

### Consequences

**Positive:**
- Clear data ownership per service
- Improved scalability
- Clean separation between persistent and transient data
- Easier to fix if something goes wrong

**Negative:**
- More complex as more code has to be written and mantained
- More configuration required during deployment

**Neutral:**
- Some data duplication across services is accepted as a trade-off for decoupling

## Implementation Details

### Database Design Overview

#### **Auth-service**

PostgreSQL:
- users,
- password_reset_tokens
- refresh_tokens
- user_sessions

#### **Game-service**

PostgreSQL:
- matches, player_inventory, pieces, shop_offers
- player_resources, piece_upgrades
Redis: Namespace (game)
- game:board:{matchId} (in-memory board matrix)
- ame:state:{matchId} (current game state)

#### **Battle-service** 

PostgreSQL: 
- battle_outcomes
- battle_logs
- piece_interactions

#### **Matchmaking-serivce**

PostgreSQL:
- matchmaking_history 
- queue_statistics
Redis: Namespaces (queue)
- queue:active (sorted set - FIFO)
- queue:user:{userId} (player data)
- queue:stats (real-time metrics)

#### **Analytics-service**

PostgreSQL: 
- gameplay_events 
- service_health_metrics 
- aggregated_stats

### Key Implementation Decisions

| Decision                        | Rationale                                           |
| ------------------------------- | --------------------------------------------------- |
| Database-per-service            | Prevents tight coupling between microservices       |
| PostgreSQL for persistence      | Strong consistency and relational integrity         |
| Redis for queues and game state | Low-latency access and high update frequency        |
| JPA/Hibernate                   | Reduces boilerplate code                            |
| Migrations (flyway)             | Manage data and allow changes if requirements change|


### Code Examples

Example of JPA/Hibernate usage:

```Java
public interface MatchmakingHistoryRepository
        extends JpaRepository<MatchmakingHistory, Long> {

    Optional<MatchmakingHistory> findFirstByUserIdAndStatus(
            Long userId,
            String status
    );
}
```

Example of a file used for migrations using flyway
```SQL
CREATE TABLE matchmaking_history (
                                     id SERIAL PRIMARY KEY,
                                     user_id BIGINT NOT NULL,
                                     joined_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
                                     matched_at TIMESTAMP,
                                     left_queue_at TIMESTAMP,
                                     match_id BIGINT,
                                     wait_time_seconds INT,
                                     status VARCHAR(50) NOT NULL DEFAULT 'WAITING',
                                     cancel_reason VARCHAR(255)
);

CREATE INDEX idx_matchmaking_history_user_id ON matchmaking_history(user_id);
CREATE INDEX idx_matchmaking_history_match_id ON matchmaking_history(match_id);
CREATE INDEX idx_matchmaking_history_status ON matchmaking_history(status);
```

### Diagrams

[auth-service](../assets/diagrams/auth-service.png)

[battle-service](../assets/diagrams/battle-service.png)

[analytics-service](../assets/diagrams/analytics-service.png)

[matchmaking-service](../assets/diagrams/matchmaking-service.png)

[game-service](../assets/diagrams/game-service.png)

## Requirements Checklist

| # | Requirement                    | Status | Evidence/Notes                                 |
| - | ------------------------------ | ------ | ---------------------------------------------- |
| 1 | Persistent storage             | ✅      | PostgreSQL databases per service               |
| 2 | Proper schema design           | ✅      | JPA entities + ER diagrams                     |
| 3 | Integration with backend logic | ✅      | Repositories used in service layer             |
| 4 | Redis for real time updates    | ⚠️     | Redis used for queues, board state, analytics.  |


**Legend:**
- ✅ Fully implemented
- ⚠️ Partially implemented
- ❌ Not implemented

## Known Limitations

| Limitation | Impact | Potential Solution |
|------------|--------|-------------------|
| Multiple databases increase setup complexity| Harder local deployment| Easier with docker-compose |
| Redis data is volatile | Data loss on restart | Normal for transient data |

## References

- [flyway docs](https://www.red-gate.com/products/flyway/community/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Redis Documentation](https://redis.io/docs)
- [Project source code](https://github.com/orgs/diploma-sdc-2025/repositories)
- 
