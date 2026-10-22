<!-- sdlc:auto:start adr -->
# ADR-0002: Use FastAPI 0.115 for Backend

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Backend technology does the project use? Decided in architecture v1.

## Considered Options

- FastAPI 0.115
- Django REST Framework
- Flask

## Decision Outcome

Chosen option: FastAPI 0.115, because Holds the whole calorie engine — Mifflin-St Jeor, activity multiplier, deficit/surplus, configurable floors and extreme-target refusal — in one auditable server (NFR-015), and sits cleanly in front of the OpenAI estimation call and the USDA seed import.

## Consequences

Async endpoints are ideal for the OpenAI call but require care with blocking DB drivers; Pydantic validation centralises field-level input checks.
<!-- sdlc:auto:end adr -->