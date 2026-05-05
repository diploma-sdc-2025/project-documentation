# Feature Walkthrough

## Feature 1: Authentication & Account Modes

### Overview

AutoChess Classic supports both **guest sessions** and **registered accounts**:

- **Guest mode** for quick entry and immediate play
- **Registered account** for persistent profile progress, statistics, and ranking history

### How to Use

1. Open the application home page.
2. Choose one path:
   - **Play as guest**
   - **Create account**
   - **Log in**
3. If registering/logging in, complete credentials and continue to the main menu.

### Expected Result

- Guest users can play immediately.
- Registered users can keep long-term progress and profile stats.

---

## Feature 2: Matchmaking

### Overview

The matchmaking flow pairs players using the queue service. The UI handles search state and mode selection.

### How to Use

1. From the main menu, click **Play**.
2. Choose an available mode in the mode picker.
3. Click **Play** in the picker to join queue.
4. Wait for assignment to a match.

### Expected Result

- User is queued and receives match assignment when an opponent is available.

### Notes

- Queue speed depends on active players and service health.
- Matchmaking uses Redis-backed queue state for fast updates.

---

## Feature 3: Profile & Statistics

### Overview

Profile visibility differs by account type.

- **Guest:** limited profile view and account creation prompt
- **Registered:** persistent stats and rating-related profile data

### How to Use

1. Open **Profile** from the main menu.
2. Check available details based on your account type.
3. If currently guest and you want progression, use **Create free account**.

### Expected Result

- Guests see a simplified profile.
- Registered users see persistent tracked values.

---

## Feature 4: Game State & Match Flow

### Overview

Game service manages match lifecycle and player state (economy, board/inventory, rounds). Battle results update match progression.

### How to Use

1. Enter a match through matchmaking.
2. Use game UI actions during available phases.
3. Progress through rounds until match completion.

### Expected Result

- Match state is updated consistently and can be resumed/refreshed through service-backed state.

---

## Feature 5: Analytics & Leaderboard

### Overview

Analytics service aggregates gameplay events and exposes leaderboard/statistics APIs. Admin-focused live streams are available for operational visibility.

### How to Use

1. Open leaderboard/statistics sections (for eligible users).
2. For API-level analytics, call analytics endpoints through Swagger or Postman.

### Expected Result

- Aggregated statistics and ranking-related data are returned.

---

## Optional API Quick References

For API testing (Swagger/Postman):

- Register: `POST /api/auth/register`
- Login: `POST /api/auth/login`
- Queue join: `POST /api/matchmaking/join`
- Queue status: `GET /api/matchmaking/status`

Use protected endpoints with:

- `Authorization: Bearer <accessToken>`

---

## Feature Availability

| Feature | Available |
|---------|-----------|
| Guest mode | Yes |
| Account registration/login | Yes |
| Matchmaking | Yes |
| Match gameplay state | Yes |
| Profile & tracked stats | Yes (registered), limited for guest |
| Analytics & leaderboard | Yes |
