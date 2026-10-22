# Development

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
### DEV-PLAN-OVERVIEW · approved · source: plan:overview

Development plan is approved at version 1. Work is organized into 9 sprints of capacity 20 points each, spanning 7 epics and user stories US-001 through US-031, each decomposed into backend/frontend/integration tasks.

Plan status: approved, version 1. Sprint sequence and goals: 1) identity foundation (email/password signup + consent) and USDA catalogue seed kickoff; 2) Google sign-in, session management, custom foods; 3) account recovery, Meilisearch search; 4) body details, calorie target, portion selection; 5) target override, safety floor, medical disclaimer, body-details review; 6) today's log screen, add-food flow, privacy notice; 7) edit/delete, re-log, date navigation, history; 8) plain-English meal estimator with fallback, home-screen install; 9) GDPR export/deletion, responsive mobile polish.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005, EPIC-006, EPIC-007

- ⚠ No execution/completion status (not-started/in-progress/done) reported for any sprint or backlog item
- ⚠ No code commits, pull requests, or build artifacts provided in this input
- ⚠ No test results included for this phase
- ⚠ Explicit sprint-to-story-id assignment is not given in the input; associations below are inferred only from matching sprint goal text to story titles and should be confirmed

### DEV-SPRINT-1 · approved · source: plan:Sprint 1

Sprint 1 goal: stand up core identity foundation (email/password signup with consent) and begin the USDA food catalogue seed.

Capacity: 20 points. Goal text: 'Stand up core identity foundation: email/password signup with consent, and kick off the USDA food catalogue seed.' Inferred relevant stories: US-001 (email/password signup), US-005 (consent for storing body details/food logs), US-022 (USDA catalogue seed, likely import-script kickoff).

**Related:** US-001, US-005, US-022

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-2 · approved · source: plan:Sprint 2

Sprint 2 goal: complete Google sign-in, session management, and let users create custom foods.

Capacity: 20 points. Goal text: 'Complete alternate sign-in (Google), session management, and let users create their own custom foods.' Inferred relevant stories: US-002 (Google sign up/sign in), US-003 (sign in and sign out / session), US-021 (create a custom food).

**Related:** US-002, US-003, US-021

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-3 · approved · source: plan:Sprint 3

Sprint 3 goal: finish account recovery flows and make the food catalogue fully searchable via Meilisearch.

Capacity: 20 points. Goal text: 'Finish account recovery flows and make the food catalogue fully searchable via Meilisearch.' Inferred relevant stories: US-004 (verify email / reset password), US-019 (search the food catalogue).

**Related:** US-004, US-019

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-4 · approved · source: plan:Sprint 4

Sprint 4 goal: capture body details, compute the daily calorie target, and support portion selection for logged foods.

Capacity: 20 points. Goal text: 'Capture body details, compute the calorie target, and support portion selection for logged foods.' Inferred relevant stories: US-006 (enter body details), US-007 (calculated daily calorie target), US-020 (choose a portion with calories auto-filled).

**Related:** US-006, US-007, US-020

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-5 · approved · source: plan:Sprint 5

Sprint 5 goal: add manual calorie-target override, a safety floor, the not-medical-advice disclaimer, and let users review body details in settings.

Capacity: 20 points. Goal text: 'Add manual target override, safety floors, the not-medical-advice disclaimer, and settings review of body details.' Inferred relevant stories: US-008 (override calculated target), US-009 (safe minimum calorie floor), US-010 (not-medical-advice disclaimer), US-011 (review/update body details later).

**Related:** US-008, US-009, US-010, US-011

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-6 · approved · source: plan:Sprint 6

Sprint 6 goal: ship today's log screen grouped by meal, the add-food flow, and the privacy notice.

Capacity: 20 points. Goal text: 'Ship today's log screen grouped by meal, the add-food flow, and the privacy notice.' Inferred relevant stories: US-012 (today's log grouped by meal), US-015 (add a food entry to a meal), US-028 (privacy notice and consent record).

**Related:** US-012, US-015, US-028

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-7 · approved · source: plan:Sprint 7

Sprint 7 goal: round out logging with edit/delete, one-tap re-log, date navigation, and simple history.

Capacity: 20 points. Goal text: 'Round out logging with edit/delete, one-tap re-log, date navigation and simple history.' Inferred relevant stories: US-016 (edit or delete a food entry), US-017 (re-log a previous meal in one tap), US-014 (move between dates), US-018 (view a simple history of recent days).

**Related:** US-014, US-016, US-017, US-018

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-8 · approved · source: plan:Sprint 8

Sprint 8 goal: add the plain-English meal estimator with editable results and graceful fallback, plus home-screen install support.

Capacity: 20 points. Goal text: 'Add the plain-English meal estimator with editable results and graceful fallback, plus home-screen install.' Inferred relevant stories: US-023 (describe a meal in plain English and get an estimate), US-024 (adjust an estimate before saving), US-025 (cope gracefully when estimation is unavailable), US-030 (save the app to the phone's home screen).

**Related:** US-023, US-024, US-025, US-030

- ⚠ No confirmation these stories were actually completed within this sprint window

### DEV-SPRINT-9 · approved · source: plan:Sprint 9

Sprint 9 goal: deliver GDPR self-service data export and account deletion, and polish the responsive mobile layout.

Capacity: 20 points. Goal text: 'Deliver GDPR self-service export and account deletion, and polish responsive mobile layout.' Inferred relevant stories: US-026 (export my personal data), US-027 (delete my account and data), US-029 (use the app comfortably on a phone). Goal explicitly frames US-026/US-027 as GDPR-related self-service rights.

**Related:** US-026, US-027, US-029

- ⚠ No confirmation these stories were actually completed within this sprint window

### EPIC-001 · approved · source: plan:EPIC-001

Personal Account, Sign-In and Consent epic covers email/password and Google signup, sign-in/out, account recovery, and explicit consent capture.

US-001 Sign up with email and password (8 pts, backend-agent): tasks - create users/email_tokens schema & migrations (backend-agent); implement POST /auth/signup (backend-agent); wire verification email via SendGrid (integration-agent). US-002 Sign up/sign in with Google (8 pts, integration-agent): tasks - implement GET /auth/google/start and /auth/google/callback (integration-agent); handle account creation/linking for Google email match (backend-agent); add first-time consent gate after Google callback (frontend-agent). US-003 Sign in and sign out (5 pts, backend-agent): no sub-tasks listed. US-004 Verify email address and reset forgotten password (8 pts, backend-agent): tasks - implement POST /auth/verify-email with expiry/single-use checks (backend-agent); implement forgot-password/reset-password endpoints (backend-agent); send verification/reset emails via SendGrid (integration-agent). US-005 Give explicit consent for storing body details and food logs (3 pts, backend-agent): no sub-tasks listed.

**Related:** US-001, US-002, US-003, US-004, US-005

- ⚠ No completion status reported for any story or task in this epic

### EPIC-002 · approved · source: plan:EPIC-002

Body Details and Daily Calorie Target epic covers entering body details, calculating and overriding the calorie target, enforcing a safety floor, and the medical disclaimer.

US-006 Enter body details after signup (5 pts, frontend-agent): no sub-tasks listed. US-007 Receive a calculated daily calorie target (8 pts, backend-agent): tasks - implement Mifflin-St Jeor + activity multiplier calculation service (backend-agent); implement GET/PUT /target endpoints (backend-agent); display calculated target with explanation (frontend-agent). US-008 Override the calculated calorie target (5 pts, backend-agent): no sub-tasks listed. US-009 Be protected by a safe minimum calorie floor (5 pts, backend-agent): no sub-tasks listed. US-010 See the 'not medical advice' disclaimer (2 pts, frontend-agent): no sub-tasks listed. US-011 Review and update body details later (5 pts, frontend-agent): no sub-tasks listed.

**Related:** US-006, US-007, US-008, US-009, US-010, US-011

- ⚠ No completion status reported for any story or task in this epic

### EPIC-003 · approved · source: plan:EPIC-003

Daily Food Log and History epic covers today's meal-grouped log, calorie summary, date navigation, adding/editing/deleting entries, re-logging, and recent history.

US-012 See today's log grouped by meal (8 pts, frontend-agent): tasks - implement GET /log endpoint with meal grouping and subtotals (backend-agent); build today's log screen UI (frontend-agent); add empty-state add-food actions per meal section (frontend-agent). US-013 See calories consumed, target and remaining (3 pts, frontend-agent): no sub-tasks listed. US-014 Move between dates (5 pts, frontend-agent): no sub-tasks listed. US-015 Add a food entry to a meal (5 pts, frontend-agent): no sub-tasks listed. US-016 Edit or delete a food entry (5 pts, frontend-agent): no sub-tasks listed. US-017 Re-log a previous meal in one tap (5 pts, frontend-agent): no sub-tasks listed. US-018 View a simple history of recent days (5 pts, frontend-agent): no sub-tasks listed.

**Related:** US-012, US-013, US-014, US-015, US-016, US-017, US-018

- ⚠ No completion status reported for any story or task in this epic

### EPIC-004 · approved · source: plan:EPIC-004

Food Catalogue, Search and Custom Foods epic covers searching the catalogue, portion selection, custom foods, and the USDA FoodData Central seed import.

US-019 Search the food catalogue (8 pts, integration-agent): tasks - set up Meilisearch index and sync job (integration-agent); implement GET /foods/search endpoint (backend-agent); build search UI with empty-state messaging (frontend-agent). US-020 Choose a portion with calories auto-filled (5 pts, frontend-agent): no sub-tasks listed. US-021 Create a custom food (5 pts, backend-agent): no sub-tasks listed. US-022 Have the catalogue seeded from USDA FoodData Central (8 pts, integration-agent): tasks - build USDA FoodData Central import script (integration-agent); implement idempotent upsert logic (backend-agent); add source attribution to food detail display (frontend-agent).

**Related:** US-019, US-020, US-021, US-022

- ⚠ No completion status reported for any story or task in this epic

### EPIC-005 · approved · source: plan:EPIC-005

Plain-English Meal Estimation epic covers describing a meal in free text to get an AI-generated calorie estimate, adjusting it, and handling unavailability gracefully.

US-023 Describe a meal in plain English and get an estimate (8 pts, integration-agent): tasks - implement POST /estimate calling OpenAI (integration-agent); build estimate review screen (frontend-agent); handle empty/unrecognisable description path (backend-agent). US-024 Adjust an estimate before saving it (5 pts, frontend-agent): no sub-tasks listed. US-025 Cope gracefully when estimation is unavailable (3 pts, frontend-agent): no sub-tasks listed.

**Related:** US-023, US-024, US-025

- ⚠ No completion status reported for any story or task in this epic

### EPIC-006 · approved · source: plan:EPIC-006

Privacy, Data Export and Account Deletion epic covers self-service data export, account/data deletion, and the privacy notice with consent record.

US-026 Export my personal data (5 pts, backend-agent): no sub-tasks listed. US-027 Delete my account and data (5 pts, backend-agent): no sub-tasks listed. US-028 Read the privacy notice and see my consent record (3 pts, frontend-agent): no sub-tasks listed. Sprint 9's goal text frames US-026/US-027 explicitly as GDPR self-service export and deletion.

**Related:** US-026, US-027, US-028

- ⚠ No completion status reported for any story or task in this epic
- ⚠ Input does not cite a formal data-protection/compliance skill module beyond the plain mention of 'GDPR' in the sprint goal text; recommend a human confirm whether a full GDPR control mapping (e.g. lawful basis, data retention limits, breach notification) has been addressed anywhere outside this plan

### EPIC-007 · approved · source: plan:EPIC-007

Mobile-First Web Delivery epic covers comfortable phone usage, home-screen installability, and free-of-charge access to the whole app.

US-029 Use the app comfortably on a phone (5 pts, frontend-agent): no sub-tasks listed. US-030 Save the app to my phone's home screen (3 pts, frontend-agent): no sub-tasks listed. US-031 Use the whole app free of charge (1 pt, backend-agent): no sub-tasks listed.

**Related:** US-029, US-030, US-031

- ⚠ No completion status reported for any story or task in this epic
<!-- sdlc:auto:end narrative -->
