# System Requirements Specification (SRS)

## Table of Contents

1. [Introduction & Scope](#1-introduction--scope)  
   Describes the purpose of the SRS, its audience, how it relates to other BA artefacts (Business Case, BRD, etc.), and clarifies what is and is not covered by this specification for the MVP.

2. [System Context & Architecture Overview](#2-system-context--architecture-overview)  
   Provides a high-level view of Dream Project as a system: main subsystems, the three core pillars (saves, achievements, sticker albums), external services (Steam, RetroAchievements, IGDB, email), and the runtime environment (web app, backend, storage).

3. [Actors, User Roles & Permissions](#3-actors-user-roles--permissions)  
   Defines technical actors (end-user browser, admin console, background jobs, external APIs) and describes the system-level roles (Regular User, Curator, Admin) and their permissions from an implementation perspective.

4. [Functional Requirements by Module](#4-functional-requirements-by-module)  
   Details system-level functional requirements grouped by major modules. Each requirement will be traceable to BRD IDs and expressed with preconditions, normal flows and key error conditions.
   
   4.1 [Accounts & Authentication](#41-accounts--authentication)  
   Registration, login/logout, password reset, language preferences, role storage, external profile linking basics.

   4.2 [Game Catalogue & Localisation Flags](#42-game-catalogue--localisation-flags)  
   Managing game and platform entries, UA-localisation flags, UA-origin and Russian-linked flags, and feature availability indicators per game/platform.

   4.3 [Save Management](#43-save-management)  
   Uploading, storing, listing and sharing saves; metadata handling; save-sharing availability rules; reporting and basic moderation handling at system level.

   4.4 [Achievement Tracking](#44-achievement-tracking)  
   Handling official (Steam), retro (RetroAchievements) and Dream Project fan-made achievements; set and item structures; progress tracking; evidence links; comments and statistics.

   4.5 [Sticker Albums](#45-sticker-albums)  
   Template creation and maintenance by Curators; slot definitions; user album instances; screenshot uploads; spoiler handling; basic AI/rule validation; completion and badges.

   4.6 [User Profile & Progress View](#46-user-profile--progress-view)  
   Profile data, visibility settings, per-game progress views, cross-game summaries, curator contribution highlighting, and safe display of linked external profiles.

   4.7 [Moderation & Reporting](#47-moderation--reporting)  
   Reporting flows for saves, screenshots, sets, albums, comments and profiles; moderation queue; actions and state changes; minimal decision logging.

   4.8 [Discovery & Recommendations](#48-discovery--recommendations)  
   Game search/browse; game detail pages consolidating pillars; simple “help me choose” recommendations and rejection/refresh behaviour.

   4.9 [Integrations & Background Jobs](#49-integrations--background-jobs)  
   High-level system behaviour for Steam and RetroAchievements sync, IGDB usage, email sending, and any scheduled/background processing required.

5. [External Interface Requirements](#5-external-interface-requirements)  
   Describes key UI expectations (web/browser support, general layout constraints), external API endpoints exposed by Dream Project (if any), and integration touchpoints with Steam, RetroAchievements, IGDB and the email provider at a contract level.

6. [Non-Functional Requirements – System View](#6-non-functional-requirements--system-view)  
   Translates BRD-level NFRs into more technical constraints: performance targets, availability expectations, security controls, localisation behaviour, accessibility considerations and maintainability hooks.

7. [Data & Storage Considerations](#7-data--storage-considerations)  
   Summarises persistence-related requirements: identifiers, basic retention rules, size limits for saves and screenshots, and how these relate to the separate **Data Model & Business Rules** document.

8. [Operational & Deployment Considerations](#8-operational--deployment-considerations)  
   Describes assumptions about hosting environment, logging and monitoring expectations, backup/restore basics, and how maintenance windows and deployments should behave from a system perspective.

9. [Traceability & Mapping to BRD](#9-traceability--mapping-to-brd)  
   Explains how SRS requirements (e.g. `SR-…` IDs) map back to BRD business requirements (`BR-…`, `NFR-…`) and forward into the **Requirements Traceability Matrix (RTM)**, ensuring end-to-end traceability from business intent to implementation and testing.

   ---
<br>
<br>


## 1. Introduction & Scope

### 1.1 Purpose of this Document
This **System Requirements Specification (SRS)** turns the Dream Project vision into a concrete, buildable system.  
It translates the Business Case and BRD into **implementable, testable, and traceable** system requirements that precise enough for engineers and QA to act on, yet readable for stakeholders who want to understand what the MVP really does—and what it *won’t* do.

---
<br>

### 1.2 Intended Audience
- **Product Owner / BA** — confirm that the described behaviour matches the Business Case and BRD.  
- **Design & Engineering** — implement flows, data handling, and integrations exactly as specified here.  
- **QA / UAT** — derive scenarios and acceptance checks from the requirements and IDs.  
- **Curators & Moderators (internal)** — see what tooling exists, where the guardrails are, and how moderation works.  
- **Stakeholders** — verify UA/Russia catalogue policy handling, privacy posture, and realistic scope.

---
<br>

### 1.3 Relationship to Other Artefacts
This SRS **refines**:
- **Business Case** — the “why”: problem, value, objectives, early success signals.  
- **BRD** — the “what”: business requirements (`BR-…`) and NFRs (`NFR-…`).  
- **Stakeholder & Communication Plan** — who reviews and decides changes.  
- **Data Model & Business Rules** — entity structures and rule detail that pair with this SRS.  
- **Diagrams & Wireframes** — the visual story that complements the text.  
- **RTM (Requirements Traceability Matrix)** — the map from **BR/NFR → SRS (SR-…) → stories → tests**.

> **Traceability:** Every system requirement carries an **SR ID** (e.g., `SR-ACC-03`) and references the BRD items it satisfies. The RTM keeps the full chain tidy.

---
<br>

### 1.4 Product Overview (MVP)
Dream Project is a **web companion** for players who want their progress to *mean something*—and to *survive* device changes, storefronts, and years. It’s built on three pillars:

1. **Save Management** — upload and (optionally) share saves with a **version-compatibility layer**.  
   - **Modern PC/Steam:** capture **build/manifest IDs** at upload, warn on mismatches, and mark older uploads as **legacy** when games update.  
   - **Retro:** respect **region facets (PAL/NTSC)** and emulator notes so users understand when a save is realistically compatible.

2. **Achievement Tracking** — reflect **official (Steam)** and **retro (RetroAchievements)** progress (read-only), plus **fan-made sets** with criteria, hints, comments, evidence links, and basic stats.

3. **Sticker Albums** — curated, spoiler-aware screenshot albums that players fill in like digital sticker books; completion awards a badge.

Supporting capabilities include a **game catalogue** with **UA-specific signals** (UA localisation, UA-origin) and **Russia-linked** flags; **profiles**, **roles** (Regular, Curator, Admin) with moderation tools; simple **discovery/recommendations**; and lightweight, read-only integrations (**Steam**, **RetroAchievements**, **IGDB**, **SendGrid**).  
Long gameplay clips are **not hosted** by Dream Project—users attach links (e.g., YouTube, GameFAQs).

---
<br>

### 1.5 Scope of the MVP (System View)
This SRS covers system behaviour for:

- **Accounts & Authentication** — registration, login/logout, password reset, EN/UA locale, link/unlink Steam/RA profiles.  
- **Catalogue & Flags** — game + platform variants; which pillars are available per title; **UA localisation/origin** and **Russia-linked** flags with clear display rules.  
- **Save Management** — upload, store, list, share, report;  
  - **Version compatibility** (Steam build/manifest capture), community confirmations, **legacy/incompatible** statuses, download-time warnings, optional **auto-archiving** of legacy saves;  
  - **Retro** region/emulator facets surfaced across the UX.  
- **Achievements** — read-only Steam/RA reflection; Dream Project fan-made sets; progress states; hints and evidence links; comments; completion statistics.  
- **Sticker Albums** — curator templates; spoiler flags; user album instances; screenshot upload; light AI/rule checks; completion badges and stats.  
- **Profiles & Progress Views** — per-game and cross-game summaries; curator attribution; privacy/visibility controls.  
- **Moderation & Reporting** — reports for saves/screenshots/sets/albums/comments/profiles; queue, actions, and lightweight audit breadcrumbs.  
- **Discovery & Recommendations** — search/browse, consolidated game pages, simple “help me choose” with rejection/refresh.  
- **Integrations & Background Jobs** — Steam/RA sync (read-only), IGDB import, transactional email, minimal schedulers with graceful degradation.  
- **Non-functional & Ops hooks** — security, performance, EN/UA localisation, accessibility basics, logging/monitoring, backups, maintenance behaviour.

---
<br>

### 1.6 Out of Scope (MVP)
- Native iOS/Android/desktop apps; offline modes.  
- Any **write-back** to Steam/RA or behaviour that violates external ToS.  
- Console account integrations (PSN/Xbox/Nintendo).  
- Hosting/transcoding long gameplay clips (links only).  
- **Automatic save conversion** between builds/emulators/platforms.  
- Payments/ads/marketplace features.  
- Enterprise-grade SLAs; MVP targets pragmatic availability for a small team.  
- Comprehensive legal frameworks (concise policies only).

---
<br>

### 1.7 Assumptions & Constraints (SRS Perspective)
- **Small team & timebox** — Must-have first, Should-have when feasible.  
- **Read-only integrations** — **Steam** and **RetroAchievements** accessible within ToS; sync is periodic, never blocking navigation.  
- **Catalogue sources** — **IGDB** for general metadata; **Kuli/Artemiano** for UA localisation/origin and Russia-linked context.  
- **Locales** — English and Ukrainian.  
- **Runtime** — modern desktop/mobile browsers.  
- **Curation & moderation** — form-based tools, limited capacity.  
- **Save compatibility is best-effort** — build/region facets + community confirmations + curator/admin overrides; no absolute guarantees.

---
<br>

### 1.8 Definitions & Document Conventions
- **Build/Manifest ID (Steam)** — the shipped build identifier, used to judge save compatibility over time.  
- **Compatibility Status** — `confirmed`, `untested`, `legacy`, `incompatible`.  
- **Legacy Save** — uploaded against an older build and not verified on the current build; may be auto-archived later.  
- **Platform Variant** — a specific game + platform combination (e.g., *PC/Steam*, *PS1/Emulator*).  
- **“Shall / Should / May”** — required / recommended / optional.  
- **ID Scheme** — system requirements `SR-<MODULE>-NN`; system NFRs `NFR-SYS-XX`; BRD cross-refs `BR-…`, `NFR-…`.

---
<br>

### 1.9 Change Control & Versioning
Changes follow the **Stakeholder & Communication Plan** and are recorded in the **RTM**.  
New or modified requirements get updated **SR IDs** and explicit BRD mappings. If SRS and BRD diverge, both are updated together with a short rationale to keep the chain of custody clear.

---
<br>
<br>

## 2. System Context & Architecture Overview

This section shows **how the whole thing hangs together**—who talks to whom, what runs where, and which moving parts keep the MVP honest (and resilient) without over-engineering.

---
<br>

### 2.1 Context in One Page

At runtime, Dream Project is a **web app** backed by a stateless **API**, a modest **database**, **object storage** for user files (saves/screenshots), and a handful of **background workers**.  
It **reads** from external services (Steam, RetroAchievements, IGDB) and **sends** transactional email via a single provider. If any external dependency blinks, the product keeps working with **graceful degradation** and clear messaging.

External world ↔︎ Dream Project:

- **Users (Browsers)**: Regular Users, Curators, Admins
- **Steam**: official achievements (read-only)
- **RetroAchievements**: retro sets & progress (read-only)
- **IGDB**: game catalogue metadata (import/seed)
- **Email provider**: registration/reset/role notices
- *(Links only)* **YouTube/GameFAQs** for evidence/guides

---
<br>

### 2.2 External Systems & Actors

- **Player Browser (End User)**
  - Uses: all three pillars, search, profiles.
  - Trust: untrusted; authenticated via web session.

- **Curator / Admin (Privileged Users)**
  - Uses: create sets, albums, catalogue edits, moderation.
  - Trust: authenticated; actions logged.

- **Steam (Official Achievements)**
  - Integration: **read-only** sync for linked accounts; no write-back.
  - Failure mode: hide/mark official progress as temporarily unavailable; rest of the product works.

- **RetroAchievements (Retro Progress)**
  - Integration: **read-only** sync for linked profiles; mirrors sets and completion.
  - Failure mode: retro progress panels go stale/hidden; saves/albums/fan sets still work.

- **IGDB (Game Metadata)**
  - Integration: periodic **import** for titles we support; human verification.
  - Failure mode: slower catalogue growth; no runtime impact on core flows.

- **Transactional Email (e.g., SendGrid)**
  - Integration: account confirmation, password reset, role changes.
  - Failure mode: temporarily limit signup/reset and show clear retry guidance.

---
<br>

### 2.3 Component View (What We’re Building)

**SR-ARC-01 – Web Application (M)**  
Responsive SPA/MPA for desktop/mobile; EN/UA locales; accessible, spoiler-aware UI.

**SR-ARC-02 – Backend API (M)**  
Stateless HTTP API that enforces authZ, validates input, orchestrates saves/achievements/albums/catalogue/moderation, and emits audit breadcrumbs.

**SR-ARC-03 – Persistence & Storage (M)**  
- **Relational DB** for users, roles, catalogue, sets, albums, comments, reports, flags.  
- **Object storage** for save files & screenshots; antivirus/validation pipeline on ingest.  
- **Search/facet index** for fast discovery (titles, tags, compatibility badges).

**SR-ARC-04 – Background Workers (M)**  
Asynchronous jobs for:
- **Steam/RetroAchievements sync** (read-only, rate-limited).  
- **Build Change Watcher (Steam)** to detect new builds for titles with public saves and open **compatibility review** tasks.  
- **Screenshot AI/rule checks** (basic signal for off-topic/inappropriate content).  
- **Auto-archiving** of long-inactive legacy saves.  
- **Email dispatch**.

**SR-ARC-05 – Integration Adapters (M)**  
Encapsulate API clients, rate limits, retries, error mapping, and ToS compliance for Steam, RA, IGDB, email.

**SR-ARC-06 – Admin Console (M)**  
Protected routes inside the web app for moderation queues, role changes, catalogue flags (UA localisation/origin, Russia-linked), and compatibility overrides.

**SR-ARC-07 – Feature Flags & Config (S)**  
Toggle risky/early features (e.g., album validation, recommendations, specific integrations) without redeploying.

**SR-ARC-08 – Telemetry & Observability (S)**  
Structured logs, request metrics, background job metrics, basic alerts on repeated sync failures and virus-scan rejects.

**SR-ARC-09 – Media Processing (S)**  
On-demand thumbnails and safe EXIF handling for screenshots; no video transcoding.

---
<br>

### 2.4 Runtime & Deployment

**SR-DEP-01 – Environments (M)**  
Dev, Staging, Production with isolated credentials and data.

**SR-DEP-02 – Secrets & Keys (M)**  
Managed outside source control; least-privilege access to DB, object storage, and third-party APIs.

**SR-DEP-03 – Backups & Restore (M)**  
Scheduled DB backups; object storage versioning where feasible; documented restore drill.

**SR-DEP-04 – Deployments (S)**  
Zero- or low-downtime deploys; schema migrations are backward-compatible when possible.

---
<br>

### 2.5 Key Data Flows (Happy Paths with Guardrails)

1. **Save Upload & Compatibility**
   - Browser → API: metadata + file → scan → store → record **Steam build/manifest** (if known) or **retro region/emulator** → show **compatibility badges**.  
   - Build Change Watcher detects a new Steam build → opens review task; community “worked/didn’t work” signals roll up to **confirmed/legacy/incompatible**.

2. **Official/Retro Achievement Sync**
   - Background worker fetches for linked accounts → updates profile/game pages.  
   - Timeouts or errors show a gentle “last updated …” badge; navigation never blocks.

3. **Fan-Made Achievement Sets**
   - Curator creates/edit set → users join → mark progress; attach evidence (links, screenshots, saves).  
   - Comments are stored; moderation can hide/remove on report.

4. **Sticker Albums**
   - Curator defines template/slots (with spoilers) → user creates an instance → uploads screenshots → AI/rule check → slot fills; completion → badge.

5. **Catalogue & UA/Russia Flags**
   - IGDB import → human verification → UA signals via **UA sources** (Kuli, Artemiano) and Admin input → flags rendered on game pages; filters available for UA users.

6. **Moderation**
   - Any item can be reported → queue → Admin action (keep/hide/remove, warn/restrict) → visible state updates.

---
<br>

### 2.6 Trust Boundaries & Security

**SR-SEC-CTX-01 – Transport & Session Security (M)**  
HTTPS everywhere; secure cookies/CSRF where applicable; modern session management.

**SR-SEC-CTX-02 – Principle of Least Privilege (M)**  
Separate roles/keys; object storage read/write scoped; adapters use minimal scopes.

**SR-SEC-CTX-03 – File Hygiene (M)**  
Antivirus scan and type validation on every upload; quarantine/reject on detection; no server-side execution of user files.

**SR-SEC-CTX-04 – External Accounts (M)**  
Store only the minimum tokens/IDs for **read-only** sync; clear unlink path in UI.

**SR-SEC-CTX-05 – Input & Rate Limits (S)**  
Validation, sane size limits, per-IP/user rate limiting on sensitive endpoints (auth, upload, report).

**SR-SEC-CTX-06 – Privacy by Default (M)**  
New saves/screenshots private by default; public sharing is explicit; profiles expose minimal personal data.

---
<br>

### 2.7 Reliability & Degradation

**SR-REL-01 – Timeouts, Retries, Circuit-breakers (M)**  
External calls have timeouts and bounded retries; circuit-breakers prevent cascading failures.

**SR-REL-02 – Staleness Indicators (M)**  
UI shows “last synced” and temporarily unavailable states for Steam/RA panels; core features remain usable.

**SR-REL-03 – Idempotent Jobs (S)**  
Background jobs are idempotent and safe to retry.

**SR-REL-04 – Fallback Content (S)**  
If IGDB is down, catalogue pages still render from local store; new imports pause.

---
<br>

### 2.8 Performance Envelope (Targets, not promises)

**SR-PERF-01 – Interactive Loads (S)**  
Key pages (home, game, profile, set/album views) render initial content within a few seconds on typical broadband.

**SR-PERF-02 – Search/Facet (S)**  
Common queries return < ~1.5s under normal load.

**SR-PERF-03 – Uploads (S)**  
Large screenshot/save uploads stream to object storage with visible progress; size caps are enforced per file type.

---
<br>

### 2.9 Localisation & Accessibility

**SR-I18N-01 – EN/UA Locales (M)**  
All user-facing strings localised; UA users see UA/Russia policy notes in Ukrainian.

**SR-A11Y-01 – Practical Accessibility (S)**  
Keyboard navigation, sufficient contrast, alt text for key imagery; spoiler blur is screen-reader friendly.

---
<br>

**Why this architecture?**  
It’s deliberately **small-team friendly**: stateless core, simple workers, object storage for heavy bits, and adapters that let us **read** from the outside world without getting entangled. The save **version-compatibility layer** (build IDs, regions, community confirmations, legacy/incompatible states) is first-class in the design, but it doesn’t require invasive hooks into game platforms—keeping us dependable, legal, and maintainable.

---
<br>
<br>

## 3. Actors, User Roles & Permissions

This section defines **who** interacts with the system and **what** they are allowed to do.  
It keeps the model simple (three roles) but precise enough to implement guardrails, moderation, and UA-policy handling.

> **Traceability:** Implements BRD items **BR-PRO-01..06**, **BR-GOV-01..05**, **BR-ACH-05..11**, **BR-ALB-01..08**, **BR-SAV-01..08**, **BR-CAT-01..05**.  
> System requirements in this section use the `SR-ROLE-*`, `SR-VIS-*`, and `SR-LINK-*` IDs.

---
<br>

### 3.1 Technical Actors (Context)

- **End-User Browser** — untrusted client; authenticated sessions; uploads saves/screenshots.  
- **Backend API** — enforces authN/authZ, validates inputs, orchestrates workflows.  
- **Background Workers** — sync (Steam, RetroAchievements), build-watcher, validation, archiving, email.  
- **Integration Adapters** — Steam / RetroAchievements / IGDB / Email (read-only where applicable).  
- **Object Storage** — saves & screenshots; antivirus and metadata pipeline.  
- **Admin Console** — protected routes for moderation, catalogue flags, role changes.

*(Full dataflows are in §2; this section focuses on **permissions**.)*

---
<br>

### 3.2 System Roles

- **Regular User** — default on registration; can use all “player” features, create content that doesn’t change platform policy.  
- **Curator** — trusted creator/maintainer; can publish **fan-made achievement sets** and **sticker album templates**, assist with catalogue quality.  
- **Admin** — platform operators; moderation, catalogue policy flags (UA localisation/origin, Russia-linked), role management, integration/config controls.

**SR-ROLE-01 (M)** — The system **shall** enforce role-based access checks on all protected actions and data.  
**SR-ROLE-02 (M)** — New accounts **shall** be created as **Regular User**.

---
<br>

### 3.3 Capability by Role (high-level)

| Capability Area | Regular User | Curator | Admin |
|---|---|---|---|
| Account, profile, link/unlink Steam/RA | ✅ | ✅ | ✅ |
| Upload/manage **saves**, set visibility, report items | ✅ | ✅ | ✅ |
| Use/join **achievement sets** (official/retro/fan-made) | ✅ | ✅ | ✅ |
| Create/edit **fan-made achievement sets** & subsets | — | ✅ | ✅ |
| Create/edit **sticker album templates** | — | ✅ | ✅ |
| Create **album instances**, upload screenshots | ✅ | ✅ | ✅ |
| Comment, rate, report content | ✅ | ✅ | ✅ |
| Moderate (hide/remove, warn/restrict) | — | — | ✅ |
| **Catalogue flags** (UA localisation/origin, Russia-linked) | — | suggest | ✅ set |
| **Compatibility overrides** for saves (legacy/incompatible) | — | suggest | ✅ set |
| Role management (promote/demote Curator) | — | — | ✅ |
| Integration & config toggles | — | — | ✅ |

**SR-ROLE-03 (M)** — Curators **shall** have create/edit rights for **fan-made sets** and **album templates**, but **not** for moderation or policy flags.  
**SR-ROLE-04 (M)** — Admins **shall** have moderation powers, policy flag control, save-compatibility overrides, role changes, and adapter/config access.

---
<br>

### 3.4 Role Changes & Approvals

**SR-ROLE-05 (M)** — The system **shall** support a Curator elevation flow with two paths:  
- **Request & Review:** Regular User submits a short application; Admin reviews and approves/declines.  
- **Direct Elevation:** Admin can elevate/demote directly (e.g., pilot cohorts).

**SR-ROLE-06 (M)** — All role changes **shall** be **audited** (actor, target, timestamp, reason).  
**SR-ROLE-07 (S)** — On approval/decline, the user **should** receive a brief in-product notice and transactional email.  
**SR-ROLE-08 (M)** — Admins **shall** be able to **temporarily suspend** content-creation privileges for a user without deleting the account.

---
<br>

### 3.5 Visibility, Privacy & Safety

**Defaults & Controls**  
**SR-VIS-01 (M)** — New **saves** and **screenshots** default to **Private**; sharing is explicit (Public / Link-only).  
**SR-VIS-02 (M)** — Users **shall** be able to change visibility of their own items at any time (except when frozen by moderation).  
**SR-VIS-03 (M)** — Public content **shall not** expose sensitive personal data; only nickname/avatar and necessary metadata (e.g., save compatibility badges).

**Reporting & Moderation Hooks**  
**SR-ROLE-09 (M)** — Any user **shall** be able to **report** saves, screenshots, sets, albums, comments, or profiles.  
**SR-ROLE-10 (M)** — Reports **shall** enter a moderation queue; Admin actions (keep/hide/remove, warn/restrict) are reflected in UI with minimal rationale text.

**Comments & Community**  
**SR-ROLE-11 (M)** — Comments **shall** be enabled at least at the **set/album level**; Admins can disable or throttle per item if abused.  
**SR-ROLE-12 (S)** — Basic rate-limits **should** apply to comments and reports to reduce spam.

---
<br>

### 3.6 External Profile Linking (Steam / RetroAchievements)

**SR-LINK-01 (M)** — Users **shall** be able to link and unlink their **Steam** and **RetroAchievements** profiles via an explicit flow.  
**SR-LINK-02 (M)** — The UI **shall** explain what becomes visible (e.g., achievements/progress) and that integration is **read-only**; unlinking is always available.  
**SR-LINK-03 (M)** — The system **shall not** store external passwords; only the minimum tokens/IDs needed for read-only sync.  
**SR-LINK-04 (S)** — Show **“last synced …”** on achievement panels; failed syncs do not block navigation.

---
<br>

### 3.7 Save Compatibility & Policy Touchpoints (by Role)

**SR-ROLE-13 (M)** — **Regular Users** can submit “Worked / Didn’t work” feedback for a save against their current **Steam build** or **retro region/emulator**.  
**SR-ROLE-14 (M)** — **Curators** can propose corrections to save metadata (e.g., wrong build/region) and participate in **compatibility reviews** opened by the build-watcher.  
**SR-ROLE-15 (M)** — **Admins** can set `legacy` / `incompatible` statuses and `incompatible_since_build` flags; changes are audited.

**UA/Russia Catalogue Policy**  
**SR-ROLE-16 (M)** — Curators may **suggest** UA localisation/origin or Russia-linked updates; **Admins** approve and apply flags.  
**SR-ROLE-17 (M)** — For users in Ukrainian locale, the UI **shall** render the additional context/annotations defined by policy.

---
<br>

### 3.8 Non-Goals (Role Model)

**SR-ROLE-18 (M)** — Roles do **not** grant the ability to:  
- write back to Steam/RA;  
- alter official/retro achievements at source;  
- bypass antivirus/validation pipelines;  
- access raw secrets/keys.

---
<br>

The rest of the SRS references this role model whenever an action requires **Regular**, **Curator**, or **Admin** permissions.

---
<br>
<br>

## 4. Functional Requirements by Module

This chapter turns business intent into concrete, testable system behaviour.  
Each sub-section (4.1–4.9) focuses on one area of the product and defines requirements with stable IDs (e.g., `SR-ACC-01`).

---
<br>

### 4.0.1 How to read each module
Every requirement in 4.1–4.9 will follow a consistent shape so engineers, QA, and curators can act on it without guesswork.

- **ID & Traceability** — `SR-<MODULE>-NN` with links back to BRD items (`BR-…`, `NFR-…`).  
- **Purpose** — why this exists (business value / user impact).  
- **Preconditions** — what must be true before the flow starts (auth, role, linked profile, etc.).  
- **Normal Flow** — the happy path, end-to-end.  
- **Error/Edge Conditions** — what we show/do when things go sideways (invalid input, timeouts, external outages).  
- **Permissions** — which roles can do this (Regular / Curator / Admin).  
- **Data Touchpoints** — entities/fields affected (only what’s necessary to understand behaviour).  
- **UX Notes** — any critical UI feedback, warnings, or accessibility/localisation specifics.  
- **NFR Hooks** — relevant performance, security, privacy, or localisation constraints.

> Cross-cutting: the **save version-compatibility layer** (Steam build/manifest IDs, retro region/emulator facets, `confirmed/untested/legacy/incompatible` statuses, community confirmations, Admin overrides) is referenced wherever it matters — not just in Saves (4.3), but also in Catalogue (4.2), Discovery (4.8), and Moderation (4.7).

---
<br>

### 4.0.2 Modules at a glance (what each sub-section will specify)

- **4.1 Accounts & Authentication (`SR-ACC-*`)**  
  Sign-up, sign-in/out, password reset, EN/UA locale selection, link/unlink **Steam** and **RetroAchievements** (read-only), safe visibility of linked info.

- **4.2 Game Catalogue & Localisation Flags (`SR-CAT-*`)**  
  Creating/updating game + platform variants, showing pillar availability (saves/achievements/albums), and applying **UA-localisation**, **UA-origin**, and **Russia-linked** flags.  
  *Also defines how catalogue entries surface save-compatibility expectations on game pages.*

- **4.3 Save Management (`SR-SAV-*`)**  
  Uploading, storing, listing and sharing saves; reporting; and the **version-compatibility model** (Steam build/manifest capture, community confirmations, legacy/archiving, retro region/emulator facets, download-time warnings).

- **4.4 Achievement Tracking (`SR-ACH-*`)**  
  Reflecting **official (Steam)** and **retro (RA)** progress (read-only), plus **fan-made sets** (structure, criteria, hints, evidence links, comments, statistics).  
  *Includes safe syncing windows and “last updated …” indicators.*

- **4.5 Sticker Albums (`SR-ALB-*`)**  
  Curator templates, spoiler-aware slots, user album instances, screenshot uploads with light AI/rule checks, completion and badges, per-album statistics.

- **4.6 User Profile & Progress View (`SR-PRO-*`)**  
  Profile fields and visibility, role display, per-game progress pages consolidating saves/achievements/albums, curator attribution, safe display of linked external profiles.

- **4.7 Moderation & Reporting (`SR-GOV-*`)**  
  Report flows for saves/screenshots/sets/albums/comments/profiles; moderation queue; actions (keep/hide/remove, warn/restrict); lightweight audit breadcrumbs.

- **4.8 Discovery & Recommendations (`SR-DIS-*`)**  
  Search and faceted browse (including **compatibility facets**), game detail pages, and a minimal “help me choose” recommender with clear reject/refresh behaviour.

- **4.9 Integrations & Background Jobs (`SR-INT-*`)**  
  Read-only sync with **Steam** and **RetroAchievements**, **IGDB** imports, transactional email, build-change watcher for Steam titles with public saves, validation/archiving jobs, and graceful degradation.

---
<br>
<br>
