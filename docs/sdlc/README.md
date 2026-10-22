# Simple calorie app — SDLC Documentation

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start overview -->
build simple calorie app

**Documentation version:** v1
<!-- sdlc:auto:end overview -->

## Contents

<!-- sdlc:auto:start contents -->
| Document | State |
|---|---|
| [Requirements](requirements/README.md) | v1 approved |
| [Architecture](architecture/README.md) | 1 entry |
| [Development](development/README.md) | 1 entry |
| [Testing](testing/README.md) | 1 entry |
| [Deployment](deployment/README.md) | 1 entry |
| [Maintenance](maintenance/README.md) | 1 entry |
| [Data model](architecture/data-model.md) | generated |
| [API (OpenAPI)](architecture/api/openapi.yaml) | generated |
| [Decisions (ADRs)](architecture/adr/README.md) | 4 decisions |
| [Sprint plan](development/sprint-plan.md) | not generated yet |
| [Developer guide](development/developer-guide.md) | generated |
| [API reference](development/api-reference.md) | 1 route |
| [Test report](testing/test-report.md) | not run yet |
| [Deployment guide](deployment/deployment-guide.md) | not deployed yet |
| [Runbook](deployment/runbook.md) | not deployed yet |
| [Release notes](deployment/release-notes.md) | 0 releases |
| [Traceability matrix](traceability-matrix.md) | 31 requirements |
| [Changelog](CHANGELOG.md) | 1 version |
<!-- sdlc:auto:end contents -->

## Open gaps

<!-- sdlc:auto:start open-gaps -->
### Requirements

- No explicit approval date or approver name is recorded for the requirements set itself (only the technology stack and runtime services are explicitly marked 'approved by product owner').
- EPIC-001 and EPIC-007 narrative text ('Assumptions carried from discovery') references authentication and delivery being built on a 'Next.js/React' stack, but the product-owner-approved stack names only 'React 19' with no mention of Next.js. This is a discrepancy between two inputs and should be confirmed before architecture/design proceeds.
- Narrative assumption text names a 'Next.js/React' stack, which conflicts with the approved stack of plain React 19 (see REQ-STACK-001) - needs resolution before build.
- Exact floor values (1200/1500 kcal) and extreme-value bounds are explicitly flagged in the input as discovery assumptions, not confirmed clinical rules, pending review before launch.
- Branded/packaged product coverage gap is an accepted risk for launch (per NFR-013) - confirm this remains acceptable as the catalogue is reviewed pre-launch.
- Dependency on a hosted LLM introduces per-call cost and latency, explicitly called out in the input as an assumption to be validated - no cost/latency budget or fallback SLA is specified.
- Narrative assumption text again references a 'Next.js/React' stack, which conflicts with the approved React-19-only stack (see REQ-STACK-001).
- The free-of-charge model is explicitly flagged in the input as an assumption for this release only, to be reviewed in a later release.
- NFR-012 (HIPAA non-applicability) and NFR-014 (unconfirmed safety-floor values) are both explicitly recorded in the input as assumptions pending review before launch - no review outcome is recorded yet.
- NFR-013 (thin branded/packaged coverage) is an accepted usability risk with no stated mitigation timeline beyond the search empty-state messaging.

### Architecture

- Architecture phase has not started - no design artifacts, diagrams, data model, or screen definitions exist yet to document.

### Development

- Development phase has not started - no planning or implementation records exist yet to document.

### Testing

- Testing phase has not started - no test plans or results exist yet to document.

### Deployment

- No deployment has been recorded yet - no environments, releases, or infrastructure provisioning evidence exists to document.

### Maintenance

- No maintenance activity can be documented because the application has not been deployed.
<!-- sdlc:auto:end open-gaps -->
