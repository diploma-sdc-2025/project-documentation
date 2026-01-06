# FAQ & Troubleshooting

## Frequently Asked Questions

### General

**Q: What type of application is this?**

A: This project is for now a backend-only microservice application. It exposes REST APIs documented via Swagger/OpenAPI and does not include a graphical frontend.

---

**Q: How do I interact with the application??**

A: Interaction is done through:
- Swagger UI (browser-based)
- API clients such as Postman

---

**Q: Where is the application hosted?**

A: The application is deployed on a Microsoft Azure Ubuntu Virtual Machine and runs using Docker Compose.

---

### Account & Access

**Q: Do I need an account to use the APIs?**

A: Yes. Most endpoints require authentication. Users must first register and log in using the Auth Service to obtain a JWT token.

---

**Q: How do I authenticate API requests?**

A: 
Include the following HTTP header in requests:
- Authorization: Bearer <accessToken>
Tokens are obtained from the /api/auth/login endpoint.

---

### Features

**Q: Why do some endpoints return 401 Unauthorized?**

A: This usually means:
- The JWT token is missing
- The token has expired
- The token was not included in the Authorization header
Re-authenticate or refresh the token to resolve this.

**Q: Are all services required to be running?**

A: Yes. For full functionality:
- All backend services must be running
- Redis must be available
- The PostgreSQL database must be accessible
---

## Troubleshooting

### Common Issues

| Problem                      | Possible Cause                  | Solution                                    |
| ---------------------------- | ------------------------------- | ------------------------------------------- |
| Swagger UI does not load     | VM is stopped                   | Start the VM and run `docker compose up -d` |
| API returns 401 Unauthorized | Missing or expired JWT          | Log in again and use a valid token          |
| Cannot connect to API        | Containers not running          | Check with `docker compose ps`              |
| Database connection error    | Azure DB firewall blocks access | Allow VM public IP in Azure PostgreSQL      |
| Slow responses               | VM resource limits              | Restart services or reduce load             |


### Error Messages

| Error Code / Message        | Meaning                  | How to Fix                  |
| --------------------------- | ------------------------ | --------------------------- |
| `401 Unauthorized`          | Authentication failed    | Login and provide valid JWT |
| `403 Forbidden`             | Insufficient permissions | Ensure correct role/token   |
| `500 Internal Server Error` | Server-side error        | Check logs                  |
| `Connection refused`        | Service not running      | Restart containers          |


## Getting Help

### Self-Service Resources

- [Project Documentation](../index.md)
- API Documentation: [Swagger UI](http://134.112.154.167:8081/swagger-ui.html)
- Source Code: [GitHub repositories](https://github.com/orgs/diploma-sdc-2025/repositories)

### Contact Support

| Channel              | Response Time | Best For               |
| -------------------- | ------------- | ---------------------- |
| Email                | ASAP          | Bugs & feature issues  |


### Reporting Bugs

When reporting a bug, please include:

1. **Steps to reproduce** - What actions lead to the issue?
2. **Expected behavior** - What should happen?
3. **Actual behavior** - What actually happens?
4. **Screenshots** - If applicable
5. **Browser/Device info** - Browser name, version, OS

Submit bug reports at: [konstantin.kernazhytski@gmail.com]
