# Sprint Plan

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start sprints -->
## Sprint 1: Sprint 1

Stand up core identity foundation: email/password signup with consent, and kick off the USDA food catalogue seed.

Capacity: 20 points · Committed: 20

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-001 | Story | Sign up with email and password | 8 | backend-agent | — |
| ↳ US-001-T1 | Task | Create users and email_tokens schema and migrations | — | backend-agent | — |
| ↳ US-001-T2 | Task | Implement POST /auth/signup endpoint | — | backend-agent | — |
| ↳ US-001-T3 | Task | Wire verification email send via SendGrid | — | integration-agent | — |
| US-005 | Story | Give explicit consent for storing body details and food logs | 3 | backend-agent | — |
| US-022 | Story | Have the catalogue seeded from USDA FoodData Central | 8 | integration-agent | — |
| ↳ US-022-T1 | Task | Build USDA FoodData Central import script | — | integration-agent | — |
| ↳ US-022-T2 | Task | Implement idempotent upsert logic | — | backend-agent | — |
| ↳ US-022-T3 | Task | Add source attribution to food detail display | — | frontend-agent | — |
| US-031 | Story | Use the whole app free of charge | 1 | backend-agent | — |

## Sprint 2: Sprint 2

Complete alternate sign-in (Google), session management, and let users create their own custom foods.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-002 | Story | Sign up and sign in with Google | 8 | integration-agent | US-001, US-005 |
| ↳ US-002-T1 | Task | Implement GET /auth/google/start and /auth/google/callback | — | integration-agent | — |
| ↳ US-002-T2 | Task | Handle account creation/linking for Google email match | — | backend-agent | — |
| ↳ US-002-T3 | Task | Add first-time consent gate after Google callback | — | frontend-agent | — |
| US-003 | Story | Sign in and sign out | 5 | backend-agent | US-001 |
| US-021 | Story | Create a custom food | 5 | backend-agent | US-001 |

## Sprint 3: Sprint 3

Finish account recovery flows and make the food catalogue fully searchable via Meilisearch.

Capacity: 20 points · Committed: 16

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-004 | Story | Verify email address and reset a forgotten password | 8 | backend-agent | US-001 |
| ↳ US-004-T1 | Task | Implement POST /auth/verify-email with expiry/single-use checks | — | backend-agent | — |
| ↳ US-004-T2 | Task | Implement forgot-password and reset-password endpoints | — | backend-agent | — |
| ↳ US-004-T3 | Task | Send verification and reset emails via SendGrid | — | integration-agent | — |
| US-019 | Story | Search the food catalogue | 8 | integration-agent | US-022, US-021 |
| ↳ US-019-T1 | Task | Set up Meilisearch index and sync job | — | integration-agent | — |
| ↳ US-019-T2 | Task | Implement GET /foods/search endpoint | — | backend-agent | — |
| ↳ US-019-T3 | Task | Build search UI with empty-state messaging | — | frontend-agent | — |

## Sprint 4: Sprint 4

Capture body details, compute the calorie target, and support portion selection for logged foods.

Capacity: 20 points · Committed: 18

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-006 | Story | Enter body details after signup | 5 | frontend-agent | US-001, US-005 |
| US-007 | Story | Receive a calculated daily calorie target | 8 | backend-agent | US-006 |
| ↳ US-007-T1 | Task | Implement Mifflin-St Jeor + activity multiplier calculation service | — | backend-agent | — |
| ↳ US-007-T2 | Task | Implement GET/PUT /target endpoints | — | backend-agent | — |
| ↳ US-007-T3 | Task | Display calculated target with explanation | — | frontend-agent | — |
| US-020 | Story | Choose a portion with calories auto-filled | 5 | frontend-agent | US-019 |

## Sprint 5: Sprint 5

Add manual target override, safety floors, the not-medical-advice disclaimer, and settings review of body details.

Capacity: 20 points · Committed: 17

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-008 | Story | Override the calculated calorie target | 5 | backend-agent | US-007 |
| US-009 | Story | Be protected by a safe minimum calorie floor | 5 | backend-agent | US-007, US-008 |
| US-010 | Story | See the 'not medical advice' disclaimer | 2 | frontend-agent | US-007 |
| US-011 | Story | Review and update body details later | 5 | frontend-agent | US-006, US-007 |

## Sprint 6: Sprint 6

Ship today's log screen grouped by meal, the add-food flow, and the privacy notice.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-012 | Story | See today's log grouped by meal | 8 | frontend-agent | US-001, US-007 |
| ↳ US-012-T1 | Task | Implement GET /log endpoint with meal grouping and subtotals | — | backend-agent | — |
| ↳ US-012-T2 | Task | Build today's log screen UI | — | frontend-agent | — |
| ↳ US-012-T3 | Task | Add empty-state add-food actions per meal section | — | frontend-agent | — |
| US-013 | Story | See calories consumed, target and remaining | 3 | frontend-agent | US-012 |
| US-015 | Story | Add a food entry to a meal | 5 | frontend-agent | US-012, US-019, US-020 |
| US-028 | Story | Read the privacy notice and see my consent record | 3 | frontend-agent | US-005, US-011 |

## Sprint 7: Sprint 7

Round out logging with edit/delete, one-tap re-log, date navigation and simple history.

Capacity: 20 points · Committed: 20

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-014 | Story | Move between dates | 5 | frontend-agent | US-012, US-013 |
| US-016 | Story | Edit or delete a food entry | 5 | frontend-agent | US-015 |
| US-017 | Story | Re-log a previous meal in one tap | 5 | frontend-agent | US-015, US-016 |
| US-018 | Story | View a simple history of recent days | 5 | frontend-agent | US-014 |

## Sprint 8: Sprint 8

Add the plain-English meal estimator with editable results and graceful fallback, plus home-screen install.

Capacity: 20 points · Committed: 19

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-023 | Story | Describe a meal in plain English and get an estimate | 8 | integration-agent | US-012, US-015 |
| ↳ US-023-T1 | Task | Implement POST /estimate calling OpenAI | — | integration-agent | — |
| ↳ US-023-T2 | Task | Build estimate review screen | — | frontend-agent | — |
| ↳ US-023-T3 | Task | Handle empty/unrecognisable description path | — | backend-agent | — |
| US-024 | Story | Adjust an estimate before saving it | 5 | frontend-agent | US-023 |
| US-025 | Story | Cope gracefully when estimation is unavailable | 3 | frontend-agent | US-023 |
| US-030 | Story | Save the app to my phone's home screen | 3 | frontend-agent | US-012 |

## Sprint 9: Sprint 9

Deliver GDPR self-service export and account deletion, and polish responsive mobile layout.

Capacity: 20 points · Committed: 15

| Key | Type | Title | Points | Role | Depends on |
|---|---|---|---|---|---|
| US-026 | Story | Export my personal data | 5 | backend-agent | US-005, US-006, US-007, US-015, US-021 |
| US-027 | Story | Delete my account and data | 5 | backend-agent | US-026 |
| US-029 | Story | Use the app comfortably on a phone | 5 | frontend-agent | US-011, US-012, US-019 |

## Unscheduled

_Nothing unscheduled._
<!-- sdlc:auto:end sprints -->
