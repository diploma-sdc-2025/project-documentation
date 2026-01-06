# 4. Retrospective

This section reflects on the project development process, lessons learned, and future improvements.

## What Went Well ✅

### Technical Successes

- Successful implementation of a microservice architecture using Spring Boot
- All services were containerized with Docker and managed using Docker Compose
- Reliable integration with Azure Database for PostgreSQL and Redis
- Swagger/OpenAPI documentation enabled easy testing and demonstration
- GitHub Actions successfully validated builds and tests on every push

### Process Successes

- Clear separation of concerns between services improved maintainability
- Manual deployment strategy reduced time usage 

### Personal Achievements

- Gained experience with cloud deployment on Azure
- Learned to design, deploy, create and test a backend system
- Learned more about docker and docker compose
- First time worked with websockets

## What Didn't Go As Planned ⚠️

| Planned                 | Actual Outcome             | Cause                          | Impact |
| ----------------------- | -------------------------- | ------------------------------ | ------ |
| Frontend implementation | Backend-only delivery      | Focus on core backend features | Medium |

### Challenges Encountered

1. **Azure Networking & Firewall Configuration**
   - Problem: Initial database connectivity issues
   - Impact: Delayed deployment
   - Resolution: Correct firewall rules and environment variable configuration

2. **[Challenge 2]**
   - Problem: Ensuring all services start and communicate correctly
   - Impact: Debugging startup and configuration which used a bit of time
   - Resolution: 1 docker compose file that builds all the services

## Technical Debt & Known Issues

| ID     | Issue                  | Severity | Description                                         | Potential Fix                          |
| ------ | ---------------------- | -------- | --------------------------------------------------- | -------------------------------------- |
| TD-001 | Manual deployment      | Medium   | Containers must be started manually after VM reboot | Add startup scripts or systemd service |
| TD-002 | Limited test coverage  | Low      | Some services lack full integration tests(the over 70% is covered though)| Add Testcontainers-based tests         |
| TD-003 | No frontend            | Medium   | No UI / frontend to actually have the full working app | Create the front-end in Swift/kotlin |


### Code Quality Issues

- Some service-layer classes could be further refactored
- Additional logs and error handling could be added
- API documentation could have more explanation

## Future Improvements (Backlog)

If there was more time, these features/improvements would be prioritized:

### High Priority

1. **Automated Deployment**
   - Description: Add CD pipeline for VM deployment
   - Value: Faster and safer releases
   - Effort: Medium

### Medium Priority

2. **Service Health Dashboard**
   - Description: Unified health and metrics view
   - Value: Improved observability

### Nice to Have
3. Frontend UI for end users


## Lessons Learned

### Technical Lessons

| Lesson                                 | Context               | Application                       |
| -------------------------------------- | --------------------- | --------------------------------- |
| Containerization simplifies deployment | Docker usage          | Use Docker in all future projects |
| Cloud networking is critical           | Azure firewall issues | Plan networking early             |

### Process Lessons

| Lesson                             | Context                     | Application                         |
| ---------------------------------- | --------------------------- | ----------------------------------- |
| Incremental development helps      | Microservices integration   | Deliver features iteratively        |


### What Would Be Done Differently

| Area       | Current Approach    | What Would Change    | Why                 |
| ---------- | ------------------- | -------------------- | ------------------- |
| Planning   | Feature-first       | Architecture-first   | Reduce refactoring  |
| Technology | Docker Compose only | Add CD automation    | Improve efficiency  |
| Process    | Manual testing      | More automated tests | Deacrease the possibility of error |
| Scope      | Backend-only        | Thin frontend        | Improve usability   |


## Personal Growth

### Skills Developed

| Skill                  | Before Project | After Project |
| ---------------------- | -------------- | ------------- |
| Docker & Containers    | A bit higher then beginner | Intermediate  |
| Cloud Deployment       | Beginner       | Intermediate  |
| Microservice Design    | Beginner       | Intermediate  |
| CI with GitHub Actions | Beginner       | Intermediate  |
| Websockets             | Beginner       | Intermediate  |



### Key Takeaways

1. Cloud deployment introduces real-world complexity
2. Clear architecture decisions save time later
3. Reliability and clarity matter more than over-engineering

---

*Retrospective completed: [2026-01-05]*
