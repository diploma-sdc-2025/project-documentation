# 4. Retrospective

This section reflects on project execution, delivery outcomes, technical trade-offs, and the most important lessons learned.

## What Went Well

### Technical Successes

- Implemented a working microservice architecture with clear service boundaries (auth, matchmaking, game, battle, analytics).
- Delivered a usable web frontend on top of the API layer (guest and registered user flows).
- Containerized services with Docker and coordinated local/VM startup through Docker Compose.
- Integrated PostgreSQL, Redis, and OpenAPI-based API documentation.
- Added realtime analytics support for admin monitoring (SSE; optional WebSocket paths explored).

### Process Successes

- Iterative delivery helped keep the project shippable at each milestone.
- Service isolation improved debugging ownership and reduced accidental coupling.
- Documentation quality improved over time, especially in technical criteria sections.

### Personal Achievements

- Gained practical Azure deployment and networking experience.
- Improved confidence in microservice design and environment-based configuration.
- Strengthened end-to-end ownership: implementation, testing, deployment, and documentation.

## What Did Not Go as Planned

| Planned | Actual Outcome | Cause | Impact |
|---|---|---|---|
| Early stable environment setup | Repeated environment/network adjustments | Azure firewall and env coordination complexity | Slower early iterations |
| Smooth multi-service startup | Several startup/config mismatch cycles | Cross-service secrets/URLs and dependency ordering | Time spent on runtime debugging |
| Uniform test depth across services | Test depth varied by service | Time constraints and feature pressure | Uneven confidence in edge cases |

### Main Challenges

1. **Cloud networking and DB access**
   - Initial PostgreSQL connectivity and firewall issues delayed stable deployments.
   - Resolved through consistent environment variable management and VM/IP rules.

2. **Service-to-service configuration drift**
   - JWT/internal secret mismatches and URL wiring caused intermittent failures.
   - Resolved with shared env conventions and clearer deployment defaults.

3. **Distributed troubleshooting overhead**
   - Failures often surfaced in one service but originated in another.
   - Mitigated through better logs and endpoint-by-endpoint validation.

## Technical Debt & Known Issues

| ID | Issue | Severity | Description | Potential Fix |
|---|---|---|---|---|
| TD-001 | Manual deployment steps | Medium | VM restart/deploy still requires manual orchestration | Add startup automation and CI/CD rollout scripts |
| TD-002 | Uneven test depth | Medium | Some services have weaker integration coverage | Expand Testcontainers/integration suites |
| TD-003 | UI polish/accessibility backlog | Low-Medium | Core UX works, but polish and accessibility can be improved | Iterative UX passes and accessibility checklist |

## Future Improvements (Backlog)

### High Priority

1. **Deployment automation (CD)**
   - Add repeatable VM deployment pipeline.
   - Value: safer, faster releases with fewer manual steps.

2. **Cross-service observability baseline**
   - Standardize correlation IDs, structured logs, and basic dashboards.
   - Value: faster incident triage across service boundaries.

### Medium Priority

3. **Deeper integration testing**
   - Expand multi-service and failure-path tests.
   - Value: stronger confidence during changes.


### Nice to Have

4.**UX refinement pass**
   - Improve responsive details, accessibility, and micro-interactions.

## Lessons Learned

### Technical Lessons

| Lesson | Context | Application |
|---|---|---|
| Service boundaries help long-term maintainability | Microservice split by domain | Keep ownership explicit and avoid shared DB shortcuts |
| Environment consistency matters as much as code | Secrets, hostnames, ports, CORS | Use standardized env templates and validation checks |
| Redis + PostgreSQL split works well for this domain | Hot state vs durable facts | Keep volatile/runtime state separate from persistent records |

### Process Lessons

| Lesson | Context | Application |
|---|---|---|
| Iterative delivery reduced risk | Scope evolved during development | Keep features deployable in small increments |
| Documentation must evolve with implementation | Initial docs diverged from runtime behavior | Update docs in the same change set as code |
| Early integration testing saves time later | Multi-service regressions were costly | Add integration checks earlier in each milestone |

### What Would Be Done Differently

| Area | Current Approach | Future Approach | Why |
|---|---|---|---|
| Planning | Feature-first execution | Milestone architecture checkpoints first | Reduce late refactoring |
| Deployment | Manual compose-centric rollout | Automated CI/CD deployment steps | Improve reliability and speed |
| Documentation | Updated mostly after feature work | Update docs together with implementation | Keep docs continuously accurate |

## Personal Growth

### Skills Developed

| Skill | Before Project | After Project |
|---|---|---|
| Docker and Compose | Beginner+ | Intermediate |
| Cloud deployment (Azure VM) | Beginner | Intermediate |
| Microservice architecture | Beginner | Intermediate |
| API-first integration and troubleshooting | Beginner | Intermediate |
| Realtime patterns (SSE/WebSocket concepts) | Beginner | Intermediate |

### Key Takeaways

1. Real-world reliability depends on both code quality and deployment discipline.
2. Clear boundaries and conventions reduce multi-service complexity.
3. Small, consistent improvements in testing and documentation compound over time.

---
