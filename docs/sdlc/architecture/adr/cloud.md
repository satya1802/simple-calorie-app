<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- GCP
- Azure

## Decision Outcome

Chosen option: AWS, because Managed RDS Postgres in an EU region with backups meets the GDPR commitment, Fargate runs the API and the Meilisearch container without patching servers, and the platform grows without re-platforming for a worldwide low-thousands audience (NFR-025).

## Consequences

Running Meilisearch as a Fargate task needs a persistent volume (EFS) for its index; a single region keeps data residency simple at this scale.
<!-- sdlc:auto:end adr -->