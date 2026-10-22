# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-SCOPE-001 · draft · source: requirements:scope

Defines the v1 delivery scope for the calorie app: a lightweight, mobile-first web app for individual self-tracking, free of charge, with explicit exclusions.

Requirements version 1, delivery scope 'lightweight'.
In-scope capabilities: personal signup/sign-in; body details intake; calculated daily calorie target with manual override; today's log grouped by breakfast/lunch/dinner/snacks; consumed/target/remaining display; food search with portion pick; add/edit/delete entries; date navigation; custom foods; 'not medical advice' disclaimer; signup consent tick; Mifflin-St Jeor calculation with activity multiplier and target-weight deficit; safety floors (~1200 kcal women, ~1500 kcal men) with refusal of extreme targets; USDA-seeded catalogue with thin branded coverage; free to use, no billing; mobile-first web app installable to home screen, no app store build.
Explicitly out of scope: coach/clinician/employer dashboards; barcode scanning; photo-of-meal recognition; weight-trend charting; macro (protein/carb/fat) breakdown; daily email reminders; native iOS/Android apps; fitness wearable/health-platform sync; social features (friends, sharing, streaks, leaderboards).

- ⚠ No explicit approval date or approver name is recorded for the requirements set itself (only the technology stack and runtime services are explicitly marked 'approved by product owner').

### REQ-STACK-001 · approved · source: requirements:technology_stack

Product owner approved an exact technology stack: FastAPI 0.115 backend, AWS cloud, PostgreSQL 16 database, React 19 frontend. Design must use exactly these, no substitutions.

Backend: FastAPI 0.115 - chosen so the safety-critical logic (Mifflin-St Jeor target calculation, activity multiplier, 1200/1500 kcal floors, refusal of extreme targets) lives in one auditable place; also fronts the OpenAI call for plain-English estimation and handles the USDA seed import.
Cloud: AWS - supports worldwide signups from one primary market at low-thousands scale with GDPR-level standards; allows deliberate EU-region placement of database and backups, managed Postgres, and room to grow without re-platforming.
Database: PostgreSQL 16 - food entries, body details, daily targets and the USDA-seeded food table are relational; supports one cascading delete for self-service account deletion and single-query data export.
Frontend: React 19.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005, EPIC-006, EPIC-007

- ⚠ EPIC-001 and EPIC-007 narrative text ('Assumptions carried from discovery') references authentication and delivery being built on a 'Next.js/React' stack, but the product-owner-approved stack names only 'React 19' with no mention of Next.js. This is a discrepancy between two inputs and should be confirmed before architecture/design proceeds.

### REQ-SERVICES-001 · approved · source: requirements:runtime_services

Product owner approved five runtime services/integrations and the API keys they require.

AI provider: OpenAI (SaaS, env OPENAI_API_KEY) - turns plain-English meal descriptions into an editable calorie breakdown, covering home-cooked meals absent from the food database.
Search: Meilisearch (container getmeili/meilisearch:v1.10.3) - typo- and partial-match-tolerant search across the USDA seed and user-created foods.
Email: SendGrid (SaaS, env SENDGRID_API_KEY) - sends verification and password-reset emails for email/password sign-in.
Auth: Google OAuth (custom, env GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, OAUTH_REDIRECT_URI) - one-tap Google sign-in alongside email/password.
Nutrition data: USDA FoodData Central (custom, env FDC_API_KEY) - seeds the searchable food database; free and authoritative but thin on branded packaged products.
Full API key list required: FDC_API_KEY, GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, OAUTH_REDIRECT_URI, OPENAI_API_KEY, SENDGRID_API_KEY.

**Related:** EPIC-001, EPIC-004, EPIC-005

### EPIC-001 · draft · source: requirements:EPIC-001

Personal account signup/sign-in and GDPR-level consent capture. One person, one account, no coach/clinician/employer/admin roles.

US-001 (critical) Sign up with email and password: valid email+password+consent creates account, signs in, routes to body details; duplicate email refused with sign-in/reset link; malformed email or weak password shows field-level error, nothing saved; verification email sent via SendGrid on creation and confirmed on screen.
US-002 (high) Google sign-up/sign-in: Continue with Google creates or signs in to matching account, routes to body details or today's log as appropriate; first-time Google users must tick data consent before continuing; abandoned/denied consent returns user to sign-in with explanation and no account created.
US-003 (critical) Sign in/out: correct credentials sign in to today's log; incorrect password shows a generic 'email or password is incorrect' message (does not reveal account existence); sign-out ends session; unauthenticated access to any logged-in screen redirects to sign-in and never shows another user's data.
US-004 (high) Verify email / reset password: valid verification link marks email verified; expired/used links rejected with re-request offer; reset request shows identical confirmation whether or not the address exists, email sent only if it does; valid reset sets new password meeting strength rule and invalidates existing sessions.
US-005 (critical) Explicit consent at signup: unticked consent box names exactly what is stored (height, weight, age, sex, activity level, target weight, food logs) with privacy/terms links; submitting without ticking is refused and nothing is stored; consent plus wording version and timestamp are recorded and shown back in settings.

**Related:** US-001, US-002, US-003, US-004, US-005, AC-001, AC-018

- ⚠ Narrative assumption text names a 'Next.js/React' stack, which conflicts with the approved stack of plain React 19 (see REQ-STACK-001) - needs resolution before build.

### EPIC-002 · draft · source: requirements:EPIC-002

Body details intake and calculated daily calorie target, with manual override and safety guardrails. Positioned as general wellness, not a medical device.

US-006 (critical) Enter body details: height, current weight, age, sex, activity level (named list), target weight, metric/imperial units; empty/out-of-range fields block save and calculation.
US-007 (critical) Calculated target: Mifflin-St Jeor BMR by sex/weight/height/age, multiplied by activity factor, adjusted by deficit/surplus from current-vs-target weight (deficit if target lower, surplus if higher, maintenance if equal); shown with plain-language estimate note; recalculates when body details change.
US-008 (high) Manual override: user can set own target within accepted range, labelled 'set by you', used everywhere; can revert to calculated; manual target survives body-detail edits with a prompt that a new calculated value is available.
US-009 (critical) Safety floor: calculated targets below ~1200 kcal (women)/~1500 kcal (men) are raised to the floor with an explanation; manual overrides below the floor are refused, previous target kept; extreme inputs (implausible target weight, extreme calorie values) are refused with an explanation and a suggestion to consult a professional; floor/extreme-bound values are an unconfirmed discovery assumption, held in adjustable configuration and flagged for pre-launch review (AC-033).
US-010 (high) 'Not medical advice' disclaimer: shown on first target display and reachable in one tap from day screen/target settings; no screen offers diagnosis, treatment or clinical claims.
US-011 (medium) Review/update body details: settings shows stored details and whether target is calculated or manual; valid edits recalculate (subject to floor) without altering already-logged days' targets; invalid edits are refused and prior details kept; current weight is input-only, no history/chart.

**Related:** US-006, US-007, US-008, US-009, US-010, US-011

- ⚠ Exact floor values (1200/1500 kcal) and extreme-value bounds are explicitly flagged in the input as discovery assumptions, not confirmed clinical rules, pending review before launch.

### EPIC-003 · draft · source: requirements:EPIC-003

Main daily food log screen, grouped by meal, with consumed/target/remaining totals, date navigation, entry edit/delete, one-tap re-log, and simple recent-days history. Calories only, no macros or charts.

US-012 (critical) Today's log grouped by meal: breakfast/lunch/dinner/snacks in that order is the first screen after setup; each entry shows name, portion, calories; each section has a subtotal; empty sections show an add-food action; 0 calories shown when nothing logged; no protein/carb/fat ever shown.
US-013 (critical) Consumed/target/remaining: shown together, remaining = target - consumed, updates immediately on any change; over-target shown as negative/'over by' in a distinct, non-judgemental style.
US-014 (high) Date navigation: previous-day control or date picker shows that date's own entries/totals/target; edits on a past date save to that date; future dates are blocked; a single control returns to today; non-today dates are clearly labelled.
US-015 (critical) Add a food entry: add-food on a meal section pre-selects that meal/date and offers both search and plain-English description; confirming saves the entry with calories to the chosen meal/date; meal selection can be changed before saving.
US-016 (critical) Edit/delete an entry: editing can change portion, calories, or meal (moves sections, recalculates totals); delete requires confirmation, cancel leaves it unchanged; another user's entry is never returned when requested directly.
US-017 (high) One-tap re-log: add flow shows a short list of recently logged foods/portions; tapping one copies it into the current meal/date and updates totals; editing the new copy leaves the original untouched.
US-018 (medium) Simple recent-days history: list of most recent days with date, consumed, applicable target, and under/over status; tapping a day opens its full log; days with no entries are shown as such, not omitted or shown misleadingly as zero; no charts, trends, averages, or weight history.

**Related:** US-012, US-013, US-014, US-015, US-016, US-017, US-018

### EPIC-004 · draft · source: requirements:EPIC-004

Searchable food catalogue seeded from USDA FoodData Central, extended by user-created custom foods; thin branded/packaged coverage accepted for launch.

US-019 (critical) Search: 2+ characters returns matches from seeded catalogue plus the user's own custom foods (clearly marked), each with name/default portion/calories, ordered by closeness/common use; no matches offer plain-English description and custom-food creation, with a note that packaged-product coverage is limited.
US-020 (critical) Portion pick: available portion options (grams, millilitres, household portions) listed with a sensible default; changing portion/quantity recalculates calories immediately; zero/negative/non-numeric quantity blocks save with a validation message.
US-021 (high) Custom foods: name + portion description + calories saved to the user's account and immediately searchable, marked as their own; blank name or invalid calories refused with field-level error; user can edit/delete their custom foods, deletions leave prior logged entries' recorded calories intact; custom foods never appear in another user's search.
US-022 (high) USDA seeding: seed process loads names/calories/at least one portion per food into an empty database and reports record count; re-running updates existing records rather than duplicating and leaves custom foods untouched; seeded food detail view carries required source attribution; thin branded coverage documented and surfaced via the search empty state.

**Related:** US-019, US-020, US-021, US-022

- ⚠ Branded/packaged product coverage gap is an accepted risk for launch (per NFR-013) - confirm this remains acceptable as the catalogue is reviewed pre-launch.

### EPIC-005 · draft · source: requirements:EPIC-005

Plain-English meal estimation via a hosted LLM (OpenAI), always presented as an editable, unsaved estimate, with graceful fallback when unavailable.

US-023 (critical) Describe and estimate: user types a plain-English description (e.g. 'two eggs and toast with butter'); app returns a breakdown of identified foods, assumed portions, calorie figures and a total, clearly labelled as an estimate that may be inaccurate, nothing saved yet; empty/unrecognisable descriptions are reported back with an invitation to rephrase or use search, nothing saved; only the typed description (never another user's data) is sent, per the privacy notice.
US-024 (critical) Adjust before saving: user can change any line's portion/calories or remove a line, with the total recalculating live; confirming saves the items as normal, later-editable entries; cancelling/navigating away saves nothing.
US-025 (high) Graceful failure: service error, timeout, or missing OPENAI_API_KEY shows a plain-language 'unavailable' message with retry/search/custom-food options; in-progress requests show a progress indicator, remain cancellable, and never produce a raw error, a partial entry, or a zero-calorie entry in the log.

**Related:** US-023, US-024, US-025

- ⚠ Dependency on a hosted LLM introduces per-call cost and latency, explicitly called out in the input as an assumption to be validated - no cost/latency budget or fallback SLA is specified.

### EPIC-006 · draft · source: requirements:EPIC-006

GDPR-level privacy handling applied uniformly worldwide: self-service data export and self-service account deletion, plus visible consent and privacy records.

US-026 (high) Export data: from settings, a machine-readable file with account details, consent record, body details, calorie target history, custom foods and all log entries, containing only the requester's own data; if not immediate, user is told it is being prepared and how it will be delivered.
US-027 (high) Delete account: user is shown exactly what will be deleted and must confirm explicitly; on confirming, account, body details, target, custom foods and all entries are removed, session ends, credentials stop working; cancelling deletes nothing; the shared USDA-seeded catalogue remains intact afterward.
US-028 (medium) Privacy notice and consent record: notice (reachable from signup and settings) explains what is stored, why, and that plain-English descriptions are processed by a third-party AI service; settings shows consent date, wording version accepted, and links to export/delete.

**Related:** US-026, US-027, US-028

### EPIC-007 · draft · source: requirements:EPIC-007

Mobile-first responsive web delivery, installable to the home screen, with no payments/subscriptions/billing anywhere.

US-029 (critical) Comfortable phone use: day screen, search, add flow, history and settings laid out for phone width with no horizontal scrolling, primary actions within thumb reach; reflows sensibly on tablet/desktop; numeric fields trigger the numeric keyboard without obscuring the focused field.
US-030 (medium) Save to home screen: browser add-to-home-screen installs with its own name/icon in a standalone window; opening from the home screen preserves a valid session and lands on today's log; no native iOS/Android build is published, all guidance points to the web address.
US-031 (medium) Free throughout: every feature (search, estimation, custom foods, history, export) is usable with no payment, plan, trial expiry or upgrade prompt; no payment provider or billing record is stored; the free model is flagged as a first-release assumption for later review.

**Related:** US-029, US-030, US-031

- ⚠ Narrative assumption text again references a 'Next.js/React' stack, which conflicts with the approved React-19-only stack (see REQ-STACK-001).
- ⚠ The free-of-charge model is explicitly flagged in the input as an assumption for this release only, to be reviewed in a later release.

### REQ-NFR-001 · draft · source: requirements:non_functional_requirements

25 non-functional requirements spanning security, privacy, compliance, maintainability, performance, usability, availability, scalability and compatibility.

Security: NFR-001 per-account data isolation everywhere; NFR-002 sign-in/reset do not reveal account existence; NFR-003 password strength rule plus session invalidation on reset; NFR-004 verification/reset links are time-limited and single-use; NFR-005 no payment/billing data stored.
Privacy: NFR-006 consent recorded (wording version + timestamp) before any body/food data is stored, including Google sign-up; NFR-007 account/body/food data and backups deliberately held in an EU region; NFR-008 only the typed meal description is sent to the third-party AI service, disclosed in the privacy notice.
Compliance: NFR-009 GDPR-level baseline for every user worldwide (notice, export, self-service deletion); NFR-010 'not medical advice' disclaimer present/reachable in one tap wherever a target/estimate/history figure appears, no diagnostic or clinical claims; NFR-011 USDA attribution shown on food detail; NFR-012 (accepted risk) HIPAA-style clinical controls not applied, on the stated assumption no clinician/insurer/covered entity is involved - recorded for review; NFR-013 (accepted risk) thin branded/packaged coverage at launch, documented and surfaced in the search empty state.
Maintainability: NFR-014 safety floors/extreme bounds held in adjustable configuration, flagged as an unconfirmed assumption pending review; NFR-015 all target-calculation rules (Mifflin-St Jeor, activity multiplier, deficit/surplus, floors, extreme refusal) implemented once on the server; NFR-016 past logged days keep their historical target/custom-food calorie figures even after later edits or deletions; NFR-017 catalogue import is safely re-runnable (updates, no duplicates, custom foods untouched, record count reported).
Performance: NFR-018 totals and recalculated calorie figures update instantly without page reload.
Usability: NFR-019 forgiving search (typo-tolerant, 2-character minimum, relevance-ordered); NFR-021 visible, cancellable progress indicator for slow estimation calls; NFR-022 calm, non-judgemental wording for over-target, floored/refused targets, and errors (no raw technical error text); NFR-023 one-handed phone usability with correct numeric keyboards.
Availability: NFR-020 logging still works (via search/custom food) when estimation is down, with no partial/zero-calorie entries written.
Compatibility: NFR-024 consistent reflow across phone/tablet/desktop; installable to home screen with standalone launch.
Scalability: NFR-025 sized for low thousands of worldwide users on managed hosting that can grow without re-platforming.

**Related:** US-003, US-004, US-005, US-007, US-009, US-010, US-011, US-013, US-014, US-016, US-019, US-020, US-021, US-022, US-023, US-024, US-025, US-026, US-027, US-028, US-029, US-030, US-031

- ⚠ NFR-012 (HIPAA non-applicability) and NFR-014 (unconfirmed safety-floor values) are both explicitly recorded in the input as assumptions pending review before launch - no review outcome is recorded yet.
- ⚠ NFR-013 (thin branded/packaged coverage) is an accepted usability risk with no stated mitigation timeline beyond the search empty-state messaging.
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-10-22

**Capabilities in scope:** Personal account signup and sign-in, Body details intake (height, weight, age, sex, activity level, target weight), Calculated daily calorie target with manual override, Today's log grouped into breakfast, lunch, dinner and snacks, Calories consumed, target and remaining on the day screen, Food search and portion pick with calories auto-filled, Add, edit and delete food entries, Date navigation between past and current days, User-created custom foods, "Not medical advice" disclaimer on calorie targets, Consent tick at signup for storing body details and food logs, Mifflin-St Jeor calculation with activity multiplier and target-weight deficit, Safety floor of ~1200 kcal (women) and ~1500 (men), extreme targets refused, Food catalogue seeded from USDA FoodData Central, thin branded coverage, Free to use with no payments, subscriptions or billing, Mobile-first web app saved to the phone home screen, no app store build

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | FastAPI | The parts of this product that must be right — the Mifflin-St Jeor target calculation, the activity multiplier, the 1200/1500 kcal safety floors and the refusal of extreme targets — belong in one auditable place, not scattered through screens. FastAPI also sits naturally in front of the OpenAI call that turns 'two eggs and toast with butter' into an editable estimate, and handles the USDA seed import comfortably. |
| cloud | AWS | Signups are open worldwide from one primary market at low thousands of users with no deadline pressure, and you've committed to GDPR-level standards — AWS lets you place the database and backups in an EU region deliberately, run managed Postgres so nobody is patching servers, and grow without re-platforming if the app takes off. |
| database | PostgreSQL | Food entries, body details, daily targets and the USDA-seeded food table are plainly tabular and related — exactly what this is for. Your GDPR commitments get easier too: self-service deletion becomes one cascading delete in one database rather than chasing a person's data across systems, and data export is a single query. |
| frontend | React | — |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Personal Account, Sign-In and Consent

Self-serve signup and sign-in for individuals tracking themselves. One person, one account, their own food log — there are no coach, clinician, employer or admin roles in this release. Supports email-and-password plus Google sign-in, with transactional email for verification and password reset. Signups are open worldwide with no geo-restriction, and the app is built to GDPR-level standards: explicit consent at signup for storing body details and food logs.

Assumptions carried from discovery: authentication is email-and-password plus Google sign-in on a Next.js/React stack with PostgreSQL, and transactional email is used for verification and password reset. Scale is assumed at low thousands of users with no unusual performance or deadline constraint. The app is free to use with no payments, subscriptions or billing in this release, so there is no paywall, plan or upgrade path anywhere in signup.

### US-001: Sign up with email and password

**Priority:** critical

> As a new visitor who wants to track my calories, I want to create an account with my email address and a password, so that my food log and calorie target are saved and available whenever I come back.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | I am on the signup screen and have not used my email address before | I enter a valid email address and a password meeting the stated minimum strength rule, tick the consent box and submit | an account is created, I am signed in, and I am taken to the body details intake step |
| AC-002 | I am on the signup screen | I submit an email address that already has an account | signup is refused with a message telling me the address is already registered and offering a link to sign in or reset my password, and no second account is created |
| AC-003 | I am on the signup screen | I submit a malformed email address or a password that does not meet the minimum strength rule | the form shows a field-level error explaining what is wrong and no account is created |
| AC-004 | I have just created an account with email and password | the account is created | a verification email is sent to my address via the transactional email service and the app shows me that verification has been sent |

### US-002: Sign up and sign in with Google

**Priority:** high

> As a new or returning user, I want to use my Google account instead of a separate password, so that I can get in quickly on my phone without remembering another password.

| ID | Given | When | Then |
|---|---|---|---|
| AC-005 | I am on the signup or sign-in screen | I choose Continue with Google and complete the Google consent flow | an account is created or I am signed in to the existing account matching that Google email, and I land on the body details step if I have not completed it or on today's log if I have |
| AC-006 | I am signing up with Google for the first time | I return from Google before any body or food data is stored | I am asked to tick the consent for storing body details and food logs before I can continue to the body details step |
| AC-007 | I abandon or deny the Google consent screen | I am returned to the app | I am shown the sign-in screen with a message that Google sign-in was not completed and no account is created |

### US-003: Sign in and sign out

**Priority:** critical

> As a returning user, I want to sign in with my credentials and sign out when I am done, so that only I can see and change my food log.

| ID | Given | When | Then |
|---|---|---|---|
| AC-008 | I have a verified account | I enter my correct email and password | I am signed in and taken to today's log |
| AC-009 | I have an account | I enter an incorrect password | I am not signed in and I see a generic 'email or password is incorrect' message that does not reveal whether the email exists |
| AC-010 | I am signed in | I choose Sign out | my session is ended and visiting any logged-in screen redirects me to the sign-in screen |
| AC-011 | I am not signed in | I request a URL for today's log, history, settings or any food entry | I am redirected to sign in and no other user's data is ever shown |

### US-004: Verify email address and reset a forgotten password

**Priority:** high

> As a user who has forgotten my password or not yet verified my email, I want to verify my address and set a new password by email, so that I can regain access to my own account without contacting anyone.

| ID | Given | When | Then |
|---|---|---|---|
| AC-012 | I have received a verification email | I open the verification link while it is still valid | my email address is marked verified and I am told verification succeeded |
| AC-013 | I open a verification or reset link that has expired or already been used | the app processes it | I am told the link is no longer valid and offered a way to request a new one |
| AC-014 | I am on the sign-in screen and have forgotten my password | I enter my email address and request a reset | the app shows the same confirmation message whether or not the address exists, and a reset email is sent only if an account exists |
| AC-015 | I open a valid password reset link | I set a new password meeting the strength rule | my password is changed, any existing sessions are invalidated, and I can sign in with the new password |

### US-005: Give explicit consent for storing body details and food logs

**Priority:** critical

> As a person signing up anywhere in the world, I want to be told clearly what personal data the app stores and to consent before it does, so that I am in control of my health-related data to a GDPR-level standard.

| ID | Given | When | Then |
|---|---|---|---|
| AC-016 | I am on the signup screen | the screen loads | an unticked consent checkbox is shown stating that the app will store my body details (height, weight, age, sex, activity level, target weight) and my food logs, with links to the privacy notice and terms |
| AC-017 | the consent checkbox is unticked | I attempt to submit signup | signup is refused with a message that consent is required, and no account, body details or food data are stored |
| AC-018 | I tick the consent box and complete signup | the account is created | the consent, its wording version and the date and time it was given are recorded against my account and are visible to me in settings |

## EPIC-002: Body Details and Daily Calorie Target

Capturing the user's body details and turning them into a daily calorie target, with a manual override and safety guardrails.

Assumptions carried from discovery and to be validated: the target is computed with the Mifflin-St Jeor BMR equation multiplied by an activity factor and adjusted by a deficit or surplus derived from the user's target weight; targets are floored at roughly 1200 kcal/day for women and 1500 kcal/day for men, and extreme targets are refused; the app is positioned as a general wellness product, not a medical device, and HIPAA is assumed not to apply because no clinician, insurer or covered entity is involved; current weight is captured as an input to the calculation only and is deliberately not tracked over time (weight-trend tracking is out of scope).

### US-006: Enter body details after signup

**Priority:** critical

> As a new user, I want to enter my height, current weight, age, sex, activity level and target weight, so that the app can work out a daily calorie target that fits me.

| ID | Given | When | Then |
|---|---|---|---|
| AC-019 | I have just completed signup and consented | I am shown the body details step | I am asked for height, current weight, age, sex, activity level (as a short list of named levels) and target weight, with metric and imperial unit choices for height and weight |
| AC-020 | I am on the body details step | I submit with a required field empty or with a value outside the accepted range for that field (for example a negative age or an implausible height) | the form shows a field-level error, no target is calculated and nothing is saved |
| AC-021 | I have entered valid body details | I submit the form | my details are saved against my account, a daily calorie target is calculated and I am shown the target with its explanation before continuing to today's log |
| AC-022 | I have saved my body details | I view the app's history or any other screen | my current weight appears only as a stored input I can edit in settings, with no weight history, series or chart anywhere in the app |

### US-007: Receive a calculated daily calorie target

**Priority:** critical

> As a user who has entered body details, I want the app to calculate a daily calorie target from them, so that I have a number to log against without working it out myself.

| ID | Given | When | Then |
|---|---|---|---|
| AC-023 | I have submitted valid body details | the target is calculated | the app derives BMR using the Mifflin-St Jeor equation for my sex, weight, height and age, multiplies it by the factor for my selected activity level, and applies a daily deficit or surplus derived from the difference between my current and target weight |
| AC-024 | my target weight is lower than my current weight | the target is calculated | the result is a calorie deficit below maintenance; and where the target weight is higher, the result is a surplus above maintenance; and where they are equal, the result is maintenance calories |
| AC-025 | a target has been calculated | it is displayed to me | I see the daily calorie number together with a plain-language note that it is an estimate based on the Mifflin-St Jeor equation and my stated activity level |
| AC-026 | I change any of my body details in settings | I save the change | the calculated target is recalculated from the new values and I am shown the updated number before it takes effect |

### US-008: Override the calculated calorie target

**Priority:** high

> As a user who has my own number in mind, I want to replace the calculated target with one I choose, so that the app tracks against the goal I actually want to follow.

| ID | Given | When | Then |
|---|---|---|---|
| AC-027 | I am viewing my calorie target in settings | I choose to set my own target and enter a number within the accepted range | the manual target is saved, is used on the day screen and in history, and is labelled as 'set by you' rather than calculated |
| AC-028 | I have a manual target in place | I choose to revert to the calculated target | the app recalculates from my current body details and uses that number again |
| AC-029 | I have a manual target in place | I change my body details | my manual target is preserved and the app tells me a newly calculated value is available if I want to switch back to it |

### US-009: Be protected by a safe minimum calorie floor

**Priority:** critical

> As a user whose inputs would produce a very low target, I want the app to refuse to give me a dangerously low number, so that the product does not encourage disordered or unsafe eating.

| ID | Given | When | Then |
|---|---|---|---|
| AC-030 | my body details and target weight would produce a calculated target below the safe floor | the target is calculated | the target is raised to the floor — approximately 1200 kcal/day for women and 1500 kcal/day for men — and I am told the number has been raised to a safe minimum |
| AC-031 | I am setting a manual override | I enter a number below the safe floor for my sex | the override is refused with an explanation of the minimum, and the previous target remains in place |
| AC-032 | I am entering body details or a manual target | the values are extreme — for example a target weight far outside a plausible range for my height, or a calorie target far above or below the accepted bounds | the app refuses to set the target, explains why, and suggests speaking to a qualified professional |
| AC-033 | the exact floor values and extreme-value bounds are an assumption from discovery rather than a confirmed clinical rule | the thresholds are implemented | they are held in configuration that can be adjusted without a code change, and the assumption is recorded for review before launch |

### US-010: See the 'not medical advice' disclaimer

**Priority:** high

> As a user looking at my calorie target, I want a clear statement that the target is an estimate and not medical advice, so that I understand this is a general wellness tool rather than clinical guidance.

| ID | Given | When | Then |
|---|---|---|---|
| AC-034 | a calorie target is shown to me for the first time after calculation | the screen is displayed | a disclaimer is shown stating that the target is an estimate, is not medical advice, and that I should consult a qualified professional before making significant dietary changes |
| AC-035 | I am on the day screen or the target settings screen | the target is displayed | the disclaimer is present or reachable from that screen in a single tap |
| AC-036 | the app is positioned as a general wellness product and not a medical device | any target, estimate or history figure is shown | no screen presents a diagnosis, treatment, clinical recommendation or claim to treat a condition |

### US-011: Review and update body details later

**Priority:** medium

> As a existing user whose weight or routine has changed, I want to edit my body details and activity level at any time, so that my calorie target stays relevant to me.

| ID | Given | When | Then |
|---|---|---|---|
| AC-037 | I am signed in | I open settings | I see my stored height, current weight, age, sex, activity level and target weight, and my current calorie target with whether it is calculated or set by me |
| AC-038 | I am editing my body details | I save valid changes | the new values replace the old ones, the target is recalculated subject to the safety floor, and days already logged keep the target that applied on those days |
| AC-039 | I am editing my body details | I enter an invalid or out-of-range value | the save is refused with a field-level error and my previous details remain unchanged |

## EPIC-003: Daily Food Log and History

The main screen of the app: a single day's log grouped into breakfast, lunch, dinner and snacks, showing calories consumed, target and remaining, with date navigation, edit and delete, one-tap re-log of a past meal, and a simple last-several-days history.

Assumption carried from discovery: history is a plain list of recent days against target, not charts or analytics. Calories only — no macro breakdown of protein, carbs or fat appears anywhere.

### US-012: See today's log grouped by meal

**Priority:** critical

> As a user tracking what I eat, I want today's entries grouped into breakfast, lunch, dinner and snacks, so that I can see at a glance what I have eaten and where.

| ID | Given | When | Then |
|---|---|---|---|
| AC-040 | I am signed in and have completed body details | I open the app | today's log is the first screen I see, with four sections — breakfast, lunch, dinner and snacks — in that order |
| AC-041 | I have entries logged today | today's log is displayed | each entry shows its food name, chosen portion and calorie figure, and each meal section shows its own calorie subtotal |
| AC-042 | I have no entries at all for today | today's log is displayed | each empty meal section shows an add-food action and the day shows 0 calories consumed against my target |
| AC-043 | I am viewing any entry or total | the screen is displayed | only calorie figures are shown, with no protein, carbohydrate or fat values anywhere |

### US-013: See calories consumed, target and remaining

**Priority:** critical

> As a user deciding what to eat next, I want the day's consumed, target and remaining calories shown together, so that I know how much room I have left today.

| ID | Given | When | Then |
|---|---|---|---|
| AC-044 | I am viewing a day's log | the screen is displayed | a summary shows total calories consumed that day, my calorie target for that day, and remaining calories calculated as target minus consumed |
| AC-045 | I add, edit or delete an entry | the change is saved | the consumed, remaining and meal subtotal figures update immediately without me reloading the page |
| AC-046 | my consumed calories exceed my target | the summary is displayed | remaining is shown as a negative or 'over by' figure in a clearly distinct style, with no judgemental or alarming language |

### US-014: Move between dates

**Priority:** high

> As a user who forgot to log yesterday, I want to move back and forward between days, so that I can fill in or review a past day's food.

| ID | Given | When | Then |
|---|---|---|---|
| AC-047 | I am on today's log | I tap the previous-day control or pick a date | the log for that date is shown with its own meal groups, entries, totals and the target that applied on that date |
| AC-048 | I am viewing a past date | I add, edit or delete an entry | the change is saved against that date and not against today |
| AC-049 | I am viewing today's log | I attempt to move to a future date | the forward control is unavailable and no future-dated log can be created |
| AC-050 | I am viewing any date other than today | the screen is displayed | the date is clearly labelled and a single control returns me to today |

### US-015: Add a food entry to a meal

**Priority:** critical

> As a user who has just eaten, I want to add a food with a portion to a chosen meal on a chosen day, so that my calorie total reflects what I actually ate.

| ID | Given | When | Then |
|---|---|---|---|
| AC-051 | I am viewing a day's log | I tap add food on a meal section | the add flow opens with that meal and that date pre-selected, and offers both food search and plain-English description |
| AC-052 | I have selected a food and a portion | I confirm the entry | the entry is saved to the chosen meal and date with its calorie figure, and the day screen shows it in that section with updated totals |
| AC-053 | I am on the add flow | I change the meal selection before saving | the entry is saved to the newly selected meal |

### US-016: Edit or delete a food entry

**Priority:** critical

> As a user who logged the wrong thing or the wrong amount, I want to change or remove an entry, so that my day's total is accurate.

| ID | Given | When | Then |
|---|---|---|---|
| AC-054 | I am viewing an entry in a day's log | I open it for editing and change the portion, the calorie figure or the meal it belongs to | the entry is updated, moved to the new meal section if changed, and the day's totals recalculate |
| AC-055 | I am viewing an entry | I choose to delete it | I am asked to confirm, and on confirming the entry is removed from the day and the totals recalculate |
| AC-056 | I am asked to confirm a deletion | I cancel | the entry remains unchanged in the log |
| AC-057 | an entry belongs to another user's account | its identifier is requested directly | the request is refused and no data is returned |

### US-017: Re-log a previous meal in one tap

**Priority:** high

> As a user who eats the same breakfast most days, I want to repeat something I have logged before without searching again, so that logging a routine meal takes seconds.

| ID | Given | When | Then |
|---|---|---|---|
| AC-058 | I have logged entries on previous days | I open the add flow for a meal | I am shown a short list of my recently logged foods and portions for quick selection |
| AC-059 | I am shown a recent entry in that list | I tap it once | a copy of that entry with the same food, portion and calories is added to the current meal and date, and the totals update |
| AC-060 | I have re-logged a past entry | I edit the new entry's portion | only the new entry changes and the original entry on its own date is unaffected |

### US-018: View a simple history of recent days

**Priority:** medium

> As a user who wants to see how the week has gone, I want a list of the last several days showing calories against target, so that I can tell whether I have generally been on track.

| ID | Given | When | Then |
|---|---|---|---|
| AC-061 | I am signed in | I open the history view | I see a simple list of the most recent days, each row showing the date, calories consumed, the target that applied and whether I was under or over |
| AC-062 | I am viewing the history list | I tap a day | that day's full log opens on the day screen |
| AC-063 | a day in the range has no entries | the history list is displayed | that day is shown as having no entries rather than being omitted or shown as zero against target in a misleading way |
| AC-064 | this release deliberately keeps history simple (an assumption from discovery) | the history view is displayed | it contains no charts, trend lines, averages or analytics, and no weight history |

## EPIC-004: Food Catalogue, Search and Custom Foods

The searchable food database that makes logging fast: seeded from USDA FoodData Central with common foods and portion sizes, and extended by foods users create themselves.

Assumption carried from discovery: because the catalogue is seeded from USDA FoodData Central, coverage of branded and packaged products will be thin at launch; users are expected to fall back on custom foods or plain-English estimation for those. Barcode scanning is out of scope for this release.

### US-019: Search the food catalogue

**Priority:** critical

> As a user logging a meal, I want to search for a food by name, so that I can pick it rather than work out calories myself.

| ID | Given | When | Then |
|---|---|---|---|
| AC-065 | I am in the add-food flow | I type at least two characters into the search box | matching foods from the seeded catalogue and my own custom foods are listed, with my custom foods clearly marked |
| AC-066 | I have typed a search term | results are returned | each result shows the food name and a default portion with its calorie figure, and the list is ordered so the closest and most commonly logged matches appear first |
| AC-067 | my search term matches nothing in the catalogue | the results are empty | I am told nothing matched and offered both 'describe it in plain English' and 'create a custom food' as next steps |
| AC-068 | branded and packaged product coverage is thin at launch (an assumption from discovery) | I search for a branded product and find no match | the empty-result message explains that packaged product coverage is limited and points me to the custom food and plain-English options |

### US-020: Choose a portion with calories auto-filled

**Priority:** critical

> As a user who ate a specific amount, I want to pick a portion size and have the calories filled in for me, so that logging is accurate without any mental arithmetic.

| ID | Given | When | Then |
|---|---|---|---|
| AC-069 | I have selected a food from search results | the portion step is shown | the available portion options for that food (such as grams, millilitres or named household portions) are listed with a sensible default pre-selected |
| AC-070 | I am on the portion step | I change the portion or the quantity | the calorie figure recalculates immediately and is shown before I save |
| AC-071 | I am on the portion step | I enter a quantity that is zero, negative or non-numeric | the entry cannot be saved and a validation message explains the accepted range |

### US-021: Create a custom food

**Priority:** high

> As a user whose regular food is not in the catalogue, I want to create my own food with a name, portion and calories, so that I can log it quickly every time without re-estimating.

| ID | Given | When | Then |
|---|---|---|---|
| AC-072 | I am in the add-food flow or in settings | I choose to create a custom food and enter a name, a portion description and calories per portion | the food is saved to my account and is immediately available in my search results marked as my own |
| AC-073 | I am creating a custom food | I leave the name blank or enter calories that are negative, non-numeric or implausibly large | the save is refused with a field-level validation message |
| AC-074 | I have custom foods saved | I view my custom foods list | I can edit or delete any of them, and deleting one leaves previously logged entries that used it intact with their recorded calorie figures |
| AC-075 | I have created a custom food | another user searches the catalogue | my custom food does not appear in their results |

### US-022: Have the catalogue seeded from USDA FoodData Central

**Priority:** high

> As a product owner, I want the food catalogue pre-loaded from USDA FoodData Central with common foods and portions, so that the app is useful from the first search on day one.

| ID | Given | When | Then |
|---|---|---|---|
| AC-076 | a fresh environment with an empty database | the seed process is run | foods from the USDA FoodData Central dataset are loaded with their names, calorie values and at least one portion definition each, and the process reports how many records were loaded |
| AC-077 | the seed process has already been run | it is run again | existing catalogue records are updated rather than duplicated and user-created custom foods are left untouched |
| AC-078 | a seeded food is displayed in search or on an entry | the user views its detail | the source of the data is attributed as required by the dataset's terms of use |
| AC-079 | branded coverage from this dataset is limited (an assumption from discovery) | the catalogue is reviewed before launch | the known coverage gap for branded and packaged products is documented and surfaced to users through the search empty state |

## EPIC-005: Plain-English Meal Estimation

The fallback for home-cooked and unusual meals: the user describes what they ate in plain English and the app estimates the calorie breakdown, which they can adjust before saving.

Assumption carried from discovery and to be validated: estimation uses a hosted large language model (OpenAI) to parse the description into foods, portions and calories, which requires an OPENAI_API_KEY and introduces an external dependency with per-call cost and latency. Estimates are always presented as editable and are never saved without the user confirming them. Photo-of-your-meal recognition is out of scope.

### US-023: Describe a meal in plain English and get an estimate

**Priority:** critical

> As a user who ate something home-cooked, I want to type what I ate in ordinary words and have the app estimate it, so that I can log a meal that is not in any food database.

| ID | Given | When | Then |
|---|---|---|---|
| AC-080 | I am in the add-food flow for a meal and date | I choose to describe my meal and type something like 'two eggs and toast with butter' | the app sends the description for estimation and returns a breakdown listing each identified food with its assumed portion and calorie figure, plus a total |
| AC-081 | an estimate has been returned | it is displayed | it is clearly labelled as an estimate that may be inaccurate, and nothing has been saved to my log yet |
| AC-082 | I submit an empty description or one that contains no recognisable food | estimation runs | I am told the description could not be interpreted and I am invited to rephrase it or use food search instead, and nothing is saved |
| AC-083 | estimation depends on a hosted LLM (an assumption from discovery) | a request is sent | the description and the resulting estimate are handled in line with the privacy notice presented at signup, and no other user's data is included in the request |

### US-024: Adjust an estimate before saving it

**Priority:** critical

> As a user reviewing an estimate, I want to change the items, portions and calories before the entry is saved, so that what ends up in my log matches what I actually ate.

| ID | Given | When | Then |
|---|---|---|---|
| AC-084 | an estimate breakdown is displayed | I change a portion or calorie figure on any line, or remove a line | the total recalculates immediately and reflects my edits |
| AC-085 | I have reviewed and adjusted an estimate | I confirm and save | the resulting items are added to the selected meal and date as entries I can later edit or delete like any other |
| AC-086 | an estimate is displayed | I cancel or navigate away without confirming | nothing is added to my log |

### US-025: Cope gracefully when estimation is unavailable

**Priority:** high

> As a user trying to log a meal when the estimation service fails, I want a clear message and a usable alternative, so that I can still log my food.

| ID | Given | When | Then |
|---|---|---|---|
| AC-087 | the estimation service returns an error, times out, or no OPENAI_API_KEY is configured | I submit a description | I am shown a plain-language message that estimation is unavailable right now, with options to retry, search the catalogue or create a custom food |
| AC-088 | an estimation request is in progress | it is taking more than a moment | a progress indicator is shown and the add flow remains responsive, including the ability to cancel the request |
| AC-089 | estimation fails | the error is handled | no partial or zero-calorie entry is written to my log and no raw technical error text is shown to me |

## EPIC-006: Privacy, Data Export and Account Deletion

GDPR-level privacy handling for a product that stores body details and food logs, available to anyone in the world: self-service export of personal data and self-service deletion that removes it.

Assumption carried from discovery: the product is positioned as a general wellness tool, and HIPAA is assumed not to apply because no clinician, insurer or covered entity is involved. Signups are open worldwide with no geo-restriction, and GDPR-level handling is applied to everyone as a single baseline rather than varying by region.

### US-026: Export my personal data

**Priority:** high

> As a user who wants a copy of my own information, I want to download my body details, calorie target and food log, so that I keep control of my data and can take it elsewhere.

| ID | Given | When | Then |
|---|---|---|---|
| AC-090 | I am signed in and in settings | I request an export of my data | a machine-readable file containing my account details, consent record, body details, calorie target history, custom foods and all food log entries is made available to me |
| AC-091 | an export has been produced | I open it | it contains only my own data and no other user's records |
| AC-092 | the export cannot be produced immediately | I request it | I am told it is being prepared and how I will receive it, rather than being left with no feedback |

### US-027: Delete my account and data

**Priority:** high

> As a user who no longer wants to use the app, I want to delete my account myself without contacting anyone, so that my body details and food log are removed.

| ID | Given | When | Then |
|---|---|---|---|
| AC-093 | I am signed in and in settings | I choose to delete my account | I am shown exactly what will be deleted and asked to confirm explicitly before anything is removed |
| AC-094 | I confirm deletion | the deletion completes | my account, body details, calorie target, custom foods and all food log entries are removed, my session is ended, and signing in with those credentials no longer works |
| AC-095 | I am on the deletion confirmation step | I cancel | nothing is deleted and I remain signed in with my data intact |
| AC-096 | my account has been deleted | the seeded food catalogue is inspected | the shared USDA-seeded catalogue remains intact and only my own custom foods are removed |

### US-028: Read the privacy notice and see my consent record

**Priority:** medium

> As a user anywhere in the world, I want to see what is stored about me and what I agreed to, so that the GDPR-level commitment is verifiable rather than just stated.

| ID | Given | When | Then |
|---|---|---|---|
| AC-097 | I am on the signup screen or in settings | I open the privacy notice | it describes what personal data is stored (body details, food logs, account details), why, and that descriptions sent for plain-English estimation are processed by a third-party AI service |
| AC-098 | I am in settings | I view my privacy information | I can see the date I gave consent, the wording version I accepted, and links to export my data and delete my account |

## EPIC-007: Mobile-First Web Delivery

Delivery as a responsive web app that works well on a phone at the dinner table and can be saved to the home screen — explicitly not a native app store build and with no payments, subscriptions or billing.

Assumptions carried from discovery: Next.js/React on PostgreSQL, scale at low thousands of users with no unusual performance or deadline constraint. Native iOS/Android builds and health-platform syncing are out of scope for this release.

### US-029: Use the app comfortably on a phone

**Priority:** critical

> As a user logging a meal at the dinner table, I want the app to work well on a small screen with one hand, so that logging is quick enough that I actually do it.

| ID | Given | When | Then |
|---|---|---|---|
| AC-099 | I open the app on a typical phone screen width | I view the day screen, search, add flow, history or settings | each screen is laid out for that width with no horizontal scrolling and with primary actions reachable within thumb reach |
| AC-100 | I open the app on a tablet or desktop browser | any screen is displayed | the same screens reflow sensibly to the larger width and remain fully usable |
| AC-101 | I am adding a food on a phone | numeric fields such as quantity or calories receive focus | the appropriate numeric keyboard is presented and the focused field is not obscured by the keyboard |

### US-030: Save the app to my phone's home screen

**Priority:** medium

> As a daily user, I want to add the app to my home screen and open it like an app, so that I can reach it in one tap without an app store download.

| ID | Given | When | Then |
|---|---|---|---|
| AC-102 | I am viewing the app in a mobile browser that supports installation | I use the browser's add-to-home-screen option | the app installs with its own name and icon and launches in a standalone window without browser address bar chrome |
| AC-103 | the app has been added to my home screen | I open it | I am still signed in if my session is valid and I land on today's log |
| AC-104 | this release is a web app only (a decision from discovery) | a user looks for the product in an app store | no native iOS or Android build is published and all guidance points to the web address |

### US-031: Use the whole app free of charge

**Priority:** medium

> As a user, I want every feature available without paying, so that I can use the app without a billing decision.

| ID | Given | When | Then |
|---|---|---|---|
| AC-105 | I am using any part of the app including search, plain-English estimation, custom foods, history and export | I use that feature | no payment, plan selection, trial expiry or upgrade prompt is presented at any point |
| AC-106 | the free model is an assumption for the first release (per discovery) | the product is built | no payment provider, subscription record or billing data is stored, and the assumption that the app remains free is recorded for review in a later release |
<!-- sdlc:auto:end epics -->
