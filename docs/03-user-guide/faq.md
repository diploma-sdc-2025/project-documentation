# FAQ & Troubleshooting

## Frequently Asked Questions

### General

**Q: What type of application is this?**

A: AutoChess Classic is a web application with a graphical frontend and backend microservices. You can use it from the browser UI, and most backend APIs are also available through Swagger/OpenAPI.

---

**Q: How do I interact with the application?**

A: You can interact in two ways:
- Browser UI (main gameplay flow)
- Swagger UI / Postman (API testing)

---

**Q: Where is the application hosted?**

A: The application is deployed on a Microsoft Azure Ubuntu VM and runs with Docker Compose.

---

### Account & Access

**Q: Do I need an account to play?**

A: Not necessarily. You can play as a guest. If you want persistent stats and ranking history, create a full account.

---

**Q: Why are my profile stats or leaderboard entries missing?**

A: Guest sessions are temporary and do not provide full tracked profile progression. Register and log in with a full account to track stats and ranking over time.

---

**Q: How do I authenticate API requests?**

A: Include the header below in protected API calls:
- `Authorization: Bearer <accessToken>`

Tokens are obtained from `/api/auth/login` (or guest auth flow if enabled).

---

### Features & Runtime

**Q: Why do some endpoints return `401 Unauthorized`?**

A: This usually means:
- JWT token is missing
- Token is expired
- `Authorization` header format is wrong

Re-authenticate and retry with a valid token.

---

**Q: Why do I get `403 Forbidden`?**

A: Your token is valid, but your role/session is not allowed for that operation (for example, admin-only routes or restricted internal endpoints).

---

**Q: Are all services required to be running?**

A: Yes, for full functionality:
- Backend services (auth, matchmaking, game, battle, analytics)
- Redis
- PostgreSQL

If one service is down, related features may fail.

---

**Q: Matchmaking takes too long. What should I do?**

A: Check that matchmaking and game services are running and healthy. In low-traffic environments, queue time can be longer because matching needs another active player.

---

## Troubleshooting

### Common Issues

| Problem | Possible cause | Solution |
|--------|----------------|----------|
| App page does not load | VM/service is down | Start VM and run `docker compose up -d` |
| Swagger UI does not load | API container not running / wrong port | Check `docker compose ps` and service logs |
| API returns `401` | Missing or expired JWT | Log in again and send `Authorization: Bearer <token>` |
| API returns `403` | Insufficient permissions | Use correct role/token |
| Database connection error | DB credentials/network/firewall issue | Verify DB env vars and Azure PostgreSQL firewall rules |
| Queue stuck / no match | Matchmaking or game service unhealthy | Restart affected services and verify health endpoints |

### Error Messages

| Error code / message | Meaning | How to fix |
|----------------------|---------|------------|
| `401 Unauthorized` | Authentication failed | Login and provide a valid JWT |
| `403 Forbidden` | Authenticated but not allowed | Use correct role/session |
| `500 Internal Server Error` | Server-side failure | Check service logs |
| `Connection refused` | Service unreachable | Ensure container is up and port is exposed |

## Getting Help

### Self-Service Resources

- [Project Documentation](../index.md)
- API Documentation: [Swagger UI](http://134.112.154.167:8081/swagger-ui.html)
- Source Code: [GitHub repositories](https://github.com/orgs/diploma-sdc-2025/repositories)

### Contact Support

| Channel | Response time | Best for |
|---------|---------------|----------|
| Email | 1-2 business days | Bugs, access issues, feature questions |

### Reporting Bugs

When reporting a bug, include:

1. **Steps to reproduce**
2. **Expected behavior**
3. **Actual behavior**
4. **Screenshots** (if applicable)
5. **Browser/Device info** (browser name, version, OS)

Submit bug reports at: <mailto:konstantin.kernazhytski@gmail.com>
