# Test Case Specification

## Contents

- **[1. Document Purpose & Conventions](#1-document-purpose--conventions)**  
  What this specification is for, who uses it, and how test cases are identified and written. Defines the ID scheme (e.g., `TC-<MODULE>-NN`), naming, preconditions, steps, expected results, and traceability links to BRD/SRS/Use Cases.

- **[2. Test Scope & Objectives](#2-test-scope--objectives)**  
  The boundaries of testing for the MVP: which features, platforms, and locales are covered; what is explicitly out of scope; success criteria for test completion.

- **[3. Coverage & Traceability Map](#3-coverage--traceability-map)**  
  Matrix mapping business requirements (BRD), system requirements (SRS), and use cases to test suites and test cases, ensuring nothing critical is missed and avoiding duplication.

- **[4. Test Approach & Levels](#4-test-approach--levels)**  
  How we test and at which levels: unit, service/integration, end-to-end (journey), and exploratory/regression. Includes smoke vs. full runs, and when to use feature flags in tests.

- **[5. Test Environment & Tools](#5-test-environment--tools)**  
  Environments (Dev/Staging/Prod-readonly), data isolation, seeded fixtures (games, variants, Steam build IDs, PAL/NTSC), and tools (runner, CI, APM, log viewer). Includes account roles used in tests.

- **[6. Test Data Management](#6-test-data-management)**  
  Creating, versioning, and cleaning test data: synthetic users, curator/moderator accounts, allowlisted evidence links, save files for compatibility labels, and retention rules (no PII/tokens).

- **[7. Entry & Exit Criteria](#7-entry--exit-criteria)**  
  Preconditions to start test cycles and conditions to finish (per sprint, Alpha, Beta, MVP). Mirrors the Definition of Done and gate criteria without duplicating them.

- **[8. Defect Severity, Priority & Workflow](#8-defect-severity-priority--workflow)**  
  Severity/priority definitions, SLAs (P1/P2/P3/P4), triage steps, reproduction evidence, and linking bugs to requirement IDs and failed test case IDs.

- **[9. Test Suites by Module](#9-test-suites-by-module)**  
  Suite index that groups detailed test cases by product module.  
  • **[ACC — Accounts & Authentication](#acc--accounts--authentication)**  
  • **[CAT — Catalogue & Localisation Flags](#cat--catalogue--localisation-flags)**  
  • **[SAV — Save Management](#sav--save-management)**  
  • **[ACH — Achievement Tracking](#ach--achievement-tracking)**  
  • **[ALB — Sticker Albums](#alb--sticker-albums)**  
  • **[PRO — Profiles & Progress](#pro--profiles--progress)**  
  • **[GOV — Moderation & Governance](#gov--moderation--governance)**  
  • **[DSC — Discovery & Search](#dsc--discovery--search)**  
  • **[INT — External Integrations & Adapters](#int--external-integrations--adapters)**

- **[10. Non-Functional Test Plan](#10-nonfunctional-test-plan)**  
  Performance (p95 targets), availability checks, security/privacy validations (no tokens/PII in logs), accessibility, localisation (EN/UA), and cost-sensitive behaviours (e.g., image dedupe, caching).

- **[11. Test Execution & Reporting](#11-test-execution--reporting)**  
  How we run cycles (smoke, regression, UAT), record results, produce daily/weekly summaries, and publish gate snapshots with evidence.

- **[12. UAT Plan](#12-uat-plan)**  
  Scope and participants (users/curators/moderators), script structure per journey, acceptance thresholds, and defect tolerance at exit.

- **[13. Risks, Assumptions & Mitigations (Testing)](#13-risks-assumptions--mitigations-testing)**  
  Test-specific risks (e.g., provider rate limits during runs, AI triage variance), assumptions (stable seeded data), and mitigations (backoff, offline fixtures).

- **[14. Roles & Responsibilities](#14-roles--responsibilities)**  
  Who writes, reviews, executes, and signs off test cases and cycles (QA, TL, BA, MOD, OPS), including ownership of test data and environments.

- **[15. Schedule & Cycles](#15-schedule--cycles)**  
  Planned smoke/regression/UAT windows aligned to the project milestones (Alpha, Beta, MVP) and how slippage impacts test scope.

- **[16. References & Cross-Links](#16-references--crosslinks)**  
  Pointers to BRD, SRS, Use Case Specification, Project Plan (Quality §10), and Risks & Assumptions Register (monitoring §8).

- **[17. Change History](#17-change-history)**  
  Versioned log of updates to this specification with dates, editors, sections changed, and rationale/IDs.

---
<br>
<br>

## 1. Document Purpose & Conventions

This specification defines **how we design, write, execute, and track test cases** for the Dream Project MVP. It ensures QA, developers, curators, moderators, and ops read tests the same way, execute them consistently, and can trace each result back to requirements.

---
<br>

### 1.1 Audience & Goals
- **Audience:** QA engineers (primary), Developers/Tech Lead, BA, Moderation Lead, Ops.
- **Goals:**
  - Provide a **consistent template** for all test cases (manual and automated).
  - Guarantee **traceability** to BRD/SRS/Use Cases and **reproducibility** across environments.
  - Make **evidence** and **outcomes** auditable for Alpha, Beta, and MVP gates.

---
<br>

### 1.2 Scope (What This Document Covers)
- Test case **structure**, **ID scheme**, **naming**, **metadata**, and **lifecycle**.
- Conventions for **preconditions**, **test data**, **steps**, **expected results**, **evidence**, and **pass/fail** rules.
- Execution standards for **localisation (EN/UA)**, **roles (Regular/Curator/Admin)**, **providers (Steam/RetroAchievements)**, **save compatibility statuses**, and **policy banners**.
- Pointers to **non-functional** checks embedded in functional tests (performance hooks, privacy/security/logging).
  
*Out of scope:* unit/service test coding style and CI specifics (covered by engineering guidelines).

---
<br>

### 1.3 Module Codes & ID Scheme
Use **module codes** aligned with SRS/Use Cases.

| Module | Code | Examples |
|---|---|---|
| Accounts & Authentication | **ACC** | sign-in, 2FA, password reset |
| Catalogue & Localisation Flags | **CAT** | search, variant selection, UA/RU discovery-only banners |
| Save Management | **SAV** | upload, download, compatibility labels, AV scan |
| Achievement Tracking | **ACH** | official sync (Steam), RA link, fan completion with evidence |
| Sticker Albums | **ALB** | template authoring, submission, AI triage, moderator decision |
| Profiles & Progress | **PRO** | profile view, badges, activity |
| Moderation & Governance | **GOV** | report → queue → decision → appeal, allowlist |
| Discovery & Search | **DSC** | typeahead, filters, ranking |
| External Integrations & Adapters | **INT** | Steam/RA/IGDB adapters, backoff, staleness banners |

- **Test Case ID:** `TC-<MODULE>-NNN` (e.g., `TC-SAV-012`).
- **Suite ID (optional):** `TS-<MODULE>-NN` (e.g., `TS-ACH-02`).

---
<br>

### 1.4 Naming & Writing Style
- **Title:** imperative, concise (e.g., “Upload save → AV pass → label visible (Desktop, EN)”).
- **Steps:** action-oriented, one assertion per step; avoid ambiguity.
- **Expected results:** observable, deterministic; reference exact UI text or status labels where relevant.
- **Copy tone:** precise; no slang; use exact UI labels.

---
<br>

### 1.5 Required Fields (per Test Case)
- **ID & Title** — `TC-…` and a short directive.  
- **Traceability** — list `BR-…`, `SR-…`, `UC-…` IDs (and `RISK-…`/`ASM-…` if validating a mitigation/assumption).  
- **Purpose** — what risk/requirement this test proves.  
- **Preconditions** — role, auth state, flags, linked providers, seeded data.  
- **Test Data** — users; games/variants (Steam build ID or PAL/NTSC); saves; evidence URLs (allowlisted); locales.  
- **Environment** — Dev or Staging (Prod is view-only unless explicitly stated).  
- **Steps** — numbered, minimal branching.  
- **Expected Results** — numbered to mirror steps; include UI notices/badges where applicable.  
- **Evidence** — what to capture (screenshot, event ID, log snippet without PII/tokens, link to run).  
- **Variants/Matrix** — locales (EN/UA), device (Desktop/Mobile), major browsers; role coverage if different.  
- **NFR Hooks** — timing thresholds (e.g., typeahead p95 ≤300ms), privacy/security checks (no tokens/PII in logs).  
- **Status & Result** — Draft/Ready/Blocked/Passed/Failed/Needs Investigation/Deferred/Obsolete.  
- **Owner & Last Run** — executor, date, environment, build tag.

---
<br>

### 1.6 Conventions & Special Rules
- **Localisation:** at minimum, **one EN and one UA** execution per critical journey. Verify UA/RU **discovery-only** banners appear **only** on catalogue/discovery surfaces (UA default ON; others OFF with toggle).  
- **Roles:** where behaviour differs by role, add a **role variant** (Regular/Curator/Admin) or separate TC IDs.  
- **Providers:** mark if **Steam** and/or **RetroAchievements** must be linked (read-only). Stale sync shows a *staleness* warning.  
- **Save compatibility:** reference statuses **confirmed / untested / legacy / incompatible** and expected UI.  
- **Evidence allowlist:** only approved domains (e.g., YouTube, Imgur) for proof; use working example fixtures.  
- **Privacy/Security:** never place tokens/PII in steps or evidence; confirm logs show **no secrets**.  
- **Feature flags:** list required flag state and rollback expectation where relevant.  
- **Data cleanup:** remove test entities (albums, sets, saves) after execution unless retained as fixtures.

---
<br>

### 1.7 Statuses & Lifecycle
**Draft → Ready → In Progress → Passed/Failed → (Needs Investigation/Deferred) → Obsolete**  
- Blocked tests must list the **blocking ID** (defect, dependency, or missing data).  
- Each cycle records **Last Run** (date, environment, build) and links to **execution evidence**.

---
<br>

### 1.8 Severity/Priority (for Failures & Execution)
- **Failure severity** (P1–P4) aligns with the Quality Plan (P1 = security/data loss; P2 = core flow broken; etc.).  
- **Execution priority** (High/Medium/Low) per gate: smoke vs. full suite defined in §2 and §9.

---
<br>

### 1.9 Test Case Template (copy-paste)
~~~yaml
id: TC-<MODULE>-NNN
title: <imperative title>
traceability:
  brd: [BR-…]
  srs: [SR-…]
  uc:  [UC-…]
  risks: [RISK-…]        # optional
  assumptions: [ASM-…]   # optional
purpose: <what risk/requirement this test proves>
preconditions:
  role: <Regular|Curator|Admin>
  auth: <signed-in|signed-out>
  providers: <none|Steam|RA|Steam+RA>
  flags: { <flag_name>: <ON|OFF> }
  locale: <EN|UA>
  environment: <Dev|Staging>
test_data:
  game: <title> (<platform/variant>; Steam build ID or PAL/NTSC)
  saves: <fixture ids/paths if applicable>
  evidence_links: [<allowlisted_url_examples>]
steps:
  - <step 1>
  - <step 2>
expected_results:
  - <result for step 1>
  - <result for step 2>
nfr_hooks:
  performance: <e.g., typeahead ≤300ms>
  privacy_security: <no tokens/PII in logs; AV scan passes>
evidence:
  capture: [screenshot, event_id, log_snippet]
  notes: <anything notable observed>
variants:
  device: <Desktop|Mobile>
  browser: <Chrome|Firefox|Safari|Edge (current-1)>
status: <Draft|Ready|Blocked|In Progress|Passed|Failed|Needs Investigation|Deferred|Obsolete>
owner: <name/initials>
last_run:
  date: <YYYY-MM-DD>
  environment: <Dev|Staging>
  build: <tag/commit>
~~~

---
<br>

### 1.10 Example Header (illustrative)
~~~yaml
id: TC-SAV-012
title: Upload save → AV pass → “confirmed” variant label visible (Desktop, EN)
traceability:
  brd: [BR-SAV-02]
  srs: [SR-SAV-05, SR-SAV-17]
  uc:  [UC-SAV-03]
purpose: Validate upload pipeline, AV scan success, and correct compatibility labelling for a confirmed variant.
~~~

---
<br>
<br>

## 2. Test Scope & Objectives

This section defines **what we will test for MVP, why, and to what depth**—so execution stays focused, traceable, and finishable within the release window.

---
<br>

### 2.1 Objectives (What success looks like)
- **Functional coverage:** prove all MVP user journeys across modules (**ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT**) on the happy path **plus** critical error/empty states.
- **Traceability:** 100% of executed test cases map to **BRD / SRS / Use Case** IDs; gaps are visible in the Coverage Map (§3).
- **Quality gates:** provide acceptance evidence for **Alpha → Beta → MVP** as defined in the Project Plan (Quality §10) and BRD Business Acceptance.
- **Defect control:** no open **P1/P2** at exit; P3 only with workarounds or feature-flag OFF; P4 in backlog.
- **NFR proof points:** meet p95 performance targets (search ≤300 ms; details ≤500 ms; upload-start ≤1 s), availability ≥99.5% in Beta/MVP months, moderation median ≤24 h, privacy/security guardrails (no tokens/PII in logs).

---
<br>

### 2.2 In Scope (MVP)
**Features & flows**
- **ACC – Accounts & Authentication:** sign-up (email), sign-in/out, password reset, session expiry, basic rate limits.
- **CAT – Catalogue & Localisation Flags:** game/variant search & filters; platform variants (Steam build ID; PAL/NTSC); UA/RU **discovery-only** policy banners (UA default ON; others OFF with toggle).
- **SAV – Save Management:** upload (with AV scan), metadata capture, download, compatibility labels (**confirmed / untested / legacy / incompatible**), community confirmations/“didn’t work”, admin override banner, basic version history.
- **ACH – Achievement Tracking:** link **Steam** (read-only) and/or **RetroAchievements** profile; view official achievements; view/author fan sets; submit completion with allowlisted evidence; moderator review & outcomes.
- **ALB – Sticker Albums:** curator template authoring (slots, spoiler tags, hint style), user submission, AI triage (first pass), moderator decision/appeal, completion badge logic.
- **PRO – Profiles & Progress:** profile view (badges, recent activity), curator credit display, locale defaults.
- **GOV – Moderation & Governance:** report flows, queue triage, allowlist policy enforcement, decisions, appeals, audit trail.
- **DSC – Discovery & Search:** typeahead, results ranking, filters, “stale data” or policy banners where applicable.
- **INT – External Integrations & Adapters:** read-only Steam/RA sync (with backoff), IGDB metadata fetch & caching; staleness badge.

**Data & variants**
- Seeded examples for:  
  - **Modern (Steam):** at least one title with 2+ **build IDs** to exercise compatibility labels.  
  - **Retro:** at least two titles (PAL and NTSC) to exercise region handling.  
  - **Evidence:** sample links from allowlisted hosts (e.g., YouTube, Imgur).

**Roles & locales**
- Execute critical journeys for **Regular**, **Curator**, and **Admin/Moderator** where behaviour differs.  
- Locales: **EN** and **UA** (minimum 1 full pass each on critical journeys).

---
<br>

### 2.3 Out of Scope (MVP)
- Native **mobile apps** (web only).  
- **Write-back** to game platforms (Steam/RA remain read-only).  
- **Direct emulator** integrations or proprietary SDK hooks.  
- Payment/monetisation, social graph, or chat features.  
- Deep anti-cheat or hardware attestation (beyond evidence allowlist + moderation).  
- Full accessibility audit beyond agreed essentials (keyboard nav, alt text, labels, contrast checks on key screens).

---
<br>

### 2.4 Platforms, Browsers, Devices & Locales
- **Devices:** Desktop (primary), Mobile web (sanity).  
- **Browsers (current–1):** Chrome, Edge, Firefox; Safari for Mobile sanity.  
- **Locales:** **EN**, **UA** (policy banners default per locale: UA ON; others OFF with toggle).  
- **Environments:** **Staging** (authoritative for test results), **Dev** (spot checks). Prod is **view-only** unless explicitly authorised.

---
<br>

### 2.5 Test Depth & Matrices
- **Happy path + key exceptions** per module (missing link, stale sync, incompatible save, non-allowlisted evidence, AI triage disagreement).  
- **Matrix axes:** Role × Locale × Device (sanity on Mobile) × Variant (Steam build / PAL vs NTSC) × Flag state (where relevant).  
- **Evidence:** screenshots, event IDs, safe log snippets; no tokens/PII.

---
<br>

### 2.6 Entry & Exit Criteria (per cycle)
**Entry**
- Staging stable; seeds loaded (games/variants/evidence fixtures); required flags OFF by default with access for test roles.  
- Linked provider test accounts available; dashboards reachable.

**Exit**
- All planned TCs executed; defects logged with repro and IDs; **P1/P2 = 0** open.  
- Gate evidence bundle prepared (pass/fail list, KPI snapshot, NFR charts, moderation SLA sample, links to dashboards).  
- Any deferred coverage is listed in §11 reporting for carry-over.

---
<br>

### 2.7 Assumptions
- Provider APIs (Steam/RA/IGDB) available within documented quotas; if throttled, tests use cached fixtures and mark results **provisional**.  
- AI triage runs in **shadow** or **flagged** mode until thresholds agreed; manual review path always available.  
- UA/RU policy banners are **catalogue/discovery-scoped only** and verified accordingly.

---
<br>
<br>

## 3. Coverage & Traceability Map

This section ensures **every business/system requirement and use case is covered by executable tests**—and that gaps are visible. It also defines how coverage stays current as artefacts evolve.

---
<br>

### 3.1 Purpose
- Prove **end-to-end coverage** for MVP across modules.
- Maintain **two-way traceability** between BRD/SRS/Use Cases ↔ Test Suites/Cases.
- Expose **gaps** early and keep them tracked until closed.

**Authoritative sources**  
- BRD — [./brd.md](./brd.md)  
- SRS — [./srs.md](./srs.md)  
- Use Case Specification — [./use-case-specification.md](./use-case-specification.md)

---
<br>

### 3.2 ID Conventions & Link Rules
- **Requirements:** `BR-<AREA>-NN` (business), `SR-<MODULE>-NN` (system).  
- **Use Cases:** `UC-<MODULE>-NN`.  
- **Test Suites:** `TS-<MODULE>-NN` (logical grouping).  
- **Test Cases:** `TC-<MODULE>-NNN`.

**Linking discipline**
- Each **TC** lists **traceability**: `brd:[]`, `srs:[]`, `uc:[]` (see §1.9 template).  
- A **requirement** may map to **many** TCs (happy path + error/edge + locale/role variants).  
- A **TC** may map to **multiple** requirements only if it truly validates all of them.  
- If a requirement has **no TC**, it is a **gap** and must carry an owner/date.

---
<br>

### 3.3 Coverage Dashboard (by Module)
> Update counts at the end of each sprint. “Covered” = at least one TC executed in Staging and linked.

| Module | BRD Reqs | SRS Reqs | Use Cases | Test Suites | Test Cases | Covered | Gaps |
|---|---:|---:|---:|---:|---:|---:|---:|
| ACC — Accounts & Auth | — | — | — | — | — | — | — |
| CAT — Catalogue & Localisation | — | — | — | — | — | — | — |
| SAV — Save Management | — | — | — | — | — | — | — |
| ACH — Achievement Tracking | — | — | — | — | — | — | — |
| ALB — Sticker Albums | — | — | — | — | — | — | — |
| PRO — Profiles & Progress | — | — | — | — | — | — | — |
| GOV — Moderation & Governance | — | — | — | — | — | — | — |
| DSC — Discovery & Search | — | — | — | — | — | — | — |
| INT — Integrations & Adapters | — | — | — | — | — | — | — |

*(Fill counts as suites/cases are created. Use the detailed matrices below to roll up numbers.)*

---
<br>

### 3.4 Requirement→Test Matrix (templates)

#### 3.4.1 BRD → Test Cases
| BRD ID | Requirement Summary | Linked Suites/Cases | Status | Notes |
|---|---|---|---|---|
| BR-ACC-01 | Email sign-up with confirmation | TS-ACC-01 → TC-ACC-001/002 | Covered | EN/UA variants split |
| BR-CAT-03 | UA/RU discovery-only banner rules | TS-CAT-02 → TC-CAT-010/011 | Covered | UA default ON; others OFF (toggle) |
| BR-SAV-02 | Upload save with AV scan & metadata | TS-SAV-01 → TC-SAV-010/012/014 | Partial | Edge case with corrupted zip pending |
| BR-ACH-05 | Submit fan completion with evidence | TS-ACH-03 → TC-ACH-020/021 | Covered | Allowlisted hosts only |
| BR-ALB-04 | Curator creates spoiler-safe template | TS-ALB-02 → TC-ALB-011 | Gap | Awaiting curator UI build |

#### 3.4.2 SRS → Test Cases
| SRS ID | Behaviour / Contract | Linked Suites/Cases | Status | Notes |
|---|---|---|---|---|
| SR-SAV-17 | Compatibility labels: confirmed/untested/legacy/incompatible | TS-SAV-02 → TC-SAV-022/023/024 | Covered | Needs PAL/NTSC + Steam build ID fixtures |
| SR-INT-06 | Steam achievements read-only sync with backoff | TS-INT-01 → TC-INT-006/007 | Covered | Staleness badge asserted |
| SR-GOV-12 | Moderator decision & audit trail | TS-GOV-02 → TC-GOV-012/013 | Covered | Appeal path tested separately |
| SR-CAT-09 | UA/RU banners only on catalogue/discovery | TS-CAT-02 → TC-CAT-010/011 | Covered | Negative check on detail pages |
| SR-ALB-08 | AI triage runs behind flag; manual override | TS-ALB-03 → TC-ALB-020/021 | Partial | Agreement sampling pending |

#### 3.4.3 Use Cases → Test Cases
| UC ID | Scenario | Linked Test Cases | Status |
|---|---|---|---|
| UC-SAV-03 | Upload → AV pass → downloadable with “confirmed” label | TC-SAV-010/012 | Covered |
| UC-ACH-04 | Link RA profile → view progress | TC-ACH-008/009 | Covered |
| UC-ALB-05 | Submit sticker → AI triage → moderator accept | TC-ALB-018/020 | Covered |
| UC-GOV-02 | Report content → moderator decision → appeal | TC-GOV-010/012/015 | Covered |
| UC-DSC-01 | Search with filters (locale, platform) | TC-DSC-004/006 | Covered |

> **How to use:** copy these templates per module; add rows for each requirement/use case; keep “Status” = *Covered / Partial / Gap*.

---
<br>

### 3.5 Gap Management
- Any **Gap/Partial** row must have: **Owner**, **ETA**, and a linked **ticket**.  
- Gaps affecting **gate criteria** (BRD Business Acceptance, Project Plan §16) are escalated in the **weekly digest**.  
- When closed, update the matrix and flip the module’s **Dashboard counts** (§3.3).

---
<br>

### 3.6 Change Impact
When a BRD/SRS/UC item changes:
1. Tag the change with IDs in the source doc’s history.  
2. Update linked **TS/TC** IDs (or create new ones) and mark impacted tests **Needs Investigation**.  
3. Re-run affected TCs in **Staging**; attach evidence; update status.

---
<br>

### 3.7 Storage & Evidence
- **Matrices** live in this section (source of truth).  
- **Execution evidence** is linked from each **TC** (screenshots, event IDs, safe logs).  
- Summaries are included in **gate packs** (Project Plan §15.8).

---
<br>
<br>

## 4. Test Approach & Levels

This section explains **how** we test Dream Project across levels (unit → service/integration → end-to-end), how feature flags are handled, and how smoke/regression/UAT runs are organized—so coverage is deep where it matters, fast where it can be, and always traceable.

---
<br>

### 4.1 Strategy in One View
- **Risk-driven + requirement-driven:** focus on user journeys and risks called out in BRD/SRS; map all tests in §3.  
- **Automation first, evidence always:** maximise automation below the UI; every executed test leaves auditable evidence.  
- **Read-only integrations:** Steam/RA/IGDB interactions are exercised with **fixtures + contract tests**; live calls run behind quotas with **staleness badges** asserted.  
- **Locales & roles:** critical flows validated in **EN and UA**, and where behaviour differs, for **Regular / Curator / Admin**.  
- **Compatibility truths:** tests explicitly assert save compatibility labels (**confirmed / untested / legacy / incompatible**) and region/build handling (Steam build ID; PAL/NTSC).  
- **Flags first, rollback ready:** new capabilities ship **flagged OFF** in Staging/Prod; tests cover both **ON/OFF** where behaviour differs.

---
<br>

### 4.2 Levels & Ownership

| Level | Purpose | Scope Focus | Env | Owner | Automation Ratio |
|---|---|---|---|---|---|
| **Unit** | Prove function/class logic and edge cases | parsers, validators, format adapters, label calculators | Dev/CI | Dev/TL | **High (80–90%)** |
| **Service / Integration** | Verify module contracts and adapter behaviour | API endpoints, persistence, AV scan stub, Steam/RA/IGDB adapters | Dev/Staging | Dev/TL | **High (70–85%)** |
| **End-to-End (E2E)** | Prove real user journeys | ACC/CAT/SAV/ACH/ALB/PRO/GOV/DSC flows | Staging | QA | **Selective (30–50%)** |
| **Exploratory / Regression** | Find gaps, verify fixes, guard against drift | High-risk areas, new UI, locale variants | Staging | QA | **Manual + charters** |
| **UAT** | Business acceptance of MVP bar | Condensed BAC journeys + negative states | Staging | BA + QA | **Manual (scripted)** |

*Tooling:* unit/service via test runners; E2E via UI runner; contract tests via schema snapshots and provider fakes.

---
<br>

### 4.3 Functional Test Taxonomy

- **Happy paths:** canonical flows per module (e.g., *Upload save → AV pass → “confirmed” label visible*).  
- **Error / edge states:** invalid input, rate-limit, stale provider data, non-allowlisted evidence, corrupted archives, incompatible saves.  
- **Empty states:** no linked providers, no albums yet, search with zero results (correct banner/copy).  
- **Role variance:** Regular vs Curator vs Admin/Moderator privileges and UI affordances.  
- **Locale variance:** EN vs UA content, plus **UA/RU discovery-only banners** limited to catalogue/discovery surfaces.  
- **Flags variance:** behaviour with feature **ON/OFF** (AI triage, album hints, etc.).  
- **Security/privacy hooks:** no tokens/PII in logs; read-only posture respected.  

All cases follow the template in §1.9 and link to BRD/SRS/UC IDs.

---
<br>

### 4.4 Non-Functional Hooks (inside functional tests)

| NFR Area | What we assert (examples) | How |
|---|---|---|
| **Performance** | p95: search ≤300 ms; detail ≤500 ms; upload-start ≤1 s | inject timers/markers; read APM/RUM panels |
| **Availability** | Up during test windows; retry/backoff doesn’t degrade UX | synthetic ping; check staleness banners |
| **Security/Privacy** | No tokens/PII in logs; AV scan invoked on upload | log scrub checks; AV stub + real scan in staging |
| **Accessibility** | Keyboard navigation, alt text/labels, contrast on key screens | axe snapshot / manual spot checks |
| **Localisation** | Correct UA strings and banner defaults; EN parity | string keys and screenshots |
| **Cost-sensitive** | Image dedupe / CDN cache hints present | header checks; storage counters |

Where a hook fails, tests log **NFR breach** with the KPI/threshold from Project Plan §15.

---
<br>

### 4.5 Automation Strategy (the “pyramid”)

1. **Unit (broad):** parsers for save metadata; compatibility calculator; banner visibility rules; evidence URL validator; RA/Steam payload mappers.  
2. **Service (contract-heavy):** REST endpoints; DB migrations; adapter contract tests using **provider fakes** and **recorded fixtures**; schema diffs break builds.  
3. **E2E (select):** one happy path per core journey + the **highest-risk negative** per module (e.g., incompatible save download; non-allowlisted evidence rejection).  
4. **Smoke:** minimal E2E set on every build to Staging; full regression weekly or per gate.

Automation does **not** attempt to cover every UI permutation—only the slices that fail loudest if broken.

---
<br>

### 4.6 Smoke vs Full Regression vs UAT

| Cycle | When | Contents | Exit |
|---|---|---|---|
| **Smoke** | each Staging deploy | 1–2 happy paths per module + health checks | 0 P1/P2; dashboards green |
| **Regression** | weekly & pre-gate | all E2E + high-value manual charters + locale/role matrices | no P1/P2; P3 allowed only with workaround/flag OFF |
| **UAT** | per gate | BAC journeys (condensed) + edge states | BAC met; sign-off pack ready (§16) |

Daily execution and weekly summaries are reported per §11.

---
<br>

### 4.7 Feature Flags & Experiments

- **Defaults:** OFF in Prod; ON in Dev; Staging per test role.  
- **Coverage:** tests specify required flag state; include a **rollback expectation** (what users see when OFF).  
- **AI triage:** runs in **shadow mode** first (compare AI vs moderator); only after ≥85% agreement can ON-path tests become mandatory.  
- **Sunset:** any long-lived flag has a removal ticket and a test plan to re-baseline.

---
<br>

### 4.8 Test Data & Fixtures

- **Games/Variants:** at least one Steam title with **2+ build IDs**; at least two retro titles (**PAL/NTSC**).  
- **Saves:** fixtures for each compatibility label; corrupted/oversized archives for negative cases.  
- **Evidence:** allowlisted sample links (YouTube, Imgur) + one intentionally **non-allowlisted** URL.  
- **Accounts:** Regular, Curator, Admin/Moderator test users.  
- **Cleanup:** tests delete created entities unless designated fixtures; storage growth monitored.

---
<br>

### 4.9 Integrations Approach

- **Provider fakes & recorded fixtures** for CI; **rate-limited live tests** in Staging (tagged `INT-*`).  
- Assert **staleness badges** when backoff triggers, and that **no write-back** is attempted.  
- Failures due to provider quotas are marked **provisional** and retried within window.

---
<br>

### 4.10 Entry/Exit per Level

**Unit**  
- *Entry:* code builds; mocks in place.  
- *Exit:* 90%+ critical path coverage; no red tests.

**Service/Integration**  
- *Entry:* DB up; adapters configured; fixtures seeded.  
- *Exit:* contracts green; schema snapshot unchanged or approved; error budgets respected.

**E2E**  
- *Entry:* Staging healthy; seeds loaded; flags as specified.  
- *Exit:* planned TCs executed; evidence stored; no P1/P2.

**UAT**  
- *Entry:* BAC checklist ready; QA dry-run passed.  
- *Exit:* sign-off bundle complete; all blockers resolved.

---
<br>

### 4.11 Evidence & Reporting

- Each executed TC stores: screenshots, event IDs, safe log snippets, environment/build tag.  
- Cycle summaries: pass/fail rates, notable defects with severity, NFR breaches mapped to KPIs.  
- Gate packs follow Project Plan §15.8; defects/decisions cross-linked by ID.

---
<br>
<br>

## 5. Test Environment & Tools

This section defines **where** tests run, **what data/tools** they use, and **how** we keep runs safe, reproducible, and observable—without leaking tokens or PII.

---
<br>

### 5.1 Environment Overview

| Env | Purpose | Data Policy | Integrations | Flags | Notes |
|---|---|---|---|---|---|
| **Dev** | Rapid developer checks | Ephemeral, reseeded often | Adapters in **fake** mode by default | New features **ON** | Not an evidence source for gates |
| **Staging** | Authoritative test runs (smoke/regression/UAT) | Stable, seeded fixtures; nightly backup | Read-only **live** calls allowed within quotas; otherwise cached fixtures | New features **OFF** by default; enabled per test role | **Source of truth** for test evidence |
| **Prod (view-only)** | Sanity reads only (no writes) | Real prod data; **no** writes | Live, read-only | All experiments **OFF** | Used sparingly for read-only sanity/monitoring checks |

> Ops & deployment practices align with SRS §8 and Project Plan §14–§17.

---
<br>

### 5.2 Configuration Matrix (Staging defaults)

| Area | Default | Variants (per test) |
|---|---|---|
| Locale | **EN** (UA runs required for critical journeys) | UA |
| Roles | Regular | Curator, Admin/Moderator |
| Providers | **Fakes + recorded fixtures** | Rate-limited live for Steam/RA/IGDB (tag `INT-*`) |
| AI Triage | **OFF** (shadow compare only) | ON after agreement ≥85% (per governance) |
| UA/RU Banners | UA locale: **ON**; others: **OFF** | Toggleable for negative checks |
| Compatibility Labels | Enabled | Admin override banner for tests needing it |

---
<br>

### 5.3 Test Accounts & Roles

| Account | Role | Use | Notes |
|---|---|---|---|
| `qa-regular@…` | Regular | Core user flows | No curator/admin rights |
| `qa-curator@…` | Curator | Author fan sets & albums | Credit visible on profiles |
| `qa-mod@…` | Admin/Moderator | Moderation queue, allowlist, overrides | Audit trail verified |
| `qa-ops@…` | Ops (limited) | Read dashboards, trigger synthetic checks | No production writes |
| `qa-steam@…` | External | Steam read-only profile | Seeded with public achievements |
| `qa-ra@…` | External | RetroAchievements profile | Seeded progress for fixtures |

> Credentials stored in the **secrets manager**; never embedded in tests or logs.

---
<br>

### 5.4 Seeded Data & Fixtures (Staging)

- **Games/Variants:**  
  - 1× modern Steam title with **≥2 build IDs** to exercise compatibility.  
  - 2× retro titles (**PAL** and **NTSC**) for region handling.
- **Saves:** one per compatibility status (**confirmed / untested / legacy / incompatible**), plus corrupted/oversized archives for negatives.
- **Fan Sets & Albums:** at least one curator-owned **fan achievement set** and one **sticker album template** (with spoiler/hint styles).
- **Evidence Links:** allowlisted examples (YouTube, Imgur) + 1 non-allowlisted URL for rejection tests.
- **Policy Banners:** UA/RU discovery-only banners configured for catalogue/discovery surfaces.

---
<br>

### 5.5 Provider Adapters & Fakes

- **Contract tests** run against **provider fakes** with recorded fixtures (schema snapshots under version control).  
- Live tests in Staging are **throttled**; on 429/5xx, switch to cached fixtures and mark results **provisional**.  
- **Staleness badges** must appear when backoff engages (assert in INT/CAT suites).  
- **No write-back** to providers—read-only posture enforced.

---
<br>

### 5.6 Tooling Stack

| Category | Tooling | Purpose | Owner |
|---|---|---|---|
| Test runner (unit/service) | Standard code runner & mocks | Logic/contracts, adapters, parsers | Dev/TL |
| E2E UI runner | Headless/browser runner | User journeys & critical negatives | QA |
| CI/CD | Pipeline with artifacts | Execute suites, store evidence, gate releases | TL/OPS |
| APM & RUM | Performance monitors | Assert p95 targets (search/detail/upload start) | TL/OPS |
| Uptime/Health | Synthetic monitors | Availability SLO checks | OPS |
| Log viewer | Centralised logs (scrubbed) | Verify no tokens/PII; correlate event IDs | TL/OPS |
| Evidence store | CI artifacts / repo `/evidence` | Screenshots, run logs, reports | QA |
| Dashboards | KPI & moderation boards | Gate snapshots & daily reviews | BA/MOD/TL |

*(Exact tool names follow team standards; this spec defines behaviour and ownership.)*

---
<br>

### 5.7 Logging, Metrics & Event IDs

- Each E2E test captures a **Run ID** and key **event IDs** to correlate UI steps with backend logs.  
- Logs are **scrubbed** (no tokens/PII). A redaction check is part of smoke runs.  
- Performance markers capture **start/stop** for typeahead, detail load, and upload start.

---
<br>

### 5.8 Data Isolation & Reset

- Staging is **reset nightly** to a known snapshot; seeds re-applied.  
- Tests **clean up** entities they create (albums, sets, saves) unless marked as fixtures.  
- Conflicting runs: use **namespaced test data** (e.g., `TC-ALB-018-<runId>`).

---
<br>

### 5.9 Secrets & Access Control

- Credentials and API keys live in **KMS/secrets manager**; accessed via CI/runner context.  
- Never print secrets; forbid logging of `Authorization` headers or tokens.  
- Roles/permissions are verified in **GOV** suites; admin-only actions must show an **authorization error** for non-admins.

---
<br>

### 5.10 Browser/Device Matrix

| Tier | Devices | Browsers |
|---|---|---|
| **Primary** | Desktop | Chrome, Edge, Firefox (current-1) |
| **Sanity** | Mobile web | Safari (iOS), Chrome (Android) |

At least one **UA** and one **EN** execution on Desktop for critical journeys; Mobile sanity for smoke.

---
<br>

### 5.11 Environment Readiness Checklist (before any cycle)

- Staging healthy; dashboards reachable; seeds loaded; **AI triage = OFF** unless scenario requires ON.  
- Test accounts valid; provider profiles accessible; quotas sufficient.  
- Evidence folders clean; CI artifacts retention set.  
- Feature flags **documented** in the cycle plan (expected ON/OFF).  
- Rollback/runbooks handy (links from Project Plan §14 & §16).

---
<br>
<br>

## 6. Test Data Management

This section defines **what test data we use, how we create/version it, where we store it, and how we clean it up**—so tests remain reliable, reproducible, and privacy-safe.

---
<br>

### 6.1 Principles
- **Synthetic by default, minimal PII** (use test names/emails; no real tokens).
- **Deterministic & versioned** (fixtures with explicit IDs and hashes).
- **Isolated per run** (namespacing to avoid cross-test interference).
- **Traceable** (each datum references the TC/TS that needs it).
- **Disposable** (cleanup on completion; nightly environment resets).
- **Provider-friendly** (Steam/RA live profiles used sparingly; otherwise fakes/recordings).

---
<br>

### 6.2 Data Types (inventory)

| Type | Examples | Purpose | Source of Truth |
|---|---|---|---|
| **Users & Roles** | `qa-regular`, `qa-curator`, `qa-mod` | Exercise role-based behaviour | Fixture seed (Staging) |
| **External Profiles** | `qa-steam` (public), `qa-ra` (RA) | Read-only achievement sync | Provider fakes + limited live |
| **Games & Variants** | Game IDs; **Steam build IDs**; **PAL/NTSC** | Drive save compatibility & locale banners | Catalogue seed (Staging) |
| **Saves** | One per status: **confirmed/untested/legacy/incompatible**; corrupted/oversized | Positive/negative save flows | Object store fixtures |
| **Fan Achievement Sets** | Curator-owned sets + per-achievement metadata | ACH flows; evidence allowlist | App DB fixtures |
| **Sticker Album Templates** | Slots, spoiler tags, hint styles | ALB authoring/submission flows | App DB fixtures |
| **Evidence Links** | Allowlisted (e.g., **YouTube**, **Imgur**) and one non-allowlisted | Proof of completion; rejection checks | Fixture list (policy YAML) |
| **Policy Banners** | UA/RU **discovery-only** toggles | Locale/policy behaviour | Config seed |
| **Compatibility Confirmations** | Community confirmations & “didn’t work” examples | SAV moderation/accuracy | App DB fixtures |

---
<br>

### 6.3 Creation & Versioning
- **Seeds (baseline):** loaded nightly into Staging from `/seeds/<YYYYMMDD>/…`.
- **Version field:** every fixture carries `version` (semver) and `source_tc` (e.g., `TC-SAV-012`).
- **Change control:** edits to seeds require a PR with updated **fixture schema** and links to impacted TCs (§3.6).

---
<br>

### 6.4 Reusable Fixtures (golden set)
- **Games/Variants**
  - **Modern:** `steam_game_A` with **≥2 build IDs** (e.g., `build_1001`, `build_1017`).
  - **Retro:** `retro_game_B (PAL)` and `retro_game_B (NTSC)`.
- **Saves**
  - `save_confirmed_build_1001.zip` (hash pinned).
  - `save_legacy_build_0987.zip` (marked **legacy**).
  - `save_incompatible_pal_on_ntsc.zip` (negative).
  - `save_corrupted_payload.zip` (AV fail path).
- **Fan Sets & Albums**
  - `fan_set_minimal_core` (10 achievements incl. 1 gradual).
  - `album_template_story_safe` (spoiler-safe hints).
- **Evidence**
  - `https://www.youtube.com/watch?v=<fixture>` (allowlisted).
  - `https://imgur.com/<fixture>` (allowlisted).
  - `https://examplefileshare.test/<fixture>` (**non-allowlisted** for rejection).

---
<br>

### 6.5 Namespacing & Run IDs
- Each execution generates `runId` (e.g., `2025-11-27T15-12-33Z`).
- Entities created by tests append `-<TCID>-<runId>` (e.g., `album-ALB-018-2025-11-27T15-12-33Z`).
- **Why:** avoids collisions between parallel runs; simplifies cleanup queries.

---
<br>

### 6.6 Fixture Schemas (authoritative)

**Save fixture**
~~~yaml
id: save_confirmed_build_1001
game_id: steam_game_A
variant:
  type: steam
  build_id: 1001
status: confirmed          # confirmed | untested | legacy | incompatible
file: /fixtures/saves/save_confirmed_build_1001.zip
sha256: <pinned-hash>
av_expected: pass          # pass | fail
metadata:
  playtime_hours: 12.4
  chapter: "3"
trace:
  srs: [SR-SAV-05, SR-SAV-17]
  uc:  [UC-SAV-03]
  tcs: [TC-SAV-010, TC-SAV-012]
version: 1.1.0
~~~

**Fan achievement set**
~~~yaml
id: fan_set_minimal_core
game_id: retro_game_B_pal
curator_user: qa-curator
achievements:
  - id: a01
    title: "First Relic"
    type: binary
    evidence: optional
  - id: a02
    title: "Treasure Hunter"
    type: incremental
    target: 50
    evidence: required
allowlisted_hosts: ["youtube.com","imgur.com"]
trace:
  srs: [SR-ACH-14, SR-GOV-09]
  tcs: [TC-ACH-020, TC-GOV-012]
version: 0.9.0
~~~

**Album template**
~~~yaml
id: album_template_story_safe
game_id: steam_game_A
slots:
  - id: s01
    title: "Prologue"
    spoiler: low
    hint: "Intro cinematic (no boss)."
  - id: s02
    title: "First Boss"
    spoiler: medium
    hint: "Arena with the broken arch."
triage:
  ai_shadow: true
  confidence_threshold: 0.85
trace:
  srs: [SR-ALB-06, SR-GOV-11]
  tcs: [TC-ALB-018, TC-ALB-020]
version: 1.0.0
~~~

---
<br>

### 6.7 Localisation & Policy Data
- **Locales:** every text fixture includes `en` and `uk` keys where UI content is asserted.
- **UA/RU banners:** configured only for **catalogue/discovery** surfaces; default **ON for UA**, **OFF** otherwise; tests toggle for negative checks.

---
<br>

### 6.8 External Providers (Steam / RA / IGDB)
- **Primary:** fakes + recorded fixtures (schema snapshots under version control).
- **Live reads (Staging):** rate-limited; on 429/5xx switch to cached; mark TC result **provisional**.
- **Staleness:** ensure “stale data” badges appear when backoff/cooldown engages (assert in INT/CAT suites).
- **No write-back** is ever attempted.

---
<br>

### 6.9 Evidence Assets
- **Allowlisted hosts only** (e.g., YouTube, Imgur) for fan achievement proof.
- Store **links**, not binaries, in test records. Screenshots from E2E runs are stored as CI artifacts.
- Moderation tests include one **non-allowlisted** URL to assert rejection path.

---
<br>

### 6.10 Cleanup & Reset
- **Per-test:** delete created entities unless explicitly tagged `retain_fixture: true`.
- **Per-cycle:** run cleanup script to purge namespaced data older than current `runId`.
- **Nightly:** Staging reset → reload seeds; rotate evidence links if providers purge content.

---
<br>

### 6.11 Retention & Access
- **Seeds & fixtures:** kept in repo under `/seeds` and `/fixtures`; reviewed via PR.
- **Execution evidence:** CI artifacts retained ≥90 days or until MVP sign-off; linked from cycle reports.
- **Access control:** only QA/TL/OPS can modify seeds; MOD can edit allowlist policy YAML via governance flow.

---
<br>

### 6.12 Data Requests & Changes
New/changed fixtures require:  
1) Schema-valid YAML,  
2) Linked TCs/requirements,  
3) Cleanup plan,  
4) Reviewer approvals (QA + TL; MOD if policy/evidence).

---
<br>

### 6.13 Quick Reference (what to use for common tests)

| Scenario | Fixture(s) |
|---|---|
| **Confirmed save download** | `save_confirmed_build_1001` |
| **Legacy warning** | `save_legacy_build_0987` |
| **Incompatible (region)** | `save_incompatible_pal_on_ntsc` |
| **AV fail on upload** | `save_corrupted_payload.zip` |
| **Fan completion (accepted)** | `fan_set_minimal_core` + allowlisted YouTube link |
| **Fan completion (rejected)** | same set + non-allowlisted URL |
| **Album submission (AI shadow)** | `album_template_story_safe` + valid screenshot |
| **Stale provider data** | Trigger backoff in INT suite (fixture timestamp past TTL) |

---
<br>
<br>

## 7. Entry & Exit Criteria

This section defines **when a test cycle may start** (entry) and **what must be true to finish** (exit). It keeps execution predictable, auditable, and aligned with MVP gates.

---
<br>

### 7.1 Purpose
- Prevent “test on shifting sand” by validating **environment readiness** up front.
- Ensure each cycle leaves a **complete, comparable evidence bundle**.
- Tie **pass/fail** to objective rules (defect policy, KPIs, BAC).

---
<br>

### 7.2 Global Preconditions (apply to all cycles)
- **Environment:** Staging is healthy (status green), seeds loaded (games/variants/fixtures), feature flags documented for the run.
- **Accounts:** `qa-regular`, `qa-curator`, `qa-mod` valid; external profiles (`qa-steam`, `qa-ra`) reachable.
- **Data:** Golden fixtures present (save statuses, album template, fan set, evidence allowlist). Namespacing with `runId` enabled.
- **Policies:** UA/RU **discovery-only** banners configured correctly (UA=ON by default; others OFF).
- **Security:** Secrets available via vault/KMS only; logging redaction checks enabled; no tokens/PII in sample logs.
- **Integrations:** Provider fakes ready; live calls rate-limited; **read-only** posture enforced; staleness badge logic active.
- **Tooling:** CI pipeline available; evidence store writable; dashboards reachable.

---
<br>

### 7.3 Entry Criteria by Cycle

| Cycle | Entry Criteria |
|---|---|
| **Smoke** | Latest Staging deploy succeeds; health checks pass; seeds reloaded ≤24h; minimal suite list approved; flags per plan. |
| **Regression** | All Smoke criteria **plus**: full suite list frozen; fixture versions pinned; provider quotas within window; AI triage kept **OFF** (shadow) unless authorised. |
| **UAT** | All Regression criteria **plus**: Business Acceptance Checklist (BAC) prepared; test scripts reviewed by BA/MOD; scope freeze for UAT window. |

---
<br>

### 7.4 Exit Criteria by Cycle

| Cycle | Exit Criteria |
|---|---|
| **Smoke** | All smoke TCs executed; **P1/P2=0**; P3/P4 logged; evidence uploaded; deployment either proceeds or rolls back per policy. |
| **Regression** | 100% planned TCs executed (or formally deferred); **P1/P2=0**; P3 allowed only with workaround or flag OFF; performance hooks met (p95: search ≤300ms, detail ≤500ms, upload-start ≤1s); logs clean (no tokens/PII). |
| **UAT** | BAC scenarios passed; sign-offs collected from BA + TL + MOD; unresolved defects documented with mitigations; release notes drafted. |

> Any failed NFR hook or policy breach must be logged as an **NFR breach** with KPI, evidence, and owner.

---
<br>

### 7.5 Gate-Level Acceptance (Alpha / Beta / MVP)

| Gate | Must Include | Pass Conditions |
|---|---|---|
| **Alpha** | Smoke + Partial Regression on core journeys (ACC, CAT, SAV, ACH) | **P1/P2=0**; core happy paths green; first cut of coverage map (§3) complete; staleness & compatibility labels validated on at least one title (Steam build + PAL/NTSC). |
| **Beta** | Full Regression + UAT (condensed) | **P1/P2=0**; P3 with workaround/flag OFF; moderation median ≤24h (sample); accessibility spot checks pass; UA localisation validated on critical screens; evidence bundles posted. |
| **MVP** | Full Regression + Full UAT pack | **P1/P2=0**; no open NFR breaches against targets; documentation/evidence complete; all BAC criteria satisfied; sign-off pack archived. |

---
<br>

### 7.6 Defect Policy (Severity, Priority, SLAs)

| Severity | Impact | Target SLA (Staging) | Exit Rule |
|---|---|---|---|
| **P1** | Security/data loss, platform unusable | Immediate fix/rollback | Must be **0** |
| **P2** | Core flow broken, no workaround | 1 business day | Must be **0** |
| **P3** | Important defect with workaround/flag OFF | 3–5 business days | Allowed with mitigation |
| **P4** | Minor issue/cosmetic | Backlog | Allowed |

All defects require: repro steps, environment/build tag, screenshots or safe logs, and **traceability IDs** (BR/SR/UC/TC).

---
<br>

### 7.7 Evidence Requirements (per cycle)
- **Artifacts:** screenshots, event IDs, safe log snippets, timing markers, coverage matrices (updated).
- **Metadata:** run date/time, environment, build tag, feature flag states, locales/roles executed.
- **Storage:** CI artifacts (≥90 days) + links in the cycle report; sign-off PDFs for UAT/MVP.

---
<br>

### 7.8 Blockers, Waivers & Deferrals
- **Blocked** TCs list the blocking ID (defect, dependency, data).  
- **Waivers**: only PL + TL may approve; must include risk statement, temporary mitigation, and expiry date.  
- **Deferrals**: appear in §11 cycle report with owner/ETA and are fed back into the **Coverage Map** (§3) as gaps.

---
<br>

### 7.9 Re-Run & Rollback Policy
- **Smoke fail:** halt deployment, investigate, re-run smoke after fix; if unresolved, rollback.  
- **Regression fail:** triage immediately; re-run affected suites post-fix; if risk high and time low, **flag OFF** feature and re-baseline tests.  
- **UAT fail:** create decision note; either fix-and-retest within window or move item out of MVP scope with stakeholder approval.

---
<br>

### 7.10 Sign-Off Roles
- **Smoke:** TL (technical), QA (execution).  
- **Regression:** TL + QA + OPS (NFR/KPIs).  
- **UAT/MVP:** BA (business), MOD (governance), TL (technical), PL (final sign-off).

> Sign-off is recorded in the cycle report and mirrored in the **Project Plan** change history.

---
<br>
<br>

## 8. Defect Severity, Priority & Workflow

This section defines **how we classify, file, triage, fix, and close defects** so that release gates remain objective and repeatable.

---
<br>

### 8.1 Definitions
- **Severity** — how **bad** the defect’s impact is (platform, data, users). Set by **QA** at report time; may be adjusted in triage.
- **Priority** — how **soon** we should fix it, considering release timing, reach, and workarounds. Set by **Tech Lead (TL)** in triage with BA/PL input.

---
<br>

### 8.2 Severity Scale (P1–P4)

| Sev | Definition | Dream Project examples |
|---|---|---|
| **P1 – Critical** | Security/data loss, legal/compliance breach, or the platform is unusable. | Saves downloadable by wrong user; tokens/PII in logs; sign-in broken for all; upload path corrupts files; admin actions available to non-admin. |
| **P2 – High** | Core flows blocked with no acceptable workaround. | Upload fails for valid saves (all users); Steam/RA linking blocked; catalogue page cannot render; moderation queue not loading. |
| **P3 – Medium** | Important defect; workaround exists or scope limited. | Wrong compatibility label shown; UA/RU banner misplacement on one page; AI triage badge not shown (manual review still possible). |
| **P4 – Low** | Minor/cosmetic or edge copy/spacing issues; doesn’t impede tasks. | Typos; small spacing issues; non-critical tooltip mismatch. |

> **Exit rule (from §7.6):** **P1/P2 must be 0** at cycle exit. P3 allowed only with workaround or feature **flag OFF**. P4 may remain in backlog.

---
<br>

### 8.3 Priority Scale (High/Medium/Low)

| Priority | Guidance |
|---|---|
| **High** | Must land in current cycle to meet gate; high user reach or high risk of regression. |
| **Medium** | Fix within planned sprints; mitigations/workarounds acceptable until then. |
| **Low** | Defer safely; bundle with housekeeping/UI polish waves. |

**Default mapping (can be overridden in triage):** P1→High, P2→High, P3→Medium, P4→Low.

---
<br>

### 8.4 SLAs (Staging)

| Severity | First Response (triage) | Fix/Workaround Target | Verification |
|---|---|---|---|
| **P1** | ≤ 30 minutes | Immediate fix or rollback | Same day |
| **P2** | ≤ 2 hours | ≤ 1 business day | Next business day |
| **P3** | ≤ 1 business day | 3–5 business days (or flag OFF) | Within 1 day of delivery |
| **P4** | ≤ 3 business days | Backlog | With scheduled batch |

---
<br>

### 8.5 Workflow & States

**Canonical flow:** `New → Triage → Assigned → In Progress → Fixed (Awaiting QA) → Verified → Closed`  
**Alternative states:** `Duplicate`, `Not a bug`, `Deferred`, `Won’t fix`, `Reopened`

1. **New** — Filed by QA (or Dev/BA) with **proposed severity** and full evidence.  
2. **Triage** — TL + QA (+ BA/PL if needed) confirm severity, set priority, owner, and target cycle.  
3. **Assigned/In Progress** — Dev investigates; links PRs/commits. If blocked, mark **Blocked** with dependency.  
4. **Fixed (Awaiting QA)** — Build tag noted; notes on risk areas and flags touched.  
5. **Verified** — QA re-tests in Staging (EN/UA and role variants if affected).  
6. **Closed** — All acceptance checks pass; coverage/traceability updated.  
7. **Reopened** — Repro persists or fix regressed elsewhere; link new/affected TCs.

**De-duplication:** mark duplicates with `Duplicate of <BUG-ID>` and link to the root ticket.

---
<br>

### 8.6 Filing Requirements (what every bug must include)

- **Title** — imperative, module first: `SAV: AV pass, but label shows "untested" (EN, Desktop)`.  
- **Environment/Build** — Dev/Staging, build tag/commit, feature flags (ON/OFF).  
- **Severity (proposed)** and **Priority (blank until triage)**.  
- **Steps to Reproduce** — numbered, minimal branching.  
- **Expected vs Actual** — observable assertions (texts, labels, states).  
- **Evidence** — screenshots, event IDs, and **safe** log snippets (no tokens/PII).  
- **Traceability** — `BR-…`, `SR-…`, `UC-…`, and failing `TC-…` IDs.  
- **Matrix** — role, locale (EN/UA), device, browser.  
- **Repro Rate** — always/often/intermittent; include runId if from a suite execution.

_Bug header template (copy-paste):_
~~~yaml
module: SAV
title: 'SAV: AV pass, but label shows "untested" (EN, Desktop)'
environment: Staging
build: 2025.11.27-rc1
flags: { ai_triage: OFF }
severity: P3            # proposed
priority:               # set in triage
role: Regular
locale: EN
device: Desktop
browser: Chrome
steps:
  - Upload save_confirmed_build_1001.zip
  - Wait for AV result → open save details
expected: '"confirmed" label is shown'
actual: '"untested" label shown'
evidence:
  screenshots: [link]
  event_ids: [evt-12345]
  logs_safe: [link]
traceability:
  brd: [BR-SAV-02]
  srs: [SR-SAV-17]
  uc:  [UC-SAV-03]
  tcs: [TC-SAV-012]
repro_rate: always
runId: 2025-11-27T15-12-33Z
~~~

---
<br>

### 8.7 Special Categories & Escalation

| Category | Examples | Extra Rules | Escalation |
|---|---|---|---|
| **Security/Privacy** | tokens/PII in logs; unauthorized access | Treat as **P1** by default; limit visibility; create incident note | TL + OPS immediately |
| **Policy/Moderation** | non-allowlisted evidence accepted; UA/RU banner violations | MOD reviews; may trigger temporary **flag OFF** | MOD + TL |
| **External Provider** | Steam/RA/IGDB outage/rate-limit | Mark result **provisional**; assert staleness badge; backoff | INT owner + TL |
| **NFR Breach** | p95 search >300ms; upload start >1s | File as **NFR breach** with KPI/evidence | TL + OPS |

---
<br>

### 8.8 Dependencies, Duplicates, Regressions
- **Dependencies:** link blocking issues/PRs; bugs may be **Blocked** until dependency resolves.  
- **Duplicates:** close as `Duplicate`; ensure root ticket’s **reach** and **evidence** are updated.  
- **Regressions:** label `regression`; must link the **previously passing TC** and the change (PR/flag) that caused it.

---
<br>

### 8.9 Deferrals & Waivers (summary; see §7.8)
- **Deferral** — allowed only for P3/P4 with mitigation; must include **owner, ETA, risk statement**.  
- **Waiver** — PL + TL approval; expiry date; re-reviewed at each gate.

---
<br>

### 8.10 Reopen Criteria
- Repro persists with same steps **on the declared fixed build**.  
- Side effects introduced (fix broke another module/locale/role).  
- Evidence updated; severity/priority re-evaluated in triage.

---
<br>

### 8.11 Metrics & Reporting (fed into §11)
- Open bugs by severity/priority and **MTTR**.  
- **Reopen rate** per module.  
- **Escaped defects** caught in UAT/post-release.  
- Burn-down toward gates (Alpha, Beta, MVP) with pass/fail by suite and NFR breaches.

---
<br>
<br>

## 9. Test Suites by Module

This section describes **how test cases are grouped into suites for each functional module** of Dream Project. The goal is to make it easy to:

- see **what exactly we test** per module (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT),
- understand which suites are **mandatory for Smoke / Regression / UAT**,
- map each suite back to **BRD / SRS / Use Cases / Risks**, and
- know **where to add new tests** when features evolve.

We are not repeating full test case details here. Instead, this section acts as a **navigation and planning layer** over the underlying `TC-…` cases.

---
<br>

### 9.1 Suite Structure & Naming

Each suite groups test cases by **module and purpose**:

- **Suite ID:** `TS-<MODULE>-NN` (e.g., `TS-SAV-01`).
- **Title:** short and imperative (e.g., “Core Save Upload & Download”).
- **Module:** one of `ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT`.
- **Purpose:** what risk/behaviour this suite focuses on (e.g., “prevent data loss in save uploads”).
- **Cycle Tag(s):** `Smoke`, `Regression`, `UAT`, or `NFR-heavy`.
- **Traceability:** list of relevant `BR-…`, `SR-…`, `UC-…`, and key `RISK-…` / `ASM-…` IDs.
- **NFR Hooks:** which performance / privacy / policy checks are asserted within this suite.

Each module will later have its own subsection:

- **9.x `<MODULE> – <Module name>`**  
  - Short overview of the module’s test strategy.  
  - Table of suites (`TS-…`) with purpose, cycle tags, and key coverage.  
  - Notes on **role/locale/variant** matrices that apply to this module.

---
<br>

### 9.2 Relationship to Individual Test Cases

- A **suite** is a **logical grouping**: it lists `TC-<MODULE>-NNN` that are typically executed together.
- A single **TC** can belong to more than one suite if it is reused (for example, both **Smoke** and **Regression**).
- Suites are the main planning unit for:
  - defining **Smoke vs Regression vs UAT** content,
  - scoping **per-release execution**, and
  - reporting **coverage by module** (see §3 Coverage & Traceability Map).

When a new feature is added in a module:

1. Create/update the relevant **TC-…** cases.
2. Assign them into one or more **TS-…** suites in this section.
3. Update **coverage tables** in §3 accordingly.

---
<br>

### 9.3 Cross-Cutting Rules for Suites

For all modules:

- **Roles:** where behaviour differs, suites must include coverage for **Regular / Curator / Admin/Moderator** as applicable.
- **Locales:** at least one EN and one UA execution in critical suites (particularly for CAT, PRO, ALB, GOV).
- **Integrations:** suites that depend on external providers (Steam, RA, IGDB) must clearly mark whether they use **fakes/fixtures** or **live** calls, and what staleness/backup behaviour is expected.
- **Compatibility:** any suite touching saves or variants must include checks for **confirmed / untested / legacy / incompatible** where relevant.
- **Policy:** suites that can surface UA/RU or evidence-allowlist rules must include positive and negative cases.

---
<br>
<br>

### 9.4 ACC — Accounts & Authentication

This module covers **sign-up, sign-in, sign-out, password recovery, and session security**. The focus is to ensure that:

- users can reliably create and access accounts,
- sessions are secure and expire correctly,
- abuse (brute-force, guessing) is limited without hurting legitimate users, and
- localisation (EN/UA) and policy copy stay consistent.

ACC suites are mostly **high impact** for every cycle: Smoke, Regression, and UAT.

---

#### 9.4.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-ACC-01** | Core Sign-Up & Sign-In | Smoke, Regression, UAT | Prove that new users can register, confirm email, sign in, and sign out safely. | Email registration, confirmation link, first login, logout; EN/UA variants; Regular vs Curator/Admin visibility of options. |
| **TS-ACC-02** | Password Reset & Account Recovery | Regression, UAT | Ensure users can safely restore access without exposing account data. | “Forgot password” flow, reset tokens, expired/invalid links, success confirmation; EN/UA content; basic abuse checks (too many requests). |
| **TS-ACC-03** | Session Management & Security | Regression, NFR-heavy | Validate session lifecycle and protections against session-related issues. | Idle timeout, explicit logout, concurrent sessions rules, remember-me (if implemented), secure cookie flags, no tokens/PII in logs. |
| **TS-ACC-04** | Rate Limiting & Abuse Protection | Regression, NFR-heavy | Confirm safeguards for brute-force attempts and noisy integrations. | Login attempt throttling, sign-up throttling per IP/email, error messaging without leaking which field is wrong, basic lockout behaviour. |

---

#### 9.4.2 TS-ACC-01 Core Sign-Up & Sign-In

**Purpose:**  
Validate that **new and existing users** can smoothly register, confirm their account, sign in, and sign out in both **EN and UA**, without security regressions.

**Cycle Tags:** `Smoke`, `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-ACC-01`, `BR-ACC-02`
- SRS: `SR-ACC-01`–`SR-ACC-08`
- Use Cases: `UC-ACC-01 Sign up`, `UC-ACC-02 Sign in & sign out`
- Risks: `RISK-SEC-01`, `RISK-UAT-01`

**Typical included test cases (examples):**
- `TC-ACC-001` – Email sign-up (EN, Regular user)
- `TC-ACC-002` – Email sign-up (UA, Regular user)
- `TC-ACC-003` – Email confirmation link (valid / expired)
- `TC-ACC-004` – First sign-in after confirmation
- `TC-ACC-005` – Sign-out and redirect behaviour
- `TC-ACC-006` – Failed sign-in with wrong password (generic error, no leakage)

**NFR hooks:**
- Response time for sign-in ≤ acceptable limits (e.g., p95 within 500 ms under normal load).
- No tokens/PII in logs for any part of the flow.

---

#### 9.4.3 TS-ACC-02 Password Reset & Account Recovery

**Purpose:**  
Prove that **locked-out or returning users** can recover their accounts safely via password reset, with **token security** and correct localisation.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-ACC-03`
- SRS: `SR-ACC-09`–`SR-ACC-12`
- Use Cases: `UC-ACC-03 Password reset`
- Risks: `RISK-SEC-02`, `RISK-UX-03`

**Typical included test cases (examples):**
- `TC-ACC-020` – Request password reset (EN)
- `TC-ACC-021` – Request password reset (UA)
- `TC-ACC-022` – Use valid reset link to set a new password
- `TC-ACC-023` – Expired reset link shows safe error
- `TC-ACC-024` – Invalid/forged token does not reveal whether email exists
- `TC-ACC-025` – After reset, old password no longer works

**NFR hooks:**
- Reset emails generated within expected time window.
- No secrets (tokens, raw email contents) written to logs.

---

#### 9.4.4 TS-ACC-03 Session Management & Security

**Purpose:**  
Validate **session lifecycle**, including idle timeout, explicit logout, and security flags, to support a secure, predictable user experience.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ACC-04`
- SRS: `SR-ACC-13`–`SR-ACC-17`
- Use Cases: `UC-ACC-04 Session lifecycle`
- Risks: `RISK-SEC-03`, `RISK-OPS-01`

**Typical included test cases (examples):**
- `TC-ACC-040` – Session created on successful login; secure cookie flags set
- `TC-ACC-041` – Idle session timeout logs user out with clear messaging
- `TC-ACC-042` – Logout clears session and prevents back-button access
- `TC-ACC-043` – Session not shared across different browsers/devices improperly
- `TC-ACC-044` – (If applicable) “Remember me” duration and behaviour

**NFR hooks:**
- Session timeout within configured limits.
- Confirm cookies use `HttpOnly`, `Secure` (in HTTPS), and appropriate `SameSite` setting.
- No session identifiers written in cleartext logs.

---

#### 9.4.5 TS-ACC-04 Rate Limiting & Abuse Protection

**Purpose:**  
Ensure the platform resists **basic credential stuffing and noisy behaviour** without leaking sensitive information or locking out legitimate users too aggressively.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ACC-05`
- SRS: `SR-ACC-18`–`SR-ACC-21`
- Use Cases: touches `UC-ACC-02`, `UC-ACC-03`
- Risks: `RISK-SEC-04`, `RISK-AVAIL-01`

**Typical included test cases (examples):**
- `TC-ACC-060` – Multiple failed login attempts trigger rate limit and generic error
- `TC-ACC-061` – Rate-limited user recovers after cooldown
- `TC-ACC-062` – Excessive sign-up attempts per IP/email trigger throttling
- `TC-ACC-063` – Rate limiting messages do **not** reveal whether a given email exists
- `TC-ACC-064` – Abuse of password reset endpoint is throttled

**NFR hooks:**
- Rate limiting thresholds and cooldowns within configured bounds.
- No meaningful degradation to legitimate users during typical usage.
- Monitoring events logged for abuse patterns (without PII).

---
<br>
<br>

### 9.5 CAT — Catalogue & Localisation Flags

This module covers **how games are discovered and displayed** in Dream Project: search, filters, platform variants (Steam build IDs, PAL/NTSC), and **localisation-specific policy flags** (especially UA/RU-related banners). The goal is to make sure:

- users can reliably find the game/variant they need for saves, achievements, or albums,
- they see **correct variant context** (modern vs retro, Steam build vs PAL/NTSC),
- **Ukrainian localisation** and **Russia-related policy warnings** behave exactly as designed, and
- external metadata (covers, genres, years, tags) handles staleness or gaps gracefully.

CAT suites are **core** for Smoke/Regression and highly visible in UAT.

---

#### 9.5.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-CAT-01** | Core Search & Browse | Smoke, Regression, UAT | Ensure players can search and browse the catalogue by title/platform and land on the correct game pages. | Basic/advanced search, filters, empty states; EN/UA; Regular vs Curator vs Admin views. |
| **TS-CAT-02** | Variants & Compatibility Surfacing | Regression, UAT | Validate that platform variants are clearly displayed and tied to save compatibility info. | Steam build IDs, PAL/NTSC variants, “save features available/unavailable”, game where saves are not shareable. |
| **TS-CAT-03** | Localisation & UA/RU Policy Banners | Regression, NFR-heavy, UAT | Confirm UA-specific behaviour (localisation flags, RU-related warnings) appears **only** where intended. | UA vs EN catalogue view, UA/RU discovery-only banners, “at your own risk” copy, no banners where they shouldn’t be. |
| **TS-CAT-04** | Metadata, External Sources & Staleness | Regression, NFR-heavy | Cover titles, genres, years, and Ukrainian-related flags coming from external metadata sources and handling stale/missing data. | IGDB + UA-specific sources integration (via INT), “stale data” badges, missing-cover placeholders, fallbacks. |

---

#### 9.5.2 TS-CAT-01 Core Search & Browse

**Purpose:**  
Prove that **search and browse** behave predictably: users can find a game by name/platform, see a clear list of matching titles, and navigate to the right detail page regardless of locale or role.

**Cycle Tags:** `Smoke`, `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-CAT-01 Search`, `BR-CAT-02 Basic filters`
- SRS: `SR-CAT-01`–`SR-CAT-06`
- Use Cases: `UC-DSC-01 Search`, `UC-CAT-01 Browse catalogue`
- Risks: `RISK-DISC-01` (users can’t find games), `RISK-UX-02` (confusing results)

**Typical included test cases (examples):**
- `TC-CAT-001` – Search by exact title (EN, Desktop, Regular user)
- `TC-CAT-002` – Search by partial title with typeahead suggestions (EN/UA)
- `TC-CAT-003` – Filter by platform (Steam vs Retro) and verify results
- `TC-CAT-004` – Browse by “All games” and paginate
- `TC-CAT-005` – Empty search result (safe “no results” message; suggestion text)
- `TC-CAT-006` – Curator/Admin sees additional controls (e.g., “Edit metadata”) where applicable

**NFR hooks:**
- Typeahead/search response p95 ≤ 300 ms under normal load.
- No PII or tokens in logs for search queries.
- Search remains responsive with larger seeded catalogues.

---

#### 9.5.3 TS-CAT-02 Variants & Compatibility Surfacing

**Purpose:**  
Ensure that **game variants** (Steam build IDs, PAL vs NTSC, remasters vs originals) are clearly presented and wired into the **save compatibility layer**, so users understand when save-sharing is possible or not.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-SAV-02` (save compatibility visibility), `BR-CAT-03`
- SRS: `SR-CAT-07`–`SR-CAT-10`, `SR-SAV-17` (compatibility statuses)
- Use Cases: `UC-CAT-02 Select variant`, `UC-SAV-03 Upload & download with labels`
- Risks: `RISK-SAV-01` (users try incompatible saves), `RISK-UX-03` (confusing variant selection)

**Typical included test cases (examples):**
- `TC-CAT-020` – Steam title with 2 build IDs: both variants shown and selectable
- `TC-CAT-021` – Retro title with PAL and NTSC variants visible and labelled
- `TC-CAT-022` – Variant clearly indicates whether **save-sharing is supported or not** (e.g., online-only game)
- `TC-CAT-023` – For a variant with known incompatible saves, catalogue links explain limitations and show correct label (“save feature unavailable”)
- `TC-CAT-024` – Variant change updates the messaging shown in Save Management (cross-link with TS-SAV-02)
- `TC-CAT-025` – Admin overrides variant compatibility label (e.g., from untested → confirmed) and users see updated label with admin note

**NFR hooks:**
- Variant selection does not materially slow detail page load (p95 ≤ 500 ms).
- Compatibility labels correctly mapped from backend flags; no missing/“unknown” labels on seeded fixtures.
- Logs include variant IDs/build IDs but no user-identifying data.

---

#### 9.5.4 TS-CAT-03 Localisation & UA/RU Policy Banners

**Purpose:**  
Validate that **localisation** (EN/UA) and **policy banners** behave exactly as agreed:

- UA users can quickly see whether a game has **Ukrainian localisation** or is a **Ukrainian-developed** title.
- Titles with Russian publishers/developers (or otherwise flagged) show **“at your own risk”** style warnings and explanatory copy.
- UA/RU policy banners appear **only** on catalogue/discovery surfaces and never leak into unrelated screens.

**Cycle Tags:** `Regression`, `NFR-heavy`, `UAT`

**Traceability (examples):**
- BRD: `BR-CAT-04 Localisation flags`, `BR-POL-01 UA/RU notices`
- SRS: `SR-CAT-11`–`SR-CAT-15`
- Use Cases: `UC-CAT-03 View localisation info`, `UC-DSC-02 Filter by Ukrainian localisation`
- Risks: `RISK-POL-01` (wrong or missing warning), `RISK-LOC-01` (UA users see Russian content without context)

**Typical included test cases (examples):**
- `TC-CAT-040` – EN user, neutral game: no UA/RU policy banners shown
- `TC-CAT-041` – UA user, neutral game: no explicit RU warning; just standard UA localisation indicators if applicable
- `TC-CAT-042` – UA user, game flagged with Russian publisher: catalogue card and detail view show “at your own risk” style warning and contextual text
- `TC-CAT-043` – UA user filters “Ukrainian localisation only”: only games with Ukrainian UI/audio/subs appear
- `TC-CAT-044` – UA user filters “Ukrainian-developed games” (where data available) and sees credited information
- `TC-CAT-045` – Ensure UA/RU banners and warnings are **not** shown on screens that shouldn’t display them (e.g., sticker album editor for a game that’s not flagged)
- `TC-CAT-046` – Admin updates localisation flags (e.g., newly added UA localisation); catalogue updates labels accordingly

**NFR hooks:**
- All UA strings correctly loaded; no mixed-language artefacts in key flows.
- Policy banners do not block navigation or hide essential actions.
- No policy-related data or flags are exposed via public APIs beyond what is intended to be user-visible.

---

#### 9.5.5 TS-CAT-04 Metadata, External Sources & Staleness

**Purpose:**  
Make sure that **metadata** pulled from external sources (e.g., generic databases like IGDB plus UA-specific sources) is displayed correctly, and that **stale or missing data** is handled gracefully with explicit “stale” or “unknown” states instead of silent failure.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-CAT-05 Metadata`, `BR-INT-02`
- SRS: `SR-INT-03`–`SR-INT-06`, `SR-CAT-16`–`SR-CAT-19`
- Use Cases: `UC-INT-01 Refresh metadata`, `UC-CAT-04 View game details`
- Risks: `RISK-INT-01` (inconsistent metadata), `RISK-UX-04` (broken covers/genres)

**Typical included test cases (examples):**
- `TC-CAT-060` – Game with complete metadata (cover, year, genre, platform tags) shows correct info from fixtures
- `TC-CAT-061` – Game with missing cover image shows a neutral placeholder instead of broken UI
- `TC-CAT-062` – External metadata older than TTL shows a “stale data” badge but remains viewable
- `TC-CAT-063` – When metadata refresh fails (e.g., provider 5xx or quota exceeded), the catalogue still loads, and detail page shows a gentle “unable to refresh right now” notice; last-known-good data retained
- `TC-CAT-064` – UA-specific info (e.g., “has Ukrainian localisation”, “Ukrainian developer”) is displayed when available from UA-oriented sources or manual flags
- `TC-CAT-065` – Admin corrects or overrides metadata (e.g., wrong developer attribution); override is respected and clearly labelled as such in internal/admin views

**NFR hooks:**
- Metadata refresh tasks do not slow down user-facing pages (refresh async; detail pages rely on cached data).
- Staleness and error conditions are surfaced through **user-friendly banners**, not cryptic errors.
- Logs capture provider error/status codes without leaking API keys or secrets.

---
<br>
<br>

### 9.6 SAV — Save Management

This module covers **uploading, storing, browsing, and downloading save files**, plus **compatibility signalling** between game variants (Steam build IDs, PAL/NTSC, etc.) and games that simply **do not support safe save-sharing**.

The objectives are to ensure that:

- users can safely upload and download saves (their own and, when permitted, others’),
- saves are clearly tied to the **correct game variant** and labelled as **confirmed / untested / legacy / incompatible**,
- users can **find “useful” saves** (e.g., after a difficulty spike or before a key branch) without confusion,
- games that cannot safely support save sharing are **explicitly marked**, and
- retention, quotas, and deletion behave predictably and safely.

---

#### 9.6.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-SAV-01** | Core Upload & Download | Smoke, Regression, UAT | Prove that users can upload saves with correct metadata and download them again safely. | AV scan, metadata capture, public/private visibility, basic download; EN/UA; Regular vs Curator/Admin behaviours. |
| **TS-SAV-02** | Compatibility & Variants | Regression, UAT, NFR-heavy | Validate compatibility labels and variant wiring (Steam build IDs, PAL/NTSC, games that do not support sharing). | `confirmed/untested/legacy/incompatible`, variant selection, “save feature unavailable” messaging, admin override, community confirmations. |
| **TS-SAV-03** | Save Discovery & Usage Scenarios | Regression, UAT | Check that players can search, filter, and meaningfully use shared saves (e.g., skip a spike, explore alternate branch). | Search by chapter/branch/difficulty, usage reasons, visibility rules, basic UX around “continue from here”. |
| **TS-SAV-04** | Retention, Archival & Safety | Regression, NFR-heavy | Ensure quotas, versioning, archival, and deletion behave safely and visibly. | Per-game limits, auto-archiving old versions, legacy-marking after updates, user-initiated delete with clear warnings and no data leaks. |

---

#### 9.6.2 TS-SAV-01 Core Upload & Download

**Purpose:**  
Ensure that a user can **upload a save**, pass AV scanning, capture required metadata, and **download** that save later (including saves from other users when allowed), with clear and safe UX.

**Cycle Tags:** `Smoke`, `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-SAV-01 Core save management`, `BR-SAV-02 Compatibility visibility`
- SRS: `SR-SAV-01`–`SR-SAV-08`
- Use Cases: `UC-SAV-01 Upload save`, `UC-SAV-02 Download save`
- Risks: `RISK-SAV-01` (data loss), `RISK-SEC-01` (malicious file), `RISK-UX-01` (confusing flows)

**Typical included test cases (examples):**
- `TC-SAV-001` – Upload valid save for a supported game variant (EN; Regular user); AV scan passes; save appears in user’s list.
- `TC-SAV-002` – Upload valid save (UA UI); metadata (game, platform, variant, playtime, chapter) is correctly captured from user input/form.
- `TC-SAV-003` – Upload corrupted or obviously invalid archive; AV scan or validation fails; user sees clear error; file not stored.
- `TC-SAV-004` – Download own save and verify metadata and status are consistent (e.g., “confirmed” or “untested” as appropriate).
- `TC-SAV-005` – Download another user’s **public** save; permissions and attribution (who created it) are clear.
- `TC-SAV-006` – Private save is visible only to owner and Admin/Moderator (not to other Regular users).
- `TC-SAV-007` – Upload fails mid-stream (network glitch) → no partial/corrupt save entry left hanging; user gets a retry option.

**NFR hooks:**
- Upload start time within acceptable bounds (p95 ≤ 1 s to receive feedback that upload has begun).
- AV scans invoked for every upload that passes basic file checks; no bypass.
- Logs store high-level events (upload success/fail, AV result) but no raw file content or sensitive local paths.

---

#### 9.6.3 TS-SAV-02 Compatibility & Variants

**Purpose:**  
Ensure compatibility signals and variant wiring are correct so users **know where a save will and will not work**, and avoid accidental attempts on incompatible builds/regions or games that fundamentally do not support safe save-sharing.

**Cycle Tags:** `Regression`, `UAT`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-SAV-02` (save compatibility visibility), `BR-CAT-03` (variants)
- SRS: `SR-SAV-09`–`SR-SAV-20`, `SR-CAT-07`–`SR-CAT-10`
- Use Cases: `UC-SAV-03 Upload with compatibility labels`, `UC-CAT-02 Select variant for saves`
- Risks: `RISK-SAV-02` (users brick progress with wrong save), `RISK-SAV-03` (false “confirmed” labels)

**Typical included test cases (examples):**
- `TC-SAV-020` – Save uploaded for Steam game with build ID 1001 → catalogue/variant UI shows **build 1001** and initial status `untested`.
- `TC-SAV-021` – After community confirmations reach threshold (e.g., several users report “worked”), status auto-updates to `confirmed` with visible explanation.
- `TC-SAV-022` – Negative confirmations push status to `incompatible`, or at least prevent it from being marked `confirmed`.
- `TC-SAV-023` – Retro game with PAL variant: save clearly labelled as PAL; if user views from NTSC variant, UI warns “This save was created on PAL; may not work on NTSC.”
- `TC-SAV-024` – Game flagged as **“does not support save sharing”** (e.g., account-bound online service); upload entry points disabled; users see clear explanation that save-sharing isn’t offered for this title.
- `TC-SAV-025` – After Steam game is updated to a newer build, previously “confirmed” saves are automatically re-labelled as `legacy` with a banner explaining that the game has updated.
- `TC-SAV-026` – Admin/Moderator manually overrides a compatibility label (e.g., from `untested` to `confirmed` after off-platform verification); override is visible in admin/audit view and reflected in UI.
- `TC-SAV-027` – Save from an obviously wrong variant (e.g., DLC-only save applied to base game) is flagged or prevented if detectable, with honest messaging.

**NFR hooks:**
- Compatibility label calculations are deterministic and repeatable (same inputs → same labels).
- No compatibility inference logic runs on the client that contradicts server truth.
- Logging captures variant IDs/build IDs and high-level decisions but not game content.

---

#### 9.6.4 TS-SAV-03 Save Discovery & Usage Scenarios

**Purpose:**  
Validate that users can **find and use saves for practical scenarios**, not just raw upload/download. This includes skipping a difficult segment, exploring alternate branches, or trying different builds/character setups.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-SAV-03` (save discovery), `BR-SAV-04` (scenario-based usage)
- SRS: `SR-SAV-21`–`SR-SAV-27`
- Use Cases: `UC-SAV-04 Search shared saves`, `UC-SAV-05 Resume from another user’s save`
- Risks: `RISK-UX-04` (users overwhelmed/confused), `RISK-SAV-04` (wrong expectations about where a save leads)

**Typical included test cases (examples):**
- `TC-SAV-040` – Search shared saves for a game by chapter/level → returns list with clear “where in the game” indicators.
- `TC-SAV-041` – Filter shared saves by **use-case tags** (e.g., “before major choice”, “after hard boss”, “alternate build”) and verify that tags appear correctly.
- `TC-SAV-042` – View a shared save’s detail page: shows game, variant, difficulty, playtime, chapter, and any notes from the uploader (“Hard boss defeated; you can explore area afterwards”).
- `TC-SAV-043` – User downloads a save tagged as “before major branch”; UI warns that continuing from this point may spoil parts of the story, with option to proceed.
- `TC-SAV-044` – If a game does not support shared saves (per TS-SAV-02), discovery UI explains this and hide save search for that title.
- `TC-SAV-045` – Curator or Admin can feature/spotlight certain shared saves (e.g., community-recommended branch points) and these appear prominently in search/browse results.
- `TC-SAV-046` – When there are no shared saves for a game, empty state appears with clear messaging and (optional) prompt to upload your own.

**NFR hooks:**
- Save search/filter behaviour meets the same performance targets as general catalogue search (p95 ≤ 300 ms for queries).
- No personal data beyond username/avatar and voluntary notes are displayed.
- Pagination and ordering behave consistently across locales/roles.

---

#### 9.6.5 TS-SAV-04 Retention, Archival & Safety

**Purpose:**  
Ensure that **storage limits, versioning, archival, and deletion** work predictably and safely so that users do not accidentally lose important saves and the platform stays manageable.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-SAV-05` (retention & quotas)
- SRS: `SR-SAV-28`–`SR-SAV-34`
- Use Cases: `UC-SAV-06 Manage versions & archive`
- Risks: `RISK-SAV-05` (unplanned data loss), `RISK-OPS-02` (storage runaway)

**Typical included test cases (examples):**
- `TC-SAV-060` – Per-game version cap: uploading a 26th save when the limit is 25 automatically archives (or prompts to delete) oldest version with clear messaging.
- `TC-SAV-061` – User manually deletes an old save; confirmation step is clear; save disappears from lists and is no longer downloadable.
- `TC-SAV-062` – Save marked as `legacy` due to game update is moved to an **archive** section where users can still download but see warnings.
- `TC-SAV-063` – Automatic archival/deletion schedule (if configured) respects grace periods and sends notifications if applicable.
- `TC-SAV-064` – Admin/Moderator can remove a malicious or inappropriate save; owner sees an appropriate notice (e.g., via activity log) but no sensitive moderation details.
- `TC-SAV-065` – After deletion/archival, no stale links remain in search results or profile stats.

**NFR hooks:**
- Archival operations do not significantly impact normal user actions (offloaded to background jobs where possible).
- No “hard delete” of data happens without appropriate safeguards/logging; audit trail available to Admin/Moderator.
- Storage metrics and thresholds are visible to Ops, but without exposing file contents.

---
<br>
<br>

### 9.7 ACH — Achievement Tracking

This module covers **viewing official achievements**, **integrating RetroAchievements / Steam where possible**, and **creating & using fan-made achievement sets** inside Dream Project. It also includes **completion tracking**, **evidence submission**, and **discussion/rating** around sets and individual achievements.

Objectives:

- show official + retro achievements clearly, without pretending we control the underlying platforms;
- let curators create, version, and maintain **fan sets** that are credible and discoverable;
- make completion tracking realistic: **auto-sync where possible**, manual + evidence-based where not;
- keep evidence flows safe (allowlisted hosts, moderation-ready) and locality-sensitive (UA/EN).

---

#### 9.7.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-ACH-01** | Official & Retro Integrations | Regression, NFR-heavy, UAT | Validate reading official and RetroAchievements data into Dream Project. | Steam/RA sync (read-only), staleness, partial outages, achievement lists & state mapping. |
| **TS-ACH-02** | Fan Set Authoring & Versioning | Regression, UAT | Ensure curators can create, update, and publish fan-made sets cleanly. | Templates, versioning, metadata, roles/permissions, discoverability. |
| **TS-ACH-03** | Completion Tracking & Evidence | Regression, UAT, NFR-heavy | Confirm completion tracking works across official, retro-integrated, and native fan sets. | Integration-based completion, manual marking, evidence flows (YouTube/Imgur), AI/help for triage. |
| **TS-ACH-04** | Discussion, Rating & Governance | Regression, NFR-heavy | Cover comments, ratings, abuse reports and moderation hooks on sets/achievements. | Threaded comments, ratings, report flows, allowlist/policy behaviour. |

---

#### 9.7.2 TS-ACH-01 Official & Retro Integrations

**Purpose:**  
Ensure Dream Project **accurately reflects external achievement status** where we have integrations (Steam, RetroAchievements) without promising impossible features or violating provider rules.

**Cycle Tags:** `Regression`, `NFR-heavy`, `UAT`

**Traceability (examples):**
- BRD: `BR-ACH-01 Official achievements`, `BR-ACH-02 Retro integrations`
- SRS: `SR-INT-10`–`SR-INT-18`, `SR-ACH-01`–`SR-ACH-06`
- Use Cases: `UC-INT-02 Link Steam/RA`, `UC-ACH-01 View official achievements`
- Risks: `RISK-INT-02` (bad mappings), `RISK-UX-05` (confusing progress vs platform)

**Typical included test cases (examples):**
- `TC-ACH-001` – Link Steam profile with public achievements → Dream Project fetches and displays achievements list for a known game, with states (locked/unlocked, timestamps) from fixtures.
- `TC-ACH-002` – Link RetroAchievements profile → Dream Project shows RA-based achievements and progress for a retro title seeded in fixtures.
- `TC-ACH-003` – When provider returns partial data (e.g., some fields missing), Dream Project still renders a stable list with “unknown” for non-critical fields rather than failing.
- `TC-ACH-004` – Provider quota exceeded or 5xx → we show last-known-good data and a **“stale”** indicator instead of an error page; log INT event.
- `TC-ACH-005` – Unlinking Steam/RA stops further sync but does not delete already-visible history; UI clearly shows integration is no longer active.
- `TC-ACH-006` – For games where no integration exists, Dream Project **does not** pretend to auto-sync; UI only offers native/fan features (if any) with honest messaging.

**NFR hooks:**
- External calls are read-only and rate-limited; on failure, circuit breaker/staleness behaviour kicks in.
- Provider errors never leak raw tokens, headers, or sensitive payloads in logs.
- Achievement list rendering remains responsive; metadata updates done async.

---

#### 9.7.3 TS-ACH-02 Fan Set Authoring & Versioning

**Purpose:**  
Let **curators** create, update, and maintain **fan-made achievement sets** that are structured, discoverable, and safe, with clear versioning and ownership.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-ACH-03 Fan sets`, `BR-GOV-01 Curator roles`
- SRS: `SR-ACH-07`–`SR-ACH-18`, `SR-GOV-05`–`SR-GOV-09`
- Use Cases: `UC-ACH-02 Create fan set`, `UC-ACH-03 Edit fan set`

**Typical included test cases (examples):**
- `TC-ACH-020` – Curator creates a new fan set for a game: defines title, description, tags (e.g., “completionist route”), difficulty, and per-achievement fields (title, description, type, optional hints).
- `TC-ACH-021` – Curator drafts changes (e.g., adds new achievements) and saves them as a **new version** without immediately publishing; previous version remains live.
- `TC-ACH-022` – Curator publishes an updated version; users see the **latest** version as default, with access to version history for transparency.
- `TC-ACH-023` – Non-curator (Regular user) cannot modify fan sets, but can view all public metadata and version history.
- `TC-ACH-024` – Curator marks a set as “experimental” or “advanced”; discovery filters reflect these facets.
- `TC-ACH-025` – Admin can lock or unpublish a fan set (e.g., due to policy breach), leaving an appropriate notice for the curator in an internal or profile view.
- `TC-ACH-026` – UA localisation keys for fan set metadata render correctly for UI elements (system labels), while curator-authored text remains in the language they wrote it in (no auto-translation implied).

**NFR hooks:**
- Fan set edit flows do not break existing completion records (backwards compatibility).
- Versioning operations are atomic (no partial states visible to users).
- Logs track who made which changes and when (audit trail), without storing full textual contents redundantly.

---

#### 9.7.4 TS-ACH-03 Completion Tracking & Evidence

**Purpose:**  
Ensure that **achievement completion** can be tracked in a credible and user-respectful way across:

- official sets (via Steam where supported),
- retro sets (via RetroAchievements integration), and
- native Dream Project fan sets (using manual completion + optional evidence).

**Cycle Tags:** `Regression`, `UAT`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ACH-04 Completion tracking`, `BR-SAV-03` (cross-links)
- SRS: `SR-ACH-19`–`SR-ACH-32`, `SR-GOV-10`–`SR-GOV-14`
- Use Cases: `UC-ACH-04 View completion`, `UC-ACH-05 Mark fan achievement complete`

**Typical included test cases (examples):**
- `TC-ACH-040` – For a Steam-integrated game, unlocked achievements from Steam are reflected as “completed” in Dream Project with appropriate timestamps (fixture-based).
- `TC-ACH-041` – For a RetroAchievements-integrated game, RA completion state is imported and mapped to Dream Project views; progress (e.g., 30/50 cheevos) appears correctly.
- `TC-ACH-042` – For a **native fan set**, Regular user marks an achievement as complete:  
  - sets status from “not started” → “completed”, and  
  - optionally attaches evidence (save link or screenshot/video link from an allowlisted host).
- `TC-ACH-043` – Attempt to attach evidence from a **non-allowlisted** host (e.g., random file share) is rejected with clear guidance.
- `TC-ACH-044` – AI-assisted triage runs in **shadow mode** (optional): system proposes “likely valid” vs “uncertain”, but moderator decision is still primary; result is not auto-accepted.
- `TC-ACH-045` – Moderator reviews a sample of fan completion proofs; can mark them as valid/invalid, optionally adding a private note; decision updates user completion statistics.
- `TC-ACH-046` – Aggregate stats on an achievement set/page show:  
  - % of users who completed it,  
  - time ranges (e.g., median time to complete), and  
  - recent completers listing (nickname, avatar, timestamp),  
  without exposing sensitive identifiers.
- `TC-ACH-047` – Completion decks for UA users look and behave the same as EN, including any time-based stats (only localised labels differ).

**NFR hooks:**
- Evidence links are stored as URLs only; no heavy media hosted by Dream Project is required for tests.
- AI triage, when enabled, has constraints: no destructive action based on AI judgement alone; logs note AI vs moderator decisions.
- Completion stats queries are efficient (no visible lag when viewing an achievement or set for seeded test data sizes).

---

#### 9.7.5 TS-ACH-04 Discussion, Rating & Governance

**Purpose:**  
Cover **social and governance features** around achievements: comments, ratings, reporting, and moderation, especially for **fan sets** that might have contentious content or low quality.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ACH-05 Community features`, `BR-GOV-02 Reporting & moderation`
- SRS: `SR-ACH-33`–`SR-ACH-40`, `SR-GOV-15`–`SR-GOV-22`
- Use Cases: `UC-ACH-06 Comment & rate`, `UC-GOV-02 Report content`, `UC-GOV-03 Moderate fan sets`

**Typical included test cases (examples):**
- `TC-ACH-060` – Regular user leaves a comment on an individual achievement; thread structure and timestamps are shown.
- `TC-ACH-061` – Regular user rates a fan set (e.g., 1–5 stars or similar scale); aggregated rating shows updated value and number of ratings.
- `TC-ACH-062` – User edits or deletes their own comment within allowed window; cannot edit someone else’s comment.
- `TC-ACH-063` – User reports a comment or fan set as problematic; item is flagged and appears in moderator queue (GOV suites cross-reference).
- `TC-ACH-064` – Moderator actions on reported content (hide, warn, remove) are reflected in the UI without leaking private details to the reporter.
- `TC-ACH-065` – Fan sets with consistently low ratings and/or large numbers of valid reports may be auto-deprioritised in discovery (per rules), with the behaviour covered here.
- `TC-ACH-066` – UA and EN users see discussions and ratings consistently; any UA/RU policy notes (if present) are aligned with catalogue-level policy, not randomly injected here.

**NFR hooks:**
- Comment/rating APIs respect rate limits (no spam floods).
- No PII or sensitive tokens are logged with comments; only the necessary user ID references.
- Moderation decisions produce an audit trail accessible to Admin/Moderator, not to general users.

---
<br>
<br>

### 9.8 ALB — Sticker Albums

This module covers **album templates created by curators**, **screenshot upload & validation**, **spoiler-safe progression for new players**, and **completion stats/badges**. It’s where the “screenshots as stickers” idea really lives.

Objectives:

- let curators define **themed albums** (locations, characters, story beats, etc.),  
- give players a **guided, spoiler-sensitive way** to fill albums (especially “new player” albums),
- use **AI-assisted checks** to catch obviously wrong or inappropriate screenshots without blocking normal use, and
- surface completion progress, badges, and album stats without leaking anything sensitive.

---

#### 9.8.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-ALB-01** | Album Template Authoring & Types | Regression, UAT | Ensure curators can design album templates with slots, spoiler levels, and “new-player-safe” behaviour. | Template creation/editing, slot definitions, album types (new-player vs full-spoiler), UA/EN labels. |
| **TS-ALB-02** | Screenshot Upload, Validation & Spoilers | Regression, NFR-heavy, UAT | Validate screenshot upload, AI validation, spoiler blurring, and hints so users don’t miss key moments. | Drag & drop, AI checks, inappropriate content flags, hints/previews for next slot, spoiler blur/unblur. |
| **TS-ALB-03** | Progression, Completion & Badges | Regression, UAT | Confirm album progress, completion, and badges behave as specified and feed into profile stats. | Slot-by-slot progress, completion %/timestamps, profile badges, completion stats for each album. |
| **TS-ALB-04** | Ownership, Sharing & Moderation | Regression, NFR-heavy | Cover who can see/edit/delete screenshots, reporting flows, and moderator actions on problematic content. | Ownership rules, visibility, reporting, moderator actions, linkage with policy (UA/RU) when relevant to the game. |

---

#### 9.8.2 TS-ALB-01 Album Template Authoring & Types

**Purpose:**  
Allow **curators** to design album templates for a game: define slots, spoiler levels, hints, and album type (e.g., “for new players” vs “full story retrospective”), while enforcing role and localisation rules.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-ALB-01 Album concept`, `BR-ALB-02 Curator tools`
- SRS: `SR-ALB-01`–`SR-ALB-10`
- Use Cases: `UC-ALB-01 Create album template`, `UC-ALB-02 Edit album template`
- Risks: `RISK-ALB-01` (confusing templates), `RISK-ALB-02` (spoilers mishandled)

**Typical included test cases (examples):**
- `TC-ALB-001` – Curator creates a new album template for a game: defines title, description, album type (“new-player-friendly” vs “full story”), and a set of slots.
- `TC-ALB-002` – Each slot can be given: title, short description, **spoiler level** (low/medium/high), and a **hint** that describes when to take the screenshot without fully spoiling it.
- `TC-ALB-003` – For “new-player-friendly” albums, slot descriptions/hints are shown in a **spoiler-safe mode**: e.g., hints are vague but still understandable (“after the first boss fight in the forest”) rather than explicit plot dumps.
- `TC-ALB-004` – For “full story” / retrospective albums, fuller descriptions are allowed; warning label indicates potential spoilers.
- `TC-ALB-005` – Curator edits an existing template and publishes a new version; in-progress user collections keep their current state, while future slot changes are applied cleanly.
- `TC-ALB-006` – Regular users cannot edit templates but can browse them and see which album types exist for a game (new-player vs full-spoiler).
- `TC-ALB-007` – UA/EN UI labels for album and slot metadata behave correctly; curator-entered content is left as-is without auto-translation.

**NFR hooks:**
- Template edits/versioning are atomic (no half-applied changes visible to users).
- Album templates load quickly on album selection (p95 detail load ≤ 500 ms).
- Audit trail for template changes is preserved for Admin/Moderator without duplicating all textual content.

---

#### 9.8.3 TS-ALB-02 Screenshot Upload, Validation & Spoilers

**Purpose:**  
Validate that players can **fill album slots with screenshots**, that **AI-assisted validation** helps catch clearly wrong content, and that **spoiler handling + hints** help users avoid missing key moments (especially in “new-player” albums).

**Cycle Tags:** `Regression`, `NFR-heavy`, `UAT`

**Traceability (examples):**
- BRD: `BR-ALB-03 Screenshot flows`, `BR-ALB-04 Spoiler handling`
- SRS: `SR-ALB-11`–`SR-ALB-24`, `SR-GOV-11`–`SR-GOV-14`
- Use Cases: `UC-ALB-03 Fill album slots`, `UC-ALB-04 View hints & spoilers`

**Typical included test cases (examples):**
- `TC-ALB-020` – Regular user opens an album and sees:  
  - a visual grid/list of slots,  
  - which ones are empty/filled, and  
  - for **new-player** albums, a short, spoiler-safe hint for the **next recommended slot**.
- `TC-ALB-021` – User uploads a screenshot via drag & drop or file picker to a specific slot; upload succeeds, thumbnail appears, and the slot is marked as filled.
- `TC-ALB-022` – AI-validation runs in **shadow mode or assistive mode**:  
  - obviously wrong game/image is flagged for review or gently highlighted,  
  - inappropriate/NSFW content is flagged and hidden pending moderator review.
- `TC-ALB-023` – If AI believes the screenshot might not match the slot, user sees a warning (“This might not fit this sticker; are you sure?”) but can still proceed unless policy rules require a hard block (e.g., NSFW).
- `TC-ALB-024` – For **new-player-friendly** albums, next-slot hints are visible *before* the moment where the screenshot should be taken, without revealing full story detail; the hint can be expanded to a slightly more explicit description if the user opts in.
- `TC-ALB-025` – High-spoiler slots are initially blurred for users who haven’t reached them yet; user can explicitly choose “Show details” to reveal description and example screenshot if curator provided one.
- `TC-ALB-026` – User attempts to upload an image that exceeds size/format limits; receives a clear error without breaking other album content.
- `TC-ALB-027` – User replaces an existing screenshot in a slot; the old one is no longer visible in normal UI but remains in audit/moderation history as needed.
- `TC-ALB-028` – UA/EN users see spoiler/hint controls and AI-warning copy correctly localised.

**NFR hooks:**
- Screenshot upload/thumbnail generation is responsive (user feedback within ~1 second that upload started).
- AI validation decisions are logged with minimal metadata (no full image content stored in logs; image itself lives in the media store).
- AI cannot auto-publish or auto-delete without moderator review; it only recommends.

---

#### 9.8.4 TS-ALB-03 Progression, Completion & Badges

**Purpose:**  
Ensure that album progress, completion, and **profile badges** work as expected, with stats that match our business rules: completion percentage, who completed which album, and high-level timing data.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-ALB-05 Completion & badges`, `BR-PRO-01 Profile stats`
- SRS: `SR-ALB-25`–`SR-ALB-32`, `SR-PRO-08`–`SR-PRO-13`
- Use Cases: `UC-ALB-05 View album progress`, `UC-PRO-03 View profile badges`

**Typical included test cases (examples):**
- `TC-ALB-040` – As a user fills slots, progress indicator updates (e.g., “3/10 stickers collected – 30%”); visual progress matches slot data.
- `TC-ALB-041` – Completing all required slots for an album triggers:  
  - album marked as **completed**, and  
  - a **game-specific badge** appears on the user’s profile.
- `TC-ALB-042` – Completing an album again (e.g., after template updates or optional slots added) does not issue duplicate badges but may update completion timestamps or progress counters.
- `TC-ALB-043` – Album detail page shows completion statistics:  
  - % of users who completed this album, and  
  - a recent-completion list with nickname, avatar, and completion timestamp.
- `TC-ALB-044` – Time-to-complete metrics (e.g., how long between first sticker and completion) are calculated and displayed in aggregate; individual user timelines are not overexposed.
- `TC-ALB-045` – UA/EN users see the same progression mechanics; only labels change (translations).
- `TC-ALB-046` – If an album template changes (e.g., new optional slots), existing completion status is handled according to rules (e.g., remains “complete” if new slots are optional, or marked “updated” if they become required).

**NFR hooks:**
- Progress and completion stats queries are efficient (no timeouts on seeded data).
- Profile views remain fast even when a user has many completed albums.
- Stats anonymise or limit exposed data so no sensitive info is leaked beyond public profile basics.

---

#### 9.8.5 TS-ALB-04 Ownership, Sharing & Moderation

**Purpose:**  
Cover **who controls screenshots**, how albums and individual stickers are shared, and how **reporting & moderation** works when content is problematic (e.g., NSFW, harassment, policy issues).

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ALB-06 Ownership`, `BR-GOV-03 Content moderation`
- SRS: `SR-ALB-33`–`SR-ALB-40`, `SR-GOV-23`–`SR-GOV-30`
- Use Cases: `UC-ALB-06 Manage own album stickers`, `UC-GOV-04 Report sticker`, `UC-GOV-05 Moderate visual content`

**Typical included test cases (examples):**
- `TC-ALB-060` – Only the **uploader** (and Admin/Moderator) can delete or replace a screenshot; other users can view it if album visibility allows, but not modify it.
- `TC-ALB-061` – Regular user reports a screenshot as inappropriate or off-topic; it appears in the moderation queue with the right classification.
- `TC-ALB-062` – Moderator hides or removes a problematic screenshot; album slot shows a placeholder or “removed” status for others, while audit trail is kept internally.
- `TC-ALB-063` – If a game is subject to UA/RU policy flags, any album surfaces that display the game’s name/cover show the **same policy banner** as catalogue (but do not duplicate banners on purely internal moderation screens).
- `TC-ALB-064` – Album visibility rules are respected: private albums visible only to owner, shared/community albums visible to others according to rules; badge and aggregate stats only count public or opt-in completions if that’s the policy.
- `TC-ALB-065` – UA and EN users have identical access to reporting and moderation-related UI (terms translated, behaviour identical).
- `TC-ALB-066` – Deletion or hiding of a screenshot does **not** break profile stats (completion counts remain consistent or are recomputed according to clear rules).

**NFR hooks:**
- Reporting actions and moderation decisions are fully auditable by Admin/Moderator.
- No screenshot content or moderation details are exposed in logs beyond IDs and necessary metadata.
- Major moderation actions (e.g., mass removal) are safe to perform and do not leave dangling references in albums, profiles, or completion stats.

---
<br>
<br>

### 9.9 PRO — Profiles & Progress

This module covers **user-facing profiles** and **aggregated progress views**: who a user is on Dream Project (nickname, avatar, role), plus **what they’ve done** (completed achievement sets, sticker albums, saves used/shared, curated content).

Objectives:

- give users a **clean, game-focused identity** (not real-world PII),
- present **progress and completion stats** that use data from SAV, ACH, ALB without leaking anything sensitive,
- reflect **roles** (Regular / Curator / Moderator / Admin) and capabilities clearly, and
- respect **localisation (EN/UA)** and **privacy/security constraints** for external links (Steam / RetroAchievements).

---

#### 9.9.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-PRO-01** | Core Profile View & Edit | Smoke, Regression, UAT | Validate profile basics: nickname, avatar, public/visible fields, locale toggles. | View own profile vs others’, edit nickname/avatar, no sensitive fields; EN/UA behaviour. |
| **TS-PRO-02** | Progress & Aggregated Stats | Regression, UAT, NFR-heavy | Ensure progress metrics (achievements, albums, saves) are aggregated and displayed correctly. | Counts, completion percentages, time-to-complete, recent activity feed; cross-check with module data. |
| **TS-PRO-03** | Roles, Capabilities & Badges | Regression, UAT | Confirm that roles and earned badges are shown correctly and that capabilities match the role. | Role labels, curator/admin markers, profile badges for albums/sets, no ability leaks. |
| **TS-PRO-04** | Localisation, Privacy & External Profiles | Regression, NFR-heavy | Cover EN/UA localisation, what external profile info is visible, and privacy constraints. | EN/UA UI, UA/RU policy messaging where relevant, limited RA/Steam exposure, no dangerous details leaked. |

---

#### 9.9.2 TS-PRO-01 Core Profile View & Edit

**Purpose:**  
Allow users to **view and customise their public Dream Project identity** (nickname, avatar, short “about” text if we support it) and see which parts are public vs private.

**Cycle Tags:** `Smoke`, `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-PRO-01 Profile basics`
- SRS: `SR-PRO-01`–`SR-PRO-06`
- Use Cases: `UC-PRO-01 View own profile`, `UC-PRO-02 View another user’s profile`, `UC-PRO-04 Edit profile`
- Risks: `RISK-PRIV-01` (oversharing), `RISK-UX-06` (confusing profile layout)

**Typical included test cases (examples):**
- `TC-PRO-001` – Own profile (EN): user sees nickname, avatar, role label, joined date, basic stats; edit controls are available.
- `TC-PRO-002` – Own profile (UA): same structure, all system labels translated; nickname/avatar untouched.
- `TC-PRO-003` – Other user’s profile: viewer sees **only** public information (nickname, avatar, public stats); no email, no internal IDs, no sensitive metadata.
- `TC-PRO-004` – Edit nickname & avatar: upload passes format/size checks; avatar updates everywhere after refresh; nickname changes are applied and logged.
- `TC-PRO-005` – Attempt to set invalid nickname (too long, forbidden characters) → validation messages appear; no partial updates.
- `TC-PRO-006` – If simple “about” text/short bio is supported: length & content validation (no HTML injection), visible on profile but not exposed via logs.
- `TC-PRO-007` – Profile URL / handle behaves consistently and does not reveal internal numeric IDs directly (or only exposes a safe, stable identifier).

**NFR hooks:**
- Profile pages load quickly (p95 ≤ 400–500 ms on seeded data).
- No tokens, emails, or raw IDs are written to logs when viewing or editing profiles.
- Avatar upload uses the same safety constraints as other media (size/type), and storage is centralised.

---

#### 9.9.3 TS-PRO-02 Progress & Aggregated Stats

**Purpose:**  
Display **aggregated progress** for a user by consuming data from SAV, ACH, and ALB: number of completed achievement sets, completed albums, saves shared/used, and high-level time-to-complete figures.

**Cycle Tags:** `Regression`, `UAT`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-PRO-02 Progress overview`, `BR-ACH-04`, `BR-ALB-05`, `BR-SAV-03`
- SRS: `SR-PRO-07`–`SR-PRO-15`
- Use Cases: `UC-PRO-03 View progress & stats`, plus cross-links to `UC-ACH-04`, `UC-ALB-05`, `UC-SAV-05`
- Risks: `RISK-DATA-01` (inconsistent stats), `RISK-UX-07` (overwhelming or misleading numbers)

**Typical included test cases (examples):**
- `TC-PRO-020` – Profile shows counts of:  
  - completed official/native achievement sets,  
  - completed sticker albums, and  
  - saves uploaded/shared and “starred” by others (if we track this).
- `TC-PRO-021` – When user completes a new sticker album (from ALB suites), profile stats increment appropriately after refresh.
- `TC-PRO-022` – Completion of a fan achievement set (with valid evidence) is reflected as “completed sets” in the profile.
- `TC-PRO-023` – Removal or invalidation of a completion (e.g., moderator revokes a suspicious fan completion) updates stats accordingly.
- `TC-PRO-024` – Time-based stats (e.g., median time to complete an album or set from first action) displayed in aggregate form, not with precise per-user timelines that could be sensitive.
- `TC-PRO-025` – “Recent activity” list (if implemented) shows high-level actions (e.g., “Completed Album X”, “Completed Set Y”), but does not expose which exact save file or evidence link was used.
- `TC-PRO-026` – UA and EN users see the same stats; only label text changes.
- `TC-PRO-027` – Stats remain consistent when viewing the same profile via different entry points (e.g., from comments/leaderboards vs direct link).

**NFR hooks:**
- Aggregation queries are efficient and cacheable; profile stats do not cause timeouts when a user has many completions.
- No raw evidence URLs or internal identifiers for saves/achievements are surfaced in profile stats—only counts and high-level descriptors.
- When underlying data is temporarily unavailable (e.g., background job delay), profile displays a safe fallback (e.g., “stats updating, please check back”) instead of incorrect zeros.

---

#### 9.9.4 TS-PRO-03 Roles, Capabilities & Badges

**Purpose:**  
Reflect a user’s **role** and **earned badges** in their profile, ensuring that:

- capabilities implied by role (Regular, Curator, Moderator, Admin) are not leaked or misrepresented,
- badges (e.g., album completions, curator recognition) appear correctly and are not forgeable, and
- any role changes over time are handled safely.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-GOV-01 Roles`, `BR-ALB-05`, `BR-ACH-03`
- SRS: `SR-PRO-16`–`SR-PRO-22`, `SR-GOV-01`–`SR-GOV-06`
- Use Cases: `UC-PRO-05 View roles & badges`, `UC-GOV-01 Promote user to curator`

**Typical included test cases (examples):**
- `TC-PRO-040` – Profile displays role label (e.g., “Regular player”, “Curator”, “Moderator”) clearly and consistently across UI.
- `TC-PRO-041` – Curator’s profile shows additional context (e.g., number of fan sets or album templates created), without exposing internal moderation tools.
- `TC-PRO-042` – Moderator/Admin role is indicated in a controlled way (e.g., subtle badge or text), but does not reveal back-office links or controls to regular users.
- `TC-PRO-043` – Completing an album triggers a game-specific badge; the badge appears in the profile “badges” area and links back to album details (if available).
- `TC-PRO-044` – Completing certain milestones (e.g., several fan sets, or being an active curator) may trigger curator-focused badges; badges are awarded only once and not duplicate.
- `TC-PRO-045` – Role change (Regular → Curator, Curator → Regular, etc.) is reflected in profile; capabilities in other modules (ACH/ALB/GOV) update accordingly.
- `TC-PRO-046` – Regular user cannot spoof badges or roles via client-side tricks (e.g., no trust in local UI for role/badge data).
- `TC-PRO-047` – UA/EN views show the same badge sets; names/descriptions translated where they are system-defined.

**NFR hooks:**
- Role/badge resolution happens server-side; UI doesn’t infer capabilities from front-end hints.
- Badge counts and icons are cached and do not slow profile loads.
- Audit trail exists for role changes (who changed what and when), visible to Admin.

---

#### 9.9.5 TS-PRO-04 Localisation, Privacy & External Profiles

**Purpose:**  
Validate that profile behaviour respects:

- **localisation** (EN/UA),
- **privacy decisions** around external profiles (Steam / RetroAchievements), and
- minimal, safe exposure of external links for users who opt in.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-PRO-03 Privacy & external links`, `BR-INT-03`
- SRS: `SR-PRO-23`–`SR-PRO-30`, `SR-INT-19`–`SR-INT-22`
- Use Cases: `UC-PRO-06 Manage external profiles`, `UC-INT-02 Link Steam/RA`

**Typical included test cases (examples):**
- `TC-PRO-060` – EN user with linked Steam/RA profiles: profile shows minimal, user-approved info (e.g., “Linked to Steam” with optional display name), not raw account IDs or email.
- `TC-PRO-061` – UA user with linked profiles: same behaviour and labels, translated; no extra data just because of locale.
- `TC-PRO-062` – User chooses to **hide** or unlink external profiles; profile no longer displays them, and future syncs stop, but previously fetched non-sensitive achievement info may remain as part of progress (per design).
- `TC-PRO-063` – Public profile never reveals precise external handles or links unless user explicitly opts in (and even then, the UI indicates that this is user-controlled).
- `TC-PRO-064` – Logs and internal audit trails store only what is needed for syncing and debugging (e.g., hashed or opaque identifiers), not passwords or tokens.
- `TC-PRO-065` – UA/RU policy: if the game list/activities displayed in the profile mention games with RU-related policy flags, the same **“at your own risk”** banners from catalogue are used where appropriate, but profile itself does not become a policy billboard.
- `TC-PRO-066` – Switching locale from EN to UA on profile view adjusts only system labels and policy text; underlying stats and activity remain identical.

**NFR hooks:**
- No external tokens or secrets ever appear in the UI or standard logs.
- Changing privacy or link/unlink options is fast and atomic; there are no intermediate states where some parts of the profile show old data and others new.
- Localisation consistently uses the same keyset as elsewhere (e.g., status texts, labels for badges and stats).

---
<br>
<br>

### 9.10 GOV — Moderation & Governance

This module covers **reporting, review, and policy enforcement** across Dream Project:

- how content is **reported** (saves, screenshots, fan sets, comments, evidence links),
- how Moderators/Admins **review and act** (hide, warn, remove, ban, override),
- how **policy rules** (evidence allowlist, UA/RU game flags, NSFW rules) are applied, and
- how we keep a **clear audit trail** without leaking sensitive details.

GOV is cross-cutting: many suites here are referenced from ACH, ALB, SAV, and PRO.

---

#### 9.10.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-GOV-01** | Reporting & Intake | Regression, UAT | Ensure users can report problematic content, and reports land reliably in moderation queues. | Reporting from saves, albums, achievements, comments; categories, metadata, UX. |
| **TS-GOV-02** | Moderation Queue & Actions | Regression, NFR-heavy, UAT | Validate moderator workflows: reviewing, acting on items, notifying owners, and keeping audit logs. | Queue filters, decisions (hide/remove/warn), partial vs global actions, audit trail. |
| **TS-GOV-03** | Policy Enforcement (Evidence, UA/RU, NSFW) | Regression, NFR-heavy | Confirm platform policies are enforced consistently across modules. | Evidence allowlist, NSFW detection hooks, UA/RU banners for games, content removal rules. |
| **TS-GOV-04** | Roles, Permissions & Auditability | Regression, NFR-heavy | Cover role-based access to moderation tools and long-term governance records. | Moderator/Admin capabilities, read-only views, change histories, export/reporting basics. |

---

#### 9.10.2 TS-GOV-01 Reporting & Intake

**Purpose:**  
Guarantee that users can **flag problematic content** from where they see it (saves, stickers, achievements, comments, profiles), that reports are categorised, and that nothing silently disappears.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-GOV-02 Reporting`, `BR-ALB-06`, `BR-ACH-05`, `BR-SAV-05`
- SRS: `SR-GOV-01`–`SR-GOV-08`
- Use Cases: `UC-GOV-01 Report content`, `UC-GOV-02 View moderation queue`
- Risks: `RISK-GOV-01` (reports lost), `RISK-POL-01` (policy breaches unchecked)

**Typical included test cases (examples):**
- `TC-GOV-001` – Regular user reports a **save** (e.g., suspected malware, misleading description) from the save detail view; report dialog offers category, optional comment, and confirms submission.
- `TC-GOV-002` – Regular user reports a **screenshot/sticker** from an album; same report flow with “off-topic”, “spoiler abuse”, “NSFW” categories.
- `TC-GOV-003` – Regular user reports a **fan achievement set** or a specific achievement for low quality, harassment, or policy issues.
- `TC-GOV-004` – Regular user reports a **comment**; success message is generic (“Thanks, we’ll review this”) and does not promise specific outcomes.
- `TC-GOV-005` – All reports appear in the moderation queue with correct source module, content type, reporter (internal ID), category, and timestamp.
- `TC-GOV-006` – Duplicate reports for the same item are grouped or displayed in a way that avoids overwhelming moderators while still indicating volume.
- `TC-GOV-007` – UA and EN users see the same categories with translated labels; behaviour identical.

**NFR hooks:**
- Reporting actions are quick (p95 ≤ 400 ms) and never block the main user flow.
- No reporter-identifying details are shown to the content owner; only moderators can see who reported.
- Logs record report IDs and target IDs, but not full free-text comments unless necessary, and never in unredacted analytics streams.

---

#### 9.10.3 TS-GOV-02 Moderation Queue & Actions

**Purpose:**  
Validate the **day-to-day workflow** of Moderators/Admins: triaging items, applying decisions, and seeing the impact on user-facing content while maintaining a reliable audit trail.

**Cycle Tags:** `Regression`, `NFR-heavy`, `UAT`

**Traceability (examples):**
- BRD: `BR-GOV-03 Moderation actions`, `BR-ALB-06`, `BR-ACH-05`
- SRS: `SR-GOV-09`–`SR-GOV-18`
- Use Cases: `UC-GOV-03 Review reports`, `UC-GOV-04 Moderate item`

**Typical included test cases (examples):**
- `TC-GOV-020` – Moderator opens the moderation queue: can filter by content type (save, sticker, achievement, comment), severity, and status (new/in progress/resolved).
- `TC-GOV-021` – Moderator reviews a reported **save**: sees enough context (game, variant, uploader, short description, report reasons) to decide whether to hide/remove.
- `TC-GOV-022` – Action “Hide from public” removes an item from public listings but keeps it visible to the owner with a neutral message (“This item is hidden due to policy review.”).
- `TC-GOV-023` – Action “Remove” hides the item from everyone except Admins; owner sees an appropriate notification in activity/history (without exposing reporter details).
- `TC-GOV-024` – Moderator issues a **warning** to a user (for recurring issues); warning appears in user’s notification center or activity log.
- `TC-GOV-025` – Multiple related items (e.g., a whole fan set and its comments) can be handled consistently in a batch with clear summary of actions.
- `TC-GOV-026` – Decision history for a given item (who did what, when, and why) can be viewed by Moderators/Admins for future context.
- `TC-GOV-027` – UA vs EN moderator views use the same workflows; only system labels differ.

**NFR hooks:**
- Queue interface remains responsive even with many open reports (pagination, sensible defaults).
- Moderation actions are transactional — no half-hidden content (either fully applied or rolled back).
- All actions are logged with moderator ID and a minimal but useful rationale code; free-text notes are optional and kept private.

---

#### 9.10.4 TS-GOV-03 Policy Enforcement (Evidence, UA/RU, NSFW)

**Purpose:**  
Confirm that platform **policy rules** are consistently enforced, particularly:

- **evidence host allowlist** for achievement completion proofs,
- **UA/RU game policy flags** (e.g., “at your own risk” warnings for RU-linked titles),
- basic **NSFW/abuse handling** for screenshots and comments.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-POL-01 UA/RU policy`, `BR-POL-02 Evidence policy`
- SRS: `SR-GOV-19`–`SR-GOV-28`, `SR-INT-20`–`SR-INT-22`
- Use Cases: `UC-GOV-05 Enforce evidence policy`, `UC-GOV-06 Manage UA/RU flags`

**Typical included test cases (examples):**
- `TC-GOV-040` – User attempts to attach completion evidence from an **allowlisted** host (YouTube, Imgur) → accepted, queued for optional moderation.
- `TC-GOV-041` – User attempts to attach evidence from a **non-allowlisted** host (e.g., random file-share); UI rejects with clear guidance on allowed hosts.
- `TC-GOV-042` – Moderator updates the evidence allowlist (add/remove domain) via governance tools; changes propagate without disrupting existing accepted evidence.
- `TC-GOV-043` – UA user views a game flagged with RU publisher/developer: the same “at your own risk” banner appears in catalogue, album, and achievement context screens; it does **not** appear where it shouldn’t (e.g., generic profile list of games without context).
- `TC-GOV-044` – Flags for “has Ukrainian localisation” and “Ukrainian-developed” are applied from data sources and/or manual overrides; UA-specific UI surfaces reflect them consistently.
- `TC-GOV-045` – Screenshot or comment marked as NSFW is auto-hidden from general users and queued for moderator review; owner sees a general notice that content is under review.
- `TC-GOV-046` – Policy changes (e.g., tightening NSFW rules, changing what “RU-affiliated” means) can be applied via config, and existing items adapt to new rules without manual retagging where possible.

**NFR hooks:**
- Policy rules are **centralised** (config-driven) and referenced consistently across modules; no hard-coded one-off behaviours.
- Evidence allowlist checks happen server-side, not only in front-end code.
- UA/RU flags and NSFW status never leak in raw form to public APIs beyond what is explicitly meant to be visible.

---

#### 9.10.5 TS-GOV-04 Roles, Permissions & Auditability

**Purpose:**  
Ensure that moderation and governance tools are only available to the right people, that role changes are safe, and that an **audit trail** exists for long-term accountability.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-GOV-01 Roles & responsibilities`, `BR-PRO-03 Privacy`
- SRS: `SR-GOV-29`–`SR-GOV-36`, `SR-PRO-20`–`SR-PRO-30`
- Use Cases: `UC-GOV-07 Manage moderator roles`, `UC-GOV-08 View audit logs`

**Typical included test cases (examples):**
- `TC-GOV-060` – Regular user **cannot** access moderation queues or governance tools (direct URL attempts yield “not authorised”).
- `TC-GOV-061` – Moderator can access queues and act on content but cannot change system-wide policies or roles.
- `TC-GOV-062` – Admin can assign or revoke Moderator/Curator roles; changes appear in the affected user’s profile and are reflected in module capabilities (ACH/ALB/GOV/PRO).
- `TC-GOV-063` – Every moderation action (hide/remove/warn) is recorded in an audit log with: actor, action type, target ID, timestamp, and minimal reason codes.
- `TC-GOV-064` – Role-change events (Regular → Curator, Curator → Moderator, etc.) are recorded in the audit log with who initiated the change.
- `TC-GOV-065` – Read-only audit views allow Admins (and possibly lead Moderators) to search history by game, user, content type, or time range, without enabling them to edit past entries.
- `TC-GOV-066` – Export of audit data (if supported) respects privacy constraints (no tokens or raw content; focus on metadata and IDs only).

**NFR hooks:**
- Permission checks are enforced server-side; no reliance on front-end hiding of links.
- Audit logs are write-once/append-only at the application level; no casual editing or deletion by moderators.
- Audit queries are performant for typical history volumes and can be pruned/archived according to retention policy without losing current accountability.

---
<br>
<br>

### 9.11 DSC — Discovery & Search

This module covers **cross-cutting discovery** across Dream Project:

- the **global search bar** and entry flows (homepage, header),
- unified results for **games, saves, achievement sets, albums, and (optionally) users**,
- **filters/facets** (platform, variant, localisation flags, type of content),
- lightweight **recommendations** when the user doesn’t know what to pick yet.

Objectives:

- help users get from “I have a vague idea” to the **right game or content** quickly,
- make cross-entity results (games vs saves vs albums vs sets) **clear, not chaotic**,
- respect **UA/RU policy flags** and localisation filters in search/discovery,
- keep performance acceptable even as catalogues and content grow.

---

#### 9.11.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-DSC-01** | Global Search & Result Layout | Smoke, Regression, UAT | Validate the global search bar, result types, and navigation from results. | Mixed results (games, saves, sets, albums), type grouping, EN/UA behaviour. |
| **TS-DSC-02** | Filters, Facets & Sorting | Regression, UAT, NFR-heavy | Ensure filters/facets narrow results predictably and reflect catalogue/policy rules. | Platform, game type, localisation flags, UA/RU flags, relevance/date sorting. |
| **TS-DSC-03** | Recommendations & Entry Flows | Regression, UAT | Test “I don’t know what to pick” flows and basic recommendation cards. | First-time vs returning user suggestions, “Not for me” button, simple history-based logic. |
| **TS-DSC-04** | Empty States, Errors & Performance | Regression, NFR-heavy | Cover “no result” and error states, plus basic performance hooks. | No-results UX, partial outages, timeouts, performance assertions on global search. |

---

#### 9.11.2 TS-DSC-01 Global Search & Result Layout

**Purpose:**  
Make sure the **global search bar** works as the main navigation entry point: users can search once and see clearly structured results across **games, saves, achievement sets, and albums**, without mixing everything into an unreadable feed.

**Cycle Tags:** `Smoke`, `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-DSC-01 Global discovery`, `BR-CAT-01 Search`, `BR-SAV-03`, `BR-ACH-03`, `BR-ALB-01`
- SRS: `SR-DSC-01`–`SR-DSC-10`
- Use Cases: `UC-DSC-01 Global search`, `UC-CAT-01 Browse catalogue`, `UC-SAV-04`, `UC-ACH-04`, `UC-ALB-05`
- Risks: `RISK-DISC-01` (users “can’t find anything”), `RISK-UX-08` (mixed results confusion)

**Typical included test cases (examples):**
- `TC-DSC-001` – Typing a popular game title in the global search (EN):  
  - results grouped by type (“Games”, “Saves”, “Achievement sets”, “Sticker albums”),  
  - the game itself appears at the top of “Games”.
- `TC-DSC-002` – Same search in UA locale: grouping and ordering identical; only labels translated.
- `TC-DSC-003` – Clicking a **game** result leads to the game detail page (CAT), not some intermediate dead-end.
- `TC-DSC-004` – Clicking a **save** result leads to the save detail view (SAV); variant and compatibility labels visible there.
- `TC-DSC-005` – Clicking an **achievement set** result opens the set details (ACH), with correct version and source (official/RA/fan).
- `TC-DSC-006` – Clicking a **sticker album** result opens album template or personal album view (ALB), depending on context.
- `TC-DSC-007` – If multiple result pages are needed, pagination or infinite scroll behaves consistently, and the user does not lose filter context when moving between pages.

**NFR hooks:**
- Global search round-trip p95 ≤ ~300 ms under standard test load for seeded data.
- No raw PII or tokens logged when executing search queries.
- Result layout remains readable and consistent on both desktop and smaller screens.

---

#### 9.11.3 TS-DSC-02 Filters, Facets & Sorting

**Purpose:**  
Ensure filters and facets actually **do what they say**: narrowing cross-entity results based on platform, game type (retro/modern), localisation flags (UA available or not), and UA/RU policy flags, with clear and predictable sorting.

**Cycle Tags:** `Regression`, `UAT`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-DSC-02 Filters & facets`, `BR-CAT-02 Filters`, `BR-POL-01 UA/RU policy`
- SRS: `SR-DSC-11`–`SR-DSC-20`, `SR-CAT-11`–`SR-CAT-19`
- Use Cases: `UC-DSC-02 Filter results`, `UC-CAT-03 View localisation info`

**Typical included test cases (examples):**
- `TC-DSC-020` – Apply **platform filter** (e.g., “Retro only”) in global search results:  
  - only retro games, retro saves, and retro-focused sets/albums appear;  
  - modern-only items are hidden.
- `TC-DSC-021` – Apply **Steam/PC filter**: only games and content tied to Steam/PC variants show up.
- `TC-DSC-022` – Filter by “Has Ukrainian localisation” → only games (and associated saves/albums/sets) linked to UA-localised titles appear.
- `TC-DSC-023` – Filter by “Ukrainian-developed” (where data exists) → results restricted accordingly; if data is missing, items are either excluded or clearly marked as “unknown”, per design.
- `TC-DSC-024` – UA user toggles “Hide RU-linked titles” (if such a toggle exists): games with RU-linked publishers and their associated content are hidden or clearly greyed-out with explanation, depending on design.
- `TC-DSC-025` – Sorting by **relevance** vs **recency** vs **popularity**:  
  - relevance uses text match + type weighting,  
  - recency uses last activity date,  
  - popularity uses usage metrics (e.g., saves downloaded, albums completed).
- `TC-DSC-026` – Combining multiple filters (e.g., platform + localisation) behaves predictably; no contradictory states (UI always shows which filters are active).
- `TC-DSC-027` – EN vs UA: filter options and labels are identical in meaning; base behaviour is the same.

**NFR hooks:**
- Filter and sort changes update results quickly (p95 ≤ ~300 ms) without full-page reload where possible.
- Filters are applied server-side; no client-side “fake” filtering that could diverge from truth.
- UA/RU and localisation flags are taken from the same central logic as catalogue, not duplicated.

---

#### 9.11.4 TS-DSC-03 Recommendations & Entry Flows

**Purpose:**  
Test flows for users who **don’t have a specific game in mind yet**. This includes landing-page recommendations and gentle “pick a game” helpers, using simple rules (history-based, popularity-based, or curated sets), plus a **“Not for me”** dismissal mechanism.

**Cycle Tags:** `Regression`, `UAT`

**Traceability (examples):**
- BRD: `BR-DSC-03 Recommendations`, `BR-PRO-02 Progress overview`
- SRS: `SR-DSC-21`–`SR-DSC-28`, `SR-PRO-07`–`SR-PRO-10`
- Use Cases: `UC-DSC-03 View recommendations`, `UC-DSC-04 Dismiss suggestion`

**Typical included test cases (examples):**
- `TC-DSC-040` – **First-time user** with no history opens the homepage: sees a recommendation block with a small set of suggested games (e.g., popular or currently featured), not an empty screen.
- `TC-DSC-041` – **Returning user** who previously used save management heavily sees recommendations biased towards:  
  - games they interacted with (saves/achievements/albums), or  
  - similar retro/modern titles based on simple rules/curation.
- `TC-DSC-042` – Clicking a recommendation card takes the user directly to the **intended entry point**: for example, a pre-selected game detail page with clear actions for saves/achievements/albums.
- `TC-DSC-043` – User clicks “Not suitable for me” / “Not now” on a suggested game:  
  - that specific recommendation is dismissed for this session (and ideally remembered),  
  - a new suggestion appears or the block shrinks gracefully.
- `TC-DSC-044` – UA vs EN: recommendation copy and hints are fully localised, but the **logic** behind recommendations does not change by locale.
- `TC-DSC-045` – Recommended games respect **UA/RU policy flags** and localisation filters: if a UA user chose to hide RU-linked titles, those titles never appear in recommendations.
- `TC-DSC-046` – Recommendations remain stable if external metadata/ratings providers are temporarily unavailable; simple fallback sets (e.g., curated lists) are used instead of blank panels.

**NFR hooks:**
- Recommendation generation remains lightweight for MVP (simple rules/snapshots, not heavy ML) and does not slow page load.
- Dismissal state is persisted in a small, privacy-conscious way (no over-collection of history).
- No sensitive external IDs or tokens appear in logs when computing recommendations.

---

#### 9.11.5 TS-DSC-04 Empty States, Errors & Performance

**Purpose:**  
Cover **edge states** of discovery: no results, partial outages, timeouts, and basic performance guarantees around global search and discovery pages.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-DSC-04 UX resilience`, `BR-INT-02`
- SRS: `SR-DSC-29`–`SR-DSC-36`, `SR-INT-03`–`SR-INT-06`
- Use Cases: `UC-DSC-01`–`UC-DSC-04`, `UC-INT-01 Refresh metadata`

**Typical included test cases (examples):**
- `TC-DSC-060` – Search query with no matches: user sees a friendly “no results” message, possible suggestions (e.g., check spelling, try fewer filters), and easy access to reset filters.
- `TC-DSC-061` – External metadata/search provider is down or slow: results use cached data and a small, non-blocking banner states that some enhancements may be temporarily unavailable.
- `TC-DSC-062` – Partial failure (e.g., games load, but saves section for a query times out): UI clearly shows which section failed and offers retry; rest of results remain usable.
- `TC-DSC-063` – Search throttling or rate limiting (for abuse protection) returns a clear message without exposing implementation details.
- `TC-DSC-064` – Switching between EN and UA on a search results page preserves the search query and filters; only labels and messages change.
- `TC-DSC-065` – When pagination/infinite scroll reaches the end of results, the UI makes that clear; no infinite loading spinner.
- `TC-DSC-066` – Stress/performance test on seeded data confirms that global search and discovery pages stay within agreed latency targets (p95 around 300–500 ms for typical queries, under agreed load).

**NFR hooks:**
- Graceful degradation is the default: **never** a hard 500-style error just because an external dependency failed.
- Logging for errors includes enough context (query, filter set, provider status) without capturing user-identifying information.
- Search and discovery endpoints are monitored (latency, error rate) so regressions are noticed early.

---
<br>
<br>

### 9.12 INT — External Integrations & Adapters

This module covers the **service layer** that talks to:

- **Steam** (for official achievement status; possibly minimal game metadata),
- **RetroAchievements (RA)** (for retro achievement sets and completion),
- **generic game metadata providers** (e.g., IGDB-like APIs),
- **UA-oriented metadata sources** (for “Ukrainian localisation” / “Ukrainian-developed” flags),
- and **evidence/media hosts** (e.g., YouTube, Imgur) via an allowlist.

Other modules (ACH, CAT, DSC, GOV, PRO) **consume** these adapters. INT tests focus on:

- connection & credential handling,
- mapping external data into stable internal structures,
- staleness, rate limiting & fallbacks,
- and **not leaking secrets** (tokens, IDs) into logs or UI.

---

#### 9.12.1 Suite Overview

| Suite ID | Name | Cycle Tags | Purpose | Key Coverage / Notes |
|---|---|---|---|---|
| **TS-INT-01** | Steam Integration (Achievements & Minimal Metadata) | Regression, NFR-heavy | Validate the adapter that reads Steam achievements and any minimal game data we use. | Auth/keys, fixtures, mapping to internal achievement model, staleness, provider errors. |
| **TS-INT-02** | RetroAchievements Integration (Retro Sets & Progress) | Regression, NFR-heavy, UAT | Ensure RA integration correctly pulls sets and progress for retro titles. | Profile linking, set/achievement mapping, progress, outages, privacy. |
| **TS-INT-03** | Game Metadata & UA-Specific Sources | Regression, NFR-heavy | Cover IGDB-like + UA sources for localisation/developer flags and core metadata. | Title/platform/genre mapping, UA localisation flags, staleness/fallbacks. |
| **TS-INT-04** | Evidence & Media Host Allowlist | Regression, NFR-heavy | Confirm URL allowlist for completion evidence and embedded media behaves correctly. | Host validation, config updates, logging, no secret leakage. |

> UI-level behaviour that uses these integrations (e.g., achievement lists, UA/RU banners) is primarily tested in ACH, CAT, DSC, GOV suites; INT focuses on the **adapter behaviour** and data contracts.

---

#### 9.12.2 TS-INT-01 Steam Integration (Achievements & Minimal Metadata)

**Purpose:**  
Validate the adapter that talks to **Steam** for:

- reading official achievement status for linked users,
- optionally pulling **minimal game metadata** we rely on (ID, title, platform flags),
- handling errors, throttling, and staleness without breaking downstream modules.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-ACH-01`, `BR-INT-01`
- SRS: `SR-INT-10`–`SR-INT-15`, `SR-ACH-01`–`SR-ACH-06`
- Use Cases: `UC-INT-02 Link Steam`, `UC-ACH-01 View official achievements`
- Risks: `RISK-INT-02` (bad mappings), `RISK-PRIV-02` (data leaks)

**Typical included test cases (examples):**
- `TC-INT-001` – Using fixtures, adapter pulls achievements for a Steam game and maps them into internal fields (ID, name, description, unlocked flag, unlock timestamp).
- `TC-INT-002` – Adapter gracefully handles missing non-critical fields (e.g., description) and fills sensible defaults (`null` or “N/A”) instead of failing.
- `TC-INT-003` – When Steam API returns HTTP 5xx or network timeout, adapter returns a structured “provider unavailable” result; callers can display **stale** cached data.
- `TC-INT-004` – Rate-limiting from Steam (e.g., HTTP 429 or similar) causes the adapter to back off and surface a controlled error; no tight retry loop.
- `TC-INT-005` – Any Steam keys/tokens used are kept in configuration/secret storage and never appear in logs, error messages, or front-end payloads.
- `TC-INT-006` – A linked Steam profile can be disabled/unlinked; adapter stops making calls for that mapping and returns “integration not active” to consumers.

**NFR hooks:**
- Adapter calls fit within latency budgets (configured timeout; no unbounded waits).
- Responses are normalised into internal DTOs; callers never manipulate raw Steam JSON.
- Logging redacts or omits any headers or payload fields that might contain secrets.

---

#### 9.12.3 TS-INT-02 RetroAchievements Integration (Retro Sets & Progress)

**Purpose:**  
Ensure that RetroAchievements integration:

- links Dream Project users to RA profiles,
- pulls retro achievement **sets and progress**, and
- maps them to internal data without pretending we issue those achievements.

**Cycle Tags:** `Regression`, `NFR-heavy`, `UAT`

**Traceability (examples):**
- BRD: `BR-ACH-02`, `BR-INT-02`
- SRS: `SR-INT-16`–`SR-INT-22`, `SR-ACH-02`–`SR-ACH-06`
- Use Cases: `UC-INT-03 Link RA profile`, `UC-ACH-01 View retro achievements`
- Risks: `RISK-INT-03` (bad link), `RISK-UX-05` (confusing RA vs native)

**Typical included test cases (examples):**
- `TC-INT-020` – User links RA profile (using whatever token/key RA supports); adapter stores an opaque mapping and can fetch sets + progress for a known retro title from fixtures.
- `TC-INT-021` – Adapter maps RA achievements (ID, title, description, type, awarded flag, timestamps) to internal structures; no RA secrets in the mapping.
- `TC-INT-022` – If RA is unreachable (timeout, 5xx), adapter returns “provider unavailable” while leaving existing data intact and marked as **stale** for callers.
- `TC-INT-023` – When an RA profile is unlinked, adapter removes or disables the mapping; subsequent calls return “integration not active” rather than errors.
- `TC-INT-024` – RA progress is correctly associated to the right game/variant in our catalogue; wrong-game scenarios (ID mismatch) are detected via fixtures and surfaced as internal errors only (not user-facing).
- `TC-INT-025` – Adapter respects RA’s rate limits and doesn’t spam endpoints; throttle/backoff behaviour observed in structured logs.

**NFR hooks:**
- RA credentials/tokens are never logged; only opaque mapping IDs are stored.
- Adapter exposes a minimal surface area (no generic RA passthrough API).
- Caching/TTL logic prevents constant polling for the same user/game combination.

---

#### 9.12.4 TS-INT-03 Game Metadata & UA-Specific Sources

**Purpose:**  
Cover adapters for:

- a **generic metadata provider** (IGDB-like),
- and one or more **UA-focused sources** (to flag Ukrainian localisation and Ukrainian-developed titles),

ensuring coherent, merged metadata for catalogue consumers.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-CAT-05 Metadata`, `BR-POL-01` (UA/RU context), `BR-INT-03`
- SRS: `SR-INT-03`–`SR-INT-09`, `SR-CAT-16`–`SR-CAT-19`
- Use Cases: `UC-INT-01 Refresh metadata`, `UC-CAT-04 View game details`

**Typical included test cases (examples):**
- `TC-INT-040` – Generic provider returns full record (title, year, genre, platforms); adapter maps fields into internal game model and stores them.
- `TC-INT-041` – UA-specific source indicates whether a game has **Ukrainian localisation**; adapter merges this into internal fields (`has_ua_localisation = true/false/unknown`).
- `TC-INT-042` – UA-specific source or manual configuration marks a game as **Ukrainian-developed**; adapter merges/overrides dev/publisher info while keeping original data in the internal model for admin reference.
- `TC-INT-043` – If a provider returns incomplete data or is missing a game, adapter marks metadata as partial and leaves prior data intact with a “stale” or “partial” flag for callers.
- `TC-INT-044` – When external provider is down or returns errors, adapter surfaces a structured error; catalogue consumers fall back to cached metadata without crashing.
- `TC-INT-045` – Manual admin overrides applied through internal tools (e.g., fix a wrongly attributed developer or UA flag) are respected by adapters and **not** overwritten blindly on next refresh (unless explicitly configured to do so).
- `TC-INT-046` – UA/RU-specific flags used for catalogue banners are derived from these internal fields; adapter does not implement policy logic itself, just exposes flags.

**NFR hooks:**
- Metadata refresh jobs obey per-provider rate limits and backoff rules.
- Sensitive fields (API keys, raw provider responses) are never exposed in user-facing endpoints.
- All merging logic is deterministic and well-logged (e.g., “provider X says UA-localised=true; internal flag set to true”).

---

#### 9.12.5 TS-INT-04 Evidence & Media Host Allowlist

**Purpose:**  
Validate the adapter/rules that manage **allowlisted hosts** for completion evidence (videos, screenshots on third-party sites) and lightweight media embedding.

**Cycle Tags:** `Regression`, `NFR-heavy`

**Traceability (examples):**
- BRD: `BR-POL-02 Evidence policy`
- SRS: `SR-GOV-19`–`SR-GOV-24`, `SR-INT-23`–`SR-INT-26`
- Use Cases: `UC-ACH-05 Mark achievement complete with evidence`, `UC-GOV-05 Enforce evidence policy`

**Typical included test cases (examples):**
- `TC-INT-060` – Evidence URL from **allowlisted** host (e.g., YouTube, Imgur) is accepted by adapter and returned as “allowed”.
- `TC-INT-061` – Evidence URL from **non-allowlisted** host is rejected as “disallowed”; caller can show a clear error message.
- `TC-INT-062` – Hostname variations (`www.youtube.com`, `youtu.be`) and standard query parameters are recognised correctly; path and query are not over-validated.
- `TC-INT-063` – Admin updates allowlist (e.g., add/remove host) via configuration; adapter immediately reflects changes for new validations.
- `TC-INT-064` – Evidence URLs are stored and returned as **URLs only** (no media downloaded); adapter never proxies or caches full media.
- `TC-INT-065` – Logs record only high-level info (host, status allowed/blocked, timestamp) and never full URLs in analytics streams if that’s considered sensitive.
- `TC-INT-066` – Malformed URLs are safely handled (no crashes, no open redirects); adapter returns “invalid URL” result for callers.

**NFR hooks:**
- Validation calls are lightweight and safe to execute frequently (e.g., on form submission).
- Allowlist is centrally configured and consistent across ACH/ALB/GOV; no per-module divergence.
- No secrets (API keys, tokens) are ever involved in evidence URL validation; it is purely structural/host-based.

---
<br>
<br>

## 10. Non-Functional Test Plan

This section describes how we will validate the **non-functional qualities** of Dream Project: performance, security, reliability, usability, localisation, and policy-related behaviour.  

It builds on the **NFR hooks** called out in each module suite (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT) and groups them into a small number of **targeted NFR activities** so we don’t miss critical behaviours or duplicate effort.

---
<br>

### 10.1 Scope & Objectives

**Scope**

The Non-Functional Test Plan covers:

- **Performance & load** of core flows (search, profile, saves, achievements, albums).
- **Security & privacy** basics from a QA perspective (no penetration testing, but obvious leaks/unsafe behaviours).
- **Reliability & recovery**, including how the system behaves under partial failures and timeouts.
- **Usability & accessibility** at a pragmatic level, especially for core flows.
- **Localisation & regional behaviour**, with emphasis on EN/UA and UA/RU policy banners.

**Objectives**

- Verify that **core user journeys remain fast enough** under expected MVP load.
- Detect obvious **security/privacy regressions** in functional flows (tokens/PII leakage, unsafe URLs).
- Confirm that **external outages** (Steam, RA, metadata providers) degrade gracefully, not catastrophically.
- Ensure **labels, error messages, and banners** are understandable and consistent in EN and UA.
- Provide a **clear NFR sign-off** that complements (not replaces) any future specialised performance or security audits.

---
<br>

### 10.2 Performance & Load Testing

**Targets**

- Global search & discovery (DSC).
- Game detail & catalogue pages (CAT).
- Save detail and upload flows (SAV).
- Achievement set and album views (ACH, ALB).
- Profile views & stats (PRO).

**Approach**

- Use a **small, realistic data set** (dozens of games, hundreds of saves/achievements/albums) to avoid “empty system” bias.
- Shape **synthetic load** around the most common flows:
  - search → game detail → save/achievement/album,
  - profile view → recent activity,
  - album filling and simple evidence submission.
- Measure:
  - p50/p95 response times,
  - error rates under moderate load,
  - basic resource usage trends (if environment supports it).

**Key checks**

- Global search (DSC-01) and catalogue filters (DSC-02 / CAT suites) stay within target latency (≈ 300–500 ms p95 under MVP load).
- Save upload start and completion give **immediate feedback** to the user; background tasks (AV, thumbnails, AI checks) don’t block UI unnecessarily.
- Profile and progress endpoints (PRO-02) remain responsive even for users with many entries.
- No catastrophic degradation when metadata providers are slow; caches and fallbacks keep main screens usable.

---
<br>

### 10.3 Security & Privacy-Oriented Testing (QA View)

> **Note:** This is **not** a full security audit or penetration test. The goal is to catch obvious issues that can be tested from the QA side.

**Focus areas**

- **Authentication & session** behaviour (ACC, PRO).
- **Evidence URL validation & host allowlist** (INT-04, GOV-03).
- **Logging behaviour** for sensitive data (all suites’ NFR hooks).

**Key checks**

- No access to restricted views (moderation queues, admin tools) without correct role (GOV-04, PRO-03).
- No tokens, passwords, or raw external IDs in:
  - API responses,
  - browser-visible HTML/JS,
  - or standard logs.
- Evidence URLs:
  - are validated against allowlisted hosts server-side,
  - never cause open redirects or script injection when rendered,
  - are stored as simple URLs (no unintentional media hosting).
- Session cookies:
  - use expected security flags (`HttpOnly`, `Secure` where applicable),
  - are invalidated on logout and timeout.

**Deliverable**

- A short **security-oriented QA checklist** completed per build, linked back to SRS NFRs and GOV/INT requirements.

---
<br>

### 10.4 Reliability, Failure Handling & Recovery

**Scope**

- Behaviour on **external provider failures** (Steam, RA, metadata APIs).
- Behaviour on **internal partial failures** (time-limited queries, background jobs).
- Basic **retry / circuit-breaker** behaviour as visible from the outside.

**Key checks**

- When Steam/RA or metadata providers are unavailable:
  - the system falls back to cached or last-known-good data,  
  - user sees a non-blocking banner (“some information may be stale”),  
  - main page remains usable (no 500-style error screens).
- When a single section fails on a composite page (e.g., saves for a game timeout):
  - the failure is **localised** (only that section shows an error with retry),
  - other sections remain interactive.
- Automatic background tasks (metadata refresh, stats aggregation, archival):
  - either succeed, or fail with **clear logging**,  
  - never leave user-facing data in a corrupted or half-updated state.

**Recovery validation**

- Repeat a small set of functional tests after **simulated outages** to confirm:
  - queues catch up,
  - stale flags disappear where appropriate,
  - no data loss is visible from a user perspective.

---
<br>

### 10.5 Usability & Accessibility (Lightweight)

**Scope**

- Core journeys for Regular users:
  - sign-up & sign-in (ACC),
  - search & pick a game (DSC, CAT),
  - upload/download saves (SAV),
  - view achievements/sets (ACH),
  - fill a sticker album (ALB),
  - view profile & progress (PRO).
- Basic accessibility concerns:
  - keyboard navigation,
  - focus handling,
  - colour/contrast for key states (selected/disabled/warning).

**Key checks**

- Flows are **navigable and understandable** without reading long documentation:
  - search leads naturally to game detail → saves/achievements/albums,
  - hints around spoiler-sensitive content are clear (ALB).
- Error messages are:
  - specific enough to help users fix issues,
  - not overloaded with technical jargon.
- For key screens, confirm:
  - focus order is logical,
  - primary actions are discoverable,
  - warning banners (e.g., UA/RU policy, legacy save warnings) don’t obscure core actions.
- For albums and achievements:
  - hints and spoiler warnings are visually distinct but not overwhelming,
  - players can understand what to do next (especially in “new-player” albums) without reading every detail.

---
<br>

### 10.6 Localisation & Regional Behaviour (EN / UA & UA/RU)

**Scope**

- UI localisation for **English** and **Ukrainian**:
  - labels, navigation, validation messages, policy notices.
- **UA/RU policy behaviour**:
  - banners for RU-linked titles,
  - flags for Ukrainian localisation / Ukrainian-developed games,
  - UA-specific views in catalogue, search, albums, and achievements.

**Key checks**

- EN ↔ UA toggle:
  - preserves current context (search filters, profile, game page),
  - changes only UI text and date/number formatting where applicable.
- UA/RU policy:
  - UA users see “at your own risk” style banners on RU-linked titles,
  - the same flags are used consistently across catalogue, album, and achievement contexts,
  - these banners do **not** appear in contexts where they would be confusing (e.g., generic profile stats without game context).
- Ukrainian-specific filters:
  - “Has Ukrainian localisation” and “Ukrainian-developed” behave consistently in catalogue and discovery.
- Content authored by users or curators remains in the language they chose; we only localise system-level labels and descriptions.

---
<br>

### 10.7 Non-Functional Acceptance Criteria

For Dream Project MVP to be considered **acceptable** from a non-functional perspective, the following must hold:

- **Performance**
  - Core pages (global search, catalogue detail, profile, album/achievement views) hit agreed response targets for p95 under realistic load.
  - No severe performance regressions compared to baseline measurements.

- **Security & Privacy (QA scope)**
  - No known leaks of tokens, passwords, or direct external IDs in logs or UI.
  - Evidence allowlist and session handling behave according to SRS and BRD requirements.

- **Reliability**
  - External provider failures result in **graceful degradation**, not blank or broken screens.
  - Background tasks can be interrupted and resumed without visible data corruption.

- **Usability & Accessibility**
  - Core user flows can be completed without workarounds.
  - No critical UX blockers (confusing states, impossible navigation) identified in test runs.

- **Localisation & Policy**
  - EN and UA experiences are feature-equivalent.
  - UA/RU policy banners and localisation flags appear only where intended and are consistent.

Once these criteria are met, and module-level NFR hooks (in sections 9.4–9.12) have been exercised at least once in a dedicated NFR pass, the test team can provide a **non-functional “go / no-go” recommendation** for the MVP release.

---
<br>
<br>

## 11. Test Execution & Reporting

This section describes **how** the test cases in this specification will be executed, tracked, and reported during the Dream Project MVP cycle.  
It is intentionally lightweight but explicit enough that a small team (or even a single tester) can **repeat the process** without guessing.

---
<br>

### 11.1 Execution Cycles & Test Runs

Dream Project MVP testing is organised into a few **short, repeatable cycles** rather than one huge “big bang” run.

**Planned cycles**

- **Cycle 0 – Smoke / Setup**
  - Goal: verify that environments, accounts, and core flows are minimally usable.
  - Coverage:  
    - ACC, CAT, PRO basic flows  
    - one simple path through SAV, ACH, ALB  
  - Outcome: either “OK to proceed” or a short list of **blockers**.

- **Cycle 1 – Core Functional Coverage**
  - Goal: execute all **high-priority** (`P1`) functional cases for each module:
    - ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT.
  - Focus: happy paths and critical edge cases; limited NFR checks.

- **Cycle 2 – Extended & Edge Cases**
  - Goal: execute remaining `P2` test cases and selected `P3` where risk is higher.
  - Focus: cross-module flows (e.g., discovery → saves → achievements → profile), negative tests, error handling.

- **Cycle 3 – Regression & UAT Prep**
  - Goal: re-run all **smoke + key regression** suites plus any test cases affected by recent changes.
  - Focus: stability for demo/UAT; verify fixes for high/medium defects.

> If timelines force us to skip a full cycle, we prioritise:
> 1) Smoke & Core coverage, 2) Regression on critical paths, 3) NFR spot checks.

For each cycle we record:

- test run name (e.g., `MVP-Cycle-2-Extended`),
- environment, build/version, date range,
- execution status (planned/executed/passed/failed/blocked) per test case.

---
<br>

### 11.2 Roles & Responsibilities

Given this is a **portfolio / small-team** project, roles may be combined, but responsibilities stay distinct.

- **Test Owner / QA**
  - Plans cycles and selects which suites to run.
  - Executes manual test cases and records results.
  - Raises defects and links them to test cases and requirements.

- **Business Analyst**
  - Clarifies expected behaviour when doubt appears.
  - Confirms whether issues are **true defects** vs **documentation gaps**.
  - Updates BRD / SRS / Use Cases when requirements change.

- **Developer(s)**
  - Fix defects and deploy new builds.
  - Provide technical explanation for tricky failures (e.g., integration edge cases).
  - Implement test hooks where needed (e.g., flags for NFR tests, fixtures).

- **Product/Project Owner**
  - Makes **go / no-go** calls based on defect reports and coverage summaries.
  - Prioritises which open issues can be deferred beyond MVP.

---
<br>

### 11.3 Daily Execution Workflow

A typical test execution day for a cycle should follow a simple rhythm:

1. **Morning – Planning & Sync**
   - Check which build/version is deployed to the test environment.
   - Review the **cycle plan** and pick the next batch of test cases (e.g., “TS-SAV-01 + TS-ACH-03 today”).
   - Re-check any **known blockers** or environment limitations.

2. **During the day – Execute & Log**
   - For each test case:
     - Set status: `Not Run` → `In Progress` → `Passed` / `Failed` / `Blocked`.
     - If **Failed**:
       - capture clear steps, observed vs expected, environment, screenshots/log snippets.
       - create a defect and link `TC-…` and, where possible, `BR-…` / `SR-…`.
     - If **Blocked**:
       - note the blocking issue (e.g., environment down, dependency missing).
   - Keep notes of **unexpected behaviour** even if it doesn’t fully break the case (possible UX improvements).

3. **End of day – Micro-report**
   - Summarise:
     - tests executed / passed / failed / blocked (counts per module),
     - new defects raised (with rough severity),
     - anything that might impact the next day (e.g., “Steam sandbox unstable, will delay INT tests”).

---
<br>

### 11.4 Test Status & Reporting

At the end of each cycle, we produce a **cycle report** that stays consistent across runs.

**Key elements**

- **Cycle metadata**
  - Cycle name, dates, environment, build/version.
- **Coverage summary**
  - Number of test cases by status: `Passed / Failed / Blocked / Not Run`.
  - Breakdown by module (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT).
- **Defect summary**
  - Count of open defects by severity (Critical/High/Medium/Low).
  - Highlights of the most important issues affecting MVP scope.
- **NFR snapshot**
  - Any performance/security/localisation checks run in this cycle and their outcome.
- **Risk notes**
  - Known areas with low coverage or remaining uncertainty.
  - Pointers to the Risks & Assumptions Register for updated entries.

---
<br>

### 11.5 Defect Management & Traceability

While there may not be a full issue tracker in use, we still treat defects in a **traceable** way.

- **Defect IDs**  
  Use a simple pattern like `DEF-001`, `DEF-002`, … or reuse IDs from GitHub/Jira if present.

- **Minimum fields**
  - Title (short summary).
  - Steps to reproduce.
  - Expected vs actual result.
  - Environment / build.
  - Severity (Critical/High/Medium/Low).
  - Linked artefacts:
    - Test case ID (`TC-…`).
    - Requirement ID (`BR-…` or `SR-…`) where applicable.

- **Traceability**
  - This document already links test suites to BRD/SRS/Use Cases.  
  - At test execution time, we extend that chain:  
    **Requirement → Test case → Execution result → Defect(s)**.

---
<br>

### 11.6 Exit Criteria & Sign-Off (Test Perspective)

From a **test execution** point of view, Dream Project MVP is ready when:

- All **P1 test cases** in scope for MVP:
  - have been executed at least once in the latest build,
  - and are marked `Passed`, with any failures either fixed or consciously deferred.
- No **Critical** defects remain open, and no **High** defects remain open on:
  - authentication,  
  - save upload/download and compatibility labelling,  
  - achievement/fan set core flows,  
  - sticker album basic flows,  
  - profile visibility/privacy.
- Non-functional checks in Section 10:
  - have been executed at least once,  
  - and no severe regressions were identified (or they are clearly documented in the risk log).

The final test summary for MVP should conclude with a short, explicit statement, for example:

> “Based on the executed tests and current open issues, QA recommends **GO** / **NO-GO** for Dream Project MVP, with the following known limitations: …”

---
<br>
<br>

## 12. UAT Plan

This section describes how **User Acceptance Testing (UAT)** will be organised for Dream Project MVP.  
The goal is to validate that the system, as implemented, is **fit for purpose** from the point of view of real users and domain stakeholders, not just from a purely technical perspective.

---
<br>

### 12.1 UAT Objectives & Scope

**Objectives**

UAT aims to:

- Confirm that Dream Project supports the **intended end-to-end workflows**:
  - discover a game and understand what the platform offers for it,
  - manage saves (where technically feasible for that game),
  - explore and use achievement sets (official, retro-integrated, and fan-made),
  - fill sticker albums and receive completion badges,
  - view a coherent profile and progress overview.
- Validate that the system behaves in line with the **Business Requirements Document (BRD)** and **Use Case Specification**, at a level that is understandable to non-technical stakeholders.
- Identify any **gaps, UX issues, or confusing behaviours** that would block or seriously reduce adoption by the target communities (retro players, regular players, curators, moderators).

**In scope**

- End-to-end flows that real users are expected to perform in MVP:
  - account sign-up, sign-in, basic profile setup;
  - searching/browsing for games (retro and modern);
  - uploading and using saves where allowed;
  - linking Steam / RetroAchievements profiles for supported games;
  - browsing and using achievement sets;
  - filling sticker albums (new-player-safe and full-story variants);
  - viewing profile stats and badges;
  - light reporting / moderation flows from a user’s perspective.
- Behaviour in **EN and UA** locales for core journeys.

**Out of scope**

- Low-level technical tests (performance/load, security penetration, detailed edge-case scenarios).
- Full regression of every negative path (covered in system/regression testing).
- Deep admin/operations workflows that are primarily internal (only basic moderator/Admin usage if needed to support UAT scenarios).

---
<br>

### 12.2 UAT Participants & Roles

Even if the “team” is small, UAT roles are defined so expectations are clear.

- **UAT Coordinator**
  - Prepares UAT scenarios and test data.
  - Briefs participants and collects feedback.
  - Consolidates UAT results and prepares the UAT summary.

- **Business / Product Stakeholder**
  - Evaluates whether implemented behaviour matches the **business intent**.
  - Decides which issues are blockers for MVP vs acceptable for later.

- **Representative Users**
  - **Retro-focused player**: cares about emulators, RetroAchievements, save-sharing for old titles.
  - **Regular player**: more familiar with modern platforms (Steam), may focus on achievements, albums, and basic saves.
  - **Curator-type user**: interested in authoring or maintaining achievement sets and sticker albums.
  - (Optionally) a **Moderator/Admin representative** for light moderation actions needed to support UAT flows.

Each participant receives:

- a short **UAT brief** (objectives, what to test, how to report issues),
- access to the test environment and credentials (or registration instructions),
- links to the UAT scenarios and feedback form/template.

---
<br>

### 12.3 UAT Scenarios (High-Level)

UAT will use a small set of **realistic, narrative-style scenarios** rather than executing every technical test case again.  
Examples (to be elaborated in a separate UAT checklist or script):

1. **US-UAT-01 – Discover a game and see what Dream Project offers**
   - Search for a known retro game.
   - View its catalogue page: see platform variants, UA flags, and what’s available (saves, achievements, albums).
   - Decide whether the platform is useful for this game at a glance.

2. **US-UAT-02 – Use saves to overcome a difficulty spike**
   - Find a game and a shared save that lets you continue after a difficult section.
   - Download and understand where this save will work (compatibility labels).
   - Confirm that the information presented would be enough to use the save in practice.

3. **US-UAT-03 – Explore achievements (official, retro, fan-made)**
   - Link a Steam or RetroAchievements profile (where applicable).
   - View achievements for a chosen game.
   - Browse one fan-made set and assess if structure, descriptions, and progress views are understandable.

4. **US-UAT-04 – Fill a sticker album without spoiling the game**
   - Open a “new-player-friendly” album template for a game.
   - Use hints to understand when to take screenshots and how not to miss key moments.
   - Upload screenshots for a few slots and see how spoiler warnings behave.
   - Complete a small album and observe how the badge and profile updates look.

5. **US-UAT-05 – View personal profile & progress**
   - View your own profile in EN and UA.
   - Check that progress indicators (sets completed, albums completed, saves shared/used) are understandable and believable.
   - Verify that public information shown on another user’s profile feels safe and appropriate.

6. **US-UAT-06 – Report problematic content**
   - From a save, sticker, or achievement set, submit a report for an issue (e.g., misleading description, NSFW).
   - Confirm that the reporting process is clear and doesn’t feel aggressive or confusing.

For each scenario, participants will capture:

- **Was the scenario successful?** (Yes/No/Partially)
- **Comments on clarity and usability.**
- **Any behaviour that felt wrong or surprising.**

---
<br>

### 12.4 UAT Entry & Exit Criteria

**Entry criteria**

UAT can start when:

- The build has passed **smoke tests** and the **key P1 system test cases** for each module (per section 11) are green or have acceptable workarounds.
- Core documentation is available to UAT participants:
  - short UAT brief,
  - links to relevant sections of BRD / Use Case Specification (optional but useful).
- Test environment is stable:
  - accounts can be created or pre-configured,
  - integrations (Steam/RA, metadata) are reachable or simulated as needed.

**Exit criteria**

UAT is considered complete when:

- All planned UAT scenarios have been executed by at least one representative participant.
- All **identified issues** have been:
  - logged and categorised (blocker / major / minor / UX improvement),
  - discussed with the business/product stakeholder.
- No **blocker** issues remain open, and all **major** issues have either:
  - been fixed and re-verified, or
  - been explicitly accepted as known limitations for MVP with a follow-up item in the backlog / risk register.

The UAT Coordinator then prepares a short **UAT Summary** with:

- list of scenarios executed,
- high-level outcome (e.g., “5/6 scenarios fully successful, 1 partially due to X”),
- list of accepted limitations.

---
<br>

### 12.5 Defect Handling & Feedback Loop

During UAT, issues will be recorded in a **simple, consistent format**, similar to defects from system testing but tagged as “UAT”.

For each UAT issue:

- **ID & title** (e.g., `UAT-ISSUE-003 – Confusing spoiler warnings in album view`),
- Scenario ID (`US-UAT-0X`),
- Steps taken,
- Expected vs actual behaviour,
- Severity from a **user acceptance** point of view (Blocker / Major / Minor / Cosmetic),
- Suggested change (optional).

Handling workflow:

1. UAT participant records the issue.
2. UAT Coordinator reviews and normalises description and severity.
3. Business/BA + Developer decide:
   - fix for MVP,
   - fix post-MVP,
   - or accept as-is (with rationale).
4. If fixed, issue is **retested** during the next test cycle or a dedicated re-check.

---
<br>

### 12.6 Schedule & Practical Notes

Given the scale of Dream Project, UAT can be run in a **short, focused window** (for example, 2–5 days), with:

- Day 1:  
  - Kick-off session, environment access, run `US-UAT-01` and `US-UAT-02`.
- Day 2:  
  - `US-UAT-03` and `US-UAT-04` (achievements and albums).
- Day 3:  
  - `US-UAT-05` and `US-UAT-06` (profiles, reporting), plus retests for any quick fixes.

If the same person plays multiple roles (BA, “user”, coordinator), the plan still applies; the difference is only **who** executes, not **what** is executed.

Regular short check-ins (even a 10–15 minute note at the end of each day) help ensure:

- issues are not lost,
- priorities are adjusted early,
- and it is clear whether UAT is converging towards acceptance or uncovering fundamental gaps.

---
<br>
<br>

## 13. Risks, Assumptions & Mitigations (Testing)

This section highlights **testing-specific risks and assumptions** for Dream Project MVP and summarises how they will be monitored and mitigated.  
It complements, but does not replace, the broader *Risks & Assumptions Register*.

---
<br>

### 13.1 Testing Risks

| ID | Risk | Description (Testing View) | Likely Impact | Initial Level | Mitigation / Handling |
|----|------|----------------------------|---------------|---------------|------------------------|
| TR-01 | Limited testing capacity | Testing is performed by a very small team (or a single person), which may limit how many test cases and NFR checks can be executed per cycle. | Gaps in coverage, undetected issues in low-priority areas. | Medium | Prioritise **P1** test cases and critical cross-module flows; mark lower-priority areas explicitly as partial coverage. Use short, well-defined cycles (Section 11) to avoid “everything at once” testing. |
| TR-02 | Environment instability | Test environment or external sandboxes (Steam, RA, metadata APIs) may be unstable or intermittently unavailable. | Blocked tests, false failures, unpredictable behaviour during execution. | Medium | Log environment status for each run; mark affected tests as **Blocked** (not Failed). Use fixtures/mocks where possible for INT-level tests. Re-run affected tests when environment stabilises. |
| TR-03 | Incomplete integration coverage | Not all external integration scenarios (Steam, RetroAchievements, metadata providers) can be fully simulated or exercised in MVP. | Some edge cases around integrations remain untested. | Medium | Focus on **most realistic** and business-critical integration paths; document which integration scenarios are **intentionally untested** and track them in the Risk & Assumptions Register. |
| TR-04 | Manual evidence review complexity | Achievement completion evidence (videos, screenshots, links) can be varied and time-consuming to validate manually. | Limited test depth for evidence flows; potential edge cases missed. | Medium | Use a **small, representative set** of evidence examples instead of many similar ones. Emphasise validation of allowlist logic and moderation hooks rather than exhaustively checking every content variant. |
| TR-05 | Data volatility in test env | Test data (games, saves, sets, albums) may be modified by development activities or parallel testing. | Test steps break or results become inconsistent between runs. | Medium | Define a **core test data set** and protect it as much as possible (read-only where feasible). Where data must change, include a simple reset/reseed script or procedure before major runs. |
| TR-06 | Coverage blind spots in NFRs | Performance, reliability, and localisation tests may only be partially executed due to time or tooling limitations. | Some non-functional weaknesses may remain undetected. | Medium | Treat NFR testing as **targeted checks** (Section 10), clearly document which NFR hooks have been exercised, and add uncovered ones as explicit follow-ups in the Risk & Assumptions Register. |
| TR-07 | Ambiguity between bug vs change request | Some feedback (especially in UAT) may blur the line between “defect” and “desired improvement”. | Confusion in defect tracking; inconsistent prioritisation. | Low–Medium | For each issue, confirm expected behaviour against BRD/SRS/Use Cases. Label as “Defect” or “Change request” explicitly; reflect final decision in the risk/assumption logs where it affects scope. |
| TR-08 | UA/RU policy behaviour not fully exercised | Policy-related behaviour (banners, hiding RU-linked titles, UA localisation flags) might not be tested in all relevant combinations. | Inconsistent or surprising behaviour for Ukrainian users. | Medium | Include **targeted** policy scenarios in DSC, CAT, ALB, ACH test suites and UAT. Any untested combinations are recorded as residual risk, not silently ignored. |

---
<br>

### 13.2 Testing Assumptions

| ID | Assumption | Rationale | Impact if False |
|----|------------|-----------|-----------------|
| TA-01 | A stable test environment (or clearly labelled test sandbox) is available for functional and UAT cycles. | Many test cases require consistent URLs, data, and external integration settings. | Testing may be repeatedly blocked or give unreliable results; additional time will be needed for environment stabilisation. |
| TA-02 | External services (Steam, RA, metadata providers) have at least a **basic sandbox or predictable test profile** available. | Integration tests rely on known fixtures or predictable accounts. | Integration-related tests become mostly theoretical or must rely on mocks only; this increases risk of integration surprises later. |
| TA-03 | A minimal set of representative test data (games, saves, sets, albums, users) is created and kept in sync with documents. | Many scenarios (especially UAT) depend on specific examples that make sense to non-technical stakeholders. | Test execution becomes harder to reproduce; scenarios may fail simply because the expected data is missing or inconsistent. |
| TA-04 | Time is reserved for at least one **regression cycle** before MVP sign-off. | Changes late in the cycle can break flows that previously worked. | New defects may reach UAT or “release” without being noticed, eroding confidence in the system. |
| TA-05 | BA / product stakeholder is available to clarify expected behaviour when uncertainties arise. | Some details, especially around policy and evidence handling, cannot be fully captured in advance. | Testers may classify issues incorrectly (e.g., treating intended behaviour as bugs or vice versa), leading to confusion and rework. |
| TA-06 | UAT participants are briefed and understand the limits of MVP scope. | Avoids misinterpreting “out-of-scope” items as critical failures. | UAT feedback might overemphasise features that were never planned for MVP, skewing priorities. |

If any assumption proves invalid during real work, it should be **promoted** into a risk entry and re-evaluated in the main *Risks & Assumptions Register*.

---
<br>

### 13.3 Mitigation Approach & Fallback Strategies

To keep testing effective and realistic under constraints:

- **Risk-based prioritisation**
  - Functional and non-functional tests are prioritised based on:
    - criticality of the flow (e.g., sign-in, saves, albums),
    - user-facing impact,
    - integration complexity.
  - Lower-risk areas may receive lighter coverage but are not silently ignored; their status is documented.

- **Tight coupling with the Risk & Assumptions Register**
  - When a testing risk or invalid assumption is identified, it is recorded in the central register with:
    - link to relevant test cases / modules,
    - current status and next review date.
  - This prevents testing concerns from “living only in QA notes”.

- **Use of fixtures, mocks, and controlled profiles**
  - Where external services are unreliable or limited:
    - focus on **adapter-level** INT tests with stable fixtures,
    - verify at least one realistic end-to-end flow,
    - document the parts that had to be simulated.

- **Transparent limitation notes in test reports**
  - Execution reports (Section 11) must explicitly mention:
    - which modules or scenarios were only partially covered,
    - which NFR checks could not be run,
    - which external dependencies behaved inconsistently.
  - This avoids overstating confidence in areas that haven’t been tested thoroughly.

---
<br>

### 13.4 Links to Risk & Assumptions Register

Testing-related entries in this section should be **kept aligned** with the dedicated *Risks & Assumptions Register* document:

- New risks discovered during test planning or execution (TR-xx) are added to the central register with references back to:
  - this Test Case Specification (Section 13),
  - the modules/suites they affect (Sections 9–12).
- Assumptions that are invalidated (TA-xx) are:
  - updated in the central register,
  - potentially reclassified as risks with mitigation actions.

This linkage ensures that testing is not isolated from the broader project risk view and makes it clear how test activities contribute to managing overall project uncertainty.

---
<br>
<br>

## 14. Roles & Responsibilities

This section defines who is responsible for **planning, executing, and acting on** the testing described in this document.  
In a small team, several roles may be played by the same person, but the **responsibility boundaries** stay the same.

---
<br>

### 14.1 Role Definitions

| Role | Description |
|------|-------------|
| **Business Analyst (BA)** | Owns the requirements baseline (BRD, SRS, Use Cases). Ensures that test design and defect triage stay aligned with business intent and scope. |
| **QA / Test Owner** | Owns this Test Case Specification. Plans cycles, prioritises suites, executes tests, and reports results. Maintains links between tests, defects, and requirements. |
| **Developer(s)** | Implement functionality and fixes. Support testability (test hooks, fixtures), clarify technical behaviour, and review test results that indicate defects or design gaps. |
| **Product / Project Owner (PO)** | Makes decisions on scope, priorities, and release readiness. Interprets test reports and UAT feedback in terms of “go / no-go” and post-MVP backlog. |
| **Curator / Domain Representative** | Provides practical input on how real users (especially creators) will interact with achievements, albums, and saves. Validates that flows feel meaningful and usable. |
| **Moderator / Admin Representative** | Validates moderation and governance workflows (reporting, queues, policy enforcement) from an operational point of view. |

> Note: in this project, BA, QA and sometimes Curator/Domain Representative can be embodied by the same person; the table below still treats them as logically separate roles.

---
<br>

### 14.2 Responsibilities by Testing Activity

| Activity | BA | QA / Test Owner | Developer(s) | PO | Curator / Domain Rep | Moderator / Admin Rep |
|---------|----|------------------|--------------|----|----------------------|------------------------|
| **Test scope definition** | A | R | C | C | C | C |
| **Test case design (this document)** | A | R | C | I | C | C |
| **Traceability to BRD / SRS / Use Cases** | A | R | C | I | I | I |
| **Test data definition (games, saves, sets, albums)** | A | R | C | I | C | I |
| **Environment readiness check** | C | R | C | I | I | I |
| **Functional test execution** | C | R | C | I | I | I |
| **NFR checks (performance, reliability, localisation)** | C | R | C | I | I | I |
| **UAT scenario definition** | A | R | C | C | C | C |
| **UAT execution** | C | C | I | C | R | C |
| **Defect creation & classification** | A (behaviour) | R | C | I | C (for UX/domain) | C (for moderation flows) |
| **Fix implementation & unit tests** | I | C | R | I | I | I |
| **Defect verification / regression** | C | R | C | I | I | I |
| **Risk & assumption updates (testing-related)** | A | R | C | C | I | I |
| **Release / MVP readiness decision** | C | C | C | A/R | I | I |

Legend:  
**R** – Responsible (does the work)  
**A** – Accountable (owns the outcome, final decision)  
**C** – Consulted (provides input, may review)  
**I** – Informed (kept up to date)

---
<br>

### 14.3 Collaboration & Handover

- **Before each test cycle**
  - BA and QA review any **requirement changes** since the last cycle.
  - QA confirms priorities and which modules/suites will be executed.
  - Developers highlight recent changes that may need targeted regression.

- **During test execution**
  - QA logs defects and ambiguities; BA helps decide whether each item is:
    - a true defect,
    - a change request,
    - or an expected but undocumented behaviour.
  - Developers provide clarifications for technical edge cases and integrations (Steam / RA / metadata).

- **For UAT**
  - BA and QA prepare concise **UAT scenarios** and ensure test data supports them.
  - Curator / Domain and Moderator / Admin representatives participate where their flows are exercised.
  - PO reviews UAT issues and decides which ones must be resolved pre-MVP.

- **At cycle / release sign-off**
  - QA produces a **cycle report** (Section 11), including status of P1 cases and major defects.
  - BA confirms that remaining issues do not invalidate key requirements.
  - PO uses this information to decide on **GO / NO-GO** and to update the future backlog.

This structure keeps testing anchored in the **requirements** while ensuring that technical, domain, and product perspectives are all represented when interpreting results and making decisions.

---
<br>
<br>

## 15. Schedule & Cycles

This section outlines the **high-level timetable** for testing Dream Project MVP and how the different cycles (system testing, NFR checks, and UAT) fit together.  
Exact calendar dates can be adapted, but the **sequence and dependencies** between activities should remain the same.

---
<br>

### 15.1 Overall Timeline Structure

Testing is organised into four main phases:

1. **Preparation & Smoke (T0)**  
   - Finalise test artefacts (this document, key data sets).  
   - Verify environment and integrations are minimally usable.  

2. **Core System Testing (T1)**  
   - Execute all **P1 functional** cases for core modules.  
   - Start running selected non-functional checks.  

3. **Extended & Regression Testing (T2)**  
   - Execute remaining **P2** cases and key cross-module scenarios.  
   - Run focused regression on areas touched by fixes.  

4. **UAT & Final Regression (T3)**  
   - Run UAT scenarios.  
   - Re-run smoke and core regression for the release candidate.  

For a compact MVP, these phases can be mapped roughly as:

- **Week 1** – Preparation & Smoke (T0)  
- **Week 2** – Core System Testing (T1)  
- **Week 3** – Extended & Regression (T2)  
- **Week 4** – UAT & Final Regression (T3)  

If time is tighter, phases can overlap (for example, starting UAT on stable areas while T2 continues on others).

---
<br>

### 15.2 Pre-Conditions for Starting Each Phase

- **T0 – Preparation & Smoke**
  - Core features deployed to test environment at least once.
  - Ability to:
    - register / sign in,
    - access catalogue and search,
    - open basic module pages (SAV, ACH, ALB, PRO).

- **T1 – Core System Testing**
  - Smoke tests green for:
    - ACC, CAT, SAV, ACH, ALB, PRO.
  - Minimum seed data created (a few games, saves, sets, albums, users).
  - External integrations configured or mocked at adapter level (INT).

- **T2 – Extended & Regression**
  - Majority of P1 defects from T1 analysed and fixes in progress.
  - Clear list of affected modules for targeted regression.

- **T3 – UAT & Final Regression**
  - UAT scenarios drafted (Section 12).  
  - No open **Critical** defects on core flows.  
  - Test environment stable enough for non-technical users to work without constant interruptions.

---
<br>

### 15.3 Cycle Breakdown

#### 15.3.1 Cycle T0 – Preparation & Smoke (≈ 2–3 days)

**Goals**

- Confirm basic viability of the environment.
- Finalise test data and refine ambiguous test cases.

**Key activities**

- Run minimal smoke suite:
  - registration/login,
  - search and game detail,
  - basic navigation into saves/achievements/albums/profile.
- Adjust:
  - test data (create/edit sample games, saves, sets, albums),
  - environment configuration (locales, UA/RU banners, external endpoints).
- Log any **environment-level issues** (marked as Blockers, not Defects against requirements).

---

#### 15.3.2 Cycle T1 – Core System Testing (≈ 1 week)

**Goals**

- Execute **all P1 functional test cases** for each module:
  - ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT (where realistic).
- Start small-scale non-functional checks on:
  - search performance,
  - save upload responsiveness,
  - basic localisation behaviour (EN/UA).

**Key activities**

- Run module suites focusing on happy paths and essential edge cases.
- Log defects with links to requirements and test cases.
- Provide a **mid-cycle status**:
  - % P1 executed,
  - main blockers,
  - areas needing design clarification.

---

#### 15.3.3 Cycle T2 – Extended & Regression (≈ 1 week)

**Goals**

- Execute remaining **P2** test cases and selected higher-risk **P3** cases.
- Validate **cross-module flows**, e.g.:
  - discovery → game → saves → profile,
  - game → achievements → profile stats,
  - album completion → badges → profile.
- Run **targeted NFR tests** (Section 10) on performance, reliability, localisation.

**Key activities**

- Focus on:
  - negative tests (invalid inputs, error handling),
  - policy-paths (UA/RU banners, evidence rules),
  - moderation/reporting flows (GOV).
- Re-test high-severity defect fixes from T1.
- End-of-cycle summary:
  - remaining open defects,
  - coverage gaps,
  - NFR tests executed and their outcome.

---

#### 15.3.4 Cycle T3 – UAT & Final Regression (≈ 1 week)

**Goals**

- Execute UAT scenarios (Section 12) with representative participants.
- Re-run smoke + core regression on the release candidate build.

**Key activities**

- Prepare UAT access and short briefing.
- Run scenarios:
  - US-UAT-01 … US-UAT-06 (or final set).
- Log UAT issues with clear “Defect vs Change request” status.
- Execute final regression run:
  - smoke suite,
  - selected P1 flows from each module,
  - any areas touched by late fixes.

**Outcome**

- UAT summary, including:
  - scenario success/failure,
  - accepted limitations.
- Final test summary for MVP including:
  - executed coverage,
  - outstanding risks,
  - recommendation for GO / NO-GO.

---
<br>

### 15.4 Buffers & Contingencies

Given limited time and resources, the schedule includes built-in **flexibility**:

- **Functional buffer**
  - If T1 uncovers more issues than expected, extend T1 slightly by taking some P2 cases into T2.
- **NFR buffer**
  - If formal performance/security tools are not available, focus on targeted, manually observed checks and document what remains untested.
- **UAT buffer**
  - Reserve time in T3 for at least **one re-test pass** of fixes that arise from UAT feedback.

If a phase must be shortened, priority is:

1. Preserve smoke + P1 coverage.  
2. Preserve one meaningful NFR pass.  
3. Preserve at least one UAT execution of each scenario.

---
<br>

### 15.5 Dependencies & Alignment with Project Plan

Testing cycles should stay aligned with the **Project Plan** milestones:

- T0/T1 align with **MVP feature freeze** on core modules.
- T2 aligns with **stabilisation** and completion of secondary features.
- T3 aligns with **MVP readiness** and any demo / publication date.

Whenever scope or dates in the Project Plan change, this section should be revisited and updated so that:

- test coverage remains realistic,
- the most critical flows are still exercised,
- and known reductions in coverage are explicitly documented rather than implicit.

---
<br>
<br>

## 16. References & Cross-Links

This section lists the main documents and artefacts that this **Test Case Specification** relies on, and shows how they fit together.  
It exists so that anyone reading this file can quickly jump to the right source of truth when a test, requirement, or risk needs clarification.

---
<br>

### 16.1 Referenced Documents

| ID | Document | Purpose / Notes |
|----|----------|-----------------|
| **DOC-BC** | **Business Case – Dream Project** (`business-case.md`) | Describes the overall problem, vision, business objectives, success criteria, scope boundaries, and high-level risks/assumptions. Forms the top-level “why” for all testing. |
| **DOC-BA** | **Business Analysis Document** (`business-analysis-document.md`) | Provides detailed business context, stakeholder analysis, AS-IS / TO-BE views, business processes, and business rules. Many test expectations for behaviour and flows trace back here. |
| **DOC-SCP** | **Stakeholder & Communication Plan** (`stakeholder-and-communication-plan.md`) | Defines stakeholder groups, communication channels, and decision paths. Used in this document mainly for UAT planning, roles & responsibilities, and escalation/communication expectations. |
| **DOC-BRD** | **Business Requirements Document (BRD)** (`brd.md`) | Captures business-level requirements, solution scope, non-functional needs (business view), assumptions, constraints, and dependencies. `BR-…` IDs in this Test Case Specification refer to entries here. |
| **DOC-SRS** | **System Requirements Specification (SRS)** (`srs.md`) | Provides detailed functional and non-functional system requirements, organised by modules and system views. `SR-…` IDs and module structure (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT) are taken from here. |
| **DOC-UCS** | **Use Case Specification** (`use-case-specification.md`) | Describes user-facing flows and interactions in use case form. `UC-…` IDs referenced in test suites and individual test cases link back to these scenarios. |
| **DOC-RISK** | **Risks & Assumptions Register** (`risk-and-assumptions-register.md`) | Central register for project-level risks and assumptions. Testing-specific risks and assumptions in Section 13 are cross-referenced with entries here. |
| **DOC-PLAN** | **Project Plan** (`project-plan.md`) | Outlines MVP scope, work breakdown, schedule, milestones, and resources. Section 15 of this document mirrors and aligns the testing schedule with this plan. |
| **DOC-RTM** | **Requirements Traceability Matrix (RTM)** (planned) | Will provide a consolidated mapping between `BR-…`, `SR-…`, `UC-…`, and `TC-…` items. This Test Case Specification is written so that it can be plugged into the RTM without restructuring. |
| **DOC-DIAG** | **Diagrams & Visuals** (`Diagrams.md` / visual artefacts) | Contains context diagrams, process diagrams, wireframes, and data model visuals used to clarify flows and entities tested here, especially for navigation and data-related tests. |

---
<br>

### 16.2 Traceability Anchors in This Document

This Test Case Specification uses a consistent set of **IDs and anchors** so it can be navigated and cross-checked against other artefacts:

- **Requirements**
  - `BR-…` – Business requirements from **DOC-BRD**.
  - `SR-…` – System requirements from **DOC-SRS**.
  - `NFR-…` – Non-functional requirements (where explicitly defined in BRD/SRS).

- **Use Cases**
  - `UC-…` – Use cases from **DOC-UCS**.  
    Each test suite in Section 9 lists relevant use cases in its traceability block.

- **Test Suites & Cases**
  - `TS-<MODULE>-NN` – Test suites in this document (e.g., `TS-SAV-01`, `TS-ACH-02`).
  - `TC-<MODULE>-NNN` – Individual test cases (e.g., `TC-SAV-005`).  
    These IDs are designed to be stable so they can be referenced from:
    - RTM,
    - defect reports,
    - UAT notes.

- **Risks & Assumptions**
  - `RISK-…` and `ASSUMP-…` – Project-level entries in **DOC-RISK**.
  - `TR-…` and `TA-…` – Testing-focused risks and assumptions in Section 13 of this document, which should be mirrored or referenced in **DOC-RISK**.

Whenever a test suite references `BR-…`, `SR-…`, `UC-…`, `RISK-…`, or `ASSUMP-…`, the intent is:

> requirement → use case → test suite / test case → defect (if any) → risk/assumption updates.

---
<br>

### 16.3 Update & Maintenance Rules

To keep cross-links reliable over time:

1. **When a requirement changes**  
   - Update the relevant `BR-…` or `SR-…` entry in **DOC-BRD** / **DOC-SRS**.  
   - Check this Test Case Specification for:
     - impacted suites (Section 9),
     - impacted non-functional hooks (Sections 10–11),
     - any references in Sections 12–15.
   - Adjust test descriptions and traceability lists accordingly.

2. **When a new feature or module appears**
   - Extend **DOC-BRD**, **DOC-SRS**, and **DOC-UCS** first.
   - Add:
     - new module/submodule sections in this document (Section 9),
     - new UAT scenarios if needed (Section 12),
     - any new NFR hooks (Section 10).

3. **When a risk or assumption is discovered during testing**
   - Record it in **Section 13** (`TR-…` / `TA-…`).
   - Mirror or reference it in **DOC-RISK**, tying it to:
     - the impacted requirement(s),
     - the relevant modules/suites.

4. **When the project plan or schedule changes**
   - Reflect the change in **DOC-PLAN**.
   - Adjust Section 15 (Schedule & Cycles) so the relationship between:
     - test cycles,
     - milestones,
     - and UAT timing remains explicit.

5. **When diagrams are updated**
   - If context diagrams, process diagrams, or wireframes change in **DOC-DIAG**:
     - review test cases that rely heavily on those visuals (especially navigation and process-related suites),
     - update wording in test descriptions to match new flows or screen layouts.

By following these rules, the Test Case Specification remains aligned with the rest of the Dream Project documentation set and can serve as a reliable bridge between **requirements**, **design**, and **implementation**.

---
<br>
<br>

## 17. Change History

| Version | Date       | Author | Description |
|---------|-----------|--------|-------------|
| 0.1     | 2025-11-27 | BA/QA  | Initial draft of Test Case Specification structure (sections 1–8) and first version of core module coverage outline. |
| 0.2     | 2025-11-28 | BA/QA  | Expanded section 9 with detailed module suites (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT). Added NFR hooks (Section 10) and basic execution/reporting flow (Section 11). |
| 0.3     | 2025-11-30 | BA/QA  | Added UAT Plan (Section 12), testing risks/assumptions (Section 13), roles & responsibilities (Section 14), schedule & cycles (Section 15), and references (Section 16). Aligned traceability references with BRD, SRS, and Use Case Specification. |
| 0.4     | 2025-12-01 | BA/QA  | Minor wording refinements, localisation/UA–RU policy clarifications in GOV/DSC/INT-related tests, and consistency fixes across suite descriptions and IDs. |

