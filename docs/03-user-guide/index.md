# 3. User Guide

This section explains how end users can access and use AutoChess Classic through the web UI, plus optional API usage via Swagger/Postman.

## Contents

- [Features Walkthrough](features.md)
- [FAQ & Troubleshooting](faq.md)

## Getting Started

### System Requirements

| Requirement | Minimum | Recommended |
|------------|---------|-------------|
| **Browser** | Chrome 90+, Firefox 88+, Safari 14+ | Latest version |
| **Internet** | Required | Stable broadband |
| **Device** | Desktop / Laptop | Desktop |
| **Tools** | Browser | Browser + Swagger UI/Postman for API testing |

### Accessing the Application

1. Open your web browser.
2. Navigate to: [Application URL](https://kon-autochess.francecentral.cloudapp.azure.com)
3. Optional API docs (dev): Swagger UI on service ports (for example `http://localhost:8081/swagger-ui.html` when running locally).

### First Use (UI Flow)

#### Step 1: Open the home page

You can choose one of the following:

- **Play as guest** for quick access.
- **Create account** to keep persistent stats and ranking history.
- **Log in** if you already have an account.

#### Step 2: Start matchmaking

1. Click **Play**
2. Choose an available mode
3. Click **Play** in the mode picker

#### Step 3: Play and track progress

- Guests can play matches but have limited profile tracking.
- Registered users can view persistent stats and leaderboard-related progress.

### Optional API Flow (Swagger/Postman)

1. Register: `POST /api/auth/register`
2. Login: `POST /api/auth/login`
3. Use protected endpoints with:
   `Authorization: Bearer <accessToken>`

## Quick Start Guide

| Task | How to do it |
|------|--------------|
| Open app | Visit `https://kon-autochess.francecentral.cloudapp.azure.com` |
| Play quickly | Use guest mode from home page |
| Create account | Use register flow from UI |
| Login | Use login flow from UI or `POST /api/auth/login` |
| Call protected APIs | Send `Authorization: Bearer <token>` |
| Check service health | `GET /actuator/health` |

## User Roles

| Role | Permissions | Access level |
|------|-------------|--------------|
| **Guest user** | Play matchmaking without permanent account profile | Limited |
| **Registered user** | Full gameplay + persistent stats/profile tracking | Standard |
| **Administrator** | Analytics/admin maintenance flows | Elevated |
