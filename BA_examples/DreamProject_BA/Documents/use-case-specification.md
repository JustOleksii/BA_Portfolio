# Use Case Specification

## Table of Contents

- [1. Document Purpose & Conventions](#1-document-purpose--conventions)  
  *What this file does, how to read each use case, the ID format (`UC-<MODULE>-NN`), and traceability rules.*

- [2. Actors & Assumptions](#2-actors--assumptions)  
  *Who interacts with the system (Regular User, Curator, Moderator/Admin, External Services) and MVP assumptions (EN/UA, read-only Steam/RA, compatibility layer).*

- [3. Use Case Index (by Module)](#3-use-case-index-by-module)  
  *The complete list of use cases grouped by module (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT) with short titles.*

- [4. Use Case Specifications](#4-use-case-specifications)  
  *Detailed flows for each `UC-*`, following one fixed template (Goal, Preconditions, Main Success Scenario, Alternate/Exception Flows, Postconditions, Business Rules from **BA Doc §8**, Data Touchpoints, UX Notes, NFR Hooks, Open Issues).*

- [5. Business Rules References](#5-business-rules-references)  
  *Pointers to the exact rules in **Business Analysis Document §8** that govern the use cases.*

- [6. NFR Hooks & Quality Constraints](#6-nfr-hooks--quality-constraints)  
  *Performance, security, privacy, accessibility, and localisation constraints from **SRS §6** that shape the flows.*

- [7. Traceability & Cross-Links](#7-traceability--cross-links)  
  *How `UC-*` maps to **BRD `BR-*`** and **SRS `SR-*`**, and how the RTM stays in sync.*

---
<br>

## 1. Document Purpose & Conventions

**Why this document exists**  
This file explains, in plain and testable terms, how people and external services achieve their goals in Dream Project. It is not UI artwork and it is not an API manual. Instead, it’s a set of **goal-driven use cases** that any engineer, tester, or reviewer can follow from trigger to outcome without guessing what happens in the middle.

**What’s in scope**  
MVP behaviours across our three pillars — **Save Management**, **Achievement Tracking** (official, retro, fan), and **Sticker Albums** — plus supporting areas (**Accounts**, **Catalogue**, **Profiles**, **Moderation**, **Discovery**, **Integrations**). Web application only, with **EN/UA** locales.

**Who should read this**  
- Product/BA reviewers validating intent and edge cases.  
- Engineers turning flows into endpoints, screens, and jobs.  
- QA writing scenarios straight from the “Main Success” and “Alternate” flows.  
- Community/Moderation leads confirming reporting and appeal paths.

**How to read a use case**  
Each use case is a small story with strict structure. Start at the **Trigger**, walk the **Main Success Scenario** step by step, then scan the **Alternate/Exception Flows** to see how we behave when things go sideways. If a rule or limit is mentioned, you’ll find its source in the references at the end of the use case.

**ID scheme**  
`UC-<MODULE>-NN`, where `<MODULE>` mirrors the SRS sections:  
`ACC` (Accounts), `CAT` (Catalogue), `SAV` (Saves), `ACH` (Achievements), `ALB` (Albums), `PRO` (Profiles), `GOV` (Moderation), `DSC` (Discovery), `INT` (Integrations).  
*Example:* `UC-SAV-03` — “Search/Filter Saves”.

**Template used for every use case**  
- **Goal** — the actor’s objective in one sentence.  
- **Scope/Level** — user goal vs sub-function.  
- **Primary Actor** — who initiates the flow.  
- **Stakeholders & Interests** — who else is affected and why.  
- **Preconditions** — must be true before we start.  
- **Trigger** — what kicks it off.  
- **Main Success Scenario** — numbered, end-to-end “happy path”.  
- **Alternate/Exception Flows** — realistic deviations, errors, timeouts.  
- **Postconditions** — what’s guaranteed on success; what minimal guarantees hold on failure.  
- **Business Rules** — references to **Business Analysis Document §8 (Business Rules)**.  
- **Data Touchpoints** — the key entities/fields we touch, at a functional level.  
- **UX Notes** — essential feedback, spoilers/locale/accessibility hints.  
- **NFR Hooks** — constraints from **SRS §6** that shape timing, security, privacy, or quality.  
- **Open Issues** — decisions intentionally left for later (kept short).

**Traceability**  
Each `UC-*` cites its related **BRD** `BR-*` items and **SRS** `SR-*` requirements. The RTM includes a `UC-*` column so we maintain forward/backward links. If a use case proposes something not present upstream, it will be marked **NEW-SCOPE** and paused for approval.

**Out of scope (for this document)**  
Wireframes and visual design, low-level database schemas, and HTTP/JSON contracts. Those live in other artefacts and will be linked when needed.

---
<br>
<br>

## 2. Actors & Assumptions

### 2.1 Actors

- **Regular User**  
  Uses any subset of the platform (not necessarily all three pillars). Can browse the game catalogue; upload/download compatible saves; view official/retro/fan achievement sets; provide evidence for fan achievements; create and complete sticker albums; adjust privacy/locale. Sees clear notices when the **Save** pillar is unavailable for a game/variant (e.g., multiplayer design, account-bound progression, no local saves).

- **Curator**  
  Community contributor who creates/edits **fan achievement sets** and **sticker album templates** (including spoiler policy and non-spoiler hints). Can publish/unpublish their content and respond to feedback within moderation policy.

- **Moderator/Admin**  
  Reviews user reports; manages quarantine and removals for saves/screenshots/fan sets; applies compatibility/status overrides when justified; processes appeals. Admin may also maintain catalogue policy flags (UA localisation, RU-linked metadata) and correct variant mappings.

- **External Services**  
  - **Steam** — user-consented OAuth linking; **read-only** import of official achievement schema and user progress (when visible).  
  - **RetroAchievements** — API key/username linking; **read-only** sets and user progress for mapped legacy titles.  
  - **IGDB** — game/variant metadata (platforms, regions, covers).  
  - **Kuli / Artemiano** — supplemental signals for UA localisation/origin.  
  - **Storage/CDN/Email/Vision** — infrastructure for files, delivery, and screenshot triage.  
  - **Evidence providers** — allowlisted links only (e.g., YouTube, GameFAQs) for fan-achievement proof.

---
<br>

### 2.2 Assumptions (MVP)

- **Platform shape**  
  Web application (responsive), **EN/UA** locales; privacy-first defaults; spoiler-safe presentation where marked.

- **Integrations**  
  Steam and RetroAchievements are **read-only** (no write-backs). Availability may fluctuate; the system degrades gracefully and surfaces “stale” indicators rather than failing hard.

- **Saves & compatibility**  
  Not all games support transferable saves. The catalogue clearly indicates pillar availability per **variant** (platform/region). The version-compatibility layer uses **Steam build/manifest IDs** for modern titles and **region/emulator** facets for retro. Public statuses are: **confirmed / untested / legacy / incompatible**. When a new build appears, previously confirmed saves revert to **untested** until reconfirmed.

- **Evidence for fan achievements**  
  Proof is via **links only** from an allowlist (no video uploads in MVP). Optional AI triage highlights likely matches; final decisions remain with users/moderators per policy.

- **Sticker albums & spoilers**  
  Templates may be marked for **new-player** use (reduced spoilers). Non-spoiler hints and “next slot preview” help users avoid missing shots; actual content stays hidden until explicitly revealed.

- **Policy & locale visibility**  
  UA/Russia policy flags are shown **only** in **Catalogue/Discovery** (never on user profiles). Visibility follows locale and user preference: visible by default in UA; toggle-controlled elsewhere.

- **Privacy & external IDs**  
  Minimal PII (email + hashed password). External profile panels (Steam/RA) are **opt-in** and **read-only**; external IDs remain hidden unless the user enables the panel.

- **Operations**  
  File uploads are antivirus-scanned and may be quarantined. AI moderation provides triage for screenshots; human review targets **≤48h** for initial decisions. Discovery/search updates propagate within minutes; background jobs handle index refresh and compatibility recalculation.

---
<br>
<br>

## 3. Use Case Index (by Module)

> IDs use `UC-<MODULE>-NN`. Short titles only; detailed specs will follow in Section 4, one by one.

### Accounts & Authentication (ACC)
- **UC-ACC-01 — Register Account**
- **UC-ACC-02 — Verify Email / Resend Link**
- **UC-ACC-03 — Log In / Log Out**
- **UC-ACC-04 — Reset Password**
- **UC-ACC-05 — Change Locale & Privacy Settings**
- **UC-ACC-06 — Link / Unlink Steam or RetroAchievements**
- **UC-ACC-07 — Delete Account (RTBF)**

### Catalogue & Localisation Flags (CAT)
- **UC-CAT-01 — Browse / Search Games**
- **UC-CAT-02 — View Game & Variants (platform, region, build)**
- **UC-CAT-03 — Toggle UA/Russia Policy Filters**
- **UC-CAT-04 — Suggest Metadata Correction (admin-reviewed)**

### Save Management (SAV)
- **UC-SAV-01 — Upload Save (with metadata & AV scan)**
- **UC-SAV-02 — View / Download Save**
- **UC-SAV-03 — Search / Filter Saves (variant, build/region, status)**
- **UC-SAV-04 — Submit Compatibility Confirmation (community)**
- **UC-SAV-05 — Report Problematic Save**
- **UC-SAV-06 — Archive / Restore / Delete Own Save**

### Achievement Tracking (ACH)
- **UC-ACH-01 — View Achievement Set (official / retro / fan)**
- **UC-ACH-02 — Sync Official / Retro Progress (read-only)**
- **UC-ACH-03 — Create / Edit Fan Achievement Set (curator)**
- **UC-ACH-04 — Mark Fan Achievement Complete (with evidence link)**
- **UC-ACH-05 — Comment / Rate Achievement Set**
- **UC-ACH-06 — Publish / Unpublish Fan Set (curator)**

### Sticker Albums (ALB)
- **UC-ALB-01 — View Album Template (type & spoiler policy)**
- **UC-ALB-02 — Create Album Instance**
- **UC-ALB-03 — Fill Slot with Screenshot (AI triage)**
- **UC-ALB-04 — Reveal / Hide Spoiler Slots**
- **UC-ALB-05 — Complete Album & Earn Badge**

### Profiles & Progress (PRO)
- **UC-PRO-01 — View Profile & Panels (Steam/RA read-only)**
- **UC-PRO-02 — Adjust Profile Visibility & Panels**
- **UC-PRO-03 — View Progress & Stats**
- **UC-PRO-04 — Apply to Become Curator (admin decision)**

### Moderation & Governance (GOV)
- **UC-GOV-01 — Report Item (save / achievement / screenshot)**
- **UC-GOV-02 — Moderator Triage & Decision**
- **UC-GOV-03 — Submit Appeal**

### Discovery & Search (DSC)
- **UC-DSC-01 — Global Search & Typeahead**
- **UC-DSC-02 — Apply Facets & Policy Toggles**
- **UC-DSC-03 — View Result Details (compatibility badges, availability)**

### External Integrations & Adapters (INT)
- **UC-INT-01 — Manually Trigger Sync (Steam / RetroAchievements)**
- **UC-INT-02 — Manage Evidence Provider Allowlist (admin)**

---
<br>
<br>

## 4. Use Case Specifications

### Accounts & Authentication (ACC)

---

#### UC-ACC-01 — Register Account

**Goal**  
Create a new Dream Project account so a user can access platform features (saves, achievements, albums) with the correct locale and privacy defaults.

**Scope/Level**  
User goal (end-to-end).

**Primary Actor**  
Prospective user (unauthenticated).

**Stakeholders & Interests**  
- **User:** Fast, safe sign-up; clear verification; control over privacy/locale.  
- **Platform:** Valid accounts, minimal PII, abuse prevention, verified email.

**Preconditions**  
- User is not signed in.  
- Registration is open (no maintenance gate).  
- Email service is available.

**Trigger**  
User selects **Sign up**.

**Main Success Scenario**  
1. System displays the sign-up form (email, password, locale EN/UA, accept Terms/Privacy).  
2. User submits.  
3. System validates inputs and applies rate limits.  
4. System creates a **pending** user and a verification token.  
5. System sends a verification email (time-limited link).  
6. System shows “Check your email” with **Resend** option (cooldown).  
7. User clicks the verification link.  
8. System verifies the token, activates the account, and signs the user in.  
9. System presents a brief welcome step for privacy/locale confirmation.

**Alternate/Exception Flows**  
- **A1: Email already registered.** Offer **Log in** or **Reset password**; do not create a duplicate.  
- **A2: Weak/invalid input.** Inline errors; user can correct and resubmit.  
- **A3: Verification link expired/invalid.** Show error; allow **Resend** (rate-limited).  
- **A4: Email service delayed.** Pending user created; banner explains delay; resend available with cooldown.  
- **A5: Never verified.** Pending users are purged by policy after TTL.

**Postconditions**  
- **Success:** Account **active**, session created; profile seeded with locale and default privacy.  
- **Minimal guarantees:** No partial activation; pending accounts have no authenticated access; PII minimised and purged if never verified.

**Business Rules**  
- **BA Document §8 — User Accounts & Roles:** registration, default role **Regular**, privacy defaults, locale selection.

**Traceability**  
- **BRD:** `BR-ACC-01` Registration, `BR-ACC-02` Email Verification, `BR-ACC-05` Locale/Privacy Defaults.  
- **SRS:** `SR-ACC-01` (form/validation), `SR-ACC-02` (pending & token), `SR-ACC-03` (verify), `SR-ACC-07` (session/CSRF), `SR-ACC-08` (resend cooldown), `SR-ACC-12` (rate-limits), `SR-ACC-15` (purge pending).

**Data Touchpoints**  
`users`, `verification_tokens`, `user_settings(locale, privacy)`, email queue/logs.

**UX Notes**  
Accessible form, clear errors, EN/UA copy; visible **Resend** with cooldown; post-verify welcome step.

**NFR Hooks**  
`NFR-SEC-01/02/04/07` (TLS, encryption at rest, AppSec baseline, PII minimisation); `NFR-UX-01/03` (WCAG, clear errors); `NFR-OBS-01/02` (correlation IDs, sign-up rates); `NFR-AVL-01` (availability).

**Open Issues**  
None for MVP.

---
<br>

#### UC-ACC-02 — Verify Email / Resend Link

**Goal**  
Activate a pending account via a time-limited verification link; allow resending if the link expires or is lost.

**Scope/Level**  
User goal.

**Primary Actor**  
Pending user.

**Stakeholders & Interests**  
- **User:** Quick activation; safe handling of expired/invalid links.  
- **Platform:** Prevent account takeovers; avoid spam.

**Preconditions**  
- A **pending** user record and verification token exist.  
- Email service operational for resend.

**Trigger**  
User clicks verification link (or requests **Resend**).

**Main Success Scenario**  
1. System validates token (not expired, not used, matches user).  
2. System marks account **active**, invalidates the token.  
3. System signs user in and shows a confirmation page.

**Alternate/Exception Flows**  
- **A1: Link expired/invalid.** Show message; user can request **Resend** (rate-limited).  
- **A2: Token reuse.** Treat as invalid; offer **Resend**.  
- **A3: Email service down (resend).** Show non-blocking warning; queue retry.

**Postconditions**  
- **Success:** Account **active**; user has a valid session.  
- **Minimal guarantees:** Tokens are single-use; expired tokens cannot activate accounts.

**Business Rules**  
- **BA Document §8 — User Accounts & Roles:** email verification, token TTL, resend cooldown.

**Traceability**  
- **BRD:** `BR-ACC-02` Email Verification.  
- **SRS:** `SR-ACC-02` (pending & token), `SR-ACC-03` (verify), `SR-ACC-08` (resend), `SR-ACC-12` (rate-limits).

**Data Touchpoints**  
`verification_tokens` (invalidate), `users.status` (pending→active), email logs.

**UX Notes**  
Clear status (“Verified”, “Link expired”), EN/UA messaging; prominent **Resend**.

**NFR Hooks**  
Security (token entropy/TTL), availability, observability (verify success/error rates).

**Open Issues**  
None.

---
<br>

#### UC-ACC-03 — Log In / Log Out

**Goal**  
Authenticate a user via email/password; establish a secure session; allow explicit logout.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Reliable access; clear errors; safe remember-me.  
- **Platform:** Brute-force protection; session security.

**Preconditions**  
- User account **active** (not pending or disabled).  
- Auth service available.

**Trigger**  
User submits credentials; or selects **Log out**.

**Main Success Scenario (Log In)**  
1. System validates email/password (constant-time compare).  
2. If valid, system creates a server-side session, sets secure, HTTP-only cookies.  
3. System redirects to the last intended page or dashboard.

**Main Success Scenario (Log Out)**  
1. User selects **Log out**.  
2. System invalidates the server-side session and deletes cookies.  
3. System redirects to a signed-out state.

**Alternate/Exception Flows**  
- **A1: Invalid credentials.** Show generic error; increment failure counter.  
- **A2: Lockout threshold reached.** Temporarily lock account; provide reset link path.  
- **A3: Unverified account.** Offer **Resend verification**; do not create session.  
- **A4: Session fixation risk.** Rotate session on login.

**Postconditions**  
- **Success:** Authenticated session exists; CSRF tokens minted.  
- **Minimal guarantees:** After logout, tokens and cookies are invalid.

**Business Rules**  
- **BA Document §8 — User Accounts & Roles:** authentication, lockout policy, session handling.

**Traceability**  
- **BRD:** `BR-ACC-03` Login, `BR-ACC-02` Verification prerequisite.  
- **SRS:** `SR-ACC-05` (login), `SR-ACC-06` (lockout), `SR-ACC-07` (CSRF/session).

**Data Touchpoints**  
`users` (failed_attempts, last_login_at), session store, audit events.

**UX Notes**  
EN/UA copy; keyboard accessible; avoid revealing which field was wrong.

**NFR Hooks**  
Security (TLS, cookie flags, lockout), performance (auth latency), observability (login success/error rates).

**Open Issues**  
None.

---
<br>

#### UC-ACC-04 — Reset Password

**Goal**  
Allow a user who controls the email address to set a new password.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Regain access securely.  
- **Platform:** Prevent token abuse.

**Preconditions**  
- Email service available.

**Trigger**  
User requests **Forgot password**.

**Main Success Scenario**  
1. System asks for email and rate-limits requests.  
2. If email exists, system creates a reset token (time-limited) and sends a link.  
3. User opens the link; system validates token.  
4. User sets a new password; system enforces policy and rotates sessions.  
5. System confirms success; user can sign in.

**Alternate/Exception Flows**  
- **A1: Unknown email.** Respond with generic success (do not reveal existence).  
- **A2: Token expired/invalid.** Show error; allow **Resend** (rate-limited).  
- **A3: Unverified account.** Allow reset; account remains pending until email is verified.

**Postconditions**  
- **Success:** Password updated; all sessions invalidated.  
- **Minimal guarantees:** Tokens are single-use and expire by TTL.

**Business Rules**  
- **BA Document §8 — User Accounts & Roles:** password policy, token TTL, rate-limits.

**Traceability**  
- **BRD:** `BR-ACC-04` Reset Password.  
- **SRS:** `SR-ACC-10` (reset request), `SR-ACC-11` (token flow), `SR-ACC-12` (rate-limits).

**Data Touchpoints**  
`password_reset_tokens`, `users.password_hash`, audit events.

**UX Notes**  
Accessible forms, masked inputs, clear success/failure states.

**NFR Hooks**  
Security (token entropy/TTL, hashing), availability, observability.

**Open Issues**  
None.

---
<br>

#### UC-ACC-05 — Change Locale & Privacy Settings

**Goal**  
Let a signed-in user update locale (EN/UA) and privacy options (profile visibility, external panels).

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Control over what’s shown publicly and in which language.  
- **Platform:** Honour preferences consistently.

**Preconditions**  
- User is signed in.

**Trigger**  
User opens **Settings**.

**Main Success Scenario**  
1. System shows current locale and privacy settings (profile visibility, external panels on/off).  
2. User adjusts settings and submits.  
3. System persists changes and applies them immediately.  
4. UI reflects updated locale and visibility.

**Alternate/Exception Flows**  
- **A1: Invalid combination.** Show inline validation (e.g., cannot enable an external panel without linking an account).  
- **A2: Rate-limited toggles.** Throttle rapid flips to prevent abuse.

**Postconditions**  
- **Success:** Settings updated; subsequent pages render accordingly.  
- **Minimal guarantees:** Partial updates are not applied; all-or-nothing save.

**Business Rules**  
- **BA Document §8 — User Accounts & Roles / Privacy:** default visibility, external panel opt-in, locale behaviour.

**Traceability**  
- **BRD:** `BR-ACC-05` Locale/Privacy Defaults & Updates.  
- **SRS:** `SR-ACC-13` (settings view), `SR-ACC-14` (persist & apply).

**Data Touchpoints**  
`user_settings` (locale, profile_visibility, external_panels).

**UX Notes**  
Explain privacy effects plainly; provide preview where relevant.

**NFR Hooks**  
Security (authorisation), performance (low latency), localisation (EN/UA strings).

**Open Issues**  
None.

---
<br>

#### UC-ACC-06 — Link / Unlink Steam or RetroAchievements

**Goal**  
Connect or disconnect external profiles to enable read-only progress sync.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Simple linking, clear what data is pulled, easy unlink.  
- **Platform:** Token security; respectful of provider ToS/quotas.

**Preconditions**  
- User signed in.  
- Provider available; quotas not exceeded.

**Trigger**  
User selects **Link Steam** or **Link RetroAchievements** (or **Unlink**).

**Main Success Scenario (Link)**  
1. System explains what will be synced (read-only) and asks for consent.  
2. User continues; system redirects to provider auth (OAuth/API key).  
3. Provider returns code/key; system stores token encrypted.  
4. System queues an initial sync job; UI shows **Linked** and **Last sync: pending**.  
5. Once sync completes, UI shows **Last sync: <timestamp>**; profile panels (if enabled) display read-only progress.

**Main Success Scenario (Unlink)**  
1. User selects **Unlink**.  
2. System confirms the impact (panels hidden; future syncs stop).  
3. System removes token and schedules purge of user-specific synced data within policy SLA; aggregates remain anonymised.  
4. UI shows **Unlinked**.

**Alternate/Exception Flows**  
- **A1: Consent denied at provider.** Show “Link cancelled”; no change.  
- **A2: Provider outage/quota.** Show non-blocking warning; allow retry later.  
- **A3: Token invalid/expired.** Mark as **Needs re-link**; do not pull data.

**Postconditions**  
- **Success:** Linked accounts sync read-only progress; unlinked accounts purge user-specific synced data by SLA.  
- **Minimal guarantees:** No write-backs to providers; tokens are encrypted and never exposed.

**Business Rules**  
- **BA Document §8 — External Profiles & Privacy:** explicit opt-in, read-only scope, unlink behaviour.

**Traceability**  
- **BRD:** `BR-INT-02` Steam Linking, `BR-INT-03` RetroAchievements Linking.  
- **SRS:** `SR-ACC-16` (consent & redirect), `SR-ACC-17` (token storage), `SR-ACC-18` (initial sync job), `SR-ACC-19` (unlink purge), `SR-ACC-12` (rate-limits).

**Data Touchpoints**  
`external_links` (provider, token_ref, last_synced_at, visibility), job queue, audit events.

**UX Notes**  
Clear, EN/UA consent text; last-sync indicator; “stale” badge if provider unreachable.

**NFR Hooks**  
Security (token handling), availability (provider outage handling), observability (sync metrics), compliance (ToS).

**Open Issues**  
None.

---
<br>

#### UC-ACC-07 — Delete Account (RTBF)

**Goal**  
Let a user permanently remove their personal data and content, subject to moderation/legal holds.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Trustworthy deletion and transparency.  
- **Platform:** Legal compliance; auditability.

**Preconditions**  
- User signed in; authentication step-up (password re-entry) required.  
- No active legal hold prevents deletion.

**Trigger**  
User selects **Delete account**.

**Main Success Scenario**  
1. System displays the consequences (loss of access; deletion/anonymisation of content; irreversible).  
2. User confirms and re-enters password.  
3. System queues deletion tasks: sessions revoked; personal data removed; user-owned uploads (saves/screenshots/comments) deleted unless under moderation/legal hold; external tokens revoked.  
4. System sends confirmation email and signs the user out.

**Alternate/Exception Flows**  
- **A1: Wrong password.** Show error; allow retry; rate-limit attempts.  
- **A2: Legal/Moderation hold.** Explain hold; block deletion; offer contact path.  
- **A3: Deletion job failure.** Keep account disabled; retry jobs; provide status email.

**Postconditions**  
- **Success:** Account removed; user cannot sign in; content deleted (or anonymised where policy dictates); aggregates may retain anonymised counts.  
- **Minimal guarantees:** If full deletion stalls, account remains disabled and PII access is blocked until completion.

**Business Rules**  
- **BA Document §8 — Privacy & Retention:** RTBF scope, legal hold, content ownership, deletion windows.

**Traceability**  
- **BRD:** `BR-ACC-07` Account Deletion.  
- **SRS:** `SR-ACC-21` (deletion request & confirmation), `SR-ACC-22` (job orchestration), `SR-ACC-23` (legal hold handling).

**Data Touchpoints**  
`users` (soft-disable→delete), `saves`/`slot_fills`/comments (owned content), `external_links`, audit events, deletion queue.

**UX Notes**  
Plain-language consequences; export link (optional) prior to deletion; clear final state.

**NFR Hooks**  
Security (step-up auth), compliance (RTBF), availability (async jobs), observability (deletion progress).

**Open Issues**  
Export-before-delete flow is optional; confirm if included in MVP.

---
<br>
<br>

### Catalogue & Localisation Flags (CAT)

---

#### UC-CAT-01 — Browse / Search Games

**Goal**  
Find games and their variants (platform/region/build) and see which pillars (Saves / Achievements / Albums) are available before opening details.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (anon or signed-in).

**Stakeholders & Interests**  
- **User:** Fast, relevant results; clear indicators for pillar availability and policy flags.  
- **Platform:** Accurate catalogue; performant search; policy visibility in the right place (catalogue/discovery only).

**Preconditions**  
- Catalogue index is available.  
- Policy toggle defaults applied (UA locale defaults on).

**Trigger**  
User opens Catalogue and types in the search bar or applies filters.

**Main Success Scenario**  
1. System shows search box plus filters (platform, region, release year range, pillar availability, compatibility status).  
2. User enters a query and/or sets filters.  
3. System returns a results grid/list with, for each title: cover, year, **pillars badges** (Save/Achievements/Albums), and a **policy banner** if UA/Russia flags apply.  
4. When the title has multiple variants, the result shows a “Variants” chip (e.g., PC, PS1 (NTSC), SNES (PAL)).  
5. User selects a title to open the details page (UC-CAT-02).

**Alternate/Exception Flows**  
- **A1: No results.** Show “No matches” with suggestions (broaden filters, try alternative spelling).  
- **A2: Index refresh in progress.** Serve last consistent snapshot and show “Updating” badge (non-blocking).  
- **A3: Policy toggle off.** Hide policy banners and RU-filter facets; results remain otherwise unchanged.  
- **A4: Temporary metadata gap.** If a known title lacks variant data, show a single “Unresolved variants” chip; details page explains status.

**Postconditions**  
- **Success:** User sees relevant titles with pillar availability and policy context.  
- **Minimal guarantees:** Results never reveal user profile data; policy information stays in catalogue/discovery context.

**Business Rules**  
- **BA Document §8 — Catalogue & Localisation Flags; Pillar Availability & Disclaimers; Policy Visibility.**

**Traceability**  
- **BRD:** `BR-CAT-01` Catalogue Search/Filter, `BR-CAT-02` Pillar Availability Badges, `BR-CAT-04` Policy Presentation Scope.  
- **SRS:** `SR-CAT-01` (search/filters), `SR-CAT-02` (result badges), `SR-CAT-03` (policy banner scope), `SR-DSC-xx` (shared facets).

**Data Touchpoints**  
`games`, `game_variants(platform, region, build_manifest_ref)`, `catalog_index`, `policy_flags(ua_localised, ru_linked)`, computed `pillar_availability`.

**UX Notes**  
Typeahead aids spelling; badges have accessible labels (EN/UA); filters persist across pagination.

**NFR Hooks**  
Performance (search p95 ≤ 700 ms), localisation (EN/UA copy), privacy (no profile data in results).

**Open Issues**  
None.

---
<br>

#### UC-CAT-02 — View Game & Variants (platform, region, build)

**Goal**  
Inspect a game’s detail page to choose a specific **variant** (platform/region/build), understand pillar availability, and see compatibility/status notes.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (anon or signed-in).

**Stakeholders & Interests**  
- **User:** Clear variant choices; upfront warnings where **Save** is unavailable (e.g., account-bound multiplayer).  
- **Platform:** Consistent semantics for compatibility statuses and policy flags.

**Preconditions**  
- The selected game exists in the catalogue.

**Trigger**  
User clicks a search result (UC-CAT-01).

**Main Success Scenario**  
1. System shows game summary (cover, year, genres) and a **Variants** table: each row lists **Platform**, **Region** (e.g., NTSC/PAL), and for modern PC variants a **Build/Manifest** indicator (known/unknown).  
2. For each variant, system displays **Pillar Availability** badges:  
   - **Save:** Available / **Unavailable** (with reason: multiplayer/account-bound/no local saves).  
   - **Achievements:** Official (Steam) / Retro (RA) / Fan (platform) — as applicable.  
   - **Albums:** Template(s) available or none yet.  
3. For variants mapped to Steam, system shows “Build/Manifest: **Known**” or “**Unknown**”; if known, link to list of **confirmed / untested / legacy / incompatible** save statuses.  
4. For retro variants, system shows region/emulator notes and compatibility status counts (confirmed/untested/legacy/incompatible).  
5. If UA/Russia policy flags apply, system shows a **catalogue-only** banner with the relevant note.  
6. User selects a variant to proceed to a pillar-specific action (e.g., open Saves or Achievements).

**Alternate/Exception Flows**  
- **A1: Unknown build/manifest (modern).** Save compatibility displays as **untested**; user is invited to check Saves and community confirmations.  
- **A2: No mapped RA set (retro).** Show “No RA set mapped” and point to fan sets if available.  
- **A3: Only Albums available.** Page explains that Saves/Achievements are unavailable for this variant; albums still usable.  
- **A4: Policy banner hidden (toggle off).** Variant view omits policy text; pillar data unchanged.

**Postconditions**  
- **Success:** User understands variant differences and pillar availability/compatibility before acting.  
- **Minimal guarantees:** Policy information remains scoped to catalogue; no personal data shown.

**Business Rules**  
- **BA Document §8 — Version Compatibility (status taxonomy); Pillar Availability & Disclaimers; Policy Visibility.**

**Traceability**  
- **BRD:** `BR-CAT-02` Variant View, `BR-SAV-02` Compatibility Surfacing, `BR-CAT-04` Policy Scope.  
- **SRS:** `SR-CAT-04` (variant table), `SR-SAV-xx` (compatibility layer badges), `SR-CAT-12` (policy flags).

**Data Touchpoints**  
`games`, `game_variants(platform, region, build_manifest_ref)`, `compat_stats(status_counts)`, `policy_flags`.

**UX Notes**  
Badges include tooltips explaining reasons (e.g., “Save unavailable: account-bound progression”). Link out to relevant pillar pages.

**NFR Hooks**  
Performance (detail load p95 ≤ 500 ms), localisation (EN/UA), data quality (variant mapping validation).

**Open Issues**  
None.

---
<br>

#### UC-CAT-03 — Toggle UA/Russia Policy Filters

**Goal**  
Enable or disable the display and filtering of UA localisation/origin and Russia-linked content **within Catalogue/Discovery only**.

**Scope/Level**  
Preference (user or browser-level).

**Primary Actor**  
Visitor or signed-in user.

**Stakeholders & Interests**  
- **User:** Control over seeing policy cues in discovery.  
- **Platform:** Keep policy presentation scoped (not on profiles).

**Preconditions**  
- Policy module enabled.  
- Signed-in users have a stored preference; visitors can use a cookie/local storage setting.

**Trigger**  
User opens **Policy Filters** and toggles visibility/filters.

**Main Success Scenario**  
1. System shows two controls: **Show policy banners** and **Filter RU-linked titles**.  
2. User toggles settings.  
3. System persists preference (account or cookie).  
4. System refreshes the results to reflect the choice (hide/show banners; include/exclude RU-linked titles).

**Alternate/Exception Flows**  
- **A1: Anonymous user in private mode.** Preference held for the session only.  
- **A2: Region mismatch.** UA locale defaults to banners **on**; others default **off** but can be enabled.

**Postconditions**  
- **Success:** Catalogue/discovery reflect preference; other areas (e.g., profiles) remain unaffected.  
- **Minimal guarantees:** Filters never expose private user data.

**Business Rules**  
- **BA Document §8 — Policy Visibility (catalogue/discovery only); UA defaults; RU-linked filter semantics.**

**Traceability**  
- **BRD:** `BR-CAT-04` Policy Scope & Controls.  
- **SRS:** `SR-CAT-12` (policy toggle persistence), `SR-DSC-14` (policy facets).

**Data Touchpoints**  
`user_settings.policy_prefs` (if signed in), client storage (if anon), `catalog_index.policy_flags`.

**UX Notes**  
Short, neutral copy; explain that policy banners never appear on user profiles.

**NFR Hooks**  
Localisation (EN/UA strings), performance (filter reflow), privacy (no spillover of policy state to profiles).

**Open Issues**  
None.

---
<br>

#### UC-CAT-04 — Suggest Metadata Correction (admin-reviewed)

**Goal**  
Allow users to suggest fixes for game/variant metadata, routed to an admin review queue.

**Scope/Level**  
Sub-function (content improvement).

**Primary Actor**  
Signed-in user (Regular or Curator).

**Stakeholders & Interests**  
- **User:** Ability to improve accuracy with minimal friction.  
- **Admin:** Review suggestions with evidence and accept/reject safely.

**Preconditions**  
- User signed in.  
- Suggestion system enabled.

**Trigger**  
User selects **Suggest a correction** on a game/variant page.

**Main Success Scenario**  
1. System opens a form with current field values (read-only) and editable proposals: title/variant label, platform/region, build/manifest ref (ID or “unknown”), UA localisation flag, RU-linked indicators.  
2. User enters proposed changes and an **evidence link** (required; allowlisted providers).  
3. User submits; system validates and rate-limits.  
4. System creates a **pending suggestion** and puts it in the admin queue.  
5. System shows confirmation (“Thanks—under review”).

**Alternate/Exception Flows**  
- **A1: Missing evidence link.** Inline error; cannot submit.  
- **A2: Duplicate open suggestion.** Show existing suggestion status; block duplicates.  
- **A3: Abuse/spam.** Rate-limit; escalate to moderation if threshold exceeded.

**Postconditions**  
- **Success:** Suggestion recorded; admin can review, merge, or reject.  
- **Minimal guarantees:** Catalogue remains unchanged until approval; audit trail preserved.

**Business Rules**  
- **BA Document §8 — Catalogue Governance; Evidence Allowlist; Rate-Limits; Auditability.**

**Traceability**  
- **BRD:** `BR-CAT-05` User Suggestions, `BR-GOV-02` Admin Review.  
- **SRS:** `SR-CAT-18` (suggestion form/validation), `SR-GOV-xx` (moderation queue).

**Data Touchpoints**  
`suggestions` (proposed fields, evidence_url, status), audit events, moderation queue.

**UX Notes**  
Make current vs. proposed values visually distinct; explain review SLA at submission.

**NFR Hooks**  
Security (link allowlist), availability (queue durability), observability (suggestion throughput).

**Open Issues**  
Admin UI specifics are covered in Moderation/Governance specs.

---
<br>
<br>

### Save Management (SAV)

---

#### UC-SAV-01 — Upload Save (with metadata & AV scan)

**Goal**  
Let a signed-in user upload a game save for a specific **variant** (platform/region/build), with required metadata and automated safety checks.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Safe, straightforward upload; clear indication of where the save will work.  
- **Platform:** Correct variant mapping; malware-free files; consistent compatibility signalling.

**Preconditions**  
- User is signed in.  
- The chosen **game variant** exists (from Catalogue).  
- Save pillar is **available** for that variant (some games disallow transfer).

**Trigger**  
User selects **Upload Save** on a variant page or from their profile.

**Main Success Scenario**  
1. System displays an upload form: file picker, variant (preselected), optional notes; required fields: **platform/region**, **build/manifest** (if modern; may be set to “unknown”), optional **playtime/chapter**.  
2. User selects file and submits.  
3. System validates size/extension per policy; assigns a temporary ID.  
4. System stores the file in quarantine and runs **AV scan**.  
5. If clean, system extracts or confirms metadata (e.g., infers build/manifest when possible), saves the record, and publishes the save entry.  
6. System sets **compatibility status** for this variant to **untested** by default (or **confirmed** if a matching, previously confirmed signature exists).  
7. System shows a success message with the new save’s page link.

**Alternate/Exception Flows**  
- **A1: Save pillar unavailable for variant.** System blocks upload and explains why (e.g., account-bound).  
- **A2: AV detection or unsupported format.** System rejects upload with reason; file never leaves quarantine.  
- **A3: Build/manifest unknown.** System records “unknown”; status = **untested**; user is prompted to add details later.  
- **A4: Metadata mismatch.** If file signature implies a different variant, system warns and offers to switch or cancel.  
- **A5: Transient storage error.** System reports failure; no record created; user can retry.

**Postconditions**  
- **Success:** Save is published with compatibility status and metadata; visible to others for download.  
- **Minimal guarantees:** Rejected files are deleted from quarantine; no partial/unsafe publication.

**Business Rules**  
- **BA Document §8 — Save Management; Version Compatibility; Content Ownership; Moderation & Reporting; Data Retention & Privacy.**

**Traceability**  
- **BRD:** `BR-SAV-01` Upload & Metadata, `BR-SAV-04` Safety (AV/Quarantine), `BR-SAV-02` Compatibility Surfacing.  
- **SRS:** `SR-SAV-11` (upload), `SR-SAV-21` (quarantine & AV), `SR-SAV-25` (metadata extraction), `SR-SAV-31` (initial status).

**Data Touchpoints**  
`saves` (variant_id, status, notes, created_by), `save_files` (object_ref, checksum), `compat_index`, audit events.

**UX Notes**  
Show plain-language reasons for rejections; tooltips for statuses (**confirmed / untested / legacy / incompatible**); EN/UA copy.

**NFR Hooks**  
Security (AV, quarantine, signed URLs), performance (ingest latency), privacy (no PII inside filenames), observability (upload success/error rates).

**Open Issues**  
None.

---
<br>

#### UC-SAV-02 — View / Download Save

**Goal**  
Allow users to view a save’s details and download it, with clear compatibility context.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (download may require sign-in if policy dictates).

**Stakeholders & Interests**  
- **User:** Trustworthy file; clear instructions and compatibility cues.  
- **Platform:** Safe distribution; accurate status; moderation overlays when needed.

**Preconditions**  
- Save exists and is not quarantined or removed.

**Trigger**  
User opens a save page or clicks **Download**.

**Main Success Scenario**  
1. System shows save details: variant, uploader, timestamp, optional notes, and **compatibility status** (with reason/link to confirmations).  
2. System shows guidance: how to place the file for the selected platform/region/build.  
3. User clicks **Download**.  
4. System issues a time-limited, secure link; download begins.  
5. System increments download count and updates last_accessed.

**Alternate/Exception Flows**  
- **A1: Quarantined/removed.** Page shows a banner explaining that the save is unavailable (with moderation reason if applicable).  
- **A2: Incompatible status.** Page warns prominently; user can still download if policy allows.  
- **A3: Rate-limited.** Excessive downloads from the same client trigger a cooldown.

**Postconditions**  
- **Success:** User receives the save file; system metrics updated.  
- **Minimal guarantees:** If blocked, file is not served and a clear reason is given.

**Business Rules**  
- **BA Document §8 — Save Access & Safety; Version Compatibility; Moderation Visibility.**

**Traceability**  
- **BRD:** `BR-SAV-03` Download & Guidance, `BR-SAV-02` Compatibility Display.  
- **SRS:** `SR-SAV-12` (view), `SR-SAV-13` (download link), `SR-SAV-29` (guidance text).

**Data Touchpoints**  
`saves`, `save_files` (signed URL), download counters, moderation flags.

**UX Notes**  
Accessibility for guidance text; status badges with concise tooltips; EN/UA support.

**NFR Hooks**  
Security (signed URLs), availability (object storage/CDN), performance (p95 link issue/serve time), observability (download metrics).

**Open Issues**  
None.

---
<br>

#### UC-SAV-03 — Search / Filter Saves (variant, build/region, status)

**Goal**  
Find relevant saves quickly using variant, build/region, and compatibility filters.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor.

**Stakeholders & Interests**  
- **User:** Precise filters; predictable sorting; painless narrowing.  
- **Platform:** Index freshness; consistent status semantics.

**Preconditions**  
- Saves are indexed; catalogue/variant metadata available.

**Trigger**  
User opens a game or variant’s **Saves** tab and applies filters.

**Main Success Scenario**  
1. System displays filters: **Platform**, **Region**, **Build/Manifest (modern)**, **Status** (confirmed/untested/legacy/incompatible), and sort (newest, most downloaded).  
2. User sets filters and submits (or uses instant facet updates).  
3. System returns a list with key fields (title snippet, uploader, timestamp, status).  
4. User optionally refines filters or clears them.

**Alternate/Exception Flows**  
- **A1: No matches.** Show empty-state suggestions; offer to reset filters.  
- **A2: Index updating.** Show “Updating” badge; serve last consistent view.

**Postconditions**  
- **Success:** Filtered list shown; user can proceed to a specific save.  
- **Minimal guarantees:** Status semantics are the same as elsewhere in the platform.

**Business Rules**  
- **BA Document §8 — Version Compatibility; Catalogue/Variant Mapping; Search Facets Semantics.**

**Traceability**  
- **BRD:** `BR-SAV-05` Save Search/Filter.  
- **SRS:** `SR-SAV-14` (filters), `SR-DSC-08` (shared facets).

**Data Touchpoints**  
`saves_index` (denormalised fields), `compat_index`, variant refs.

**UX Notes**  
Facet chips and quick clear; accessible labels for statuses.

**NFR Hooks**  
Performance (p95 result ≤ 700 ms), availability (index service), observability (facet usage).

**Open Issues**  
None.

---
<br>

#### UC-SAV-04 — Submit Compatibility Confirmation (community)

**Goal**  
Let users record whether a save worked (or didn’t) on a specific variant/build, turning **untested** into **confirmed** or **incompatible**, and flagging **legacy** when a game build changes.

**Scope/Level**  
Sub-function (quality signal).

**Primary Actor**  
Signed-in user who has tried the save.

**Stakeholders & Interests**  
- **User:** Lightweight way to help others; credit for confirmations.  
- **Platform:** Crowd-validated compatibility with auditability.

**Preconditions**  
- User is signed in.  
- The save is published.  
- The user has attempted to load it (self-attested).

**Trigger**  
User clicks **Confirm Compatibility** on a save page.

**Main Success Scenario**  
1. System opens a small form: **Outcome** (Worked / Didn’t work), **Variant/Build used** (preselected if coming from variant), optional comment.  
2. User submits.  
3. System records the confirmation, increments the appropriate counter, and updates the aggregated **compatibility status** if thresholds are met (e.g., two consistent “Worked” confirmations from distinct users).  
4. If the save’s recorded build is older than the current known build, system may mark as **legacy** (admin-reviewable rule).  
5. Page updates the status counters and shows the user’s contribution.

**Alternate/Exception Flows**  
- **A1: Conflicting signals.** Mixed confirmations keep status **untested** (or **incompatible** only if a threshold of “Didn’t work” is reached).  
- **A2: Abuse detection.** Rapid, repeated confirmations from the same account/IP are rate-limited or ignored.  
- **A3: Admin override.** Admin can set status directly with a note (e.g., known anti-transfer design).

**Postconditions**  
- **Success:** Confirmation stored; aggregated status possibly updated.  
- **Minimal guarantees:** Full audit trail; individual confirmations visible (user/time/variant/build).

**Business Rules**  
- **BA Document §8 — Version Compatibility (thresholds, legacy handling); Moderation & Auditability.**

**Traceability**  
- **BRD:** `BR-SAV-06` Community Confirmation, `BR-SAV-02` Status Semantics.  
- **SRS:** `SR-SAV-26` (confirm form), `SR-SAV-27` (aggregation rules), `SR-GOV-05` (override).

**Data Touchpoints**  
`compat_confirmations` (user_id, variant/build, outcome, comment), `compat_index` (aggregates), audit events.

**UX Notes**  
Explain thresholds in plain language; show per-variant/build chips; EN/UA.

**NFR Hooks**  
Rate-limits; observability (conflict ratio); security (spam controls).

**Open Issues**  
Exact thresholds are policy-controlled; defaults can be tuned post-MVP.

---
<br>

#### UC-SAV-05 — Report Problematic Save

**Goal**  
Enable users to report a save for malware, corruption, policy violations, or mis-categorisation.

**Scope/Level**  
Sub-function (governance).

**Primary Actor**  
Any signed-in user.

**Stakeholders & Interests**  
- **User:** Quick reporting; visible outcome.  
- **Moderator/Admin:** Clear context to act; immutable log.

**Preconditions**  
- User signed in.  
- Moderation system active.

**Trigger**  
User clicks **Report** on a save page.

**Main Success Scenario**  
1. System opens a report form with categories (malware, corruption, wrong variant/build, policy violation, other) and free-text notes.  
2. User submits; system rate-limits repeated reports.  
3. System creates a **moderation case**; the save may show a **“Under review”** banner (configurable).  
4. Moderators review and decide: keep, quarantine, or remove; optionally correct variant/build.  
5. System records the decision; the page reflects the outcome.

**Alternate/Exception Flows**  
- **A1: Duplicated reports.** System merges into an existing case.  
- **A2: Abuse/spam.** Reporter temporarily blocked from further reports.

**Postconditions**  
- **Success:** Case logged; decision applied; audit trail intact.  
- **Minimal guarantees:** Until a decision, the current visibility state is preserved (or quarantined per policy).

**Business Rules**  
- **BA Document §8 — Moderation & Reporting; Appeals; Auditability.**

**Traceability**  
- **BRD:** `BR-GOV-01` Reporting, `BR-GOV-02` Moderator Queue.  
- **SRS:** `SR-GOV-11` (report intake), `SR-GOV-12` (case workflow), `SR-SAV-22` (quarantine state).

**Data Touchpoints**  
`reports`, `moderation_cases`, `saves` (state), audit events.

**UX Notes**  
Short, guided categories; optional evidence link (text only) when reporting mis-categorisation.

**NFR Hooks**  
Rate-limits; availability (queue durability); observability (time-to-first-action).

**Open Issues**  
Whether to hide downloads while “Under review” is policy-controlled.

---
<br>

#### UC-SAV-06 — Archive / Restore / Delete Own Save

**Goal**  
Let uploaders manage their saves over time: hide from active listings (archive), bring back (restore), or remove (delete), respecting moderation/legal holds.

**Scope/Level**  
User goal.

**Primary Actor**  
Uploader (registered user).

**Stakeholders & Interests**  
- **Uploader:** Control over visibility and lifecycle.  
- **Platform:** Storage hygiene; policy compliance.

**Preconditions**  
- User is signed in and owns the save.  
- No active moderation/legal hold for deletion.

**Trigger**  
User opens **Manage** on their save.

**Main Success Scenario**  
1. **Archive:** User selects **Archive** → system removes the save from active listings/search but keeps it downloadable via direct link (configurable).  
2. **Restore:** User selects **Restore** → system returns the save to active listings/search.  
3. **Delete:** User selects **Delete** → system explains consequences (irreversible), confirms, and deletes file + record (or schedules deletion if async); compatibility aggregates adjust accordingly.

**Alternate/Exception Flows**  
- **A1: Legal/Moderation hold.** Deletion blocked; system explains reason.  
- **A2: Dependent records.** If policy requires, comments/confirmations remain but are anonymised.

**Postconditions**  
- **Success:** Save state updated as requested; indexes refreshed.  
- **Minimal guarantees:** If deletion fails mid-process, the save is hidden and queued for retry.

**Business Rules**  
- **BA Document §8 — Content Ownership; Retention & Lifecycle; Moderation Holds.**

**Traceability**  
- **BRD:** `BR-SAV-07` Owner Lifecycle Actions.  
- **SRS:** `SR-SAV-18` (archive/restore), `SR-SAV-19` (delete), `SR-DSC-12` (index refresh).

**Data Touchpoints**  
`saves` (state), `save_files` (object deletion), `compat_index`, audit events.

**UX Notes**  
Clear labels and confirmations; show current state; EN/UA messages.

**NFR Hooks**  
Availability (async jobs), security (authorisation), observability (state-change metrics).

**Open Issues**  
Direct-link behaviour for archived saves is policy-controlled (default: disabled in MVP).

---
<br>
<br>

### Achievement Tracking (ACH)

---

#### UC-ACH-01 — View Achievement Set (official / retro / fan)

**Goal**  
Let users view an achievement set for a game/variant, see progress, and inspect individual achievements (criteria, flags, evidence, discussion).

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (progress shown only for signed-in users).

**Stakeholders & Interests**  
- **User:** Clear criteria, spoiler-safe flags, progress overview, evidence/discussion in one place.  
- **Platform:** Consistent semantics across official (Steam), retro (RA), and fan sets; policy-compliant presentation.

**Preconditions**  
- Game/variant exists.  
- At least one set is available (official/retro/fan). For fan sets, a published version exists.

**Trigger**  
User opens the **Achievements** tab or follows a link to a set.

**Main Success Scenario**  
1. System displays the set header: type (**Official / Retro / Fan**), version (for fan), curator/owner (for fan), and **completion stats** (e.g., % users complete, average time-to-complete).  
2. System lists achievements with: title, criteria, optional flags (storyline / irreversible / final completion), and spoiler state.  
3. If the user is signed in, system shows their **progress state** (complete / not started / in progress) and timestamps for completions (official/retro via sync; fan via evidence approval).  
4. Each achievement links to a detail page with criteria, comments thread, evidence references (links only), and curator notes/hints (if provided).  
5. If multiple sets exist (e.g., official and a fan subset), system allows switching via tabs without losing context.  
6. Policy banners (UA/RU) do **not** appear here (catalogue/discovery scope only).

**Alternate/Exception Flows**  
- **A1: No mapped official/retro set.** Page offers fan sets if available or explains absence.  
- **A2: Fan set unpublished.** Show “Unavailable” notice; only curators/admins can view.  
- **A3: Spoiler-safe mode.** Spoilered items show blurred text/media until the user chooses **Reveal** (per locale/setting).  
- **A4: Stale progress.** If Steam/RA sync is stale, show a “Last sync” badge and offer **Sync now** (read-only).

**Postconditions**  
- **Success:** User understands what the set contains and their progress status.  
- **Minimal guarantees:** No personal identifiers exposed beyond the user’s own view; policy banners remain out of achievement pages.

**Business Rules**  
- **BA Document §8 — Achievement Tracking; Spoiler Policies; Fan Set Versioning; Evidence (links only, allowlist).**

**Traceability**  
- **BRD:** `BR-ACH-01` Unified Set View, `BR-ACH-07` Spoiler Policy, `BR-ACH-09` Completion Stats.  
- **SRS:** `SR-ACH-01` (set view), `SR-ACH-05` (flags), `SR-ACH-08` (progress pane), `SR-ACH-30` (stats).

**Data Touchpoints**  
`achievement_sets` (type, version, curator_id), `achievements` (criteria, flags), `user_achievement_progress`, `evidence_links`, `comments`, `completion_aggregates`.

**UX Notes**  
Accessible spoilers; tabs for set switching; hover tooltips explain flags and stats.

**NFR Hooks**  
Performance (p95 set load ≤ 600 ms), localisation (EN/UA), privacy (no external IDs unless user enabled panels).

**Open Issues**  
None.

---
<br>

#### UC-ACH-02 — Sync Official / Retro Progress (read-only)

**Goal**  
Pull the user’s official (Steam) or retro (RetroAchievements) progress into Dream Project without writing back to providers.

**Scope/Level**  
Sub-function (account-linked).

**Primary Actor**  
Registered user with provider linked.

**Stakeholders & Interests**  
- **User:** Accurate, read-only progress with clear “last sync” time.  
- **Platform:** Respect provider ToS/quotas; degrade gracefully on outages.

**Preconditions**  
- User has linked **Steam** and/or **RA** (see `UC-ACC-06`).  
- Provider reachable (or system handles temporary failure).

**Trigger**  
User clicks **Sync now** on a set page or a scheduled background job runs.

**Main Success Scenario**  
1. System authenticates to provider with stored token/key.  
2. System fetches achievement schema + user completions for the mapped game/variant.  
3. System stores fetched states and timestamps (no PII from provider).  
4. System updates the user’s progress view and **Last sync** indicator.  
5. Aggregate completion stats update asynchronously.

**Alternate/Exception Flows**  
- **A1: Provider outage/quota.** System shows “Sync delayed” and keeps prior data with a **stale** badge.  
- **A2: Private/hidden profile.** System explains limited visibility and skips user-specific completion.  
- **A3: Token invalid.** System marks link **Needs re-link**; no data pulled.

**Postconditions**  
- **Success:** User progress refreshed; audit trail recorded.  
- **Minimal guarantees:** Existing progress remains; no write-backs performed.

**Business Rules**  
- **BA Document §8 — External Profiles & Privacy; Read-Only Sync; Staleness Indicators.**

**Traceability**  
- **BRD:** `BR-INT-02` Steam Linking & Sync, `BR-INT-03` RA Linking & Sync.  
- **SRS:** `SR-ACH-12` (sync job), `SR-INT-05/06` (token use & quotas), `SR-PRO-xx` (profile panels).

**Data Touchpoints**  
`external_links`, `user_achievement_progress`, `sync_runs` (status, durations), `completion_aggregates`.

**UX Notes**  
Clear “Last sync” time, “stale” badge, and link to **Manage connections**.

**NFR Hooks**  
Availability (retry/backoff), observability (sync metrics), compliance (provider ToS).

**Open Issues**  
None.

---
<br>

#### UC-ACH-03 — Create / Edit Fan Achievement Set (curator)

**Goal**  
Allow curators to create and maintain fan achievement sets with clear criteria, flags, hints, and versioning.

**Scope/Level**  
User goal (curator).

**Primary Actor**  
Curator (signed-in, approved role).

**Stakeholders & Interests**  
- **Curator:** Expressive criteria, collaboration, version control, preview.  
- **Platform:** Quality, moderation, and traceability of changes.

**Preconditions**  
- User has **Curator** role.  
- Game/variant exists.

**Trigger**  
Curator clicks **New fan set** or **Edit** on an existing draft.

**Main Success Scenario**  
1. System opens the set editor: title, description, tags (e.g., “subset: low-level”), supported variant(s), spoiler policy, language.  
2. Curator adds achievements with: title, detailed criteria, optional flags (storyline / irreversible / final completion), and optional non-spoiler hints.  
3. Curator can reorder achievements and group them (sections).  
4. System validates fields and autosaves drafts.  
5. Curator **Previews** the set (as users would see it).  
6. Curator **Saves** as draft or **Submits for publish** (if required by policy).

**Alternate/Exception Flows**  
- **A1: Collaborative edits.** Co-curator access allowed; system locks or merges concurrent edits with change history.  
- **A2: Version bump.** Publishing creates a new **version**; existing completions remain tied to prior versions (visible in history).  
- **A3: Policy violations.** If flagged (e.g., spoilers, offensive text), set enters moderation queue.

**Postconditions**  
- **Success:** Draft or published set exists with version and history.  
- **Minimal guarantees:** Unpublished drafts are visible only to curators/admins.

**Business Rules**  
- **BA Document §8 — Fan Set Governance; Versioning; Spoiler Policies; Moderation.**

**Traceability**  
- **BRD:** `BR-ACH-03` Fan Set Authoring, `BR-GOV-02` Review Workflow.  
- **SRS:** `SR-ACH-14` (editor), `SR-ACH-15` (versioning), `SR-GOV-13` (queue).

**Data Touchpoints**  
`achievement_sets` (type=fan, version, state), `achievements`, `set_history`, `collab_permissions`.

**UX Notes**  
Inline previews; clear spoiler indicators; autosave with version notes.

**NFR Hooks**  
Autosave reliability, auditability, localisation.

**Open Issues**  
Whether publish requires moderator approval is policy-controlled (default: curator can self-publish, subject to reporting).

---
<br>

#### UC-ACH-04 — Mark Fan Achievement Complete (with evidence link)

**Goal**  
Record completion of a **fan** achievement with an **evidence link** (allowlisted providers only), updating the user’s progress and set stats.

**Scope/Level**  
User goal (evidence submission).

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Simple proof submission; quick feedback.  
- **Platform:** Evidence integrity without hosting heavy media.

**Preconditions**  
- A published fan set exists.  
- User is signed in.  
- Evidence provider is on the allowlist.

**Trigger**  
User opens an achievement detail and clicks **Mark complete**.

**Main Success Scenario**  
1. System shows a form with **Evidence URL** (required), optional note, and confirmation of the targeted achievement.  
2. User pastes a link (e.g., YouTube timestamp, GameFAQs thread).  
3. System validates URL domain and basic accessibility; stores evidence reference.  
4. System marks the achievement **complete (pending moderation if flagged)** and updates user progress and set aggregates.  
5. Page reflects completion with timestamp; the evidence link is visible per privacy policy.

**Alternate/Exception Flows**  
- **A1: Non-allowlisted URL.** Inline error; cannot submit.  
- **A2: Suspected abuse.** AI/heuristics flag for review; completion visible to the user but marked **Under review** to others.  
- **A3: Duplicate submission.** System prevents duplicates and shows the existing entry.

**Postconditions**  
- **Success:** Achievement completion stored; stats refreshed.  
- **Minimal guarantees:** If flagged, public visibility is limited until moderator action.

**Business Rules**  
- **BA Document §8 — Evidence Policy (links only), Allowlist, Moderation & Visibility.**

**Traceability**  
- **BRD:** `BR-ACH-05` Evidence & Completion, `BR-GOV-01` Reporting.  
- **SRS:** `SR-ACH-18/19` (evidence intake & allowlist), `SR-ACH-22` (progress update), `SR-GOV-15` (flag flow).

**Data Touchpoints**  
`user_achievement_progress`, `evidence_links`, `completion_aggregates`, moderation flags.

**UX Notes**  
Short helper text with examples of accepted URLs; show “Under review” chip when applicable.

**NFR Hooks**  
Security (URL validation), performance (fast post), observability (evidence review queue lengths).

**Open Issues**  
Whether text-only “I did it” notes are allowed without links (default: no).

---
<br>

#### UC-ACH-05 — Comment / Rate Achievement Set

**Goal**  
Enable discussion on achievements/sets and allow users to rate a fan set as a whole.

**Scope/Level**  
Sub-function (engagement & quality).

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Threaded discussion, respectful space; ability to signal quality via rating.  
- **Curator:** Feedback loop to improve sets.  
- **Platform:** Toxicity controls; moderation workflow.

**Preconditions**  
- Set exists (official/retro/fan).  
- User signed in for posting or rating.

**Trigger**  
User opens comments or rating widget.

**Main Success Scenario**  
1. System shows a threaded comments UI (per achievement and per set) and a 1–5 rating control (set-level).  
2. User posts a comment or selects a rating.  
3. System stores the entry; updates average rating (for fan sets); displays the comment with timestamp.  
4. User may edit or delete their comment within a short window (e.g., 10 minutes).

**Alternate/Exception Flows**  
- **A1: Toxicity/abuse detected.** AI/heuristics soft-block or queue for moderation; user sees a warning.  
- **A2: Rate limit exceeded.** Temporarily block further posts/edits.  
- **A3: Locked thread.** Moderators can lock threads; posting disabled.

**Postconditions**  
- **Success:** Comment/rating stored; aggregates updated.  
- **Minimal guarantees:** Audit log kept; removed comments leave a placeholder (“Deleted by moderator/user”).

**Business Rules**  
- **BA Document §8 — Comments & Ratings; Moderation & Appeals; Rate-limits.**

**Traceability**  
- **BRD:** `BR-ACH-06` Comments & Ratings, `BR-GOV-02` Moderation Queue.  
- **SRS:** `SR-ACH-24` (threads), `SR-ACH-25` (ratings), `SR-GOV-12` (triage).

**Data Touchpoints**  
`comments` (entity=achievement or set), `ratings` (set-level), `rating_aggregates`, moderation flags.

**UX Notes**  
Clear code of conduct link; collapse long threads; EN/UA copy.

**NFR Hooks**  
Rate-limits; availability; observability (abuse signals).

**Open Issues**  
Anonymous viewing of comments is allowed; posting requires sign-in (confirmed).

---
<br>

#### UC-ACH-06 — Publish / Unpublish Fan Set (curator)

**Goal**  
Move a fan set between **draft** and **published** states; manage updates via versions; unpublish when necessary.

**Scope/Level**  
User goal (curator) with governance.

**Primary Actor**  
Curator; Moderator/Admin (override).

**Stakeholders & Interests**  
- **Curator:** Control of release timing and revisions.  
- **Platform:** Stability for users; traceable changes.

**Preconditions**  
- Fan set exists in **draft** (for publish) or **published** (for unpublish).  
- No blocking moderation issues.

**Trigger**  
Curator selects **Publish** or **Unpublish** in the set editor.

**Main Success Scenario (Publish)**  
1. System validates required fields and spoilers/hints.  
2. System generates a new **version**, records change notes, sets state to **published**.  
3. Users can now discover the set; stats begin tracking.

**Main Success Scenario (Unpublish)**  
1. Curator selects **Unpublish** and provides a reason (e.g., major rework).  
2. System sets state to **unpublished**; existing completions remain in history but set disappears from discovery (direct links show “Unavailable”).  
3. Curator may continue editing as a new **draft** version.

**Alternate/Exception Flows**  
- **A1: Moderator block.** If policy requires approval, a moderator must approve publish; otherwise queued.  
- **A2: Breaking changes.** If criteria change significantly, system warns that previous completions may not apply to the new version.

**Postconditions**  
- **Success:** State updated; version history preserved; discovery visibility aligns with state.  
- **Minimal guarantees:** User history is not erased; it remains tied to previous versions.

**Business Rules**  
- **BA Document §8 — Fan Set Lifecycle; Versioning & Visibility; Moderation Overrides.**

**Traceability**  
- **BRD:** `BR-ACH-04` Publish Lifecycle, `BR-GOV-03` Overrides.  
- **SRS:** `SR-ACH-15` (versions), `SR-ACH-28` (state transitions), `SR-GOV-05` (admin override).

**Data Touchpoints**  
`achievement_sets` (state, version), `set_history`, `user_achievement_progress` (version refs), `discovery_index`.

**UX Notes**  
Prominent state badges; publish notes visible in history; warning text for breaking edits.

**NFR Hooks**  
Auditability (who/when/what), availability (index refresh), observability (publish/unpublish events).

**Open Issues**  
Whether publish requires moderator approval is policy-controlled (default: curator self-publish).

---
<br>
<br>

### Sticker Albums (ALB)

---

#### UC-ALB-01 — View Album Template (type & spoiler policy)

**Goal**  
Let users inspect a sticker-album **template** for a game/variant—understand its type (e.g., *new-player*), spoiler policy, slot themes, and completion stats—before creating their own album instance.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (progress and “create” actions require sign-in).

**Stakeholders & Interests**  
- **User:** See what the album expects without heavy spoilers; check how many people finished it and how long it typically takes.  
- **Curator:** Communicate intent, spoiler policy, and slot guidance clearly.  
- **Platform:** Consistent rules for spoilers and stats; clean path into creation.

**Preconditions**  
- At least one **album template** exists for the game/variant.

**Trigger**  
User opens the **Albums** tab and selects a template.

**Main Success Scenario**  
1. System shows template header: title, **type** (e.g., *new-player*, *standard*), spoiler policy, curator name.  
2. System lists slots with short, **non-spoiler hints** (e.g., “first arrival at…”, “ally introduction”), hiding exact content by default.  
3. System shows **completion statistics**: % users who finished; recent completers (nickname, avatar, completion time); **average time-to-complete**.  
4. If the user is signed in, system offers **Create my album** (UC-ALB-02).

**Alternate/Exception Flows**  
- **A1: Template unpublished/withdrawn.** Show “Unavailable” notice; only curators/admins can view.  
- **A2: Multiple templates.** Tabs or selector let the user switch themes without losing context.

**Postconditions**  
- **Success:** User understands expectations, spoiler posture, and community stats.  
- **Minimal guarantees:** No slot content is revealed unless the user later opts to reveal.

**Business Rules**  
- **BA Document §8 — Sticker Albums & Screenshot Rules; Spoiler Policies; Completion Statistics; Content Ownership.**

**Traceability**  
- **BRD:** `BR-ALB-01` Template View, `BR-ALB-05` Completion Stats.  
- **SRS:** `SR-ALB-01` (template display), `SR-ALB-07` (stats panel), `SR-ALB-12` (spoiler policy).

**Data Touchpoints**  
`album_templates`, `album_slots`, `completion_aggregates`, `recent_completions`.

**UX Notes**  
Clear “type” badges; slot hints are short and spoiler-safe; EN/UA copy.

**NFR Hooks**  
Performance (p95 ≤ 500 ms), localisation, privacy (show only public profile fields in completers list).

**Open Issues**  
None.

---
<br>

#### UC-ALB-02 — Create Album Instance

**Goal**  
Let a signed-in user create their **own album instance** from a chosen template, selecting spoiler posture and enabling the “next slot preview” helper so they don’t miss key moments.

**Scope/Level**  
User goal.

**Primary Actor**  
Registered user.

**Stakeholders & Interests**  
- **User:** Quick start; spoiler control; guidance to avoid missing shots.  
- **Curator:** Template intent respected.  
- **Platform:** Consistent defaults; clean lifecycle and stats.

**Preconditions**  
- User signed in.  
- A template is available.

**Trigger**  
User clicks **Create my album**.

**Main Success Scenario**  
1. System shows a short setup: confirm template, choose **spoiler posture** (*strict* for new-player, or *standard*), and toggle **Next Slot Preview** (on by default).  
2. User confirms.  
3. System creates the **album instance** with 0/X slots filled.  
4. System opens the instance view: the **next slot** is previewed via a vague hint (icon/text) without revealing specifics; other locked slots remain blurred.  
5. System shows progress and a **completion badge** placeholder.

**Alternate/Exception Flows**  
- **A1: Existing instance.** If policy allows only one active instance per template, system offers to open the existing one.  
- **A2: Template removed later.** Instance remains usable; template page shows “Template updated/removed” notice.

**Postconditions**  
- **Success:** Album instance exists; user is ready to start filling slots.  
- **Minimal guarantees:** Spoiler posture and preview setting stored as user preferences for this instance.

**Business Rules**  
- **BA Document §8 — Sticker Albums: Creation; Spoiler Posture; Next-Slot Preview; Ownership.**

**Traceability**  
- **BRD:** `BR-ALB-02` Instance Creation, `BR-ALB-07` Next-Slot Preview.  
- **SRS:** `SR-ALB-02` (instance create), `SR-ALB-10` (preview helper), `SR-ALB-12` (spoiler controls).

**Data Touchpoints**  
`album_instances` (user_id, template_id, spoiler_posture, preview_on), progress fields.

**UX Notes**  
Short, calm language for spoiler choices; preview hint stays abstract; EN/UA.

**NFR Hooks**  
Availability (create is fast and durable), observability (instance creation metrics).

**Open Issues**  
Multiple concurrent instances per template are policy-controlled (default: one active per user).

---
<br>

#### UC-ALB-03 — Fill Slot with Screenshot (AI triage)

**Goal**  
Let users upload a screenshot into a specific slot; the system performs **AI triage** to verify consistency with the slot’s hint/criteria and filters inappropriate content before accepting.

**Scope/Level**  
User goal (content submission).

**Primary Actor**  
Registered user (owner of the album instance).

**Stakeholders & Interests**  
- **User:** Low-friction upload; quick pass/fail feedback; clear guidance if uncertain.  
- **Curator:** Slot intent respected; low spam/noise.  
- **Platform:** Safety (AV, moderation), consistent validations.

**Preconditions**  
- Album instance exists.  
- Slot is available to fill (unlocked by order/policy).

**Trigger**  
User selects a slot and clicks **Add screenshot** (drag-and-drop or picker).

**Main Success Scenario**  
1. System accepts the image file and runs **AV scan**.  
2. System runs **AI triage** against the slot descriptor (template text and hints).  
3. If **pass**, system stores the image, marks the slot **filled**, and updates progress.  
4. If **uncertain**, system stores the image as **pending review**; the slot shows **“Pending”** with guidance and an estimated review window.  
5. System updates the **Next Slot Preview** panel.  
6. User sees progress increment and can proceed to the next slot.

**Alternate/Exception Flows**  
- **A1: Hard fail (policy/NSFW/mismatch).** System rejects with a clear reason; no image published.  
- **A2: Duplicate image.** System warns and allows replace/cancel.  
- **A3: Oversized/unsupported format.** System rejects with limits and tips.  
- **A4: Manual re-submit.** User can replace a **pending** image with a new one.

**Postconditions**  
- **Success:** Slot filled (or pending); progress updated; preview advances.  
- **Minimal guarantees:** Rejected files are discarded; pending items are clearly marked and hidden from public lists until approved.

**Business Rules**  
- **BA Document §8 — Screenshot Upload & Validation; AI Triage; Moderation & Appeals; Content Ownership.**

**Traceability**  
- **BRD:** `BR-ALB-03` Slot Fill & Validation, `BR-GOV-01` Reporting.  
- **SRS:** `SR-ALB-03` (upload), `SR-ALB-04` (AI triage states), `SR-ALB-05` (pending visibility), `SR-GOV-11` (AV/moderation).

**Data Touchpoints**  
`slot_fills` (image_ref, state=accepted|pending|rejected), `ai_triage` (confidence, reasons), `album_instances` (progress), audit events.

**UX Notes**  
Show simple pass/pending/fail states; provide brief “why” text; keep spoilers hidden unless user opts to reveal.

**NFR Hooks**  
Security (AV, signed URLs), performance (triage SLA), observability (triage outcomes, false-positive rate).

**Open Issues**  
Exact triage thresholds are policy-controlled; defaults can be tuned post-MVP.

---
<br>

#### UC-ALB-04 — Reveal / Hide Spoiler Slots

**Goal**  
Allow users to temporarily **reveal** a spoiler-marked slot to check what’s needed, then **hide** it again to keep the experience safe—especially for *new-player* templates.

**Scope/Level**  
Sub-function (view control).

**Primary Actor**  
Owner of the album instance.

**Stakeholders & Interests**  
- **User:** Avoid missing key moments without ruining the story.  
- **Curator:** Maintain intended spoiler posture.

**Preconditions**  
- Album instance exists.  
- Template or slot is marked spoiler-sensitive.

**Trigger**  
User clicks **Reveal** on a slot or toggles the global spoiler setting for the instance.

**Main Success Scenario**  
1. System asks for quick confirmation (short tooltip text).  
2. System reveals the needed hint/descriptor for that slot only (or per global toggle), keeping other spoilered content hidden.  
3. After filling or moving on, the user can click **Hide** to re-blur.  
4. The global **Next Slot Preview** remains non-spoiler and does not auto-reveal content.

**Alternate/Exception Flows**  
- **A1: Strict posture.** If the user chose *strict* at creation, system requires an extra confirm step (“One-time reveal”).  
- **A2: Shared device/privacy.** Option to auto-revert to hidden after leaving the page.

**Postconditions**  
- **Success:** The user learns just enough to proceed, then returns to spoiler-safe mode.  
- **Minimal guarantees:** Reveal choices affect only the current user/instance, not other users.

**Business Rules**  
- **BA Document §8 — Spoiler Policies; User Controls; Privacy.**

**Traceability**  
- **BRD:** `BR-ALB-06` Spoiler Controls & Preview.  
- **SRS:** `SR-ALB-10` (preview helper), `SR-ALB-12` (spoiler toggles).

**Data Touchpoints**  
`album_instances` (user spoiler preference), ephemeral UI state.

**UX Notes**  
Use gentle language; make **Reveal** visually distinct from normal actions; EN/UA copy.

**NFR Hooks**  
Accessibility (keyboard and screen-reader cues), localisation, privacy.

**Open Issues**  
Whether reveals should persist across sessions is policy-controlled (default: persist per instance).

---
<br>

#### UC-ALB-05 — Complete Album & Earn Badge

**Goal**  
When all slots are **accepted**, mark the album instance as **complete**, award a game-specific **badge**, and update completion statistics (including **time-to-complete**).

**Scope/Level**  
User goal (completion).

**Primary Actor**  
Owner of the album instance.

**Stakeholders & Interests**  
- **User:** Clear moment of completion; badge on profile.  
- **Platform:** Reliable stats; no premature completions.

**Preconditions**  
- Every slot is **accepted** (not pending).  
- No open moderation case blocks completion.

**Trigger**  
User fills the final required slot or clicks **Check completion**.

**Main Success Scenario**  
1. System verifies that all slots are accepted.  
2. System marks the album instance **complete** and records **completion_time** (from instance creation to last accepted slot).  
3. System mints a **badge** for the user’s profile specific to the game/template.  
4. System updates **completion statistics** for the template: percentage completed, recent completers list (nickname, avatar, timestamp, time-to-complete).  
5. UI shows a celebratory state and links back to the template page.

**Alternate/Exception Flows**  
- **A1: Pending or rejected slots remain.** System explains which slots block completion; offers to review/replace.  
- **A2: Template changed mid-run.** Instance remains valid; completion still bound to the slots the user filled.  
- **A3: Privacy setting.** If the user’s profile is private, the completers list shows anonymised entry (policy-controlled).

**Postconditions**  
- **Success:** Album is complete; badge visible on profile; stats refreshed.  
- **Minimal guarantees:** If the badge mint fails, instance remains complete; a background job retries badge issuance.

**Business Rules**  
- **BA Document §8 — Completion & Badges; Stats; Privacy; Content Ownership.**

**Traceability**  
- **BRD:** `BR-ALB-04` Completion & Badge, `BR-ALB-05` Stats.  
- **SRS:** `SR-ALB-08` (completion checks), `SR-ALB-09` (badge mint), `SR-ALB-07` (stats update), `SR-OBS-xx` (events).

**Data Touchpoints**  
`album_instances` (state=complete, completion_time), `badges`, `completion_aggregates`, profile view.

**UX Notes**  
Simple celebration; link to share (optional in MVP); EN/UA copy.

**NFR Hooks**  
Reliability (idempotent completion), observability (event logging), availability (badge/job retries).

**Open Issues**  
Template versioning policy for albums (whether instances pin a specific template revision) — to be finalised during the end-to-end audit.

---
<br>
<br>

### Profiles & Progress (PRO)

---

#### UC-PRO-01 — View Profile & Panels (Steam/RA read-only)

**Goal**  
Let visitors (and the owner) view a user’s profile with optional **read-only panels** for Steam/RetroAchievements, plus badges and recent activity—without exposing external IDs unless the owner opted in.

**Scope/Level**  
User goal (view).

**Primary Actor**  
Any visitor; profile owner.

**Stakeholders & Interests**  
- **Viewer:** Quick understanding of the user’s activity, completions, and badges.  
- **Owner:** Control over what’s public; external panels only if enabled.  
- **Platform:** Respect privacy and locale; never show catalogue policy flags on profiles.

**Preconditions**  
- Profile exists.  
- Visibility permits viewing (public; or private when owner is signed in).  
- External panels shown only if owner enabled them.

**Trigger**  
Viewer opens a profile page.

**Main Success Scenario**  
1. System renders **public profile** fields (display name, avatar) and high-level stats (completed albums, completed achievement sets, badges).  
2. If owner enabled panels, system shows **read-only** Steam/RA summaries (e.g., recent official/retro completions) **without revealing external IDs unless the owner opted to display them**.  
3. System lists recent activity (e.g., completed album, fan achievement completion with evidence icon, uploaded save) with timestamps.  
4. System localises UI (EN/UA) per viewer or owner preference.  
5. Catalogue policy flags (UA/Russia) are **not** displayed on profiles.

**Alternate/Exception Flows**  
- **A1: Private profile (viewer ≠ owner).** Show minimal card (“This profile is private”).  
- **A2: Stale sync for external panels.** Show a **stale** badge with last-sync timestamp.  
- **A3: Panels disabled.** Hide them entirely; show a neutral placeholder to the owner only.

**Postconditions**  
- **Success:** Profile is displayed according to privacy and panel settings.  
- **Minimal guarantees:** No external identifiers are exposed unless the owner explicitly enabled them.

**Business Rules**  
- **BA Document §8 — Privacy & Visibility; External Panels (read-only, opt-in); Badges & Activity; Policy Visibility Scope.**

**Traceability**  
- **BRD:** `BR-PRO-01` Profile View, `BR-PRO-03` Badges & Activity.  
- **SRS:** `SR-PRO-01` (profile rendering), `SR-PRO-05` (external panel rules), `SR-PRO-07` (activity feed), `SR-CAT-12` (policy scope constraint).

**Data Touchpoints**  
`users_public`, `user_settings(visibility, panels)`, `badges`, `activity_log`, `external_links(summary, last_synced_at)`.

**UX Notes**  
Compact, scannable layout; clear “stale” chip; no policy banners on profiles.

**NFR Hooks**  
Localisation (EN/UA), privacy, performance (p95 ≤ 500 ms), observability (profile view metrics).

**Open Issues**  
Vanity URLs (e.g., `/u/<handle>`) are optional; default is `/profile/<id>`.

---
<br>

#### UC-PRO-02 — Adjust Profile Visibility & Panels

**Goal**  
Allow the owner to switch profile **visibility** (public/private) and toggle **external panels** (Steam/RA) on or off.

**Scope/Level**  
User goal (settings).

**Primary Actor**  
Profile owner (signed in).

**Stakeholders & Interests**  
- **Owner:** Clear control over what others see.  
- **Platform:** Apply changes consistently and immediately.

**Preconditions**  
- Owner is signed in.  
- Linked accounts exist to enable corresponding panels.

**Trigger**  
Owner opens **Profile Settings**.

**Main Success Scenario**  
1. System shows current **visibility** (public/private) and panel toggles (Steam, RA) with brief explanations.  
2. Owner changes visibility and/or toggles panels.  
3. System validates dependencies (e.g., cannot enable Steam panel if not linked).  
4. System persists changes; profile page reflects them immediately.

**Alternate/Exception Flows**  
- **A1: Not linked but panel toggled on.** Inline error + link to **Manage connections**.  
- **A2: Rapid toggling.** Rate-limit to prevent spam.

**Postconditions**  
- **Success:** New visibility/panel settings are active.  
- **Minimal guarantees:** Partial updates are not applied; changes are atomic.

**Business Rules**  
- **BA Document §8 — Privacy & Visibility; External Panels (opt-in); Rate-limits.**

**Traceability**  
- **BRD:** `BR-PRO-02` Visibility & Panels.  
- **SRS:** `SR-PRO-04` (settings), `SR-ACC-13/14` (locale/privacy persistence), `SR-ACC-16..19` (link/unlink prerequisites).

**Data Touchpoints**  
`user_settings(visibility, panels)`, `external_links(state)`.

**UX Notes**  
Plain language; small preview of how the profile looks to others.

**NFR Hooks**  
Security (authorisation), localisation, performance.

**Open Issues**  
None.

---
<br>

#### UC-PRO-03 — View Progress & Stats

**Goal**  
Provide the owner with a consolidated view of **progress** (albums, achievements) and **stats** (counts, time-to-complete), including **stale** indicators and quick links to resume.

**Scope/Level**  
User goal (self view).

**Primary Actor**  
Profile owner (signed in).

**Stakeholders & Interests**  
- **Owner:** See where they stand and what to do next.  
- **Platform:** Consistent aggregation across pillars; minimal PII.

**Preconditions**  
- Owner is signed in.  
- Progress data exists or is initialised.

**Trigger**  
Owner opens **My Progress** from their profile.

**Main Success Scenario**  
1. System shows summary cards: **Albums** (in progress / completed, average time-to-complete), **Achievements** (official/retro synced, fan completed), and **Saves** (uploads/downloads by others).  
2. Cards include **stale** badges where external syncs are outdated, with a **Sync now** link (read-only).  
3. System lists actionable items (e.g., “3 slots pending review”, “2 fan achievements awaiting moderator review”, “Continue album: 5/12”).  
4. Owner can navigate directly to the relevant album/set.

**Alternate/Exception Flows**  
- **A1: No data yet.** Empty state with short guidance (discover games, link accounts, start an album).  
- **A2: Provider unreachable for sync.** Show non-blocking warning; keep last known data.

**Postconditions**  
- **Success:** Owner has a clear snapshot and next steps.  
- **Minimal guarantees:** Aggregates never expose other users’ PII.

**Business Rules**  
- **BA Document §8 — Stats & Progress; Read-only Sync; Evidence & Pending States.**

**Traceability**  
- **BRD:** `BR-PRO-03` Badges & Progress, `BR-ACH-09` Completion Stats.  
- **SRS:** `SR-PRO-08` (dashboard), `SR-ACH-12` (sync), `SR-ALB-07..09` (album stats), `SR-SAV-xx` (save counts).

**Data Touchpoints**  
`completion_aggregates`, `user_achievement_progress`, `album_instances`, `badges`, `sync_runs`.

**UX Notes**  
Highlight actionable items; concise, friendly labels; EN/UA.

**NFR Hooks**  
Performance (p95 ≤ 600 ms), privacy, observability (dashboard usage).

**Open Issues**  
None.

---
<br>

#### UC-PRO-04 — Apply to Become Curator (admin decision)

**Goal**  
Let a user apply for the **Curator** role; route the application to admins for review and decision.

**Scope/Level**  
User goal with governance.

**Primary Actor**  
Registered user (not yet a curator).

**Stakeholders & Interests**  
- **Applicant:** Clear criteria, transparent outcome.  
- **Admin:** Enough signal (examples, links) to decide quickly.

**Preconditions**  
- User is signed in and not already a curator.  
- Applications are open.

**Trigger**  
User clicks **Apply to become a Curator**.

**Main Success Scenario**  
1. System displays the application form (short bio, target genres/platforms, links to relevant work: RA sets, guides, videos, repos, etc.).  
2. User submits; system validates links (allowlist) and rate-limits resubmissions.  
3. System creates an **application** record and notifies admins.  
4. Admin reviews and records a decision (approve/decline) with optional notes.  
5. System notifies the applicant; on approval, role updates to **Curator** instantly.

**Alternate/Exception Flows**  
- **A1: Insufficient information.** System prompts for required fields; cannot submit.  
- **A2: Duplicate pending application.** Block resubmission; show status.  
- **A3: Declined with reapply window.** System sets a cooldown before next attempt.

**Postconditions**  
- **Success:** Applicant receives decision; role updated on approval.  
- **Minimal guarantees:** Full audit trail of decision and who approved.

**Business Rules**  
- **BA Document §8 — Roles & Promotion; Evidence Allowlist; Rate-limits; Auditability.**

**Traceability**  
- **BRD:** `BR-PRO-04` Curator Onboarding, `BR-GOV-03` Admin Decisions.  
- **SRS:** `SR-PRO-11` (application intake), `SR-GOV-17` (decision workflow), `SR-ACC-xx` (role update).

**Data Touchpoints**  
`curator_applications` (bio, links, status), `users.role`, audit events, notifications.

**UX Notes**  
Stateful application page (“Submitted”, “Under review”, “Decision”); respectful copy; EN/UA.

**NFR Hooks**  
Security (authorisation), availability (queue/notifications), observability (time-to-decision).

**Open Issues**  
Whether approval requires one or two admin votes—finalise in governance policy.

---
<br>
<br>

### Moderation & Governance (GOV)

---

#### UC-GOV-01 — Report Item (save / achievement / screenshot / comment)

**Goal**  
Allow users to report problematic content (malware/corruption, policy or conduct violations, spoilers violations, wrong variant/build, NSFW) so moderators can act quickly.

**Scope/Level**  
Sub-function (governance intake).

**Primary Actor**  
Signed-in user (reporter).

**Stakeholders & Interests**  
- **Reporter:** Fast, safe way to flag issues; visible status.  
- **Content owner:** Fair process; clear outcome and option to appeal.  
- **Moderator/Admin:** Enough context to decide efficiently; immutable audit trail.  
- **Platform:** Safety, policy compliance, low false positives.

**Preconditions**  
- Reporter is signed in.  
- The target item exists and is currently visible (or recently visible).  
- Moderation service is available.

**Trigger**  
User clicks **Report** on a save, achievement (or set), screenshot/album slot, or comment.

**Main Success Scenario**  
1. System opens a small form with categories (malware/corruption, spoiler/policy breach, wrong variant/build, NSFW/abuse, other) and optional notes + optional evidence link (allowlisted).  
2. User submits; system rate-limits repeated reports on the same item.  
3. System creates a **moderation case** with reporter, target, category, notes, and any evidence link.  
4. System marks the target with a lightweight **Under review** banner (configurable per category and severity).  
5. Reporter sees a confirmation and a link to their **Reports** list.

**Alternate/Exception Flows**  
- **A1: Duplicate active case.** System attaches a “me-too” to the open case and informs the user; avoids duplicate queues.  
- **A2: Abuse/spam by reporter.** Temporarily block further reports; notify user with cooldown.  
- **A3: Critical category (e.g., suspected malware).** System auto-quarantines the file (no downloads) pending decision.

**Postconditions**  
- **Success:** A case exists in the moderation queue; optional banner/quarantine applied.  
- **Minimal guarantees:** Audit log records who reported what, when, and why.

**Business Rules**  
- **BA Document §8 — Moderation & Reporting; Evidence Allowlist; Rate-Limits; Quarantine Policy; Auditability.**

**Traceability**  
- **BRD:** `BR-GOV-01` Reporting Intake.  
- **SRS:** `SR-GOV-11` (report form & categories), `SR-GOV-12` (case open), `SR-SAV-22` (quarantine state).

**Data Touchpoints**  
`reports`, `moderation_cases` (status, category, target_ref), `audit_events`, item state (quarantine flag).

**UX Notes**  
Short, respectful copy; show what happens next; EN/UA. Keep policy banners scoped to **catalogue/discovery** only (never display UA/RU policy banners on moderation pages).

**NFR Hooks**  
Rate-limits; availability (queue durability); observability (time-to-first-action, category distribution).

**Open Issues**  
Exact auto-quarantine categories configurable; default: malware/corruption only.

---
<br>

#### UC-GOV-02 — Moderator Triage & Decision

**Goal**  
Enable moderators/admins to triage cases, review context and history, and decide (keep, edit/correct, quarantine, remove, or escalate), with optional overrides (e.g., compatibility status).

**Scope/Level**  
Operational/administrative.

**Primary Actor**  
Moderator/Admin.

**Stakeholders & Interests**  
- **Moderator/Admin:** Clear queue, full context, reversible actions where possible.  
- **Content owner:** Transparent decision with reasons; appeal path.  
- **Reporter:** Resolution and rationale (when appropriate).  
- **Platform:** Consistency, safety, auditability.

**Preconditions**  
- User has Moderator/Admin role.  
- Open cases exist in the queue.  
- Required context retrievable (item history, confirmations, AI triage results, evidence links).

**Trigger**  
Moderator opens **Moderation Queue** and selects a case.

**Main Success Scenario**  
1. System shows the case summary: target item preview, category, report notes/evidence link, item owner, prior actions, related cases.  
2. System displays context tabs: **History** (uploads/edits), **AI triage** (e.g., screenshot result/confidence), **Compatibility** (for saves: current status & confirmations), **Policy** (catalogue-only flags—visible for context, not shown to end users here).  
3. Moderator chooses an action:  
   - **Keep** (no issue found).  
   - **Edit/Correct** (e.g., fix wrong variant/build mapping).  
   - **Quarantine** (temporarily hide/disable downloads or visibility).  
   - **Remove** (take down item; optionally anonymise comments).  
   - **Override** (set compatibility to confirmed/legacy/incompatible with a note).  
   - **Escalate** (legal/security).  
4. Moderator enters a short rationale and saves the decision.  
5. System updates item state, notifies the owner and (optionally) the reporter, and closes the case.

**Alternate/Exception Flows**  
- **A1: Conflicting evidence.** Moderator can **Request more info** from the owner or reporter; case remains **Pending**.  
- **A2: Repeat offender.** System suggests stricter action (e.g., temporary posting ban) per policy; moderator confirms.  
- **A3: Decision rollback.** Admin can revert last action (where safe) via audit-backed undo; not available for malware removals.

**Postconditions**  
- **Success:** Case closed with a recorded decision; item state updated; notifications sent.  
- **Minimal guarantees:** Full audit trail (who/when/what/why); user-visible banners updated accordingly.

**Business Rules**  
- **BA Document §8 — Case Workflow; Decision Types; Overrides; Notifications; Appeals; Auditability.**

**Traceability**  
- **BRD:** `BR-GOV-02` Moderator Workflow, `BR-SAV-06` Compatibility Overrides.  
- **SRS:** `SR-GOV-12` (case UI/actions), `SR-GOV-14` (notifications), `SR-SAV-27` (compat aggregation & override notes).

**Data Touchpoints**  
`moderation_cases` (status, decision, rationale), target entities (`saves`, `slot_fills`, `achievement_sets`, `comments`), `compat_index`, `notifications`, `audit_events`.

**UX Notes**  
Keyboard-friendly queue; high-contrast status chips; short decision templates to standardise rationale.

**NFR Hooks**  
Availability (queue), security (role checks), observability (time-to-decision, reopen rate).

**Open Issues**  
Standard decision templates library to be authored with Ops; MVP may start with a minimal set.

---
<br>

#### UC-GOV-03 — Submit Appeal

**Goal**  
Allow a content owner to **appeal** a moderation decision within a fixed window; route the appeal to an independent review.

**Scope/Level**  
User goal with governance.

**Primary Actor**  
Content owner (signed in).

**Stakeholders & Interests**  
- **Owner:** Fair chance to provide context/evidence.  
- **Moderator/Admin:** Efficient second-look process; low rework.  
- **Platform:** Final, auditable resolution.

**Preconditions**  
- A decision exists on the owner’s item (e.g., removal/quarantine).  
- Appeal window is still open (e.g., 7 days by policy).

**Trigger**  
Owner clicks **Appeal decision** from the decision notice or item page.

**Main Success Scenario**  
1. System displays the decision summary and the **appeal form** (short statement + optional evidence link, allowlisted).  
2. Owner submits; system validates and rate-limits multiple appeals on the same case.  
3. System creates an **appeal** record linked to the original case and routes it to an **independent** queue (different reviewer if possible).  
4. Reviewer examines context, new evidence, and prior rationale.  
5. Reviewer decides: **Uphold**, **Modify**, or **Reverse**.  
6. System updates item state if needed, records the final decision, and notifies the owner (and optionally original reporter).

**Alternate/Exception Flows**  
- **A1: Appeal window expired.** System blocks appeal and explains the deadline.  
- **A2: Duplicate appeal.** Reuse existing appeal record; show status.  
- **A3: Abuse/spam.** Deny further appeals and note policy breach.

**Postconditions**  
- **Success:** Final decision recorded; item state reconciled; notifications sent.  
- **Minimal guarantees:** Complete audit chain across original decision and appeal.

**Business Rules**  
- **BA Document §8 — Appeals Window & Process; Independent Review; Evidence Allowlist; Notifications; Auditability.**

**Traceability**  
- **BRD:** `BR-GOV-03` Appeals.  
- **SRS:** `SR-GOV-16` (appeal intake), `SR-GOV-17` (independent review), `SR-GOV-18` (finalisation).

**Data Touchpoints**  
`appeals` (statement, evidence_link, status), `moderation_cases` (link), target entity state, `audit_events`, `notifications`.

**UX Notes**  
Plain-language deadlines; show who will review (role, not identity); EN/UA.

**NFR Hooks**  
Availability (queue durability), security (authorisation), observability (appeal turn-around time).

**Open Issues**  
Define SLA targets (e.g., time-to-first-response ≤ 48h; final decision ≤ 7 days) during the end-to-end audit.

---
<br>
<br>

### Discovery & Search (DSC)

---

#### UC-DSC-01 — Global Search & Typeahead

**Goal**  
Let users quickly find games (and sometimes direct links to variants, sets, or saves) via a single search box with **typeahead** suggestions.

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (anon or signed-in).

**Stakeholders & Interests**  
- **User:** Fast, tolerant search with good suggestions and keyboard navigation.  
- **Platform:** Relevant results; consistent badges; no leakage of private data.

**Preconditions**  
- Search index is available and warm.  
- Policy preference (UA/Russia banners/filters) is loaded (account or cookie).

**Trigger**  
User focuses the global search box and starts typing.

**Main Success Scenario**  
1. System normalises input (case/diacritics), applies **EN/UA** tokenisation and simple transliteration (e.g., кирилиця ⇄ Latin).  
2. As the user types (≥2 chars), system shows **typeahead**: top games by relevance, with small cover, year, and **pillar badges** (Saves/Achievements/Albums).  
3. User selects a suggestion (Enter/Click) or submits the full query.  
4. System routes to the **Catalogue results** view with the query applied (see UC-CAT-01).

**Alternate/Exception Flows**  
- **A1: No suggestions yet.** Show “Keep typing” and recently popular titles (non-personal).  
- **A2: Network hiccup.** Gracefully degrade to a local minimal hint (“Press Enter to search”).  
- **A3: Very long queries.** Truncate safely; still allow full search.

**Postconditions**  
- **Success:** User lands on a sensible results page or opens a suggested game directly.  
- **Minimal guarantees:** No user profile data is surfaced in suggestions.

**Business Rules**  
- **BA Document §8 — Search Semantics & Facets; Pillar Badges; Policy Visibility (catalogue/discovery only).**

**Traceability**  
- **BRD:** `BR-DSC-01` Global Search & Typeahead.  
- **SRS:** `SR-DSC-01` (typeahead), `SR-DSC-02` (normalisation), `SR-DSC-03` (routing).

**Data Touchpoints**  
`catalog_index` (games & variants), `suggest_index` (popular queries), policy prefs (client or `user_settings`).

**UX Notes**  
Keyboard support (↑/↓, Enter, Esc); accessible announcement of suggestion count; EN/UA copy.

**NFR Hooks**  
Performance: typeahead p95 ≤ **150 ms** per keystroke; results p95 ≤ **700 ms**. Availability: serve last-snapshot if the index is refreshing.

**Open Issues**  
- Synonyms/aliases list (community-contributed?) — to define.  
- Per-locale stemming improvements post-MVP.

---
<br>

#### UC-DSC-02 — Apply Facets & Policy Toggles

**Goal**  
Let users refine search results with facets (platform, region, year range, pillar availability, compatibility status) and **policy toggles** (show banners / filter RU-linked titles).

**Scope/Level**  
User goal.

**Primary Actor**  
Any visitor (anon or signed-in).

**Stakeholders & Interests**  
- **User:** Predictable filtering that doesn’t “lose” the query.  
- **Platform:** Keep policy presentation **scoped** to discovery; persist preferences cleanly.

**Preconditions**  
- A result set is displayed (from UC-DSC-01 or direct navigation).  
- Policy module enabled.

**Trigger**  
User opens the facets panel and/or the **Policy** section.

**Main Success Scenario**  
1. System displays facets: **Platform**, **Region (NTSC/PAL)**, **Release year range**, **Pillars** (Saves/Achievements/Albums), **Save compatibility** (confirmed/untested/legacy/incompatible).  
2. System displays **Policy** controls: **Show policy banners** (on/off) and **Filter RU-linked titles** (on/off).  
3. User adjusts facets/toggles; results update instantly or on **Apply** (configurable).  
4. System persists policy preference (account or cookie) and keeps facets in the URL (shareable).

**Alternate/Exception Flows**  
- **A1: No matches after filtering.** Show empty state with quick **Clear filters**.  
- **A2: Anon + private mode.** Policy preference lasts for the session only.  
- **A3: Conflicting facets.** System greys out impossible combinations.

**Postconditions**  
- **Success:** Result set reflects selected filters; policy scope remains limited to discovery.  
- **Minimal guarantees:** No policy banners on user profiles or achievement pages.

**Business Rules**  
- **BA Document §8 — Facet Semantics; Pillar Availability; Compatibility Status; Policy Visibility Scope & Defaults.**

**Traceability**  
- **BRD:** `BR-DSC-02` Facets & Policy Controls; `BR-CAT-04` Policy Scope.  
- **SRS:** `SR-DSC-08` (facets), `SR-DSC-14` (policy toggles), `SR-CAT-12` (scope enforcement).

**Data Touchpoints**  
`catalog_index` (facet fields), compatibility aggregates, policy flags, `user_settings.policy_prefs` or client storage.

**UX Notes**  
Facet chips with one-click removal; URL reflects filters; tooltips explain **compatibility** statuses.

**NFR Hooks**  
Performance (facet reflow p95 ≤ **400 ms**), localisation, privacy (no user IDs in URLs).

**Open Issues**  
Consider saved searches in a later phase.

---
<br>

#### UC-DSC-03 — View Result Details (compatibility badges, availability)

**Goal**  
From a results list, let users open a title’s detail panel to preview **variant availability**, **pillar badges**, and **compatibility** at a glance before navigating to a full page.

**Scope/Level**  
Sub-function (details preview).

**Primary Actor**  
Any visitor.

**Stakeholders & Interests**  
- **User:** Quick, low-friction peek at variant differences and compatibility.  
- **Platform:** Consistent semantics with the game page; no duplicate logic.

**Preconditions**  
- A result list is visible (UC-DSC-01/02).

**Trigger**  
User clicks a **Quick View** control (or hovers, if enabled) on a search result.

**Main Success Scenario**  
1. System opens an inline **Quick View**: cover, year, short description, and a **Variants** mini-table (Platform / Region / Build).  
2. For each variant, system shows **Pillar badges** and a compact **Save compatibility** chip (counts of confirmed/untested/legacy/incompatible).  
3. If UA/Russia flags exist, a small **catalogue-only** banner is shown inside the panel (hidden if user toggled it off).  
4. User can click **Open full page** (UC-CAT-02) or jump directly to **Saves**, **Achievements**, or **Albums** tabs.

**Alternate/Exception Flows**  
- **A1: Data still loading.** Skeleton UI shown for ≤500 ms; then either content or a “Try again” link.  
- **A2: Unknown build/manifest.** Compatibility shows **untested** with a hint (“Confirmations welcome”).  
- **A3: Saves unavailable for all variants.** Show “Saves not supported (e.g., account-bound multiplayer)” with a learn-more link.

**Postconditions**  
- **Success:** User decides whether to navigate deeper, with clarity on availability and compatibility.  
- **Minimal guarantees:** Policy banners remain scoped to discovery; no profile data shown.

**Business Rules**  
- **BA Document §8 — Variant Mapping & Pillar Availability; Version Compatibility (status taxonomy); Policy Visibility (discovery only).**

**Traceability**  
- **BRD:** `BR-DSC-03` Result Details Preview; `BR-SAV-02` Compatibility Surfacing.  
- **SRS:** `SR-DSC-12` (quick view), `SR-CAT-04` (variant data), `SR-SAV-xx` (compatibility chips).

**Data Touchpoints**  
`catalog_index` (denormalised variant rows), `compat_index` (status counts), policy flags.

**UX Notes**  
Compact, keyboard-accessible panel; consistent badge labels; EN/UA copy; clear “Open full page” affordance.

**NFR Hooks**  
Performance (panel open p95 ≤ **300 ms**), availability (serve cached snapshot), observability (panel open rate and dwell time).

**Open Issues**  
Decide whether to support direct deep-links to a specific variant tab from Quick View in MVP.

---
<br>
<br>

### External Integrations & Adapters (INT)

---

#### UC-INT-01 — Manually Trigger Sync (Steam / RetroAchievements)

**Goal**  
Allow a linked user to **manually trigger** a read-only sync of official (Steam) and/or retro (RetroAchievements) achievement progress for a mapped game/variant, with safe handling of provider quotas/outages.

**Scope/Level**  
Sub-function (account-linked, read-only).

**Primary Actor**  
Registered user with at least one provider linked.

**Stakeholders & Interests**  
- **User:** Fresh progress on demand; clear “last sync” time; no risk to external accounts.  
- **Platform:** Token safety, ToS compliance, controlled rate/quotas, graceful degradation.

**Preconditions**  
- User has linked Steam and/or RA (see `UC-ACC-06`).  
- Game ↔ provider mapping exists for the chosen title/variant.  
- Sync service is available; provider reachable or handled via retry/backoff.

**Trigger**  
User clicks **Sync now** on a set page or from **My Progress**.

**Main Success Scenario**  
1. System verifies link validity and current **sync-cooldown** (e.g., not more than once every _N_ minutes per provider/user).  
2. System enqueues a **sync job** (idempotent) with `(user_id, provider, game/variant mapping)`.  
3. Worker authenticates using the stored encrypted token/key and fetches: achievement schema + user completion states.  
4. Worker **stores** fetched states (no PII from provider), updates `user_achievement_progress`, and recalculates aggregates asynchronously.  
5. UI updates **Last sync: <timestamp>**; stale badge disappears.

**Alternate/Exception Flows**  
- **A1: Cooldown active.** UI shows “Try again in ~Xm” and disables the button until cooldown ends.  
- **A2: Provider outage/quota (HTTP 5xx / 429).** Worker backs off with exponential retry; UI shows **Stale** with “Provider busy” note.  
- **A3: Token invalid/expired.** Job fails fast; link marked **Needs re-link**; user is prompted to reconnect.  
- **A4: Mapping missing.** UI explains the title/variant isn’t mapped; offers to **Suggest a correction** (UC-CAT-04).  
- **A5: Partial fetch.** Schema succeeds but user completions are private/hidden → keep last known progress; show “Limited by provider privacy”.

**Postconditions**  
- **Success:** Progress updated; audit event recorded; aggregates refresh soon after.  
- **Minimal guarantees:** No write-backs to providers; tokens never exposed; if sync fails, the prior state remains.

**Business Rules**  
- **BA Document §8 — External Profiles & Privacy (opt-in, read-only), Provider Quotas & Cooldowns, Staleness Indicators, Auditability.**

**Traceability**  
- **BRD:** `BR-INT-02` Steam Linking & Sync, `BR-INT-03` RA Linking & Sync.  
- **SRS:** `SR-INT-05` (token use & encryption), `SR-INT-06` (quota/backoff), `SR-ACH-12` (sync pipeline), `SR-OBS-xx` (sync metrics).

**Data Touchpoints**  
`external_links` (token_ref, last_synced_at, state), `user_achievement_progress`, `sync_runs` (status, durations, error codes), `completion_aggregates`.

**UX Notes**  
Single **Sync now** control per provider; disabled state with remaining cooldown; clear “Last sync” timestamp; EN/UA copy.

**NFR Hooks**  
Security (encrypted tokens, least privilege), compliance (provider ToS), reliability (idempotent jobs), performance (p95 job runtime SLO), observability (success/error rates, backoff counts).

**Open Issues**  
- Default cooldown per provider (e.g., 10–15 min) to finalise with quota review.  
- Whether to allow background auto-sync on profile view (default: off in MVP).

---
<br>

#### UC-INT-02 — Manage Evidence Provider Allowlist (admin)

**Goal**  
Let Admins manage the **allowlisted domains** for **evidence links** used in fan-achievement completions (e.g., YouTube timestamps, GameFAQs threads), ensuring integrity without hosting heavy media.

**Scope/Level**  
Operational/administrative.

**Primary Actor**  
Admin (or Moderator with the specific permission).

**Stakeholders & Interests**  
- **Admin:** Add/remove domains safely; test links; audit all changes.  
- **Users/Curators:** Predictable acceptance of evidence links; clear error messages if a domain isn’t allowed.  
- **Platform:** Prevent malicious links; keep parsing simple; avoid PII leakage.

**Preconditions**  
- Admin signed in with the **Allowlist** permission.  
- Evidence link policy enabled in the platform.

**Trigger**  
Admin opens **Evidence Allowlist** in settings.

**Main Success Scenario**  
1. System shows the current allowlist with domain patterns (e.g., `youtube.com`, `youtu.be`, `gamefaqs.gamespot.com`) and brief notes per entry.  
2. Admin clicks **Add domain** and enters: domain/pattern, optional URL validation rule (e.g., requires timestamp), and a short rationale.  
3. System validates format and checks for duplicates/conflicts.  
4. Admin optionally pastes a **test URL**; system validates it against the proposed rule and shows the pass/fail result.  
5. Admin **Saves**; the new rule becomes active and propagates to evidence validation within seconds.  
6. System records an audit event and shows a success toast.

**Alternate/Exception Flows**  
- **A1: Conflict/duplicate.** Inline error; system points to the existing rule.  
- **A2: Dangerous pattern (over-broad wildcard).** System blocks save and asks for a narrower rule.  
- **A3: Remove domain.** Admin selects **Remove**; system warns about existing evidence links relying on it. If confirmed, new submissions are blocked; existing links are marked **Needs review** (not auto-deleted).  
- **A4: Temporary block.** Admin can **Suspend** a domain (e.g., abuse spike); new submissions fail with an explanatory message.

**Postconditions**  
- **Success:** Allowlist updated; validation pipeline uses the new rules; audit trail complete.  
- **Minimal guarantees:** No retroactive deletion of existing evidence; moderation may review flagged legacy links.

**Business Rules**  
- **BA Document §8 — Evidence Policy (links-only), Allowlist Governance, Moderation & Review, Auditability.**

**Traceability**  
- **BRD:** `BR-ACH-05` Evidence & Completion (links-only), `BR-GOV-02` Moderation Controls.  
- **SRS:** `SR-ACH-18/19` (evidence intake & allowlist), `SR-GOV-14` (admin settings & audit), `SR-SEC-xx` (URL validation).

**Data Touchpoints**  
`evidence_allowlist` (domain, rules, state), `evidence_links` (validation state), `audit_events`, config cache/feature flags.

**UX Notes**  
Clear examples in the UI; test-URL tool is prominent; changes show who/when/what; EN/UA messages.

**NFR Hooks**  
Security (strict parsing, no open redirects), availability (config propagation), observability (reject rates per domain, abuse spikes).

**Open Issues**  
- Whether to support **per-game** exceptions (default: global allowlist only in MVP).  
- Automatic re-validation of legacy evidence when rules change (default: manual review queue).

---
<br>
<br>

## 5. Business Rules References

> This section lists the **canonical business rules** that the use cases in §4 rely on. It points to the authoritative source (primarily **Business Analysis Document — §8. Business Rules**) and notes the **key use cases** that depend on each rule family. The SRS holds deeper system constraints; here we only reference the rule intent and where it lives.

| Rule Family | Summary (what the rule enforces) | Canonical Source | Key UCs Impacted |
|---|---|---|---|
| **Accounts, Privacy & Roles** | Registration with email verification; default role **Regular**; locale selection (EN/UA) at sign-up; profile visibility (public/private); opt-in external panels (Steam/RA); password/lockout/rate-limit standards; account deletion (RTBF) with holds and audit. | **BA Doc §8.1** User Accounts & Roles; Privacy & Visibility | UC-ACC-01..07, UC-PRO-01..02 |
| **Catalogue, Variants & Localisation Flags** | Game **variants** = platform, region (NTSC/PAL), and (for modern PC) build/manifest; **pillar availability** badges (Saves/Achievements/Albums) and reasons when Saves are unavailable (e.g., account-bound); **policy visibility is scoped to catalogue/discovery only**; UA/RU defaults: UA locale **ON**, others **OFF**, user-toggleable. | **BA Doc §8.2** Catalogue & Flags; Policy Visibility scope/defaults | UC-CAT-01..03, UC-DSC-01..03 |
| **Save Management & Version Compatibility** | Upload requires metadata, quarantine + AV; status taxonomy **confirmed / untested / legacy / incompatible**; **community confirmations** with thresholds and audit; admin overrides; guidance text per variant; owner lifecycle (archive/restore/delete); retention and archiving windows. | **BA Doc §8.2** (compatibility), **§8.6** (retention & privacy) | UC-SAV-01..06, UC-CAT-02, UC-DSC-03, UC-GOV-02 |
| **Official & Retro Achievements (read-only)** | Steam/RA linking is opt-in; **read-only** sync; no write-backs; cooldowns/quotas; stale indicators; unlink purges user-specific synced data; progress shown only if user enabled panels. | **BA Doc §8.7** External Profiles & Privacy | UC-ACC-06, UC-ACH-01..02, UC-PRO-01..03, UC-INT-01 |
| **Fan Achievement Sets** | Curator-authored sets with criteria/flags (storyline/irreversible/final); versioning on publish; comments & ratings; evidence policy (links-only, allowlisted domains); moderation hooks; completion stats. | **BA Doc §8.3** Achievement Tracking (fan), **§8.5** Moderation, **Evidence Allowlist** | UC-ACH-01,03–06; UC-INT-02; UC-GOV-01..03 |
| **Evidence Links (links-only, allowlist)** | Evidence must be an **allowlisted domain** (e.g., YouTube timestamps, GameFAQs); stored as links only (no heavy media hosting); invalid/unsafe links rejected; legacy links handled via review if rules change. | **BA Doc §8.3 / Evidence Policy** | UC-ACH-04, UC-INT-02, UC-GOV-01..03 |
| **Sticker Albums & Spoilers** | Template **types** (e.g., *new-player*); spoiler posture (strict/standard); **Next-Slot Preview** to help users not miss shots; AI triage (accept/pending/reject); completion earns a game-specific badge; content ownership stays with uploader. | **BA Doc §8.4** Sticker Album & Screenshot Rules | UC-ALB-01..05 |
| **Moderation, Reporting & Appeals** | Report intake categories; auto-quarantine for suspected malware/corruption; moderator decision set (keep/edit/quarantine/remove/override/escalate); **appeal window** (e.g., 7 days) with independent review; full audit trail; rate-limits to prevent abuse. | **BA Doc §8.5** Moderation & Reporting; Appeals | UC-GOV-01..03, UC-SAV-05, UC-ALB-03, UC-ACH-05 |
| **Discovery & Policy Scope** | Search/typeahead semantics, facets (platform/region/year/pillars/compatibility); **policy banners and RU-filters are discovery-scoped only** (never on profiles/achievement pages). | **BA Doc §8.2** (policy scope), **Search/Discovery rules** | UC-DSC-01..03, UC-CAT-01..03 |
| **Profiles, Badges & Activity** | Public profile fields; optional read-only external panels; “stale” sync badges; badge issuance on album completion; activity feed; no policy banners on profiles. | **BA Doc §8.1** (visibility/panels), **§8.4** (badges) | UC-PRO-01..03 |
| **Data Retention & Privacy** | PII minimisation; encryption in transit/at rest; save archival after inactivity (with notice) and eventual deletion per policy; screenshot retention; logs/audit retention; GDPR-like user rights (export/delete). | **BA Doc §8.6** Data Retention & Privacy | UC-ACC-07, UC-SAV-06, UC-GOV-xx |
| **Security & Abuse Controls** | AV scanning; rate-limits; CSRF/session hardening; password policy; link validation; evidence/provider quotas; abuse detection for reports/comments/ratings. | **BA Doc §8.5** (abuse), **Security notes across §8** | All posting/sync/reporting UCs |

**Notes**

- **Authority & Changes.** If a rule in the BA Doc §8 changes, the UC(s) listed above must be reviewed for impact.  
- **Scope Discipline.** Policy-related banners remain **strictly** in catalogue/discovery per rule—UCs that render profiles, achievements, or albums must **not** surface them.  
- **Locale Defaults.** UA locale defaults policy banners **ON** in discovery; other locales default **OFF**—always user-toggleable.

---
<br>
<br>

## 6. NFR Hooks & Quality Constraints

> This section defines **named NFRs** that our use cases reference (e.g., *NFR-SEC-01*, *NFR-UX-03*). Targets are MVP-realistic and testable. “How Measured” indicates the primary signal we’ll watch in prod/pre-prod.

### 6.1 Summary Matrix (IDs, Targets, Measurement)

| ID | Category | Constraint (Target) | How Measured |
|---|---|---|---|
| **NFR-AVL-01** | Availability | User-facing API & web uptime ≥ **99.5%** (monthly); graceful degradation where possible. | Uptime SLO dashboards; synthetic checks. |
| **NFR-PERF-01** | Performance | HTML page TTFB p95 ≤ **300 ms** (cached) / ≤ **600 ms** (uncached). | APM traces; RUM. |
| **NFR-PERF-02** | Performance | Global search results p95 ≤ **700 ms**; typeahead per keystroke p95 ≤ **150 ms**. | Search service metrics; RUM. |
| **NFR-PERF-03** | Performance | Game detail/variant page p95 ≤ **500 ms**. | APM; RUM. |
| **NFR-PERF-04** | Performance | Profile & progress dashboards p95 ≤ **600 ms**. | APM; RUM. |
| **NFR-PERF-05** | Performance | Evidence submit / comment post p95 ≤ **400 ms** (server processing). | API latency histograms. |
| **NFR-PERF-06** | Performance | Image upload init ≤ **200 ms**; AI triage decision ≤ **3 s** (else mark *Pending* in ≤ **300 ms**). | Upload gateway; async job SLOs. |
| **NFR-PERF-07** | Performance | Save file secure link issuance p95 ≤ **250 ms**. | API latency. |
| **NFR-SEC-01** | Security | TLS 1.2+ everywhere; HSTS; secure cookies (HttpOnly, SameSite). | Header scanners; pen tests. |
| **NFR-SEC-02** | Security | Encryption at rest for PII & tokens (e.g., AES-256, KMS-backed). | Config audits. |
| **NFR-SEC-03** | Security | Secrets & tokens in a managed vault; least-privilege service accounts. | IaC policy checks. |
| **NFR-SEC-04** | Security | Password hashing with **Argon2id** (or bcrypt with strong params); lockout/backoff on auth failures. | Auth service tests; logs. |
| **NFR-SEC-05** | Security | CSRF protection, XSS/Clickjacking headers; input validation on all forms. | Security tests; SAST/DAST. |
| **NFR-SEC-06** | Security | AV scan for all uploads; quarantine until clean. | AV job results; quarantine queue. |
| **NFR-SEC-07** | Security | Rate limits on sign-up/login/reset/report/evidence/links. | API gateway metrics. |
| **NFR-PRIV-01** | Privacy | PII minimisation; no provider IDs shown unless user opts in. | Data model reviews; UI tests. |
| **NFR-PRIV-02** | Privacy | **RTBF** (account deletion) with full purge/anonymisation within **7 days** (MVP). | Deletion job metrics; audits. |
| **NFR-PRIV-03** | Privacy | Data export on request within **30 days** (MVP). | Ops tracker. |
| **NFR-PRIV-04** | Privacy | Logs exclude PII and tokens; access is least-privilege & audited. | Log scrubbing checks; IAM audits. |
| **NFR-LOC-01** | Localisation | Full **EN/UA** UI coverage; dates/times localised; search transliteration EN⇄UA. | i18n lint; UX review. |
| **NFR-UX-01** | Usability/Accessibility | WCAG **2.1 AA** across core flows (auth, search, catalogue, saves, achievements, albums). | Accessibility audits. |
| **NFR-UX-02** | Usability | Keyboard navigation for all primary actions; visible focus states. | UI tests. |
| **NFR-UX-03** | Usability | Plain-language errors; inline validation; no jargon in system messages. | UX copy review; RUM. |
| **NFR-OBS-01** | Observability | Structured logs with correlation IDs across web/API/worker. | Log sampling; trace linkage. |
| **NFR-OBS-02** | Observability | Golden signals per service (latency, traffic, errors, saturation) + dashboards & alerts. | Monitoring stack. |
| **NFR-OBS-03** | Observability | Audit events for security/role/moderation actions (immutable). | Audit store checks. |
| **NFR-REL-01** | Reliability | **Error budget**: ≤ **0.5%** failed requests/month per service. | SLO reports. |
| **NFR-REL-02** | Reliability | Backups daily; **RPO ≤ 24 h**, **RTO ≤ 8 h** for core DB; object storage versioning on. | DR runbooks/tests. |
| **NFR-SCL-01** | Scalability | Sustain **10k MAU**, peaks of **50 RPS** read, **10 RPS** write; burst uploads **100 concurrent**. | Capacity tests; autoscaling. |
| **NFR-COMP-01** | Compatibility | Browsers: last 2 versions of Chrome/Firefox/Safari/Edge; responsive layouts mobile→desktop. | Cross-browser CI. |
| **NFR-MNT-01** | Maintainability | Server/API unit test coverage ≥ **70%**; zero-downtime DB migrations; feature flags for risky changes. | CI/CD gates; rollout plan. |
| **NFR-INT-01** | Integrations | Provider quotas respected; exponential backoff; **cooldown** per user/provider (≥ 10–15 min). | Job logs; rate-limit metrics. |

---
<br>

### 6.2 Performance Budgets by Flow (Key UCs)

| Flow / UC Reference | Budget (p95 unless stated) | Notes |
|---|---|---|
| UC-DSC-01 Typeahead | ≤ **150 ms** per keystroke | Local cache + pre-computed suggest index. |
| UC-CAT-01 Results | ≤ **700 ms** | Facets applied server-side; partial render acceptable. |
| UC-CAT-02 Game/Variant | ≤ **500 ms** | Compatibility chips pulled from cached aggregates. |
| UC-PRO-01 Profile View | ≤ **500–600 ms** | External panels can load async; show “stale” badge if needed. |
| UC-SAV-02 Download Link | ≤ **250 ms** | Signed URL generation; CDN handles bulk. |
| UC-ALB-03 Upload + Triage | Path A (pass) ≤ **3 s**; Path B (pending) UI response ≤ **300 ms** | Pending path must be non-blocking. |
| UC-ACH-04 Evidence Submit | ≤ **400 ms** | Link validation synchronous; deeper checks async. |

---
<br>

### 6.3 Security & Privacy Baselines

- **Transport & Session**: *NFR-SEC-01/05* — TLS, HSTS, CSRF tokens, session fixation protection, secure cookies.
- **Data at Rest**: *NFR-SEC-02* — KMS-backed encryption; per-tenant or per-table keys where feasible.
- **Identity & Abuse**: *NFR-SEC-04/07* — Argon2id/bcrypt, auth lockout/backoff, global and per-route rate limits.
- **Uploads**: *NFR-SEC-06* — AV scan, quarantine; allow only known extensions & sizes.
- **Least Privilege**: *NFR-SEC-03/04* — IAM roles per service; no tokens in logs.
- **Privacy**: *NFR-PRIV-01..04* — Minimise PII, RTBF ≤ 7 days, exports ≤ 30 days, no external IDs shown unless user opts in, logs scrubbed.

---
<br>

### 6.4 Availability, Reliability & DR

- **Uptime**: *NFR-AVL-01* — 99.5% monthly. Non-critical panels (e.g., external progress) may **gracefully degrade**.
- **Error Budgets**: *NFR-REL-01* — ≤ 0.5% failed requests; budget reviews trigger feature freeze if exceeded.
- **Backups/DR**: *NFR-REL-02* — Daily full + WAL/incrementals; **RPO ≤ 24 h**, **RTO ≤ 8 h**. Object storage keeps prior versions.
- **Queues**: Durable storage; retry with exponential backoff; dead-letter inspection within 24 h.

---
<br>

### 6.5 Localisation & Accessibility

- **Locales**: *NFR-LOC-01* — EN/UA parity; UA defaults for policy banners in discovery (per rules).
- **Search**: EN⇄UA transliteration, accent folding; do not leak locale into URLs as PII.
- **Accessibility**: *NFR-UX-01/02/03* — WCAG 2.1 AA; keyboard paths for upload, moderation, and evidence; clear inline errors.

---
<br>

### 6.6 Observability & Audit

- **Tracing & Logs**: *NFR-OBS-01* — Correlation IDs across web/API/worker; JSON logs.
- **Metrics**: *NFR-OBS-02* — Standard golden signals; SLO dashboards for each service; on-call alerts.
- **Audit**: *NFR-OBS-03* — Immutable events for role changes, moderation, publish/unpublish, deletion.

---
<br>

### 6.7 Maintainability, Deployment & Change Management

- **Tests & CI**: *NFR-MNT-01* — ≥ 70% unit coverage backend; smoke tests for critical UCs.
- **Migrations**: Zero-downtime where feasible; backward-compatible schema first.
- **Feature Flags**: Dark-launch risky changes; staged rollout with quick rollback.
- **Compatibility**: *NFR-COMP-01* — Last-2 versions of major browsers; responsive breakpoints for mobile/desktop.

---
<br>

### 6.8 Scalability Targets (MVP)

- **Users**: ~**10k MAU** baseline; path to **100k** with index sharding and job scaling.
- **Traffic**: Steady **50 RPS** reads, **10 RPS** writes; upload bursts **100 concurrent**.
- **Storage**: Saves/screenshots growth modeled at **hundreds of GBs** in MVP; lifecycle rules (archive after inactivity) apply.

---
<br>

### 6.9 Release Quality Gates (Go/No-Go)

A release cannot ship unless:

1. All **p95** budgets in §6.2 are green in pre-prod load tests.  
2. **Security** checks pass (headers, CSRF, AV, secrets scan).  
3. **Accessibility** smoke checks pass for auth/search/catalogue/saves/albums.  
4. **Observability** dashboards & alerts are live; correlation IDs verified end-to-end.  
5. **DR** drill within last 90 days proves **RTO/RPO**.  
6. **Privacy**: RTBF and export flows validated in staging.

> These IDs are referenced in the “NFR Hooks” blocks inside use cases (§4). Any change to targets here requires a quick sweep to update affected UCs.

---
<br>
<br>

## 7. Traceability & Cross-Links

This section shows how the **Use Cases** in §4 connect to upstream business intent and downstream system constraints. It’s meant to be a quick “where-does-this-live?” map for reviewers, QA, and developers during changes.

### 7.1 Artefacts & Abbreviations

- **BC** — Business Case (sections: Objectives §3, Scope §5, Stakeholders §6, Risks §9, Timeline §10).
- **BA Doc** — Business Analysis Document (notably **§8. Business Rules** plus Stakeholders §3, AS-IS/TO-BE §4–5).
- **SCP** — Stakeholder & Communication Plan.
- **BRD** — Business Requirements Document (IDs `BR-<area>-NN`).
- **SRS** — System Requirements Specification (IDs `SR-<module>-NN`).
- **UCS** — This Use Case Specification (IDs `UC-<module>-NN`).
- **NFR** — Non-functional constraints in UCS §6 (IDs `NFR-…`).

ID patterns:
- **UC**: `UC-ACC-01`, `UC-CAT-02`, …  
- **BRD**: `BR-SAV-05`, `BR-ACH-03`, …  
- **SRS**: `SR-SAV-14`, `SR-ACH-12`, …  

> Rule of thumb: every UC must cite at least one **BRD** requirement and one or more **SRS** behaviours, plus relevant **NFRs** and **BA Doc §8** rule families.

---
<br>

### 7.2 Module-Level Coverage Map

| Module | UCs in §4 | Primary BRD Areas | Primary SRS Areas | Typical NFR Hooks |
|---|---|---|---|---|
| **ACC — Accounts & Auth** | UC-ACC-01…07 | BR-ACC-01 (registration), BR-ACC-02 (auth), BR-ACC-03 (privacy/localisation), BR-ACC-04 (account mgmt) | SR-ACC-01…31 | NFR-SEC-01..05, NFR-PRIV-01..04, NFR-UX-01..03 |
| **CAT — Catalogue & Flags** | UC-CAT-01…04 | BR-CAT-01 (search), BR-CAT-02 (variants), BR-CAT-04 (policy scope), BR-CAT-05 (suggest corrections) | SR-CAT-01…18 | NFR-PERF-02..03, NFR-LOC-01 |
| **SAV — Save Management** | UC-SAV-01…06 | BR-SAV-01 (upload), BR-SAV-02 (compatibility), BR-SAV-03 (download), BR-SAV-05 (filters), BR-SAV-06..07 (confirm/report/lifecycle) | SR-SAV-11…31 | NFR-SEC-06, NFR-PERF-07, NFR-REL-02 |
| **ACH — Achievement Tracking** | UC-ACH-01…06 | BR-ACH-01 (set view), BR-ACH-03..06 (fan authoring, evidence, comments/ratings), BR-ACH-07..09 (spoilers, stats) | SR-ACH-01…30 | NFR-INT-01, NFR-PRIV-01, NFR-UX-01..03 |
| **ALB — Sticker Albums** | UC-ALB-01…05 | BR-ALB-01..07 (templates, instances, validation, badges, stats, spoiler controls) | SR-ALB-01…12 | NFR-PERF-06, NFR-SEC-06, NFR-UX-01 |
| **PRO — Profiles & Progress** | UC-PRO-01…04 | BR-PRO-01..04 (profile view, visibility, badges, curator onboarding) | SR-PRO-01…11 | NFR-PRIV-01..03, NFR-PERF-04 |
| **GOV — Moderation & Governance** | UC-GOV-01…03 | BR-GOV-01..03 (reporting, moderation, appeals) | SR-GOV-11…18 | NFR-OBS-03, NFR-REL-01 |
| **DSC — Discovery & Search** | UC-DSC-01…03 | BR-DSC-01..03 (typeahead, facets, quick view) | SR-DSC-01…14 | NFR-PERF-01..03, NFR-LOC-01 |
| **INT — Integrations & Adapters** | UC-INT-01…02 | BR-INT-02..03 (Steam/RA linking & sync), BR-ACH-05 (evidence allowlist) | SR-INT-05..06 | NFR-INT-01, NFR-SEC-03 |

---
<br>

### 7.3 Representative UC Cross-References

> Full per-UC references live under each UC in §4; this table shows typical link patterns.

| UC | BRD Link(s) | SRS Link(s) | BA Doc §8 Rule Family | NFRs |
|---|---|---|---|---|
| **UC-SAV-01 Upload Save** | BR-SAV-01, BR-SAV-04 | SR-SAV-11, SR-SAV-21, SR-SAV-25, SR-SAV-31 | Save Management & Version Compatibility; Retention & Privacy | NFR-SEC-06, NFR-PERF-07 |
| **UC-CAT-02 View Game & Variants** | BR-CAT-02, BR-SAV-02, BR-CAT-04 | SR-CAT-04, SR-CAT-12 | Catalogue/Variants; Policy Visibility (discovery only) | NFR-PERF-03, NFR-LOC-01 |
| **UC-ACH-02 Sync Official/Retro** | BR-INT-02, BR-INT-03 | SR-ACH-12, SR-INT-05/06 | External Profiles (read-only sync, quotas) | NFR-INT-01, NFR-PRIV-01 |
| **UC-ALB-03 Fill Slot (AI triage)** | BR-ALB-03 | SR-ALB-03..05 | Screenshot Validation; Moderation Hooks | NFR-PERF-06, NFR-SEC-06 |
| **UC-GOV-02 Moderator Decision** | BR-GOV-02 | SR-GOV-12, SR-SAV-27 | Moderation Workflow; Overrides; Auditability | NFR-OBS-03, NFR-REL-01 |
| **UC-DSC-02 Facets & Policy Toggles** | BR-DSC-02, BR-CAT-04 | SR-DSC-08, SR-DSC-14, SR-CAT-12 | Facet Semantics; Policy Scope | NFR-PERF-02, NFR-LOC-01 |

---
<br>

### 7.4 Cross-Links to Non-UC Artefacts

- **BC §3 Objectives ↔ UCS §4 success paths:** MVP pillars and adoption KPIs reflected in UC postconditions (e.g., album completion badges, sync freshness).
- **BC §5 Scope ↔ UCS Preconditions:** Out-of-scope items (e.g., write-back to providers) appear as explicit **exclusions** in UC preconditions/alternates.
- **BA Doc §8 Business Rules ↔ UCS Business Rules blocks:** Each UC lists the applicable rule family (catalogue policy scope, spoiler posture, evidence allowlist, compatibility taxonomy).
- **SCP (Stakeholder & Communication Plan) ↔ UC Notifications:** Moderator decisions, appeals, and curator onboarding align with SCP channels/cadence.
- **SRS cross-refs:** Where a UC implies data fields or status taxonomies, SRS modules (e.g., `SR-SAV-*` for compatibility statuses) are referenced.

---
<br>

### 7.5 Change Impact Rules

When **any** of the following changes, run a traceability sweep:

1. **Business Rule update (BA Doc §8)** → Review all UCs that cite the rule family + their SRS links.  
2. **BRD requirement change** → Check matching UCs and the SRS items they reference; adjust NFR budgets if flow/timing changes.  
3. **SRS contract change** (IDs, statuses, thresholds) → Update UC **Error/Edge** and **Data Touchpoints**; verify QA scenarios.  
4. **NFR target change** → Update UC **NFR Hooks** and §6 matrices; re-run performance/accessibility gates.  
5. **Policy scope** (UA/Russia visibility) → Verify CAT/DSC UCs and ensure profiles/achievement pages remain policy-banner-free.

---
<br>

### 7.6 Verification Aids

- **Coverage checklist (per UC):** BRD link ✓ · SRS link ✓ · BA Doc §8 family ✓ · NFR hooks ✓ · Preconditions/Alternates ✓  
- **Auditability:** GOV UCs must cite **NFR-OBS-03**; ACC/GOV must cite **audit events** in Data Touchpoints.  
- **Localisation:** Any UC with user-facing copy should cite **NFR-LOC-01**.

---
<br>

### 7.7 Known Gaps & Follow-Ups

- **None outstanding** at this time. A final package-wide audit will confirm that all UC **Business Rules** blocks match BA Doc §8 wording and that all policy-scope constraints (catalogue/discovery only) are consistently enforced.
