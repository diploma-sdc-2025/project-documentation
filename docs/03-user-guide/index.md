# 3. User Guide

This section provides instructions for end users on how to use the application.

## Contents

- [Features Walkthrough](features.md)
- [FAQ & Troubleshooting](faq.md)

## Getting Started

### System Requirements

| Requirement  | Minimum                             | Recommended      |
| ------------ | ----------------------------------- | ---------------- |
| **Browser**  | Chrome 90+, Firefox 88+, Safari 14+ | Latest version   |
| **Internet** | Required                            | Stable broadband |
| **Device**   | Desktop / Laptop                    | Desktop          |
| **Tools**    | Browser or API client (Postman)     | Swagger UI       |


### Accessing the Application

1. Open your web browser
2. Navigate to: **[Application URL: http://134.112.154.167/)](http://134.112.154.167)**
3. Swagger url: http://134.112.154.167:8081/swagger-ui.html

### First Use (API Flow)

#### Step 1: User Registration

This can be executed directly via Swagger UI or Postman.
1. Creates a new user account in the system
2. Requires username, email, and password

#### Step 2: User Login

1. POST /api/auth/login
2. Authenticates the user
3. Returns a JWT access token and refresh token

#### Step 3: [Main Dashboard]

After this, you would be on the main page of the app on frontend and using this access token can check other endpoints in other services
- Copy the accessToken from the login response
- In Swagger UI, click Authorize
- Paste the token as: Bearer <accessToken>
- Execute secured endpoints (game, matchmaking, battle, analytics)



## Quick Start Guide

| Task                 | How To                              |
| -------------------- | ----------------------------------- |
| Register user        | `POST /api/auth/register`           |
| Login                | `POST /api/auth/login`              |
| Get JWT token        | Use login response                  |
| Call protected API   | Use `Authorization: Bearer <token>` |
| Check service health | `GET /actuator/health`              |

## User Roles

| Role                   | Permissions                                  | Access Level |
| ---------------------- | -------------------------------------------- | ------------ |
| **Anonymous User**     | Register, login                              | Limited heavily |
| **Authenticated User** | Access game, matchmaking, battle APIs        | Limited      |
| **Administrator**      | Database & Analytics service management      | Full         |
