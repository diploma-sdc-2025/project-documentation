# 1. Project Overview

This section covers the business context, goals, and requirements for the project.

## Contents

- [Problem Statement & Goals](problem-and-goals.md)
- [Stakeholders & Users](stakeholders.md)
- [Scope](scope.md)
- [Features](features.md)

## Executive Summary

AutoChess Classic is a strategy game that transforms traditional chess into an auto-battler experience focused on planning, positioning, and unit synergy rather than manual turn-by-turn play. The project addresses the problem of long and complex chess matches that can be discouraging for casual or time-constrained players, while still offering enough strategic depth to engage experienced chess enthusiasts. By automating combat and introducing roguelike progression mechanics, the game allows players to make meaningful tactical decisions within short play sessions. The primary beneficiaries are casual strategy gamers and chess players seeking a modern, accessible alternative to classical chess. The key outcome of the project is a replayable, balanced game with matches lasting under 10 minutes, supported by data-driven balancing to adapt gameplay based on player behavior.

## Key Highlights

| Aspect | Description |
|--------|-------------|
| **Problem** | Traditional chess matches are time-consuming and intimidating for casual or new players. |
| **Solution** |A fast-paced auto-battler that preserves chess strategy while automating combat. |
| **Target Users** | Casual strategy players and chess enthusiasts seeking shorter, modern gameplay sessions. |
| **Key Features** | Automated battles, classic chess pieces like in real chess, short match duration, roguelike progression, data-driven balancing. |
| **Tech Stack** | Backend: Java (Spring Boot), REST APIs, PostgreSQL, JWT (Spring Security), Docker, GitHub Actions (CI/CD), Azure App Service. Monitoring: Spring Boot Actuator + Micrometer. UI: Figma (no implemented mobile frontend yet). |
