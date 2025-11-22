# Project Plan

## Table of Contents

- [1. Introduction & Planning Approach](#1-introduction--planning-approach)  
  What this plan is for, how it will be used, and the delivery approach (lean/agile, MVP-first, read-only integrations).

- [2. Objectives & Success Criteria (Planning View)](#2-objectives--success-criteria-planning-view)  
  The measurable outcomes this plan targets (drawn from the Business Case/BRD) and how progress will be tracked.

- [3. Scope & Deliverables (MVP)](#3-scope--deliverables-mvp)  
  In-scope work packages for MVP, explicit out-of-scope items, and links to the BRD and SRS for detail.

- [4. Work Breakdown Structure (WBS)](#4-work-breakdown-structure-wbs)  
  High-level workstreams and work packages mapped to modules (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT, DATA/OPS).

- [5. Schedule & Milestones](#5-schedule--milestones)  
  Timeline overview with major milestones, sprint/release dates, and critical path notes.

- [6. Iteration & Release Plan](#6-iteration--release-plan)  
  Sprint cadence, release increments (Alpha → Beta → MVP), and acceptance checkpoints.

- [7. Resource & Responsibility Plan](#7-resource--responsibility-plan)  
  Team roles, approximate allocation by phase, and a brief RACI for key activities.

- [8. Budget & Cost Baseline](#8-budget--cost-baseline)  
  Planned budget for MVP (people, cloud, tooling), cost tracking method, and variance thresholds.

- [9. Risk & Assumptions Integration](#9-risk--assumptions-integration)  
  How this plan consumes the Risks & Assumptions Register (IDs, owners, reviews) without duplicating it.

- [10. Quality Management Plan](#10-quality-management-plan)  
  Definition of Done, review gates, test strategy (unit/integration/UAT), and release readiness criteria.

- [11. Communication & Meetings](#11-communication--meetings)  
  Planning-specific ceremonies (stand-ups, sprint reviews, demos) and pointers to the Stakeholder & Communication Plan.

- [12. Change Control & Configuration Management](#12-change-control--configuration-management)  
  How scope changes are proposed, assessed, and approved; branching/versioning and environment controls.

- [13. Procurement & External Services](#13-procurement--external-services)  
  Planning for external APIs (Steam/RA), storage/CDN, AI services; access, quotas, and renewal checkpoints.

- [14. Deployment & Release Management](#14-deployment--release-management)  
  Environments, feature flags, rollbacks, data migrations, and go-live checklists.

- [15. Measurement & Reporting](#15-measurement--reporting)  
  Delivery KPIs (burn-down/up, throughput, lead time), status formats, and dashboards.

- [16. Acceptance & Handover](#16-acceptance--handover)  
  UAT scope, business acceptance criteria mapping, documentation set, and knowledge transfer.

- [17. Post-Launch Support & Operations](#17-postlaunch-support--operations)  
  On-call model, incident response, SLAs/SLOs, and backlog triage after MVP.

- [18. Cross-References](#18-crossreferences)  
  Links to the Business Case, BRD, SRS, Use Cases, Stakeholder Plan, and the Risks & Assumptions Register.

- [19. Change History](#19-change-history)  
  Versioned log of updates to the Project Plan.

---
<br>
<br>

## 1. Introduction & Planning Approach

### 1.1 Purpose of This Plan
The **Project Plan** explains *how* we will deliver the Dream Project MVP: the delivery model, roles, workstreams, milestones, and the way we make trade-offs. It ties the “what” in the **BRD** and **SRS** to a concrete, time-boxed plan and keeps scope, risks, and outcomes aligned.  
See: [Business Case](./business-case.md) · [BRD](./brd.md) · [SRS](./srs.md).

---
<br>

### 1.2 How to Use This Plan
- Treat this as the **operational playbook** for MVP.  
- Each section points to the source artefact rather than duplicating it.  
- When scope/NFRs change, we update this plan **and** the originating doc, keeping links current (see §18 Cross-References).
- Day-to-day risk/assumption status lives in the **Risks & Assumptions Register**; this plan consumes it (see §9).  
See: [Risks & Assumptions Register](./risk-and-assumptions-register.md).

---
<br>

### 1.3 Delivery Approach (Lean, MVP-First)
- **MVP horizon:** deliver a usable web product in ~**4 months**, focusing on three pillars: **Saves**, **Achievements (official + fan)**, **Sticker Albums**.  
- **Agile/Iterative:** two-week sprints, incremental releases (Alpha → Beta → MVP), thin vertical slices per module (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT).  
- **Read-only integrations:** Steam / RetroAchievements are **read-only** (user-consented sync); no write-back in MVP.  
- **Safety & trust first:** moderation tools, evidence allowlist, and privacy/security baselines are in scope from Day 1.  
- **No native mobile in MVP:** mobile-web first; revisit post-MVP if usage justifies.

---
<br>

### 1.4 Planning Principles
- **Value over volume:** prioritise flows that unlock visible user value (upload/browse saves; view/mark achievements; create/fill albums).  
- **Traceability:** every work item maps to a BRD/SRS/UC ID; avoid re-writing requirements inside tasks.  
- **Design for learning:** instrument core flows to validate assumptions quickly (sync adoption, curator throughput, AI triage quality).  
- **Feature-flagged delivery:** ship behind flags, enable gradually, keep rollbacks simple.  
- **Scope discipline:** anything not required for MVP success criteria (in Business Case/BRD) is deferred.

---
<br>

### 1.5 Scope Boundary (Summary)
In scope for MVP:
- Accounts & roles; Game catalogue with **localisation flags** and **UA/RU discovery-only policy banners**;  
- Save Management with **build/manifest & region compatibility labels** (confirmed/untested/legacy/incompatible);  
- Achievement tracking: **official (Steam) sync (read-only)**, **fan sets** (curator-authored, evidence links), **RetroAchievements** profile linkage;  
- Sticker Albums with AI-assisted validation and spoiler handling;  
- Profiles, basic discovery/search, moderation/governance, and minimal ops tooling.

Out of scope for MVP:
- Native mobile apps; write-back to providers; emulator integrations; complex automation for fan-set auto-detection.

(Details in [BRD §4 Scope](./brd.md) and [SRS §4 Functional Requirements](./srs.md).)

---
<br>

### 1.6 Governance & Cadence
- **Sprints:** 2 weeks; planning/review each sprint; monthly roadmap check.  
- **Risk cadence:** weekly register review; Critical risks tracked daily (see §9 & register §8).  
- **Decision rights:** Project Lead decides scope/schedule trade-offs; BA owns traceability; Tech Lead owns architecture/NFRs.  
- **Communication:** stand-ups (daily), demo (bi-weekly), stakeholder digest (weekly).  
See: [Stakeholder & Communication Plan](./stakeholder-and-communication-plan.md).

---
<br>

### 1.7 Planning Assumptions (High-Level)
- Read-only provider sync meets early user expectations; curators can publish initial fan sets; AI triage achieves acceptable agreement rates; storage/egress stays within baseline with lifecycle rules.  
- If any assumption fails validation, we adjust scope/NFRs within one week (per the validation plan).  
See: [Assumptions Log](./risk-and-assumptions-register.md#6-assumptions-log-active).

---
<br>
<br>

## 2. Objectives & Success Criteria (Planning View)

This section translates the *why* (Business Case) into *what we must hit* during delivery. Each objective has measurable success criteria, owners, and where we’ll read the numbers from.

---
<br>

### 2.1 Planning Objectives

- **O1 — Ship a usable MVP on time (4-month horizon).**  
  Deliver Saves, Achievements (official sync + fan sets), and Sticker Albums as working, discoverable flows for real platform users.

- **O2 — Prove early adoption and utility.**  
  Show that people *use* the three pillars (link providers, upload/download saves, complete fan sets, fill albums).

- **O3 — Seed high-quality community content.**  
  Curators publish and maintain enough fan sets and album templates to make the platform worth returning to.

- **O4 — Keep data accurate & trustworthy.**  
  Save compatibility labels and UA/RU discovery-only policy banners are correct and explainable.

- **O5 — Meet baseline NFRs.**  
  Performance, availability, privacy/security, and moderation SLAs match SRS/NFR budgets.

- **O6 — Control cost & delivery risk.**  
  Stay within the MVP budget and reduce High/Critical risks as we approach release.

---
<br>

### 2.2 KPIs & Targets

> Targets are for **MVP + 30 days** unless noted. Owners update weekly in the project digest.

| ID | KPI | Target | Owner | Source of Truth |
|---|---|---|---|---|
| K1 | **MVP schedule** | Code complete by end of Month 4; Beta at Month 3 | Project Lead | Roadmap, sprint burndown |
| K2 | **Monthly Active Users (MAU)** | ≥ **100** MAU; locale mix ≈ **EN 75% / UA 25%** | BA | Product analytics |
| K3 | **Provider links (Steam/RA)** | ≥ **50%** of engaged users link ≥1 provider | BA | Onboarding/link events |
| K4 | **Save uploads** | ≥ **200** uploads; ≥ **300** downloads | Tech Lead | Storage events |
| K5 | **Save compatibility accuracy** | “Didn’t work” confirmations **< 10%** of downloads on *confirmed* variants | Tech Lead | Compatibility service metrics |
| K6 | **Fan achievement completions** | ≥ **400** accepted completions; **≥60%** with acceptable evidence | BA | Achievements service |
| K7 | **Curators (active)** | **≥10** active curators; **≥20** published fan sets | BA | Curator admin |
| K8 | **Sticker albums in use** | **≥50** active album templates across **≥50** games | BA | Album service |
| K9 | **AI sticker triage quality** | Moderator agreement **≥85%**; appeals **<10%** | Tech Lead | Triage QA dashboard |
| K10 | **Moderation SLA** | Median time-to-first-action **≤ 24h**; backlog < 72h | Mod Lead | Mod queue |
| K11 | **Performance** | p95: Catalogue/Search **≤300ms**, Detail pages **≤500ms**, Upload start **≤1s** | Tech Lead | APM/RUM |
| K12 | **Availability** | Web/API **≥99.5%** in Beta and MVP months | Ops Lead | Uptime monitor |
| K13 | **Security/privacy incidents** | **0** P1 incidents; **0** token leaks | Tech Lead | Security logs |
| K14 | **Costs vs budget** | Storage/CDN within **+10%** of baseline; forecast updated monthly | Ops Lead | Cost dashboards |
| K15 | **Risk burn-down** | All Critical risks closed/accepted; High risks have mitigation + contingency | BA | R&A Register |

---
<br>

### 2.3 Phase Exit Criteria (Go/No-Go)

**Alpha (end of Month 2) — “Vertical slice ready”**  
- Accounts/roles, catalogue search, and **one** end-to-end happy path for each pillar:  
  *upload→list→download* (Saves), *sync official→display* (Achievements), *create album→submit sticker→moderation decision* (Albums).  
- Basic moderation queue + evidence allowlist.  
- Initial dashboards for K3–K5, K9, K11.

**Beta (end of Month 3) — “Invite users”**  
- Read-only Steam/RA sync behind feature flags; curator authoring for fan sets; album templates with spoiler options.  
- Save compatibility labels (confirmed/untested/legacy/incompatible) live.  
- NFR budgets instrumented; alert thresholds configured.  
- Open to early adopters/curators; begin measuring K2, K6–K10.

**MVP (end of Month 4) — “Usable in the wild”**  
- All targets K1, K11–K15 at or near green; no open Critical risks; High risks mitigated with contingency.  
- UA/RU **discovery-only** policy banners in catalogue (UA default ON; others OFF but toggleable).  
- Documentation: help pages for evidence examples, save compatibility, and curator guidelines.

---
<br>

### 2.4 Measurement & Reporting

- **Dashboards:** Integrations, Compatibility, Moderation, Albums/Triage, Search/Perf, Costs (as defined in the Risks & Assumptions Register §8).  
- **Cadence:** Daily check for Criticals; **weekly** KPI review in the project digest; **monthly** portfolio view.  
- **Traceability:** Each KPI references BRD/SRS items where behaviour is defined; updates propagate to those artefacts when thresholds change.

---
<br>

### 2.5 Dependencies That Can Move the Goalposts

- Provider reliability & quotas (Steam/RA) affect K3, K6, K11 → mitigated via cooldowns/backoff and “stale” badges.  
- Curator onboarding velocity affects K7–K9 → mitigated via outreach and authoring UX.  
- Storage/CDN pricing changes affect K14 → mitigated via lifecycle rules and caching.

---
<br>
<br>

## 3. Scope & Deliverables (MVP)

This section defines exactly **what we will ship for MVP**, what we **won’t**, and the **deliverables** expected from each work package. It references the BRD/SRS instead of repeating requirements.

---
<br>

### 3.1 In Scope — Work Packages

**WP-ACC — Accounts & Authentication**  
- Email/password + OAuth (where available), sessions, password reset, basic privacy settings.  
- Roles: Regular, Curator, Admin; role-change workflow (application + approval).  
- Cross-refs: BRD §7 (Profile & Roles), SRS §4.1 (`SR-ACC-*`).

**WP-CAT — Game Catalogue & Localisation Flags**  
- Searchable game catalogue with platforms/variants; UA localisation tags; **discovery-only** UA/RU policy banners (UA default ON; others OFF with toggle).  
- Variant facets (e.g., Steam build/manifest, retro region PAL/NTSC).  
- Cross-refs: BRD §6, §10; SRS §4.2 (`SR-CAT-*`).

**WP-SAV — Save Management & Compatibility**  
- Upload/browse/download saves; metadata (title/platform/variant, playtime notes); versioning; AV scan.  
- **Compatibility layer**: `confirmed / untested / legacy / incompatible` via build/manifest + region mapping, community confirmations, admin override.  
- Cross-refs: BRD §6, §10; SRS §4.3 (`SR-SAV-*`).

**WP-ACH — Achievement Tracking (Official + Fan + RA)**  
- **Read-only** sync for official achievements (Steam); profile link to **RetroAchievements** for retro/emulator sets.  
- Fan-set authoring (curators): create/version sets; per-achievement pages (criteria, media, flags), comments/ratings.  
- Fan completion flow (user): manual mark with **evidence link** (allowlisted hosts), review/appeal.  
- Cross-refs: BRD §6–§7; SRS §4.4 (`SR-ACH-*`), §5 (INT).

**WP-ALB — Sticker Albums**  
- Album templates (themes, spoiler levels), slot rules, AI-assisted screenshot triage with *Pending* → accept/deny + appeal.  
- Progressive hints for next slot without heavy spoilers; completion badges + basic completion stats.  
- Cross-refs: BRD §6–§7; SRS §4.5 (`SR-ALB-*`).

**WP-PRO — Profiles & Progress**  
- Public profile (opt-in elements): badges, recent activity, completions; privacy controls.  
- Curator credit on sets/albums authored.  
- Cross-refs: BRD §7; SRS §4.6 (`SR-PRO-*`).

**WP-GOV — Moderation & Governance**  
- Report/queue/appeals; evidence-allowlist admin; rate limits; audit trail.  
- Role tools for Admin/Moderators (bulk actions, priorities).  
- Cross-refs: BRD §7; SRS §4.7 (`SR-GOV-*`).

**WP-DSC — Discovery & Search**  
- Typeahead, filters (platform, variant, tag), sort; surface save compatibility chips and album availability.  
- **Lightweight suggestions** (heuristics/curation); ML recommender is deferred.  
- Cross-refs: BRD §6; SRS §4.8 (`SR-DSC-*`).

**WP-INT — External Integrations & Adapters**  
- Read-only connectors: Steam (official achievements), RetroAchievements (profile).  
- Evidence host allowlist (YouTube, Imgur, etc.) with per-domain rules; provider cooldowns/backoff.  
- Cross-refs: BRD §10; SRS §5 (`SR-INT-*`).

**WP-DATA/OPS — Data, Observability & Operations**  
- Object storage + CDN; lifecycle policies (archive cold saves), image resizing/dedupe.  
- Dashboards/alerts for sync, compatibility, moderation, triage, perf, costs; backups/restore drill.  
- Cross-refs: SRS §7–§8; NFRs §6.

---
<br>

### 3.2 Explicitly Out of Scope (MVP)

- **Write-back** to Steam/RA or any provider; any action that risks ToS violations.  
- **Emulator/proprietary SDK integrations**; game client overlays or memory scanning.  
- **Native mobile apps** (iOS/Android); mobile web only.  
- **Automated fan-achievement detection** on PC/console; manual with evidence only.  
- **Competitions/tournaments, social graph, messaging**, or creator monetisation.  
- **Advanced ML recommendations** (beyond simple heuristics/curation).  
- **Broad localisation** beyond EN/UA.

(See Business Case §5 Scope and BRD §4 for prior agreement.)

---
<br>

### 3.3 Deliverables Checklist (per Work Package)

For each WP above, MVP is **Done** when all of the following are true:

- **Functionality** meets linked BRD/SRS requirements (IDs present in tickets).  
- **UI/UX** has empty/error states, accessibility basics, and copy for “stale/readonly/pending” as applicable.  
- **Security/Privacy**: no secrets in logs; PII minimal and encrypted; allowlist in effect (ACH/GOV).  
- **Moderation/Operations**: dashboards and alerts wired (where relevant); runbook updated.  
- **Docs/Help**: short in-product help for evidence examples, save compatibility labels, curator guidelines.  
- **Feature Flags**: on by role/environment; rollback path defined.  
- **Tests**: unit/integration; happy + error paths covered; basic load check for hot endpoints.

---
<br>

### 3.4 MVP Handover Outputs

- Deployed **web app + API** with feature flags as above.  
- **Admin/Moderator console** (reports, appeals, allowlist).  
- **Dashboards**: Integrations, Compatibility, Moderation, Triage, Search/Perf, Costs.  
- **Knowledge base** pages referenced from UI (short how-tos).  
- **Updated cross-links** in BRD/SRS/UCS and Risks & Assumptions Register.

---
<br>
<br>

## 4. Work Breakdown Structure (WBS)

This section structures MVP delivery into **workstreams** and **work packages (WP)**. Each WP has a short **WBS dictionary** (scope, tasks, dependencies, exit criteria) and keeps tight traceability to BRD/SRS without duplicating requirements.

---
<br>

### 4.1 Workstreams Overview

| WBS ID | Work Package | Scope (short) | Primary Outputs | Key Refs |
|---|---|---|---|---|
| 4.2.1 | **WP-ACC — Accounts & Authentication** | Sign-up/sign-in, roles, privacy | Auth service, role-change flow, settings | BRD §7; SRS §4.1 (`SR-ACC-*`) |
| 4.2.2 | **WP-CAT — Catalogue & Localisation Flags** | Game pages, variants, UA flags, discovery-only policy banners | Searchable catalogue w/ facets & banners | BRD §6/§10; SRS §4.2 (`SR-CAT-*`) |
| 4.2.3 | **WP-SAV — Save Management & Compatibility** | Save uploads/downloads, metadata, compatibility status | Save API/UI + compatibility engine | BRD §6/§10; SRS §4.3 (`SR-SAV-*`) |
| 4.2.4 | **WP-ACH — Achievement Tracking** | Official (Steam) sync, RA link, fan sets | Sync readers, fan-set authoring & completion | BRD §6–§7/§10; SRS §4.4, §5 (`SR-ACH-*`, `SR-INT-*`) |
| 4.2.5 | **WP-ALB — Sticker Albums** | Templates, AI triage, spoiler flow, badges | Album builder, triage pipeline, completion | BRD §6–§7; SRS §4.5 (`SR-ALB-*`) |
| 4.2.6 | **WP-PRO — Profiles & Progress** | Public profile, activity, curator credit | Profile pages + privacy controls | BRD §7; SRS §4.6 (`SR-PRO-*`) |
| 4.2.7 | **WP-GOV — Moderation & Governance** | Reports, queues, appeals, allowlist | Mod console, policies, audit trail | BRD §7; SRS §4.7 (`SR-GOV-*`) |
| 4.2.8 | **WP-DSC — Discovery & Search** | Typeahead, filters, lightweight recs | Search API/UI, catalogue surfacing | BRD §6; SRS §4.8 (`SR-DSC-*`) |
| 4.2.9 | **WP-INT — External Integrations** | Steam/RA adapters, evidence host rules | Read-only connectors, cooldown/backoff | BRD §10; SRS §5 (`SR-INT-*`) |
| 4.2.10 | **WP-DATA/OPS — Data & Operations** | Storage/CDN, observability, backups | Buckets/CDN, dashboards, runbooks | SRS §7–§8; NFRs §6 |

> **Cross-cutting** (applies to all): accessibility basics, i18n (EN/UA), error/empty states, feature flags, instrumentation, and help content.

---
<br>

### 4.2 WBS Dictionary (by Work Package)

#### 4.2.1 WP-ACC — Accounts & Authentication
- **Scope:** Email/password + OAuth (where available), sessions, role-change (Regular→Curator), privacy controls.
- **Key Tasks**
  - 4.2.1.1 Auth service & DB schemas (users, sessions, roles).
  - 4.2.1.2 OAuth providers (scoped; config-driven enable/disable).
  - 4.2.1.3 Role-change workflow (application form, review, approval).
  - 4.2.1.4 Security hardening (rate limits, multi-factor optional, password reset).
  - 4.2.1.5 Privacy settings (profile visibility, evidence link redaction in public).
  - 4.2.1.6 UX: forms, errors, empty states, email templates.
  - 4.2.1.7 Telemetry & audit (sign-in attempts, role changes).
- **Dependencies:** DATA/OPS (secrets, email service); GOV (audit policy).
- **Exit Criteria:** All flows tested (happy/error); audit trail on role-change; no secrets in logs.

#### 4.2.2 WP-CAT — Catalogue & Localisation Flags
- **Scope:** Game catalogue with **variants** (Steam build/manifest, retro PAL/NTSC), UA localisation tags, **discovery-only** UA/RU policy banners.
- **Key Tasks**
  - 4.2.2.1 Catalogue schema (games, platforms, variants, tags).
  - 4.2.2.2 Import pipeline (IGDB-like + curated overrides for UA flags/policy).
  - 4.2.2.3 Variant facet UI + detail page; banner toggles (UA default ON; others OFF).
  - 4.2.2.4 Admin override tools (flags, banners, variant metadata).
  - 4.2.2.5 Search index (title, aliases, platform, tags).
  - 4.2.2.6 Instrumentation (toggle usage, mis-flag reports).
- **Dependencies:** INT (metadata sources), DSC (indexing), GOV (policy copy).
- **Exit Criteria:** Searchable catalogue; accurate flags on seed set; toggleable banners with analytics.

#### 4.2.3 WP-SAV — Save Management & Compatibility
- **Scope:** Upload/browse/download saves; metadata; versioning; AV scan; **compatibility status** (`confirmed/untested/legacy/incompatible`).
- **Key Tasks**
  - 4.2.3.1 Object storage integration + AV sandbox.
  - 4.2.3.2 Save metadata form & extractor (game/platform/variant notes).
  - 4.2.3.3 Versioning (retain N versions, archive policy).
  - 4.2.3.4 **Compatibility engine**: build/manifest mapping, region rules, community confirmations, admin override.
  - 4.2.3.5 UI chips & filters for status; community confirm/“didn’t work” flow.
  - 4.2.3.6 Download safeguards (warnings on legacy/incompatible).
  - 4.2.3.7 Instrumentation (confirm time, failure rate).
- **Dependencies:** CAT (variants), INT (Steam build IDs), DATA/OPS (storage, costs), GOV (reports).
- **Exit Criteria:** E2E upload→list→download works; compatibility labels present; failure rate < target on confirmed variants (see KPIs).

#### 4.2.4 WP-ACH — Achievement Tracking
- **Scope:** **Read-only** official sync (Steam), **RetroAchievements** profile link, fan-set authoring & completion with evidence links.
- **Key Tasks**
  - 4.2.4.1 Steam reader (user-consented, quotas, cooldowns).
  - 4.2.4.2 RA profile linkage (read-only identity & badge pull).
  - 4.2.4.3 Fan-set authoring UI (create/version, per-achievement pages, flags).
  - 4.2.4.4 Comments/ratings; moderation hooks.
  - 4.2.4.5 Completion form with **allowlisted evidence** preview.
  - 4.2.4.6 Review/appeal workflow; curator tools.
  - 4.2.4.7 Telemetry (sync success, completions, appeals).
- **Dependencies:** INT (Steam/RA), GOV (evidence allowlist, reviews), PRO (profile badges/credits).
- **Exit Criteria:** Official achievements visible; fan sets publishable; completions with evidence review live.

#### 4.2.5 WP-ALB — Sticker Albums
- **Scope:** Album templates (themes, spoiler levels), submission flow, AI-assisted triage, appeals, completion badges & stats.
- **Key Tasks**
  - 4.2.5.1 Template builder (slots, rules, spoiler settings, hints).
  - 4.2.5.2 Submission UI (drag & drop, metadata, spoiler protection).
  - 4.2.5.3 **AI triage** service (thresholds, pending path).
  - 4.2.5.4 Moderator review & appeal flow.
  - 4.2.5.5 Completion logic & badges; per-album stats.
  - 4.2.5.6 Progressive hints for next slot (non-spoiler).
  - 4.2.5.7 Telemetry (agreement %, appeals).
- **Dependencies:** DATA/OPS (image processing/CDN), GOV (queues), PRO (badges), DSC (surface albums).
- **Exit Criteria:** Create→submit→decision works; AI agreement ≥ target on sample; badges/stats visible.

#### 4.2.6 WP-PRO — Profiles & Progress
- **Scope:** Public profile (opt-in elements), activity, curator credit, privacy controls.
- **Key Tasks**
  - 4.2.6.1 Profile schema & privacy flags.
  - 4.2.6.2 Profile UI (achievements, albums, badges).
  - 4.2.6.3 Curator credit surfaces (sets/albums authored).
  - 4.2.6.4 Activity feed (recent actions; rate-limited).
  - 4.2.6.5 Settings for visibility and evidence link redaction.
- **Dependencies:** ACC (roles), ACH/ALB (data), GOV (policy), DATA/OPS (caching).
- **Exit Criteria:** Profile renders without PII leaks; privacy toggles effective.

#### 4.2.7 WP-GOV — Moderation & Governance
- **Scope:** Reports, queues, bulk actions, appeals, allowlist/domain rules, audit.
- **Key Tasks**
  - 4.2.7.1 Report endpoints & UI hooks (saves, achievements, screenshots).
  - 4.2.7.2 Moderation console (priorities, bulk actions, SLAs).
  - 4.2.7.3 Evidence **allowlist** management (per-domain rules).
  - 4.2.7.4 Appeals handling & audit trail.
  - 4.2.7.5 Rate-limits, abuse prevention, blocked users.
- **Dependencies:** ACH/ALB/SAV (report sources), DATA/OPS (dashboards), LEGAL (takedown copy).
- **Exit Criteria:** End-to-end report→decision flow; audit export; SLA metrics visible.

#### 4.2.8 WP-DSC — Discovery & Search
- **Scope:** Typeahead, filters, sort; surface compatibility chips & album availability; **heuristic** suggestions.
- **Key Tasks**
  - 4.2.8.1 Search API + index build (games, variants, tags).
  - 4.2.8.2 Typeahead UI; filters (platform, variant, UA tag).
  - 4.2.8.3 Result cards (compatibility chips, album/fan-set presence).
  - 4.2.8.4 Suggestions (curated/heuristic); ML recommender deferred.
  - 4.2.8.5 Perf budgets & caching; analytics events.
- **Dependencies:** CAT (index source), SAV/ALB/ACH (badges/chips), DATA/OPS (APM/RUM).
- **Exit Criteria:** p95 ≤300ms typeahead; relevant surfacing; analytics wired.

#### 4.2.9 WP-INT — External Integrations & Adapters
- **Scope:** Read-only adapters for Steam/RA; evidence host allowlist; cooldown/backoff.
- **Key Tasks**
  - 4.2.9.1 Provider client libs (auth, headers, rate policies).
  - 4.2.9.2 Sync workers (queues, retries, cooldowns).
  - 4.2.9.3 Evidence host allowlist + preview handlers.
  - 4.2.9.4 Provider status/quotas dashboard & alerts.
- **Dependencies:** DATA/OPS (queues, secrets), GOV (policy), ACH (consumers).
- **Exit Criteria:** Compliant read-only sync; no write-back; dashboard shows health.

#### 4.2.10 WP-DATA/OPS — Data & Operations
- **Scope:** Buckets/CDN, lifecycle rules, image processing, observability, backups/restore.
- **Key Tasks**
  - 4.2.10.1 Buckets & CDN provisioning; lifecycle (archive cold saves).
  - 4.2.10.2 Image pipeline (resize, dedupe by checksum).
  - 4.2.10.3 Dashboards (Integrations, Compatibility, Moderation, Triage, Search/Perf, Costs).
  - 4.2.10.4 Alerts & runbooks (thresholds per Risks & Assumptions §8).
  - 4.2.10.5 Backups & restore drill; incident playbooks; secrets management.
- **Dependencies:** All WPs.
- **Exit Criteria:** Dashboards live; alerts firing to owners; restore drill passed; costs within baseline.

---
<br>

### 4.3 Cross-Cutting Tasks (apply to all WPs)

- 4.3.1 Accessibility & internationalisation (EN/UA copy, basic ARIA, keyboard flow).  
- 4.3.2 Error/empty-state patterns (stale sync, pending review, legacy/incompatible saves).  
- 4.3.3 Feature flagging & rollout plans (by role/env).  
- 4.3.4 Instrumentation taxonomy (events, params, PII rules).  
- 4.3.5 Short help pages (evidence examples, compatibility labels, curator guidelines).  
- 4.3.6 Security & privacy hardening (rate limits, headers, secret scanning).

---
<br>

### 4.4 WBS Notes

- **Estimation:** tracked per WP as T-shirt sizes (S/M/L/XL) in the backlog; refined in sprint planning.  
- **Traceability:** each task links to BRD/SRS/UC IDs; avoid requirement text in tickets.  
- **Done =** functional + UX + NFR hooks + monitoring + docs + flags + tests (see §3.3 Deliverables Checklist).

---
<br>
<br>

## 5. Schedule & Milestones

A 4-month roadmap from project start (**Month 1 → Month 4**) with sprint cadence (**2 weeks**). Dates are expressed as **M#W#** so the plan is portable; insert actual calendar dates when the start date is fixed.

---
<br>

### 5.1 Timeline Overview (by Sprint)

| Sprint | Window (relative) | Focus (primary WPs) | Key Outcomes |
|---|---|---|---|
| **S1** | M1W1–W2 | **DATA/OPS**, **ACC**, **CAT** (schema), **INT** (provider spikes) | Environments, buckets/CDN, auth skeleton, catalogue schema & import spike, Steam/RA read-only spike, basic dashboards wired. |
| **S2** | M1W3–W4 | **ACC**, **CAT**, **SAV** (storage), **GOV** (reports) | Sign-up/in + sessions; searchable game list (seed set); object storage + AV scan; report hooks + mod queue stub. |
| **S3** | M2W1–W2 | **SAV** (upload/list/download), **CAT** (variants), **DSC** (typeahead) | End-to-end saves; variant facets (build/manifest, PAL/NTSC); p95 targets set for search/typeahead. |
| **S4** | M2W3–W4 | **ALB** (templates, submission), **ACH** (Steam reader), **GOV** (console v1) | Album builder + submission; read-only official achievements visible; moderation console v1. **→ Alpha Gate** |
| **S5** | M3W1–W2 | **ACH** (fan sets authoring), **ALB** (AI triage shadow), **SAV** (compatibility engine) | Fan-set authoring; AI triage in shadow mode; compatibility statuses (confirmed/untested/legacy/incompatible). |
| **S6** | M3W3–W4 | **PRO** (profiles), **INT** (RA link), **DSC** (surface chips/albums), **NFR** baselines | Public profiles (opt-in); RA linkage; discovery surfaces compatibility chips & album presence; perf/uptime alert thresholds live. **→ Beta Gate** |
| **S7** | M4W1–W2 | **GOV** (appeals/allowlist), **ALB** (appeals), **ACH** (evidence flow), **CAT** (UA/RU banners) | Evidence allowlist with previews; appeal paths; UA/RU **discovery-only** policy banners (UA default ON; others OFF, toggleable). |
| **S8** | M4W3–W4 | **Hardening & Docs** (help pages), **DATA/OPS** (restore drill), **Flags & Rollback**, **UAT** | Help content (evidence examples, compatibility labels, curator guide); backup/restore drill passed; feature flags set; UAT + Go/No-Go. **→ MVP Release** |

> Cross-cutting each sprint: instrumentation, accessibility basics, empty/error states, security hardening, copy polish.

---
<br>

### 5.2 Milestones & Exit Criteria

- **M1 Checkpoint — Foundations in place**  
  *Exit criteria:* environments + storage/CDN live; auth skeleton; catalogue schema/import path; provider spikes complete; initial dashboards visible.

- **Alpha (end of M2) — Vertical slices working**  
  *Exit criteria:*  
  - Saves: upload → list → download (happy path)  
  - Achievements: read-only official sync visible per user  
  - Albums: create template → submit screenshot → moderator decision  
  - Moderation console v1; search/typeahead online with budgets set

- **Beta (end of M3) — Invite early users/curators**  
  *Exit criteria:*  
  - Fan-set authoring & publication; RA profile link  
  - Save **compatibility** labels live + user confirmations  
  - AI triage in active use with manual review path  
  - NFR dashboards/alerts configured; top risks tracked

- **MVP (end of M4) — Usable in the wild**  
  *Exit criteria:*  
  - No **Critical** risks open; **High** risks mitigated with contingency  
  - KPIs trending to green (perf p95, availability, moderation SLA)  
  - UA/RU catalogue banners live (scoped to discovery only)  
  - Help pages in UI; restore drill passed; release notes prepared

---
<br>

### 5.3 Critical Path & Dependencies

1. **DATA/OPS provisioning → ACC** (auth & secrets)  
2. **CAT variants** (build/manifest, PAL/NTSC) **→ SAV compatibility engine**  
3. **INT Steam/RA** (read-only clients) **→ ACH official view & RA link**  
4. **ALB submission & triage** **→ GOV appeals & allowlist**  
5. **DSC indexing** depends on **CAT**, surfaces status from **SAV/ALB/ACH**

> Any slip on 2–4 threatens Alpha/Beta readiness; contingency is to **feature-flag** affected panels and proceed with other slices.

---
<br>

### 5.4 Buffers & Contingency

- **Integration buffer:** 1 week across S5–S6 for provider quirks/quotas; UI shows **stale** badges if sync lags.  
- **Moderation capacity buffer:** volunteer moderators added before S7; throttle new submissions if queues exceed SLA.  
- **Performance buffer:** S6 includes tuning window for p95 targets on search/detail/upload.  
- **Release buffer:** final hardening (S8) reserves time for docs, flags, rollback testing, and UAT fixes.

---
<br>

### 5.5 Release Gates (Go/No-Go Inputs)

- **Alpha/Beta/MVP checklists** draw from: risk register (High/Critical), NFR dashboards, UAT results, restore drill outcome, and doc readiness.  
- **Decision maker:** Project Lead (with BA, Tech Lead, Ops/Mod leads present).  
- **If No-Go:** scope trims via flags (defer AI auto-accept, keep manual path; restrict catalogue banners to fewer surfaces; narrow save variant coverage to confirmed-only).

---
<br>

### 5.6 Artefacts Produced per Milestone

- **Alpha:** working slices demo, updated SRS traceability notes, initial runbooks.  
- **Beta:** curator guide, evidence examples, triage tuning report, KPI dashboard links.  
- **MVP:** release notes, help pages, admin/mod handbook, backup/restore report, post-launch on-call rota.

---
<br>
<br>

## 6. Iteration & Release Plan

This section explains **how we iterate** (ceremonies, increments, quality bars) and **how we release** (flags, gates, rollbacks). It aligns with the 4-month schedule in §5 and references the BRD/SRS without duplicating requirements.

---
<br>

### 6.1 Cadence & Ceremonies

- **Sprint length:** 2 weeks (M1W1 → M4W4).
- **Daily stand-up (15m):** blockers, plan for the day, risk/alert highlights.
- **Sprint planning (90m):** select stories from the prioritised backlog; every story maps to BRD/SRS/UC IDs.
- **Backlog refinement (60m / week):** break down upcoming work; confirm acceptance tests & NFR hooks.
- **Sprint review (60m):** demo the working slice to stakeholders; record decisions and flagged follow-ups.
- **Sprint retro (45m):** one improvement we’ll keep, one we’ll change next sprint.
- **Weekly risk sync (30m):** review High/Critical risks & overdue assumptions (ties to the Register §8).

Artifacts updated each sprint: **burndown**, **increment demo notes**, **DoD checklist**, **risk notes**.

---
<br>

### 6.2 Increment Plan (Alpha → Beta → MVP)

**Alpha (end of Month 2)** — *Vertical slices working*  
- **Scope:** One full happy path per pillar (Saves, Achievements, Albums) plus basic moderation and search.  
- **Acceptance checklist:**
  - Upload→list→download (Saves) works on seed titles; AV scan active.
  - Read-only official achievements visible for linked accounts.
  - Album template→submit screenshot→moderator decision works; AI triage in **shadow** mode.
  - Moderation console v1; search/typeahead online; p95 budgets defined.
  - Feature flags in place; basic dashboards live.
- **Stakeholder demo:** live walk-through of all three pillars.
- **Gate:** **Go** if checklists pass and no Critical risks block usage.

**Beta (end of Month 3)** — *Invite early users/curators*  
- **Scope:** Fan-set authoring & publication; RA profile link; compatibility status for saves; AI triage active (with manual path).  
- **Acceptance checklist:**
  - Fan sets: create/version/publish; per-achievement pages (criteria, media, flags).
  - Evidence allowlist enforced; reviewer/appeal flow in GOV.
  - Save **compatibility**: confirmed/untested/legacy/incompatible chips + community confirmations.
  - NFR dashboards & alert thresholds configured; top risks tracked with contingencies.
- **Stakeholder demo:** curator authoring, evidence review, compatibility confirmations.
- **Gate:** **Go** if High risks have mitigation & contingency; KPIs trending toward §2 targets.

**MVP (end of Month 4)** — *Usable in the wild*  
- **Scope:** Production-ready versions of Beta features + UA/RU discovery-only catalogue banners; help content; restore drill; flags/rollback tested.  
- **Acceptance checklist:**
  - KPIs: perf p95 targets met; availability ≥99.5%; mod SLA median ≤24h.
  - No Critical risks; High risks mitigated with contingency.
  - Help pages (evidence examples, compatibility labels, curator guidelines) published and linked from UI.
  - Backup/restore drill passed; release notes prepared.
- **Gate:** Project Lead decision with BA/Tech/Ops/Mod leads present. If **No-Go**, enable scope trims via flags (see §5.5).

---
<br>

### 6.3 Backlog Management & Prioritisation

- **Source of truth:** single backlog tagged by module (ACC/CAT/SAV/ACH/ALB/PRO/GOV/DSC/INT/DATA-OPS).  
- **Prioritisation:** MoSCoW + risk reduction + KPI impact; anything not needed for §2 KPIs is deferred.  
- **Story format:** “As a \<role\> I can \<behaviour\> so that \<value\>” + acceptance tests + NFR hooks + traceability IDs.  
- **Definition of Ready (DoR):** acceptance tests, UX/error states, flags, instrumentation events identified.

---
<br>

### 6.4 Release Management & Feature Flags

- **Environment flow:** Dev → Staging (flagged) → Prod (flag-controlled rollout).  
- **Flags:** per feature/panel; role-aware (curator/admin) and environment-aware.  
- **Canary:** 5–10% traffic for new surfaces; widen based on KPIs and error rates.  
- **Rollbacks:** deploys are reversible; DB migrations use **expand-migrate-contract** pattern.  
- **Copy & banners:** “stale” badges for delayed sync; warnings for legacy/incompatible saves; discovery-only UA/RU banners in catalogue.

---
<br>

### 6.5 Testing Strategy by Iteration

- **Alpha:** unit + service integration tests for core flows; smoke tests; seed data for demos.  
- **Beta:** broaden integration tests; add contract tests for Steam/RA; governance tests (report/appeal); performance baselines.  
- **MVP:** regression suite; load checks on search/upload/triage; security checks (secret scanning, headers, rate limits); restore drill.

> QA notes: cover **happy + error + empty states**; include evidence-link previews, allowlist rejections, legacy/incompatible warnings.

---
<br>

### 6.6 UAT Plan

- **Participants:** early adopters (UA & EN), curators, moderators.  
- **Scope:** end-to-end user journeys:  
  1) search title → download compatible save;  
  2) link provider → view official achievements;  
  3) complete fan achievement with evidence;  
  4) create template → submit stickers → get decisions.  
- **Acceptance capture:** structured checklist per journey; defects triaged within 24h.  
- **Exit:** all Critical defects fixed; High defects have workarounds or are feature-flagged off.

---
<br>

### 6.7 Documentation & Enablement

- **User-facing:** short help pages linked from UI (evidence examples, compatibility labels, curator guidelines).  
- **Internal:** runbooks for alerts, moderation handbook, release/rollback steps.  
- **Updates:** docs are part of the DoD for each work package (see §3.3).

---
<br>

### 6.8 Telemetry, KPIs & Reporting

- **Events:** consistent taxonomy across modules (no PII; pseudonymised IDs).  
- **Dashboards:** Integrations, Compatibility, Moderation, Triage, Search/Perf, Costs (as in the Risks & Assumptions Register §8).  
- **Reporting:** weekly digest summarises KPI movement vs §2 targets and risk changes.

---
<br>

### 6.9 Exit & Rollback Criteria

- **Exit** an increment when the acceptance checklist passes and gates are signed.  
- **Rollback** a feature when error rate, KPI regression, or risk severity crosses a documented threshold; open a follow-up ticket to address root cause before re-enabling.

---
<br>
<br>

## 7. Resource & Responsibility Plan

This section defines **who does what**, when they do it, and how we keep ownership clear without bloating headcount. It aligns staffing to the WBS (§4) and the schedule (§5).

---
<br>

### 7.1 Team Roles & Core Responsibilities

- **Project Lead (PL)** — delivery owner, scope/schedule trade-offs, release Go/No-Go, vendor/legal escalations.  
- **Business Analyst / Product (BA)** — requirements & traceability, acceptance criteria, KPI definition, curator outreach, register owner (risks/assumptions).  
- **Tech Lead / Architect (TL)** — system design, module interfaces, NFR budgets, code review standards, security/privacy guardrails.  
- **Backend Engineer (BE)** — API/services, data models, integrations (Steam/RA adapters with TL), save compatibility engine.  
- **Frontend Engineer (FE)** — Web UI, accessibility basics, instrumentation hooks, feature flags/rollouts.  
- **QA / Test Engineer (QA)** — test strategy, automation where sensible, UAT support, exit criteria evidence.  
- **UX/UI Designer (UX)** — low-fi wireframes, error/empty states, copy with BA, help pages.  
- **Ops / DevOps (OPS)** — environments, object storage/CDN, observability dashboards, backups/restore drill, cost tracking.  
- **Moderation Lead (MOD)** — report queues, evidence allowlist policy, appeals, moderator onboarding.  
- **Administrator (ADM)** — admin tools, policy switches (per-title save availability, banner overrides), access control.

> Note: “Curators” and “Volunteer Moderators” are **external contributors** (not headcount). Their onboarding and recognition are driven by BA + MOD.

---
<br>

### 7.2 Staffing by Phase (FTE Estimates)

> FTE = full-time equivalent per month. Small, Ukraine-based junior-leaning team with one senior TL/PL.

| Role | M1 | M2 | M3 | M4 | Notes |
|---|:---:|:---:|:---:|:---:|---|
| Project Lead (PL) | 0.4 | 0.4 | 0.5 | 0.6 | Heavier near release gates. |
| BA / Product | 0.6 | 0.8 | 0.8 | 0.7 | Curator outreach peaks M2–M3. |
| Tech Lead (TL) | 0.8 | 0.8 | 0.8 | 0.7 | Architecture & reviews; pairs on tricky items. |
| Backend Engineer (BE) | 1.0 | 1.0 | 1.0 | 1.0 | Services, adapters, compatibility engine. |
| Frontend Engineer (FE) | 1.0 | 1.0 | 1.0 | 1.0 | All modules’ UI; flags; instrumentation. |
| QA / Test | 0.3 | 0.5 | 0.8 | 1.0 | Automation grows; UAT & hardening in M4. |
| UX/UI | 0.5 | 0.5 | 0.4 | 0.3 | Wireframes → polish; help pages in M4. |
| Ops / DevOps | 0.5 | 0.5 | 0.6 | 0.7 | Dashboards, alerts, restore drill before MVP. |
| Moderation Lead (MOD) | 0.2 | 0.3 | 0.5 | 0.6 | Recruit volunteers; refine queues & SLAs. |
| Administrator (ADM) | 0.2 | 0.3 | 0.4 | 0.4 | Admin tools, policy toggles, overrides. |

**Assumptions**
- Volunteers: **Moderators (3–5)** from Month 3; **Curators (≥10)** by Month 3 (see BRD targets).  
- Contingency: TL can temporarily cover BE; BA can cover ADM policy setup; OPS can assist with simple QA env tasks.

---
<br>

### 7.3 RACI — Key Activities (Responsible / Accountable / Consulted / Informed)

| Activity | PL | BA | TL | BE | FE | QA | UX | OPS | MOD | ADM |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Requirements & traceability (BRD→SRS→UC) | I | **A/R** | C | C | C | C | C | I | I | I |
| System architecture & NFR budgets | I | C | **A/R** | C | C | C | I | C | I | I |
| Catalogue & localisation flags; UA/RU discovery-only banners | I | **A/R** | C | C | C | C | C | I | I | **C** |
| Save compatibility engine (build/manifest, PAL/NTSC, labels) | I | C | **A** | **R** | C | C | I | C | I | C |
| Steam reader (read-only), RA linkage | I | C | **A** | **R** | C | C | I | C | I | I |
| Fan-set authoring & evidence flow | I | **A/R** | C | C | **R** | C | C | I | **C** | C |
| Sticker albums & AI triage thresholds | I | C | **A** | C | **R** | C | **C** | I | **C** | I |
| Moderation console, allowlist policy, appeals | I | C | C | C | C | C | I | I | **A/R** | **C** |
| Discovery & search surfacing | I | C | **A** | C | **R** | C | C | I | I | I |
| Observability dashboards, alerts, restore drill | I | I | C | C | C | C | I | **A/R** | I | I |
| Security & privacy guardrails | I | C | **A** | **R** | **R** | C | I | **R** | I | I |
| Release mgmt (flags, rollback, gates) | **A** | C | **R** | R | R | **C** | I | **R** | I | I |
| UAT & acceptance sign-off | **A** | **R** | C | C | C | **R** | C | I | I | I |
| Curator & moderator onboarding | I | **A/R** | I | I | I | I | I | I | **R** | I |

Legend: **A** = Accountable · **R** = Responsible · **C** = Consulted · **I** = Informed

---
<br>

### 7.4 Time, Coverage & Collaboration Rules

- **Core hours:** overlap 10:00–17:00 EET for stand-ups, reviews, pairing.  
- **Code reviews:** at least **1 TL + 1 peer** for critical paths (INT, SAV, GOV).  
- **Design handoff:** UX → FE with annotated wireframes + copy; FE raises gaps early.  
- **Testing:** QA defines acceptance tests **before** implementation; FE/BE add instrumentation as part of DoD.  
- **Docs:** short help pages (evidence examples, compatibility labels, curator guidelines) are part of each WP’s DoD.

---
<br>

### 7.5 On-Call, Incidents & Release Support

- **Pre-MVP (M4):** light on-call rotation (TL/OPS primary, BE/FE backup); incident severity playbook tied to dashboards.  
- **Rollbacks:** TL/OPS can revert within 15–30 minutes; migrations use expand-migrate-contract.  
- **User comms:** BA owns status copy; MOD prepares moderation macros; PL approves public notes if needed.

---
<br>

### 7.6 External Contributors (Curators & Volunteer Moderators)

- **Curators:** recruited via outreach posts + RA community; BA screens; MOD/BA onboard with authoring guide; recognition via profile badges/credits.  
- **Volunteer moderators:** invited from early users; MOD trains on console; limited permissions; SLA expectations documented.  
- **Tools:** curator authoring (ACH), album template builder (ALB), moderation console (GOV); all behind role flags.

---
<br>

### 7.7 Cross-References

- Stakeholder & Communication Plan — ceremonies, channels, escalation:  
  [`./stakeholder-and-communication-plan.md`](./stakeholder-and-communication-plan.md)  
- BRD — objectives, acceptance, dependencies:  
  [`./brd.md`](./brd.md)  
- SRS — module responsibilities & NFRs:  
  [`./srs.md`](./srs.md)  
- Risks & Assumptions Register — ownership of high/critical items:  
  [`./risk-and-assumptions-register.md`](./risk-and-assumptions-register.md)

---
<br>
<br>

## 8. Budget & Cost Baseline

A pragmatic MVP budget based on a **small, Ukraine-based team** (junior-leaning with one senior TL/PL), **4 months** of delivery, and the scope in §3–§4. This plan tracks **People**, **Cloud/Tools**, and a **contingency** buffer.

> Source alignment: see Business Case — *§11 Financial & Effort Overview*  
> [`./business-case.md`](./business-case.md)

---
<br>

### 8.1 Summary (4-month MVP)

| Category | Estimated Cost (USD) | Notes |
|---|---:|---|
| **People** | **$34,650** | Based on §7 FTEs × monthly role rates. |
| **Cloud & Tools** | **$1,220** | Object storage/CDN, AV scan, monitoring/APM, email, domain/SSL, light compute for image processing. |
| **Contingency (15%)** | **$5,380** | Applied to People + Cloud to cover slips, provider quirks, minor scope padding. |
| **Total (MVP)** | **$41,250** | Target baseline (rounding applied). |

> Guardrail: **±10%** variance allowed at the work-package level; anything beyond triggers scope or schedule trade-offs (§12).

---
<br>

### 8.2 Assumptions Behind the Numbers

- **Team composition:** exactly as in §7 (PL, BA, TL, BE, FE, QA, UX, OPS, MOD, ADM) with the monthly FTE profile shown there.  
- **Rates:** junior/mid UA market; one senior TL/PL at modest senior rate.  
- **Contributors:** **Curators** and **volunteer moderators** are unpaid contributors (recognition via badges/credits).  
- **Integrations:** **read-only** Steam/RetroAchievements; no paid API tiers assumed for MVP.  
- **Cloud choices:** commodity object storage + CDN, low-tier APM/monitoring, basic AV/sandbox scanning.

---
<br>

### 8.3 People Cost Breakdown (rolled-up by month)

| Month | Blend of FTEs (see §7.2) | Est. People Cost |
|---|---|---:|
| **M1** | Foundations (DATA/OPS, ACC, CAT) | **$7,900** |
| **M2** | Alpha vertical slices (SAV/ACH/ALB) | **$8,475** |
| **M3** | Beta features (PRO/INT/compatibility) | **$9,110** |
| **M4** | Hardening, UAT, release | **$9,155** |
| **Total** |  | **$34,650** |

*(Rounded to the nearest \$5 for readability.)*

---
<br>

### 8.4 Cloud & Tools Detail (baseline)

| Item | Unit Assumption | Monthly | 4-Month Total | Notes |
|---|---|---:|---:|---|
| Object storage + CDN | ~150–250 GB stored; modest egress | $150 | **$600** | Lifecycle rules keep costs in check. |
| AV / sandbox scan | Low-volume file checks | $50 | **$200** | For save uploads only. |
| Monitoring/APM | Starter tier | $40 | **$160** | APM + uptime. |
| Email/SMS (auth, notices) | Low send volume | $20 | **$80** | Transactional only. |
| Image processing compute | Resizing/dedupe | $30 | **$120** | Batch jobs; no GPU. |
| Domain & SSL | 1 domain + cert | $15 | **$60** | Annualised monthly. |
| **Subtotal** |  |  | **$1,220** |  |

> If sponsorships/credits are available, apply to **storage/CDN** first (largest lever).

---
<br>

### 8.5 Tracking & Variance Rules

- **Baseline vs Actuals:** tracked **monthly**; show burn vs plan in the §15 reporting dashboard.  
- **Variance thresholds:**  
  - *People:* ±10% at WP level → review scope/sequence; >10% → escalate and rebalance.  
  - *Cloud:* >**+10% MoM** vs plan → tighten quotas/lifecycle, defer hi-res previews, widen caching.  
- **Change control:** any change that pushes the total above **$45k** requires Project Lead approval with options (trim, defer, or add funding).

---
<br>

### 8.6 Cost Controls & Levers

- **Feature flags** to temporarily disable expensive panels (e.g., hi-res previews, heavy search facets).  
- **Lifecycle policies** (archive cold saves after 60–90 days; user-visible but cold storage).  
- **CDN caching** and image **dedupe** by checksum to reduce egress and storage.  
- **Queues & cooldowns** to smooth provider/API usage and avoid quota spikes.  
- **Volunteer capacity**: recruit moderators before S7 to keep review SLAs without adding headcount.

---
<br>

### 8.7 Risks to Budget & Mitigations

- **Storage/egress spikes** from popularity → use quotas, archive rules, and seek credits (see Risk RISK-006).  
- **Integration churn** (Steam/RA limits) → build backoff/cooldowns; avoid paid tiers in MVP (RISK-001).  
- **Moderation backlog** leading to paid help → recruit volunteers; add bulk tools (RISK-005).  
- **Performance misses** causing extra infra → cache aggressively; defer non-critical panels (RISK-012).

---
<br>

### 8.8 Exit Criteria (Budget Perspective)

MVP budget is considered **on target** when:
- Total MVP spend **≤ \$41.25k + 10%**;  
- All §2 KPIs are trending to green;  
- No Critical risks open; High risks have mitigation & contingency;  
- Dashboards show storage/egress within plan; restore drill passed.

---
<br>
<br>

## 9. Risk & Assumptions Integration

This section explains **how the Project Plan consumes the Risks & Assumptions Register** without duplicating it, and how outcomes from that register change scope, schedule, and release decisions.

> Primary source: **Risks & Assumptions Register**  
> [./risk-and-assumptions-register.md](./risk-and-assumptions-register.md)

---
<br>

### 9.1 What Lives Where

- **Register (source of truth):** scoring model, active risks, closed/accepted risks, active assumptions, validation plan, monitoring/alerts.  
  - See: [§3 Scoring Model](./risk-and-assumptions-register.md#3-scoring-model--categories),  
    [§4 Risks (Active)](./risk-and-assumptions-register.md#4-risk-register-active),  
    [§6 Assumptions (Active)](./risk-and-assumptions-register.md#6-assumptions-log-active),  
    [§7 Validation Plan](./risk-and-assumptions-register.md#7-assumptions-validation-plan),  
    [§8 Monitoring & Reporting](./risk-and-assumptions-register.md#8-monitoring--reporting-cadence).
- **This plan:** consumes the register to **sequence work**, **gate releases**, and **apply contingency/flags**.

---
<br>

### 9.2 Weekly Operating Loop

| When | Input | Action | Output/Owner |
|---|---|---|---|
| **Mon** | High/Critical risks; overdue validations | Re-score, confirm owners/dates; adjust sprint scope if needed | Updated register (BA), sprint change note (PL) |
| **Wed** | Dashboards (sync, compatibility, triage, perf, costs) | Compare vs thresholds; open tasks/flags | Task tickets with BRD/SRS/UC links (TL/OPS) |
| **Fri** | Digest draft | Summarise changes, decisions needed | 1-page digest with IDs & due dates (BA) |

Escalation rules as in the register [§2.5](./risk-and-assumptions-register.md#25-escalation-rules).

---
<br>

### 9.3 Release Gates Consume the Register

**Alpha/Beta/MVP** gates in §5 read:

- **Open risks:** No **Critical** open; **High** must have mitigation + contingency (see [§4](./risk-and-assumptions-register.md#4-risk-register-active)).  
- **Assumptions:** Any **Low-confidence / High-consequence** item past due in [§7](./risk-and-assumptions-register.md#7-assumptions-validation-plan) blocks the gate unless a scope flag removes the dependency.  
- **Alerts:** No red KPIs from [§8](./risk-and-assumptions-register.md#8-monitoring--reporting-cadence) on perf/availability/moderation at the gate window.

Gate decisions are recorded in **§5.5** and the register’s [§10 Change History](./risk-and-assumptions-register.md#10-change-history).

---
<br>

### 9.4 Traceability & IDs (No Duplication)

- Every change in scope/schedule references **risk/assumption IDs** (e.g., `RISK-002`, `ASM-003`) and upstream **BRD/SRS/UC IDs**.  
- Tickets include: `Why now` → link to the register card → acceptance tests/NFR hooks tied to SRS.  
- Cross-link hygiene checklist follows register [§9.4](./risk-and-assumptions-register.md#94-link-hygiene-checklist).

---
<br>

### 9.5 What We Do When a Risk Escalates

1. **Re-score** (BA + owner) using the model in [§3](./risk-and-assumptions-register.md#3-scoring-model--categories).  
2. **Choose one**: mitigate now, accept (with rationale), transfer, or defer behind a **feature flag**.  
3. **Plan changes** in this order:  
   - **Flags:** disable risky panel/surface; keep other slices on track.  
   - **Sequence:** swap sprint items to reduce exposure.  
   - **Scope:** trim Nice-to-have stories; never duplicate requirements—reference BRD/SRS.  
4. **Communicate:** add to weekly digest; update gate readiness.

---
<br>

### 9.6 What We Do When an Assumption Fails

- Mark **Invalidated (False)** in [§6](./risk-and-assumptions-register.md#6-assumptions-log-active).  
- Within **one week**, produce options with trade-offs (copy/UX tweaks, flag a dependency, widen manual path, or adjust NFR budgets).  
- Update BRD/SRS/UCs and this plan **only by reference** (IDs + short rationale), keeping the register as the narrative.

---
<br>

### 9.7 Register Items Most Likely to Move the Plan

- **RISK-002** (save compatibility mapping) — can **defer variant coverage** behind flags if confirmations lag.  
- **RISK-004** (AI triage accuracy) — keep **manual review** path; throttle auto-accept until agreement ≥ target.  
- **RISK-006** (storage/egress costs) — apply quotas/lifecycle; **defer hi-res previews**.  
- **ASM-002** (curator output) — if unmet, **seed internal sets** and prioritise RA mapping.

(See full cards in [§4/§6](./risk-and-assumptions-register.md#4-risk-register-active).)

---
<br>

### 9.8 Ownership & RACI Touchpoints

- **Register Owner:** **BA** (curates, triages, publishes digest).  
- **Decision maker for trade-offs:** **Project Lead** (scope/schedule).  
- **Technical mitigations/NFR tuning:** **Tech Lead** (+ OPS/MOD domain owners).  
- **Evidence/allowlist & community health:** **Moderation Lead**.

RACI details align with §7.3 of this plan.

---
<br>

### 9.9 Reporting Artifacts (Produced by This Plan)

- **Weekly one-pager**: deltas to risks/assumptions, KPIs vs targets, decisions needed.  
- **Gate checklist** addendum: register snapshot and actions taken since last gate.  
- **Change log**: links back to register [§10](./risk-and-assumptions-register.md#10-change-history) and this plan’s §19.

---
<br>
<br>

## 10. Quality Management Plan

This plan defines **what “good” looks like** for Dream Project MVP and **how we prove it**. It ties together gates, test layers, acceptance evidence, and the non-functional budgets defined in the SRS.

---
<br>

### 10.1 Quality Objectives

- Deliver a **usable MVP in 4 months** with three pillars (Saves, Achievements, Albums) that real users can rely on.  
- Keep trust high: **accurate save compatibility labels**, **safe evidence links**, and **clear discovery-only policy banners**.  
- Meet baseline **NFRs** (performance, availability, security/privacy, moderation SLA) and keep costs within the budget baseline.  
- Provide crisp **observability** and **runbooks** so issues are found fast and resolved safely.

Cross-refs: BRD §11 (Acceptance), SRS §6 (System NFRs), Risks & Assumptions Register §8 (Monitoring).

---
<br>

### 10.2 Definition of Done (DoD)

A story/work item is **Done** when all apply:

1. **Functionality**: Behaviour matches SRS/UC IDs; happy path and key error/empty states implemented.  
2. **UX & Copy**: Forms, hints, warnings present (e.g., *stale sync*, *legacy/incompatible save*).  
3. **Accessibility & i18n**: Keyboard navigation, basic ARIA roles; EN/UA copy covered; UA/RU banners only in discovery as per policy.  
4. **Security/Privacy**: No secrets/PII in logs; evidence links restricted to allowlisted domains; rates/headers enforced.  
5. **Instrumentation**: Events named per taxonomy; dashboards receive data.  
6. **Testing**: Unit + integration tests; e2e for critical flows; acceptance tests green.  
7. **Docs**: Help snippet (if user-facing), runbook note (if operational), short admin/mod note (if relevant).  
8. **Flags & Rollback**: Feature behind a flag with a clear rollback path.

---
<br>

### 10.3 Review Gates & Readiness Checklists

**Alpha (end of M2) — Vertical slices**  
- Saves: upload→list→download; AV scan.  
- Achievements: **read-only** official sync visible per user.  
- Albums: template→submit→moderator decision (AI in **shadow**).  
- Moderation console v1; typeahead online; p95 budgets defined.  
- Early dashboards live (sync, moderation, perf).

**Beta (end of M3) — Early users/curators**  
- Fan-set authoring & publication; RA profile link.  
- Save **compatibility** labels active with confirmations.  
- AI triage active (manual path exists); allowlist enforced + appeals.  
- NFR dashboards + alert thresholds configured; no open **Critical** risks.

**MVP (end of M4) — Public-ready**  
- KPIs trending green: perf p95, availability ≥99.5%, moderation median ≤24h.  
- UA/RU banners limited to catalogue/discovery; toggles behave as specified.  
- Help pages published; backup/restore drill passed; flags/rollback tested.

Gate evidence: demo notes, test summaries, dashboard screenshots, and a short gate report signed by PL.

---
<br>

### 10.4 Testing Strategy (Layers)

- **Unit tests**: core logic (compatibility mapping, evidence validators, triage thresholds).  
- **Service/Integration tests**: adapters (Steam/RA) with contract tests and safe fixtures; storage, queues, AV scan.  
- **End-to-End (e2e)**: user journeys—save upload/download; link provider→view achievements; submit fan completion with evidence; create album→submit→decision.  
- **Non-functional**:  
  - **Performance**: p95 targets—typeahead ≤300ms; detail pages ≤500ms; upload start ≤1s.  
  - **Security**: secret scanning, dependency audit, headers/rate-limit checks.  
  - **Privacy**: log scrubbing tests; PII minimisation checks.  
- **UAT**: curated UA/EN cohort (users/curators/moderators) runs scripted checklists; defects triaged within 24h.

Test data: seeded games/variants (incl. PAL/NTSC, Steam build IDs), dummy accounts/tokens; no real secrets.

---
<br>

### 10.5 Defect Triage & SLAs

| Severity | Example | SLA (Beta/MVP windows) | Action |
|---|---|---|---|
| **P1 – Blocker** | Data loss, auth bypass, PII leak | Fix or rollback **same day** | Stop-ship unless mitigated |
| **P2 – High** | Core flow broken for many; wrong save label on confirmed | Fix in **48h** or flag off | Hotfix preferred |
| **P3 – Medium** | Non-core bug, UI glitch, copy error | Next sprint | Bundle with module work |
| **P4 – Low** | Polish, minor visual | Backlog | When capacity allows |

Bug bar for gates: **No P1/P2 open**; P3 must have workarounds or be flagged off.

---
<br>

### 10.6 Code Quality & CI

- **Reviews**: 1 TL + 1 peer on INT/SAV/GOV; at least 1 peer elsewhere.  
- **Static checks**: linters/formatters; secret scan in CI; dependency audit.  
- **Coverage**: meaningful coverage on critical paths; e2e present for the four core journeys.  
- **Branching**: trunk-based with short-lived branches; CI gates on tests + checks.  
- **Artifacts**: test reports and coverage published per build.

---
<br>

### 10.7 Accessibility & Localisation QA

- Keyboard-only flows, focus states, and contrast checks on key pages.  
- Alt text on images; labels on inputs; ARIA for dynamic content.  
- UA/EN copy reviewed; **policy banners are discovery-only** and default per locale (UA ON; others OFF, toggleable).

---
<br>

### 10.8 Security & Privacy Assurance

- **Read-only** provider integrations; no write-back.  
- KMS-backed secrets; least-privileged tokens; rotation procedure documented.  
- Evidence **allowlist** enforced with per-domain rules; preview sanitisation.  
- PII minimised and encrypted; export/delete supported where applicable.  
- Logs scrubbed; no tokens/PII; AV scan active for uploads.

---
<br>

### 10.9 Performance & Availability Acceptance

- Meets p95 targets (search/typeahead, detail views, upload start).  
- Availability ≥**99.5%** for Beta and MVP months.  
- No red alerts in the week before each gate; runbooks tested (on-call ready).

---
<br>

### 10.10 Observability & Runbooks

- Dashboards: Integrations, Compatibility, Moderation, Triage, Search/Perf, Costs—**populated and reviewed weekly**.  
- Alerts: thresholds per Risks Register; paging to named owners.  
- Runbooks: incident steps for provider outage, moderation overload, cost spikes, and performance regressions.  
- **Restore drill** passed before MVP (evidence recorded).

---
<br>

### 10.11 Documentation Quality

- **User help**: evidence examples, compatibility labels explainer, curator guidelines (linked in UI).  
- **Internal**: moderation handbook, allowlist policy, release/rollback steps, mapping/admin overrides guide.  
- Versioned and accessible from the admin console and repo wiki.

---
<br>

### 10.12 Acceptance Evidence & Sign-off

- Evidence bundle per gate: demo notes, test summaries, defect list with statuses, KPI screenshots, and risk snapshot.  
- **Sign-off**: BA confirms scope/acceptance; TL confirms NFRs; PL signs gate decision.  
- All items cross-linked to BRD/SRS/UC IDs; changes recorded in this plan’s Change History (§19).

---
<br>
<br>

## 11. Communication & Meetings

This section defines **who meets when, why, and with what inputs/outputs**. It complements the Stakeholder & Communication Plan (channels, stakeholders, escalation paths) without duplicating it.  
See: [./stakeholder-and-communication-plan.md](./stakeholder-and-communication-plan.md)

---
<br>

### 11.1 Objectives

- Keep delivery aligned with **scope, KPIs, and risks**.
- Make decisions **traceable** (DRIs, outcomes, links to BRD/SRS/UC IDs).
- Minimise time in meetings; favour **asynchronous** updates where possible.

---
<br>

### 11.2 Cadence Overview

| Rhythm | Meeting | Purpose | Core Attendees | Inputs | Outputs |
|---|---|---|---|---|---|
| **Daily (15m)** | **Stand-up** | Surface blockers; plan the day; flag alerts/risks. | PL, TL, BE, FE, QA, BA, OPS, MOD | Board, alert snapshots | Blockers list; owner & ETA |
| **Weekly (30m)** | **Risk & KPI Sync** | Review High/Critical risks and KPI deltas. | BA (chair), PL, TL, OPS, MOD | Risk register §8 dashboards | Updated owners/dates; actions |
| **Bi-weekly (90m)** | **Sprint Planning** | Select stories; confirm acceptance/NFR hooks. | PL, BA, TL, BE, FE, QA, UX | Prioritised backlog; capacity | Sprint goal; committed list |
| **Bi-weekly (60m)** | **Sprint Review / Demo** | Show working slices; collect decisions. | All core + invited stakeholders | Increment; demo notes | Decisions; follow-ups |
| **Bi-weekly (45m)** | **Retrospective** | Improve process; address recurring issues. | Delivery team | Retro board | 1 keep / 1 change for next sprint |
| **Weekly (async)** | **Stakeholder Digest** | Share concise status, risks, asks. | BA → all stakeholders | KPI deltas; risks; plan | 1-pager with links & DRIs |
| **Per gate (60m)** | **Go/No-Go** | Decide Alpha/Beta/MVP release. | PL (chair), BA, TL, OPS, MOD | Gate checklist; risk snapshot | Go/No-Go; mitigation plan |
| **Ad-hoc (≤30m)** | **Design/Tech Huddles** | Resolve specific design/tech questions fast. | Relevant DRIs only | Design sketches; spikes | Decision note; ticket links |
| **UAT windows** | **UAT Check-ins** | Track journey coverage & defects. | BA, QA, FE/BE reps | UAT sheet; defect log | Fix list; ship/flag decisions |

Time zone: **EET** with overlap 10:00–17:00.

---
<br>

### 11.3 Agendas & I/O (lightweight)

**Stand-up (15m)**  
- *Agenda:* Yesterday / Today / Blockers (alerts/risks first).  
- *Output:* Named owner + ETA per blocker.

**Risk & KPI Sync (30m)**  
- *Agenda:* Top 3 risks → score/owner/date, KPI reds, decisions needed.  
- *Output:* Updated register entries; tasks linked to BRD/SRS/UC IDs.

**Sprint Planning (90m)**  
- *Agenda:* Capacity → pick stories → acceptance tests & NFR hooks → flags/instrumentation noted.  
- *Output:* Committed sprint; traceability verified.

**Review / Demo (60m)**  
- *Agenda:* Short demos by module (SAV/ACH/ALB/…); decisions & risks captured live.  
- *Output:* Decision notes; follow-ups.

**Retro (45m)**  
- *Agenda:* What helped? What hurt? One change we’ll try next.  
- *Output:* Action items for next sprint.

**Go/No-Go (60m)**  
- *Agenda:* Gate checklist, open High/Critical risks, rollback readiness.  
- *Output:* Decision + mitigation/flag plan.

---
<br>

### 11.4 Channels & Artefacts

- **Async status:** weekly digest (BA) with links to dashboards & risk IDs.  
- **Issues & Docs:** single backlog (stories → BRD/SRS/UC IDs), repo wiki for help/runbooks.  
- **Dashboards:** Integrations, Compatibility, Moderation, Triage, Search/Perf, Costs (owners per Risks Register §8).  
- **Decision records:** lightweight “Decision Note” attached to the ticket or wiki (see template below).  
- **Escalation:** follow Stakeholder Plan rules; Critical → PL immediately, then weekly digest.  

---
<br>

### 11.5 Templates (copy-paste)

**Decision Note (≤5 bullets)**
~~~
Decision: <one sentence>
Context: <why now / options considered>
Owner (DRI): <name>
Impacts: <BRD/SRS/UC IDs; flags; risks>
Review date: <yyyy-mm-dd>
~~~

**Weekly Digest (max 1 page)**
~~~
This week: <done / in-flight>
KPIs: <greens / ambers / reds with short notes>
Risks: <top 3 with IDs and actions>
Asks: <decisions needed / unblockers>
Next: <focus for the coming week>
Links: <dashboards, register, BRD/SRS/UC sections>
~~~

---
<br>

### 11.6 Participation Rules

- **Be brief, link out.** Meetings are decision accelerators, not status monologues.  
- **Right room, right time.** Ad-hoc huddles for specifics; avoid dragging the full team.  
- **Trace everything.** Decisions get a note; tasks get IDs; risks get owners/dates.  
- **Camera optional, clarity mandatory.** Summaries posted within the hour for Review/Go-No-Go.

---
<br>
<br>

## 12. Change Control & Configuration Management

This section defines **how changes are proposed, assessed, approved, implemented, and traced** without duplicating requirements from BRD/SRS. It also sets the rules for **branching, versioning, environments, flags, and migrations**.

---
<br>

### 12.1 Goals & Scope

- Keep MVP delivery **predictable** while allowing safe iteration.
- Ensure every change is **traceable** to BRD/SRS/UC IDs and to risks/assumptions where relevant.
- Protect user trust with **tested rollbacks**, guarded secrets, and auditable history.

**Applies to:** code, infrastructure config, feature flags, data schema, allowlists/policy toggles, dashboards/thresholds, and user-facing copy that changes behaviour or legal meaning.

Cross-refs: BRD (§12 Traceability), SRS (§8 Ops, §6 NFRs), Risks & Assumptions Register (§9 Cross-References).

---
<br>

### 12.2 Change Classes

| Class | Examples | Approval | Validation |
|---|---|---|---|
| **Standard** | Minor UI copy, non-functional refactors, docs | 1 peer reviewer | CI green + smoke |
| **Normal** | Feature work in WBS, NFR tweaks, new dashboards | PR: 1 peer + **TL** (or delegate) | CI + e2e on affected journeys |
| **Config/Policy** | Evidence allowlist updates, UA/RU banner overrides, rate limits | **MOD** (policy) or **ADM** (system) + visibility to BA | Staging test + audit entry |
| **Schema/Data** | DB migrations, index changes | **TL** + **OPS** | Expand–migrate–contract runbook + rollback tested |
| **Emergency** | Hotfix for P1 (security, PII, data loss) | **PL** or **TL** (one approver) | Post-fix retro + backfill tests within 48h |

> If unsure, treat as **Normal**.

---
<br>

### 12.3 Request → Decision → Implementation (RACI)

1. **Raise a Change** (ticket)
   - **Content:** purpose, scope, risk/assumption IDs, BRD/SRS/UC links, test plan, rollback plan.
   - **R:** Requestor (any core role) · **A:** BA for scope mapping.

2. **Triage & Impact**
   - Check **dependencies** (Schedule §5, WBS §4) and **risks** (Register §4/§6).
   - **R:** BA + TL · **A:** PL (if schedule/goalposts move).

3. **Approval**
   - As per **Class** (12.2). For cross-module impact, PL signs off.

4. **Build & Review**
   - Trunk-based development with short-lived branches; PR includes **traceability IDs** and test evidence.
   - **R:** BE/FE · **A/C:** TL/QA.

5. **Test & Stage**
   - CI green; staging deploy with feature **flags OFF**; run e2e and smoke; update runbook if needed.
   - **R:** QA/OPS.

6. **Release & Monitor**
   - Progressive rollout (flags/canary 5–10% → 100%); dashboards watched (Register §8).
   - **R:** OPS/TL · **A:** PL for gates.

7. **Rollback (if needed)**
   - Revert code/flag; for schema use **expand–migrate–contract** rollback path.
   - **R:** OPS/TL; incident note filed; Register updated.

---
<br>

### 12.4 Source Control & Branching

- **Model:** trunk-based; feature branches kept < 5 days.
- **PR Review:** at least **1 TL + 1 peer** for INT/SAV/GOV; ≥1 peer elsewhere.
- **Commit hygiene:** reference IDs (e.g., `BR-ACH-05`, `SR-SAV-26`, `UC-ALB-03`, `RISK-002`).
- **Tags:** `v<major>.<minor>.<patch>` on releases; release notes stored in repo (`/docs/releases/MVP-YYYY-MM-DD.md`).

---
<br>

### 12.5 Versioning Rules

- **App Releases:** semantic tags as above; **release notes** include changes, flags, migrations, and known risks.
- **APIs (internal):** bump **minor** for additive, **major** for breaking; document deprecation window.
- **Schemas:** migration scripts versioned; each script contains forward and backward steps.
- **Policies/Allowlists:** versioned JSON/YAML with audit log (who/why/when) and link to decision note.

---
<br>

### 12.6 Environments & Promotion

| Env | Purpose | Flags | Data |
|---|---|---|---|
| **Dev** | Fast iteration, integration checks | ON for dev surfaces | Synthetic/dev fixtures |
| **Staging** | Pre-prod validation & demos | OFF by default; enable for test roles | Masked data, seeded titles/variants |
| **Prod** | Live users | Gradual enablement (role-aware) | Real data (encrypted PII) |

Promotion requires: CI green → staging tests green → **Go** from PL/TL for Normal/Schema changes.

---
<br>

### 12.7 Feature Flags & Rollbacks

- **Types:** kill-switch, percentage rollout, role-scoped (curator/admin).
- **Rules:** default OFF in prod; document **owner**, **sunset date**, **rollback steps**.
- **Observability:** each flag emits on/off and exposure events; tie to KPIs (Plan §2).

---
<br>

### 12.8 Database Migrations

- **Pattern:** **expand → migrate → contract**; never block requests.
- **Process:**  
  1) Add non-breaking structures (expand).  
  2) Backfill/migrate with idempotent jobs.  
  3) Switch reads/writes.  
  4) Drop old structures (contract) in a later release.
- **Rollback:** keep dual-write or shims until contract done; tested on staging with production-like volumes.

---
<br>

### 12.9 Configuration, Secrets & Keys

- **Config:** environment-specific files under version control (no secrets), validated at boot.
- **Secrets:** KMS-managed; rotated; never in code or logs; short-lived tokens for providers.
- **Audit:** all config/secret changes logged with editor, timestamp, and ticket link.

---
<br>

### 12.10 Documentation & Decision Records

- **Decision Notes** (see §11.5 template) attached to tickets for: policy changes, NFR budget shifts, and cross-module technical choices.
- **Runbooks:** updated when alerts, thresholds, or flows change (Register §8).
- **Help pages:** updated when user-facing behaviour or wording changes (evidence examples, compatibility labels, curator guidelines).

---
<br>

### 12.11 Emergency Changes (P1)

- **Trigger:** security breach, PII exposure, data loss, widespread outage.
- **Process:** one approver (**PL** or **TL**) may bypass normal review; must include rollback and immediate monitoring.
- **Aftermath:** within **48h**, retro & tests added; register entry updated with root cause and prevention steps.

---
<br>

### 12.12 Audit Trail & Traceability

- **Every change** links to: ticket → BRD/SRS/UC IDs → risk/assumption (if applicable).
- **Systems logged:** code repo, CI, deployment tool, feature-flag changes, migration runner, policy editors.
- **Retention:** keep logs ≥ 12 months; redact PII and tokens.

---
<br>

### 12.13 Change Calendars & Freezes

- **Freeze windows:** 72h before **Alpha/Beta/MVP** gates — only P1 hotfixes allowed.
- **Blackout periods:** coordinated with major outreach events; flags allow **read-only** UI changes if needed.

---
<br>
<br>

## 13. Procurement & External Services

This section lists **what we must procure**, **how we get access**, and **how we stay compliant and on-budget** for MVP. It focuses on *read-only* data sources, storage/CDN, observability, and light AI/AV utilities. Numbers align with §8 **Budget & Cost Baseline**.

---
<br>

### 13.1 Scope & Principles

- **Read-only first.** No write-back to game platforms in MVP.  
- **Minimal vendors.** Prefer one S3-compatible storage + one CDN; starter tiers for APM/email.  
- **Compliance aware.** Follow provider ToS; keep tokens short-lived; no scraping.  
- **Cost control.** Choose tiers that fit the §8 baseline and add quotas/alerts from day one.

Cross-refs: [Project Budget §8](./business-case.md) · [BRD §10 Dependencies](./brd.md) · [SRS §5 External Interfaces](./srs.md)

---
<br>

### 13.2 Provider Matrix (MVP)

| Area | Provider / Asset | Purpose | Access Needed | Plan/Tier (MVP) | Owner |
|---|---|---|---|---|---|
| **Official achievements (modern)** | **Steam Web API** (read-only) | Pull user achievements (where available) | App key + user consent | Free dev tier; rate-limited | Tech Lead |
| **Retro achievements (emulators)** | **RetroAchievements API** (read-only) | Link user profile; read sets/progress | User token (read) | Free community API; respect quotas | Tech Lead |
| **Game metadata** | **IGDB** (primary) + curated overrides | Titles/platforms, release data, covers | Client credentials | Free/low-tier API; cache results | BA / Tech Lead |
| **Object storage** | **S3-compatible bucket** | Saves & screenshots storage | Bucket + IAM | Standard storage; lifecycle to cold | Ops Lead |
| **CDN** | **CDN in front of storage** | Download/preview acceleration | CDN token | Basic CDN; cache hot objects | Ops Lead |
| **AV scanning** | **Managed AV / ClamAV** | Scan uploaded saves | Service/API key (if managed) | Low-volume plan | Ops Lead |
| **APM & uptime** | **Starter APM** | Perf metrics, traces, uptime | API key | Starter tier | Ops Lead |
| **Email (transactional)** | **Transactional email** | Sign-in, reset, notices | SMTP/API key | Low-volume plan | Admin |
| **Image processing** | **Light compute** | Resize, dedupe by checksum | N/A (infra) | Small instance / functions | Ops Lead |
| **AI triage (screenshots)** | **Self-hosted small model** (CPU) | First-pass “fits template?” triage | N/A (infra) | No vendor; upgrade later if needed | Tech Lead |
| **Domain & SSL** | **Registrar + Let’s Encrypt** | App domain & TLS | Registrar acct | Annual domain; free TLS | Admin |

*Evidence hosts (YouTube, Imgur, etc.) are **not** procured: we only maintain an **allowlist** and embed previews where permitted.*

---
<br>

### 13.3 Access, Keys & Secrets

- **Key issuance:** per-service keys live in a secrets manager; never in code/commits.  
- **Least privilege:** provider tokens scoped to read-only endpoints; rotate quarterly or per incident.  
- **Onboarding checklist:** create service account → generate key → store in KMS → add to staging/prod via CI variables → test on staging.

Runbook links: SRS §8 (Ops & Deployment) · Risks & Assumptions Register §8 (Monitoring & Alerts).

---
<br>

### 13.4 Quotas, Caching & Backoff

- **Steam / RA:** enforce provider-specific rate limits, **exponential backoff**, and **cooldown windows**; show *stale data* badges if sync lags.  
- **IGDB:** cache metadata by ID; refresh on schedule (e.g., weekly) or when a curator requests an override.  
- **Storage/CDN:** enable CDN caching for common downloads and resized previews; checksum-based dedupe for images.

KPIs & thresholds live in the Register §8 dashboards (Sync success rate, 429/5xx counts, cache hit-rate).

---
<br>

### 13.5 Data Handling, Privacy & ToS

- **Read-only** stance across all platform APIs; **no scraping**.  
- **PII minimisation:** store the bare minimum (internal user IDs + provider mapping); logs hold **no tokens**.  
- **User consent:** explicit UI step before linking Steam/RA; unlink at any time; purge mapped data on request.  
- **Evidence links:** only from **allowlisted** domains; previews are sandboxed; no background fetch of private content.

Legal/IP notes are in the Business Case and Risks Register (policy banners are **discovery-only** and locale-scoped).

---
<br>

### 13.6 Cost & Renewal Calendar (MVP)

| Item | Renewal | Est. / mo | Notes |
|---|---|---:|---|
| Storage + CDN | Monthly | $150 | Largest lever; lifecycle & cache save costs. |
| APM + uptime | Monthly | $40 | Starter tier until load grows. |
| Email | Monthly | $20 | Transactional only. |
| AV scan | Monthly | $50 | Low upload volume. |
| Image compute | Monthly | $30 | CPU-only; no GPU. |
| Domain | Annual | $15/mo (amortised) | Free TLS via Let’s Encrypt. |

> Totals align with **§8.4 Cloud & Tools** and the MVP **$1,220** baseline over 4 months.

---
<br>

### 13.7 Risks & Contingencies (Procurement Angle)

- **RISK-001 Provider quotas/policy shifts (Steam/RA).**  
  *Mitigation:* strict backoff/cooldowns, cache, visible *stale* badges; switch to **manual refresh** if needed.

- **RISK-006 Storage/egress costs spike.**  
  *Mitigation:* quotas, archive cold saves after 60–90 days, restrict hi-res previews, seek credits/sponsorships.

- **RISK-004 AI triage below threshold.**  
  *Mitigation:* keep full manual review path; tune thresholds; consider a managed vision API if budget allows.

All tracked in the Register §4; gates consume them per Project Plan §9.

---
<br>

### 13.8 Procurement Checklist (per Service)

1. Confirm **read-only** scope & ToS compliance.  
2. Create service account and **limit scopes**.  
3. Store secrets in **KMS**; wire to staging/prod via CI.  
4. Add **dashboards/alerts** (errors, quotas, costs).  
5. Write **runbook** (key rotation, outage procedure).  
6. Add to **renewal calendar** and owner list.  
7. Cross-link ticket to **BRD/SRS/UC IDs** and relevant **risk**.

---
<br>

### 13.9 Owners & Escalation

- **Tech Lead:** Steam/RA, AI triage, IGDB integration design.  
- **Ops Lead:** storage/CDN, APM, AV, image pipeline, dashboards.  
- **Admin:** email, domain, policy/allowlist tooling.  
- **BA:** vendor ToS sanity checks; dependency tracking in BRD/plan.  
- **Project Lead:** approves any paid tier changes or over-baseline spend.

---
<br>
<br>

## 14. Deployment & Release Management

This section defines **how we deploy, roll out, and—if needed—roll back** Dream Project. It aligns with environments in §12, readiness gates in §10, and the schedule in §5–§6.

---
<br>

### 14.1 Environments & Access

| Env | Purpose | Data & Secrets | Access |
|---|---|---|---|
| **Dev** | Rapid iteration, integration checks | Synthetic/seed data; dev keys | All engineers (scoped) |
| **Staging** | Pre-prod validation, demos, UAT | Masked/seeded data; prod-like config | Core team + invited testers |
| **Prod** | Live users | Real data; KMS-managed secrets, read-only provider tokens | TL/OPS for deploy; Admin for policy toggles |

Guardrails  
- Principle of **least privilege**; audit all deploys, flag changes, and schema migrations.  
- No test accounts with elevated roles in Prod.

---
<br>

### 14.2 Release Types & Tagging

| Type | Examples | Tag/Notes | Approval |
|---|---|---|---|
| **Normal** | Feature increments per WBS | `vX.Y.Z` + release notes | TL + one peer |
| **Config/Policy** | Allowlist domains, UA/RU banner overrides | Versioned JSON/YAML | MOD/ADM + BA visibility |
| **Schema/Data** | New columns, indices, backfills | Expand→Migrate→Contract plan | TL + OPS |
| **Hotfix (P1)** | Security, PII, data loss | `vX.Y.Z-hotfixN` | PL or TL (single approver) |

Release notes include: scope, flags, migrations, runbook links, known risks.

---
<br>

### 14.3 Pipeline (Build → Stage → Prod)

1. **Build**  
   - CI: tests (unit/integration), lint, secret scan, dependency audit.  
   - Artifacts signed and stored.

2. **Stage**  
   - Deploy with **flags OFF**.  
   - Run e2e on core journeys; smoke test admin/mod consoles; check dashboards (perf, errors, quotas).  
   - UAT where applicable.

3. **Promote to Prod**  
   - Go/No-Go by PL (inputs: §10 gate checklist, risk snapshot, dashboards).  
   - Canary 5–10% → widen in steps while watching KPIs/errors.

Rollback is always planned before promote (see §14.7).

---
<br>

### 14.4 Feature Flags & Rollout Strategy

- **Flag types:** kill-switch, percentage rollout, role-scoped (curator/admin).  
- **Defaults:** OFF in Prod; ON in Dev; selectively ON in Staging for test roles.  
- **Observability:** every flag emits exposure/on-off events; dashboards show adoption and error correlation.  
- **Sunset:** each flag has an owner and a target removal date (tracked in backlog).

Examples  
- Enable **AI triage auto-accept** only after moderator agreement ≥ target.  
- Restrict **UA/RU discovery-only banners** to catalogue surfaces; locale default = UA ON, others OFF (toggleable).

---
<br>

### 14.5 Database & Data Migrations

Process: **Expand → Migrate → Contract**  
- **Expand:** add new tables/columns (non-breaking); dual-write if needed.  
- **Migrate:** backfill idempotently; verify counts and logs.  
- **Contract:** switch reads/writes; remove legacy structures in a later release.

Rollback  
- Keep shims/dual-write until contract complete; verified on Staging with production-like volumes.

Seed Data  
- Minimal seed sets: sample games/variants (Steam build IDs; PAL/NTSC), curator templates, demo evidence links (non-PII).

---
<br>

### 14.6 Go-Live Checklist (per release)

| Area | Check | Owner |
|---|---|---|
| **Readiness** | Gate checklist signed (Alpha/Beta/MVP or interim) | PL |
| **Flags** | Default states documented; rollback steps written | TL |
| **Dashboards** | No red alerts (perf p95, availability, sync success, moderation SLA) | OPS |
| **Backups** | Last backup verified; restore drill not stale (>30 days) | OPS |
| **Security** | Keys rotated as scheduled; no secrets/tokens in logs | TL |
| **Content** | Help pages updated (evidence, compatibility, curator guide) | BA |
| **Policy** | UA/RU banners correct; allowlist changes reviewed | MOD/ADM |
| **Comms** | Release notes prepared; UAT cohort/curators informed | BA |

---
<br>

### 14.7 Incident Response & Rollback

Severity & Actions  
- **P1 (Blocker):** data loss, auth bypass, PII leak → **Rollback or hotfix same day**; status note to team; post-mortem within 48h.  
- **P2 (High):** core flow broken or wrong save label on *confirmed* variant → fix ≤48h or **flag off** affected surface.  
- **P3–P4:** next sprint unless user harm; consider copy/UX mitigations.

Rollback Steps  
1) Disable feature flag / traffic to previous version (canary back to 0%).  
2) For schema, switch to dual-write shim or revert reads to legacy path.  
3) Announce internally; add register entry; prepare post-mortem.

---
<br>

### 14.8 Post-Release Monitoring (first 72 hours)

Watch lists  
- **Integrations:** sync success, 429/5xx, time since last success (Steam/RA).  
- **Compatibility:** “didn’t work” confirmations vs downloads on **confirmed** variants.  
- **Moderation/Triage:** queue age, AI↔moderator agreement, appeals rate.  
- **Perf/Availability:** search/typeahead p95, detail p95, upload start latency, uptime.  
- **Costs:** storage growth, CDN egress, cache hit-rate.

If any KPI crosses threshold, open tasks and consider narrowing flags.

---
<br>

### 14.9 Compliance, Audit & Retention

- All deploys, flag changes, migrations, allowlist edits produce an **audit event** (who/when/why + ticket link).  
- Logs retained ≥12 months; tokens/PII redacted.  
- Read-only provider stance documented in release notes where relevant.

---
<br>

### 14.10 Go-Live Roles (who’s on deck)

| Role | Responsibility |
|---|---|
| **TL/OPS** | Execute deploy/rollback; monitor dashboards; handle infra alarms |
| **BA** | Status comms; help pages; curate feedback from early users/curators |
| **MOD/ADM** | Policy toggles; moderation backlog; appeals path ready |
| **QA** | Smoke e2e after each step; verify error/empty states |
| **PL** | Final Go/No-Go; scope trims via flags if required |

---
<br>
<br>

## 15. Measurement & Reporting

This section defines **what we measure**, **where it lives**, **who owns it**, and **how we act on it**. It reuses KPIs from §2 and dashboards described throughout the plan without duplicating requirements.

---
<br>

### 15.1 Purpose & Principles
- **Purpose:** make delivery progress and product health **observable**, **comparable to targets**, and **actionable**.
- **Principles:** one **source of truth** per KPI; **low-PII** analytics; **weekly** narrative with links; decisions recorded (see §11.5).

---
<br>

### 15.2 KPI Inventory (from §2 — targets unchanged)

| ID | KPI | Target | Owner | Source of Truth |
|---|---|---:|---|---|
| **K1** | MVP schedule on time | Code complete by end of Month 4 | Project Lead | Roadmap/burndown |
| **K2** | Monthly Active Users (MAU) & locale mix | ≥100 MAU; **EN 75% / UA 25%** | BA | Product analytics |
| **K3** | Linked providers (Steam/RA) | ≥50% of engaged users link ≥1 | BA | Onboarding/link events |
| **K4** | Save uploads/downloads | ≥200 uploads; ≥300 downloads | TL | Storage events |
| **K5** | Save compatibility accuracy | “Didn’t work” < **10%** on **confirmed** | TL | Compatibility service |
| **K6** | Fan achievement completions | ≥400 accepted; ≥60% with evidence | BA | Achievements service |
| **K7** | Active curators & published sets | ≥10 curators; ≥20 sets | BA | Curator admin |
| **K8** | Active sticker album templates | ≥50 across ≥50 games | BA | Album service |
| **K9** | AI triage quality (agreement) | ≥85% moderator agreement; appeals <10% | TL | Triage QA dashboard |
| **K10** | Moderation SLA | Median time-to-first-action ≤24h | MOD Lead | Mod queue |
| **K11** | Performance (p95) | Search ≤300ms; Details ≤500ms; Upload start ≤1s | TL | APM/RUM |
| **K12** | Availability | ≥99.5% (Beta & MVP months) | OPS | Uptime monitor |
| **K13** | Security/privacy incidents | 0 P1; 0 token leaks | TL | Sec logs |
| **K14** | Costs vs baseline | Within +10% of plan | OPS | Cost dashboards |
| **K15** | Risk burn-down | Critical=0; High mitigated | BA | R&A Register |

> Targets are evaluated **weekly**; gates (Alpha/Beta/MVP) require green/acceptable status per §10.3.

---
<br>

### 15.3 Dashboards 

| Dashboard | Primary Questions | Key Charts / Counters | Owner |
|---|---|---|---|
| **Integrations** | Are Steam/RA healthy? Are we within quotas? | Success rate by provider; 429/5xx counts; last-sync age; cooldown activations | TL |
| **Compatibility** | Are compatibility labels correct? | Download→“didn’t work” rate by **status** & **variant**; confirmation times; override usage | TL |
| **Moderation** | Are queues under control? | Queue size/age; median time-to-first-action; decisions/day; appeals rate | MOD |
| **Albums & Triage** | Is AI helping more than hurting? | AI↔moderator agreement; auto-accept rate behind flag; appeal outcomes | TL/MOD |
| **Search & Performance** | Is the app fast? | p95 for typeahead/results/detail/upload-start; error rate; cache hit-rate | TL/OPS |
| **Adoption & Content** | Is the product useful? | MAU; provider link rate; uploads/downloads; completions; templates/sets published | BA |
| **Costs** | Are we on budget? | Storage growth; CDN egress; APM cost; forecast vs baseline | OPS |

---
<br>

### 15.4 Event Taxonomy & Data Collection

**Identifiers:** pseudonymous user ID; no raw platform tokens or PII in events.  
**Common fields:** `ts`, `uid`, `role`, `locale`, `game_id`, `variant_id`, `source_page`.

**Core events (examples)**  
- `save_uploaded`, `save_downloaded`, `compat_confirmed`, `compat_didnt_work` (+ status/variant fields)  
- `provider_link_started/completed` (provider)  
- `achievement_sync_completed` (provider, count)  
- `fan_set_created/versioned/published`  
- `fan_completion_submitted/accepted/rejected` (+ evidence_host)  
- `album_template_created`, `sticker_submitted/accepted/rejected` (+ triage_decision)  
- `content_reported`, `mod_action_taken`  
- `search_performed` (+ filters), `result_clicked`  
- `banner_viewed/toggled` (UA/RU discovery-only banners)

**Retention:** raw events 12 months; rollups aggregate weekly/monthly. **No** tokens or PII in logs.

---
<br>

### 15.5 Thresholds, RAG Rules & Alerts

**RAG (Red/Amber/Green) examples**  
- **K11 Performance (p95):** Green ≤ target; Amber ≤ target + 20%; Red > target + 20%.  
- **K5 Compatibility:** Green <10% “didn’t work” on **confirmed**; Amber 10–15%; Red >15%.  
- **K9 AI agreement:** Green ≥85%; Amber 75–85%; Red <75%.  
- **K10 Moderation SLA:** Green ≤24h; Amber 24–48h; Red >48h.  
- **K12 Availability:** Green ≥99.5%; Amber 99.0–99.5%; Red <99.0%.  
- **K14 Costs:** Green ≤ +10% vs plan; Amber +10–20%; Red > +20%.

**Alerting**  
- Pager for **Red** sustained ≥30 min (perf/availability/moderation/integrations).  
- Slack/email for **Amber** > 24h or 3 consecutive days trending worse.  
- All alerts link to runbooks (SRS §8) and relevant risk IDs.

---
<br>

### 15.6 Reporting Cadence

- **Daily:** dashboards skim + incident notes in stand-up (see §11).  
- **Weekly:** **one-page digest** (KPI deltas, top risks, decisions) posted by BA; register updated.  
- **Per Gate:** snapshot pack (charts + test summaries) attached to Go/No-Go checklist (§10.3).  
- **Monthly:** variance review vs **budget** (§8) and **schedule** (§5).

---
<br>

### 15.7 Data Quality & Governance

- **Validation:** event schemas checked in CI; unknown fields rejected; clock-skew tolerance ±2 min.  
- **Sampling:** none on core KPIs; sampling allowed on verbose traces.  
- **Bot/noise filters:** rate/UA/IP heuristics; exclude staff/test accounts from product KPIs.  
- **Audit:** changes to dashboards/thresholds recorded via decision notes (see §11.5) and §12 change control.

---
<br>

### 15.8 Status Templates (copy-paste)

**Weekly KPI Snapshot (markdown)**  
~~~
Week of: <YYYY-MM-DD>

Greens: K1 K3 K8 K12
Ambers: K5 (↑2pp), K9 (↓3pp)
Reds:   K11 (search p95 340ms), K10 (median 28h)

Notes:
- Search cache miss due to new filters; hotfix planned S6-W2.
- Moderation queue grew after album push; recruiting 2 volunteers.

Decisions needed:
- Approve tighter AI auto-accept threshold? (RISK-004)

Links:
- Dashboards: <Integrations> <Compatibility> <Perf> <Costs>
- Register: /mnt/data/risk-and-assumptions-register.md
~~~

**Gate Snapshot (attachment list)**
- KPI table (K1–K15 with RAG)  
- Perf & availability charts (last 7 days)  
- Moderation & triage charts (last 14 days)  
- Risk snapshot (Critical/High) with actions  
- UAT defect summary and exit criteria checklist

---
<br>

### 15.9 Ownership Map

| Area | KPI IDs | Owner |
|---|---|---|
| Adoption & Content | K2, K3, K6, K7, K8 | **BA** |
| Compatibility | K4, K5 | **Tech Lead** |
| Triage & Moderation | K9, K10 | **MOD Lead** |
| Perf & Availability | K11, K12 | **Tech Lead / Ops** |
| Security & Privacy | K13 | **Tech Lead** |
| Costs | K14 | **Ops** |
| Risk Burn-down | K15 | **BA** |
| Schedule | K1 | **Project Lead** |

---
<br>

### 15.10 Changing Metrics or Thresholds

- Propose via ticket with **rationale + KPI IDs + risk links** → approve per §12.2 (Class: *Normal*).  
- Update dashboard configs + docs; note the change in the weekly digest and this plan’s §19 Change History.

---
<br>
<br>

## 16. Acceptance & Handover

This section defines **how the MVP is accepted** by business stakeholders and **how the product is handed over** to operations, moderators, and maintainers—without duplicating requirements. It reuses criteria and gates from BRD/SRS and the Project Plan.

Cross-refs:  
- BRD — Business Acceptance & UAT scope: [./brd.md](./brd.md)  
- SRS — NFR budgets & Ops: [./srs.md](./srs.md)  
- Use Case Specification — scenarios & acceptance hooks: [./use-case-specification.md](./use-case-specification.md)  
- Risks & Assumptions Register — gates & alerts: [./risk-and-assumptions-register.md](./risk-and-assumptions-register.md)

---
<br>

### 16.1 Purpose
- Establish a **clear, testable bar** for MVP acceptance (functional + non-functional).
- Ensure **knowledge transfer** and **operational readiness** so the team can safely run and evolve the product after release.

---
<br>

### 16.2 Acceptance Scope (what must be proven)
- **Functional slices (happy + key error/empty states)** across the three pillars:  
  **Saves**, **Achievements** (official sync + fan sets with evidence), **Sticker Albums** (submission → decision).
- **Supporting modules:** Catalogue & localisation flags (incl. UA/RU discovery-only banners), Profiles, Discovery/Search, Moderation, Integrations (Steam/RA read-only).
- **NFRs:** performance p95 targets, availability ≥99.5% (Beta/MVP months), moderation SLA (median ≤24h), privacy/security guardrails, cost within baseline (see §8).
- **Operations:** dashboards, alerts, backups/restore, runbooks, feature flags & rollback tested.

---
<br>

### 16.3 Business Acceptance Criteria (condensed)
*(Detailed criteria live in BRD §11; this section summarises the go/no-go bar for MVP.)*

- **BAC-F1 — End-to-end journeys work** for real users:
  1) Search a title → download a **compatible** save (warnings on legacy/incompatible).  
  2) Link provider (Steam/RA) → view official achievements.  
  3) Complete a **fan** achievement with allowlisted evidence → reviewed/accepted.  
  4) Create album template (curator) → user submits sticker → triage → decision/appeal path.
- **BAC-F2 — Catalogue policy is correct:** UA/RU **discovery-only** banners appear only on catalogue/discovery surfaces; default ON for UA locale, OFF otherwise (toggleable).
- **BAC-N1 — Performance:** p95 search ≤300ms; detail ≤500ms; upload start ≤1s (SRS §6).  
- **BAC-N2 — Availability:** ≥99.5% during Beta & MVP months.  
- **BAC-N3 — Moderation:** median time-to-first-action ≤24h; allowlist enforced; audit trail present.  
- **BAC-N4 — Privacy/Security:** read-only integrations; no tokens/PII in logs; secrets managed; AV scan active.  
- **BAC-C1 — Budget:** total spend within **+10%** of §8 baseline.

Failure of any **Critical** item blocks MVP (see §10.3 gates).

---
<br>

### 16.4 UAT Scope & Participants
- **Participants:** early UA/EN users, curators, moderators (see §6.6).  
- **Journeys under test:** the four from **BAC-F1** plus edge/empty states (no linked provider, incompatible save, evidence from non-allowlisted host, appeal).  
- **Data:** seeded games/variants (Steam build IDs; PAL/NTSC), dummy accounts/tokens, non-PII evidence samples.

**Defect tolerance at exit:**  
- **P1/P2:** 0 open.  
- **P3:** allowed only with workarounds or feature-flag OFF.  
- **P4:** backlog.

---
<br>

### 16.5 Acceptance Evidence Bundle (what we hand to approvers)
- **Test summaries:** pass/fail by journey with links to UC/BRD/SRS IDs.  
- **KPI snapshot:** K1–K15 with RAG (last 7–14 days).  
- **NFR charts:** perf p95, availability, moderation SLA, costs vs baseline.  
- **Ops proofs:** dashboard links, last backup + **restore drill** report, alert rules.  
- **Security/Privacy:** secret-scan & dependency audit results; logging samples (no tokens/PII).  
- **Docs:** help pages (evidence examples, compatibility labels, curator guidelines); runbooks; moderation handbook.  
- **Release notes:** flags, migrations, rollback steps.

---
<br>

### 16.6 Knowledge Transfer (KT) Plan
| Audience | Session | Contents | Owner | Artefacts |
|---|---|---|---|---|
| Ops | **Ops & Deploy** | Envs, flags, rollback, backups, alerts | TL/OPS | Runbooks, dashboard links |
| Moderators | **Governance & Evidence** | Console, allowlist policy, appeals, SLAs | MOD | Handbook, macros, policy YAML |
| Curators | **Authoring** | Fan sets, album templates, spoiler/hints | BA/UX | Authoring guide, examples |
| Support | **User Journeys** | Common issues & copy variants | BA | Help snippets, FAQs |

KT is complete when recordings/notes are stored and attendees confirm access.

---
<br>

### 16.7 Handover to Operations & Moderation
- **Ops receives:** deployment access, secrets via KMS, alert ownership, restore drill schedule, cost dashboards.  
- **Moderation receives:** console access, allowlist editor, policy files, audit export guide, SLA dashboard.

**Readiness checks:** no red alerts; on-call rota published; paging tested.

---
<br>

### 16.8 Hypercare & Post-Release Support
- **Window:** first **14 days** after MVP.  
- **Who:** TL/OPS primary on-call; BA for comms; MOD for appeals spikes.  
- **Response:** P1 same-day rollback/hotfix; P2 ≤48h or flag OFF; weekly summary in digest.  
- **Exit:** after 14 days with no P1/P2 in last 7 days and KPIs green/amber only.

---
<br>

### 16.9 Sign-Off & Records

**Sign-Off Template (copy-paste)**
~~~
Release: MVP <YYYY-MM-DD>
Scope: As per release notes /docs/releases/MVP-YYYY-MM-DD.md

I have reviewed the Acceptance Evidence Bundle and confirm that:
[ ] BAC-F1/F2 met (functional)
[ ] BAC-N1..N4 met (non-functional)
[ ] Budget within +10% baseline
[ ] No Critical risks open; High risks mitigated
[ ] Operations & Moderation handover complete

Signed:
Business Acceptance (BA): __________________  Date: ________
Technical Acceptance (TL): _________________  Date: ________
Operations Readiness (OPS): _______________  Date: ________
Moderation Readiness (MOD): _______________  Date: ________
Project Lead (PL) – Go/No-Go: _____________  Date: ________
~~~

All signed artefacts are stored alongside the release notes and linked from this plan’s **Change History** (§19).

---
<br>

### 16.10 Changes After Acceptance
- Any post-sign-off change follows **§12 Change Control**.  
- Material scope shifts require PL approval and an updated BAC reference in BRD/SRS (by ID), not duplicated text.

---
<br>
<br>

## 17. Post-Launch Support & Operations

This section defines **how we run Dream Project after MVP**—who is on call, what we watch, how we fix issues, and how we keep costs, data, and integrations healthy. It reuses practices and KPIs from §10–§15 and keeps traceability to the Risks & Assumptions Register.

---
<br>

### 17.1 Support Model & Hours

- **Support window:** 08:00–22:00 **EET** (core), with light on-call 22:00–08:00 for **P1/P2** only.  
- **Contact paths:** in-product *Report an issue*, support email, and Discord community channel (moderated).  
- **Triage intake:** all paths create tickets with environment, role, URL, and event ID (if available).

**Ownership**  
- **TL/OPS:** infrastructure, performance, integrations, storage/CDN.  
- **MOD Lead:** moderation queues, allowlist policy, appeals.  
- **BA:** user comms, help pages, curator onboarding/questions.  
- **PL:** stakeholder escalation, Go/No-Go for hotfixes.

---
<br>

### 17.2 On-Call & Rotations

- **Primary:** TL/OPS alternate weekly (1 primary, 1 secondary).  
- **Backup:** BE/FE during business hours for feature-specific incidents.  
- **Hand-off:** weekly 10-minute async note (open incidents, known risks, flag states).  
- **Paging rules:**  
  - **P1** (security, PII, data loss, auth outage, site down) → page immediately, 15-minute acknowledge.  
  - **P2** (core flow outage, wrong compatibility on confirmed variants) → page daytime; after hours if >30 min.  
  - **P3/P4** handled next business day.

---
<br>

### 17.3 Operational SLOs & SLAs

| Area | SLO / SLA | Measure | Owner |
|---|---|---|---|
| **Availability** | ≥99.5% monthly (Beta/MVP+) | Uptime monitor | OPS |
| **Performance** | p95 search ≤300ms; details ≤500ms; upload start ≤1s | APM/RUM | TL |
| **Moderation** | Median time-to-first-action ≤24h | Mod dashboard | MOD |
| **Incident Response** | P1 acknowledge ≤15m; P1 mitigate/rollback ≤60m | Pager logs | TL/OPS |
| **Backups** | Nightly; **restore drill** ≥1/30 days | Ops runbook | OPS |
| **Costs** | ≤ +10% vs baseline (§8) | Cost dashboard | OPS |

Breaches create tickets linked to **KPI IDs** (§15) and **risk IDs** (Register §4).

---
<br>

### 17.4 Monitoring & Alerting (live after MVP)

Dashboards (see §15.3) are **owned** and **reviewed daily**; alerts route to on-call:

- **Integrations:** Steam/RA success, 429/5xx, last-sync age; cooldown/backoff events.  
- **Compatibility:** “didn’t work” rate on **confirmed** variants; spike alerts by game/variant.  
- **Triage/Moderation:** AI↔moderator agreement; queue size/age; appeals rate.  
- **Perf/Availability:** p95 latencies; error rate; uptime.  
- **Costs:** storage growth, CDN egress, cache hit-rate.

Every alert links to a **runbook** (SRS §8) and an **owner**.

---
<br>

### 17.5 Incident Management

**Flow (P1/P2)**  
1) **Detect** (alert or report) → page on-call.  
2) **Stabilise** (rollback/flag off, rate-limit, switch to manual paths).  
3) **Communicate** (status note in support channel; BA drafts public copy if needed).  
4) **Diagnose & fix** (ticket with BRD/SRS/UC IDs; logs/event IDs attached).  
5) **Post-mortem** within **48h** (what happened, why, actions, owners, due dates).  

**Templates:** use §11.5 “Decision Note” for mitigation decisions; attach to change record (§12).

---
<br>

### 17.6 Data Lifecycle, Backups & Recovery

- **Backups:** nightly for DB; hourly checkpoints for object metadata; object storage uses versioning.  
- **Retention:** raw analytics 12 months; cold archive for **inactive saves** per lifecycle rules (e.g., archive after 60–90 days).  
- **Recovery:** quarterly full **restore drill** to staging; evidence recorded in §10 gate artifacts.  
- **PII:** minimised; export/delete supported upon request; logs scrubbed of tokens/PII.

---
<br>

### 17.7 Security & Privacy Operations

- **Key/secret rotation:** quarterly or on incident; short-lived provider tokens; least privilege scopes.  
- **Dependency/secret scans:** in CI on every merge; weekly report.  
- **Evidence allowlist:** policy updates via versioned YAML; changes logged/audited; MOD + ADM owners.  
- **Abuse/risk controls:** rate limits on uploads, report spam heuristics, temp account holds for repeated violations.

---
<br>

### 17.8 Integrations Care (Steam / RetroAchievements / IGDB)

- **Read-only posture** (no write-back; no scraping).  
- **Quotas:** enforce provider limits; exponential backoff with *stale data* badges if sync lags.  
- **Health checks:** provider-specific error budgets; weekly success-rate review; incidents tagged `INT-*`.  
- **Change watch:** subscribe to provider change logs; BA keeps a small “API watchlist” note in the backlog.

---
<br>

### 17.9 Cost Management

- **Monthly variance review** vs §8 baseline; OPS proposes levers: archive policies, cache tuning, image dedupe, limiting hi-res previews, or seeking credits.  
- **Alert:** >+10% MoM triggers an action plan and update in the weekly digest (§15.6).

---
<br>

### 17.10 Maintenance Windows & Freezes

- **Routine maintenance:** Tue/Thu 20:00–22:00 EET (feature flags can keep read paths up).  
- **Freezes:** 72h before planned announcements or outreach; only P1 hotfixes allowed.

---
<br>

### 17.11 Content & Community Operations

- **Curators:** BA/UX maintain authoring guide; monthly office hours; credit/badges visible on profiles.  
- **Moderators:** MOD maintains macros and appeals SOP; recruits volunteers if SLA breaches persist.  
- **Policy banners:** UA/RU **discovery-only** banners remain catalog-scoped; ADM controls overrides with audit.

---
<br>

### 17.12 Feedback → Backlog Loop

- **Inputs:** support tickets, curator/mod feedback, UAT cohort, analytics anomalies.  
- **Triage:** BA reviews weekly; converts into stories with acceptance tests and NFR hooks; links to KPIs/risks.  
- **Prioritisation:** MoSCoW + risk reduction + KPI impact (see §6.3).

---
<br>

### 17.13 Renewal & Compliance Calendar

| Asset | Renewal | Owner | Notes |
|---|---|---|---|
| Domain & DNS | Annual | ADM | Auto-renew + backup payment method |
| SSL/TLS | 60–90 day cycle (Let’s Encrypt) | OPS | Auto-renew; alert if failures occur |
| APM/Uptime | Monthly | OPS | Downgrade/upgrade with load |
| Email | Monthly | ADM | Track bounce/complaint rates |
| AV/Sandbox | Monthly | OPS | Review usage vs plan |
| Backups/Drills | Monthly drill | OPS | Evidence stored with gate artifacts |

---
<br>

### 17.14 Post-Launch KPIs (watch closely first 90 days)

- **Adoption:** K2/K3 (MAU, link rate).  
- **Content:** K6/K7/K8 (completions, sets, albums).  
- **Trust:** K5/K9/K10 (compatibility accuracy, triage agreement, moderation SLA).  
- **Health:** K11/K12/K14 (perf, availability, costs).

Weekly digest (§15.6) tells the story; gate snapshots used for major updates.

---
<br>

### 17.15 Exit from Hypercare

- **Criteria (after MVP):** no P1/P2 in last 7 days; KPIs green/amber only; moderation queues within SLA.  
- **Action:** switch to standard on-call only; reduce daily reviews to normal cadence; archive hypercare notes.

---
<br>

### 17.16 Living Artefacts

All runbooks, thresholds, and SOPs are **living documents** in `/docs/runbooks/*`. Any change follows §12 Change Control and is referenced in the weekly digest.

---
<br>
<br>

## 18. Cross-References

This section lists the **authoritative sources** this Project Plan depends on, with short notes on *what you’ll find there* and *how this plan uses it*. Links point to the latest versions of each document.

---
<br>

### 18.1 Core Product Documents

- **Business Case** — vision, problem/solution framing, value, KPIs, timeline, and budget context.  
  How this plan uses it: aligns schedule/KPIs (§5–§6, §15) and budget baselines (§8).  
  [./business-case.md](./business-case.md)

- **Business Requirements Document (BRD)** — business objectives, scope, stakeholder needs, requirements catalogue, dependencies, acceptance criteria, and traceability.  
  How this plan uses it: sources scope for WBS/schedule (§4–§6), acceptance gating (§10, §16).  
  Key sections: *Objectives & Success Criteria*, *Requirements Catalogue*, *Business Acceptance Criteria*.  
  [./brd.md](./brd.md)

- **Software Requirements Specification (SRS)** — system context, roles/permissions, functional requirements by module, external interfaces, NFRs, data, and ops.  
  How this plan uses it: informs release slices (§5–§6), NFR budgets (§10), deployment/runbooks (§14).  
  Key sections: *Functional Requirements by Module*, *External Interfaces*, *NFRs*, *Operational & Deployment*.  
  [./srs.md](./srs.md)

- **Use Case Specification** — actor goals and detailed flows per module (happy paths, alternatives, exceptions), with acceptance hooks.  
  How this plan uses it: defines UAT scope (§10, §16) and demo content at gates.  
  [./use-case-specification.md](./use-case-specification.md)

- **Stakeholder & Communication Plan** — stakeholder map, engagement strategy, channels, meetings, and escalation paths.  
  How this plan uses it: meeting cadence & roles (§11), escalation rules (§9, §12).  
  [./stakeholder-and-communication-plan.md](./stakeholder-and-communication-plan.md)

- **Risks & Assumptions Register** — scoring model, active risks, assumptions, validation plan, monitoring/alerts, and change history.  
  How this plan uses it: drives gates (§9, §10), weekly loop (§11), and contingency decisions (§5, §12).  
  [./risk-and-assumptions-register.md](./risk-and-assumptions-register.md)

---
<br>

### 18.2 Where to Look for Specific Topics

| Topic | Primary Source(s) | Notes |
|---|---|---|
| MVP scope & milestones | BRD; Project Plan §3–§6 | BRD defines *what*; this plan defines *when/how*. |
| Acceptance criteria (business) | BRD — *Business Acceptance Criteria*; Project Plan §16 | §16 summarises; BRD is the authority. |
| Module behaviours | SRS — *Functional Requirements by Module*; Use Cases §4 | Use Cases are step-by-step; SRS states rules/contracts. |
| External integrations (Steam/RA/IGDB) | SRS — *External Interfaces*; Project Plan §13–§14 | Read-only posture, quotas, keys, rollout. |
| Save compatibility rules | SRS (SAV + Catalogue); Use Cases (SAV/CAT); Risks Register | Labels, confirmations, overrides, and related risks. |
| Moderation & evidence allowlist | SRS (GOV); Use Cases (GOV/ALB/ACH); Risks Register | Policies, appeals, and AI triage thresholds. |
| KPIs & dashboards | Project Plan §15; Risks Register §8 | Targets, thresholds, alerts, owners. |
| Ops, deploy, rollback | Project Plan §12–§14; SRS §8 | Envs, flags, migrations, runbooks. |
| Budget & costs | Business Case §11; Project Plan §8 | Baseline, variance rules, levers. |

---
<br>

### 18.3 Link Hygiene (for tickets & docs)

- Reference **IDs** in commits/issues: `BR-…`, `SR-…`, `UC-…`, `RISK-…`, `ASM-…`.  
- Prefer **links to the file root** (stable) and mention the section name in text; add deep anchors only when you’re sure of the header.  
- When a decision changes scope/schedule, add a short **Decision Note** (see §11.5) and link it from the ticket and the relevant document section.

---
<br>

### 18.4 Change Traceability

All plan updates in §19 *Change History* must include: the **reason** (risk/assumption or dependency), the **source link** (one of the docs above), and the **affected sections** in this plan.

---
<br>

## 19. Change History

This section records **what changed, when, why, and who approved it** for the Project Plan. It keeps a single, auditable trail without duplicating content from other artefacts.

---
<br>

### 19.1 Purpose & Conventions
- One **row per meaningful change** (content edits, scope/date/budget shifts, policy toggles that affect this plan).
- Always include **reason/source IDs** (e.g., `RISK-004`, `ASM-002`, `BR-OBJ-03`, `SR-SAV-12`) and link to the authoritative document (see §18).
- For multi-file changes, log here and cross-link to the related doc’s own history.

---
<br>

### 19.2 Versioning Scheme
- **v0.x** – Drafts (working copies before Alpha gate).
- **v0.9-alpha** – Plan ready for **Alpha** review.
- **v0.95-beta** – Plan updated for **Beta** scope.
- **v1.0-mvp** – **MVP** release baseline (signed).
- **v1.x** – Post-MVP updates (minor = additive/clarifying; major = dates/scope/budget shifts).

Tags mirror repo release tags where relevant (see §12.4).

---
<br>

### 19.3 Log (authoritative)

| Date (YYYY-MM-DD) | Version/Tag  | Editor   | Sections Affected | Summary of Change                                                           | Reason / Source (IDs)                      | Approved by |
|---|---|---|---|---|---|---|
| <YYYY-MM-DD> | v0.1-draft  | BA / PL | §1–§4 | Initial Project Plan scaffold (approach, objectives, scope, WBS).          | Inception notes; BRD linkage (`BR-CTX-*`) | PL |
| <YYYY-MM-DD> | v0.6-draft  | BA / TL | §5–§7 | Added schedule/milestones, iteration plan, resource/RACI tables.           | Schedule workshop; risks snapshot (`RISK-*`) | PL, TL |
| <YYYY-MM-DD> | v0.8-draft  | BA / OPS | §8, §9, §10 | Budget baseline (–15% UA rates), risk integration, quality gates (Alpha/Beta). | Budget note; Register §§3–4               | PL |
| <YYYY-MM-DD> | v0.9-alpha  | BA / TL | §11–§14 | Comms cadence; change control; procurement; deploy & rollback procedures.  | SRS §8; Register §8 (alerts)               | PL, TL, OPS |
| <YYYY-MM-DD> | v0.95-beta  | BA / MOD | §15–§17 | KPI dashboards; acceptance & handover; post-launch ops & on-call model.    | BRD §11; Use Cases §4; Register updates    | PL, MOD, TL |
| <YYYY-MM-DD> | v1.0-mvp    | BA / PL | §1–§18 | Signed MVP baseline; links finalised; minor copy clarifications.           | Go/No-Go pack; gate checklist (§10.3)      | PL (sign-off) |

> Replace placeholder dates with actuals when you save each revision. Add rows for further edits; **do not** overwrite prior entries.

---
<br>

### 19.4 Entry Template

~~~
| <YYYY-MM-DD> | <version/tag> | <editor> | <sections> | <what changed> | <reason / source IDs> | <approved by> |
~~~

**Examples of “Reason / Source IDs”**  
- Risk/Assumption: `RISK-002`, `ASM-003` (see Risks & Assumptions Register).  
- BRD/SRS/UC trace: `BR-OBJ-02`, `SR-SAV-26`, `UC-ALB-03`.  
- Decision note: link to §11.5 Decision Note.

---
<br>

### 19.5 Storage & Signatures
- The **signed PDF/MD** of each milestone version (`v0.9-alpha`, `v0.95-beta`, `v1.0-mvp`) is archived in `/docs/releases/`.
- Gate sign-offs (see §16.10 template) are stored alongside the release notes and linked from the corresponding row above.