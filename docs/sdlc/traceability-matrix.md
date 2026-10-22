# Traceability Matrix

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start legend -->
Built from recorded links, not written by a model: requirement key → plan item (`source_stable_key`) → development ticket (`stable_key`) → the ticket's test steps → the last live deployment that started after the work finished.
<!-- sdlc:auto:end legend -->

<!-- sdlc:auto:start matrix -->
| Requirement | Epic | Title | Plan items | Sprint | Design | Code | Commits | Ticket | Tests | Deployed |
|---|---|---|---|---|---|---|---|---|---|---|
| US-001 | EPIC-001 | Sign up with email and password | US-001, US-001-T1, US-001-T2, US-001-T3 | 1 | POST /auth/signup, api, db, email, email_tokens, users, web | — | — | not started | not run | — |
| US-002 | EPIC-001 | Sign up and sign in with Google | US-002, US-002-T1, US-002-T2, US-002-T3 | 2 | GET /auth/google/callback, GET /auth/google/start, api, db, oauth, users, web | — | — | not started | not run | — |
| US-003 | EPIC-001 | Sign in and sign out | US-003 | 2 | POST /auth/login, POST /auth/logout, api, db, sessions, users, web | — | — | not started | not run | — |
| US-004 | EPIC-001 | Verify email address and reset a forgotten password | US-004, US-004-T1, US-004-T2, US-004-T3 | 3 | POST /auth/forgot-password, POST /auth/reset-password, POST /auth/verify-email, api, db, email, email_tokens, sessions, users, web | — | — | not started | not run | — |
| US-005 | EPIC-001 | Give explicit consent for storing body details and food logs | US-005 | 1 | POST /consent, api, db, users, web | — | — | not started | not run | — |
| US-006 | EPIC-002 | Enter body details after signup | US-006 | 4 | POST /body-details, api, body_details, db, web | — | — | not started | not run | — |
| US-007 | EPIC-002 | Receive a calculated daily calorie target | US-007, US-007-T1, US-007-T2, US-007-T3 | 4 | GET /target, PUT /target, api, body_details, calorie_targets, db, web | — | — | not started | not run | — |
| US-008 | EPIC-002 | Override the calculated calorie target | US-008 | 5 | PUT /target, api, calorie_targets, db, web | — | — | not started | not run | — |
| US-009 | EPIC-002 | Be protected by a safe minimum calorie floor | US-009 | 5 | PUT /target, api, calorie_targets, db | — | — | not started | not run | — |
| US-010 | EPIC-002 | See the 'not medical advice' disclaimer | US-010 | 5 | web | — | — | not started | not run | — |
| US-011 | EPIC-002 | Review and update body details later | US-011 | 5 | GET /target, PUT /body-details, api, body_details, calorie_targets, db, web | — | — | not started | not run | — |
| US-012 | EPIC-003 | See today's log grouped by meal | US-012, US-012-T1, US-012-T2, US-012-T3 | 6 | GET /log, api, db, food_entries, foods, web | — | — | not started | not run | — |
| US-013 | EPIC-003 | See calories consumed, target and remaining | US-013 | 6 | GET /log, api, calorie_targets, food_entries, web | — | — | not started | not run | — |
| US-014 | EPIC-003 | Move between dates | US-014 | 7 | GET /log, api, calorie_targets, food_entries, web | — | — | not started | not run | — |
| US-015 | EPIC-003 | Add a food entry to a meal | US-015 | 6 | POST /entries, api, db, food_entries, food_portions, foods, web | — | — | not started | not run | — |
| US-016 | EPIC-003 | Edit or delete a food entry | US-016 | 7 | DELETE /entries/{id}, PUT /entries/{id}, api, db, food_entries, web | — | — | not started | not run | — |
| US-017 | EPIC-003 | Re-log a previous meal in one tap | US-017 | 7 | GET /entries/recent, POST /entries, api, food_entries, web | — | — | not started | not run | — |
| US-018 | EPIC-003 | View a simple history of recent days | US-018 | 7 | GET /history, api, calorie_targets, food_entries, web | — | — | not started | not run | — |
| US-019 | EPIC-004 | Search the food catalogue | US-019, US-019-T1, US-019-T2, US-019-T3 | 3 | GET /foods/search, api, db, food_portions, foods, search, web | — | — | not started | not run | — |
| US-020 | EPIC-004 | Choose a portion with calories auto-filled | US-020 | 4 | GET /foods/{id}, api, db, food_portions, foods, web | — | — | not started | not run | — |
| US-021 | EPIC-004 | Create a custom food | US-021 | 2 | DELETE /foods/custom/{id}, POST /foods/custom, PUT /foods/custom/{id}, api, db, foods, web | — | — | not started | not run | — |
| US-022 | EPIC-004 | Have the catalogue seeded from USDA FoodData Central | US-022, US-022-T1, US-022-T2, US-022-T3 | 1 | api, db, food_portions, foods, usda, web | — | — | not started | not run | — |
| US-023 | EPIC-005 | Describe a meal in plain English and get an estimate | US-023, US-023-T1, US-023-T2, US-023-T3 | 8 | POST /estimate, ai, api, food_entries, web | — | — | not started | not run | — |
| US-024 | EPIC-005 | Adjust an estimate before saving it | US-024 | 8 | POST /entries, api, food_entries, web | — | — | not started | not run | — |
| US-025 | EPIC-005 | Cope gracefully when estimation is unavailable | US-025 | 8 | POST /estimate, ai, api, web | — | — | not started | not run | — |
| US-026 | EPIC-006 | Export my personal data | US-026 | 9 | POST /account/export, api, body_details, calorie_targets, db, food_entries, foods, users | — | — | not started | not run | — |
| US-027 | EPIC-006 | Delete my account and data | US-027 | 9 | DELETE /account, api, body_details, calorie_targets, db, food_entries, foods, sessions, users | — | — | not started | not run | — |
| US-028 | EPIC-006 | Read the privacy notice and see my consent record | US-028 | 6 | GET /privacy/consent, api, users, web | — | — | not started | not run | — |
| US-029 | EPIC-007 | Use the app comfortably on a phone | US-029 | 9 | web | — | — | not started | not run | — |
| US-030 | EPIC-007 | Save the app to my phone's home screen | US-030 | 8 | web | — | — | not started | not run | — |
| US-031 | EPIC-007 | Use the whole app free of charge | US-031 | 1 | — | — | — | not started | not run | — |
<!-- sdlc:auto:end matrix -->
