# Criterion: Well-designed UX

## Architecture Decision Record

### Status

**Status:** Accepted 

**Date:** 2026-01-03

### Context

This criterion is intended to demonstrate how the application will appear without requiring full front-end implementation. It consists of wireframe designs created in Figma for each key screen of the auto-chess battler. These designs provide a clear and accurate representation of the app’s visual layout and user experience, and they also include a prototype to test how the application interacts with users. Due to time constraints, the complete front end could not be developed; therefore, this criterion is used to partially address and represent the front-end aspect of the application.

### Decision

UX is delivered as a mobile-first Figma wireframe + interactive prototype, covering the core user journey:
- Login / Registration page
- Main menu page
- Game page


### Alternatives Considered

| Alternative | Pros | Cons | Why Not Chosen |
| ------------| -----| -----| -------------- |
| Full implemented front-end (Swift/Kotlin) | Real clickable UI connected to backend; best realism | Too time-consuming; | Scope focuses on backend + database; UX criterion satisfied with a prototype
| Minimal HTML UI | Faster to make than React; simple demo | Still takes significant time; limited design quality | Would reduce time for backend/testing/deployment |                                           |

### Consequences

**Positive:**
- UX criteria achieved
- Figma prototype shows how the user interacts and shows the full flow

**Negative:**
- Prototype is not connected to live backend, so some interactions are conceptual
- No real frontend

**Neutral:**
- Swagger/Postman remains the primary way to interact with the system

## Implementation Details

### Project Structure

Every page has the following tree:

```
iPhone 16/                   // Device frame used for mobile-first design
├── Frame/                   // Root container for the entire screen
    ├── Background           // Main screen background 
        └── Input            // User input component 
        └── Background       // Background of a frame 
        └── Button           // A button frame
        └── ...
```

### Key Implementation Decisions

| Decision | Rationale |
|----------|-----------|
| User research | Find out from the community how they want to see the game, is anyone interested|
| Scenarios | User scenarios of what can happen when someone uses the app |
| Sitemap| Shows exactly how to reach a certain screen in the app|

### Screens

[Login screen](../assets/screenshots/login-page.png)

[Main screen](../assets/screenshots/main-page.png)

[Game screen](../assets/screenshots/game-screen.png)

[Login screen](../assets/screenshots/game-screen-no-gold.png)

## Requirements Checklist

| # | Requirement | Status | Evidence/Notes |
| - | ------------| ------ | -------------- |
| 1 | UX artifacts exist (wireframes/prototype) | ✅ | Figma screens (https://www.figma.com/design/4Eq9PM6MOgPSc1SZfwbQDA/auto-chess-design?node-id=9-4&p=f&t=MlEc0UfLGkXC6YE8-0)|
| 2 | Clear user flows for core features        | ✅ | Notion (https://www.notion.so/Refined-UX-2bc1e0f95d3a80e28ba0d4de914dd70e)|
| 3 | Usability testing with real users (3–5)   | ⚠️     | Planned/ongoing: tests with collegues|

**Legend:**
- ✅ Fully implemented
- ⚠️ Partially implemented
- ❌ Not implemented

## Known Limitations

| Limitation| Impact | Potential Solution |
| ----------| -------| -------------------|
| Prototype not connected to backend | Cannot validate real loading/error states | Implement minimal frontend in future or extend prototype with more states |
| Small number of test users| Limited statistics | Get feedback from more people, maybe a post on reddit |

## References

- Figma learn (https://help.figma.com/hc/en-us)
- Notion link (https://www.notion.so/Refined-UX-2bc1e0f95d3a80e28ba0d4de914dd70e)
- Figma link (https://www.figma.com/design/4Eq9PM6MOgPSc1SZfwbQDA/auto-chess-design?node-id=9-4&p=f&t=9W528W83a23LvP1b-0)
