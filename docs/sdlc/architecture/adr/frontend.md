<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-10-22

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- React 19
- Vue
- SvelteKit

## Decision Outcome

Chosen option: React 19, because Builds the mobile-first SPA with instant client-side recalculation of totals and portion/estimate figures (NFR-018), and ships as an installable PWA with a web manifest and service worker so it saves to the home screen in a standalone window (US-030, NFR-024).

## Consequences

A plain React SPA needs an explicit router and PWA plugin (added as packages); server-side rendering is not used, which is fine for this authenticated single-user tool.
<!-- sdlc:auto:end adr -->