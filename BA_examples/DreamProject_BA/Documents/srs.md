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

### 4.1 Accounts & Authentication

> **Traceability:** BR-ACC-01..04, BR-PRO-01..02, BR-GOV-01; NFR-SEC-01..05, NFR-LOC-01..02, NFR-UX-01..05  
> **Module prefix:** `SR-ACC-*`

---
<br>

#### SR-ACC-01 — Register Account & Verify Email (M)
**Traceability:** BR-ACC-01; NFR-SEC-01, NFR-UX-01  
**Purpose:** Create a unique account and confirm email ownership before enabling public sharing and external links.  
**Preconditions:** User is signed out.  
**Normal Flow:**  
1) User submits *email, password, nickname, locale (EN/UA)* and accepts terms.  
2) System creates Account+Profile with `email_verified=false`, role=`Regular`.  
3) Sends verification email; user opens one-time link; `email_verified=true`.  
**Error/Edge Conditions:** Email in use → generic failure; token expired/invalid → offer resend; provider outage → clear retry advice.  
**Permissions:** Public (unauthenticated).  
**Data Touchpoints:** `Account(email, password_hash, email_verified)`, `Profile(nickname, locale)`; audit “account_created”, “email_verified”.  
**UX Notes:** Persistent yet gentle “Verify email” banner until verified; one-click resend with cooldown.  
**NFR Hooks:** Rate-limit signups; CAPTCHA on suspicious traffic; strong KDF (Argon2/bcrypt).

---
<br>

#### SR-ACC-02 — Sign In & Session Management (M)
**Traceability:** BR-ACC-01; NFR-SEC-01..03, NFR-UX-02  
**Purpose:** Let users authenticate safely and keep sessions stable.  
**Preconditions:** Account exists.  
**Normal Flow:**  
1) User enters email + password.  
2) System issues secure session/refresh tokens.  
3) Optionally “remember me” extends session lifetime.  
**Error/Edge Conditions:** Wrong creds → generic error; too many attempts → temporary lock/slowdown; session theft attempt → invalidate and force re-login.  
**Permissions:** Public (signin), authenticated thereafter.  
**Data Touchpoints:** Session store; `last_login_at`.  
**UX Notes:** No account enumeration in errors; show current session device list if available.  
**NFR Hooks:** CSRF protections for cookie flows; secure, httpOnly cookies.

---
<br>

#### SR-ACC-03 — Sign Out (M)
**Traceability:** BR-ACC-01; NFR-SEC-01  
**Purpose:** Allow explicit session termination.  
**Preconditions:** Signed in.  
**Normal Flow:** User clicks Sign out → tokens revoked.  
**Error/Edge Conditions:** Expired/invalid session → ignore and land on signed-out state.  
**Permissions:** Authenticated.  
**Data Touchpoints:** Session store.  
**UX Notes:** Clear confirmation of sign-out.  
**NFR Hooks:** Idempotent, safe to call multiple times.

---
<br>

#### SR-ACC-04 — Password Reset (M)
**Traceability:** BR-ACC-01; NFR-SEC-01..02, NFR-UX-02  
**Purpose:** Recover access without exposing account existence.  
**Preconditions:** Email was registered (not disclosed).  
**Normal Flow:**  
1) User submits email.  
2) If not throttled, send time-limited reset link.  
3) User sets new password; invalidate other sessions.  
**Error/Edge Conditions:** Expired/bad link → offer new link; email provider outage → explain delay.  
**Permissions:** Public (request), authenticated (post-change session).  
**Data Touchpoints:** One-time token store; password hash; session store.  
**UX Notes:** Neutral messaging (“If an account exists, we’ve sent…”).  
**NFR Hooks:** Rate-limit requests; short token TTL (e.g., 60 min).

---
<br>

#### SR-ACC-05 — Account Security Controls (S)
**Traceability:** NFR-SEC-01..03  
**Purpose:** Guard against brute-force and abuse.  
**Preconditions:** None.  
**Normal Flow:** Apply progressive delays/temporary lock after N failed attempts; optional CAPTCHA on high-risk events.  
**Error/Edge Conditions:** Lock state reached → communicate next allowed attempt.  
**Permissions:** System-wide.  
**Data Touchpoints:** Auth attempt counters; risk flags.  
**UX Notes:** Keep errors generic; avoid revealing lock specifics.  
**NFR Hooks:** IP/user-based throttling; monitoring for spikes.

---
<br>

#### SR-ACC-06 — Profile Management & Localisation (M)
**Traceability:** BR-PRO-01; NFR-LOC-01..02, NFR-UX-01  
**Purpose:** Let users control nickname, avatar, and language.  
**Preconditions:** Signed in.  
**Normal Flow:** Edit nickname, avatar URL/upload, locale (EN/UA).  
**Error/Edge Conditions:** Nickname collision → suggest alternatives; invalid image → reject with reason.  
**Permissions:** Owner; Admin may edit for safety reasons.  
**Data Touchpoints:** `Profile(nickname, avatar_url, locale)`.  
**UX Notes:** First-run locale picker; UA users see UA-specific notes in catalogue.  
**NFR Hooks:** Image size/type caps; CDN thumbnails.

---
<br>

#### SR-ACC-07 — Visibility Defaults (M)
**Traceability:** BR-PRO-05; NFR-SEC-05, NFR-UX-03  
**Purpose:** Default new saves/screens to private.  
**Preconditions:** Signed in.  
**Normal Flow:** User sets default visibility (Private/Public/Link-only); applied to future uploads.  
**Error/Edge Conditions:** None significant.  
**Permissions:** Owner.  
**Data Touchpoints:** `Profile.visibility_defaults`.  
**UX Notes:** Short explainer tooltip near the toggle.  
**NFR Hooks:** None beyond persistence.

---
<br>

#### SR-ACC-08 — Link Steam Profile (Read-only) (M)
**Traceability:** BR-INT-01, BR-ACH-01, BR-ACH-03; NFR-SEC-03  
**Purpose:** Show official achievements/progress for linked users.  
**Preconditions:** Signed in; email verified.  
**Normal Flow:**  
1) Start link; complete official consent/identifier flow.  
2) Store minimal ID/token; mark link `active`; background sync.  
**Error/Edge Conditions:** User cancels/denied; rate-limit → show status and keep profile usable.  
**Permissions:** Owner; Admin can sever link for safety.  
**Data Touchpoints:** `ExternalLinks.steam_id`, `last_synced_at(status)`.  
**UX Notes:** Clear “read-only”, “unlink anytime”, and “last synced …”.  
**NFR Hooks:** Bounded sync frequency; retries with backoff.

---
<br>

#### SR-ACC-09 — Link RetroAchievements Profile (Read-only) (M)
**Traceability:** BR-INT-02, BR-ACH-02..04; NFR-SEC-03  
**Purpose:** Reflect retro sets/progress for linked users.  
**Preconditions:** Signed in; email verified.  
**Normal Flow:** Enter RA identifier/flow; store minimal ID; background sync.  
**Error/Edge Conditions:** Invalid RA ID; API outage → stale indicator only.  
**Permissions:** Owner; Admin can sever link for safety.  
**Data Touchpoints:** `ExternalLinks.ra_id`, `last_synced_at(status)`.  
**UX Notes:** Same clarity as Steam linking.  
**NFR Hooks:** Rate-limited sync; graceful degradation.

---
<br>

#### SR-ACC-10 — External Linking UX & Safety (M)
**Traceability:** BR-INT-01..02; NFR-SEC-03, NFR-UX-03  
**Purpose:** Keep linking transparent and reversible.  
**Preconditions:** Signed in.  
**Normal Flow:** Panels show *Linked / Unlinked*, *Last synced …*, and a **Refresh** (cooldown).  
**Error/Edge Conditions:** Refresh during cooldown → tooltip; provider error → non-blocking banner.  
**Permissions:** Owner.  
**Data Touchpoints:** `ExternalLinks.status/last_synced_at`.  
**UX Notes:** Explain data scope; no write-back; unlink confirmation.  
**NFR Hooks:** Cooldown (e.g., 15 min); telemetry on failures.

---
<br>

#### SR-ACC-11 — Role Model Basics (M)
**Traceability:** BR-PRO-02, BR-GOV-01  
**Purpose:** Ensure consistent default role and visibility of role.  
**Preconditions:** Account exists.  
**Normal Flow:** New users are `Regular`; role shown in settings/profile.  
**Error/Edge Conditions:** None.  
**Permissions:** Admin can view/change roles.  
**Data Touchpoints:** `Account.role`.  
**UX Notes:** Brief description of role capabilities.  
**NFR Hooks:** Audit role reads/writes as needed.

---
<br>

#### SR-ACC-12 — Curator Elevation & Audit (M)
**Traceability:** BR-ACC-04, BR-GOV-01  
**Purpose:** Promote engaged users to Curator with governance.  
**Preconditions:** Signed in; Admin present.  
**Normal Flow:**  
1) User requests elevation **or** Admin elevates directly.  
2) Decision recorded; user notified.  
**Error/Edge Conditions:** Duplicate request; missing rationale → prompt.  
**Permissions:** Admin approves/denies; user submits request.  
**Data Touchpoints:** Role change log (actor, target, timestamp, reason).  
**UX Notes:** Simple form; clear expectations of Curator powers.  
**NFR Hooks:** Notification via email + in-app; audit trail retained.

---
<br>

#### SR-ACC-13 — Account Deletion (Soft-Delete) (S)
**Traceability:** NFR-SEC-05, NFR-LAW-02  
**Purpose:** Respect user choice while preserving platform integrity.  
**Preconditions:** Signed in; ownership confirmed.  
**Normal Flow:**  
1) User initiates deletion; confirm.  
2) Soft-delete account; anonymise public content where feasible; revoke tokens.  
**Error/Edge Conditions:** Content under active investigation → block with rationale; email provider down → in-app confirmation still completes.  
**Permissions:** Owner; Admin can hard-delete per policy.  
**Data Touchpoints:** Account status; content ownership fields.  
**UX Notes:** Plain-language summary of what is removed/kept.  
**NFR Hooks:** Background job to anonymise large content graphs.

---
<br>

#### SR-ACC-14 — Transactional Emails (M)
**Traceability:** BR-ACC-01, BR-GOV-01  
**Purpose:** Deliver essential email for verification, resets, and role/security notices.  
**Preconditions:** Email provider configured.  
**Normal Flow:** Queue messages via provider; show in-product confirmation.  
**Error/Edge Conditions:** Provider outage → retry and inform user.  
**Permissions:** System.  
**Data Touchpoints:** Outbox log, delivery status if available.  
**UX Notes:** Users can’t opt out of essentials.  
**NFR Hooks:** Retry with backoff; dedupe to avoid spam.

---
<br>

#### SR-ACC-15 — Abuse Controls on Auth Endpoints (S)
**Traceability:** NFR-SEC-01..03  
**Purpose:** Reduce brute force and scripted abuse.  
**Preconditions:** None.  
**Normal Flow:** Rate limits per IP/account; anomaly detection toggles CAPTCHA.  
**Error/Edge Conditions:** Limit exceeded → “Try again later.”  
**Permissions:** System.  
**Data Touchpoints:** Throttle counters; security logs.  
**UX Notes:** Keep responses minimal to avoid info leaks.  
**NFR Hooks:** Alerting on spikes; WAF rules where applicable.

---
<br>

#### SR-ACC-16 — First-Run Locale & UA Policy Context (M)
**Traceability:** BR-PRO-01, BR-CAT-04; NFR-LOC-01..02, NFR-UX-01  
**Purpose:** Set a clear language baseline and inform UA users about policy notes early.  
**Preconditions:** Signed out or newly registered.  
**Normal Flow:** On first run, show compact locale selector (EN/UA). If UA chosen, show one-time tooltip about UA localisation/origin and Russia-linked flags.  
**Error/Edge Conditions:** Blocked localStorage/cookies → fall back to profile page prompt after sign-in.  
**Permissions:** Public.  
**Data Touchpoints:** `Profile.locale` (after sign-in); client hint stored.  
**UX Notes:** Non-intrusive; accessible; dismissible.  
**NFR Hooks:** Strings fully localised; no hard dependency on geo/IP.

---
<br>

#### SR-ACC-17 — Per-Content Default Visibility (M)
**Traceability:** BR-PRO-05; NFR-SEC-05, NFR-UX-03  
**Purpose:** Let users predefine defaults for **saves** and **screenshots** separately.  
**Preconditions:** Signed in.  
**Normal Flow:** In Settings, user sets default visibility (Private/Public/Link-only) for each content type; applied to future uploads.  
**Error/Edge Conditions:** None significant.  
**Permissions:** Owner.  
**Data Touchpoints:** `Profile.visibility_defaults.{saves,screens}`.  
**UX Notes:** Short tooltips explaining implications; link to Privacy basics.  
**NFR Hooks:** Server enforces defaults on upload endpoints.

---
<br>

#### SR-ACC-18 — External Profile Panel Visibility (M)
**Traceability:** BR-INT-01..02, BR-PRO-05; NFR-SEC-03  
**Purpose:** Control how linked **Steam/RA** panels appear to others.  
**Preconditions:** Signed in; link active.  
**Normal Flow:** User chooses panel visibility: Private / Profile-only / Public.  
**Error/Edge Conditions:** If provider down, keep last-known panel but mark “stale”.  
**Permissions:** Owner; Admin may hide for safety.  
**Data Touchpoints:** `ExternalLinks.visibility`.  
**UX Notes:** Clear note that visibility affects *display* only; links remain read-only.  
**NFR Hooks:** Respect on all profile/game pages and APIs.

---
<br>

#### SR-ACC-19 — User-Initiated Sync Refresh (S)
**Traceability:** BR-INT-01..02; NFR-SEC-03, NFR-UX-03  
**Purpose:** Give users a controlled “Refresh now” without hammering providers.  
**Preconditions:** Signed in; link active.  
**Normal Flow:** User clicks **Refresh** → enqueue sync if not in cooldown → update `last_synced_at` on completion.  
**Error/Edge Conditions:** Cooldown active → tooltip shows remaining time; provider error → non-blocking banner.  
**Permissions:** Owner.  
**Data Touchpoints:** `ExternalLinks.last_synced_at`, refresh queue.  
**UX Notes:** Show spinner + “queued” state.  
**NFR Hooks:** Cooldown (e.g., 15 min), retries with backoff.

---
<br>

#### SR-ACC-20 — Sync Staleness & Escalation (S)
**Traceability:** BR-INT-01..02; NFR-UX-03  
**Purpose:** Communicate stale panels and self-heal.  
**Preconditions:** Link active.  
**Normal Flow:** If `last_synced_at` > threshold (e.g., 72h), show “stale” chip and attempt background refresh once.  
**Error/Edge Conditions:** Repeated failures → suggest unlink/relink.  
**Permissions:** Owner.  
**Data Touchpoints:** `ExternalLinks.status`, `last_synced_at`.  
**UX Notes:** Polite, non-alarming copy.  
**NFR Hooks:** One silent retry per threshold breach.

---
<br>

#### SR-ACC-21 — Session List & Remote Sign-Out (S)
**Traceability:** NFR-SEC-01..02  
**Purpose:** Let users kill other active sessions.  
**Preconditions:** Signed in.  
**Normal Flow:** Settings shows current sessions (device/approx location/last seen); user can “Sign out others”.  
**Error/Edge Conditions:** Token already expired → ignore; show success anyway.  
**Permissions:** Owner.  
**Data Touchpoints:** Session store.  
**UX Notes:** Plain-language device labels.  
**NFR Hooks:** Idempotent; secure token revocation.

---
<br>

#### SR-ACC-22 — Change Password in Settings (M)
**Traceability:** NFR-SEC-01..02  
**Purpose:** Rotate credentials without reset flow.  
**Preconditions:** Signed in; knows current password.  
**Normal Flow:** Enter current + new password → update; invalidate other sessions.  
**Error/Edge Conditions:** Wrong current password; weak new password → explain rules.  
**Permissions:** Owner.  
**Data Touchpoints:** Password hash; session store.  
**UX Notes:** Clear confirmation and security note.  
**NFR Hooks:** KDF; rate-limit attempts.

---
<br>

#### SR-ACC-23 — Change Email with Re-verification (M)
**Traceability:** NFR-SEC-01..02  
**Purpose:** Safely move the account to a new address.  
**Preconditions:** Signed in; password re-entry required.  
**Normal Flow:** User enters new email → send verify link → on confirm, switch primary email.  
**Error/Edge Conditions:** Link expired → resend; provider outage → advise later retry.  
**Permissions:** Owner.  
**Data Touchpoints:** `Account.email`, `email_verified`.  
**UX Notes:** Warn that public sharing/linking pauses until verified.  
**NFR Hooks:** Audit event and notification to old email.

---
<br>

#### SR-ACC-24 — Nickname Change & Uniqueness (M)
**Traceability:** BR-PRO-01; NFR-UX-01  
**Purpose:** Manage display identity safely.  
**Preconditions:** Signed in.  
**Normal Flow:** User changes nickname; system enforces global uniqueness.  
**Error/Edge Conditions:** Collision → suggest variants; rate-limit rapid changes.  
**Permissions:** Owner; Admin can force-set on abuse.  
**Data Touchpoints:** `Profile.nickname`; optional change log.  
**UX Notes:** Show how it appears publicly.  
**NFR Hooks:** Case-insensitive uniqueness.

---
<br>

#### SR-ACC-25 — Avatar Upload Pipeline (S)
**Traceability:** NFR-SEC-05, NFR-PERF-02  
**Purpose:** Keep avatars safe and lightweight.  
**Preconditions:** Signed in.  
**Normal Flow:** User uploads image → type/size validated → virus scan → stored → thumbnail generated.  
**Error/Edge Conditions:** Invalid type/size → clear error; scan fail → reject.  
**Permissions:** Owner.  
**Data Touchpoints:** Object storage; `Profile.avatar_url`.  
**UX Notes:** Live preview; guidance on size.  
**NFR Hooks:** Size caps (e.g., 1–2MB); CDN thumbnails.

---
<br>

#### SR-ACC-26 — Terms & Privacy Versioning (M)
**Traceability:** BR-ACC-01; NFR-LAW-01  
**Purpose:** Record which terms a user accepted.  
**Preconditions:** Sign-up or next sign-in after terms update.  
**Normal Flow:** Show summary of changes; require accept; store `tos_version_accepted`.  
**Error/Edge Conditions:** Decline → restrict to read-only until accepted (or allow account deletion).  
**Permissions:** Owner.  
**Data Touchpoints:** `Account.tos_version_accepted`, timestamp.  
**UX Notes:** Concise, accessible language with link to full text.  
**NFR Hooks:** Versioned documents; audit trail.

---
<br>

#### SR-ACC-27 — Security Notifications (M)
**Traceability:** NFR-SEC-01..03  
**Purpose:** Alert users about critical account events.  
**Preconditions:** Email configured.  
**Normal Flow:** Send email on password change, email change, new device session, link/unlink external.  
**Error/Edge Conditions:** Provider outage → queue for retry.  
**Permissions:** System.  
**Data Touchpoints:** Outbox log.  
**UX Notes:** Plain, actionable copy with “not you?” guidance.  
**NFR Hooks:** Rate-limit duplicates; backoff retries.

---
<br>

#### SR-ACC-28 — Account Deactivation & Grace Period (S)
**Traceability:** NFR-LAW-02, NFR-SEC-05  
**Purpose:** Allow undo before permanent removal.  
**Preconditions:** Signed in; deletion requested.  
**Normal Flow:** Mark account “deactivated”; login within **14 days** restores; after grace, proceed to soft-delete/anonymise (see SR-ACC-13).  
**Error/Edge Conditions:** Attempts after purge → offer new signup.  
**Permissions:** Owner; Admin can bypass in abuse cases.  
**Data Touchpoints:** `Account.status`, purge job.  
**UX Notes:** Clear countdown and what remains public.  
**NFR Hooks:** Timed job; integrity checks before purge.

---
<br>

#### SR-ACC-29 — Data Export Request (Nice-to-have)
**Traceability:** NFR-LAW-02  
**Purpose:** Let users obtain a copy of key account data.  
**Preconditions:** Signed in.  
**Normal Flow:** User requests export → queued → email link to download JSON/CSV of profile, links, content **metadata** (not files).  
**Error/Edge Conditions:** Large queue → set expectations; link expiry.  
**Permissions:** Owner.  
**Data Touchpoints:** Export service, object storage for file.  
**UX Notes:** Explain scope and exclusions.  
**NFR Hooks:** Rate-limit; signed URL with short TTL.

---
<br>

#### SR-ACC-30 — Verification Gating Enforcement (M)
**Traceability:** BR-ACC-02; NFR-SEC-01  
**Purpose:** Ensure unverified accounts can’t expose or link sensitive things.  
**Preconditions:** Signed in; `email_verified=false`.  
**Normal Flow:** Block public sharing and external linking; show persistent “Verify email” prompt with resend.  
**Error/Edge Conditions:** Resend throttled; token expired → regenerate.  
**Permissions:** Owner.  
**Data Touchpoints:** `Account.email_verified`.  
**UX Notes:** Friendly, not punitive.  
**NFR Hooks:** Global middleware guard on restricted actions.

---
<br>

#### SR-ACC-31 — Anti-Enumeration for Email & Reset (M)
**Traceability:** NFR-SEC-01..02  
**Purpose:** Prevent probing for valid accounts.  
**Preconditions:** Public endpoints.  
**Normal Flow:** Use **neutral messages** for signup, reset, and email change requests; uniform response timing where practical.  
**Error/Edge Conditions:** High volume from IP → throttle or CAPTCHA.  
**Permissions:** System.  
**Data Touchpoints:** Throttle counters, security logs.  
**UX Notes:** Messages like “If an account exists…”  
**NFR Hooks:** WAF rules; anomaly alerts.

---
<br>

### 4.2 Game Catalogue & Localisation Flags

> **Traceability:** BR-CAT-01..05, BR-SAV-03..05, BR-INT-01..03, BR-GOV-01..04; NFR-LOC-01..02, NFR-SEC-01..05, NFR-UX-01..05  
> **Module prefix:** `SR-CAT-*`  
> **Cross-cutting:** save-compatibility layer (Steam build/manifest; retro region/emulator) is surfaced on game/variant pages and in search facets.

---
<br>

#### SR-CAT-01 — Game Entity (M)
**Purpose:** Store canonical game info used across the platform.  
**Preconditions:** Admin access.  
**Normal Flow:** Create Game with title, description, release year, cover, IGDB ref; publish after review.  
**Error/Edge:** Missing/ambiguous IGDB → set fields to “unknown” with note.  
**Permissions:** Admin (create/edit); Curator (suggest).  
**Data Touchpoints:** `Game{game_id,title,alt_titles[],description,genres[],franchises[],release_year,cover_asset,igdb_ref}`.  
**UX Notes:** Show human title + year; alt titles in tooltip.  
**NFR Hooks:** Index `title`/`alt_titles` for search.

---
<br>

#### SR-CAT-02 — Platform Variant Entity (M)
**Purpose:** Model concrete playable variants (e.g., PC/Steam, PS1/Emulator).  
**Preconditions:** Game exists.  
**Normal Flow:** Add Variant with platform code, label, Steam AppID?, RA id?, DLC/editions[], status.  
**Error/Edge:** Duplicate/obsolete variants → mark `hidden`.  
**Permissions:** Admin (edit); Curator (suggest).  
**Data Touchpoints:** `Variant{variant_id,game_id,platform_code,variant_label,steam_app_id?,ra_game_id?,dlc_editions[],status,notes}`.  
**UX Notes:** Variant selector on game page.  
**NFR Hooks:** Consistent keys for joins to saves/achievements.

---
<br>

#### SR-CAT-03 — Pillar Availability (M)
**Purpose:** Tell users which pillars apply to a variant.  
**Preconditions:** Variant exists.  
**Normal Flow:** Set `saves_viability`, `official_achievements`, `retro_achievements`, counts for fan sets/albums.  
**Error/Edge:** Unknown → default cautious (see SR-CAT-29).  
**Permissions:** Admin set; Curator suggest.  
**Data Touchpoints:** `VariantPillars{...}`.  
**UX Notes:** Chips on variant page (“Saves: Viable/Conditional/Not viable”).  
**NFR Hooks:** Keep counts in denormalised fields for speed.

---
<br>

#### SR-CAT-04 — UA/Russia Policy Flags (M)
**Purpose:** Capture UA localisation/origin and Russia-linked signals.  
**Preconditions:** Game exists.  
**Normal Flow:** Admin sets flags with short justification + sources.  
**Error/Edge:** Disputed evidence → see SR-CAT-18.  
**Permissions:** Admin set; Curator propose.  
**Data Touchpoints:** `Policy{ua_localisation,ua_origin,russia_linked,policy_notes,sources[]}`.  
**UX Notes:** Locale-sensitive rendering (see SR-CAT-16).  
**NFR Hooks:** Audit policy changes.

---
<br>

#### SR-CAT-05 — External References & Evidence (S)
**Purpose:** Keep minimal links that justify catalogue data.  
**Preconditions:** None.  
**Normal Flow:** Store IGDB/Kuli/Artemiano/official links per field.  
**Error/Edge:** Dead link → mark stale.  
**Permissions:** Admin; Curator suggest.  
**Data Touchpoints:** `sources[]` per Game/Variant/Policy.  
**UX Notes:** “Source” icon opens list.  
**NFR Hooks:** No hotlinking heavy assets.

---
<br>

#### SR-CAT-06 — IGDB Seed & Human Review (M)
**Purpose:** Bootstrap entries quickly but safely.  
**Preconditions:** Admin signed in.  
**Normal Flow:** Search IGDB → import skeleton → Admin reviews/edits → publish.  
**Error/Edge:** IGDB outage → manual entry allowed.  
**Permissions:** Admin.  
**Data Touchpoints:** Game/Variant fields populated from IGDB.  
**UX Notes:** Clear “Imported from IGDB” badge until reviewed.  
**NFR Hooks:** Rate-limit IGDB calls; cache results.

---
<br>

#### SR-CAT-07 — Admin Edit UI (M)
**Purpose:** Let Admins maintain catalogue without DB access.  
**Preconditions:** Admin signed in.  
**Normal Flow:** Edit core fields, pillar availability, Steam/RA IDs, DLC, policy flags.  
**Error/Edge:** Conflicting edits → optimistic locking warning.  
**Permissions:** Admin.  
**Data Touchpoints:** Game/Variant/Policy.  
**UX Notes:** Inline validation; preview before publish.  
**NFR Hooks:** Change log (SR-CAT-09).

---
<br>

#### SR-CAT-08 — Curator Suggestions (M)
**Purpose:** Harness curator knowledge without direct write.  
**Preconditions:** Curator role.  
**Normal Flow:** Submit suggestion (diff + reason) → queue → Admin accepts/declines.  
**Error/Edge:** Duplicate suggestion → merge notice.  
**Permissions:** Curator (submit); Admin (moderate).  
**Data Touchpoints:** `CatalogueSuggestions`.  
**UX Notes:** Show status to submitter.  
**NFR Hooks:** Notifications on decision.

---
<br>

#### SR-CAT-09 — Catalogue Change Log (S)
**Purpose:** Track who changed what and why.  
**Preconditions:** Edit action.  
**Normal Flow:** Record editor, timestamp, field diffs, reason.  
**Error/Edge:** None.  
**Permissions:** System; Admin view.  
**Data Touchpoints:** `CatChangeLog`.  
**UX Notes:** Viewable timeline for Admins.  
**NFR Hooks:** Prune after retention window.

---
<br>

#### SR-CAT-10 — Saves Viability Determination (M)
**Purpose:** Set per-variant feasibility for save sharing.  
**Preconditions:** Variant exists.  
**Normal Flow:** Choose `viable` / `conditional` / `not_viable` per guidance.  
**Error/Edge:** Unclear data → default `conditional`.  
**Permissions:** Admin set; Curator suggest.  
**Data Touchpoints:** `VariantPillars.saves_viability`.  
**UX Notes:** Tooltip explains rationale.  
**NFR Hooks:** Tied to upload guard (SR-CAT-12).

---
<br>

#### SR-CAT-11 — Standard Viability Reasons (M)
**Purpose:** Provide consistent, translatable reasons.  
**Preconditions:** Setting viability.  
**Normal Flow:** Multi-select reasons: `profile_bound`, `server_only_progress`, `no_local_saves`, `anti_cheat_risk`, `cloud_lock_in`, `edition_dlc_mismatch`, `build_drift`, `other`.  
**Error/Edge:** None.  
**Permissions:** Admin set; Curator suggest.  
**Data Touchpoints:** `saves_viability_reason[]`.  
**UX Notes:** Reasons surface in tooltips/banners.  
**NFR Hooks:** Localised labels.

---
<br>

#### SR-CAT-12 — Upload Guardrail (M)
**Purpose:** Prevent meaningless uploads when sharing is not viable.  
**Preconditions:** Variant `not_viable`.  
**Normal Flow:** Block save upload; show explanation + help link.  
**Error/Edge:** API misuse → 4xx with reason code.  
**Permissions:** System.  
**Data Touchpoints:** None (no save created).  
**UX Notes:** Friendly message; link to why.  
**NFR Hooks:** Server-side enforcement.

---
<br>

#### SR-CAT-13 — Conditional Hints (S)
**Purpose:** Tell users what must match for success.  
**Preconditions:** Variant `conditional`.  
**Normal Flow:** Show hints like “Same DLC set required”, “Match Steam build/manifest recommended”.  
**Error/Edge:** Missing DLC list → omit that hint.  
**Permissions:** System.  
**Data Touchpoints:** Pull from Variant DLC list; saves layer.  
**UX Notes:** Short, non-alarmist.  
**NFR Hooks:** Localised.

---
<br>

#### SR-CAT-14 — Retro Facets Declaration (M)
**Purpose:** Encode region/emulator expectations for retro.  
**Preconditions:** Emulator variant.  
**Normal Flow:** Set expected regions (NTSC-U/J, PAL) and common emulator families/cores.  
**Error/Edge:** Unknown → show generic warning.  
**Permissions:** Admin set; Curator suggest.  
**Data Touchpoints:** `retro_regions[]`, `emulator_families[]`.  
**UX Notes:** Render on variant page; feed download warnings.  
**NFR Hooks:** Used by search facets.

---
<br>

#### SR-CAT-15 — Policy Flags Ownership & Evidence (M)
**Purpose:** Ensure flags are defensible.  
**Preconditions:** Admin editing.  
**Normal Flow:** Admin sets flags with short note and source links.  
**Error/Edge:** Missing source → soft warning, allow save with TODO.  
**Permissions:** Admin set; Curator propose.  
**Data Touchpoints:** `Policy.*`, `sources[]`.  
**UX Notes:** “Why shown” expandable note (UA locale prominent).  
**NFR Hooks:** Change log.

---
<br>

#### SR-CAT-16 — Locale-Sensitive Display (M)
**Purpose:** Show policy with the right emphasis by locale.  
**Preconditions:** User locale known.  
**Normal Flow:** UA locale → prominent flags and short policy text; EN locale → neutral icon/line with “Learn more”.  
**Error/Edge:** Locale unset → default to EN; show prompt.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Avoid overwhelming EN users; clarity for UA users.  
**NFR Hooks:** Full i18n coverage.

---
<br>

#### SR-CAT-17 — UA Filters & Hiding Russia-linked (S)
**Purpose:** Help UA users discover relevant games.  
**Preconditions:** UA locale.  
**Normal Flow:** Add quick filters: UA-localised, UA-origin; toggle to hide Russia-linked with notice.  
**Error/Edge:** None.  
**Permissions:** System.  
**Data Touchpoints:** Search facet config.  
**UX Notes:** Toggle is reversible with context text.  
**NFR Hooks:** Facet indexing.

---
<br>

#### SR-CAT-18 — Dispute & Correction Flow (M)
**Purpose:** Update flags when new evidence appears.  
**Preconditions:** Flag exists.  
**Normal Flow:** Curator submits dispute with links → Admin reviews → updates flags; change logged.  
**Error/Edge:** Low-quality source → decline with note.  
**Permissions:** Curator submit; Admin decide.  
**Data Touchpoints:** `CatalogueSuggestions`, `CatChangeLog`, `Policy.*`.  
**UX Notes:** Status visible to submitter.  
**NFR Hooks:** Notifications.

---
<br>

#### SR-CAT-19 — Pillar Chips on Variant Page (M)
**Purpose:** Make capabilities obvious at a glance.  
**Preconditions:** Variant loaded.  
**Normal Flow:** Render chips for Saves (Viable/Conditional/Not viable), Achievements (Official/Retro/Fan sets count), Albums (count).  
**Error/Edge:** Unknown data → show “?” chip with tooltip.  
**Permissions:** Public.  
**Data Touchpoints:** Variant pillar fields.  
**UX Notes:** Consistent icon set; tooltips link to help.  
**NFR Hooks:** Lightweight cached.

---
<br>

#### SR-CAT-20 — Link-to-Progress CTA (M)
**Purpose:** Encourage, not force, linking Steam/RA.  
**Preconditions:** User not linked.  
**Normal Flow:** Show small CTA to link profiles; do not block browsing.  
**Error/Edge:** None.  
**Permissions:** Public/Authenticated.  
**Data Touchpoints:** None.  
**UX Notes:** Soft CTA, not modal.  
**NFR Hooks:** None.

---
<br>

#### SR-CAT-21 — Compatibility Summary Block (M)
**Purpose:** Summarise version/region constraints.  
**Preconditions:** Variant loaded.  
**Normal Flow:** Steam variant → mention build/manifest awareness; Retro → region/emulator hints; link to details.  
**Error/Edge:** No data → generic caution.  
**Permissions:** Public.  
**Data Touchpoints:** Variant + saves layer.  
**UX Notes:** Short, scannable block.  
**NFR Hooks:** Localised.

---
<br>

#### SR-CAT-22 — DLC/Edition Callouts (S)
**Purpose:** Reduce common mismatches.  
**Preconditions:** DLC known.  
**Normal Flow:** Show “This game often requires DLC X for save compatibility.”  
**Error/Edge:** DLC list empty → skip.  
**Permissions:** Public.  
**Data Touchpoints:** Variant.dlc_editions.  
**UX Notes:** Non-judgmental copy.  
**NFR Hooks:** Localised.

---
<br>

#### SR-CAT-23 — Search/Browse Core Facets (M)
**Purpose:** Let users find variants that fit their goals.  
**Preconditions:** Index built.  
**Normal Flow:** Facets: platform, year, genre, saves_viability, official_achievements, retro_achievements, fan_sets_available, albums_available.  
**Error/Edge:** No results → helpful empty state.  
**Permissions:** Public.  
**Data Touchpoints:** Search index.  
**UX Notes:** Clear counts; chips in results.  
**NFR Hooks:** Target <~1.5s response typical.

---
<br>

#### SR-CAT-24 — Policy Facets (UA Locale) (S)
**Purpose:** Add UA-specific discoverability.  
**Preconditions:** UA locale.  
**Normal Flow:** Facets for UA-localisation level, UA-origin, hide Russia-linked.  
**Error/Edge:** None.  
**Permissions:** Public.  
**Data Touchpoints:** Index fields.  
**UX Notes:** Explanatory tooltip on hide toggle.  
**NFR Hooks:** Indexed fields.

---
<br>

#### SR-CAT-25 — Sort Options (S)
**Purpose:** Useful ordering beyond relevance.  
**Preconditions:** Search query.  
**Normal Flow:** Sort by popularity (activity), release year, recently updated.  
**Error/Edge:** Ties → stable secondary key.  
**Permissions:** Public.  
**Data Touchpoints:** Index sort fields.  
**UX Notes:** Remember last sort per session.  
**NFR Hooks:** Cached aggregates for popularity.

---
<br>

#### SR-CAT-26 — Result Badges (M)
**Purpose:** Convey key facts in lists.  
**Preconditions:** Results present.  
**Normal Flow:** Show badges for pillar availability and policy flags per locale rules.  
**Error/Edge:** Unknown → neutral badge.  
**Permissions:** Public.  
**Data Touchpoints:** Variant/Policy.  
**UX Notes:** Accessible labels for screen readers.  
**NFR Hooks:** Lightweight rendering.

---
<br>

#### SR-CAT-27 — Unknown/Conflicting Data Handling (M)
**Purpose:** Avoid false certainty.  
**Preconditions:** Missing or conflicting sources.  
**Normal Flow:** Mark fields as unknown; show small notice; allow Admin override.  
**Error/Edge:** None.  
**Permissions:** Admin override; Public view.  
**Data Touchpoints:** Field-level “unknown” flags.  
**UX Notes:** Honest, non-alarming tone.  
**NFR Hooks:** None.

---
<br>

#### SR-CAT-28 — Hidden Variants (S)
**Purpose:** Retain history without clutter.  
**Preconditions:** Variant obsolete/duplicate.  
**Normal Flow:** Mark `hidden`; exclude from search; keep deep links working with banner.  
**Error/Edge:** Attempt to upload save to hidden variant → block with reason.  
**Permissions:** Admin set.  
**Data Touchpoints:** `Variant.status`.  
**UX Notes:** “This variant is deprecated” banner.  
**NFR Hooks:** Redirects optional later.

---
<br>

#### SR-CAT-29 — Safe Defaults (M)
**Purpose:** Bias to caution where uncertain.  
**Preconditions:** Data gaps.  
**Normal Flow:** Default `saves_viability=conditional`; show caution tooltips; do not block usage unless `not_viable`.  
**Error/Edge:** None.  
**Permissions:** System.  
**Data Touchpoints:** Variant fields.  
**UX Notes:** Avoid over-promising.  
**NFR Hooks:** None.

---
<br>

#### SR-CAT-30 — Indexing (S)
**Purpose:** Keep catalogue snappy.  
**Preconditions:** Index service.  
**Normal Flow:** Index games/variants and facets; update within minutes of edits.  
**Error/Edge:** Index delay → stale badge.  
**Permissions:** System.  
**Data Touchpoints:** Search index.  
**UX Notes:** None.  
**NFR Hooks:** Async indexing with retries.

---
<br>

#### SR-CAT-31 — Caching (S)
**Purpose:** Reduce latency on hot pages.  
**Preconditions:** Cache layer.  
**Normal Flow:** Short-lived cache on game/variant pages; bust on edits.  
**Error/Edge:** Cache miss → fetch live.  
**Permissions:** System.  
**Data Touchpoints:** Cache keys per game/variant.  
**UX Notes:** None.  
**NFR Hooks:** ETag/Last-Modified headers.

---
<br>

#### SR-CAT-32 — Localisation of Strings (M)
**Purpose:** Full EN/UA coverage, including policy text.  
**Preconditions:** i18n framework.  
**Normal Flow:** All labels/tooltips/help copy localised; UA policy notes shown per locale rules.  
**Error/Edge:** Missing translation → fallback to EN with marker.  
**Permissions:** System.  
**Data Touchpoints:** Locale bundles.  
**UX Notes:** Glossary-consistent terms.  
**NFR Hooks:** Translation linting in CI.

---
<br>
<br>

### 4.3 Save Management

> **Traceability:** BR-SAV-01..08, BR-CAT-05, BR-GOV-02..03, BR-PRO-05, BR-DIS-01..03; NFR-SEC-01..05, NFR-LOC-01..02, NFR-UX-01..05, NFR-PERF-01..02  
> **Module prefix:** `SR-SAV-*`  
> **Cross-cutting:** Uses the version-compatibility layer (Steam **build/manifest IDs**; retro **region/emulator** facets; statuses: `confirmed / untested / legacy / incompatible`; **community confirmations**; **Admin overrides**).

---
<br>

#### SR-SAV-01 — Save Record Schema (M)
**Traceability:** BR-SAV-01  
**Purpose:** Persist the minimum, portable facts about a save.  
**Preconditions:** Game + Variant exist.  
**Normal Flow:** On successful upload, store: `game_id`, `variant_id`, uploader, visibility, timestamps, file size, checksum, **compatibility facets** (see SR-SAV-02), optional progress marker, and optional DLC/edition flags.  
**Error/Edge:** Missing variant → reject with guidance.  
**Permissions:** Uploader (create/edit non-file metadata), Admin (all).  
**Data Touchpoints:** `Save{...}` row + object storage pointer.  
**UX Notes:** Show a compact summary card on the save page.  
**NFR Hooks:** Keys indexed for search/filter.

---
<br>

#### SR-SAV-02 — Compatibility Facets (M)
**Traceability:** BR-SAV-02, BR-CAT-05  
**Purpose:** Make compatibility **machine-readable** and testable.  
**Preconditions:** Upload or edit.  
**Normal Flow:**  
- **Steam/modern:** capture `steam_app_id?`, `build_manifest_id?`, `tested_on_builds[]`, `incompatible_since_build?`, `status`.  
- **Retro:** capture `region`, `emulator_family`, optional `core/version` notes, and `status`.  
**Error/Edge:** Unknown build → mark as `untested`.  
**Permissions:** Uploader sets; Admin/Curator can correct (SR-SAV-12).  
**Data Touchpoints:** Fields above in `Save`.  
**UX Notes:** Badges + tooltips explain facets.  
**NFR Hooks:** Values validated against Variant.

---
<br>

#### SR-SAV-03 — Catalogue Variant Binding (M)
**Traceability:** BR-SAV-01, BR-CAT-03  
**Purpose:** Ensure saves exist only where **sharing is viable**.  
**Preconditions:** Variant loaded.  
**Normal Flow:** Bind save to Variant; if Variant is `not_viable`, block upload with reason (ties to SR-CAT-12).  
**Error/Edge:** Attempt to bind to hidden/deprecated variant → explain and suggest active one.  
**Permissions:** System enforced.  
**Data Touchpoints:** `save.variant_id`.  
**UX Notes:** Friendly, actionable rejection message.  
**NFR Hooks:** Server-side guard.

---
<br>

#### SR-SAV-04 — Upload Preconditions & Safety (M)
**Traceability:** BR-SAV-01, NFR-SEC-05  
**Purpose:** Keep uploads safe and predictable.  
**Preconditions:** Authenticated user; file within type/size limits.  
**Normal Flow:** Stream to object storage → **antivirus scan** (SR-SAV-24) → persist metadata.  
**Error/Edge:** Scan fail / unsupported type → reject with code; storage failure → retry once then report.  
**Permissions:** Regular/Curator/Admin.  
**Data Touchpoints:** Save row + file location.  
**UX Notes:** Progress bar; clear, humane errors.  
**NFR Hooks:** Rate limits, size caps.

---
<br>

#### SR-SAV-05 — Guided Metadata Capture (M)
**Traceability:** BR-SAV-02  
**Purpose:** Reduce mislabelling at source.  
**Preconditions:** Upload in progress.  
**Normal Flow:**  
- If Steam ID detectable or user linked Steam → suggest **AppID + current build** (editable).  
- If retro → require **Region** and suggest **emulator family**.  
**Error/Edge:** User skips unknown build → mark `untested`.  
**Permissions:** Uploader.  
**Data Touchpoints:** Compatibility fields.  
**UX Notes:** Inline help on where to find build/region.  
**NFR Hooks:** None.

---
<br>

#### SR-SAV-06 — DLC/Edition Disclosure (S)
**Traceability:** BR-SAV-02  
**Purpose:** Flag common gotchas.  
**Preconditions:** Variant with DLC/editions configured.  
**Normal Flow:** Uploader ticks relevant DLC/edition flags; they render on save page.  
**Error/Edge:** No DLC catalogued → hide control.  
**Permissions:** Uploader/Admin.  
**Data Touchpoints:** `save.dlc_flags[]`.  
**UX Notes:** Short hint “Mismatch may break loads.”  
**NFR Hooks:** Localised labels.

---
<br>

#### SR-SAV-07 — Post-Upload Metadata Edit (M)
**Traceability:** BR-SAV-01  
**Purpose:** Let uploaders fix mistakes without reuploading.  
**Preconditions:** Authenticated as uploader or Admin.  
**Normal Flow:** Edit non-file fields (progress marker, DLC flags, build info); checksum remains.  
**Error/Edge:** Changing variant to `not_viable` → block and explain.  
**Permissions:** Uploader/Admin (Curator with suggestion).  
**Data Touchpoints:** Save row.  
**UX Notes:** Audit “edited by …”.  
**NFR Hooks:** Optimistic locking.

---
<br>

#### SR-SAV-08 — Build Awareness (Steam) (M)
**Traceability:** BR-SAV-03  
**Purpose:** Tie saves to the **game build** they came from.  
**Preconditions:** Steam variant.  
**Normal Flow:** Store **build/manifest** at upload; show on save page and in tooltips.  
**Error/Edge:** Build unknown → `untested`.  
**Permissions:** Uploader/Admin.  
**Data Touchpoints:** `save.build_manifest_id`.  
**UX Notes:** Copy explains “why build matters.”  
**NFR Hooks:** Validated format.

---
<br>

#### SR-SAV-09 — Build Change Watcher (S)
**Traceability:** BR-SAV-03, BR-GOV-03  
**Purpose:** Notice **new builds** and prompt review.  
**Preconditions:** Public saves exist for a Steam AppID.  
**Normal Flow:** Background job polls build info; on change, open **Compatibility Review** task (list affected saves).  
**Error/Edge:** API outage → backoff, mark stale.  
**Permissions:** System; Admin/Curator act on tasks.  
**Data Touchpoints:** Review queue; `tested_on_builds[]`.  
**UX Notes:** Dashboard card for maintainers.  
**NFR Hooks:** Rate-limited polling.

---
<br>

#### SR-SAV-10 — Compatibility Status Model (M)
**Traceability:** BR-SAV-03  
**Purpose:** Present a **single clarity signal** per save.  
**Preconditions:** Save exists.  
**Normal Flow:** Status ∈ {`confirmed`,`untested`,`legacy`,`incompatible`}.  
- `confirmed`: ≥N “worked” confirmations for current build/region;  
- `untested`: no signal for current build/region;  
- `legacy`: uploaded on **older build** and not verified on current;  
- `incompatible`: credible reports show failure on current build/region.  
**Error/Edge:** Conflicting signals → remain `untested` until threshold.  
**Permissions:** Computed; Admin can override (SR-SAV-12).  
**Data Touchpoints:** Save status, build fields, feedback aggregates.  
**UX Notes:** Colored chips; hover explains reasoning.  
**NFR Hooks:** Thresholds configurable.

---
<br>

#### SR-SAV-11 — Community Compatibility Signals (M)
**Traceability:** BR-SAV-04  
**Purpose:** Crowd-verify without heavy moderation.  
**Preconditions:** Authenticated; user downloaded the save.  
**Normal Flow:** User submits **“Worked for me / Didn’t load”** + optional note; system stores response **scoped to their current build/region**.  
**Error/Edge:** Obvious spam (burst duplicates) → throttle.  
**Permissions:** Regular/Curator/Admin.  
**Data Touchpoints:** `SaveFeedback{save_id, build/region, verdict}`.  
**UX Notes:** Tiny inline widget; shows counts per build.  
**NFR Hooks:** One feedback per user/build; rate limits.

---
<br>

#### SR-SAV-12 — Admin/Curator Overrides (M)
**Traceability:** BR-GOV-02..03  
**Purpose:** Resolve ambiguity quickly.  
**Preconditions:** Evidence present (feedback, tests).  
**Normal Flow:** Admin sets `incompatible_since_build`, flips status to `legacy/incompatible`, or corrects build/region metadata; Curators can **propose** same.  
**Error/Edge:** Reversal → logged with reason.  
**Permissions:** Admin (set), Curator (suggest).  
**Data Touchpoints:** Save fields; change log.  
**UX Notes:** Banner shows manual override with timestamp.  
**NFR Hooks:** Audit retained.

---
<br>

#### SR-SAV-13 — Download-Time Warnings (M)
**Traceability:** BR-SAV-03, BR-UX-01  
**Purpose:** Prevent avoidable misloads.  
**Preconditions:** User clicks Download.  
**Normal Flow:** Compare user’s selected build/region vs save facets → show warning if mismatched (“Save for Build 102345; you’re on 102982”). Retro: warn on **region/emulator** mismatch.  
**Error/Edge:** Cannot detect user build → generic caution.  
**Permissions:** Public/Authenticated.  
**Data Touchpoints:** None beyond read.  
**UX Notes:** Clear, non-blocking modal; link to help.  
**NFR Hooks:** Fast check (local data).

---
<br>

#### SR-SAV-14 — Legacy Auto-Archiving (S)
**Traceability:** BR-SAV-06  
**Purpose:** Keep lists fresh without deleting history.  
**Preconditions:** Save `legacy` and inactive ≥12 months.  
**Normal Flow:** Move to **Archive**; exclude from default lists; keep searchable via filters; email uploader with restore option.  
**Error/Edge:** Recent interaction → skip this cycle.  
**Permissions:** System; uploader/Admin can restore.  
**Data Touchpoints:** `save.is_archived`.  
**UX Notes:** “Archived” badge and filter pill.  
**NFR Hooks:** Nightly job; idempotent.

---
<br>

#### SR-SAV-15 — Faceted Search (M)
**Traceability:** BR-DIS-01..03  
**Purpose:** Help users **find a compatible save fast**.  
**Preconditions:** Index available.  
**Normal Flow:** Filters: game, variant, **Steam build (exact/range)**, **status**, DLC flags, uploader, date, popularity.  
**Error/Edge:** No results → show tips (loosen build/status).  
**Permissions:** Public.  
**Data Touchpoints:** Search index pulling from Save fields.  
**UX Notes:** Sticky filters; keyboard search.  
**NFR Hooks:** Target <~1.5s typical.

---
<br>

#### SR-SAV-16 — Compatibility Badges in Lists (M)
**Traceability:** BR-UX-01  
**Purpose:** Convey compatibility at a glance.  
**Preconditions:** Listing saves.  
**Normal Flow:** Show compact badges: `Build 102345`, `Legacy`, `Retro PAL`, `Incompatible`.  
**Error/Edge:** Unknown build → show `Untested`.  
**Permissions:** Public.  
**Data Touchpoints:** Save fields.  
**UX Notes:** Screen-reader labels for each badge.  
**NFR Hooks:** Lightweight rendering.

---
<br>

#### SR-SAV-17 — Reporting Workflow (M)
**Traceability:** BR-GOV-02  
**Purpose:** Let the community flag problems.  
**Preconditions:** Authenticated.  
**Normal Flow:** Report categories: corruption, malware suspicion, mislabelled build/region, ToS concerns. Item gets a **caution banner** and enters moderation queue.  
**Error/Edge:** Duplicate report → collapse into one thread.  
**Permissions:** Any user (report); Admin (action).  
**Data Touchpoints:** `Reports{...}` queue; save status.  
**UX Notes:** Short reason + optional evidence link.  
**NFR Hooks:** Rate limits; audit actions.

---
<br>

#### SR-SAV-18 — Mislabel Protection Prompt (S)
**Traceability:** BR-SAV-04  
**Purpose:** Nudge corrections before damage spreads.  
**Preconditions:** Multiple “didn’t load” on current build; pattern indicates mislabel.  
**Normal Flow:** System prompts uploader/curators to review build/region; offers quick edit/suggest flow.  
**Error/Edge:** Uploader inactive → route to Admin queue.  
**Permissions:** Uploader/Curator/Admin.  
**Data Touchpoints:** Save fields; suggestions.  
**UX Notes:** Polite, non-punitive copy.  
**NFR Hooks:** Thresholds configurable.

---
<br>

#### SR-SAV-19 — Retention Rules (M)
**Traceability:** BR-SAV-06  
**Purpose:** Manage storage responsibly.  
**Preconditions:** Inactivity periods reached.  
**Normal Flow:** Notify owner before archival; after longer dormancy, consider deletion of **quarantined** or clearly **malicious** saves only (never auto-delete healthy public saves).  
**Error/Edge:** Owner disables notifications → in-app only.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Retention flags; notification log.  
**UX Notes:** Clear timelines and recovery steps.  
**NFR Hooks:** Batch jobs, re-entrant.

---
<br>

#### SR-SAV-20 — Minimal Personal Data (M)
**Traceability:** BR-PRO-05, NFR-SEC-05  
**Purpose:** Keep save pages privacy-lean.  
**Preconditions:** Public view.  
**Normal Flow:** Show uploader **nickname/avatar** only; no emails or external IDs.  
**Error/Edge:** Suspended user → hide avatar, keep nickname.  
**Permissions:** System.  
**Data Touchpoints:** Read-only profile basics.  
**UX Notes:** Respect profile visibility settings.  
**NFR Hooks:** None.

---
<br>

#### SR-SAV-21 — No Automatic Save Conversion (M)
**Traceability:** BR-SAV-08  
**Purpose:** Avoid risky transformations we can’t guarantee.  
**Preconditions:** N/A.  
**Normal Flow:** The platform **does not** convert save formats between builds/emulators/platforms.  
**Error/Edge:** Requests to convert → link to docs explaining why.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Clear statement on save pages.  
**NFR Hooks:** None.

---
<br>

#### SR-SAV-22 — No Write-Back to Games (M)
**Traceability:** BR-SAV-08, NFR-SEC-01  
**Purpose:** Stay within ToS and user safety.  
**Preconditions:** N/A.  
**Normal Flow:** Only **downloads**; never modifies game folders or Steam Cloud.  
**Error/Edge:** Attempts to “auto-install” → blocked with explanation.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Help link: “How to place this save.”  
**NFR Hooks:** None.

---
<br>

#### SR-SAV-23 — Heuristics, Not Guarantees (M)
**Traceability:** BR-SAV-03  
**Purpose:** Set the right expectation.  
**Preconditions:** Displaying compatibility.  
**Normal Flow:** UI copy clarifies that compatibility is best-effort based on facets + community signals; success not guaranteed.  
**Error/Edge:** None.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Gentle, non-legalese copy.  
**NFR Hooks:** Localised.

---
<br>

#### SR-SAV-24 — File Scanning & Quarantine (M)
**Traceability:** BR-SAV-01, NFR-SEC-05  
**Purpose:** Keep users safe from malicious files.  
**Preconditions:** Upload started.  
**Normal Flow:** AV scan each file; if suspicious → **quarantine**; Admin reviews or auto-purges after window.  
**Error/Edge:** Scanner outage → block uploads with apology banner.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Scan logs; quarantine store.  
**UX Notes:** Minimal detail to avoid aiding attackers.  
**NFR Hooks:** Time-boxed quarantine; alerts on spikes.

---
<br>

#### SR-SAV-25 — Visibility Controls & Sharing (M)
**Traceability:** BR-PRO-05  
**Purpose:** Give owners precise control.  
**Preconditions:** Save exists.  
**Normal Flow:** Owner sets **Private / Public / Link-only**; can change anytime unless frozen by moderation.  
**Error/Edge:** Publicising unverified email → blocked by SR-ACC-30.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** `save.visibility`.  
**UX Notes:** Short tooltip on each mode.  
**NFR Hooks:** API enforces visibility on all reads.

---
<br>

#### SR-SAV-26 — Download Audit Crumbs (S)
**Traceability:** BR-GOV-03  
**Purpose:** Diagnose issues without tracking users.  
**Preconditions:** Download event.  
**Normal Flow:** Record anonymised event (save_id, timestamp, rough client type).  
**Error/Edge:** None.  
**Permissions:** System; Admin read.  
**Data Touchpoints:** Lightweight analytics table.  
**UX Notes:** None.  
**NFR Hooks:** No PII; retention window.

---
<br>

#### SR-SAV-27 — Rate Limits & Abuse Controls (S)
**Traceability:** NFR-SEC-01..03  
**Purpose:** Keep the service stable.  
**Preconditions:** N/A.  
**Normal Flow:** Apply sane per-user/IP limits on upload/download/report endpoints.  
**Error/Edge:** Exceeded → “Please try again later.”  
**Permissions:** System.  
**Data Touchpoints:** Throttle counters.  
**UX Notes:** Generic error messages.  
**NFR Hooks:** Burst detection; WAF rules.

---
<br>

#### SR-SAV-28 — Help Docs & How-To Placement (S)
**Traceability:** BR-UX-01  
**Purpose:** Reduce support load and failed attempts.  
**Preconditions:** N/A.  
**Normal Flow:** Contextual links (“Where to find your build”, “How to place a save”) near forms and warnings.  
**Error/Edge:** Docs unavailable → fallback copy inline.  
**Permissions:** Public.  
**Data Touchpoints:** None.  
**UX Notes:** UA/EN versions.  
**NFR Hooks:** Static, cached.

---
<br>
<br>


### 4.4 Achievement Tracking

> **Traceability:** BR-ACH-01..14, BR-INT-01..02, BR-GOV-02..04, BR-PRO-03..05, BR-DIS-01..03; NFR-SEC-01..05, NFR-UX-01..05, NFR-LOC-01..02, NFR-PERF-01..02  
> **Module prefix:** `SR-ACH-*`  
> **Scope:** Official (Steam) achievements **read-only**; RetroAchievements (RA) **read-only**; **fan-made** sets hosted on Dream Project with criteria, hints, evidence links, comments, ratings, and stats. No competitions or speedrun ladders.

---
<br>

#### SR-ACH-01 — Core Entities (M)
**Traceability:** BR-ACH-01  
**Purpose:** Provide a consistent data backbone for official/retro/fan progress.  
**Preconditions:** Game + Variant exist.  
**Normal Flow:** Persist:  
- `AchSet{set_id, game_id, variant_scope (game|variant), type (official|retro|fan), title, description, version, status (draft|published|archived), owner_curator_id?, created_at, updated_at}`  
- `AchItem{item_id, set_id, key, title, criteria_md, flags{storyline, irreversible, final}, media_refs[], order}`  
- `AchProgress{user_id, set_id, item_id?, state (not_started|in_progress|completed), completed_at?, evidence{save_id?, screenshot_id?, links[]}, visibility (private|public|link)}`  

**Error/Edge:** Duplicate keys/ordering → reject.  
**Permissions:** Curator/Admin (define sets/items); users (progress).  
**Data Touchpoints:** Tables above.  
**UX Notes:** Each **item** and **set** has its own page.  
**NFR Hooks:** Index on `{game_id,set_id}`, `{user_id,set_id}`.

---
<br>

#### SR-ACH-02 — Official (Steam) Sync (M)
**Traceability:** BR-ACH-01, BR-INT-01  
**Purpose:** Reflect official achievements for linked Steam users (read-only).  
**Preconditions:** User linked Steam (SR-ACC-08); variant has `steam_app_id`.  
**Normal Flow:** Background sync fetches user/game achievement states; populate/refresh `AchSet(type=official)` and `AchProgress` (completed items + timestamps when available).  
**Error/Edge:** Private Steam data or API outage → show “last synced … / unavailable”; no blocking.  
**Permissions:** System; user may unlink (SR-ACC-10).  
**Data Touchpoints:** AchSet/Item for official; AchProgress per user.  
**UX Notes:** “Official (Steam) • read-only” badge; no manual edits.  
**NFR Hooks:** Rate-limited sync; staleness chip after threshold.

---
<br>

#### SR-ACH-03 — RetroAchievements Sync (M)
**Traceability:** BR-ACH-02..03, BR-INT-02  
**Purpose:** Mirror RA sets and progress for linked RA users (read-only).  
**Preconditions:** User linked RA (SR-ACC-09); variant has `ra_game_id`.  
**Normal Flow:** Import RA set structure and item states; update `AchSet(type=retro)` and `AchProgress`.  
**Error/Edge:** RA API down → mark stale; keep last known.  
**Permissions:** System; user may unlink.  
**Data Touchpoints:** AchSet/Item (retro), AchProgress.  
**UX Notes:** “Retro (RA) • read-only” badge; deep link to RA page.  
**NFR Hooks:** Respect RA rate limits; backoff on errors.

---
<br>

#### SR-ACH-04 — Achievement Panels on Game/Variant Pages (M)
**Traceability:** BR-ACH-01..04, BR-DIS-01  
**Purpose:** Show all relevant sets in one place per game/variant.  
**Preconditions:** Game/Variant loaded.  
**Normal Flow:** Render chips/sections for **Official (Steam)**, **Retro (RA)**, and **Fan-made (N)**; clicking opens the set page.  
**Error/Edge:** No sets → show empty state with CTA “Browse fan-made sets”.  
**Permissions:** Public (view).  
**Data Touchpoints:** Read AchSet counts.  
**UX Notes:** Soft CTA to link Steam/RA if unlinked.  
**NFR Hooks:** Cached counts.

---
<br>

#### SR-ACH-05 — Create Fan-Made Set (M)
**Traceability:** BR-ACH-04..06  
**Purpose:** Let Curators define high-quality sets for any game (and optional variant).  
**Preconditions:** Curator role.  
**Normal Flow:** Curator enters title/description/scope, creates **draft** set; adds items (§SR-ACH-08); publish when ready.  
**Error/Edge:** Missing minimum items → block publish; plagiarism reports route to moderation.  
**Permissions:** Curator (create/publish own); Admin (edit/force unpublish).  
**Data Touchpoints:** AchSet.  
**UX Notes:** “Draft/Published” status chip; preview mode.  
**NFR Hooks:** Audit edits.

---
<br>

#### SR-ACH-06 — Set Versioning (M)
**Traceability:** BR-ACH-06  
**Purpose:** Allow iterative improvements without breaking existing runs.  
**Preconditions:** Published set exists.  
**Normal Flow:** Curator creates **new version**; users joining after publish are pinned to latest; existing participants stay on their version with optional **migrate** prompt if safe.  
**Error/Edge:** Breaking changes (remove/retitle final item) → require Admin approval.  
**Permissions:** Curator (propose); Admin (approve breaking).  
**Data Touchpoints:** `AchSet.version`, migration map.  
**UX Notes:** Version label on set page; changelog tab.  
**NFR Hooks:** Immutable historical items per version.

---
<br>

#### SR-ACH-07 — Set Visibility & Publishing Rules (M)
**Traceability:** BR-ACH-06, BR-GOV-02  
**Purpose:** Ensure only reviewable, coherent sets go public.  
**Preconditions:** Draft set with items.  
**Normal Flow:** Publish when: ≥1 item, titles unique, flags set, criteria present; optional curator co-owners.  
**Error/Edge:** Reports or low rating → can be **soft-hidden** pending review (SR-ACH-19).  
**Permissions:** Curator publish own; Admin override.  
**Data Touchpoints:** `AchSet.status`.  
**UX Notes:** “Under review” banner if soft-hidden.  
**NFR Hooks:** Audit state changes.

---
<br>

#### SR-ACH-08 — Achievement Item Definition (M)
**Traceability:** BR-ACH-05  
**Purpose:** Capture clear, reusable criteria per item.  
**Preconditions:** Draft set.  
**Normal Flow:** For each item: title, **criteria (markdown)**, optional tags/flags: `storyline`, `irreversible`, `final_completion_indicator`; ordered list.  
**Error/Edge:** Empty criteria → block save; duplicate title within set → reject.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** AchItem.  
**UX Notes:** Short helper text; spoiler-aware rendering (SR-ACH-17/30).  
**NFR Hooks:** Markdown sanitised.

---
<br>

#### SR-ACH-09 — Evidence Types for Fan Progress (M)
**Traceability:** BR-ACH-07..08  
**Purpose:** Let users substantiate completion without heavy tooling.  
**Preconditions:** User joined a fan-made set.  
**Normal Flow:** On completing an item, user may attach **(a)** save reference (`save_id`), **(b)** screenshot (goes through same AV/AI pipeline as albums), **(c)** external link (YouTube, GameFAQs, etc.).  
**Error/Edge:** Disallowed domain → reject; file scan fails → reject.  
**Permissions:** Participant user (add/remove own evidence); Admin/Curator (review).  
**Data Touchpoints:** `AchProgress.evidence`.  
**UX Notes:** Explain that long clips are **links only**; evidence **visibility** selectable.  
**NFR Hooks:** Link whitelist/validation; AV scan for uploads.

---
<br>

#### SR-ACH-10 — Join Fan-Made Set (M)
**Traceability:** BR-ACH-04  
**Purpose:** Start tracking a user’s run against a set.  
**Preconditions:** Authenticated; set published.  
**Normal Flow:** User clicks **Join** → create `AchProgress` rows lazily on first item interaction; show progress bar on set page and profile.  
**Error/Edge:** Attempt to join archived set → block with message.  
**Permissions:** Regular/Curator/Admin.  
**Data Touchpoints:** AchProgress for `set_id`.  
**UX Notes:** Non-intrusive confirmation; option to leave set.  
**NFR Hooks:** Idempotent join.

---
<br>

#### SR-ACH-11 — Manual Progress Marking (Fan Sets) (M)
**Traceability:** BR-ACH-07  
**Purpose:** Record completion where no official/retro feed exists.  
**Preconditions:** Joined set.  
**Normal Flow:** User marks item **Completed**; optionally add evidence (SR-ACH-09).  
**Error/Edge:** Rapid toggling → throttle; conflicting moderator decision (set to “rejected”) → show banner.  
**Permissions:** Participant (own progress); Admin/Curator (moderation).  
**Data Touchpoints:** AchProgress per item.  
**UX Notes:** Clear states (Not started / In progress / Completed).  
**NFR Hooks:** Audit toggles.

---
<br>

#### SR-ACH-12 — AI-Assisted Evidence Triage (S)
**Traceability:** BR-GOV-03, NFR-SEC-05  
**Purpose:** Reduce manual review load.  
**Preconditions:** Evidence submitted.  
**Normal Flow:** Lightweight model flags **NSFW/off-topic** screenshots and suspicious links → queue for curator/admin review.  
**Error/Edge:** Model unavailable → skip gracefully; rely on reports.  
**Permissions:** System; Admin/Curator review.  
**Data Touchpoints:** Moderation queue entries.  
**UX Notes:** No “AI verdict” shown to users, only moderation result.  
**NFR Hooks:** False-positive rate monitored.

---
<br>

#### SR-ACH-13 — Moderation & Appeals (Fan Sets) (M)
**Traceability:** BR-GOV-02..04  
**Purpose:** Keep sets and evidence trustworthy.  
**Preconditions:** Report or AI flag exists.  
**Normal Flow:** Admin can **hide/remove** items/evidence, warn or restrict users; Curators can **recommend** actions. Content owners can **appeal** within 7 days.  
**Error/Edge:** Repeated abuse → temporary posting suspension.  
**Permissions:** Admin (action), Curator (recommend), User (appeal).  
**Data Touchpoints:** Moderation actions log; content status.  
**UX Notes:** Short rationale visible to owner.  
**NFR Hooks:** Audit trail preserved.

---
<br>

#### SR-ACH-14 — Set Ratings (M)
**Traceability:** BR-ACH-09  
**Purpose:** Surface quality fan-made sets.  
**Preconditions:** Published set.  
**Normal Flow:** Users rate set (e.g., 1–5) once; aggregate score displayed. Low score + reports → auto-queue for review.  
**Error/Edge:** Spam bursts → rate-limit, collapse dupes.  
**Permissions:** Any authenticated user.  
**Data Touchpoints:** `AchSetRating{user_id,set_id,score}`; aggregate fields on AchSet.  
**UX Notes:** Show count + average; no brigading incentives.  
**NFR Hooks:** One vote per user; abuse detection.

---
<br>

#### SR-ACH-15 — Completion Statistics (Set & Item) (M)
**Traceability:** BR-ACH-10..11  
**Purpose:** Provide transparent progress metrics.  
**Preconditions:** Progress data exists.  
**Normal Flow:** For each item and set: show **% of participants completed** and a **list of users** (nickname, avatar, timestamp) who finished; obey visibility settings.  
**Error/Edge:** Profile private → hide user from public list.  
**Permissions:** Public view (respecting visibility).  
**Data Touchpoints:** Aggregates over AchProgress.  
**UX Notes:** Paginated “Who completed” list.  
**NFR Hooks:** Nightly rollups for performance.

---
<br>

#### SR-ACH-16 — Time-to-Complete (Set) (M)
**Traceability:** BR-ACH-11  
**Purpose:** Give a sense of effort.  
**Preconditions:** Participant has ≥1 completed item.  
**Normal Flow:** Compute **elapsed time** from first completion (or join) to **final** item completion; display per user and median for the set.  
**Error/Edge:** Gaps > 6 months → show both elapsed and **active time** estimate (count of completion days).  
**Permissions:** Public view (respecting visibility).  
**Data Touchpoints:** AchProgress timestamps.  
**UX Notes:** Tooltip explains calculation.  
**NFR Hooks:** Batch computation.

---
<br>

#### SR-ACH-17 — Dedicated Pages (Set & Item) (M)
**Traceability:** BR-ACH-05..11  
**Purpose:** Make every set and item addressable and discussable.  
**Preconditions:** Set/item exists.  
**Normal Flow:**  
- **Set page**: description, version, items list, rating, stats, comments, join/leave.  
- **Item page**: criteria, flags, media, comments, evidence snippets (respecting visibility).  
**Error/Edge:** Hidden/archived → show banner or 404 if removed.  
**Permissions:** Public view; posting per role.  
**Data Touchpoints:** AchSet/Item/Progress/Comments.  
**UX Notes:** Spoiler-aware rendering (hide criteria text flagged as spoiler until revealed).  
**NFR Hooks:** Cached; ETag.

---
<br>

#### SR-ACH-18 — Comments & Threads (Set & Item) (M)
**Traceability:** BR-ACH-08, BR-GOV-02  
**Purpose:** Host focused discussions and tips.  
**Preconditions:** Authenticated for posting.  
**Normal Flow:** Threaded comments with basic formatting; users can link **GameFAQs/YouTube** guides; report abusive posts; Admin can hide.  
**Error/Edge:** Spam/rate-limit kicks in; link checks fail → reject.  
**Permissions:** Any authenticated user (post), Admin/Curator (moderate).  
**Data Touchpoints:** `Comments{target_type(target_id), user_id, body, created_at, status}`.  
**UX Notes:** Collapse long threads; spoiler tags supported.  
**NFR Hooks:** Rate limits; link whitelist.

---
<br>

#### SR-ACH-19 — Themed Subsets & Multiple Active Sets (M)
**Traceability:** BR-ACH-06  
**Purpose:** Support parallel creative approaches (e.g., “Low-Level”, “No HUD”).  
**Preconditions:** Game has at least one fan-made set.  
**Normal Flow:** Curators mark set as **subset of** base set (or themed standalone); multiple sets may be **simultaneously active** per game.  
**Error/Edge:** Circular subset references → reject.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** `AchSet.parent_set_id?`.  
**UX Notes:** Show subset badge and link back to base.  
**NFR Hooks:** None.

---
<br>

#### SR-ACH-20 — Cross-Variant Scope (S)
**Traceability:** BR-ACH-04  
**Purpose:** Let curators choose whether a set applies to all variants or a specific one.  
**Preconditions:** Set draft.  
**Normal Flow:** `variant_scope=game` (all variants) or choose specific `variant_id`.  
**Error/Edge:** Variant later hidden → show banner; keep set visible at game level.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** AchSet.variant_scope / variant mapping.  
**UX Notes:** Scope label near title.  
**NFR Hooks:** Search facets respect scope.

---
<br>

#### SR-ACH-21 — Official/Retro Mapping to Our Model (S)
**Traceability:** BR-ACH-01..03  
**Purpose:** Normalise external data for consistent UI.  
**Preconditions:** Sync runs.  
**Normal Flow:** Map Steam/RA structures into AchSet/Item equivalents with `type=official|retro`; preserve external IDs for deep links.  
**Error/Edge:** Schema drift → log and skip affected items until adapter update.  
**Permissions:** System/Admin.  
**Data Touchpoints:** AchSet/Item fields incl. `ext_ref`.  
**UX Notes:** External icon + link on pages.  
**NFR Hooks:** Adapter versioning.

---
<br>

#### SR-ACH-22 — Progress Visibility Controls (M)
**Traceability:** BR-PRO-05  
**Purpose:** Respect user privacy for fan-made progress.  
**Preconditions:** Participant exists.  
**Normal Flow:** User sets visibility for **their** progress (Private / Public / Link-only) per set; default from profile.  
**Error/Edge:** Official/RA progress is governed by external privacy → reflect as read-only.  
**Permissions:** Owner; Admin may hide for safety.  
**Data Touchpoints:** `AchProgress.visibility`.  
**UX Notes:** Clear tooltips; inherits profile defaults.  
**NFR Hooks:** API enforces on all reads.

---
<br>

#### SR-ACH-23 — Search & Browse Fan-Made Sets (S)
**Traceability:** BR-DIS-01..03  
**Purpose:** Help users discover good sets quickly.  
**Preconditions:** Index built.  
**Normal Flow:** Facets: game, platform/variant, rating, recency, curator, tags/flags (storyline/irreversible/final).  
**Error/Edge:** No results → suggest popular sets.  
**Permissions:** Public.  
**Data Touchpoints:** Index of AchSet metadata.  
**UX Notes:** Badges for rating and type.  
**NFR Hooks:** <~1.5s typical.

---
<br>

#### SR-ACH-24 — Duplicate & Plagiarism Safeguards (S)
**Traceability:** BR-GOV-02  
**Purpose:** Keep catalogue clean and fair.  
**Preconditions:** New set draft.  
**Normal Flow:** Dedup heuristic on title/items; warn if near-duplicate of existing set; curator must add differentiation note or link to subset.  
**Error/Edge:** Abuse → moderation.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** Similarity index.  
**UX Notes:** Guidance message, not a hard block.  
**NFR Hooks:** Background similarity job.

---
<br>

#### SR-ACH-25 — Flags: Storyline / Irreversible / Final (M)
**Traceability:** BR-ACH-05  
**Purpose:** Communicate risk and completion semantics.  
**Preconditions:** Item edit.  
**Normal Flow:** Curator marks flags; UI shows badges; **final** defines set completion.  
**Error/Edge:** No item flagged as final → set uses “all items” rule (explicit in UI).  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** `AchItem.flags`.  
**UX Notes:** Warning for irreversible items.  
**NFR Hooks:** Localised labels.

---
<br>

#### SR-ACH-26 — Guide Links & Tips (M)
**Traceability:** BR-ACH-08  
**Purpose:** Provide optional help without hosting content.  
**Preconditions:** Item or set exists.  
**Normal Flow:** Curators add short tips and **links** to reputable guides (GameFAQs, YouTube, wiki).  
**Error/Edge:** Link fails validation → reject.  
**Permissions:** Curator/Admin (tips); users can comment links in threads.  
**Data Touchpoints:** `AchItem.media_refs[]` / tips.  
**UX Notes:** “External guide” icon + domain.  
**NFR Hooks:** Link whitelist.

---
<br>

#### SR-ACH-27 — Integration Failure Modes (M)
**Traceability:** BR-INT-01..02  
**Purpose:** Degrade gracefully when Steam/RA are unavailable.  
**Preconditions:** External outage.  
**Normal Flow:** Official/retro panels show “Temporarily unavailable / last synced …”; fan-made sets operate normally.  
**Error/Edge:** Prolonged outage → hide staleness timer after max.  
**Permissions:** System.  
**Data Touchpoints:** `last_synced_at/status`.  
**UX Notes:** Non-blocking banners only.  
**NFR Hooks:** Circuit breakers, backoff.

---
<br>

#### SR-ACH-28 — Data Retention & Deletion (M)
**Traceability:** NFR-LAW-02, BR-GOV-04  
**Purpose:** Respect user control and legal basics.  
**Preconditions:** Account deletion or request.  
**Normal Flow:** On account deletion, anonymise user in AchProgress and Comments; preserve aggregate stats. Evidence files/links owned by user are removed where feasible.  
**Error/Edge:** Evidence referenced by moderation record → retain minimal copy per policy.  
**Permissions:** System/Admin.  
**Data Touchpoints:** AchProgress, Comments, evidence refs.  
**UX Notes:** Explain what remains in aggregates.  
**NFR Hooks:** Batch job; audit.

---
<br>

#### SR-ACH-29 — Profile Timeline Signals (S)
**Traceability:** BR-PRO-03  
**Purpose:** Reflect meaningful achievement activity on profiles.  
**Preconditions:** Progress change.  
**Normal Flow:** Add lightweight timeline entries: “Completed **[Set]**”, “Finished **[Item]**”, with visibility respected.  
**Error/Edge:** Private progress → no public entry.  
**Permissions:** System.  
**Data Touchpoints:** Profile activity log.  
**UX Notes:** Link back to set/item pages.  
**NFR Hooks:** Pruned after retention window.

---
<br>

#### SR-ACH-30 — Spoiler Handling (M)
**Traceability:** BR-ACH-05, NFR-UX-05  
**Purpose:** Avoid accidental spoilers in criteria and comments.  
**Preconditions:** Rendering criteria/comments.  
**Normal Flow:** Curators can mark text segments as **spoiler**; UI blurs/hides until user reveals.  
**Error/Edge:** Missing marks → allow users to self-tag in comments.  
**Permissions:** Curator/Admin (authoritative); users (comments).  
**Data Touchpoints:** Marked ranges/flag.  
**UX Notes:** Keyboard- and screen-reader-friendly reveal.  
**NFR Hooks:** Sanitise markup.

---
<br>

#### SR-ACH-31 — API & Rate Limits (S)
**Traceability:** NFR-PERF-01..02, NFR-SEC-03  
**Purpose:** Keep the system responsive and fair.  
**Preconditions:** Public/API access.  
**Normal Flow:** Read endpoints for sets/items/progress; write endpoints for fan progress/comments with per-user throttles.  
**Error/Edge:** Limit exceeded → generic “try later”.  
**Permissions:** As per role; public reads.  
**Data Touchpoints:** Counters, logs.  
**UX Notes:** Avoid exposing exact limits.  
**NFR Hooks:** Caching on read endpoints.

---
<br>

#### SR-ACH-32 — Non-Goals & ToS Safety (M)
**Traceability:** BR-ACH-12..14, NFR-SEC-01  
**Purpose:** Make boundaries explicit.  
**Preconditions:** N/A.  
**Normal Flow:** No overlays, memory scanners, or automation that could trigger anti-cheat or violate ToS. No write-back to Steam/RA.  
**Error/Edge:** User requests automation → explain policy and alternatives (manual evidence, RA integration).  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Clear, friendly policy note.  
**NFR Hooks:** None.

---
<br>
<br>

### 4.5 Sticker Albums

> **Traceability:** BR-ALB-01..08, BR-PRO-03..05, BR-GOV-02..04, BR-DIS-01..03; NFR-SEC-01..05, NFR-UX-01..05, NFR-LOC-01..02, NFR-PERF-01..02  
> **Module prefix:** `SR-ALB-*`  
> **Scope:** Curator-built **album templates** with spoiler-aware **slots**; user **album instances** filled by **screenshots**. Light **AI/rule checks**, human moderation, completion **badges**, and **statistics**. No leaderboards or competitions.

---
<br>

#### SR-ALB-01 — Core Entities (M)
**Purpose:** Establish a consistent schema for templates, slots, instances, and fills.  
**Preconditions:** Game + Variant exist.  
**Normal Flow:** Persist:
- `AlbumTemplate{template_id, game_id, variant_scope (game|variant), title, description_md, type (new_player|full_discovery|themed), status (draft|published|archived), owner_curator_id?, version, created_at, updated_at}`
- `AlbumSlot{slot_id, template_id, title, hint_md, spoiler_level (none|light|heavy), required (bool), order, validation_mode (ai|review|ai_then_review), allowed_formats (image), frame/aspect}`
- `AlbumInstance{instance_id, template_id, user_id, visibility (private|public|link), progress_state (0..1), completed_at?}`
- `SlotFill{fill_id, instance_id, slot_id, screenshot_id, state (pending|accepted|rejected), reviewer_id?, reviewed_at?, notes}`

**Error/Edge:** Duplicate slot order or missing required fields → reject.  
**Permissions:** Curator/Admin (templates/slots); Users (instances/fills).  
**Data Touchpoints:** Tables above + object storage for screenshots.  
**UX Notes:** Each **template** and **instance** has its own page; slots render as cards.  
**NFR Hooks:** Indices on `{template_id, order}`, `{instance_id, slot_id}`.

---
<br>

#### SR-ALB-02 — Create & Edit Album Template (M)
**Purpose:** Enable Curators to define high-quality albums per game/variant.  
**Preconditions:** Curator role.  
**Normal Flow:** Curator creates **draft** template, sets type, scope, description; adds slots (SR-ALB-03); previews; publishes (SR-ALB-04).  
**Error/Edge:** Game/variant missing → block with guidance.  
**Permissions:** Curator (own), Admin (any).  
**Data Touchpoints:** `AlbumTemplate`.  
**UX Notes:** Live preview, spoiler toggles, slot count indicator.  
**NFR Hooks:** Audit edits; optimistic locking.

---
<br>

#### SR-ALB-03 — Define Slots (M)
**Purpose:** Capture what each screenshot should depict without spoiling.  
**Preconditions:** Draft template.  
**Normal Flow:** For each slot set `title`, `hint_md` (non-spoiler first; optional expanded hint), `spoiler_level`, `required`, `order`, `frame/aspect`, `validation_mode`.  
**Error/Edge:** Empty title/hint → block; overlapping orders → reject.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** `AlbumSlot`.  
**UX Notes:** Hint editor with spoiler tags; show example frame/aspect overlay.  
**NFR Hooks:** Sanitise markdown.

---
<br>

#### SR-ALB-04 — Publish / Archive Template (M)
**Purpose:** Gate visibility to reviewed, coherent albums.  
**Preconditions:** Template has ≥1 slot; description and type set.  
**Normal Flow:** Curator publishes; template becomes visible in game/variant page; Admin can archive.  
**Error/Edge:** Reports/low quality → soft-hide pending review.  
**Permissions:** Curator publish own; Admin override/archive.  
**Data Touchpoints:** `AlbumTemplate.status`.  
**UX Notes:** “Draft/Published/Archived” chip; changelog tab.  
**NFR Hooks:** Audit state transitions.

---
<br>

#### SR-ALB-05 — Template Versioning (M)
**Purpose:** Evolve templates without breaking user progress.  
**Preconditions:** Published template.  
**Normal Flow:** Curator creates **new version**; new instances bind to latest; existing instances stay on old version with optional **migrate** prompt (if slots are only additive/non-breaking).  
**Error/Edge:** Removing or re-ordering required slots → needs Admin approval.  
**Permissions:** Curator propose; Admin approve breaking.  
**Data Touchpoints:** `AlbumTemplate.version`, migration map.  
**UX Notes:** Version badge; “What changed” panel.  
**NFR Hooks:** Preserve historical slot definitions.

---
<br>

#### SR-ALB-06 — Album Types (M)
**Purpose:** Support spoiler posture and theme: `new_player`, `full_discovery`, `themed`.  
**Preconditions:** Template edit.  
**Normal Flow:** Curator selects `type`; affects default spoiler/reveal behaviour (SR-ALB-16) and discovery facets (SR-ALB-24).  
**Error/Edge:** None.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** `AlbumTemplate.type`.  
**UX Notes:** Short explanations of types in UI.  
**NFR Hooks:** Localised labels.

---
<br>

#### SR-ALB-07 — Create Album Instance (M)
**Purpose:** Let users start their own copy to fill.  
**Preconditions:** Template published; user authenticated.  
**Normal Flow:** User clicks **Start Album** → create `AlbumInstance` (visibility default=Private); render slots with states; progress bar = 0%.  
**Error/Edge:** Template archived → block with message.  
**Permissions:** Regular/Curator/Admin (as user).  
**Data Touchpoints:** `AlbumInstance`.  
**UX Notes:** Non-intrusive confirmation; visibility selector.  
**NFR Hooks:** Idempotent on duplicate clicks.

---
<br>

#### SR-ALB-08 — Upload Screenshot to Slot (M)
**Purpose:** Capture a candidate image for a slot.  
**Preconditions:** Instance exists; user owns it.  
**Normal Flow:** User uploads image → EXIF scrub + AV scan → optional auto-crop to frame → send to **AI/rule checks** (SR-ALB-10) → set `SlotFill.state=pending`.  
**Error/Edge:** Wrong type/oversize → reject; scan fail → quarantine (SR-ALB-27).  
**Permissions:** Owner; Admin can upload on behalf only for testing.  
**Data Touchpoints:** Object storage; `SlotFill`.  
**UX Notes:** Progress bar; crop tool; “pending review” chip.  
**NFR Hooks:** Size cap (e.g., 10–15 MB); CDN thumbs.

---
<br>

#### SR-ALB-09 — Replace / Remove Screenshot (M)
**Purpose:** Respect content ownership and iteration.  
**Preconditions:** SlotFill exists; owner signed in.  
**Normal Flow:** Owner can replace image or remove fill; if accepted before, replacement resets to `pending`.  
**Error/Edge:** Under active moderation → temporary lock.  
**Permissions:** Owner; Admin can remove for policy.  
**Data Touchpoints:** `SlotFill`, storage delete/replace.  
**UX Notes:** Warning that replacement resets acceptance.  
**NFR Hooks:** Safe delete with grace period.

---
<br>

#### SR-ALB-10 — AI/Rule-Based Validation (M)
**Purpose:** Triage at scale: check slot fit & policy risks.  
**Preconditions:** Upload complete.  
**Normal Flow:** Lightweight model checks **semantic match** to hint, **NSFW**, and obvious mismatches; set confidence; route:  
- `ai` → auto-accept/reject by threshold;  
- `ai_then_review` → auto-classify + queue;  
- `review` → queue only.  

**Error/Edge:** Model unavailable → fall back to `review`.  
**Permissions:** System; Admin/Curator review.  
**Data Touchpoints:** Validation results, moderation queue.  
**UX Notes:** User sees only *pending/accepted/rejected* with short reason; no AI scores exposed.  
**NFR Hooks:** Monitor FP/FN; adjustable thresholds.

---
<br>

#### SR-ALB-11 — Human Review & Moderation (M)
**Purpose:** Resolve borderline cases and enforce policy.  
**Preconditions:** Item in queue or reported.  
**Normal Flow:** Reviewer sees side-by-side **hint vs screenshot**; can **accept**, **reject** (with reason), or **hide** (policy breach).  
**Error/Edge:** Conflicting decisions → last Admin action wins; audit logged.  
**Permissions:** Admin (decide), Curator (recommend).  
**Data Touchpoints:** `SlotFill.state`, `notes`, audit log.  
**UX Notes:** Previews with zoom, frame overlay.  
**NFR Hooks:** Fast image proxy; keyboard shortcuts.

---
<br>

#### SR-ALB-12 — Slot States & Progress (M)
**Purpose:** Keep instance progress accurate.  
**Preconditions:** Instance exists.  
**Normal Flow:** `pending → accepted` increments progress; `rejected` does not. Progress = `accepted_required / required_slots`.  
**Error/Edge:** Template version update → recalc with migration rules.  
**Permissions:** System.  
**Data Touchpoints:** `AlbumInstance.progress_state`, `completed_at`.  
**UX Notes:** Progress bar; required vs optional counts.  
**NFR Hooks:** Efficient roll-up on write.

---
<br>

#### SR-ALB-13 — Completion & Badges (M)
**Purpose:** Reward finishing an album.  
**Preconditions:** All **required** slots accepted.  
**Normal Flow:** Mark instance complete; set `completed_at`; award **game-specific badge** on profile; add profile timeline entry.  
**Error/Edge:** Later slot rejection (appeal) → badge remains but instance shows “re-opened” until resolved.  
**Permissions:** System/Admin (revocation on abuse).  
**Data Touchpoints:** `AlbumInstance.completed_at`, `ProfileBadges`.  
**UX Notes:** Celebration modal; share link (if visibility allows).  
**NFR Hooks:** Idempotent awarding.

---
<br>

#### SR-ALB-14 — Completion Statistics (M)
**Purpose:** Provide transparency and social proof.  
**Preconditions:** Instances exist.  
**Normal Flow:** Show **% of users** who completed the album and a **list of users** (nickname, avatar, timestamp) respecting visibility.  
**Error/Edge:** Private instance → excluded from public list.  
**Permissions:** Public (view), System (compute).  
**Data Touchpoints:** Aggregates over `AlbumInstance`.  
**UX Notes:** Paginated “Who completed”.  
**NFR Hooks:** Nightly rollups.

---
<br>

#### SR-ALB-15 — Time-to-Complete (M)
**Purpose:** Indicate effort required.  
**Preconditions:** Completed instances.  
**Normal Flow:** Compute elapsed time from instance creation (or first accepted slot) to completion; show per user and median per template.  
**Error/Edge:** Very long gaps → show “active days” metric.  
**Permissions:** Public (view).  
**Data Touchpoints:** Instance timestamps.  
**UX Notes:** Tooltip explains calculation.  
**NFR Hooks:** Batch computation.

---
<br>

#### SR-ALB-16 — Progressive Reveal & “Next-Up” Hint (M)
**Purpose:** Help **new_player** users avoid missing moments without spoilers.  
**Preconditions:** Instance started; template `type=new_player`.  
**Normal Flow:** Before each required slot, show **non-spoiler hint** (silhouette/blur + short cue like “After meeting the ship captain”). Users can **peek** extended hint (explicit action).  
**Error/Edge:** User disables hints → hide globally for that instance.  
**Permissions:** Owner.  
**Data Touchpoints:** Instance prefs.  
**UX Notes:** Gentle, optional; never auto-reveal heavy spoilers.  
**NFR Hooks:** Localised copy; accessible blur.

---
<br>

#### SR-ALB-17 — “Upcoming Window” Reminders (S)
**Purpose:** Reduce missed captures near key moments.  
**Preconditions:** Instance active.  
**Normal Flow:** User can opt-in to lightweight reminders (email/in-app) tied to typical **story beats** (curator-authored, non-spoiler phrasing).  
**Error/Edge:** Email provider down → in-app only.  
**Permissions:** Owner (opt-in), System send.  
**Data Touchpoints:** Reminder prefs; outbox.  
**UX Notes:** Explain that timing is approximate.  
**NFR Hooks:** Rate-limit sends.

---
<br>

#### SR-ALB-18 — Visibility Controls (M)
**Purpose:** Let users decide who can see their album instances.  
**Preconditions:** Instance exists.  
**Normal Flow:** Set visibility **Private / Link-only / Public**; default from profile.  
**Error/Edge:** Under moderation freeze → lock changes.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** `AlbumInstance.visibility`.  
**UX Notes:** Clear tooltips; public share URL if allowed.  
**NFR Hooks:** Enforced on all reads.

---
<br>

#### SR-ALB-19 — Commenting (Template & Instance) (M)
**Purpose:** Enable discussion and tips without mixing with evidence.  
**Preconditions:** Authenticated for posting.  
**Normal Flow:** Threaded comments on **template** and **instance** pages; report abusive posts; Admin can hide/remove.  
**Error/Edge:** Spam → throttle; link check fails → reject.  
**Permissions:** Any authenticated user (post); Admin/Curator (moderate).  
**Data Touchpoints:** `Comments{target_type,target_id,...}`.  
**UX Notes:** Spoiler tags supported.  
**NFR Hooks:** Rate limits; link whitelist.

---
<br>

#### SR-ALB-20 — Search & Browse Templates (M)
**Purpose:** Help users discover relevant albums quickly.  
**Preconditions:** Index ready.  
**Normal Flow:** Facets: game, platform/variant, **type** (new_player/full_discovery/themed), slot count, recency, curator, popularity.  
**Error/Edge:** No results → show popular templates.  
**Permissions:** Public.  
**Data Touchpoints:** Index of `AlbumTemplate` metadata.  
**UX Notes:** Badges for type and slot count.  
**NFR Hooks:** <~1.5s typical.

---
<br>

#### SR-ALB-21 — Instance Gallery & Sharing (S)
**Purpose:** Provide a pleasant way to view a completed album.  
**Preconditions:** Instance public or link-only.  
**Normal Flow:** Render a shareable gallery view with slot frames and captions; no raw EXIF; allow download of **web-sized** images.  
**Error/Edge:** Private instance → 404 unless owner.  
**Permissions:** Public per visibility; Owner download originals.  
**Data Touchpoints:** Generated thumbnails.  
**UX Notes:** Lightweight, mobile-friendly.  
**NFR Hooks:** CDN caching.

---
м

#### SR-ALB-22 — Ownership & Licensing (M)
**Purpose:** Clarify who can remove content and on what terms.  
**Preconditions:** Screenshot uploaded.  
**Normal Flow:** **Uploader owns** the screenshot; may delete it anytime; Admin may remove for policy/DMCA.  
**Error/Edge:** Deleting an accepted slot fill → progress recalculates.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** Storage deletion; SlotFill state.  
**UX Notes:** Clear confirmation text.  
**NFR Hooks:** Soft-delete grace window.

---
<br>

#### SR-ALB-23 — Policy & Reporting (M)
**Purpose:** Keep albums safe and on-topic.  
**Preconditions:** Authenticated reporter or AI flag.  
**Normal Flow:** Users can report fills/templates/instances (NSFW, off-topic, infringement); items get a caution banner and enter moderation queue.  
**Error/Edge:** Duplicate reports → collapsed.  
**Permissions:** Any user (report); Admin (action), Curator (recommend).  
**Data Touchpoints:** Reports queue; content status.  
**UX Notes:** Minimal reason picker + optional note.  
**NFR Hooks:** Rate limits; audit.

---
<br>

#### SR-ALB-24 — Discovery Facets for Users (S)
**Purpose:** Let users filter by spoiler posture and difficulty.  
**Preconditions:** Templates indexed.  
**Normal Flow:** Facets include **type**, spoiler level distribution (none/light/heavy), presence of optional slots, average completion time.  
**Error/Edge:** Insufficient data → hide facet.  
**Permissions:** Public.  
**Data Touchpoints:** Aggregates.  
**UX Notes:** Tooltips explain types/spoilers.  
**NFR Hooks:** Precomputed stats.

---
<br>

#### SR-ALB-25 — Cross-Variant Scope (S)
**Purpose:** Allow one album to apply to all variants or a specific one.  
**Preconditions:** Template edit.  
**Normal Flow:** `variant_scope=game` (applies across variants) or bind to a `variant_id`.  
**Error/Edge:** Variant hidden → banner on template.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** `AlbumTemplate.variant_scope`.  
**UX Notes:** Scope badge on template card.  
**NFR Hooks:** Search respects scope.

---
м

#### SR-ALB-26 — Instance Progress on Profile (S)
**Purpose:** Reflect album activity in user profile.  
**Preconditions:** Instance changes.  
**Normal Flow:** Add timeline entries (“Completed **[Album]**”); show badge if earned.  
**Error/Edge:** Private instance → no public signal.  
**Permissions:** System.  
**Data Touchpoints:** Profile activity log, badges.  
**UX Notes:** Links to gallery.  
**NFR Hooks:** Prune per retention.

---
<br>

#### SR-ALB-27 — File Hygiene & Quarantine (M)
**Purpose:** Keep image uploads safe.  
**Preconditions:** Upload started.  
**Normal Flow:** AV scan; EXIF removed; suspicious → quarantine; Admin review or purge.  
**Error/Edge:** Scanner down → block with apology.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Scan logs; quarantine store.  
**UX Notes:** Minimal detail to avoid aiding attackers.  
**NFR Hooks:** Alerts on spikes.

---
<br>

#### SR-ALB-28 — Performance & Caching (S)
**Purpose:** Keep album pages snappy.  
**Preconditions:** Cache layer.  
**Normal Flow:** Cache template pages and public galleries; invalidate on edit/moderation.  
**Error/Edge:** Cache miss → fetch live.  
**Permissions:** System.  
**Data Touchpoints:** Cache keys per template/instance.  
**UX Notes:** None.  
**NFR Hooks:** ETag/Last-Modified.

---
<br>

#### SR-ALB-29 — Accessibility & Localisation (M)
**Purpose:** Make albums usable for more people.  
**Preconditions:** Rendering UI.  
**Normal Flow:** Localise all strings (EN/UA); provide alt text fields; spoiler blur is screen-reader friendly; keyboard navigation across slots.  
**Error/Edge:** Missing translation → fall back to EN with marker.  
**Permissions:** System.  
**Data Touchpoints:** Locale bundles; alt-text on slots/fills.  
**UX Notes:** Meet practical a11y standards.  
**NFR Hooks:** Lint translations.

---
<br>

#### SR-ALB-30 — Rate Limits & Abuse Controls (S)
**Purpose:** Prevent spam and overload.  
**Preconditions:** N/A.  
**Normal Flow:** Apply per-user/IP limits to uploads, comments, reports.  
**Error/Edge:** Exceeded → “Please try again later.”  
**Permissions:** System.  
**Data Touchpoints:** Throttle counters.  
**UX Notes:** Neutral messaging.  
**NFR Hooks:** WAF/burst detection.

---
<br>

#### SR-ALB-31 — Help & “How to Capture” (S)
**Purpose:** Reduce confusion for new players.  
**Preconditions:** N/A.  
**Normal Flow:** Contextual help near slots: where/when to capture, how to take screenshots on major platforms/emulators, and ethical usage.  
**Error/Edge:** Help CDN down → inline fallback.  
**Permissions:** Public.  
**Data Touchpoints:** Static content.  
**UX Notes:** UA/EN versions; spoiler-aware phrasing.  
**NFR Hooks:** Cached.

---
<br>

#### SR-ALB-32 — Non-Goals & Safety Boundaries (M)
**Purpose:** Make limits clear.  
**Preconditions:** N/A.  
**Normal Flow:** No video uploads (links only). No overlays, memory scanners, or tools that could violate ToS/anti-cheat. No auto-capture hooks into games.  
**Error/Edge:** Requests for auto-capture → explain policy and point to manual guidance.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Friendly, explicit copy.  
**NFR Hooks:** None.

---
<br>
<br>

### 4.6 User Profile & Progress View

> **Traceability:** BR-PRO-01..05, BR-ACH-08..12, BR-ALB-01..08, BR-SAV-01..06, BR-GOV-02..04, BR-DIS-01..03; NFR-UX-01..05, NFR-LOC-01..02, NFR-SEC-01..05, NFR-PERF-01..02  
> **Module prefix:** `SR-PROF-*`  
> **Scope:** Public-facing and self views of a user’s profile, including overview, activity timeline, achievements (official/retro/fan), sticker albums, saves, badges, and privacy/visibility controls. Respects linking/visibility from §4.1, saves from §4.3, achievement rules from §4.4, album rules from §4.5. 

---
<br>

#### SR-PROF-01 — Profile Sections & Routing (M)
**Purpose:** Provide a consistent structure for all profiles.  
**Preconditions:** Profile exists.  
**Normal Flow:** Profile pages expose tabs: **Overview**, **Achievements**, **Albums**, **Saves**, **About** (basic info), plus **Moderation** (Admins only).  
**Error/Edge:** Nonexistent/suspended user → 404 or “Profile unavailable” banner.  
**Permissions:** Public (view respecting visibility).  
**Data Touchpoints:** `Profile`, linked aggregates.  
**UX Notes:** Sticky tab header; mobile-friendly.  
**NFR Hooks:** Cache public tabs; ETag.

---
<br>

#### SR-PROF-02 — Overview Tab (M)
**Purpose:** Show a high-signal snapshot without spoilers.  
**Preconditions:** Profile loaded.  
**Normal Flow:** Display avatar, nickname, **badges**, quick stats (completed sets/albums, public saves), latest **timeline** items (respecting visibility), and soft CTAs to view other tabs.  
**Error/Edge:** No public data → empty states with hints.  
**Permissions:** Public view; Owner sees private counts marked.  
**Data Touchpoints:** Aggregates over achievements/albums/saves; badges.  
**UX Notes:** Spoiler-safe snippets only.  
**NFR Hooks:** Nightly rollups for counts.

---
<br>

#### SR-PROF-03 — Achievements Tab (M)
**Purpose:** Present official, retro, and fan-made progress.  
**Preconditions:** Ach sets exist or panels linked.  
**Normal Flow:** Sections for **Official (Steam)**, **Retro (RA)**, **Fan-made**; each lists sets with progress bars and **time-to-complete** for completed sets (from §4.4).  
**Error/Edge:** Panel unlinked or private → “Link to show” or “Hidden by owner”.  
**Permissions:** Public view; visibility respected per set (§4.4 SR-ACH-22).  
**Data Touchpoints:** `AchSet`, `AchProgress` aggregates; external linking state.  
**UX Notes:** Badges: “read-only” for Steam/RA; spoilers collapsed by default.  
**NFR Hooks:** Cached summaries.

---
<br>

#### SR-PROF-04 — Albums Tab (M)
**Purpose:** Show album instances and completion.  
**Preconditions:** Album instances exist.  
**Normal Flow:** Grid of **album instances** with progress/completed badge, completion timestamp, and link to gallery (if public/link-only).  
**Error/Edge:** Private instances → hidden; counts still indicate private totals to Owner only.  
**Permissions:** Public view; visibility enforced (§4.5 SR-ALB-18).  
**Data Touchpoints:** `AlbumInstance` aggregates.  
**UX Notes:** Template “type” badge (new_player/full_discovery/themed).  
**NFR Hooks:** Thumbnails via CDN.

---
<br>

#### SR-PROF-05 — Saves Tab (M)
**Purpose:** Surface the user’s contributions of save files.  
**Preconditions:** Saves exist.  
**Normal Flow:** List **public/link-only** saves with compatibility badges (build/region/status), download count, and report link.  
**Error/Edge:** Private saves → hidden; owner sees all with visibility chips.  
**Permissions:** Public view; Owner full.  
**Data Touchpoints:** `Save` metadata and aggregates.  
**UX Notes:** “Learn how to use saves safely” help link.  
**NFR Hooks:** Paged list; fast filters.

---
<br>

#### SR-PROF-06 — About Tab (M)
**Purpose:** Provide minimal account facts without PII.  
**Preconditions:** Profile exists.  
**Normal Flow:** Show nickname, join date, locale, role (Regular/Curator/Admin), and **external panels visibility** (Steam/RA) if public. No email or external IDs exposed.  
**Error/Edge:** Hidden panels → “Owner has hidden external profiles.”  
**Permissions:** Public view; Owner sees full.  
**Data Touchpoints:** `Profile`, `ExternalLinks.visibility`.  
**UX Notes:** Plain-language privacy explanations.  
**NFR Hooks:** i18n strings.

---
<br>

#### SR-PROF-07 — External Panels Rendering (M)
**Purpose:** Present Steam/RA panels safely and read-only.  
**Preconditions:** Links active and visible.  
**Normal Flow:** Show last-synced time, summary of completed official/retro achievements, and deep link buttons.  
**Error/Edge:** Provider outage → stale chip; unlink → panel disappears.  
**Permissions:** Public/Owner per visibility.  
**Data Touchpoints:** `ExternalLinks.status/last_synced_at`.  
**UX Notes:** “Read-only” label; do not display external usernames if owner chooses to hide.  
**NFR Hooks:** Cached fragments.

---
<br>

#### SR-PROF-08 — Timeline (Activity Feed) (M)
**Purpose:** Communicate meaningful events without noise.  
**Preconditions:** Events exist.  
**Normal Flow:** Show entries like **“Completed [Set]”**, **“Finished [Album]”**, **“Published a fan-made set/album”**, or **“Uploaded a save”**; each entry respects item visibility.  
**Error/Edge:** All items private → empty state.  
**Permissions:** Public view; Owner sees private marker on hidden entries.  
**Data Touchpoints:** Profile activity log derived from other modules.  
**UX Notes:** Time-ago formatting; pagination.  
**NFR Hooks:** Retention window; indexing.

---
<br>

#### SR-PROF-09 — Badges (M)
**Purpose:** Reward notable milestones.  
**Preconditions:** Badge events fired by other modules.  
**Normal Flow:** Display **game-specific album badges** and generic badges (e.g., “First fan-set author”), with timestamps.  
**Error/Edge:** Revoked badge → show removed in Owner’s view with reason.  
**Permissions:** Public view; Admin revoke.  
**Data Touchpoints:** `ProfileBadges`.  
**UX Notes:** Tooltip explains how earned.  
**NFR Hooks:** Sprite sheets for icons.

---
<br>

#### SR-PROF-10 — Curator Contributions (M)
**Purpose:** Showcase a curator’s authored content.  
**Preconditions:** User role Curator or authored items exist.  
**Normal Flow:** Dedicated section lists **fan-made achievement sets** and **album templates** authored or co-authored, with ratings and usage counts.  
**Error/Edge:** Archived items → separated with badge.  
**Permissions:** Public view.  
**Data Touchpoints:** AchSet/AlbumTemplate ownership.  
**UX Notes:** “Co-authored” chip.  
**NFR Hooks:** Paginated.

---
<br>

#### SR-PROF-11 — Visibility Controls (Per Section) (M)
**Purpose:** Give owners granular privacy sliders.  
**Preconditions:** Owner view.  
**Normal Flow:** For **Achievements (fan)**, **Albums**, **Saves**, **Timeline**, choose **Private / Link-only / Public** (defaults from §4.1).  
**Error/Edge:** Moderation freeze → temporarily disable toggles with banner.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** Visibility prefs per section.  
**UX Notes:** Short, unambiguous copy; remembers last setting.  
**NFR Hooks:** Enforced consistently in all reads.

---
<br>

#### SR-PROF-12 — Shareable Profile Links (S)
**Purpose:** Allow controlled sharing.  
**Preconditions:** Profile has public or link-only sections.  
**Normal Flow:** Generate main profile URL and **link-only** share URLs for specific tabs where allowed.  
**Error/Edge:** Tab entirely private → no link-only URL.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** Tokenised share links for link-only.  
**UX Notes:** Explain access scope.  
**NFR Hooks:** Signed URLs with TTL for link-only (optional).

---
<br>

#### SR-PROF-13 — Spoiler Handling on Profile (M)
**Purpose:** Avoid revealing plot-sensitive info.  
**Preconditions:** Rendering achievements/albums.  
**Normal Flow:** Collapse spoiler-marked criteria and heavy-spoiler album hints by default; user can reveal.  
**Error/Edge:** None.  
**Permissions:** Public/Owner.  
**Data Touchpoints:** Spoiler flags from sets/albums.  
**UX Notes:** Screen-reader accessible reveal.  
**NFR Hooks:** i18n.

---
<br>

#### SR-PROF-14 — Locale & UA Context (S)
**Purpose:** Honour localisation preferences.  
**Preconditions:** Locale known.  
**Normal Flow:** Profile UI strings render in EN/UA; UA users see brief note explaining how UA/Russia flags are shown elsewhere (catalogue), not on profiles.  
**Error/Edge:** Missing translation → EN fallback.  
**Permissions:** System.  
**Data Touchpoints:** Locale bundles.  
**UX Notes:** Keep note minimal and non-intrusive.  
**NFR Hooks:** Translation lint.

---
<br>

#### SR-PROF-15 — Public vs Self Data Density (S)
**Purpose:** Keep public pages lightweight but informative.  
**Preconditions:** Profile view context.  
**Normal Flow:** Public view: summarised counts, lists paginated; Owner view: richer breakdowns (private counts labelled).  
**Error/Edge:** None.  
**Permissions:** System.  
**Data Touchpoints:** Aggregation queries.  
**UX Notes:** Clear “Only you can see …” labels.  
**NFR Hooks:** Separate cache keys per view mode.

---
<br>

#### SR-PROF-16 — Search & Discovery of Users (S)
**Purpose:** Let others find notable contributors.  
**Preconditions:** Index built.  
**Normal Flow:** Search by nickname; filters for **Curators**, **top save contributors**, **most completed albums/sets** (public-only).  
**Error/Edge:** No matches → suggestions.  
**Permissions:** Public.  
**Data Touchpoints:** User index with public aggregates.  
**UX Notes:** Avoid showing emails or external IDs.  
**NFR Hooks:** <~1.5s typical.

---
<br>

#### SR-PROF-17 — Report Profile / Content (M)
**Purpose:** Keep community safe.  
**Preconditions:** Authenticated reporter.  
**Normal Flow:** Report categories: abusive nickname/avatar, plagiarised curator content, off-topic/NSFW gallery; enters moderation queue.  
**Error/Edge:** Duplicate reports collapsed.  
**Permissions:** Any user (report); Admin (action).  
**Data Touchpoints:** Reports queue.  
**UX Notes:** Simple reason picker + optional note.  
**NFR Hooks:** Rate limits; audit.

---
<br>

#### SR-PROF-18 — Admin View & Actions (M)
**Purpose:** Give moderators a single control surface.  
**Preconditions:** Admin.  
**Normal Flow:** Admin-only tab showing strikes, reports, recent actions; can **hide avatar**, **suspend posting**, **revoke badges**, or **soft-suspend** profile.  
**Error/Edge:** Appeal in progress → banner with link.  
**Permissions:** Admin.  
**Data Touchpoints:** Moderation records; profile status.  
**UX Notes:** Clear, reversible actions where possible.  
**NFR Hooks:** Full audit.

---
<br>

#### SR-PROF-19 — Performance & Caching (S)
**Purpose:** Ensure snappy profile loads.  
**Preconditions:** Cache layer.  
**Normal Flow:** Cache public Overview/Achievements/Albums summaries; invalidate on relevant content changes.  
**Error/Edge:** Cache stampede prevented via locks.  
**Permissions:** System.  
**Data Touchpoints:** Cache keys per user/tab.  
**UX Notes:** None.  
**NFR Hooks:** ETag/Last-Modified.

---
<br>

#### SR-PROF-20 — Accessibility (M)
**Purpose:** Make profiles usable for more people.  
**Preconditions:** Rendering UI.  
**Normal Flow:** Proper headings, alt text on badges/avatars, keyboard tab order on tabs and lists, high-contrast safe.  
**Error/Edge:** Missing alt text → generic but non-empty.  
**Permissions:** System.  
**Data Touchpoints:** Alt text fields for badges.  
**UX Notes:** Meets practical a11y standards.  
**NFR Hooks:** Accessibility lint in CI.

---
<br>

#### SR-PROF-21 — Owner’s Quick Settings (S)
**Purpose:** Reduce friction for privacy changes.  
**Preconditions:** Owner view.  
**Normal Flow:** **Quick panel** on Overview to toggle section visibilities and external panel visibility without leaving the page.  
**Error/Edge:** Moderation freeze → disabled toggles.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** Visibility prefs; ExternalLinks.visibility.  
**UX Notes:** Instant feedback; toasts.  
**NFR Hooks:** Debounced saves.

---
<br>

#### SR-PROF-22 — Deep Links to Items (M)
**Purpose:** Let visitors jump to the referenced content.  
**Preconditions:** Content public or link-only.  
**Normal Flow:** Timeline cards and tab lists link directly to **set/item pages**, **album galleries**, **save pages**.  
**Error/Edge:** Private target → respectful “Not available” page.  
**Permissions:** Public respecting visibility.  
**Data Touchpoints:** None.  
**UX Notes:** Consistent link styling + icons.  
**NFR Hooks:** None.

---
<br>

#### SR-PROF-23 — Owner Drafts Snapshot (S)
**Purpose:** Help creators resume work.  
**Preconditions:** Owner is Curator with drafts.  
**Normal Flow:** Overview shows small panel of **draft sets/albums** with “Continue editing”.  
**Error/Edge:** None.  
**Permissions:** Owner only.  
**Data Touchpoints:** AchSet/AlbumTemplate drafts.  
**UX Notes:** Private by design.  
**NFR Hooks:** None.

---
<br>

#### SR-PROF-24 — Counts & Rollups Integrity (M)
**Purpose:** Keep numbers trustworthy.  
**Preconditions:** Aggregations.  
**Normal Flow:** Public counts exclude private/link-only where appropriate; Owner sees total with markers.  
**Error/Edge:** Late-arriving moderation → recalc on next job.  
**Permissions:** System.  
**Data Touchpoints:** Aggregation tables; visibility rules.  
**UX Notes:** Subtle footnote “Counts reflect visible items.”  
**NFR Hooks:** Nightly and on-change recompute.

---
<br>

#### SR-PROF-25 — Self-View vs Public Preview (S)
**Purpose:** Let owners check what others see.  
**Preconditions:** Owner signed in.  
**Normal Flow:** Toggle **“View as visitor”** to simulate public view with current privacy settings.  
**Error/Edge:** None.  
**Permissions:** Owner.  
**Data Touchpoints:** None.  
**UX Notes:** Clear banner when preview active.  
**NFR Hooks:** Client-only toggle; no data changes.

---
<br>

#### SR-PROF-26 — Profile Suspension & Messages (M)
**Purpose:** Communicate account limits clearly.  
**Preconditions:** Suspension or restriction active.  
**Normal Flow:** Profile shows banner describing limited capabilities (e.g., posting disabled) with link to policy/appeal.  
**Error/Edge:** Appeal accepted → banner removed.  
**Permissions:** Public/Owner view; Admin controls state.  
**Data Touchpoints:** Profile status fields.  
**UX Notes:** Neutral, factual language.  
**NFR Hooks:** Audit trails.

---
<br>

#### SR-PROF-27 — Minimal PII Exposure (M)
**Purpose:** Prevent accidental data leakage.  
**Preconditions:** Public view.  
**Normal Flow:** Never show email, IP, external account IDs/handles unless owner explicitly chooses to show a **panel** and it does not reveal sensitive IDs.  
**Error/Edge:** Legacy data → migration strips sensitive fields.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Rendering layer; profile fields.  
**UX Notes:** Privacy footnote on About tab.  
**NFR Hooks:** Static checks in templates.

---
<br>

#### SR-PROF-28 — API & Rate Limits (S)
**Purpose:** Keep profile endpoints stable.  
**Preconditions:** API access.  
**Normal Flow:** Public GET for profiles/tabs; throttled POST for visibility changes (owner only).  
**Error/Edge:** Limit exceeded → generic error.  
**Permissions:** As per visibility; owner for writes.  
**Data Touchpoints:** Logs, counters.  
**UX Notes:** Do not expose limits publicly.  
**NFR Hooks:** CDN cache on public GETs.

---
<br>
<br>

### 4.7 Moderation & Reporting

> **Traceability:** BR-GOV-01..05, BR-SAV-01..08, BR-ACH-04..14, BR-ALB-01..08, BR-PRO-05, BR-CAT-04..05, BR-DIS-01..03;  
> **NFR:** NFR-SEC-01..05, NFR-LAW-01..02, NFR-UX-01..05, NFR-LOC-01..02, NFR-PERF-01..02  
> **Module prefix:** `SR-MOD-*`  
> **Scope:** Community reports, AI triage, moderator workflows, actions (soft-hide, quarantine, remove, override), user sanctions, appeals, audit, and transparency.  
> **Cross-cutting:** Ties into Saves (§4.3 quarantine/compat), Achievements (§4.4 evidence/comments), Albums (§4.5 fills/comments), Catalogue policy flags (§4.2), and Profile privacy (§4.6).

---
<br>

#### SR-MOD-01 — Content Types & IDs (M)
**Purpose:** Define what can be reported and moderated.  
**Preconditions:** Content exists.  
**Normal Flow:** Supported targets: `save`, `album_template`, `album_instance`, `slot_fill`, `ach_set`, `ach_item`, `ach_evidence`, `comment`, `profile`, `catalogue_policy_flag`.  
**Error/Edge:** Unknown type → reject.  
**Permissions:** Public view; report/authenticated (see SR-MOD-03).  
**Data Touchpoints:** `ModerationTarget{type, target_id}`.  
**UX Notes:** Consistent “Report” entry across components.  
**NFR Hooks:** Type enum locked.

---
<br>

#### SR-MOD-02 — Report Reason Taxonomy (M)
**Purpose:** Standardise report categories for triage & localisation.  
**Preconditions:** None.  
**Normal Flow:** Reasons include: `malware/suspicious file`, `mislabel/misleading`, `NSFW/inappropriate`, `harassment/hate`, `copyright/DMCA`, `off-topic/spam`, `language-policy`, `privacy/safety`.  
**Error/Edge:** Free-text reason allowed only with category `other`.  
**Permissions:** System; Admin edit list.  
**Data Touchpoints:** `Report.reason_code`, `reason_text`.  
**UX Notes:** Localised labels; short examples.  
**NFR Hooks:** Versioned list; analytics keyed by code.

---
<br>

#### SR-MOD-03 — Submit Report (M)
**Purpose:** Let users flag problematic content safely.  
**Preconditions:** Authenticated.  
**Normal Flow:** User opens “Report”, selects reason, optional note/link; submit → create `Report` and queue (SR-MOD-06).  
**Error/Edge:** Duplicate by same user on same target within 7d → collapse.  
**Permissions:** Any signed-in user.  
**Data Touchpoints:** `Report{report_id, target_type, target_id, reporter_id, reason, note, status}`.  
**UX Notes:** 3–5 clicks; neutral copy; thank-you toast.  
**NFR Hooks:** Rate-limits per user/IP.

---
<br>

#### SR-MOD-04 — Reporter Safety & Anonymity (M)
**Purpose:** Protect reporters from retaliation.  
**Preconditions:** Report exists.  
**Normal Flow:** Reporters are **never** shown to content owners or public; only Admins see reporter ID.  
**Error/Edge:** Legal request (court/LE) → see SR-MOD-21.  
**Permissions:** Admin view; not visible to owners.  
**Data Touchpoints:** `Report.reporter_id`.  
**UX Notes:** “Your report is anonymous to the poster.”  
**NFR Hooks:** Access controls, audit.

---
<br>

#### SR-MOD-05 — Report Abuse Controls (M)
**Purpose:** Prevent weaponised reporting.  
**Preconditions:** None.  
**Normal Flow:** Throttle repeated reports; detect brigading (burst from new accounts) → weight lower, auto-queue for review.  
**Error/Edge:** False positives → moderator can whitelist reporter.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Throttle counters, reporter reputation.  
**UX Notes:** Generic “Please try later” messaging.  
**NFR Hooks:** WAF rules, anomaly detection.

---
<br>

#### SR-MOD-06 — Triage Scoring & Queues (M)
**Purpose:** Prioritise what moderators see first.  
**Preconditions:** Reports or AI flags present.  
**Normal Flow:** Score by **severity (reason)** + **volume** + **reporter reputation** + **target visibility**; route to queues: *Safety*, *Copyright/DMCA*, *Content Quality*, *Metadata/Policy*, *Malware/Quarantine*.  
**Error/Edge:** Queue overload → auto-escalate SLA alerts (SR-MOD-22).  
**Permissions:** System; Admin view.  
**Data Touchpoints:** `QueueItem{score, queue, eta}`.  
**UX Notes:** Badges per queue.  
**NFR Hooks:** Real-time scoring, stable ordering.

---
<br>

#### SR-MOD-07 — AI Pre-screen (S)
**Purpose:** Reduce human load on obvious cases.  
**Preconditions:** Upload/report exists.  
**Normal Flow:** AI flags NSFW/off-topic/mismatch; for `malware` use AV engine (ties to SR-SAV-24 / SR-ALB-27). Mark `ai_flag` with label + confidence.  
**Error/Edge:** AI unavailable → skip.  
**Permissions:** System; Admin sees flags.  
**Data Touchpoints:** `Report.ai_flags[]`.  
**UX Notes:** AI verdict **not** surfaced to users.  
**NFR Hooks:** Monitor FP/FN; adjustable thresholds.

---
<br>

#### SR-MOD-08 — Moderator Dashboard (M)
**Purpose:** Provide a single control surface.  
**Preconditions:** Admin role.  
**Normal Flow:** Queues, filters, search by target/user, bulk select; open **Case View** (SR-MOD-09).  
**Error/Edge:** Conflicting edits → optimistic locking.  
**Permissions:** Admin.  
**Data Touchpoints:** Reports, targets, actions log.  
**UX Notes:** Keyboard shortcuts; fast preview panes.  
**NFR Hooks:** <~1.5s list loads.

---
<br>

#### SR-MOD-09 — Case View & Safe Preview (M)
**Purpose:** Review context without risk.  
**Preconditions:** Case open.  
**Normal Flow:** Show target, history, reporter notes, AI flags; **safe viewers** (no active content, EXIF scrubbed, server-side link fetch with HEAD only).  
**Error/Edge:** Dead external links → show status.  
**Permissions:** Admin.  
**Data Touchpoints:** Read content, fetch link metadata via proxy.  
**UX Notes:** Side-by-side: evidence vs policy checklist.  
**NFR Hooks:** No client-side third-party calls.

---
<br>

#### SR-MOD-10 — Outcomes & Actions (M)
**Purpose:** Standardise possible decisions.  
**Preconditions:** Case reviewed.  
**Normal Flow:** Actions: **dismiss**, **soft-hide**, **quarantine**, **remove**, **edit/label correction**, **compatibility override** (ties SR-SAV-12), **policy-flag update** (ties SR-CAT-18), **user warning/strike**, **temp suspension**, **role demotion**, **ban**.  
**Error/Edge:** Destructive actions require confirmation & reason.  
**Permissions:** Admin (decide), Curator (recommend edit/label).  
**Data Touchpoints:** `ModAction{action, actor, reason, timestamp}`; target status fields.  
**UX Notes:** Clear outcomes; undo where possible (soft-hide).  
**NFR Hooks:** Full audit trail.

---
<br>

#### SR-MOD-11 — Soft-Hide Behaviour (M)
**Purpose:** Reduce harm without losing context.  
**Preconditions:** Soft-hide action chosen.  
**Normal Flow:** Content remains accessible via direct link to owner/mods with **warning banner**; excluded from search/feeds.  
**Error/Edge:** Repeated reports after soft-hide → escalate to remove.  
**Permissions:** System/Admin.  
**Data Touchpoints:** `target.visibility=soft_hidden`.  
**UX Notes:** Banner explains status and appeal route.  
**NFR Hooks:** Enforced in all listing APIs.

---
<br>

#### SR-MOD-12 — Quarantine for Files (M)
**Purpose:** Isolate risky binaries/images.  
**Preconditions:** AV or moderator flag.  
**Normal Flow:** Move file to quarantine store; block downloads; show caution banner; notify uploader.  
**Error/Edge:** False positive → Admin restores.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Quarantine table; storage pointers.  
**UX Notes:** Minimal info to avoid aiding attackers.  
**NFR Hooks:** Time-boxed retention.

---
<br>

#### SR-MOD-13 — Copyright & DMCA Fast-Path (M)
**Purpose:** Handle legal takedowns properly.  
**Preconditions:** Copyright report received.  
**Normal Flow:** Capture formal claim data; **temporarily disable** target for public while reviewing; if valid → remove & retain evidence; notify owner with counter-notice flow.  
**Error/Edge:** Fraudulent claims → deny and record.  
**Permissions:** Admin/Legal.  
**Data Touchpoints:** Legal case records; target status.  
**UX Notes:** Plain-language emails; deadlines visible.  
**NFR Hooks:** Legal hold retention (SR-MOD-21).

---
<br>

#### SR-MOD-14 — Language Policy Enforcement (M)
**Purpose:** Enforce supported UGC languages and UA policy.  
**Preconditions:** Target is `comment`/`template` text.  
**Normal Flow:** Only **EN/UA** allowed for user-facing text; RU content → request EN/UA rewrite or remove if non-compliant; catalogue UA/Russia flags displayed per locale (policy itself managed in §4.2).  
**Error/Edge:** False detection → allow manual override.  
**Permissions:** Admin enforce; Curator recommend.  
**Data Touchpoints:** Mod actions; comment status.  
**UX Notes:** Respectful, clear policy notice.  
**NFR Hooks:** Optional language detector (server-side).

---
<br>

#### SR-MOD-15 — Mislabel & Metadata Corrections (M)
**Purpose:** Fix harmful inaccuracies without deleting.  
**Preconditions:** Report reason `mislabel/misleading`.  
**Normal Flow:** Moderator edits labels (e.g., save build/region, album hint, ach criteria typos) or requests curator/uploader correction; change log recorded.  
**Error/Edge:** Disputed correction → route to appeal (SR-MOD-19).  
**Permissions:** Admin edit; Curator can accept suggestions on their content.  
**Data Touchpoints:** Target fields; change log.  
**UX Notes:** Banner “Corrected by moderation”.  
**NFR Hooks:** Versioning where applicable.

---
<br>

#### SR-MOD-16 — Compatibility Override (S)
**Purpose:** Resolve save compatibility ambiguity.  
**Preconditions:** Evidence shows failure/success.  
**Normal Flow:** Admin sets `incompatible_since_build` or flips status (`confirmed/legacy/incompatible`) per §4.3, with reason & links.  
**Error/Edge:** New counter-evidence → reopen case.  
**Permissions:** Admin (decide), Curator (propose).  
**Data Touchpoints:** Save fields; ModAction log.  
**UX Notes:** Public chip shows “Moderator-verified”.  
**NFR Hooks:** Audit.

---
<br>

#### SR-MOD-17 — User Sanctions (M)
**Purpose:** Apply proportionate, escalating consequences.  
**Preconditions:** Policy breach confirmed.  
**Normal Flow:** **Warning → Posting throttle → Temp suspension (x days) → Role demotion (Curator→Regular) → Ban.**  
**Error/Edge:** Evasion detected → extend ban, lock new signups (device/IP heuristics).  
**Permissions:** Admin.  
**Data Touchpoints:** `UserSanctions{type, start, end, reason}`.  
**UX Notes:** Clear messages; link to policy & appeal.  
**NFR Hooks:** Timers for auto-lift.

---
<br>

#### SR-MOD-18 — Strike System & Decay (S)
**Purpose:** Make enforcement predictable and fair.  
**Preconditions:** Sanctionable event.  
**Normal Flow:** Assign 1–3 strikes based on severity; strikes **decay** after N months without incidents.  
**Error/Edge:** Manual adjustment allowed by senior Admin with reason.  
**Permissions:** Admin.  
**Data Touchpoints:** Strike ledger.  
**UX Notes:** Owners can see their strike count privately.  
**NFR Hooks:** Reporting on recidivism.

---
<br>

#### SR-MOD-19 — Appeals Workflow (M)
**Purpose:** Offer due process.  
**Preconditions:** Action taken (soft-hide/remove/sanction).  
**Normal Flow:** Owner can appeal within **7 days**; second-pair review required; decision logged; content reinstated or decision upheld.  
**Error/Edge:** Vexatious repeated appeals → limit.  
**Permissions:** Owner (appeal), Admin (decide).  
**Data Touchpoints:** `Appeal{target, grounds, outcome}`.  
**UX Notes:** Status updates; expected SLA shown.  
**NFR Hooks:** Notifications; audit.

---
<br>

#### SR-MOD-20 — Notifications (M)
**Purpose:** Keep stakeholders informed.  
**Preconditions:** Report/decision events.  
**Normal Flow:** Reporter gets “received/closed” notices; owner gets “reported/decision” notices; moderators get SLA breach alerts.  
**Error/Edge:** Email outage → in-app only.  
**Permissions:** System.  
**Data Touchpoints:** Outbox; in-app inbox.  
**UX Notes:** Neutral tone; no reporter identity.  
**NFR Hooks:** Rate-limit, backoff.

---
<br>

#### SR-MOD-21 — Evidence Retention & Legal Hold (M)
**Purpose:** Preserve records for audits or legal processes.  
**Preconditions:** Removal/DMCA/appeal.  
**Normal Flow:** Store minimal necessary snapshots and logs under **legal hold** until expiry; not publicly accessible.  
**Error/Edge:** Storage limit → alert Admin.  
**Permissions:** Admin/Legal.  
**Data Touchpoints:** Evidence store; audit logs.  
**UX Notes:** None (internal).  
**NFR Hooks:** Encrypted at rest; access logging.

---
<br>

#### SR-MOD-22 — SLA & Metrics (S)
**Purpose:** Ensure timely responses.  
**Preconditions:** Queue running.  
**Normal Flow:** Targets: *Safety* < 12h, *Malware* < 6h, *DMCA* per law, others < 72h; dashboard shows breach risk.  
**Error/Edge:** Holiday load → auto-rotate on-call; show degraded mode banner internally.  
**Permissions:** Admin.  
**Data Touchpoints:** Metrics time series.  
**UX Notes:** Traffic light indicators.  
**NFR Hooks:** Alerting.

---
<br>

#### SR-MOD-23 — Moderator Permissions & Escalation (M)
**Purpose:** Separate duties and avoid concentration of power.  
**Preconditions:** Admin roles defined.  
**Normal Flow:** Roles: **Moderator**, **Senior Moderator**, **Legal**, **Admin**; destructive actions (permanent remove/ban) require Senior+; policy-flag updates require two-person review.  
**Error/Edge:** Self-review blocked.  
**Permissions:** As above.  
**Data Touchpoints:** Role map; action approvals.  
**UX Notes:** UI enforces who can click what.  
**NFR Hooks:** Audit approvals.

---
<br>

#### SR-MOD-24 — Conflict-of-Interest Guard (S)
**Purpose:** Prevent self-moderation and bias.  
**Preconditions:** Moderator is related to target (author/co-author/reporter).  
**Normal Flow:** System blocks action; requires different moderator.  
**Error/Edge:** Small team → allow Senior override with reason.  
**Permissions:** System/Senior Admin.  
**Data Touchpoints:** Ownership lookups.  
**UX Notes:** Clear rationale shown.  
**NFR Hooks:** Logged.

---
<br>

#### SR-MOD-25 — Locale & Routing (S)
**Purpose:** Match cases to language-capable moderators.  
**Preconditions:** Case has language signal (EN/UA).  
**Normal Flow:** Route UA-language cases to UA-capable mods; otherwise EN.  
**Error/Edge:** No UA mod available → allow EN mod with machine translation view.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Moderator profiles (languages).  
**UX Notes:** Flag chip shows language.  
**NFR Hooks:** MT service optional; cache.

---
<br>

#### SR-MOD-26 — Bulk Actions (S)
**Purpose:** Handle waves efficiently.  
**Preconditions:** Many similar reports.  
**Normal Flow:** Select multiple related targets → apply **soft-hide/remove/label fix** in one action; each target logged individually.  
**Error/Edge:** Mixed types with different policies → block, require per-type batches.  
**Permissions:** Admin.  
**Data Touchpoints:** ModAction records per target.  
**UX Notes:** Confirmation shows count & impact.  
**NFR Hooks:** Chunked processing.

---
<br>

#### SR-MOD-27 — System-Generated Reports (S)
**Purpose:** Feed known issues into moderation without users.  
**Preconditions:** Detectors trigger (e.g., malware, mass-downloads, spam patterns).  
**Normal Flow:** Create synthetic `Report` with `reporter_id=system`; send to proper queue.  
**Error/Edge:** Detector noise → tune thresholds.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Detector logs.  
**UX Notes:** Labeled as “System flag”.  
**NFR Hooks:** Metrics per detector.

---
<br>

#### SR-MOD-28 — Data Retention & Purge Policy (M)
**Purpose:** Limit storage of sensitive moderation data.  
**Preconditions:** Time passes.  
**Normal Flow:** Purge closed reports and non-legal-hold evidence after **12 months**; keep aggregate metrics and anonymised samples.  
**Error/Edge:** Ongoing appeal → extend retention.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Report/evidence TTL.  
**UX Notes:** None (internal).  
**NFR Hooks:** Scheduled jobs; verifiable deletion.

---
<br>

#### SR-MOD-29 — Transparency Report (S)
**Purpose:** Provide community-level accountability.  
**Preconditions:** Metrics accumulated.  
**Normal Flow:** Quarterly publish anonymised counts by category, actions taken, appeal outcomes, SLA compliance.  
**Error/Edge:** Low volume → combine with next quarter.  
**Permissions:** Admin publish.  
**Data Touchpoints:** Aggregated metrics.  
**UX Notes:** Plain charts; UA/EN versions.  
**NFR Hooks:** Export pipeline.

---
<br>

#### SR-MOD-30 — Policy Surface & Help (S)
**Purpose:** Make rules discoverable where users need them.  
**Preconditions:** UI rendering.  
**Normal Flow:** Inline links to **Community Guidelines**, **UGC Language Policy (EN/UA)**, **Save Safety**, **Evidence Tips**, and **Appeals** wherever reporting/moderation UX appears.  
**Error/Edge:** Docs CDN down → inline fallback.  
**Permissions:** Public.  
**Data Touchpoints:** Static content.  
**UX Notes:** Short, human-readable.  
**NFR Hooks:** Cached, versioned.

---
<br>
<br>

### 4.8 Discovery & Search

> **Traceability:** BR-DIS-01..03, BR-CAT-01..05, BR-SAV-01..08, BR-ACH-01..14, BR-ALB-01..08, BR-PRO-03..05, BR-GOV-02..04  
> **NFR:** NFR-PERF-01..02, NFR-UX-01..05, NFR-LOC-01..02, NFR-SEC-01..05, NFR-LAW-01..02  
> **Module prefix:** `SR-DISC-*`  
> **Scope:** Unified discovery across Games/Variants, Saves, Achievement sets, Album templates, and Users. Locale-aware (EN/UA), privacy-respecting, compatibility-aware (build/region facets), with strong indexing & ranking.

---
<br>

#### SR-DISC-01 — Searchable Entities & Vertical Tabs (M)
**Purpose:** Define what can be discovered and how results are organised.  
**Preconditions:** Index populated.  
**Normal Flow:** Single search input with vertical tabs: **Games**, **Saves**, **Achievements**, **Albums**, **Users**; default tab = Games.  
**Error/Edge:** Empty query → show curated/trending per tab.  
**Permissions:** Public view; respect visibility.  
**Data Touchpoints:** Read-only index per entity.  
**UX Notes:** Keyboard `Tab` cycles verticals; mobile-friendly chips.  
**NFR Hooks:** Fast tab switch via cached queries.

---
<br>

#### SR-DISC-02 — Global Typeahead (M)
**Purpose:** Reduce time-to-result with instant suggestions.  
**Preconditions:** Index supports prefix queries.  
**Normal Flow:** On typing, show top game titles, variants (with platform), curators, and popular sets/albums; select navigates directly.  
**Error/Edge:** Rate-limit on burst typing.  
**Permissions:** Public.  
**Data Touchpoints:** Lightweight “suggest” index.  
**UX Notes:** Highlights matched substrings; supports EN/UA and transliteration.  
**NFR Hooks:** < 150 ms typical response.

---
<br>

#### SR-DISC-03 — Query Grammar & Filters (M)
**Purpose:** Make queries expressive but simple.  
**Preconditions:** Parser available.  
**Normal Flow:** Support `platform:pc`, `status:confirmed`, `type:fan`, `locale:ua`, `set:official|retro|fan`, quoted phrases, `-term` exclusion, and ranges `build:102340-102999`.  
**Error/Edge:** Unknown filter → ignore with hint.  
**Permissions:** Public.  
**Data Touchpoints:** Parser tokens to index filters.  
**UX Notes:** Autocomplete filter names; small helper below input.  
**NFR Hooks:** Robust parsing against injection.

---
<br>

#### SR-DISC-04 — Typo Tolerance & Transliteration (M)
**Purpose:** Handle misspellings and UA/EN input quirks.  
**Preconditions:** Query received.  
**Normal Flow:** Apply fuzzy matching (edit distance ≤1–2), ignore diacritics, support UA⇄EN transliteration for names.  
**Error/Edge:** Ambiguous → offer “Did you mean …”.  
**Permissions:** Public.  
**Data Touchpoints:** Normalised fields in index.  
**UX Notes:** Show correction clearly, but keep original query.  
**NFR Hooks:** Performance guardrails on fuzzy search.

---
<br>

#### SR-DISC-05 — Synonyms & Alt Titles (S)
**Purpose:** Find games by alternate or regional names.  
**Preconditions:** Synonym list available.  
**Normal Flow:** Index `alt_titles[]` and curated synonym maps; match during search.  
**Error/Edge:** Conflicting synonyms → prefer exact title.  
**Permissions:** Admin edits synonyms.  
**Data Touchpoints:** Synonym dictionary; index fields.  
**UX Notes:** Tooltip shows “Also known as …”.  
**NFR Hooks:** Dictionary versioning.

---
<br>

#### SR-DISC-06 — Locale-Aware Ranking (S)
**Purpose:** Prioritise results that fit the user’s language.  
**Preconditions:** Locale detected (EN/UA).  
**Normal Flow:** Boost UA-localised games/sets for UA users; maintain neutrality for EN if user hasn’t opted into UA focus.  
**Error/Edge:** Unknown locale → neutral.  
**Permissions:** System.  
**Data Touchpoints:** Policy flags from catalogue.  
**UX Notes:** No intrusive banners; ranking is subtle.  
**NFR Hooks:** Configurable boosts.

---
<br>

#### SR-DISC-07 — Russia-Linked Hide Toggle (UA) (S)
**Purpose:** Respect UA policy preference in discovery.  
**Preconditions:** UA locale or explicit toggle use.  
**Normal Flow:** Facet/toggle “Hide Russia-linked”; excludes flagged content (from §4.2).  
**Error/Edge:** User turns off → results include all with small policy note.  
**Permissions:** Public.  
**Data Touchpoints:** Policy flags in index.  
**UX Notes:** Short explanation tooltip; reversible.  
**NFR Hooks:** Facet filter, not a hard delete.

---
<br>

#### SR-DISC-08 — Games Facets (M)
**Purpose:** Narrow to relevant games/variants fast.  
**Preconditions:** Games index.  
**Normal Flow:** Facets: platform, year, genre, **saves_viability**, **has_official/retro/fan sets**, **has_albums**, UA flags.  
**Error/Edge:** Too many facets → collapse group.  
**Permissions:** Public.  
**Data Touchpoints:** Denormalised facet counts.  
**UX Notes:** Clear counts; sticky applied filters.  
**NFR Hooks:** Precomputed facet buckets.

---
<br>

#### SR-DISC-09 — Saves Facets (M)
**Purpose:** Find a compatible save quickly.  
**Preconditions:** Saves index.  
**Normal Flow:** Facets: game, variant, **build manifest (exact/range)**, **status** (confirmed/legacy/untested/incompatible), region/emulator (retro), DLC flags, uploader, date, popularity.  
**Error/Edge:** No build data → hide build facet.  
**Permissions:** Public respecting visibility.  
**Data Touchpoints:** Save fields from §4.3.  
**UX Notes:** Badges in results mirror chosen facets.  
**NFR Hooks:** Numeric range support for build.

---
<br>

#### SR-DISC-10 — Achievement Sets Facets (M)
**Purpose:** Discover the right set for a playthrough.  
**Preconditions:** Ach index ready.  
**Normal Flow:** Facets: **type** (official/retro/fan), rating, recency, curator, flags (storyline/irreversible/final), variant scope.  
**Error/Edge:** Sparse data → hide facet.  
**Permissions:** Public.  
**Data Touchpoints:** AchSet metadata.  
**UX Notes:** “Read-only” badge for official/retro.  
**NFR Hooks:** Cached aggregates.

---
<br>

#### SR-DISC-11 — Album Templates Facets (M)
**Purpose:** Let users choose by spoiler posture and scope.  
**Preconditions:** Albums index.  
**Normal Flow:** Facets: **type** (new_player/full_discovery/themed), slot count, recency, curator, completion rate.  
**Error/Edge:** Missing stats → suppress metric facets.  
**Permissions:** Public.  
**Data Touchpoints:** AlbumTemplate metadata + rollups.  
**UX Notes:** Type badge and slot-count chips.  
**NFR Hooks:** Nightly rollups.

---
<br>

#### SR-DISC-12 — User Facets (S)
**Purpose:** Spotlight contributors and curators.  
**Preconditions:** Users index.  
**Normal Flow:** Filters for role (Curator/Admin/public), top save contributors, most completed albums/sets (public-only).  
**Error/Edge:** Private profiles → excluded from public rankings.  
**Permissions:** Public.  
**Data Touchpoints:** Public aggregates from profiles.  
**UX Notes:** No PII ever shown.  
**NFR Hooks:** Privacy filters baked in.

---
<br>

#### SR-DISC-13 — Sorting Per Vertical (M)
**Purpose:** Provide sensible orderings beyond relevance.  
**Preconditions:** Results present.  
**Normal Flow:**  
- Games: relevance, popularity, recently updated.  
- Saves: **compatibility closeness** (build match first), popularity, newest.  
- Achievements: rating, recency, popularity.  
- Albums: completion rate, recency, popularity.  
- Users: contributions, recency.  
**Error/Edge:** Ties → stable secondary key.  
**Permissions:** Public.  
**Data Touchpoints:** Sort fields per index.  
**UX Notes:** Remember last sort per tab/session.  
**NFR Hooks:** Precomputed popularity metrics.

---
<br>

#### SR-DISC-14 — Result Cards & Badges (M)
**Purpose:** Communicate key facts at a glance.  
**Preconditions:** Rendering results.  
**Normal Flow:** Cards include cover/thumbnail, title, chips (pillar availability, **compatibility**, **policy flags** per locale), short snippet, and deep link.  
**Error/Edge:** Unknown fields → neutral chips.  
**Permissions:** Public.  
**Data Touchpoints:** Read-only metadata.  
**UX Notes:** Accessible labels; consistent iconography.  
**NFR Hooks:** CDN images.

---
<br>

#### SR-DISC-15 — Empty States & Guidance (M)
**Purpose:** Help users recover from zero results.  
**Preconditions:** No matches.  
**Normal Flow:** Show “Broaden your search” tips and quick toggles to clear strict filters (e.g., widen build range, include legacy saves).  
**Error/Edge:** None.  
**Permissions:** Public.  
**Data Touchpoints:** None.  
**UX Notes:** Friendly, non-blaming tone.  
**NFR Hooks:** None.

---
<br>

#### SR-DISC-16 — Personalised Boosts (S)
**Purpose:** Subtly favour content aligned with user history.  
**Preconditions:** Authenticated user.  
**Normal Flow:** Boost games/sets related to recently viewed titles, linked platforms (Steam/RA), and locales; never override explicit filters.  
**Error/Edge:** New user → no boost.  
**Permissions:** System (no PII stored in index).  
**Data Touchpoints:** Session history aggregates.  
**UX Notes:** Optional “Why this result is boosted?” disclosure.  
**NFR Hooks:** Lightweight signals; privacy-safe.

---
<br>

#### SR-DISC-17 — Saved & Recent Searches (S)
**Purpose:** Reduce repetition for common queries.  
**Preconditions:** Authenticated.  
**Normal Flow:** User can save queries per vertical; recent searches auto-listed locally; saved searches sync to account.  
**Error/Edge:** None.  
**Permissions:** Owner.  
**Data Touchpoints:** Saved queries table (no results stored).  
**UX Notes:** One-click re-run buttons.  
**NFR Hooks:** Quotas per user.

---
<br>

#### SR-DISC-18 — Deep Linking to Filtered Views (S)
**Purpose:** Allow sharing of specific discovery states.  
**Preconditions:** Results page with filters.  
**Normal Flow:** Generate URL with query + facets; respects visibility on open.  
**Error/Edge:** Private results on open → empty-safe state.  
**Permissions:** Public respecting visibility.  
**Data Touchpoints:** None.  
**UX Notes:** “Copy search link” action.  
**NFR Hooks:** Short-link service optional.

---
<br>

#### SR-DISC-19 — Indexing Pipeline (M)
**Purpose:** Keep search fresh and consistent.  
**Preconditions:** Background workers.  
**Normal Flow:** Near-real-time updates on create/edit/moderation; denormalise necessary fields; backfill jobs on failure.  
**Error/Edge:** Index lag detected → show “Recently updated” badge sourced from DB, not index.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Change feed → index.  
**UX Notes:** None.  
**NFR Hooks:** Idempotent upserts; retries with backoff.

---
<br>

#### SR-DISC-20 — Visibility & Policy Filters at Index Time (M)
**Purpose:** Prevent leaking private/soft-hidden items.  
**Preconditions:** Item visibility known.  
**Normal Flow:** Index only public/link-only projections; exclude private; mark **soft-hidden** to suppress in lists while keeping direct links per policy.  
**Error/Edge:** Visibility flip → update index ASAP.  
**Permissions:** System.  
**Data Touchpoints:** Visibility flags in index.  
**UX Notes:** None.  
**NFR Hooks:** Strong consistency targets on visibility changes.

---
<br>

#### SR-DISC-21 — Privacy-Preserving Analytics (S)
**Purpose:** Improve discovery without tracking individuals.  
**Preconditions:** Logging pipeline.  
**Normal Flow:** Log query text (hashed), filters used, click-through on anonymised IDs; no IP, email, or external IDs stored.  
**Error/Edge:** Opt-out → disable analytics cookie.  
**Permissions:** System/Admin aggregate view.  
**Data Touchpoints:** Aggregated metrics.  
**UX Notes:** Simple privacy notice in footer.  
**NFR Hooks:** Retention limits (e.g., 6 months).

---
<br>

#### SR-DISC-22 — A/B Experiments on Ranking (S)
**Purpose:** Tune relevance without risking stability.  
**Preconditions:** Experiment framework.  
**Normal Flow:** Bucket users; test small ranking boosts (e.g., recency vs rating); monitor CTR and satisfaction.  
**Error/Edge:** Underperforming → auto-rollback.  
**Permissions:** Admin.  
**Data Touchpoints:** Experiment assignments; metrics.  
**UX Notes:** No visible experiment labels.  
**NFR Hooks:** Guardrails on metric deltas.

---
<br>

#### SR-DISC-23 — Pagination & Infinite Scroll (S)
**Purpose:** Scale to large result sets.  
**Preconditions:** Results > page size.  
**Normal Flow:** Desktop: numbered pages; Mobile: infinite scroll with “Back to top”; maintain stable ordering across pages.  
**Error/Edge:** Mid-scroll index updates → do not reshuffle already loaded items.  
**Permissions:** Public.  
**Data Touchpoints:** Page tokens/cursors.  
**UX Notes:** Loading spinners; preserve scroll on back.  
**NFR Hooks:** Cursor-based pagination.

---
<br>

#### SR-DISC-24 — Accessibility (M)
**Purpose:** Make search usable for more people.  
**Preconditions:** Rendering UI.  
**Normal Flow:** Proper form labels, ARIA for results, keyboard focus management, screen-reader-friendly badges and facets.  
**Error/Edge:** Missing labels → lint failures.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** High-contrast safe; large tap targets on mobile.  
**NFR Hooks:** A11y checks in CI.

---
<br>

#### SR-DISC-25 — SEO for Public Landing Pages (S)
**Purpose:** Help users discover game pages from the web.  
**Preconditions:** Public game/variant pages.  
**Normal Flow:** Server-rendered metadata for **game/variant** pages (not internal search results); sitemaps for games.  
**Error/Edge:** Noindex on internal results pages.  
**Permissions:** Public.  
**Data Touchpoints:** Static metadata generation.  
**UX Notes:** Clean titles and descriptions.  
**NFR Hooks:** Sitemap refresh nightly.

---
<br>

#### SR-DISC-26 — Security & Abuse Controls (M)
**Purpose:** Protect search endpoints.  
**Preconditions:** Public API.  
**Normal Flow:** Rate-limits per IP/user, WAF patterns, reject suspicious query payloads; cap fuzzy expansions.  
**Error/Edge:** Throttled → neutral “Try later.”  
**Permissions:** System.  
**Data Touchpoints:** Throttle counters, WAF logs.  
**UX Notes:** Never show technical reasons to end-users.  
**NFR Hooks:** Alerting on spikes.

---
<br>

#### SR-DISC-27 — “Compatible First” Boost for Saves (M)
**Purpose:** Put most likely-to-work saves up front.  
**Preconditions:** Saves search with build/region signals.  
**Normal Flow:** Boost exact **build manifest** matches, then close-range builds, then legacy; retro: region/emulator match first.  
**Error/Edge:** No build provided → fall back to popularity/recency.  
**Permissions:** Public.  
**Data Touchpoints:** Save compatibility facets in index.  
**UX Notes:** Small label “Build match” on top saves.  
**NFR Hooks:** Configurable distance function.

---
<br>

#### SR-DISC-28 — Query Builder (Advanced) (S)
**Purpose:** Help power users craft precise filters.  
**Preconditions:** Desktop view or expanded panel.  
**Normal Flow:** Visual builder for multi-filters (AND/OR groups); generates query string; sharable link (SR-DISC-18).  
**Error/Edge:** Invalid combos → greyed controls with reasons.  
**Permissions:** Public.  
**Data Touchpoints:** None.  
**UX Notes:** Collapsible advanced panel.  
**NFR Hooks:** Validated client-side.

---
<br>

#### SR-DISC-29 — Curated Collections & Trending (S)
**Purpose:** Showcase notable items without searching.  
**Preconditions:** Curator/Admin curation or metrics available.  
**Normal Flow:** Landing surfaces: “New & Noteworthy Fan Sets”, “Popular Sticker Albums”, “Recently Confirmed Saves”.  
**Error/Edge:** Low volume → fallback to all-time popular.  
**Permissions:** Public; Admin manages lists.  
**Data Touchpoints:** Curated lists; metrics.  
**UX Notes:** Carousel/grid layouts.  
**NFR Hooks:** Cached blocks.

---
<br>

#### SR-DISC-30 — API Endpoints (Read) (S)
**Purpose:** Allow clients to query discovery programmatically.  
**Preconditions:** API gateway.  
**Normal Flow:** `GET /search/{vertical}` with query + facets; responses include paging cursors and facet counts; visibility enforced.  
**Error/Edge:** Invalid filters → 400 with hints.  
**Permissions:** Public read; rate-limited.  
**Data Touchpoints:** Index adapters.  
**UX Notes:** Developer docs with examples.  
**NFR Hooks:** Strict timeouts; circuit breakers.

---
<br>

#### SR-DISC-31 — Deletion & Soft-Hide Propagation (M)
**Purpose:** Keep results consistent with moderation and privacy.  
**Preconditions:** Content removed/hidden.  
**Normal Flow:** Remove/flag in index within minutes; direct links honour soft-hide rules; saved search links degrade gracefully.  
**Error/Edge:** Indexing lag → serve from DB suppression list temporarily.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Visibility change feed → index.  
**UX Notes:** “No longer available” placeholders when necessary.  
**NFR Hooks:** Dual-layer suppression (index + runtime).

---
<br>
<br>

### 4.9 External Integrations & Data Adapters

> **Traceability:** BR-INT-01..08, BR-CAT-01..05, BR-ACH-01..14, BR-SAV-01..08, BR-ALB-01..08, BR-GOV-02..04;  
> **NFR:** NFR-SEC-01..05, NFR-PERF-01..02, NFR-LAW-01..02, NFR-LOC-01..02, NFR-UX-01..05  
> **Module prefix:** `SR-INT-*`  
> **Scope (MVP):** Read-only integrations with **IGDB** (catalogue metadata), **Steam Web API** (official achievements for linked users; basic app metadata), **RetroAchievements API** (sets & user progress), **YouTube/GameFAQs** (link validation/metadata for evidence), **Email provider** (transactional mail), **S3-compatible object storage** (files/images), **AV scanner**, **image processing/CDN**, and an optional **lightweight vision moderation** service.  
> **Cross-cutting:** Version-compatibility layer (Steam **build/manifest IDs**; retro **region/emulator** facets; statuses `confirmed / untested / legacy / incompatible`; community confirmations; Admin overrides) is fed by adapters where available. UA/Russia policy flags originate in Catalogue (§4.2) and are not fetched from third parties.

---
<br>

#### SR-INT-01 — Adapter Pattern & Contract (M)
**Purpose:** Standardise how external systems are integrated.  
**Preconditions:** None.  
**Normal Flow:** Each provider implements `Adapter` with: **config** (keys, endpoints), **capabilities** (read-only sync, link validation, file ops, mail send), **rate-limit policy**, **health check**, **error taxonomy**, **mapping layer**, **test stub**.  
**Error/Edge:** Missing capability → fail at boot with explicit message.  
**Permissions:** System/Admin configure.  
**Data Touchpoints:** Adapter registry; secrets vault.  
**UX Notes:** None (internal).  
**NFR Hooks:** Adapters are sandboxable; timeouts & retries baked in.

---
<br>

#### SR-INT-02 — Secrets & Credentials Management (M)
**Purpose:** Keep API keys and tokens safe.  
**Preconditions:** Provider credentials issued.  
**Normal Flow:** Store credentials in **secrets vault**; rotate on schedule; never log secrets; adapters read via short-lived env injection.  
**Error/Edge:** Expired/invalid → surface actionable admin alert; backoff retries.  
**Permissions:** Admin (provision/rotate).  
**Data Touchpoints:** Secrets store; adapter config.  
**UX Notes:** Admin UI shows expiry status only.  
**NFR Hooks:** KMS/HSM-backed encryption; audit trails.

---
<br>

#### SR-INT-03 — IGDB Metadata Adapter (M)
**Purpose:** Populate game & variant metadata.  
**Preconditions:** Twitch/IGDB credentials; rate limits configured.  
**Normal Flow:** Nightly sync of **games**, **platforms**, **genres**, **release dates**, **alt titles**, **cover art**; map to Catalogue (§4.2).  
**Error/Edge:** IGDB outage → skip and reschedule; schema change → log and quarantine affected records.  
**Permissions:** System/Admin.  
**Data Touchpoints:** `Game`, `Variant`, `AltTitle`, `Images`.  
**UX Notes:** Display source badge “Metadata: IGDB” on game pages.  
**NFR Hooks:** Denormalised search index refresh (§4.8).

---
<br>

#### SR-INT-04 — UA Localisation & Provenance Augmentation (S)
**Purpose:** Enrich catalogue with **UA language availability** and **developer/publisher origin** signals.  
**Preconditions:** Public sources available (e.g., Kuli, Artemiano) and internal curation.  
**Normal Flow:** Import/curate UA-specific data into a **policy extension table**; expose flags used by Catalogue (§4.2) and Discovery (§4.8).  
**Error/Edge:** Conflicts between sources → prefer curated/admin override; keep provenance notes.  
**Permissions:** Curator/Admin maintain.  
**Data Touchpoints:** `GamePolicy{ua_localised?, ua_developer?, ru_linked?}` with source refs.  
**UX Notes:** Flags surface only where policy says (not on profiles).  
**NFR Hooks:** Manual review workflow; no automated scraping commitments.

---
<br>

#### SR-INT-05 — Steam Linking & OAuth (M)
**Purpose:** Allow users to **link** Steam for read-only achievement sync.  
**Preconditions:** User authenticated on Dream Project; Steam OAuth keys configured.  
**Normal Flow:** User initiates link → OAuth redirect → store refresh token; user can **unlink** anytime (SR-ACC-10).  
**Error/Edge:** Denied consent/2FA fail → show safe retry.  
**Permissions:** Owner to link/unlink; System stores tokens.  
**Data Touchpoints:** `ExternalLinks{provider=steam, token_ref, visibility, last_synced_at}`.  
**UX Notes:** Clear “read-only, no write-back” copy.  
**NFR Hooks:** Encrypt tokens; limited scopes.

---
<br>

#### SR-INT-06 — Steam Achievements Sync (M)
**Purpose:** Mirror **official achievements** & progress.  
**Preconditions:** SR-INT-05 linked; Variant has `steam_app_id`.  
**Normal Flow:** Scheduled job fetches **app schema/achievements** and **user states**; map to `AchSet(type=official)` and `AchProgress` (§4.4).  
**Error/Edge:** Private profile/API outage → mark **stale**; don’t block UI.  
**Permissions:** System; Owner may delete cached data by unlinking.  
**Data Touchpoints:** AchSet/Item, AchProgress.  
**UX Notes:** “Official (Steam) • read-only” chip with last sync time.  
**NFR Hooks:** Respect provider rate limits; exponential backoff.

---
<br>

#### SR-INT-07 — Steam Build/Manifest Signals (S)
**Purpose:** Feed compatibility layer with build identifiers.  
**Preconditions:** Steam app known.  
**Normal Flow:** Where possible, fetch/apply **build or manifest IDs** for public apps; otherwise allow **curator/admin manual entry** or **community-sourced IDs** with provenance; watch for build changes (SR-SAV-09).  
**Error/Edge:** No official endpoint → operate in **manual/untested** mode.  
**Permissions:** Admin/Curator may update; System monitors.  
**Data Touchpoints:** `Variant.build_manifest_catalog`, `Save.tested_on_builds[]`.  
**UX Notes:** Transparent “source: manual/auto” tooltip.  
**NFR Hooks:** Never scrape; no Steam client hooking.

---
<br>

#### SR-INT-08 — RetroAchievements Linking (M)
**Purpose:** Link RA accounts for read-only sync.  
**Preconditions:** User provides RA username/API key (per RA model).  
**Normal Flow:** Store token reference; user can unlink anytime.  
**Error/Edge:** Invalid key → clear guidance to re-enter.  
**Permissions:** Owner link/unlink.  
**Data Touchpoints:** `ExternalLinks{provider=ra, token_ref, visibility, last_synced_at}`.  
**UX Notes:** “Retro (RA) • read-only” chip.  
**NFR Hooks:** Encrypt tokens; respect RA rate limits.

---
<br>

#### SR-INT-09 — RetroAchievements Sync (M)
**Purpose:** Mirror RA **sets** and **user progress**.  
**Preconditions:** SR-INT-08 linked; Variant has `ra_game_id`.  
**Normal Flow:** Scheduled job imports RA set structure and user item states; map to `AchSet(type=retro)` and `AchProgress` (§4.4).  
**Error/Edge:** RA outage → mark stale; retain last good data.  
**Permissions:** System/Admin.  
**Data Touchpoints:** AchSet/Item, AchProgress.  
**UX Notes:** Deep link to RA game page.  
**NFR Hooks:** Backoff; adapter versioning on schema drift.

---
<br>

#### SR-INT-10 — External Evidence Link Validator (M)
**Purpose:** Safely support **YouTube/GameFAQs/…** links as evidence (§4.4) without hosting.  
**Preconditions:** User submits link.  
**Normal Flow:** Check against **allowlist domains**; perform server-side **HEAD** (no client exposure) to validate reachability; store title/thumbnail metadata (if available) with TTL.  
**Error/Edge:** Non-allowlisted or unreachable → reject with reason.  
**Permissions:** Owner submit; Admin extend allowlist.  
**Data Touchpoints:** `EvidenceLink{url, provider, title, meta_expires_at}`.  
**UX Notes:** Provider icon; “opens in new tab”.  
**NFR Hooks:** Strict URL parsing; size/time caps.

---
<br>

#### SR-INT-11 — S3-Compatible Object Storage (M)
**Purpose:** Store **saves** and **screenshots** reliably.  
**Preconditions:** Bucket configured; credentials in vault.  
**Normal Flow:** Streamed uploads; server-side encryption; lifecycle rules (thumbnails, archives, quarantine).  
**Error/Edge:** Upload failure → retry, then report error; regional outage → failover if configured.  
**Permissions:** System; Owner rights per visibility rules.  
**Data Touchpoints:** Object keys in Save/Screenshot records.  
**UX Notes:** Progress indicators; resumable where possible.  
**NFR Hooks:** SSE-S3/KMS; integrity checksums.

---
<br>

#### SR-INT-12 — Image Processing & CDN (S)
**Purpose:** Fast, safe delivery of images.  
**Preconditions:** Storage operational; CDN configured.  
**Normal Flow:** Generate thumbnails; strip EXIF; serve via CDN with caching headers; signed URLs for link-only/private where applicable.  
**Error/Edge:** Processor down → fallback to original with size cap.  
**Permissions:** System.  
**Data Touchpoints:** Derivative asset records.  
**UX Notes:** None (transparent).  
**NFR Hooks:** Cache invalidation on moderation/replace.

---
<br>

#### SR-INT-13 — Antivirus / Malware Scanning (M)
**Purpose:** Protect users from malicious files.  
**Preconditions:** Upload initiated.  
**Normal Flow:** Scan each uploaded file (saves/images); suspicious → **quarantine** (SR-SAV-24 / SR-ALB-27) and queue moderation.  
**Error/Edge:** Scanner unavailable → block upload with friendly message.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Scan logs; quarantine store.  
**UX Notes:** Minimal surface detail to avoid aiding attackers.  
**NFR Hooks:** Time-boxed quarantine; alerting on spikes.

---
<br>

#### SR-INT-14 — Email Provider (M)
**Purpose:** Deliver transactional emails (verification, resets, notifications).  
**Preconditions:** Provider API key configured.  
**Normal Flow:** Send via API with templated UA/EN content; retry with backoff; fallback to in-app only on persistent failures.  
**Error/Edge:** Hard bounce → mark address invalid and prompt user to update.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Outbox table; delivery receipts (non-PII).  
**UX Notes:** Clear subject lines; unsubscribe only for non-essential mail.  
**NFR Hooks:** DKIM/SPF; sender reputation monitoring.

---
<br>

#### SR-INT-15 — Lightweight Vision Moderation (S)
**Purpose:** Triage screenshots for **NSFW/off-topic** before human review.  
**Preconditions:** Model configured (self-hosted or third-party).  
**Normal Flow:** Score uploads; auto-accept/reject within safe thresholds; otherwise queue (SR-ALB-10/11). No user-visible AI scores.  
**Error/Edge:** Model down → bypass to human review.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Validation result attached to SlotFill.  
**UX Notes:** Only final moderation outcome shown to users.  
**NFR Hooks:** Track FP/FN; adjustable thresholds; no training on user data without consent.

---
<br>

#### SR-INT-16 — Webhook Receiver Framework (S)
**Purpose:** Safely accept provider callbacks (if used).  
**Preconditions:** Provider supports webhooks.  
**Normal Flow:** Verify **HMAC signatures**, strict IP allowlist, idempotency keys; enqueue processing jobs.  
**Error/Edge:** Signature fail → drop & log.  
**Permissions:** System.  
**Data Touchpoints:** Webhook logs; job queue.  
**UX Notes:** None.  
**NFR Hooks:** Timeouts; retries with backoff.

---
<br>

#### SR-INT-17 — Rate Limit & Quota Management (M)
**Purpose:** Prevent provider throttling and stay within terms.  
**Preconditions:** Adapter configured.  
**Normal Flow:** Central limiter per provider; share budget across workers; expose utilisation to Admin dashboard.  
**Error/Edge:** Quota exhausted → degrade UI (show stale data banners) and pause non-critical syncs.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Rate limit counters; telemetry.  
**UX Notes:** Polite “last updated …” messages.  
**NFR Hooks:** Circuit breakers.

---
<br>

#### SR-INT-18 — Error Taxonomy & Fallbacks (M)
**Purpose:** Make failures predictable and recoverable.  
**Preconditions:** External call attempted.  
**Normal Flow:** Classify as `transient`, `auth`, `quota`, `schema_drift`, `permanent`; apply mapped retry/backoff or surface **staleness**; never crash user flows.  
**Error/Edge:** Repeated schema_drift → auto-disable adapter and alert.  
**Permissions:** System/Admin visibility.  
**Data Touchpoints:** Error logs with correlation IDs.  
**UX Notes:** Human-readable, non-technical messages.  
**NFR Hooks:** SLOs for error rates.

---
<br>

#### SR-INT-19 — Mapping Registry & Schema Versioning (M)
**Purpose:** Keep our internal model stable as providers evolve.  
**Preconditions:** Adapter active.  
**Normal Flow:** Maintain **mapping specs** per provider → internal entities (Game, Variant, AchSet/Item, AchProgress); version mappings; keep migration notes.  
**Error/Edge:** Provider field removal → stop mapping that field and mark as deprecated.  
**Permissions:** Admin/Engineer.  
**Data Touchpoints:** Mapping docs; code version tags.  
**UX Notes:** None.  
**NFR Hooks:** Contract tests in CI.

---
<br>

#### SR-INT-20 — Privacy & PII Boundaries (M)
**Purpose:** Avoid over-collection and exposure.  
**Preconditions:** Data import.  
**Normal Flow:** Store only **what’s needed**; do **not** persist external usernames/IDs if user sets panel to hidden (§4.6). No password or session data from providers.  
**Error/Edge:** Legacy data found → migrate or purge.  
**Permissions:** System/Admin.  
**Data Touchpoints:** ExternalLinks visibility; purge jobs.  
**UX Notes:** Clear privacy notes in UI.  
**NFR Hooks:** GDPR-style deletion jobs.

---
<br>

#### SR-INT-21 — User-Initiated Unlink & Data Purge (M)
**Purpose:** Honour user control over linked data.  
**Preconditions:** Linked account exists.  
**Normal Flow:** On **unlink**, stop sync, revoke tokens (if applicable), purge cached per-user external data (ach progress) while keeping aggregates anonymised.  
**Error/Edge:** Token revoke fails → retry; mark as to-be-revoked.  
**Permissions:** Owner/Admin.  
**Data Touchpoints:** ExternalLinks, AchProgress (official/retro).  
**UX Notes:** Explain what remains in aggregates.  
**NFR Hooks:** Idempotent purge.

---
<br>

#### SR-INT-22 — Admin Manual Overrides & Tools (S)
**Purpose:** Give maintainers safe levers.  
**Preconditions:** Admin role.  
**Normal Flow:** Admin UI to **force re-sync**, **set build IDs**, **map RA/Steam game IDs**, view adapter health, and inspect last payloads (redacted).  
**Error/Edge:** Dangerous actions gated behind confirmation.  
**Permissions:** Admin.  
**Data Touchpoints:** Adapter state; catalogue overrides.  
**UX Notes:** Clear warnings; audit every action.  
**NFR Hooks:** Role-based access; logs immutable.

---
<br>

#### SR-INT-23 — Test Stubs & Sandboxes (S)
**Purpose:** Enable development without real provider calls.  
**Preconditions:** Dev/test environment.  
**Normal Flow:** Each adapter ships a **stub** with canned responses and error modes; toggle via config.  
**Error/Edge:** None.  
**Permissions:** Devs.  
**Data Touchpoints:** Local fixtures.  
**UX Notes:** None.  
**NFR Hooks:** Deterministic outputs for CI.

---
<br>

#### SR-INT-24 — Scheduled Sync Cadence (S)
**Purpose:** Balance freshness vs. quotas.  
**Preconditions:** Job scheduler.  
**Normal Flow:** IGDB: nightly; Steam/RA: staggered per user/game (e.g., every 12–24h) with on-demand sync button (cool-down enforced).  
**Error/Edge:** User spams manual sync → throttle.  
**Permissions:** System; Owner manual sync.  
**Data Touchpoints:** `last_synced_at`, job queue.  
**UX Notes:** Show next eligible sync time.  
**NFR Hooks:** Distributed scheduling.

---
<br>

#### SR-INT-25 — Evidence & Guide Providers Allowlist (S)
**Purpose:** Keep link ecosystem clean.  
**Preconditions:** None.  
**Normal Flow:** Maintain allowlist (YouTube, GameFAQs, official wikis); Admin can add/remove; links outside allowlist require moderator approval or are blocked.  
**Error/Edge:** Phishing domain detected → auto-block and purge references.  
**Permissions:** Admin manage; users submit.  
**Data Touchpoints:** Allowlist table; link validator.  
**UX Notes:** Short tooltip on allowed providers.  
**NFR Hooks:** Periodic refresh of known-good domains.

---
<br>

#### SR-INT-26 — Image Recognition Hints (S)
**Purpose:** Help album AI understand slot intent without spoilers.  
**Preconditions:** Template/slot exists.  
**Normal Flow:** Curator can add **keyword hints** (non-spoiler) per slot to feed the vision model; hints live in adapter config, not user data.  
**Error/Edge:** Overly revealing hint flagged → curator revises.  
**Permissions:** Curator/Admin.  
**Data Touchpoints:** Slot hint store (AI-only).  
**UX Notes:** Copy warns to avoid spoilers.  
**NFR Hooks:** Hints not exposed publicly.

---
<br>

#### SR-INT-27 — Legal/ToS Compliance Gates (M)
**Purpose:** Ensure integrations stay within provider rules.  
**Preconditions:** Adapter enabled.  
**Normal Flow:** Each adapter embeds a **compliance checklist** (no scraping, no automation, no write-back, no anti-cheat interference); build watcher uses only permitted sources or manual entry.  
**Error/Edge:** Policy change detected → auto-disable adapter, alert Admin.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Compliance metadata.  
**UX Notes:** None.  
**NFR Hooks:** Periodic policy review reminder.

---
<br>

#### SR-INT-28 — Observability & Telemetry (S)
**Purpose:** Operate integrations with confidence.  
**Preconditions:** Metrics/logging stack.  
**Normal Flow:** Emit per-adapter metrics (latency, error rate, quota use, staleness); structured logs with correlation IDs; health endpoints.  
**Error/Edge:** SLO breach → pager alerts.  
**Permissions:** Admin/On-call.  
**Data Touchpoints:** Metrics store; log index.  
**UX Notes:** Admin dashboard widgets.  
**NFR Hooks:** Sampling to control cost.

---
<br>

#### SR-INT-29 — Data Residency & Localisation (S)
**Purpose:** Honour legal and performance constraints.  
**Preconditions:** Multi-region setup.  
**Normal Flow:** Store user content in EU region by default; CDN edge delivery; provider regions chosen to minimise latency and meet UA/EU expectations.  
**Error/Edge:** Provider lacks EU region → document and obtain consent if needed.  
**Permissions:** Admin configure.  
**Data Touchpoints:** Storage/CDN region settings.  
**UX Notes:** Brief note in Privacy page.  
**NFR Hooks:** Geo-replication policies.

---
<br>

#### SR-INT-30 — Right-to-Be-Forgotten Boundaries (M)
**Purpose:** Clarify what we can/can’t delete externally.  
**Preconditions:** User deletion/unlink.  
**Normal Flow:** Purge **our** cached external data; we do **not** delete data on Steam/RA/YouTube; inform user they must act with the provider directly.  
**Error/Edge:** Provider offers erasure API → optionally implement and document.  
**Permissions:** System/Admin.  
**Data Touchpoints:** Purge logs; user notifications.  
**UX Notes:** Clear, honest explanation.  
**NFR Hooks:** Audit completion of purge.

---
<br>

#### SR-INT-31 — Safety Boundaries (M)
**Purpose:** Restate hard limits for integration safety.  
**Preconditions:** N/A.  
**Normal Flow:** No anti-cheat-sensitive hooks, no process injection, no game memory scanning, no auto-capture, no write-back to platform storage/cloud.  
**Error/Edge:** Requests for such features → blocked with policy link.  
**Permissions:** System.  
**Data Touchpoints:** None.  
**UX Notes:** Friendly but explicit copy in help.  
**NFR Hooks:** None.

---
<br>
<br>

## 5. External Interface Requirements

> This section defines the interfaces visible outside the core: the **web UI**, our **public read APIs**, third-party provider integrations (Steam, RetroAchievements, IGDB, storage/CDN, email, vision moderation), **inbound webhooks**, and **import/export formats**.

---
<br>

### 5.0 How to Read This Section
- **Direction** — Outbound (we call a provider) / Inbound (provider calls us) / End-user (browser/UI) / Policy (rules layer).
- **Auth** — Mechanism & scopes (e.g., OAuth, API key, session).
- **Protocol** — Transport, formats, versioning, security.
- **Contracts** — Minimal request/response shapes, fields, states.
- **Limits** — File sizes, quotas, timeouts, rates.
- **Failure Modes** — Degrade/rollback behaviors on errors.
- **Privacy** — PII boundaries, visibility, retention.

> Cross-cutting: The **version-compatibility layer** (Steam build/manifest IDs; retro region/emulator facets; statuses `confirmed | untested | legacy | incompatible`; community confirmations; Admin overrides) is fed by adapters and surfaced in UI & API.

---
<br>

### 5.1 End-User Web UI (Browser)

**Direction:** End-user  
**Auth:** Session cookie; CSRF token on writes; OAuth only during external account linking.  
**Protocol:** HTTPS (TLS 1.2+), HSTS, HTTP/2; JSON over REST/XHR; images via CDN.

**Contracts:**  
- **Browsers:** Latest Chrome/Firefox/Edge/Safari (n–1).  
- **i18n:** `Accept-Language` or profile → **EN/UA** strings; locale formats for dates/numbers.  
- **Accessibility:** WCAG 2.1 AA; keyboardable tabs/dialogs/galleries; alt text for avatars/badges.  
- **Uploads:**  
  - Saves ≤ **200 MB** (configurable), native save extensions or `.zip`; AV scan; checksum.  
  - Screenshots ≤ **15 MB** (`png | jpeg | webp`); EXIF stripped server-side.  
  - Evidence = links only (allowlist), no video uploads.  
- **Downloads:** Saves served with SHA-256 checksum header + compatibility chips (build/region/status).  
- **Spoilers:** Heavy spoilers collapsed; explicit reveal; screen-reader friendly.

**Limits:** Front-end enforces caps; back-end authoritative.  
**Failure Modes:** Clear toasts/banners; retry hints; no stack traces.  
**Privacy:** No external IDs shown unless owner enables read-only panels; no third-party scripts on secure views.

---
<br>

### 5.2 Public REST API (Read-Only, MVP)

**Direction:** Inbound  
**Auth:** API key (server-to-server) or none for strictly public endpoints; per-key rate limits.  
**Protocol:** HTTPS JSON; versioned base path **`/api/v1`**; ETag/Last-Modified; CORS allowlist.

**Contracts (representative):**  
- `GET /api/v1/search/{vertical}` where vertical ∈ `games | saves | achievements | albums | users`  
  - Query: `q`, facets (`platform`, `build`, `status`, `type`, `locale`, …), `page`, `per_page`, `sort`  
  - Response: `{ items: [...], facets: {...}, page, per_page, next_cursor? }`  
- `GET /api/v1/games/{id}` — game + variants + pillar availability.  
- `GET /api/v1/achievements/sets/{id}` — set metadata (official/retro/fan), aggregates (no private progress).  
- `GET /api/v1/albums/templates/{id}` — template + slot descriptors (spoiler-aware), completion stats.  
- `GET /api/v1/saves/{id}` — save metadata (build/region/status), uploader nickname if public, download URL (signed if needed).

**Limits:** Default **60 req/min** per key; typical payload ≤ **1 MB**; pagination cap via cursoring.  
**Failure Modes:**  

    {
      "error": {
        "code": "rate_limited | not_found | forbidden | invalid_query | server_error",
        "message": "human-readable",
        "correlation_id": "uuid"
      }
    }

**Privacy:** Honors visibility; no emails/external IDs; link-only resources require signed URLs.

---
<br>

### 5.3 IGDB Metadata Adapter

**Direction:** Outbound  
**Auth:** Provider key in secrets vault.  
**Protocol:** HTTPS JSON; scheduled pulls (nightly); backoff on errors.

**Contracts:** Sync **games**, **platforms**, **genres**, **release dates**, **alt titles**, **covers** → `Game`, `Variant`, `AltTitle`, `Images`.  
**Limits:** Respect quotas; batch pagination.  
**Failure Modes:** Outage/schema drift → skip cycle, log; UI shows “metadata last updated …”.  
**Privacy:** No user data sent; attribution badge “Metadata: IGDB”.

---
<br>

### 5.4 Steam — OAuth Linking & Official Achievements

**Direction:** Outbound + End-user OAuth  
**Auth:** User OAuth; minimal scopes; unlink anytime.  
**Protocol:** HTTPS JSON; scheduled per user/app sync.

**Contracts:**  
- Link flow stores token ref + user visibility.  
- Sync app schema & user achievement states → `AchSet(type=official)`, `AchProgress`.  
- **Build/Manifest** signals ingested where available (else manual/community entry).

**Limits:** Provider rate limits; staggered 12–24h syncs; manual “Sync now” with cooldown.  
**Failure Modes:** Private profile/outage → mark **stale**; never block fan features.  
**Privacy:** Steam ID never shown unless owner enables panel; no write-back; no anti-cheat-sensitive behavior.

---
<br>

### 5.5 RetroAchievements — Linking & Sets

**Direction:** Outbound  
**Auth:** RA username/API key; unlink anytime.  
**Protocol:** HTTPS JSON; scheduled syncs.

**Contracts:** Import **sets** & **user progress** for mapped titles → `AchSet(type=retro)`, `AchItem`, `AchProgress` (read-only).  
**Limits:** Respect RA quotas.  
**Failure Modes:** API down → keep last good state; mark stale.  
**Privacy:** Panel optional/hidden by default; no write-back.

---
<br>

### 5.6 Evidence Links (YouTube, GameFAQs, etc.)

**Direction:** Outbound (server-side metadata fetch)  
**Auth:** None (public pages).  
**Protocol:** HTTPS; **allowlist** enforced; HEAD/GET with tight caps.

**Contracts:** Store `{ url, provider, title?, thumbnail_url?, meta_expires_at }` with TTL.  
**Limits:** Metadata timeouts ≤ **2s**; size caps; reject non-allowlisted.  
**Failure Modes:** Unreachable/blocked → user-facing guidance to retry or pick allowed host.  
**Privacy:** No client-side third-party calls in secure views.

---
<br>

### 5.7 Storage, CDN & Antivirus

**Direction:** Outbound to S3-compatible storage & CDN  
**Auth:** Access keys in vault; signed URLs for private/link-only.  
**Protocol:** HTTPS; multipart uploads; server-side encryption (KMS).

**Contracts:**  
- **Saves:** binary/blob or zip; DB sidecar with `game_id`, `variant`, **build**, **compat_status**, checksum.  
- **Screenshots:** original + derivatives; EXIF stripped; CDN cached.  
- **Quarantine:** separate bucket/prefix for suspicious files.

**Limits:** Saves ≤ **200 MB**; screenshots ≤ **15 MB**.  
**Failure Modes:** AV down → block uploads; image processor down → serve originals (size-capped).  
**Privacy:** Signed URLs; non-guessable keys; logs omit full private URLs.

---
<br>

### 5.8 Transactional Email Provider

**Direction:** Outbound  
**Auth:** API key; DKIM/SPF.  
**Protocol:** HTTPS JSON.

**Contracts:** UA/EN templates for verification, reset, moderation, appeals, notifications.  
**Limits:** Provider quotas; retries with backoff.  
**Failure Modes:** Persistent failure → in-app only fallback.  
**Privacy:** Minimal PII; unsubscribe only for non-essential emails.

---
<br>

### 5.9 Vision Moderation (Screenshots)

**Direction:** Outbound  
**Auth:** API key or internal service account.  
**Protocol:** HTTPS JSON (or internal RPC).

**Contracts:** Input = thumbnail; Output = labels `nsfw | off_topic | mismatch` + scores for **triage only**.  
**Limits:** Size/time cap; throttle; batch where possible.  
**Failure Modes:** Model down → route to human queue.  
**Privacy:** No training on user content without explicit consent; store decisions, not images.

---
<br>

### 5.10 Inbound Webhooks (Framework)

**Direction:** Inbound  
**Auth:** HMAC signature + IP allowlist; idempotency keys.  
**Protocol:** HTTPS JSON `POST /webhooks/{provider}`.

**Contracts:** Envelope `{ event_type, occurred_at, payload }`; provider-specific mapping done in adapters.  
**Limits:** **2s** processing budget → enqueue job; reply 200 on accept.  
**Failure Modes:** Bad sig/IP → 401/403; retries handled by provider or via our backfills.  
**Privacy:** Payloads redacted in logs; correlation IDs only.

---
<br>

### 5.11 Import / Export Formats

**Direction:** End-user (browser) & Public API (download)  
**Auth:** Session/API key; visibility enforced.  
**Protocol:** HTTPS; binary streams; optional JSON sidecars.

**Contracts:**  
- **Saves (import):** native save extensions or `.zip`; required metadata: `{ game, variant/platform, build? (modern), region/emulator? (retro), short_description }`; AV scan; checksum; initial compat = `untested`.  
- **Saves (export):** original file; optional JSON sidecar `{ game_id, variant_id, builds_tested: [], region, status, sha256 }`.  
- **Screenshots:** accept `png | jpeg | webp`; originals + derivatives; no EXIF; optional alt text.  
- **Evidence links:** allowlist domains; stored minimal metadata; purge when removed upstream.

**Limits:** Size caps as per **5.1 / 5.7**; rate-limits on repeated uploads.  
**Failure Modes:** Invalid format → descriptive error; quarantine on AV suspicion.  
**Privacy:** Respect visibility; signed URLs for non-public assets.

---
<br>

### 5.12 Error & Status Semantics (External)

**Direction:** Policy  
**Auth:** N/A  
**Protocol:** HTTP status + JSON body.

**Contracts:**  
- Success: **200/206** with ETag; **304** on cache hit.  
- Client errors: **400** (invalid query), **401/403** (auth/forbidden), **404** (not found or private), **409** (conflict/idempotency), **429** (rate-limited).  
- Server/provider: **5xx**.  
- Body includes `"correlation_id"` for support.

**Limits:** Error payloads ≤ **2 KB**.  
**Failure Modes:** On upstream outage, return cached/stale with banner where applicable.  
**Privacy:** Do not reveal existence of private resources; neutral error wording.

---
<br>

### 5.13 Security & Privacy at Interface Boundaries

**Direction:** Policy  
**Auth:** Session/OAuth/API keys; rotation for secrets.  
**Protocol:** TLS everywhere; HSTS; modern ciphers; strict **CORS** (no wildcard on credentials); **CSRF** on state-changing browser calls.

**Contracts:** Security headers (CSP, X-Frame-Options, Referrer-Policy).  
**Limits:** WAF + rate limits on public endpoints.  
**Failure Modes:** On anomaly spikes, degrade to stricter rate caps; preserve core read paths.  
**Privacy:** PII minimisation; no emails/external IDs in public API; unlink/delete purges cached external data, aggregates stay anonymised.

---
<br>

### 5.14 Localisation & UA/Russia Policy Surfacing (Interface Rules)

**Direction:** Policy  
**Auth:** N/A  
**Protocol:** Locale negotiation via header/profile.

**Contracts:**  
- **Locales:** EN/UA across UI & public API strings.  
- **Policy surfacing:** UA/Russia flags appear in **Catalogue/Discovery** only (not on profiles), per locale and user toggle.  
- **Toggle:** “Hide Russia-linked” adds a discovery filter; default follows UA locale, optional for EN.

**Limits:** None beyond normal search/filter constraints.  
**Failure Modes:** Missing translations → EN fallback with marker for internal QA.  
**Privacy:** Policy flags are metadata on games/variants, not on users; no geofencing or PII inference.

---
<br>
<br>

## 6. Non-Functional Requirements — System View

> This section defines measurable, testable **system qualities** that apply across modules (Accounts §4.1, Catalogue §4.2, Saves §4.3, Achievements §4.4, Albums §4.5, Profiles §4.6, Moderation §4.7, Discovery §4.8, Integrations §4.9) and across the external interfaces (§5).  
> **ID scheme:** `NFR-<DOMAIN>-NN` (e.g., `NFR-PERF-01`). Each item lists **Statement**, **Rationale**, **Verification**, and **Applies to**.

---
<br>

### 6.0 Reading Guide & Scope
- **Statement** — precise, measurable requirement.  
- **Rationale** — why this matters for Dream Project.  
- **Verification** — how we’ll test/measure it (envs, tools, metrics).  
- **Applies to** — modules/interfaces most affected.  
Cross-cutting: the **version-compatibility layer** (Steam *build/manifest IDs*, retro *region/emulator* facets, statuses `confirmed/untested/legacy/incompatible`) is in scope for performance, consistency and UX guarantees below.

---
<br>

### 6.1 Performance & Capacity (`NFR-PERF-*`)

#### NFR-PERF-01 — API Read Latency
- **Statement:** For **public read** endpoints (games, search, sets, albums, saves, profiles), **p95 ≤ 600 ms** and **p99 ≤ 1,200 ms** under normal load.
- **Rationale:** Keeps discovery and browsing snappy.
- **Verification:** Synthetic probes + prod APM; load tests at projected RPS.
- **Applies to:** §4.8 Discovery, §5.2 API, game/variant pages.

#### NFR-PERF-02 — Page Performance Budgets
- **Statement:** Landing/game/search pages meet **LCP ≤ 2.5 s (p75)**, **CLS ≤ 0.1**, **TTI ≤ 3 s** on mid-range mobile over 4G.
- **Rationale:** Web UX expectations; improves engagement.
- **Verification:** Lighthouse CI, Web Vitals RUM.
- **Applies to:** Web UI (§5.1), Discovery (§4.8).

#### NFR-PERF-03 — Search & Typeahead
- **Statement:** **Typeahead ≤ 150 ms (p95)**; full **search ≤ 700 ms (p95)** with facets; **index propagation ≤ 5 min** after content/visibility changes.
- **Rationale:** Instant feedback and near-real-time freshness.
- **Verification:** APM timers; index lag monitors; CI timings.
- **Applies to:** §4.8, §5.2.

#### NFR-PERF-04 — Upload/Download Throughput
- **Statement:** Support **200 MB** save uploads with **resumable** flows; maintain **≥ 5 parallel uploads** per user without degraded UX; downloads start within **≤ 400 ms TTFB** (cached).
- **Rationale:** Saves are large and critical; interruptions happen.
- **Verification:** Controlled network sims, CDN logs, S3 metrics.
- **Applies to:** §4.3 Saves, §5.7 Storage/CDN, §5.1 Web.

#### NFR-PERF-05 — Background Jobs Freshness
- **Statement:** External sync (Steam/RA) staleness **≤ 24 h**; visibility/moderation changes reflected in listings **≤ 3 min**.
- **Rationale:** Trustworthy data & moderation outcomes.
- **Verification:** Job lag dashboards; change-feed SLOs.
- **Applies to:** §4.4, §4.7, §4.8, §4.9.

---
<br>

### 6.2 Availability & Reliability (`NFR-AVL-*`)

#### NFR-AVL-01 — Service Availability (MVP)
- **Statement:** Monthly **availability ≥ 99.5%** for read paths; write paths **≥ 99.0%**.
- **Rationale:** Reasonable target for an MVP with few operators.
- **Verification:** External uptime monitors; SLO reports.
- **Applies to:** All web/API (§5.1–5.2).

#### NFR-AVL-02 — Graceful Degradation
- **Statement:** On adapter outages (Steam/RA/IGDB), the UI **degrades** with “stale data” banners; fan features stay usable.
- **Rationale:** Keep core value available even when providers fail.
- **Verification:** Chaos tests; feature flags; manual drills.
- **Applies to:** §4.4, §4.9, §5.4–5.5.

#### NFR-AVL-03 — Data Durability (Objects)
- **Statement:** User saves/screenshots stored on **redundant object storage** with **lifecycle policies** and integrity checks (checksums).
- **Rationale:** Content loss is unacceptable.
- **Verification:** Periodic checksum audits; restore tests.
- **Applies to:** §5.7 Storage/CDN.

#### NFR-AVL-04 — Idempotency & Retries
- **Statement:** Webhooks and external calls are **idempotent**; retries use **exponential backoff** with jitter.
- **Rationale:** Prevent duplicates and thundering herds.
- **Verification:** Contract tests; failure injection.
- **Applies to:** §5.10, §4.9.

---
<br>

### 6.3 Security & Privacy (`NFR-SEC-*`)

#### NFR-SEC-01 — Transport Security
- **Statement:** **TLS 1.2+**, **HSTS**, modern ciphers; no plaintext endpoints.
- **Rationale:** Baseline confidentiality/integrity.
- **Verification:** SSL scans; CSP/HSTS checks.
- **Applies to:** All external (§5).

#### NFR-SEC-02 — Encryption at Rest
- **Statement:** DB and object storage encrypted with **KMS-managed keys**; secrets never stored in code; **vault** for credentials.
- **Rationale:** Reduce blast radius if storage is compromised.
- **Verification:** Config audits; key rotation logs.
- **Applies to:** §5.7, adapters §4.9.

#### NFR-SEC-03 — Secrets Hygiene
- **Statement:** API keys/tokens rotated **≤ 90 days**; access is least-privilege; never logged; short-lived env injection.
- **Rationale:** Mitigate key leakage risks.
- **Verification:** Vault audit trails; CI checks for secret leaks.
- **Applies to:** §4.9, §5.4–5.5.

#### NFR-SEC-04 — AppSec Baseline
- **Statement:** Meet **OWASP ASVS L2** intent for inputs, auth, session, CSRF; **file AV scan mandatory** for uploads.
- **Rationale:** Common web threats, plus risky files.
- **Verification:** SAST/DAST; pen-tests; AV quarantine metrics.
- **Applies to:** §5.1, §5.7, §4.3, §4.5.

#### NFR-SEC-05 — Access Control & Audit
- **Statement:** RBAC for Admin/Curator/Moderator; **immutable audit logs** for moderation and admin actions with **≥ 12 months** retention.
- **Rationale:** Accountability and forensics.
- **Verification:** Audit log immutability checks; role tests.
- **Applies to:** §4.7 Moderation, Admin tools.

#### NFR-SEC-06 — Rate Limits & WAF
- **Statement:** Public endpoints protected by **IP/user rate limits** and **WAF** rules; upload/report/contact forms bot-hardened.
- **Rationale:** Abuse and DDoS mitigation.
- **Verification:** Load/abuse tests; WAF dashboards.
- **Applies to:** §5.1–5.2.

#### NFR-SEC-07 — PII Minimisation & RTBF
- **Statement:** Store **minimal PII**; never expose emails/external IDs publicly; on **unlink/delete**, purge cached external data within **30 days** (aggregates remain anonymised).
- **Rationale:** Privacy by design.
- **Verification:** Data flow reviews; deletion job reports.
- **Applies to:** §4.1, §4.6, §4.9, §5.13.

---
<br>

### 6.4 Compliance & Legal (`NFR-LAW-*`)

#### NFR-LAW-01 — Takedown Process
- **Statement:** **DMCA/copyright** flow with temporary disable-on-claim, counter-notice path, and **legal hold** for evidence.
- **Rationale:** Lawful handling of UGC/IP claims.
- **Verification:** Case audits; SLA timers.
- **Applies to:** §4.7, §5.12.

#### NFR-LAW-02 — Data Subject Rights
- **Statement:** Provide **export** and **deletion** capabilities for user data within **30 days** of request; document limits for third-party platforms.
- **Rationale:** GDPR-style expectations.
- **Verification:** Ticket → completion tracking; spot tests.
- **Applies to:** §5.13, §4.9.

---
<br>

### 6.5 Localisation & Internationalisation (`NFR-LOC-*`)

#### NFR-LOC-01 — Language Coverage
- **Statement:** Full **EN/UA** coverage for UI, emails, error messages; **EN fallback** where strings are missing (flagged for fix).
- **Rationale:** Two primary audiences.
- **Verification:** i18n lint; translation completeness reports.
- **Applies to:** §5.1, §5.8.

#### NFR-LOC-02 — Locale Behaviour
- **Statement:** Dates/numbers/timezones respect user locale; search supports UA/EN **transliteration** and **alt titles**.
- **Rationale:** Make finding content natural.
- **Verification:** Search tests; formatting snapshots.
- **Applies to:** §4.8, §5.2.

#### NFR-LOC-03 — UA/Russia Policy Exposure
- **Statement:** UA/Russia flags appear **only** in Catalogue/Discovery with user toggle; **never** on profiles.
- **Rationale:** Policy clarity without user doxxing.
- **Verification:** UI snapshot tests; queries with toggle on/off.
- **Applies to:** §4.2, §4.8, §5.14.

---
<br>

### 6.6 Accessibility & UX Quality (`NFR-UX-*`)

#### NFR-UX-01 — WCAG Target
- **Statement:** Meet **WCAG 2.1 AA** for navigation, contrast, alt text, focus states.
- **Rationale:** Inclusive design baseline.
- **Verification:** Axe CI, manual screen-reader passes.
- **Applies to:** §5.1.

#### NFR-UX-02 — Spoiler Safety
- **Statement:** Spoiler-marked content is **collapsed by default**, with accessible “reveal” control.
- **Rationale:** Protects player experience.
- **Verification:** UI tests; a11y tree checks.
- **Applies to:** §4.4, §4.5, §4.6.

#### NFR-UX-03 — Error Messaging
- **Statement:** Errors are **plain-language**, localised, with actionable next steps; no stack traces to users.
- **Rationale:** Trust and clarity.
- **Verification:** Copy reviews; UX snapshots.
- **Applies to:** All UI.

#### NFR-UX-04 — Mobile Responsiveness
- **Statement:** Core flows (search, view game, upload, view profile) work on **≤ 360 px** width devices.
- **Rationale:** Real-world usage on phones.
- **Verification:** Responsive snapshots; device lab tests.
- **Applies to:** §5.1.

#### NFR-UX-05 — Progress & Feedback
- **Statement:** Long operations (uploads, scans, sync) show **visible progress** and **completion toasts**.
- **Rationale:** Reduce abandonment and duplicates.
- **Verification:** UI integration tests.
- **Applies to:** §4.3, §4.5, §4.9.

---
<br>

### 6.7 Observability & Operations (`NFR-OBS-*`)

#### NFR-OBS-01 — Structured Logging
- **Statement:** All requests/jobs emit structured logs with **correlation IDs**; PII redacted by default.
- **Rationale:** Debugging without leaking data.
- **Verification:** Log samples; redaction tests.
- **Applies to:** All services.

#### NFR-OBS-02 — Metrics & SLOs
- **Statement:** Track **latency, error rate, index lag, quota use, staleness**; publish **SLO dashboards** for PERF/AVL targets.
- **Rationale:** Run the system by numbers.
- **Verification:** Grafana/New Relic dashboards; alert tests.
- **Applies to:** §6.1–6.2, §4.9.

#### NFR-OBS-03 — Tracing
- **Statement:** Distributed tracing on **read & write critical paths** with **≥ 10% sampling**.
- **Rationale:** Pinpoint slow or failing spans.
- **Verification:** Trace store sampling audits.
- **Applies to:** API, jobs, adapters.

#### NFR-OBS-04 — Alerting Hygiene
- **Statement:** Actionable alerts with runbooks; **no alert** without an assigned owner; paging only for SLO/SLA breaches.
- **Rationale:** Keep on-call sane.
- **Verification:** Runbook links; alert reviews.
- **Applies to:** Ops.

---
<br>

### 6.8 Scalability & Maintainability (`NFR-SCL-*`, `NFR-MNT-*`)

#### NFR-SCL-01 — Horizontal Scale
- **Statement:** Stateless API tier scales to **baseline 100 RPS sustained** with 2× headroom; sessions externalised.
- **Rationale:** Predictable growth path.
- **Verification:** Load tests; auto-scale drills.
- **Applies to:** API, Web.

#### NFR-SCL-02 — Search Index Capacity
- **Statement:** Support **≥ 1M** games/variants and **≥ 10M** saves/screenshots indexed with facet counts within **≤ 1 s** render budget.
- **Rationale:** Room for community growth.
- **Verification:** Index sizing tests; facet timing probes.
- **Applies to:** §4.8.

#### NFR-MNT-01 — Code Health
- **Statement:** CI with lint/type checks; **unit test coverage ≥ 70%** on core logic; contracts verified for adapters.
- **Rationale:** Confidence to change.
- **Verification:** CI gates; coverage reports.
- **Applies to:** All repos.

#### NFR-MNT-02 — API Versioning
- **Statement:** Public API versioned (`/api/v1`); **deprecation window ≥ 90 days** with docs.
- **Rationale:** Safe evolution for clients.
- **Verification:** Docs; deprecation headers.
- **Applies to:** §5.2.

---
<br>

### 6.9 Backup, DR & Retention (`NFR-DR-*`, `NFR-RET-*`)

#### NFR-DR-01 — Backups & Restore
- **Statement:** Daily DB backups; **RPO ≤ 24 h**, **RTO ≤ 24 h** (MVP); quarterly restore drills.
- **Rationale:** Recover from bad days.
- **Verification:** Restore runbooks; drill reports.
- **Applies to:** DB, metadata stores.

#### NFR-RET-01 — Content Retention
- **Statement:** Saves archived after **12 months** of inactivity (with notices) per business rules; moderation evidence kept **12 months** unless in legal hold.
- **Rationale:** Storage cost & policy compliance.
- **Verification:** Lifecycle rules; purge job logs.
- **Applies to:** §4.3, §4.7.

#### NFR-DR-02 — Region Strategy
- **Statement:** Store user content in **EU region** by default; CDN edge delivery; document exceptions where providers lack EU regions.
- **Rationale:** Latency & regional expectations.
- **Verification:** Infra config audits.
- **Applies to:** §4.9, §5.7, §5.13.

---
<br>

### 6.10 Data Quality & Consistency (`NFR-DQ-*`)

#### NFR-DQ-01 — Catalogue Integrity
- **Statement:** Enforce unique game/variant keys; manage alt titles & synonyms; RA/Steam IDs mapped with provenance.
- **Rationale:** Clean discovery & deduplication.
- **Verification:** DB constraints; data lint jobs.
- **Applies to:** §4.2, §4.9.

#### NFR-DQ-02 — Compatibility Status Accuracy
- **Statement:** Save **compatibility flips** (e.g., confirmed→legacy) require **evidence** (community threshold or moderator action) and propagate to search within **≤ 5 min**.
- **Rationale:** Users rely on these badges.
- **Verification:** Event→index timers; mod audit logs.
- **Applies to:** §4.3, §4.7, §4.8.

#### NFR-DQ-03 — Eventual Consistency Bounds
- **Statement:** User-visible aggregates (counts, completion rates) converge within **≤ 10 min** of underlying events.
- **Rationale:** Numbers must stabilise quickly.
- **Verification:** Aggregation lag monitors.
- **Applies to:** Profiles §4.6, Albums §4.5, Achievements §4.4.

---
<br>

### 6.11 Interoperability & Adapter Contracts (`NFR-INT-*`)

#### NFR-INT-01 — Timeouts & Circuit Breakers
- **Statement:** External calls time out **≤ 2 s** (metadata) / **≤ 6 s** (bulk sync); breakers trip on error spikes, serving cached data.
- **Rationale:** Keep UX responsive under failures.
- **Verification:** Chaos tests; breaker dashboards.
- **Applies to:** §4.9, §5.3–5.6.

#### NFR-INT-02 — Stubs & Sandboxes
- **Statement:** Every adapter ships **test stubs** with canned responses and error modes for CI.
- **Rationale:** Deterministic tests; no provider coupling.
- **Verification:** CI uses stubs by default.
- **Applies to:** §4.9.

---
<br>

### 6.12 Acceptance & Reporting

- **System must publish** a quarterly **SLO report** (latency, availability, index lag, moderation SLAs) and a **transparency report** for moderation (§4.7).  
- **NFR verification matrix** will map each `NFR-*` to the specific test/monitoring artifacts (Lighthouse runs, load test outputs, APM dashboards, backup drill logs).

---
<br>
<br>

## 7. Data & Storage Considerations

> This section turns the information model (§2 & §4.x) into concrete **storage, indexing, lifecycle, and protection** decisions.  
> **ID scheme for data requirements:** `DS-<AREA>-NN` to aid traceability (e.g., `DS-OBJ-03`). Where relevant, items link back to BRD (§7) and NFRs (§6).

---
<br>

### 7.0 Scope & Reading Guide
- **What’s covered:** relational schema boundaries, object storage for files, search index denormalisation, caching, lifecycle/retention, backups/DR, encryption & privacy, data residency, and data-quality rules.  
- **What’s not:** detailed column types for *every* field (kept in the migration scripts/ERD in Visuals). Below we list the **authoritative tables** and **critical fields** needed to reason about behaviour.

---
<br>

### 7.1 Storage Architecture at a Glance
- **Primary DB:** Relational (e.g., PostgreSQL 15+) for core entities: Users, Roles, Games/Variants, Saves, Achievements, Albums, Moderation, Events.  
- **Object Store:** S3-compatible for **saves** and **screenshots** (originals + derivatives), with **quarantine** prefix.  
- **Search Index:** Denormalised read model (e.g., OpenSearch/Algolia) for **discovery** and **typeahead**.  
- **Cache:** Redis (sessions, rate limits, short-lived query results, idempotency keys).  
- **CDN:** For images/screenshots; signed URLs for private/link-only.  
- **Analytics/Logs:** Structured logs with correlation IDs; aggregated metrics (non-PII).

---
<br>

### 7.2 Core Relational Model (Authoritative Tables)

> The following tables are the **source of truth**; indexes are indicated when performance-critical.

- **users** *(id, email, password_hash, locale, created_at, deleted_at?)*  
  - Indexes: `email unique`, `created_at`.  
  - Visibility prefs kept in `user_settings(user_id, key, value_jsonb)`.

- **roles** / **user_roles** *(user_id, role, since_at)*  
  - Roles: `regular|curator|moderator|admin`.  
  - Keeps **role change history** (`since_at`) rather than mutating in place.

- **external_links** *(id, user_id, provider, token_ref, visibility, last_synced_at)*  
  - Providers: `steam|ra`. Tokens encrypted; no plain external IDs if panel hidden.

- **games** *(id, igdb_id?, title, ua_localised?, ua_developer?, ru_linked?, created_at)*  
- **variants** *(id, game_id, platform, region?, ra_game_id?, steam_app_id?, build_manifest_catalog jsonb, first_seen_at)*  
  - `build_manifest_catalog` stores known Steam build/manifest IDs + provenance.

- **saves** *(id, variant_id, uploader_id, title, description, status, sha256, size_bytes, dlc_flags jsonb, created_at, soft_deleted_at?)*  
  - **status** ∈ `confirmed|untested|legacy|incompatible`.  
  - **compatibility** details live in **save_compat_tests** (see §7.3).  
  - Indexes: `(variant_id, status)`, `created_at desc`, `sha256`.

- **save_versions** *(save_id, vnum, object_key, created_at)* — optional if we expose internal versioning.  
- **save_reports** *(id, save_id, reporter_id, reason, created_at)* — thin link to moderation cases.

- **ach_sets** *(id, variant_id, type, curator_id?, title, description_md, rating_agg, created_at, read_only?)*  
  - **type** ∈ `official|retro|fan`; official/retro are **read-only** (`read_only=true`).  
- **ach_items** *(id, set_id, key, title, criteria_md, flags jsonb, order_index)*  
  - Flags: `storyline|irreversible|final|spoiler_level`.  
- **ach_progress** *(user_id, item_id, state, unlocked_at?, evidence_id?)* with **state** ∈ `locked|in_progress|unlocked`.  
- **ach_evidence_links** *(id, owner_id, url, provider, title?, thumb_url?, approved?)* — allowlisted providers only.

- **album_templates** *(id, variant_id, type, curator_id, title, description_md, slots_count, spoiler_policy, created_at)*  
  - **type** ∈ `new_player|full_discovery|themed`.  
- **album_slots** *(id, template_id, slot_key, short_hint, non_spoiler_hints[], order_index, ai_hints jsonb?)*  
- **album_instances** *(id, template_id, owner_id, created_at, completion_pct)*  
- **slot_fills** *(id, instance_id, slot_id, object_key, taken_at?, approved?, moderation_state, created_at)*

- **moderation_cases** *(id, target_type, target_id, queue, status, created_at, closed_at?)*  
- **mod_actions** *(id, case_id, actor_id, action, reason, created_at, delta_jsonb)* — immutable ledger.  
- **reports** *(id, target_type, target_id, reporter_id, reason_code, note, created_at)*

- **events** *(id, user_id?, type, target_type, target_id, payload jsonb, occurred_at)* — append-only telemetry for aggregates.

> **DS-REL-01:** All tables with user-generated content must have `created_at`, `updated_at`, and **soft-delete** via `*_deleted_at` or status field for reversible actions.

---
<br>

### 7.3 Version-Compatibility Layer (Modern & Retro)

- **save_compat_tests** *(id, save_id, build_manifest_id?, region?, emulator_id?, result, tested_by, evidence_link_id?, tested_at)*  
  - **result** ∈ `works|works_with_warnings|fails`.  
  - Stores **steam build/manifest** where known; for retro: **region (PAL/NTSC)** and **emulator core**.

- **variant_builds** *(variant_id, build_id, source, first_seen_at, last_seen_at?)*  
  - **source** ∈ `auto_steam|manual_admin|community`.  
  - Used to **flip** saves to `legacy` when new builds appear (policy from §4.3).

- **compat_overrides** *(target_type, target_id, new_status, reason_md, actor_id, created_at)*  
  - Moderator/Admin override log (ties to §4.7).

> **DS-COMP-01:** A save’s **public status** is the **most conservative** of: latest successful compat test vs the current variant build, any moderator override, and user-reported failures that reached the threshold (per §4.7).  
> **DS-COMP-02:** When a new `variant_builds` record is added, all **confirmed** saves transition to **`untested`** until retested; nightly job attempts community revalidation.

---
<br>

### 7.4 Object Storage Layout (Saves, Screenshots, Quarantine)

- **Buckets/Prefixes (example):**
*dp-prod-s3/
saves/
{save_id}/orig/{filename}
{save_id}/meta/sidecar.json
{save_id}/archive/{vnum}/{filename}
screenshots/
{slot_fill_id}/orig.{ext}
{slot_fill_id}/thumb.{ext}
quarantine/
{uuid}/{original_filename}*
- **Checksums & dedup:**  
- Compute **sha256** server-side; deduplicate identical payloads by linking multiple save rows to one object key if identical (space saver).  
- **EXIF/PII:** Strip metadata on image ingest; write-protect originals; thumbnails re-derived on demand.  
- **Lifecycle:**  
- Thumbnails cached; **inactivity archive** for saves after **12 months** (see §6.9).  
- Quarantined objects auto-deleted after **N days** unless under legal hold.  
- **Signed URLs:** Generated for private/link-only fetches; short TTLs.

> **DS-OBJ-01:** Every stored object must be referenced by a **DB record**; orphan sweeper runs daily.  
> **DS-OBJ-02:** Quarantine assets **never** served to end users; only safe previews in moderator UI.

---
<br>

### 7.5 Search Index & Denormalisation

- **Indexes per vertical:** `games`, `saves`, `ach_sets`, `albums`, `users`, plus a lightweight `suggest` index.  
- **Projected fields:**  
- Games: `title`, `alt_titles`, `platforms`, `has_saves|sets|albums`, `ua_flags`, `ru_linked`.  
- Saves: `game`, `variant`, **build closeness**, **status**, DLC flags, uploader nickname (if public), popularity.  
- Ach sets: `type (official|retro|fan)`, rating, flags, curator.  
- Albums: `type`, slot count, completion rate.  
- **Freshness SLOs:** visibility/moderation changes ≤ **3 min**; new/updated content ≤ **5 min** (§6.1).  
- **Privacy guard:** only **public projections** are indexed; private/link-only items excluded.

> **DS-IDX-01:** Visibility change events create **suppression tokens** used at query time until the index catches up (dual-layer safety).  
> **DS-IDX-02:** Store **transliteration keys** and **synonym maps** to support UA/EN discovery.

---
<br>

### 7.6 Caching & Sessions
- **Redis** for: session store, rate-limits, CSRF tokens, idempotency keys, ephemeral search results, and “manual sync cooldowns”.  
- **TTL discipline:** short TTLs (≤ 5 min) on discovery caches; token TTLs aligned with security policy.

> **DS-CACHE-01:** No PII in cache values; keys are opaque with namespace prefixes (e.g., `sess:`, `ratel:`, `srch:`).

---
<br>

### 7.7 Data Lifecycle & Retention

- **Saves:**  
- **Archive** after 12 months of no downloads or updates (email notice + in-app banner).  
- **Purge** on owner deletion request unless under moderation/legal hold.  
- **Screenshots:** stored indefinitely unless the album instance is deleted or content is removed via moderation.  
- **Moderation:** reports & evidence retained **12 months** post-closure; longer if legal hold.  
- **External links:** metadata (titles/thumbnails) carried with **TTL**; refreshed on access.

> **DS-LIFE-01:** Lifecycle jobs are **idempotent** and produce audit entries (`events` table).  
> **DS-LIFE-02:** On **unlink** (Steam/RA), user-specific progress is purged within **30 days**; aggregates stay anonymised.

---
<br>

### 7.8 Consistency & Transactions

- **Write paths:** use DB transactions for multi-table changes (e.g., creating a save + initial compat record).  
- **Eventual consistency:** aggregates (ratings, completion %) recomputed asynchronously with **≤ 10 min** convergence target (§6.10).  
- **Idempotency:** uploads, webhooks, and external sync rely on **idempotency keys** to avoid duplicates.

> **DS-CONS-01:** Any operation that affects **visibility** (moderation, privacy change) must be **transactional** and emit a **visibility-change event** to drive index suppression.

---
<br>

### 7.9 Backups & Disaster Recovery

- **DB backups:** daily snapshots; **RPO ≤ 24h**, **RTO ≤ 24h** (MVP); quarterly restore drills (§6.9).  
- **Object store:** provider durability + inventory checks; periodic **checksum audits**.  
- **Runbooks:** documented restore procedures for DB and object prefixes.

> **DS-DR-01:** Backups must **exclude** secrets; restore jobs run in **isolated** environment before promotion.

---
<br>

### 7.10 Security & Privacy at Rest

- **Encryption:** DB volumes & S3 objects encrypted (KMS); per-tenant keys optional later.  
- **Secrets:** Vault-managed; rotation ≤ 90 days (§6.3).  
- **Access control:** strict RBAC on admin/mod tools; immutable audit logs for actions.  
- **PII minimisation:** store only required fields; external IDs hidden unless owner enables panel.

> **DS-SEC-01:** Save files are treated as **untrusted binaries**; must pass AV scan before any processing (thumbnails, etc.).  
> **DS-SEC-02:** Remove **EXIF & GPS** from all user images upon ingest.

---
<br>

### 7.11 Data Residency & Localisation

- **Residency:** Default region **EU** for DB and object store; CDN edges for global delivery; exceptions documented if a provider lacks EU regions.  
- **Localisation data:** UA/Russia policy flags live on **games/variants**, surfaced only in Catalogue/Discovery (never on profiles).

> **DS-LOC-01:** Locale affects **index ranking** and **UI strings**, not underlying stored values (store canonical).

---
<br>

### 7.12 Data Quality & Governance

- **Referential integrity:** FK constraints on all child tables (`variants → games`, `saves → variants`, etc.).  
- **Uniqueness:** `(game_id, platform, region)` unique for variants; `(set_id, key)` unique for ach_items; `(user_id, item_id)` unique for ach_progress.  
- **Validation hooks:** server-side validation for **allowlisted** evidence links; **file-type** sniffing on uploads.  
- **Provenance:** manual/curator/admin overrides require a **reason** and are logged (mods ledger).  
- **Deduplication:** identical `sha256` saves can share physical object; logical rows remain distinct for metadata.

> **DS-GOV-01:** Any schema change must include **migrations**, **backfill scripts**, and an **index impact note** for §4.8.  
> **DS-GOV-02:** Data dictionary (fields, enums, constraints) kept in repo and exported to the ERD in **Visuals**.

---
<br>

### 7.13 Volume & Growth Assumptions (for sizing)

- **Users:** 5–10k in year 1 (MVP target ~500–1k active).  
- **Saves:** 50–200k objects (avg 5–20 MB); long-tail heavy.  
- **Screenshots:** 1–5M images (avg 300–800 KB after derivatives).  
- **Index:** 1M+ game/variant docs via IGDB; saves/albums/sets scale with community.

> **DS-CAP-01:** Partition large tables by **time** (monthly) for `saves`, `slot_fills`, `events`; archive partitions older than 18 months.

---
<br>
<br>

## 8. Operational & Deployment Considerations

> How we run Dream Project in production: environments, CI/CD, infra-as-code, secrets, observability, incident response, moderation ops, cost control, and change management.  
> **ID scheme:** `OP-<AREA>-NN` (e.g., `OP-CI-01`). Each item follows **Intent · Practices · Verification & Owner**.

---
<br>

### 8.0 Scope & Principles

**OP-PR-01 — Operate by SLOs**  
- **Intent:** Align day-to-day ops to the NFR SLOs (§6).  
- **Practices:** Publish dashboards for latency/availability/index-lag/moderation SLA; quarterly SLO review.  
- **Verification & Owner:** SLO review notes and dashboards — **Tech Lead (TL)**, **BA/PO**.

**OP-PR-02 — Everything as Code**  
- **Intent:** Make infra/config reproducible and auditable.  
- **Practices:** IaC for infra; GitOps for config/policies; DB schema migrations in repo; PR reviews mandatory.  
- **Verification & Owner:** Change diffs in PRs; Terraform plan approvals — **DevOps/Infra**, **TL**.

#### 8.0.a Owner Map (MVP)
- **BA/PO:** requirements, docs, policy, data steward duties.  
- **Tech Lead (TL):** architecture, security champion, releases, incident command.  
- **Backend Lead (BE Lead):** APIs, DB, search, integrations, migrations.  
- **Frontend Lead (FE Lead):** web UI, accessibility, performance budgets.  
- **DevOps/Infra (part-time):** CI/CD, IaC, cloud, monitoring, backups.  
- **QA Lead:** test strategy, automation, release validation.  
- **Community/Moderation Lead:** queues, SLAs, appeals, content policy.  
- **Platform Admin (ops persona):** console tasks, user support.  
> Roles can be combined in a lean team (e.g., TL = Security Champion).

---
<br>

### 8.1 Environments & Config
- **Intent:** Isolate risk; keep configuration consistent and reviewable.  
- **Practices:** Separate **dev/staging/prod**; no prod data downstream; central config store (read-only at runtime), **secrets in vault**; feature flags for risky paths.  
- **Verification & Owner:** Env isolation smoke tests; config drift checks — **DevOps/Infra**, **TL**.

---
<br>

### 8.2 CI/CD & Release Strategy
- **Intent:** Ship frequently, safely, and reversibly.  
- **Practices:** CI: lint, SAST, unit/integration tests, container build, SBOM; **blue/green or canary** (10%→50%→100%); auto DB migrations with preflight and rollback plan.  
- **Verification & Owner:** Green CI gates; canary dashboards; rollback drill logs — **DevOps/Infra**, **TL**, **QA Lead**.

---
<br>

### 8.3 Infrastructure & IaC
- **Intent:** Codify infra and enforce defense-in-depth.  
- **Practices:** VPC with private subnets for app/DB; least-privilege security groups; **WAF + CDN** at the edge; **multi-AZ** app and DB replicas (MVP).  
- **Verification & Owner:** Terraform plan reviews; AZ fault drill outcomes — **DevOps/Infra**, **TL**.

---
<br>

### 8.4 Secrets & Keys
- **Intent:** Protect credentials and reduce exposure windows.  
- **Practices:** Vault-managed secrets; rotation ≤ **90 days**; short-lived env injection; never log secrets; provider tokens encrypted at rest.  
- **Verification & Owner:** Rotation report; CI secret scans — **DevOps/Infra**, **TL**.

---
<br>

### 8.5 Observability
- **Intent:** Detect issues fast and trace them end-to-end.  
- **Practices:** Structured logs with **correlation IDs**; metrics for latency/error/index-lag/queue-lag/quota use; distributed tracing **≥10%** sampling; dashboards per SLO.  
- **Verification & Owner:** Dashboards live and alert tests pass — **DevOps/Infra**, **BE Lead**.

---
<br>

### 8.6 Backup, DR & Resilience
- **Intent:** Recover from data loss and ride through provider outages.  
- **Practices:** Daily DB backups (**RPO ≤ 24h**, **RTO ≤ 24h**); object checksum audits; graceful degradation on Steam/RA/IGDB failures (stale markers); feature flags to disable sick adapters.  
- **Verification & Owner:** Quarterly restore drills; chaos tests results — **DevOps/Infra**, **BE Lead**, **TL**.

---
<br>

### 8.7 Incident Response
- **Intent:** Minimise MTTR with clear roles and scripts.  
- **Practices:** Sev ladder (Sev1 data/security, Sev2 SLO breach, Sev3 partial degradation); on-call rotation; runbooks for rollback, stuck queues, reindex, adapter disable, WAF lock-down; blameless postmortems ≤ 5 biz days.  
- **Verification & Owner:** Postmortem docs; runbook links in alerts — **TL (Incident Cmdr)**, **DevOps/Infra**, **QA Lead**.

---
<br>

### 8.8 Data Operations
- **Intent:** Keep data consistent, lawful, and exportable.  
- **Practices:** Zero-downtime reindex (dual-write/snapshot); GDPR-style export/delete within **30 days**; quarterly PII minimisation review.  
- **Verification & Owner:** Ticket SLA logs; reindex metrics — **BA/PO**, **BE Lead**.

---
<br>

### 8.9 Moderation Operations
- **Intent:** Safe community with predictable SLAs.  
- **Practices:** AI triage → human queue; **initial review ≤ 48h** (MVP); immutable moderation ledger; 7-day appeals flow; quarantine never publicly served.  
- **Verification & Owner:** Queue dashboards; sample audits — **Community/Moderation Lead**, **Platform Admin**.

---
<br>

### 8.10 Cost Management
- **Intent:** Avoid surprise bills and scale economically.  
- **Practices:** Cost tags per env/component (api, workers, index, storage, cdn); monthly budget alarms; storage lifecycle (archive inactive saves after **12 months**).  
- **Verification & Owner:** Monthly cost report — **DevOps/Infra**, **BA/PO**.

---
<br>

### 8.11 Security Operations
- **Intent:** Shrink vulnerability windows and access risk.  
- **Practices:** Patch cadence monthly; critical CVEs ≤ **7 days**; SAST/DAST in CI; annual pen-test; quarterly RBAC review; break-glass accounts with hardware keys.  
- **Verification & Owner:** SBOM gates; access audit logs — **TL (Security Champion)**, **DevOps/Infra**.

---
<br>

### 8.12 Release & Feature Toggling
- **Intent:** Launch incrementally and fail safe.  
- **Practices:** Canary with **auto-abort** on p95/error thresholds; kill switches for AI moderation, evidence allowlist, and adapter syncs.  
- **Verification & Owner:** Flag audit; release dashboard — **TL**, **DevOps/Infra**, **QA Lead**.

---
<br>

### 8.13 Maintenance Windows & Status Page
- **Intent:** Keep users informed during planned work.  
- **Practices:** 48h notice in-app; optional read-only mode; public status page (API, uploads, images, search, adapters) with incident history.  
- **Verification & Owner:** Status updates recorded — **Platform Admin**, **DevOps/Infra**.

---
<br>

### 8.14 Access Management (Internal)
- **Intent:** Enforce least privilege with full auditability.  
- **Practices:** Separate **operator** vs **moderator** roles; no direct DB for routine tasks; quarterly RBAC review; break-glass procedure documented.  
- **Verification & Owner:** RBAC review notes; audit trail sampling — **TL**, **Platform Admin**.

---
<br>

### 8.15 Runbooks & Knowledge Base
- **Intent:** Make fixes repeatable under stress.  
- **Practices:** One runbook per critical path (deploy, rollback, reindex, adapter outage, AV spike); quarterly fire-drills; onboarding checklists for on-call/moderators/curators.  
- **Verification & Owner:** Drill notes in repo; checklist completion — **TL**, **Community/Moderation Lead**.

---
<br>

### 8.16 Capacity Planning & Load Testing
- **Intent:** Stay ahead of demand and avoid cliff edges.  
- **Practices:** Quarterly load tests to §6 targets (API **100 RPS** baseline with 2× headroom; 5 parallel uploads/user; search p95 ≤ **700 ms**); shard/replica plan and re-shard playbook for search.  
- **Verification & Owner:** Load test report approved — **BE Lead**, **DevOps/Infra**.

---
<br>

### 8.17 Third-Party Integrations Ops
- **Intent:** Respect quotas/ToS and degrade gracefully.  
- **Practices:** Quota dashboards (Steam/RA/IGDB) with alarms at **70/90%**; auto-stagger jobs; policy watch with auto-disable on ToS change; manual/admin overrides documented.  
- **Verification & Owner:** Adapter health board; policy checklist — **BE Lead (Integrations)**, **TL**.

---
<br>

### 8.18 Change Management
- **Intent:** Make every change reviewable and reversible.  
- **Practices:** PR-based changes with mandatory reviewer; post-release smoke + synthetic checks; prefer roll-forward when safe, with clear rollback path.  
- **Verification & Owner:** Release notes + PR links — **TL**, **QA Lead**, **BA/PO**.

---
<br>
<br>

## 9. Traceability & Mapping to BRD

> This section shows how the **SRS** fulfils the **Business Requirements** from the **BRD**, and where applicable which **Non-Functional Requirements (NFRs)** influence each area.  
> Row-level (requirement-to-requirement) links live in the **Requirements Traceability Matrix (RTM)** in the Documents package. This section is the **module-level map** and the rules for keeping traceability healthy.

---
<br>

### 9.1 Purpose & Policy

- **Forward traceability:** Every SRS requirement `SR-*` must point to one or more BRD business requirements `BR-*`.  
- **Backward traceability:** Every BRD requirement must be implemented or explicitly deferred in the SRS.  
- **NFR hooks:** When an SRS requirement is materially constrained by NFRs, reference the relevant `NFR-*`.  
- **Change control:** Any change in BRD or SRS must update the RTM within the same PR.

> **ID conventions recap:**  
> **BRD**: `BR-<AREA>-NN` (e.g., `BR-ACC-03`).  
> **SRS**: `SR-<MODULE>-NN` (e.g., `SR-ACC-12`).  
> **NFR**: `NFR-<DOMAIN>-NN` (e.g., `NFR-SEC-04`).

---
<br>

### 9.2 Module-Level Mapping to BRD & NFRs

| SRS Module (Section) | SRS ID Prefix | Primary BRD Coverage | Key NFR Influence |
|---|---|---|---|
| **4.1 Accounts & Authentication** | `SR-ACC-*` | `BR-ACC-01..` (registration, login, password reset, 2FA, locale, unlink external) | `NFR-SEC-01..05` (transport, encryption, secrets, RBAC), `NFR-UX-01/03`, `NFR-LOC-01/02` |
| **4.2 Game Catalogue & Localisation Flags** | `SR-CAT-*` | `BR-CAT-01..` (game/variant records, UA flags, RU-linked metadata, pillar availability) | `NFR-DQ-01` (catalogue integrity), `NFR-LOC-03` (policy exposure), `NFR-PERF-03` (search freshness) |
| **4.3 Save Management** | `SR-SAV-*` | `BR-SAV-01..` (upload/download, metadata, versioning, compatibility layer, moderation/report) | `NFR-SEC-04` (AV scan), `NFR-PERF-04` (throughput), `NFR-DQ-02` (compat accuracy), `NFR-RET-01` |
| **4.4 Achievement Tracking** | `SR-ACH-*` | `BR-ACH-01..` (official via Steam, retro via RA, fan sets, evidence, comments/ratings) | `NFR-INT-01/02` (timeouts/stubs), `NFR-UX-02` (spoilers), `NFR-SEC-07` (PII minimisation) |
| **4.5 Sticker Albums** | `SR-ALB-*` | `BR-ALB-01..` (templates, spoiler policies, AI validation triage, completion stats) | `NFR-UX-02` (spoiler safety), `NFR-PERF-04` (uploads), `NFR-SEC-04` (file hygiene) |
| **4.6 Profiles & Progress** | `SR-PRO-*` | `BR-PRO-01..` (profile panels, privacy, aggregates, roles history, curator badges) | `NFR-SEC-07` (privacy/RTBF), `NFR-DQ-03` (aggregate convergence), `NFR-LOC-01/02` |
| **4.7 Moderation & Reporting** | `SR-GOV-*` | `BR-GOV-01..` (reporting, queues, quarantine, appeals, admin overrides) | `NFR-SEC-05` (audit), `NFR-LAW-01` (takedowns), `NFR-AVL-02` (graceful degradation) |
| **4.8 Discovery & Search** | `SR-DSC-*` | `BR-DSC-01..` (search, filters, UA/RU toggles, typeahead, result badges) | `NFR-PERF-01/03` (latency, freshness), `NFR-LOC-02/03`, `NFR-DQ-01/02` |
| **4.9 External Integrations & Adapters** | `SR-INT-*` | `BR-INT-01..` (IGDB, Steam, RA, evidence allowlist, storage/CDN, email, vision triage) | `NFR-INT-01/02`, `NFR-SEC-01..03`, `NFR-AVL-04`, `NFR-OBS-02` |
| **5. External Interface Requirements** | (per interface) | Cross-cuts multiple BR areas (public API & UI constraints for ACC/CAT/SAV/ACH/ALB/DSC) | `NFR-SEC-*`, `NFR-UX-*`, `NFR-PERF-*` |

> **Note:** The **version-compatibility layer** (build/manifest IDs for Steam; region/emulator facets for retro; statuses `confirmed/untested/legacy/incompatible`) threads through **BR-SAV**, **BR-CAT**, **BR-DSC**, and **BR-GOV** and is implemented across `SR-SAV-*`, `SR-CAT-*`, `SR-DSC-*`, `SR-GOV-*`.

---
<br>

### 9.3 Example Row-Level Mappings (excerpt)

> Full RTM has one row per requirement. This excerpt shows the pattern only.

| BRD Requirement | SRS Requirement(s) | Related NFR(s) | Notes |
|---|---|---|---|
| `BR-ACC-03` (Email+Password Login) | `SR-ACC-05` (login), `SR-ACC-06` (lockout), `SR-ACC-07` (CSRF/session) | `NFR-SEC-01/04` | Lockout & CSRF explicitly covered. |
| `BR-SAV-04` (Save Upload w/ Scan) | `SR-SAV-11` (upload), `SR-SAV-24` (AV scan), `SR-SAV-22` (quarantine) | `NFR-SEC-04`, `NFR-PERF-04` | Quarantine never publicly served. |
| `BR-ACH-08` (Fan Sets Evidence) | `SR-ACH-18` (evidence links), `SR-ACH-19` (allowlist) | `NFR-SEC-07`, `NFR-INT-01` | No video upload; links only. |
| `BR-INT-02` (Steam Linking) | `SR-INT-05` (OAuth link), `SR-INT-06` (sync) | `NFR-SEC-03`, `NFR-AVL-02` | Read-only; stale banners on outage. |
| `BR-DSC-03` (UA/RU Toggle) | `SR-DSC-14` (policy filters), `SR-CAT-12` (flags) | `NFR-LOC-03`, `NFR-DQ-01` | Flags only in catalogue/discovery. |

---
<br>

### 9.4 Coverage & Gaps

- **Coverage rule:** RTM must show **100%** of `BR-*` covered by one or more `SR-*`, or explicitly tagged **Deferred** with a target release.  
- **Gap handling:** If a BRD item has no SRS link, flag as **GAP**; if an SRS item has no BRD link, flag as **ORPHAN** (possible scope creep).  
- **NFR links:** Where an NFR materially constrains behaviour (e.g., file size caps, latency), include it in the RTM row.

---
<br>

### 9.5 Change Workflow

1. Update BRD or SRS content.  
2. Update **RTM** rows in the same PR (add/modify links).  
3. CI check verifies: no **GAP** or **ORPHAN** rows; all IDs parse; anchors resolve.  
4. Product/BA reviews impact; Tech Lead reviews technical feasibility.

---
<br>

### 9.6 RTM Location & Format

- **Location (Documents):** `/documents/rtm.md`.  
- **Format:** Markdown table with columns: `BR-ID`, `BR Title`, `SRS IDs`, `NFR IDs`, `Status {Implemented|Deferred}`, `Notes`.  
- **Export:** Optional CSV export for testing tools.

---
<br>

### 9.7 Known Deferrals (MVP)

> If any BRD items are out of MVP scope, list them here to keep the RTM terse.

- *Example placeholder:* `BR-INT-09` (additional modern platforms beyond Steam) → **Deferred** to Phase II; not mapped in SRS MVP.

---
<br>

**Summary:** With this mapping policy, we preserve **bidirectional traceability**: every BRD goal is implemented or explicitly deferred, every SRS behaviour is justified, and key NFRs are attached where they shape the design. The full RTM provides the granular evidence; this section stays as your high-level map.



