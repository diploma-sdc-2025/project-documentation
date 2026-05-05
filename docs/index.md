# Auto-Chess

## Project Information

| Field | Value |
|-------|-------|
| Student | Konstantin Kernazhytski |
| **Group** | Java |
| **Supervisor** | Pavlo Andriiash |
| **Date** | 2026-01-07 |

## Links

| Resource | URL |
|----------|-----|
| Production (HTTPS) | https://kon-autochess.francecentral.cloudapp.azure.com |
| Repository |  https://github.com/orgs/diploma-sdc-2025/repositories |
| API specification | Unified OpenAPI: `api-docs/reference/openapi.yaml` in the project repo  Per-service Swagger UI may exist on each port in development. |
| Design | https://www.figma.com/design/4Eq9PM6MOgPSc1SZfwbQDA/auto-chess-design |

## Elevator Pitch

AutoChess Classic is a strategy game that reimagines traditional chess as an auto-battler: players plan economy and positioning while battles resolve automatically. It targets casual and chess-aware players who want shorter sessions than classical chess. The implementation includes a **React** web client and **Spring Boot** services for auth, matchmaking, game state, battle evaluation (Stockfish), and analytics.

## Evaluation Criteria Checklist

Documentation files live under `docs/02-technical/criteria/` (filenames match below).

| # | Criterion | Documentation |
|---|-----------|----------------|
| 1 | Backend | [backend.md](02-technical/criteria/backend.md) |
| 2 | UX | [UX.md](02-technical/criteria/UX.md) |
| 3 | Database | [database.md](02-technical/criteria/database.md) |
| 4 | Real-time Analytics | [Real-time Analytics.md](02-technical/criteria/Real-time%20Analytics.md) |
| 5 | Containerization | [Containerization.md](02-technical/criteria/Containerization.md) |
| 6 | API documentation | [API documentation.md](02-technical/criteria/API%20documentation.md) |
| 7 | Automated testing | [Automated testing.md](02-technical/criteria/Automated%20testing.md) |

## Documentation Navigation

- [Project Overview](01-project-overview/index.md) — Business context, goals, and requirements
- [Technical Implementation](02-technical/index.md) — Architecture, tech stack, and criteria
- [User Guide](03-user-guide/index.md) — How to use the application
- [Retrospective](04-retrospective/index.md) — Lessons learned and future improvements

---

*Document created: 01.02.2026*  
*Last updated: 04.05.2026*
