# Deployment & DevOps

## Infrastructure

### Deployment Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                           Microsoft Azure                           │
│                    Ubuntu Virtual Machine (IaaS)                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌──────────────────┐  ┌──────────────────┐   ┌────────────────┐    │
│  │  Auth Service    │  │ Matchmaking Service  |  Game Service  │    │
│  │  (Spring Boot)   │  │  (Spring Boot)   │   |  (Spring Boot) │    │
│  │  Port: 8081      │  │  Port: 8082      │   |  Port: 8083    │    │
│  └──────────────────┘  └──────────────────┘   └────────────────┘    │
│                                                                     │
│  ┌──────────────────┐  ┌──────────────────┐                         │
│  │ Battle Service   │  │ Analytics Service│                         │
│  │ (Spring Boot)    │  │ (Spring Boot)    │                         │
│  │ Port: 8084       │  │ Port: 8080       │                         │
│  └──────────────────┘  └──────────────────┘                         │
│                                                                     │
│                 ┌──────────────────────────┐                        │
│                 │        Redis (Cache)     │                        │
│                 │        Port: 6379        │                        │
│                 └──────────────────────────┘                        │
│                                                                     │
│        ┌─────────────────────────────────────────────┐              │
│        │ Azure Database for PostgreSQL               │              │
│        │ Port: 5432                                  │              │
│        └─────────────────────────────────────────────┘              │
│                                                                     │
└───────────────────────────────┼─────────────────────────────────────┘
                                │
                          [ Internet ]

```

### Environments

| Environment     | URL                              | Branch      |
| --------------- | -------------------------------- | ----------- |
| **Development** | `http://localhost:8080-8084`     | `feature/*` |
| **Production**  | HTTPS demo host + `/api/*` via nginx; JVM ports internal/local | `main`      |


## CI 

### Flow 

```
┌──────────────┐   ┌──────────┐   ┌──────────────────────────────┐
│ Local Build  │──▶│  Commit │──▶│ Manual / scripted deploy (VM) │
│ & Testing    │   │          │   │ Optional CI when workflows exist │
└──────────────┘   └──────────┘   └──────────────────────────────┘

```


| Step              | Tool           | Actions                           |
| ----------------- | -------------- | --------------------------------- |
| **Build (Local)** | Maven          | `mvn clean package`               |
| **Test (Local)**  | JUnit          | `mvn test`                        |
| **Commit**        | Git            | Push code to GitHub               |
| **CI Validation** | *(Optional)* GitHub Actions per repo | Add workflows when maintained |
| **Deploy**        | Docker Compose | Manual deployment to Azure VM     |

*Example GitHub Actions workflows may live in individual service repositories; the monorepo snapshot may not include `.github/workflows` at the root.*

## Environment Variables

| Variable          | Description               | Required | Example                                    |
| ----------------- | ------------------------- | -------- | ------------------------------------------ |
| `AUTH_JWT_SECRET` | JWT signing secret (≥32 bytes; dev-only default in properties) | Yes | `<set-via-env-never-commit>` |
| `DB_SERVER`       | PostgreSQL host           | Yes      | `autochess-db.postgres.database.azure.com` |
| `DB_PORT`         | PostgreSQL port           | Yes      | `5432`                                     |
| `DB_USERNAME`     | Database user             | Yes      | `dbuser`                                   |
| `DB_PASSWORD`     | Database password         | Yes      | `postgres`                                 |
| `DB_NAME`         | Service-specific database | Yes      | `auth-service`                             |
| `REDIS_HOST`      | Redis host                | No       | `redis`                                    |
| `REDIS_PORT`      | Redis port                | No       | `6379`                                     |


**Secrets Management:**
Secrets are stored in a .env file on the Azure VM and are not committed to version control.
The file is created manually during deployment.

## How to Run Locally

### Prerequisites

Prerequisites

- Java 21+
- Maven
- Docker & Docker Compose
- PostgreSQL 
- Redis 

### Setup Steps

```bash

# 1. Create an empty folder, inside it create 5 folders, each named like the services on github.
mkdir autochess 
cd autochess
# 2.Clone each repo to each folder
git clone [repository-url]
#3 Create the 6th folder, named deployement and clone this repo 
git clone [repository-url]
#4 Add an env file to the project inside the deployement folder with all the needed parameters
cd deployement
nano .env
#5 Build and run the following command from the IDE for example, inside the deployement folder:
docker compose up -d --build

#6 All done, the services are running
```

### Verify Installation

After starting the server:

1. Open Swagger UI: [swagger](http://localhost:8081/swagger-ui.html)
2. Check health on the VM/service host if Actuator is exposed (replace host): `http://<host>:8081/actuator/health`
3. Test authentication flow in swagger or Postman: Register → Login → Refresh token 

## Monitoring & Logging

| Aspect                  | Tool                | Dashboard URL    |
| ----------------------- | ------------------- | ---------------- |
| **Application Logs**    | Docker logs         | Azure VM (SSH)   |
| **Database Monitoring** | Azure Portal        | Azure PostgreSQL |
| **Container Status**    | Docker Compose      | VM CLI           |
| **Uptime**              | Manual              | Actuator health URLs per exposed service |

