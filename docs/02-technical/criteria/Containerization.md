# Criterion: Containerization

## Architecture Decision Record

### Status

**Status:** Accepted 

**Date:** [2026-01-04]

### Context

The diploma project requires the system to demonstrate containerization, showing that the application can be packaged, deployed, and executed consistently across environments.
AutoChess Classic consists of multiple independent Spring Boot microservices, which introduces the following forces and constraints:
- Services can be independently deployable
- Local development and cloud deployment should behave consistently
- The deployment platform (Azure) supports container-based workloads
- The project must remain manageable within a solo developer scope
- Infrastructure setup should be reproducible and documented

### Decision

This project is deployed using **Docker and Docker Compose** and follows a **multi-container architecture**.
Each backend component runs in its own isolated container, built from a dedicated Docker image.
Docker is responsible for:
- Service isolation
- Environment configuration
- Resource limitation (memory)
- Networking between services
- Consistent local deployment using Docker Compose
All services are started together using a single `docker-compose.yml` file.

### Alternatives Considered

| Alternative                       | Pros              |                 Cons                     |       Why Not Chosen           |
| --------------------------------- | ------------------| ---------------------------------------- | ----------------------------   |
| Kubernetes (AKS)                  | industry standard | High complexity, learn a new techonology | Overkill for the project scope |
| Single container for all services | Easier setup      | Breaks microservices isolation           | Violates architectural goals   |


### Consequences

**Positive:**
- Consistent runtime environment across all stages
- Simplified local setup 
- Clear separation of microservices
- Cloud-ready deployment model

**Negative:**
- Can be a mess configuring multiple Dockerfiles
- Requires basic container knowledge
- Slow builds for bigger apps

**Neutral:**
- Scaling is limited to platform capabilities (sufficient for demo)

## Implementation Details

### Project Structure

- Every service has a docker file
- Own repository for the docker-compose.yml which runs the containers
- Connects to the Azure db
- Environment variables are used for all configuration values

### Key Implementation Decisions

| Decision                         | Rationale                                |
| -------------------------------- | ---------------------------------------- |
| One Docker image per service     | Preserves microservice independence      |
| Multi-stage Docker builds        | Smaller image size and faster startup    |
| Docker Compose for local dev     | Simple control of all dependencies       |
| Same images for cloud deployment | Avoids environment-specific bugs         |
| Env-var configuration only       | Keeps images immutable                   |

### Code Examples

This is how one of the services builds the picture
```yml
services:

  auth-service:
    build: ../auth-service
    ports:
      - "8081:8080"
    networks:
      - backend
    deploy:
      resources:
        limits:
          memory: 384M
    environment:
      SERVER_PORT: 8080
      AUTH_JWT_SECRET: ${AUTH_JWT_SECRET}
      DB_SERVER: ${DB_SERVER}
      DB_PORT: ${DB_PORT}
      DB_NAME: ${AUTH_DB_NAME}
      DB_USERNAME: ${DB_USERNAME}
      DB_PASSWORD: ${DB_PASSWORD}
      APP_USER: ${AUTH_APP_USER}
      APP_PASSWORD: ${AUTH_APP_PASSWORD}
```

### Diagrams

[logical-architecture-diagram](../../assets/diagrams/logical-architecture-diagram.png)

## Requirements Checklist

| # | Requirement                    | Status | Evidence/Notes                                |
| - | ------------------------------ | ------ | --------------------------------------------- |
| 1 | Application is containerized   | ✅      | Dockerfiles for all services                  |
| 2 | Multiple services supported    | ✅      | One container per microservice                |
| 3 | Reproducible local environment | ✅      | Docker Compose setup                          |
| 4 | Cloud deployment               | ⚠️     | Azure App Service deployment (manual scaling) |


**Legend:**
- ✅ Fully implemented
- ⚠️ Partially implemented
- ❌ Not implemented

## Known Limitations

| Limitation                  | Impact                | Potential Solution      |
| --------------------------- | --------------------- | ----------------------- |
| Manual image builds         | Slower iteration      | Add CI/CD pipeline      |
| No container health checks  | Limited observability | Add Docker healthcheck  |


## References

- Docker Documentation: (https://docs.docker.com/)
- Docker Compose Documentation: (https://docs.docker.com/compose/)
- Azure App Service for Containers: (https://learn.microsoft.com/azure/app-service/configure-custom-container)
