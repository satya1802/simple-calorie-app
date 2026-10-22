<!-- sdlc:auto:start adr -->
# ADR-0003: Use PostgreSQL 16 for Database

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Database technology does the project use? Decided in architecture v1.

## Considered Options

- PostgreSQL 16
- MySQL
- SQLite

## Decision Outcome

Chosen option: PostgreSQL 16, because Accounts, consent, body details, dated targets, foods, portions and entries are tabular and related; GDPR export is one query and deletion is one cascading delete, and placing the instance plus backups in an EU region satisfies NFR-007.

## Consequences

Full-text search exists in Postgres but Meilisearch is the approved tool for forgiving, typo-tolerant food search, so search lives there rather than in the database.
<!-- sdlc:auto:end adr -->