# Requirements Traceability Matrix (RTM)

## Contents

- **[1. Document Purpose & Scope](#1-document-purpose--scope)**  
  Explains why the RTM exists in Dream Project, what it covers, and how it should be used by BA, QA, and devs during change impact analysis, testing, and sign-off.

- **[2. Sources & ID Conventions](#2-sources--id-conventions)**  
  Lists the upstream artefacts this matrix depends on (Business Case, BRD, SRS, Use Case Specification, Test Case Specification, Risk & Assumptions Register) and defines all ID formats used (`BR-…`, `SR-…`, `UC-…`, `TC-…`, `NFR-…` etc.).

- **[3. RTM Structure & Reading Guide](#3-rtm-structure--reading-guide)**  
  Describes how the matrix is organised (by business requirement, by system requirement, and by use case), what each column means, and how to interpret “Covered / Partial / Gap” statuses.

- **[4. Business Requirements Traceability (BRD → SRS / UC / Tests)](#4-business-requirements-traceability-brd--srs--uc--tests)**  
  For each **business requirement** from the BRD, shows which system requirements, use cases, and test cases implement or validate it, plus coverage status and comments.

- **[5. System Requirements Traceability (SRS → UC / Tests)](#5-system-requirements-traceability-srs--uc--tests)**  
  For each **system requirement** from the SRS, lists related use cases and test cases, indicating whether behaviour is fully, partially, or not yet covered in tests.

- **[6. Use Case Traceability (UC → Requirements / Tests)](#6-use-case-traceability-uc--requirements--tests)**  
  For each **use case** in the Use Case Specification, shows the linked business and system requirements and the test cases that exercise that scenario.

- **[7. Non-Functional Requirements Mapping](#7-non-functional-requirements-mapping)**  
  Maps non-functional requirements (performance, security/privacy, availability, localisation, moderation SLAs) to the tests and monitoring/measurement points that provide evidence for them.

- **[8. Risks, Assumptions & Test Evidence Links](#8-risks-assumptions--test-evidence-links)**  
  Connects key risks and assumptions from the Risks & Assumptions Register to the requirements and test cases that mitigate or validate them, including where evidence lives (reports, dashboards).

- **[9. Coverage Summary & Gaps](#9-coverage-summary--gaps)**  
  High-level view by module (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT): how many requirements are fully covered, partially covered, or not covered at all, with explicit gap entries and owners.

- **[10. Change Impact & Maintenance Rules](#10-change-impact--maintenance-rules)**  
  Defines how to update the RTM when requirements, use cases, or tests change, and how to use the matrix for impact analysis when something is added/removed/modified.

- **[11. Ownership & Review Cadence](#11-ownership--review-cadence)**  
  Clarifies who owns the RTM, who can update it, and how often it should be reviewed (e.g., per sprint, per gate).

- **[12. Change History](#12-change-history)**  
  Version log for the RTM itself: when it was created, what major updates were made, and by whom.

---
<br>
<br>

## 1. Document Purpose & Scope

The Requirements Traceability Matrix (RTM) for **Dream Project** provides a **single, consolidated view** of how all major requirements are connected to design, implementation, and testing artefacts.

In practical terms, this document answers four questions:

1. **Where did this requirement come from?**  
   – link to the original business need or problem statement.

2. **How is it specified at system level?**  
   – which SRS entries turn the idea into concrete system behaviour.

3. **Where is it exercised in flows and tests?**  
   – which use cases and test cases cover it.

4. **Is it actually covered, partially covered, or not covered at all yet?**  
   – coverage status that can be reviewed at any point in time.

---
<br>

### 1.1 Objectives

The RTM is created to:

- **Ensure full forward and backward traceability**
  - From Business Requirements (`BR-…`) → System Requirements (`SR-…`) → Use Cases (`UC-…`) → Test Cases (`TC-…`).
  - And back: from any test or defect, it should be possible to find the impacted requirement(s).

- **Support impact analysis**
  - When a requirement is added, changed, or removed, the RTM shows:
    - which use cases must be updated,
    - which test suites and individual test cases are affected,
    - which related risks or assumptions may need to be revisited.

- **Make coverage and gaps explicit**
  - Show which requirements are **Fully Covered**, **Partially Covered**, or currently **Not Covered** by tests.
  - Highlight modules or requirement groups where coverage is weak or still missing.

- **Align multiple documents**
  - Keep the **Business Case, BRD, SRS, Use Case Specification, Test Case Specification, Project Plan, and Risk Register** in sync by providing cross-links between them.

---
<br>

### 1.2 Scope

The RTM covers:

- All **Business Requirements** in the BRD that are in scope for **Dream Project MVP**.
- All **System Requirements** in the SRS for MVP modules:
  - ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT.
- All **Use Cases** that describe user-visible flows for these requirements.
- All **Test Suites and Test Cases** defined in the **Test Case Specification** that validate those flows.
- Selected **Non-Functional Requirements** where explicit test evidence exists (performance, reliability, security/privacy, localisation, UA/RU policy behaviour).

Out of scope:

- Detailed implementation-level design (class diagrams, code-level design decisions).
- Test cases that are purely exploratory and not yet mapped to specific requirements.
- Future post-MVP features that have not yet been formalised in BRD/SRS.

---
<br>

### 1.3 Intended Audience

The RTM is meant to be read and updated by:

- **Business Analyst** – to confirm that each requirement has downstream design and test coverage, and to assess impact when scope changes.
- **QA / Test Owner** – to see which requirements and use cases are already covered by tests and where new or updated test cases are needed.
- **Developers** – to understand which tests and flows are tied to the requirements they are implementing or modifying.
- **Product / Project Owner** – to evaluate readiness and risk before decisions on MVP scope, cut lines, and release.

---
<br>

### 1.4 How to Use This Document

At a high level:

- When **adding or changing a requirement**:
  - find or add its entry in the relevant RTM section (Business or System),
  - link it to corresponding SRS items, use cases, and tests,
  - update coverage status and any notes.

- When **planning or reviewing tests**:
  - scan the RTM for requirements marked **Partially Covered** or **Not Covered**,
  - ensure test design and execution explicitly address those gaps.

- When **analysing a defect or risk**:
  - start from the affected test case(s),
  - move back through the RTM to see which requirements and use cases are impacted,
  - update the Risk & Assumptions Register where needed.

---
<br>
<br>

## 2. Sources & ID Conventions

This section lists the **upstream documents** that feed into the RTM and defines the **ID formats** used across all Dream Project artefacts.  
All traceability in later sections relies on these IDs being stable and used consistently.

---
<br>

### 2.1 Source Documents

The RTM pulls its entries from the following documents:

| Code | Document | File (example) | Main Contents Used in RTM |
|------|----------|----------------|----------------------------|
| **DOC-BC** | Business Case | `business-case.md` | Vision, problem statement, high-level business objectives, success criteria, scope, major risks/assumptions. |
| **DOC-BA** | Business Analysis Document | `business-analysis-document.md` | Detailed business context, stakeholders, AS-IS / TO-BE views, business processes, business rules, data view. |
| **DOC-SCP** | Stakeholder & Communication Plan | `stakeholder-and-communication-plan.md` | Stakeholder groups, influence/interest levels, communication channels and cadence. |
| **DOC-BRD** | Business Requirements Document | `brd.md` | Formal business requirements (`BR-…`), solution scope, non-functional requirements (business view), assumptions, constraints, dependencies. |
| **DOC-SRS** | System Requirements Specification | `srs.md` | Functional and system-level non-functional requirements (`SR-…` / `NFR-…`), organised by modules and system views. |
| **DOC-UCS** | Use Case Specification | `use-case-specification.md` | Use cases (`UC-…`) per module, with actors, preconditions, main/alternate flows and postconditions. |
| **DOC-RISK** | Risks & Assumptions Register | `risk-and-assumptions-register.md` | Project risks (`RISK-…`), assumptions (`ASSUMP-…`), scoring, status, and mitigation/validation notes. |
| **DOC-PLAN** | Project Plan | `project-plan.md` | MVP scope, work breakdown, schedule, milestones, resources, and planning risks. |
| **DOC-TCS** | Test Case Specification | `test-case-specification.md` | Test suites (`TS-…`) and test cases (`TC-…`), test approach, NFR test plan, UAT plan, test risks (`TR-…` / `TA-…`). |
| **DOC-DIAG** | Diagrams & Visuals | `Diagrams.md` + visual files | Context diagrams, process diagrams, data model visuals and wireframes used to interpret flows and entities. |

Where possible, the RTM columns will reference the **codes and IDs** defined in these documents rather than repeating full descriptions.

---
<br>

### 2.2 ID Schemes

To keep traceability clear, Dream Project uses a small, consistent set of ID patterns.

#### 2.2.1 Business & System Requirements

- **Business Requirements**  
  - Format: `BR-<AREA>-NN` or `BR-NN`  
    - Examples: `BR-GEN-01`, `BR-SAV-03`, `BR-ACH-02`  
  - Defined and described in **DOC-BRD**.

- **System Requirements**  
  - Format: `SR-<MODULE>-NN`  
    - `<MODULE>` ∈ `ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT`  
    - Examples: `SR-ACC-01`, `SR-SAV-12`, `SR-ALB-05`  
  - Defined and described in **DOC-SRS**.

- **Non-Functional Requirements (System View, when explicitly numbered)**  
  - Format: `NFR-<CATEGORY>-NN` or `NFR-NN`  
    - Examples: `NFR-PERF-01`, `NFR-SEC-02`  
  - Some are embedded in BRD/SRS text; where IDs exist, they are referenced explicitly.

#### 2.2.2 Use Cases

- **Use Case IDs**  
  - Format: `UC-<MODULE>-NN`  
    - `<MODULE>` as above.  
    - Examples: `UC-ACC-01`, `UC-SAV-03`, `UC-ALB-02`  
  - Defined in **DOC-UCS**, including actors, flows, and postconditions.

#### 2.2.3 Tests

- **Test Suites**  
  - Format: `TS-<MODULE>-NN`  
    - Example: `TS-ACC-01`, `TS-ACH-02`, `TS-GOV-04`  
    - Each suite groups related test cases (e.g., sign-in flows, album creation, moderation queue).

- **Test Cases**  
  - Format: `TC-<MODULE>-NNN`  
    - Example: `TC-ACC-001`, `TC-SAV-015`, `TC-DSC-040`  
  - Defined and described in **DOC-TCS** under the relevant module suite.

These IDs are intended to be **stable anchors** for the RTM; if a test case is deleted or merged, the ID should be retired, not silently reused.

#### 2.2.4 Risks & Assumptions

- **Project-level Risks**  
  - Format: `RISK-NN` or `RISK-<AREA>-NN`  
    - Examples: `RISK-GEN-01`, `RISK-INT-02`  
  - Defined in **DOC-RISK**.

- **Project-level Assumptions**  
  - Format: `ASSUMP-NN`  
    - Examples: `ASSUMP-01`, `ASSUMP-05`  
  - Defined in **DOC-RISK**.

- **Testing-specific Risks / Assumptions**  
  - Format:  
    - Testing risk: `TR-NN` (e.g., `TR-01`)  
    - Testing assumption: `TA-NN` (e.g., `TA-03`)  
  - Defined in **DOC-TCS** (Section 13) and cross-linked back to **DOC-RISK** where relevant.

---
<br>

### 2.3 Module Codes

Throughout the RTM, module-level requirements, use cases, and tests share a common **module code**:

| Code | Module / Area | Examples of Scope |
|------|----------------|-------------------|
| **ACC** | Accounts & Authentication | Registration, login, session handling, password reset, role assignment. |
| **CAT** | Game Catalogue & Localisation Flags | Game metadata, platform/variant info, UA localisation flags, UA/RU policy flags. |
| **SAV** | Save Management | Save upload/download, metadata, compatibility labels, archival. |
| **ACH** | Achievement Tracking | Official, retro-integrated and fan-made sets, progress, evidence links. |
| **ALB** | Sticker Albums | Album templates, slot rules, spoiler-safe variants, completion badges. |
| **PRO** | Profiles & Progress | User profile, badges, statistics, visibility/privacy controls. |
| **GOV** | Moderation & Governance | Reporting, moderation queues, policy enforcement, roles and audit. |
| **DSC** | Discovery & Search | Global search, filters/facets, recommendations, empty/error states. |
| **INT** | External Integrations & Adapters | Steam, RetroAchievements, metadata providers, evidence host allowlist. |

These codes are used:

- in requirement IDs (`SR-SAV-…`, `SR-ACH-…`),
- in use case IDs (`UC-ALB-…`, `UC-DSC-…`),
- in test IDs (`TS-PRO-…`, `TC-GOV-…`),
- and as grouping keys in RTM tables in Sections 4–9.

---
<br>
<br>

## 3. RTM Structure & Reading Guide

This section explains **how the RTM is organised**, what each table shows, and how to read the columns and coverage statuses.  
The next sections (4–9) apply this structure to Dream Project’s actual requirements.

---
<br>

### 3.1 Levels of Traceability

The RTM is split into several **complementary views**:

- **Business-driven view** (Section 4)  
  Each **Business Requirement (`BR-…`)** is the starting point.  
  For every BR, we list:
  - which **System Requirements (`SR-…`)** implement it,
  - which **Use Cases (`UC-…`)** describe it in user terms,
  - which **Test Cases (`TC-…`)** verify it,
  - and whether coverage is **Full / Partial / None**.

- **System-driven view** (Section 5)  
  Each **System Requirement (`SR-…`)** is the starting point.  
  For every SR, we show:
  - which **BRs** it supports (if any),
  - which **UCs** use it,
  - which **TCs** cover it,
  - and any notes about gaps or future work.

- **Use case-driven view** (Section 6)  
  Each **Use Case (`UC-…`)** is the starting point.  
  For every UC, we show:
  - linked **BRs** and **SRs**,
  - key **test suites/cases** that exercise it,
  - and whether it is adequately covered, partially covered, or untested.

- **Non-functional mapping** (Section 7)  
  Focuses on **NFRs** (performance, security/privacy, reliability, localisation, moderation/policy), and how they:
  - relate back to BRD/SRS entries, and
  - are supported by specific NFR tests, monitoring hooks, or UAT checks.

- **Coverage summary & gaps** (Section 9)  
  Provides a **summary by module** (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT), showing:
  - how many requirements are fully/partially/not covered,
  - and where explicit gaps remain.

This allows you to **start from wherever you are** (business request, system change, use case, or test) and find the corresponding links.

---
<br>

### 3.2 Coverage Status Codes

To avoid long prose in every row, the RTM uses a small set of **coverage codes**:

- **FC – Fully Covered**  
  - The requirement has at least **one meaningful use case** and **one or more test cases**.  
  - Those tests are implemented and planned to be executed in regular cycles.

- **PC – Partially Covered**  
  - Some aspects of the requirement are covered, but:
    - not all flows are tested, or
    - certain modules/roles are missing, or
    - only functional, but not non-functional aspects are covered.  
  - Often indicates that more test cases are planned or a future enhancement is expected.

- **NC – Not Covered**  
  - No concrete test case exists yet, or the requirement is only covered indirectly.  
  - These entries should either:
    - be addressed by new tests, or
    - be consciously accepted as a gap (with rationale).

- **NA – Not Applicable (in this view)**  
  - Used occasionally where a requirement is deliberately out of MVP scope or where a test dimension doesn’t make sense (e.g., some very high-level BRs that are validated only via UAT/business review, not atomic test cases).

Each RTM table includes a **Coverage** column using these codes, plus an optional **Notes** column for short explanations (e.g., “only EN locale covered in this version”, “NFR tests planned post-MVP”).

---
<br>

### 3.3 Column Definitions

The exact set of columns varies slightly between sections, but the main patterns are:

#### 3.3.1 Business Requirements View (Section 4)

Typical columns:

| Column | Meaning |
|--------|---------|
| **BR ID** | Business Requirement ID from BRD (`BR-…`). |
| **BR Title / Summary** | Short description of the business requirement. |
| **Module(s)** | Primary modules affected (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT). |
| **Related SR IDs** | System Requirements implementing this BR. |
| **Related UC IDs** | Use Cases that operationalise this BR. |
| **Related TC IDs / Suites** | Test Cases or suites that verify this BR. |
| **NFR Links** | Any NFRs strongly tied to this BR. |
| **Coverage** | `FC / PC / NC / NA` as defined above. |
| **Notes** | Short comments: gaps, assumptions, future scope. |

#### 3.3.2 System Requirements View (Section 5)

Typical columns:

| Column | Meaning |
|--------|---------|
| **SR ID** | System Requirement ID from SRS (`SR-…`). |
| **SR Title / Summary** | Short description of the system behaviour. |
| **BR Link(s)** | Business Requirements that this SR supports. |
| **Module** | Single main module (ACC, SAV, etc.). |
| **UC Link(s)** | Use Cases that involve this SR. |
| **TC Link(s)** | Test Cases that exercise this SR. |
| **Coverage** | `FC / PC / NC / NA`. |
| **Notes** | Implementation status, known gaps, dependencies. |

#### 3.3.3 Use Case View (Section 6)

Typical columns:

| Column | Meaning |
|--------|---------|
| **UC ID** | Use Case ID from Use Case Specification (`UC-…`). |
| **Name / Summary** | Short use case title. |
| **Actors** | Primary actors (Regular User, Curator, Admin, etc.). |
| **BR Link(s)** | Business Requirements that motivate this UC. |
| **SR Link(s)** | System Requirements this UC touches. |
| **TC Link(s)** | Test suites and cases that exercise this UC. |
| **Coverage** | `FC / PC / NC / NA`. |
| **Notes** | Known missing alternate flows, localisation coverage, etc. |

---
<br>

### 3.4 How to Read & Navigate the RTM

Some typical use patterns:

- **Starting from a business requirement (top–down)**  
  1. Find the BR in Section 4.  
  2. Check which SRs, UCs, and TCs are linked.  
  3. Look at **Coverage** and **Notes** to see if additional tests are needed.

- **Starting from a system change (middle–out)**  
  1. Identify the affected `SR-…` in SRS.  
  2. Open Section 5, find that SR ID.  
  3. See which UCs and TCs are linked.  
  4. Use that list to plan regression tests and update use case descriptions if needed.

- **Starting from a use case (flow–centric)**  
  1. Find `UC-…` in Section 6.  
  2. Review linked BR/SR entries to understand the intent and constraints.  
  3. See which test cases cover it and whether alternate/error flows are adequately tested.

- **Starting from a test (bottom–up)**  
  1. Take the `TC-…` ID from the Test Case Specification.  
  2. Find it in the appropriate RTM section (usually 4–6 under the module it belongs to).  
  3. Navigate back to requirements and use cases to see **what exactly** that test is proving.

---
<br>
<br>

## 4. Business Requirements Traceability (BRD → SRS / UC / Tests)

This section starts from **Business Requirements (BRD)** and shows how each one is realised in:

- **System Requirements** (`SR-…`) from the SRS,
- **Use Cases** (`UC-…`) from the Use Case Specification,
- **Test Suites / Test Cases** (`TS-…` / `TC-…`) from the Test Case Specification,

plus an explicit **coverage status**.

> The table below focuses on the **core MVP BRs**. Additional BRs can be added in the same pattern.

---

### 4.1 Traceability Table (Excerpt)

| BR ID | BR Title / Summary | Module(s) | Related SR IDs (examples) | Related UC IDs (examples) | Related TS / TC IDs (examples) | NFR Links | Coverage | Notes |
|-------|--------------------|-----------|---------------------------|----------------------------|---------------------------------|-----------|----------|-------|
| **BR-ACC-01** | Users can register and sign in securely to Dream Project. | ACC, PRO | `SR-ACC-01` (registration), `SR-ACC-02` (login), `SR-ACC-05` (password reset) | `UC-ACC-01` Sign up, `UC-ACC-02` Sign in, `UC-ACC-03` Password reset | `TS-ACC-01` (basic auth), e.g. `TC-ACC-001`–`TC-ACC-010` | `NFR-SEC-01` (secure auth), `NFR-AVAIL-01` | **FC** | Core authentication flows fully covered in functional tests and UAT smoke checks. |
| **BR-ACC-02** | Platform supports role-based access: Regular User, Curator, Admin. | ACC, PRO, GOV | `SR-ACC-06` (role model), `SR-PRO-03` (role display), `SR-GOV-01` (role checks) | `UC-ACC-04` Assign roles, `UC-GOV-01` Access moderation tools | `TS-ACC-02` (roles), `TS-GOV-01` (moderation access) | `NFR-SEC-01` | **PC** | Role model and main checks tested; some low-frequency admin flows intentionally light for MVP. |
| **BR-CAT-01** | Provide a unified game catalogue with variants (platform/region) and key metadata. | CAT, DSC | `SR-CAT-01`–`SR-CAT-05` (catalogue listing/detail), `SR-DSC-01` (search) | `UC-CAT-01` Browse catalogue, `UC-CAT-02` View game details, `UC-DSC-01` Global search | `TS-CAT-01` (listing), `TS-DSC-01` (search basics) | `NFR-PERF-01` (search latency) | **FC** | Listing, details, and basic search covered; advanced filters handled as P2 tests. |
| **BR-CAT-02** | Indicate Ukrainian localisation and Ukrainian-developed games. | CAT | `SR-CAT-16`–`SR-CAT-19` (UA flags), `SR-INT-03`–`SR-INT-09` (UA metadata) | `UC-CAT-03` View localisation flags | `TS-CAT-02` (UA flags), `TS-INT-03` (UA metadata adapter) | `NFR-LOC-01` | **PC** | Flags and banners tested for representative titles; exhaustive coverage of all combinations deferred. |
| **BR-CAT-03** | Mark RU-linked titles and display warnings for Ukrainian users. | CAT, GOV | `SR-CAT-20` (RU flag), `SR-GOV-12`–`SR-GOV-15` (policy banners) | `UC-CAT-04` View RU-linked game, `UC-GOV-03` Configure policy banners | `TS-CAT-03` (policy banners), `TS-GOV-03` (policy enforcement) | `NFR-LOC-02` (regional behaviour) | **PC** | Main banner behaviour covered; edge cases (mixed publishers) tracked as risk and future tests. |
| **BR-SAV-01** | Allow users to upload, store, and download save files for supported games. | SAV, CAT | `SR-SAV-01`–`SR-SAV-06` (upload/download, metadata), `SR-CAT-07` (game–save link) | `UC-SAV-01` Upload save, `UC-SAV-02` Download save | `TS-SAV-01` (basic saves), `TS-SAV-02` (download) | `NFR-SEC-02` (data protection), `NFR-AVAIL-01` | **FC** | Happy paths and key error cases covered; very rare edge formats left for post-MVP. |
| **BR-SAV-02** | Provide metadata and search/filter for saves (chapter, playtime, difficulty, etc.). | SAV, DSC | `SR-SAV-07`–`SR-SAV-10` (metadata fields), `SR-DSC-03` (save filters) | `UC-SAV-03` Search saves | `TS-SAV-03` (filtering), `TS-DSC-02` (save facet tests) | `NFR-PERF-01` | **PC** | Main filters covered; some niche metadata combinations not exhaustively tested. |
| **BR-SAV-03** | Indicate version/compatibility and legacy status of saves. | SAV, INT | `SR-SAV-11`–`SR-SAV-15` (compatibility flags), `SR-INT-01`–`SR-INT-02` (build/manifest info) | `UC-SAV-04` View compatibility, `UC-SAV-05` Archive legacy saves | `TS-SAV-04` (compatibility labels), `TS-INT-01` (Steam build mapping) | `NFR-USAB-01` (clear warnings) | **PC** | Core labels and warnings tested; community confirmation flows marked for later enhancement. |
| **BR-ACH-01** | Provide a unified view of official achievements for supported modern titles. | ACH, INT | `SR-ACH-01`–`SR-ACH-04`, `SR-INT-10`–`SR-INT-15` (Steam adapter) | `UC-ACH-01` View official achievements | `TS-ACH-01` (official sets), `TS-INT-01` (Steam integration) | `NFR-PERF-02` | **FC** | Representative titles and profiles tested end-to-end; further titles rely on same mapping logic. |
| **BR-ACH-02** | Integrate RetroAchievements for retro titles, showing sets and progress. | ACH, INT | `SR-ACH-05`–`SR-ACH-08`, `SR-INT-16`–`SR-INT-22` (RA adapter) | `UC-ACH-02` View retro sets, `UC-INT-03` Link RA profile | `TS-ACH-02` (retro sets), `TS-INT-02` (RA integration) | `NFR-REL-01` (handling outages) | **PC** | Core flows covered; some RA-specific edge cases (rare set types) left as future tests. |
| **BR-ACH-03** | Allow curators to create and maintain fan-made achievement sets with evidence support. | ACH, PRO, GOV | `SR-ACH-09`–`SR-ACH-18` (fan sets, evidence), `SR-GOV-07`–`SR-GOV-11` (moderation of sets) | `UC-ACH-03` Create set, `UC-ACH-04` Edit set, `UC-ACH-05` Mark complete with evidence | `TS-ACH-03` (set authoring), `TS-GOV-02` (set moderation), `TS-INT-04` (evidence allowlist) | `NFR-USAB-01`, `NFR-SEC-03` | **PC** | Main flows covered; complex evidence disputes and bulk-edit operations not in MVP. |
| **BR-ALB-01** | Provide screenshot-based sticker albums with curator-defined templates. | ALB, CAT | `SR-ALB-01`–`SR-ALB-06` (templates, slots), `SR-CAT-09` (album link to game) | `UC-ALB-01` Browse albums, `UC-ALB-02` View album | `TS-ALB-01` (templates), `TS-CAT-02` (game–album link) | `NFR-USAB-02` (visual clarity) | **FC** | Template definition and basic browsing fully covered. |
| **BR-ALB-02** | Support “new-player-friendly” albums with spoiler-safe hints and gradual reveal. | ALB | `SR-ALB-07`–`SR-ALB-12` (spoiler hints & visibility), `SR-ALB-15` (progressive reveal) | `UC-ALB-03` Use spoiler-safe album | `TS-ALB-02` (spoiler hints), `TS-ALB-03` (slot guidance) | `NFR-LOC-01` (EN/UA text clarity) | **PC** | Key flows tested; some nuanced combinations of hints vs hardcore albums noted for future refinement. |
| **BR-ALB-03** | Award badges and reflect album completion in user profiles. | ALB, PRO | `SR-ALB-16`–`SR-ALB-18` (completion), `SR-PRO-05`–`SR-PRO-06` (badges & stats) | `UC-ALB-04` Complete album, `UC-PRO-02` View badges | `TS-ALB-04` (completion events), `TS-PRO-02` (profile stats) | `NFR-DATA-01` (consistency) | **FC** | Completion → profile pipeline tested for representative albums and users. |
| **BR-PRO-01** | Provide a unified profile view showing saves, achievements, albums, and badges. | PRO, SAV, ACH, ALB | `SR-PRO-01`–`SR-PRO-06`, `SR-SAV-03`, `SR-ACH-04`, `SR-ALB-16` | `UC-PRO-01` View own profile, `UC-PRO-03` View another user’s profile | `TS-PRO-01` (profile basics), `TS-PRO-02` (stats), plus cross-checks in SAV/ACH/ALB suites | `NFR-PERF-03`, `NFR-PRIV-01` | **PC** | Primary profile views covered; long-history performance and some privacy toggles tested as P2. |
| **BR-PRO-02** | Give users basic control over profile visibility and public data. | PRO | `SR-PRO-07`–`SR-PRO-10` (privacy settings) | `UC-PRO-04` Manage privacy | `TS-PRO-03` (privacy) | `NFR-PRIV-01` | **PC** | Main toggles tested; more granular future controls considered post-MVP. |
| **BR-GOV-01** | Provide reporting and moderation workflows for saves, achievements, and albums. | GOV, SAV, ACH, ALB | `SR-GOV-01`–`SR-GOV-08`, `SR-SAV-19` (report save), `SR-ACH-19`, `SR-ALB-19` | `UC-GOV-02` Handle reports, `UC-SAV-06` Report save, `UC-ACH-06` Report set | `TS-GOV-01` (queue), `TS-GOV-02` (decisions), plus report tests in SAV/ACH/ALB suites | `NFR-SEC-04` (abuse control) | **PC** | Core moderation flows covered; bulk operations and detailed audit exports not included for MVP. |
| **BR-GOV-02** | Enforce UA/RU-specific policy rules (warnings, exclusions, messaging). | GOV, CAT, DSC | `SR-GOV-12`–`SR-GOV-15` (policy rules), `SR-CAT-20`, `SR-DSC-07`–`SR-DSC-08` (filtered views) | `UC-CAT-04` RU title view, `UC-DSC-03` UA-filtered discovery | `TS-CAT-03`, `TS-DSC-03`, `TS-GOV-03` | `NFR-LOC-02` | **PC** | Representative scenarios tested for UA locale; other locales treated as neutral. |
| **BR-DSC-01** | Provide discovery and search across games, saves, achievements, and albums. | DSC, CAT, SAV, ACH, ALB | `SR-DSC-01`–`SR-DSC-05` | `UC-DSC-01` Global search, `UC-DSC-02` Filter by content type | `TS-DSC-01` (search), `TS-DSC-02` (filters) | `NFR-PERF-01` | **FC** | Core discovery paths tested end-to-end; advanced ranking tweaks handled as later tuning. |
| **BR-DSC-02** | Provide simple recommendations and highlights based on usage and curation. | DSC, PRO, ALB, ACH | `SR-DSC-06`–`SR-DSC-09`, `SR-PRO-08` (activity signals) | `UC-DSC-03` View recommended content | `TS-DSC-03` (recommendations), plus spot checks in PRO/ALB/ACH suites | `NFR-USAB-03` | **PC** | Basic recommendation surfaces tested; heuristics expected to evolve after MVP. |
| **BR-INT-01** | Integrate with Steam for official achievement status and minimal game metadata. | INT, ACH, CAT | `SR-INT-10`–`SR-INT-15`, `SR-ACH-01`–`SR-ACH-04`, `SR-CAT-05` | `UC-INT-02` Link Steam, `UC-ACH-01` View official achievements | `TS-INT-01`, `TS-ACH-01` | `NFR-REL-01` | **PC** | Main flows tested; Steam-specific edge cases beyond this subset tracked as known risk. |
| **BR-INT-02** | Integrate RetroAchievements for retro sets and progress. | INT, ACH | `SR-INT-16`–`SR-INT-22`, `SR-ACH-05`–`SR-ACH-08` | `UC-INT-03`, `UC-ACH-02` | `TS-INT-02`, `TS-ACH-02` | `NFR-REL-01` | **PC** | Similar to BR-ACH-02; adapter behaviour and mapping validated on representative titles. |
| **BR-INT-03** | Use external metadata sources (including UA-focused) for game info and flags. | INT, CAT | `SR-INT-03`–`SR-INT-09`, `SR-CAT-16`–`SR-CAT-19` | `UC-INT-01` Refresh metadata, `UC-CAT-03` View UA flags | `TS-INT-03`, `TS-CAT-02` | `NFR-DATA-02` | **PC** | Metadata merging and UA flags tested; wide-scale metadata sync left to controlled tasks. |
| **BR-NF-01** | Support English and Ukrainian UI, with UA-specific policy messaging. | ACC, CAT, DSC, ALB, ACH, PRO, GOV | `SR-ACC-09` (locale switch), `SR-CAT-16`–`SR-CAT-20`, `SR-DSC-07`, `SR-ALB-07`, `SR-GOV-12` | `UC-ACC-05` Change language, plus localisation aspects in other UCs | Localisation checks within `TS-ACC-01`, `TS-CAT-02`, `TS-DSC-03`, `TS-ALB-02`, `TS-GOV-03` | `NFR-LOC-01`, `NFR-LOC-02` | **PC** | EN/UA tested on core journeys; full translation of every sub-screen deferred as incremental work. |
| **BR-NF-02** | Ensure basic privacy, security and data protection for user accounts and content. | ACC, PRO, SAV, ACH, ALB, GOV | `SR-ACC-10`, `SR-PRO-07`, `SR-SAV-19`, `SR-ACH-19`, `SR-ALB-19`, `SR-GOV-05` | Security/privacy expectations embedded in many UCs | Security-oriented checks in `TS-ACC-01`, `TS-SAV-01`, `TS-ACH-03`, `TS-ALB-01`, `TS-GOV-01` | `NFR-SEC-01`, `NFR-PRIV-01` | **PC** | QA-level security checks in place; deeper security review explicitly outside MVP scope. |

---
<br>

### 4.2 Notes

- The table above is an **excerpt** showing the structure and mapping for key MVP business requirements.
- Additional BRs from the BRD can be added in the same format, especially if further features are brought into scope later.
- Coverage codes (`FC`, `PC`, `NC`, `NA`) should be periodically reviewed against:
  - updates to the Test Case Specification,
  - new UAT findings,
  - and changes captured in the Risks & Assumptions Register.

---
<br>
<br>

## 5. System Requirements Traceability (SRS → UC / Tests)

This section starts from **System Requirements (`SR-…`)** defined in the SRS and shows how each one is:

- linked back to **Business Requirements (`BR-…`)**,
- realised in **Use Cases (`UC-…`)**, and
- validated by **Test Suites / Test Cases (`TS-…` / `TC-…`)**.

The tables below are **structured excerpts** for key MVP SRs across all modules. The same pattern can be extended to the full SR list from the SRS.

---
<br>

### 5.1 Accounts & Authentication (ACC)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-ACC-01** | Support user registration with email/password and basic profile setup. | `BR-ACC-01` | `UC-ACC-01` Sign up | `TS-ACC-01` (core auth), e.g. `TC-ACC-001`–`TC-ACC-004` | **FC** | Registration happy path, invalid inputs, duplicate email, and basic validation covered. |
| **SR-ACC-02** | Support user sign-in with secure session handling. | `BR-ACC-01`, `BR-NF-02` | `UC-ACC-02` Sign in | `TS-ACC-01`, e.g. `TC-ACC-005`–`TC-ACC-008` | **FC** | Includes locked account behaviour and basic rate-limiting checks. |
| **SR-ACC-05** | Provide password reset via email link with expiry. | `BR-ACC-01` | `UC-ACC-03` Password reset | `TS-ACC-01`, e.g. `TC-ACC-009`–`TC-ACC-010` | **PC** | Core flow tested; corner timing cases (expired link edge windows) left as lower priority. |
| **SR-ACC-06** | Maintain role-based access: Regular, Curator, Admin. | `BR-ACC-02` | `UC-ACC-04` Assign roles, `UC-GOV-01` Access moderation tools | `TS-ACC-02` (roles), `TS-GOV-01` (access control) | **PC** | Main role boundaries covered; rare admin-only tools tested at smoke level only. |
| **SR-ACC-09** | Allow users to switch UI language between EN/UA. | `BR-NF-01` | `UC-ACC-05` Change language | Localisation checks in `TS-ACC-01`, `TS-CAT-02`, `TS-DSC-03` | **PC** | Core pages verified in EN/UA; some secondary views may be EN-only initially. |

---
<br>

### 5.2 Game Catalogue & Localisation Flags (CAT)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-CAT-01** | Provide browsable game catalogue with paging and basic filters. | `BR-CAT-01`, `BR-DSC-01` | `UC-CAT-01` Browse catalogue, `UC-DSC-01` Global search | `TS-CAT-01`, `TS-DSC-01` | **FC** | Listing, paging, basic title/platform filters fully covered. |
| **SR-CAT-03** | Show game details including platforms/variants, description, and links to saves/achievements/albums. | `BR-CAT-01`, `BR-SAV-01`, `BR-ACH-01`, `BR-ALB-01` | `UC-CAT-02` View game details | `TS-CAT-01`, cross-checks in `TS-SAV-01`, `TS-ACH-01`, `TS-ALB-01` | **PC** | Core details and links tested; some edge metadata fields not yet part of tests. |
| **SR-CAT-16** | Flag games with Ukrainian localisation available. | `BR-CAT-02`, `BR-NF-01` | `UC-CAT-03` View localisation flags | `TS-CAT-02`, `TS-INT-03` | **PC** | Representative titles checked; bulk correctness of all flags depends on external metadata quality. |
| **SR-CAT-18** | Flag games developed by Ukrainian studios. | `BR-CAT-02` | `UC-CAT-03` | `TS-CAT-02`, `TS-INT-03` | **PC** | Same as above; tested on curated examples, not all catalogue entries. |
| **SR-CAT-20** | Mark RU-linked titles and display warnings for UA users. | `BR-CAT-03`, `BR-GOV-02`, `BR-NF-01` | `UC-CAT-04` View RU-linked game | `TS-CAT-03`, `TS-GOV-03` | **PC** | Policy banners verified for a sample of RU-linked cases; corner cases (mixed publishers) noted as risk. |

---
<br>

### 5.3 Save Management (SAV)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-SAV-01** | Allow users to upload save files for supported games with basic validation. | `BR-SAV-01` | `UC-SAV-01` Upload save | `TS-SAV-01`, e.g. `TC-SAV-001`–`TC-SAV-006` | **FC** | Valid and invalid files, size limits, and basic metadata capture tested. |
| **SR-SAV-03** | Allow users to download saves with clear instructions where they should be placed. | `BR-SAV-01` | `UC-SAV-02` Download save | `TS-SAV-02` | **FC** | Tested for PC saves and emulator cases; console-only titles clearly marked as not supported. |
| **SR-SAV-07** | Store and expose save metadata (chapter/level, playtime, difficulty, notes). | `BR-SAV-02`, `BR-PRO-01` | `UC-SAV-01`, `UC-SAV-03`, `UC-PRO-01` | `TS-SAV-01`, `TS-SAV-03`, `TS-PRO-02` | **PC** | Typical metadata covered; some optional fields treated as free-form notes. |
| **SR-SAV-11** | Track compatibility scope for each save (game variant, platform, region). | `BR-SAV-03` | `UC-SAV-04` View compatibility | `TS-SAV-04`, `TS-INT-01` | **PC** | Steam build/manifest and retro region flags tested; community confirmations not yet automated. |
| **SR-SAV-13** | Indicate legacy/incompatible saves and warn users before download. | `BR-SAV-03` | `UC-SAV-05` Archive legacy saves | `TS-SAV-04` | **PC** | Warnings covered; auto-archival policies tested on a small, targeted dataset. |

---
<br>

### 5.4 Achievement Tracking (ACH)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-ACH-01** | Display official achievement sets for supported Steam titles. | `BR-ACH-01`, `BR-INT-01` | `UC-ACH-01` View official achievements | `TS-ACH-01`, `TS-INT-01` | **FC** | Representative titles tested end-to-end, including locked/unlocked states. |
| **SR-ACH-05** | Display RetroAchievements sets and progress for linked profiles. | `BR-ACH-02`, `BR-INT-02` | `UC-ACH-02` View retro sets, `UC-INT-03` Link RA profile | `TS-ACH-02`, `TS-INT-02` | **PC** | Base mapping and progress states tested; less common RA features treated as future scope. |
| **SR-ACH-09** | Allow curators to create fan-made achievement sets for a game. | `BR-ACH-03` | `UC-ACH-03` Create fan set | `TS-ACH-03` | **PC** | Creation, editing and publishing tested; advanced templates and mass changes not in MVP. |
| **SR-ACH-12** | Allow users to submit evidence (screenshots, clips, links) for fan-set completion. | `BR-ACH-03` | `UC-ACH-05` Mark complete with evidence | `TS-ACH-03`, `TS-INT-04`, `TS-GOV-02` | **PC** | Evidence submission and basic review tested; complex disputes and appeals handled at policy level only. |
| **SR-ACH-15** | Show completion statistics and timelines for achievement sets. | `BR-ACH-01`, `BR-ACH-03`, `BR-PRO-01` | `UC-ACH-01`, `UC-PRO-02` | `TS-ACH-02`, `TS-PRO-02` | **PC** | Representative stats verified; large-scale aggregation/performance covered by NFR tests. |

---
<br>

### 5.5 Sticker Albums (ALB)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-ALB-01** | Allow curators to define album templates with slots tied to game moments or themes. | `BR-ALB-01` | `UC-ALB-01` Browse albums, `UC-ALB-02` View album | `TS-ALB-01` | **FC** | Template creation and basic browsing fully covered. |
| **SR-ALB-07** | Provide spoiler-safe hints for “new-player-friendly” albums. | `BR-ALB-02` | `UC-ALB-03` Use spoiler-safe album | `TS-ALB-02`, `TS-ALB-03` | **PC** | Hints and gradual reveal tested on selected albums; UX fine-tuning via UAT. |
| **SR-ALB-10** | Show the next slot(s) in advance so users can anticipate screenshot moments without heavy spoilers. | `BR-ALB-02` | `UC-ALB-03` | `TS-ALB-03` | **PC** | Behaviour validated for standard story-driven albums; complex branching albums future work. |
| **SR-ALB-16** | Mark albums as complete when all required slots are filled with validated screenshots. | `BR-ALB-03` | `UC-ALB-04` Complete album | `TS-ALB-04` | **FC** | Completion status, badge awarding and profile updates all tested. |
| **SR-ALB-19** | Allow reporting of inappropriate screenshots in albums. | `BR-GOV-01`, `BR-NF-02` | `UC-ALB-05` Report sticker | `TS-ALB-05`, `TS-GOV-01` | **PC** | Reporting paths covered; bulk removal and complex appeals kept out of MVP. |

---
<br>

### 5.6 Profiles & Progress (PRO)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-PRO-01** | Provide a profile page summarising user activity (saves, achievements, albums). | `BR-PRO-01` | `UC-PRO-01` View own profile | `TS-PRO-01`, `TS-PRO-02`, plus cross-checks in SAV/ACH/ALB suites | **PC** | Main view covered; very large histories tested as part of NFR/performance checks. |
| **SR-PRO-05** | Show badges earned from album completions and special events. | `BR-ALB-03`, `BR-PRO-01` | `UC-PRO-02` View badges | `TS-PRO-02`, `TS-ALB-04` | **FC** | Badge pipeline from album → profile verified end-to-end. |
| **SR-PRO-07** | Allow users to control visibility of profile information (e.g., achievements, saves). | `BR-PRO-02`, `BR-NF-02` | `UC-PRO-04` Manage privacy | `TS-PRO-03` | **PC** | Core toggles tested; finer-grained controls deferred to post-MVP phase. |
| **SR-PRO-09** | Anonymise or hide sensitive identifiers when profiles are public. | `BR-NF-02` | `UC-PRO-03` View another user’s profile | `TS-PRO-03`, plus checks in GOV-related tests | **PC** | Public views verified not to expose raw emails or external IDs. |

---
<br>

### 5.7 Moderation & Governance (GOV)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-GOV-01** | Provide a moderation queue for reported saves, achievements, and albums. | `BR-GOV-01` | `UC-GOV-02` Handle reports | `TS-GOV-01`, `TS-GOV-02` | **PC** | Queue and decision flows tested; performance on very large queues treated as future concern. |
| **SR-GOV-05** | Log moderation actions for audit (who changed what, when). | `BR-GOV-01`, `BR-NF-02` | `UC-GOV-02` | `TS-GOV-02` | **PC** | Spot-checked logs; full audit export/reports not in MVP. |
| **SR-GOV-07** | Allow moderators to approve/reject evidence for fan-made achievements. | `BR-ACH-03`, `BR-GOV-01` | `UC-ACH-05`, `UC-GOV-02` | `TS-ACH-03`, `TS-GOV-02` | **PC** | Basic decisions tested; complex multi-moderator workflows future work. |
| **SR-GOV-12** | Enforce UA/RU policy banners on RU-linked content for UA users. | `BR-GOV-02`, `BR-CAT-03`, `BR-NF-01` | `UC-CAT-04`, `UC-DSC-03` | `TS-CAT-03`, `TS-DSC-03`, `TS-GOV-03` | **PC** | Representative scenarios covered; other locales treated as neutral. |

---
<br>

### 5.8 Discovery & Search (DSC)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-DSC-01** | Provide a single global search entry point across games and content. | `BR-DSC-01`, `BR-CAT-01` | `UC-DSC-01` Global search | `TS-DSC-01` | **FC** | Search across games, saves, achievements, albums tested on representative data sets. |
| **SR-DSC-03** | Support filters/facets by content type, platform, localisation, and UA-specific attributes. | `BR-DSC-01`, `BR-CAT-02` | `UC-DSC-02` Filter results | `TS-DSC-02`, `TS-CAT-02` | **PC** | Main filters covered; highly combined filter sets treated as lower priority. |
| **SR-DSC-06** | Show recommended games/content based on user activity and curated signals. | `BR-DSC-02` | `UC-DSC-03` View recommendations | `TS-DSC-03`, some checks in PRO/ALB/ACH suites | **PC** | Basic recommendation blocks tested; ranking heuristics expected to evolve later. |
| **SR-DSC-07** | Respect UA/RU policy when showing search and discovery results. | `BR-GOV-02`, `BR-NF-01` | `UC-DSC-03` | `TS-DSC-03`, `TS-GOV-03` | **PC** | Hide/flag rules validated on sample data; full catalogue correctness dependent on metadata integrity. |

---
<br>

### 5.9 External Integrations & Adapters (INT)

| SR ID | SR Summary | BR Link(s) | UC Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|------------|------------|------------|------------------|----------|-------|
| **SR-INT-01** | Fetch and map Steam build/manifest IDs for compatibility labelling. | `BR-SAV-03`, `BR-INT-01` | `UC-INT-02` Link Steam, `UC-SAV-04` View compatibility | `TS-INT-01`, `TS-SAV-04` | **PC** | Verified on a limited number of titles; broader mapping covered by adapter design, not exhaustive tests. |
| **SR-INT-03** | Pull game metadata (including UA-specific flags) from external providers. | `BR-INT-03`, `BR-CAT-02` | `UC-INT-01` Refresh metadata | `TS-INT-03`, `TS-CAT-02` | **PC** | Merging and conflict resolution tested on curated examples. |
| **SR-INT-16** | Synchronise RetroAchievements sets and progress for linked profiles. | `BR-ACH-02`, `BR-INT-02` | `UC-INT-03`, `UC-ACH-02` | `TS-INT-02`, `TS-ACH-02` | **PC** | Representative profiles tested; outage handling covered via NFR/INT tests. |
| **SR-INT-20** | Validate external evidence URLs against allowlisted hosts. | `BR-ACH-03`, `BR-GOV-01`, `BR-NF-02` | `UC-ACH-05` Mark complete with evidence | `TS-INT-04`, `TS-GOV-02` | **PC** | Allowlist behaviour verified; host list maintenance and rare host patterns to be refined over time. |

---
<br>

### 5.10 Notes

- The rows above are **representative** for each module.  
  To complete the RTM, each remaining `SR-…` from the SRS can be added using the same column structure.
- Coverage statuses (`FC`, `PC`, `NC`, `NA`) should be synchronised with:
  - the latest Test Case Specification,
  - executed test reports,
  - and decisions recorded in the Risks & Assumptions Register.

---
<br>
<br>

## 6. Use Case Traceability (UC → Requirements / Tests)

This section starts from **Use Cases (`UC-…`)** and shows how each one is:

- backed by **Business Requirements (`BR-…`)**,
- implemented via **System Requirements (`SR-…`)**, and
- validated by **Test Suites / Test Cases (`TS-…` / `TC-…`)**.

It is a **flow-centric view**: if someone wants to know “where is this user journey specified and how is it tested?”, this is the place to look.

> The tables below are representative for key MVP use cases per module.  
> Additional UCs from the Use Case Specification can be added in the same pattern.

---
<br>

### 6.1 Accounts & Authentication (ACC)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-ACC-01** | Sign up to Dream Project | Anonymous User | `BR-ACC-01` | `SR-ACC-01`, `SR-ACC-02` | `TS-ACC-01` (registration/auth), e.g. `TC-ACC-001`–`TC-ACC-004` | **FC** | Covers happy path and basic validation; negative edge cases (rare email formats) are P2. |
| **UC-ACC-02** | Sign in to existing account | Regular User | `BR-ACC-01`, `BR-NF-02` | `SR-ACC-02`, `SR-ACC-10` | `TS-ACC-01`, e.g. `TC-ACC-005`–`TC-ACC-008` | **FC** | Includes locked account handling and generic error messages. |
| **UC-ACC-03** | Reset forgotten password | Regular User | `BR-ACC-01` | `SR-ACC-05` | `TS-ACC-01`, e.g. `TC-ACC-009`–`TC-ACC-010` | **PC** | Main flow covered; edge timing windows for expired links not deeply tested. |
| **UC-ACC-04** | Assign or change user role (Curator/Admin) | Admin | `BR-ACC-02` | `SR-ACC-06`, `SR-PRO-03` | `TS-ACC-02` (roles), `TS-GOV-01` (access checks) | **PC** | Core transitions tested (Regular → Curator, Regular → Admin); bulk changes and demotions are post-MVP. |
| **UC-ACC-05** | Change interface language (EN/UA) | Regular User | `BR-NF-01` | `SR-ACC-09` | Localisation checks in `TS-ACC-01`, `TS-CAT-02`, `TS-DSC-03` | **PC** | Main navigation and core pages validated in both languages; secondary pages may temporarily fall back to EN. |

---
<br>

### 6.2 Game Catalogue & Localisation Flags (CAT)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-CAT-01** | Browse game catalogue with basic filters | Regular User | `BR-CAT-01`, `BR-DSC-01` | `SR-CAT-01`, `SR-DSC-01` | `TS-CAT-01`, `TS-DSC-01` | **FC** | Browsing, paging, and simple filters (platform, genre) fully covered. |
| **UC-CAT-02** | View detailed game page | Regular User | `BR-CAT-01`, `BR-SAV-01`, `BR-ACH-01`, `BR-ALB-01` | `SR-CAT-03`, `SR-CAT-05`, `SR-CAT-09` | `TS-CAT-01`, cross-checks in `TS-SAV-01`, `TS-ACH-01`, `TS-ALB-01` | **PC** | Links to saves/achievements/albums tested; some less common metadata fields not yet in tests. |
| **UC-CAT-03** | See Ukrainian localisation / origin flags | Regular User | `BR-CAT-02`, `BR-NF-01` | `SR-CAT-16`, `SR-CAT-18`, `SR-INT-03`–`SR-INT-09` | `TS-CAT-02`, `TS-INT-03` | **PC** | Representative UA-localised and UA-developed titles checked against metadata. |
| **UC-CAT-04** | View RU-linked game with UA-specific warning | Ukrainian User | `BR-CAT-03`, `BR-GOV-02`, `BR-NF-01` | `SR-CAT-20`, `SR-GOV-12`–`SR-GOV-15`, `SR-DSC-07` | `TS-CAT-03`, `TS-DSC-03`, `TS-GOV-03` | **PC** | Core warning behaviour covered; nuanced publisher scenarios treated as a known risk. |

---
<br>

### 6.3 Save Management (SAV)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-SAV-01** | Upload a save file for a game | Regular User | `BR-SAV-01`, `BR-SAV-02` | `SR-SAV-01`, `SR-SAV-07` | `TS-SAV-01` | **FC** | Valid/invalid uploads, metadata capture, and basic error handling covered. |
| **UC-SAV-02** | Download and use a save file | Regular User | `BR-SAV-01`, `BR-SAV-03` | `SR-SAV-03`, `SR-SAV-11`–`SR-SAV-13` | `TS-SAV-02`, `TS-SAV-04` | **PC** | Clear instructions and compatibility labels tested; not all possible game/OS combinations explored. |
| **UC-SAV-03** | Search and filter saves | Regular User | `BR-SAV-02`, `BR-DSC-01` | `SR-SAV-07`–`SR-SAV-10`, `SR-DSC-03` | `TS-SAV-03`, `TS-DSC-02` | **PC** | Chapter/level, playtime, difficulty filters validated on sample data. |
| **UC-SAV-04** | Inspect save compatibility details | Regular User | `BR-SAV-03` | `SR-SAV-11`, `SR-INT-01` | `TS-SAV-04`, `TS-INT-01` | **PC** | Steam build/manifest and retro region flags verified; community confirmation paths future work. |
| **UC-SAV-05** | Archive or handle legacy saves | Regular User | `BR-SAV-03` | `SR-SAV-13`, `SR-SAV-15` | `TS-SAV-04` | **PC** | Legacy warnings and archival behaviours tested on small data set. |
| **UC-SAV-06** | Report a problematic save | Regular User | `BR-GOV-01`, `BR-NF-02` | `SR-SAV-19`, `SR-GOV-01`–`SR-GOV-03` | `TS-SAV-05`, `TS-GOV-01` | **PC** | Reporting path covered; multi-step dispute resolution sits in post-MVP policy work. |

---
<br>

### 6.4 Achievement Tracking (ACH)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-ACH-01** | View official achievement set for a modern title | Regular User | `BR-ACH-01`, `BR-INT-01` | `SR-ACH-01`–`SR-ACH-04`, `SR-INT-10`–`SR-INT-15` | `TS-ACH-01`, `TS-INT-01` | **FC** | Locked/unlocked states and basic stats covered for representative Steam titles. |
| **UC-ACH-02** | View RetroAchievements sets and progress | Retro-focused User | `BR-ACH-02`, `BR-INT-02` | `SR-ACH-05`–`SR-ACH-08`, `SR-INT-16`–`SR-INT-22` | `TS-ACH-02`, `TS-INT-02` | **PC** | Typical RA sets tested; rare RA-specific mechanics left as future enhancement. |
| **UC-ACH-03** | Create and publish a fan-made achievement set | Curator | `BR-ACH-03` | `SR-ACH-09`–`SR-ACH-11` | `TS-ACH-03` | **PC** | Creation/edit flows covered; bulk operations and version branching not in MVP. |
| **UC-ACH-04** | Edit or retire an existing fan-made set | Curator | `BR-ACH-03`, `BR-GOV-01` | `SR-ACH-10`, `SR-ACH-14`, `SR-GOV-07` | `TS-ACH-03`, `TS-GOV-02` | **PC** | Basic update/retire flows tested; multi-curator scenarios remain simplified. |
| **UC-ACH-05** | Mark achievement set as complete with evidence | Regular User, Curator, Moderator | `BR-ACH-03`, `BR-NF-02` | `SR-ACH-12`, `SR-ACH-15`, `SR-INT-20`, `SR-GOV-07` | `TS-ACH-03`, `TS-INT-04`, `TS-GOV-02` | **PC** | Evidence submission and allowlist logic tested; deep dispute resolution is policy-level only. |
| **UC-ACH-06** | Report an inappropriate or low-quality fan set | Regular User | `BR-GOV-01` | `SR-ACH-19`, `SR-GOV-01`–`SR-GOV-03` | `TS-ACH-04`, `TS-GOV-01` | **PC** | Reporting and queue entries covered; long-term curation strategy tracked in risk/roadmap. |

---
<br>

### 6.5 Sticker Albums (ALB)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-ALB-01** | Browse available sticker albums for a game | Regular User | `BR-ALB-01`, `BR-CAT-01` | `SR-ALB-01`, `SR-CAT-09` | `TS-ALB-01`, `TS-CAT-02` | **FC** | Album catalogue and game–album linking fully tested. |
| **UC-ALB-02** | View album structure and slots | Regular User | `BR-ALB-01` | `SR-ALB-01`–`SR-ALB-06` | `TS-ALB-01` | **FC** | Slot layout and descriptions covered for representative templates. |
| **UC-ALB-03** | Use a “new-player-friendly” spoiler-safe album | New Player | `BR-ALB-02`, `BR-NF-01` | `SR-ALB-07`–`SR-ALB-12`, `SR-ALB-15` | `TS-ALB-02`, `TS-ALB-03` | **PC** | Hints, gradual reveal and “don’t miss” guidance tested on sample albums; nuanced wording tuned via UAT. |
| **UC-ALB-04** | Complete an album and receive a badge | Regular User | `BR-ALB-03`, `BR-PRO-01` | `SR-ALB-16`–`SR-ALB-18`, `SR-PRO-05`–`SR-PRO-06` | `TS-ALB-04`, `TS-PRO-02` | **FC** | End-to-end completion → badge → profile stats pipeline verified. |
| **UC-ALB-05** | Report an inappropriate sticker / screenshot | Regular User | `BR-GOV-01`, `BR-NF-02` | `SR-ALB-19`, `SR-GOV-01`–`SR-GOV-03` | `TS-ALB-05`, `TS-GOV-01` | **PC** | Reporting path tested; complex edge cases handled through general moderation rules. |

---
<br>

### 6.6 Profiles & Progress (PRO)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-PRO-01** | View own profile | Regular User | `BR-PRO-01`, `BR-NF-02` | `SR-PRO-01`–`SR-PRO-06`, `SR-SAV-07`, `SR-ACH-15`, `SR-ALB-16` | `TS-PRO-01`, `TS-PRO-02`, plus cross-checks in SAV/ACH/ALB suites | **PC** | Main summary, recent activity and quick stats covered; extreme history sizes tested via NFR checks. |
| **UC-PRO-02** | View badge and completion history | Regular User | `BR-PRO-01`, `BR-ALB-03` | `SR-PRO-05`–`SR-PRO-06`, `SR-ALB-16` | `TS-PRO-02`, `TS-ALB-04` | **FC** | Shows completed albums and related badges; used as verification of ALB → PRO integration. |
| **UC-PRO-03** | View another user’s public profile | Regular User | `BR-PRO-01`, `BR-PRO-02`, `BR-NF-02` | `SR-PRO-03`, `SR-PRO-09` | `TS-PRO-03` | **PC** | Public view verified not to expose sensitive identifiers; some combinations depend on privacy settings. |
| **UC-PRO-04** | Manage profile visibility & privacy settings | Regular User | `BR-PRO-02`, `BR-NF-02` | `SR-PRO-07`–`SR-PRO-10` | `TS-PRO-03` | **PC** | Core toggles tested (e.g., hide achievements); future fine-grained options outside MVP. |

---
<br>

### 6.7 Moderation & Governance (GOV)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-GOV-01** | Access moderation tools & dashboards | Admin, Moderator | `BR-ACC-02`, `BR-GOV-01` | `SR-ACC-06`, `SR-GOV-01`–`SR-GOV-02` | `TS-GOV-01` | **PC** | Access control and basic dashboard visibility covered; detailed dashboard metrics are post-MVP. |
| **UC-GOV-02** | Process reported content (review, approve, remove) | Moderator | `BR-GOV-01`, `BR-NF-02` | `SR-GOV-01`–`SR-GOV-08`, `SR-SAV-19`, `SR-ACH-19`, `SR-ALB-19` | `TS-GOV-01`, `TS-GOV-02`, plus report tests in SAV/ACH/ALB | **PC** | End-to-end report → decision flow covered; extensive audit exports left for future. |
| **UC-GOV-03** | Configure UA/RU policy banners and behaviour | Admin | `BR-GOV-02`, `BR-CAT-03`, `BR-NF-01` | `SR-GOV-12`–`SR-GOV-15`, `SR-CAT-20`, `SR-DSC-07` | `TS-GOV-03`, `TS-CAT-03`, `TS-DSC-03` | **PC** | Policy rules tested for UA users; other locales treated as neutral for MVP. |

---
<br>

### 6.8 Discovery & Search (DSC)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-DSC-01** | Run a global search across Dream Project content | Regular User | `BR-DSC-01`, `BR-CAT-01` | `SR-DSC-01`, `SR-DSC-02` | `TS-DSC-01` | **FC** | Basic search behaviour and result structure validated for games and content. |
| **UC-DSC-02** | Filter and refine search results | Regular User | `BR-DSC-01`, `BR-CAT-02` | `SR-DSC-03`, `SR-CAT-16`–`SR-CAT-18` | `TS-DSC-02`, `TS-CAT-02` | **PC** | Typical filters (type, platform, UA flags) covered; complex combined filters are lower priority. |
| **UC-DSC-03** | View recommendations and policy-aware discovery views | Regular User, Ukrainian User | `BR-DSC-02`, `BR-GOV-02`, `BR-NF-01` | `SR-DSC-06`–`SR-DSC-09`, `SR-DSC-07`, `SR-GOV-12` | `TS-DSC-03`, `TS-GOV-03` | **PC** | Recommendation blocks and UA/RU filtering tested on representative examples; ranking quality expected to evolve. |

---
<br>

### 6.9 External Integrations & Adapters (INT)

| UC ID | Name / Summary | Actors | BR Link(s) | SR Link(s) | TS / TC Link(s) | Coverage | Notes |
|-------|----------------|--------|-----------|------------|------------------|----------|-------|
| **UC-INT-01** | Refresh external metadata for one or more games | Admin | `BR-INT-03`, `BR-CAT-02` | `SR-INT-03`–`SR-INT-09` | `TS-INT-03`, `TS-CAT-02` | **PC** | Manual metadata refresh flows tested; full automated scheduling handled at project/ops level. |
| **UC-INT-02** | Link a Steam account and sync achievements | Regular User | `BR-INT-01`, `BR-ACH-01` | `SR-INT-10`–`SR-INT-15`, `SR-ACH-01`–`SR-ACH-04` | `TS-INT-01`, `TS-ACH-01` | **PC** | Linking, successful sync and basic error handling covered; rare API edge cases treated as risk. |
| **UC-INT-03** | Link a RetroAchievements profile and sync sets/progress | Retro-focused User | `BR-INT-02`, `BR-ACH-02` | `SR-INT-16`–`SR-INT-22`, `SR-ACH-05`–`SR-ACH-08` | `TS-INT-02`, `TS-ACH-02` | **PC** | Representative RA profiles tested; complex RA-specific outages and throttling partly simulated. |

---
<br>

### 6.10 Notes

- This section focuses on **core MVP flows** that are most representative and most critical for Dream Project.
- For a fully exhaustive RTM, every `UC-…` from the Use Case Specification can be added here with the same column structure.
- Coverage statuses in this view should remain in sync with:
  - Business and System views (Sections 4 and 5),
  - the Test Case Specification,
  - and test execution reports.

---
<br>
<br>

## 7. Non-Functional Requirements Mapping

This section links **non-functional requirements (NFRs)** to:

- the **areas of the system** they affect (BR/SR, modules),
- the **use cases** where they are most visible,
- the **tests and evidence** that validate them (functional tests, NFR-specific checks, UAT, monitoring).

It is not a full performance/security design; it is a **map** of “where we prove” that key qualities are at least minimally met for the MVP.

---
<br>

### 7.1 Performance & Availability

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-PERF-01** | Search, catalogue, and save filtering must feel responsive for typical loads. | `BR-CAT-01`, `BR-SAV-02`, `BR-DSC-01`; `SR-CAT-01`, `SR-DSC-01`–`SR-DSC-03`, `SR-SAV-07`–`SR-SAV-10` | `UC-CAT-01` Browse catalogue, `UC-DSC-01` Global search, `UC-SAV-03` Search saves | Functional checks in `TS-CAT-01`, `TS-DSC-01`, `TS-SAV-03` plus simple performance observations (response times recorded in test notes). | **PC** | MVP relies on lightweight indexing and small data volumes; formal performance testing tools can be added later if needed. |
| **NFR-PERF-02** | Achievement views (official and retro) should load within acceptable time for typical game profiles. | `BR-ACH-01`, `BR-ACH-02`, `BR-INT-01`, `BR-INT-02`; `SR-ACH-01`–`SR-ACH-08`, `SR-INT-10`–`SR-INT-22` | `UC-ACH-01` View official achievements, `UC-ACH-02` View RA sets | `TS-ACH-01`, `TS-ACH-02`, `TS-INT-01`, `TS-INT-02` with response time observations for representative titles. | **PC** | Full load/performance scenarios on very large libraries are outside MVP; basic sanity checks in place. |
| **NFR-PERF-03** | Profile pages remain usable even for users with long histories. | `BR-PRO-01`; `SR-PRO-01`–`SR-PRO-06` | `UC-PRO-01` View own profile, `UC-PRO-02` View badges | `TS-PRO-01`, `TS-PRO-02` plus spot checks on synthetic “heavy” profiles. | **PC** | Intent is to avoid obviously slow pages; formal profiling and optimisation is future work. |
| **NFR-AVAIL-01** | Core flows (sign in, browse, basic search) should be available most of the time during MVP. | `BR-ACC-01`, `BR-CAT-01`, `BR-DSC-01`; ACC/CAT/DSC modules | `UC-ACC-02`, `UC-CAT-01`, `UC-DSC-01` | Smoke/sanity suites from `TS-ACC-01`, `TS-CAT-01`, `TS-DSC-01` used as basic availability checks in each build. | **PC** | No formal SLA; availability is validated via repeated smoke runs and environment stability checks. |

---
<br>

### 7.2 Security & Privacy

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-SEC-01** | Authentication and session handling must be secure at a basic level. | `BR-ACC-01`, `BR-NF-02`; `SR-ACC-02`, `SR-ACC-05`, `SR-ACC-10` | `UC-ACC-02` Sign in, `UC-ACC-03` Reset password | `TS-ACC-01`: invalid credentials, locked accounts, password reset link behaviour; basic checks on error messages (no sensitive leaks). | **PC** | Deep security review (pen tests, advanced hardening) outside MVP; this NFR sets a minimum bar. |
| **NFR-SEC-02** | Save files must be stored and served securely to avoid obvious misuse. | `BR-SAV-01`, `BR-SAV-03`; `SR-SAV-01`, `SR-SAV-03`, `SR-SAV-19` | `UC-SAV-01`, `UC-SAV-02`, `UC-SAV-06` | `TS-SAV-01`, `TS-SAV-02`, `TS-SAV-05`: checks for allowed MIME types, size limits, basic abuse scenarios. | **PC** | MVP focuses on clear error cases and simple constraints; malware scanning and advanced controls are future enhancements. |
| **NFR-SEC-03** | Evidence links and uploads for fan-made achievements must not encourage unsafe behaviour. | `BR-ACH-03`, `BR-GOV-01`, `BR-NF-02`; `SR-ACH-12`, `SR-INT-20`, `SR-GOV-07` | `UC-ACH-05` Mark complete with evidence, `UC-GOV-02` Process reports | `TS-ACH-03`, `TS-INT-04`, `TS-GOV-02`: allowlisted hosts, rejection of unsupported file types/URLs. | **PC** | Focuses on simple allowlist/banlist logic; full content inspection is not in scope. |
| **NFR-PRIV-01** | Public profiles and content must not expose sensitive identifiers. | `BR-PRO-02`, `BR-NF-02`; `SR-PRO-07`–`SR-PRO-10`, `SR-GOV-05` | `UC-PRO-03` View another user’s profile | `TS-PRO-03`: checks for absence of raw emails/external IDs, verification of privacy toggles. | **PC** | MVP covers basic privacy; finer control (per-field visibility) can be added later. |

---
<br>

### 7.3 Localisation & UA/RU Policy Behaviour

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-LOC-01** | English and Ukrainian UI must be understandable and consistent across core flows. | `BR-NF-01`; ACC/CAT/DSC/ALB/ACH/PRO | `UC-ACC-05` Change language, plus localised views in `UC-CAT-01`, `UC-DSC-01`, `UC-ALB-03` | Localisation checks inside `TS-ACC-01`, `TS-CAT-02`, `TS-DSC-03`, `TS-ALB-02`. | **PC** | Not every label is translated in MVP; priority is on main user flows, with gaps logged for later completion. |
| **NFR-LOC-02** | UA/RU policy messaging (warnings, banners, flags) must be correct and only shown where appropriate. | `BR-CAT-03`, `BR-GOV-02`, `BR-NF-01`; `SR-CAT-20`, `SR-GOV-12`–`SR-GOV-15`, `SR-DSC-07` | `UC-CAT-04` View RU-linked game, `UC-DSC-03` Policy-aware discovery | `TS-CAT-03`, `TS-DSC-03`, `TS-GOV-03`: tests for UA locale vs other locales, RU-linked vs neutral games. | **PC** | Representative examples covered; full catalogue correctness depends on external metadata and is tracked as a risk. |

---
<br>

### 7.4 Usability & UX

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-USAB-01** | Compatibility and risk warnings for saves and achievements must be clearly communicated. | `BR-SAV-03`, `BR-ACH-03`; `SR-SAV-11`–`SR-SAV-13`, `SR-ACH-12`, `SR-GOV-07` | `UC-SAV-02`, `UC-SAV-04`, `UC-ACH-05` | `TS-SAV-04`, `TS-ACH-03`, `TS-GOV-02`: checks that warnings are visible, and final actions require confirmation. | **PC** | Focus on clear messages for legacy/incompatible saves and evidence restrictions. |
| **NFR-USAB-02** | Sticker albums should be easy to understand and visually readable. | `BR-ALB-01`, `BR-ALB-02`; `SR-ALB-01`–`SR-ALB-06` | `UC-ALB-01`, `UC-ALB-02`, `UC-ALB-03` | `TS-ALB-01`, `TS-ALB-02`, `TS-ALB-03`: layout, slot labels, hints. | **PC** | Verified via structured test cases and subjective UAT feedback; pixel-perfect design not in scope here. |
| **NFR-USAB-03** | Recommendations and discovery views must be simple enough for users to understand why items are shown. | `BR-DSC-02`; `SR-DSC-06`–`SR-DSC-09` | `UC-DSC-03` View recommendations | `TS-DSC-03` plus UAT notes on recommendations panels. | **PC** | MVP focuses on basic explanatory labels (e.g., “Because you followed X”); complex explainability is optional. |

---
<br>

### 7.5 Data Quality & Consistency

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-DATA-01** | Profile statistics, completion percentages and counts must be internally consistent. | `BR-PRO-01`, `BR-ALB-03`, `BR-ACH-01`–`BR-ACH-03`; `SR-PRO-05`–`SR-PRO-06`, `SR-ALB-16`, `SR-ACH-15` | `UC-PRO-01`, `UC-PRO-02`, `UC-ALB-04`, `UC-ACH-01` | Cross-checks in `TS-PRO-02`, `TS-ALB-04`, `TS-ACH-02`: verify totals against underlying items. | **PC** | MVP tests focus on obvious mismatches; full reconciliation rules for very complex histories can evolve later. |
| **NFR-DATA-02** | External metadata (platforms, UA flags, RU flags) must be merged without obvious contradictions. | `BR-CAT-01`–`BR-CAT-03`, `BR-INT-03`; `SR-INT-03`–`SR-INT-09`, `SR-CAT-16`–`SR-CAT-20` | `UC-INT-01` Refresh metadata, `UC-CAT-02`, `UC-CAT-03`, `UC-CAT-04` | `TS-INT-03`, `TS-CAT-02`, `TS-CAT-03`: checks on curated sample titles with known attributes. | **PC** | Catalogue-wide correctness depends on sources (IGDB/UA-focused sites); sample-based validation is all that is realistic for MVP. |

---
<br>

### 7.6 Reliability & Integrations

| NFR ID | Description | Affected Areas | UC Link(s) (examples) | Tests / Evidence | Coverage | Notes |
|--------|-------------|----------------|------------------------|------------------|----------|-------|
| **NFR-REL-01** | External services (Steam, RetroAchievements, metadata providers) should fail gracefully without breaking core UX. | `BR-INT-01`–`BR-INT-03`, `BR-ACH-02`; `SR-INT-10`–`SR-INT-22`, `SR-DSC-07` | `UC-INT-01`–`UC-INT-03`, `UC-ACH-02`, `UC-DSC-03` | Error-path checks in `TS-INT-01`, `TS-INT-02`, `TS-INT-03`, plus negative scenarios in `TS-ACH-02`, `TS-DSC-03`. | **PC** | Simulated outages and error responses covered; full resilience patterns and retries are not fully exercised in MVP. |

---
<br>

### 7.7 How to Use This Mapping

- When **planning NFR tests**, use this section to:
  - identify which **modules and use cases** are affected by a given NFR,
  - see which **test suites** already contain checks,
  - and decide where additional tests or monitoring are needed.

- When **assessing release readiness**, review:
  - which NFRs are currently **Partially Covered** (`PC`),
  - whether these partial coverages are acceptable for MVP,
  - and which items should be raised as risks in the **Risks & Assumptions Register**.

- When **changing architecture or integrations**, update:
  - the relevant NFR entries here,
  - the linked SR/UC/Test references,
  - and the evidence sources (e.g., new monitoring dashboards, new performance test scripts).

This keeps non-functional behaviour visible in the same way as functional requirements, instead of being an undocumented assumption.

---
<br>
<br>

## 8. Risks, Assumptions & Test Evidence Links

This section connects **project-level risks and assumptions** from the *Risks & Assumptions Register* to:

- the **requirements** and **use cases** they affect, and  
- the **tests and evidence** that help mitigate or validate them.

The goal is not to duplicate the entire register, but to show **where the RTM provides concrete proof** that some risks are being addressed and that assumptions are being checked.

---
<br>

### 8.1 Risk–Requirement–Test Mapping (Examples)

The table below lists **representative MVP risks** and links them to BR/SR/UC/Test items that provide partial mitigation or early detection.

| Risk ID | Risk Summary | Impacted BR / SR / NFR (examples) | UC / Flow Impact (examples) | Tests / Evidence (examples) | RTM Role |
|---------|--------------|------------------------------------|-----------------------------|-----------------------------|----------|
| **RISK-01** | Low adoption / limited engagement with Dream Project MVP. | `BR-PRO-01`, `BR-DSC-02`, `BR-ALB-01`–`BR-ALB-03`; related SRs in PRO/DSC/ALB | `UC-PRO-01` View profile, `UC-DSC-03` View recommendations, `UC-ALB-01`–`UC-ALB-04` | Functional & UAT scenarios in `TS-PRO-01`, `TS-DSC-03`, `TS-ALB-01`–`TS-ALB-04` verify that “hook” features actually work end-to-end. | RTM shows that core engagement features are implemented and testable; it does **not** guarantee user adoption, but it ensures the basic experience is coherent. |
| **RISK-02** | Misconfiguration or poor quality of UA/RU flags and warnings. | `BR-CAT-02`, `BR-CAT-03`, `BR-GOV-02`, `BR-NF-01`; `SR-CAT-16`–`SR-CAT-20`, `SR-GOV-12`–`SR-GOV-15`, `SR-DSC-07`; `NFR-LOC-02` | `UC-CAT-03` UA flags, `UC-CAT-04` RU warnings, `UC-DSC-03` discovery with policy rules | `TS-CAT-02`, `TS-CAT-03`, `TS-DSC-03`, `TS-GOV-03` exercise representative UA/RU scenarios and verify banners/texts. | RTM highlights where UA/RU policy behaviour is specified and tested; catalogue-wide correctness still depends on external metadata and manual curation (called out in the Risk Register). |
| **RISK-03** | External integrations (Steam, RetroAchievements, metadata sources) are unstable or change behaviour. | `BR-INT-01`–`BR-INT-03`, `BR-ACH-02`, `BR-CAT-01`; `SR-INT-10`–`SR-INT-22`, `SR-DSC-07`; `NFR-REL-01` | `UC-INT-01`–`UC-INT-03`, `UC-ACH-02`, `UC-DSC-03` | Negative/timeout/error scenarios in `TS-INT-01`, `TS-INT-02`, `TS-INT-03`, plus error-path checks in `TS-ACH-02`, `TS-DSC-03`. | RTM makes it clear which requirements depend on external APIs and where fallback behaviour is tested, so impact of integration failures can be quickly assessed. |
| **RISK-04** | Inadequate moderation of user-generated content (saves, screenshots, fan sets, evidence). | `BR-GOV-01`, `BR-NF-02`, `BR-ACH-03`, `BR-SAV-01`, `BR-ALB-01`; `SR-GOV-01`–`SR-GOV-08`, `SR-SAV-19`, `SR-ACH-19`, `SR-ALB-19`; `NFR-SEC-03` | `UC-GOV-02` Process reports, `UC-SAV-06` Report save, `UC-ACH-06` Report set, `UC-ALB-05` Report sticker | Moderation and reporting flows in `TS-GOV-01`, `TS-GOV-02`, `TS-SAV-05`, `TS-ACH-04`, `TS-ALB-05`. | RTM shows that there are explicit flows and tests around reporting and decisions; residual risk around volume and nuance of cases remains in the Risk Register. |
| **RISK-05** | Data quality issues in game metadata, localisation flags, or UA/RU classification. | `BR-CAT-01`–`BR-CAT-03`, `BR-INT-03`; `SR-INT-03`–`SR-INT-09`, `SR-CAT-16`–`SR-CAT-20`; `NFR-DATA-02` | `UC-INT-01` Refresh metadata, `UC-CAT-02` Game details, `UC-CAT-03`/`UC-CAT-04` flags & warnings | Sample-based checks in `TS-INT-03`, `TS-CAT-02`, `TS-CAT-03` validate merging and flagging rules on curated examples. | RTM clarifies that tests verify **logic and merging rules** rather than every record; remaining catalogue-wide risk is explicitly tracked. |
| **RISK-06** | Security/privacy shortcomings (sensitive identifiers exposed, unsafe evidence links, misuse of saves). | `BR-NF-02`, `BR-ACH-03`, `BR-SAV-03`; `SR-ACC-10`, `SR-PRO-07`–`SR-PRO-10`, `SR-INT-20`, `SR-GOV-05`; `NFR-SEC-01`–`NFR-SEC-03`, `NFR-PRIV-01` | `UC-ACC-02`, `UC-ACC-03`, `UC-PRO-03`, `UC-ACH-05`, `UC-SAV-02`, `UC-SAV-06` | Security/privacy-relevant scenarios in `TS-ACC-01`, `TS-PRO-03`, `TS-ACH-03`, `TS-INT-04`, `TS-SAV-01`, `TS-SAV-05`. | RTM shows which tests explicitly check privacy and evidence constraints; more advanced security review is marked as outside MVP and remains an open risk item. |
| **RISK-07** | Schedule / scope creep threatens delivery of the defined MVP. | Many BRs in scope, particularly `BR-SAV-01`–`BR-SAV-03`, `BR-ACH-01`–`BR-ACH-03`, `BR-ALB-01`–`BR-ALB-03`, `BR-PRO-01` | All core UCs for SAV/ACH/ALB/PRO | RTM itself plus project-level checks in the Project Plan (scope vs coverage). | RTM helps identify which requirements are already backed by UCs and tests; anything not traced is a candidate for scope trimming or future iteration. |

> The full risk list remains in the *Risks & Assumptions Register*.  
> Here we only show where **traceability** and **test evidence** help to address or monitor those risks.

---
<br>

### 8.2 Assumptions–Validation Mapping (Examples)

Some key assumptions define how Dream Project MVP is expected to be used. This table shows where those assumptions are **anchored in requirements** and **touched by tests**.

| Assumption ID | Assumption Summary | Related BR / SR / NFR (examples) | UC / Flow Impact (examples) | Tests / Evidence (examples) | Validation Approach |
|---------------|--------------------|-----------------------------------|-----------------------------|-----------------------------|---------------------|
| **ASSUMP-01** | A small but active group of Curators will create and maintain fan-made achievement sets and sticker albums. | `BR-ACH-03`, `BR-ALB-01`–`BR-ALB-03`; `SR-ACH-09`–`SR-ACH-14`, `SR-ALB-01`–`SR-ALB-06` | `UC-ACH-03`/`UC-ACH-04` (fan sets), `UC-ALB-01`–`UC-ALB-03` (album templates) | Functional coverage in `TS-ACH-03`, `TS-ALB-01`–`TS-ALB-03`. | RTM confirms that the **tools** for curators exist and are tested; actual community uptake must be validated via usage metrics post-launch. |
| **ASSUMP-02** | Users will be willing to link their Steam / RetroAchievements profiles to get richer tracking. | `BR-INT-01`–`BR-INT-02`, `BR-ACH-01`–`BR-ACH-02`; `SR-INT-10`–`SR-INT-22`, `SR-ACH-01`–`SR-ACH-08` | `UC-INT-02` Link Steam, `UC-INT-03` Link RA profile, `UC-ACH-01`/`UC-ACH-02` | Link/sync flows in `TS-INT-01`, `TS-INT-02`, `TS-ACH-01`, `TS-ACH-02`. | RTM shows the integration flows are in place and verified; real user willingness is measured later via proportion of linked accounts. |
| **ASSUMP-03** | Users are comfortable sharing screenshots and saves on a dedicated platform, provided basic moderation and privacy controls exist. | `BR-SAV-01`, `BR-ALB-01`, `BR-PRO-02`, `BR-GOV-01`; `SR-SAV-01`, `SR-ALB-01`, `SR-PRO-07`–`SR-PRO-10`, `SR-GOV-01`–`SR-GOV-08`; `NFR-PRIV-01`, `NFR-SEC-02` | `UC-SAV-01`, `UC-ALB-02`, `UC-PRO-04`, `UC-GOV-02`, reporting UCs | Tests covering uploads, privacy toggles and moderation flows in `TS-SAV-01`, `TS-ALB-01`, `TS-PRO-03`, `TS-GOV-01`–`TS-GOV-02`. | RTM ensures the **infrastructure** for safe sharing exists; actual comfort level is validated via adoption, content volume, and feedback. |
| **ASSUMP-04** | External metadata sources will remain available and broadly reliable (within MVP timeframe). | `BR-INT-03`, `BR-CAT-02`–`BR-CAT-03`; `SR-INT-03`–`SR-INT-09`, `SR-CAT-16`–`SR-CAT-20`; `NFR-DATA-02`, `NFR-REL-01` | `UC-INT-01` Refresh metadata, `UC-CAT-02`/`UC-CAT-03`/`UC-CAT-04` | Integration and error tests in `TS-INT-03`, `TS-CAT-02`, `TS-CAT-03`. | RTM shows where behaviour is defined and tested; longer-term reliability is monitored via logs and operational checks, not just test cases. |
| **ASSUMP-05** | The EN/UA language split is sufficient for the initial user communities. | `BR-NF-01`, `BR-CAT-02`; `SR-ACC-09`, `SR-CAT-16`–`SR-CAT-19`; `NFR-LOC-01`–`NFR-LOC-02` | `UC-ACC-05`, localisation aspects of `UC-CAT-01`, `UC-CAT-03`, `UC-DSC-01` | Localisation checks in `TS-ACC-01`, `TS-CAT-02`, `TS-DSC-03`, `TS-ALB-02`. | RTM shows EN/UA flows are actually present and tested; demand for other languages is evaluated post-launch. |

---
<br>

### 8.3 How This Ties Back to the Register

- The **Risks & Assumptions Register** remains the **source of truth** for:
  - full descriptions,
  - scoring (probability/impact),
  - owners and mitigation/validation plans.

- The RTM adds:
  - a clear indication of **which requirements and flows** are affected by a given risk/assumption,
  - and where **test evidence** exists (or is missing) to support mitigation or validation.

When something changes in the register (e.g., a risk is closed, a new high-impact risk is added), this section should be revisited to:

1. Add or remove rows as needed.  
2. Update the linked BR/SR/UC/TC IDs.  
3. Mark any **gaps** where new tests or requirements are needed.

In this way, risk/assumption management and requirements/test management stay connected, instead of drifting apart into separate, incompatible views of the project.

---
<br>
<br>

## 9. Coverage Summary & Gaps

This section summarises **how well the MVP is covered** by:

- Business Requirements → System Requirements → Use Cases → Test Cases  
- across all modules (ACC, CAT, SAV, ACH, ALB, PRO, GOV, DSC, INT),

and highlights **where gaps remain** or where coverage is intentionally light for MVP.

---
<br>

### 9.1 Legend (Recap)

Coverage codes used below:

- **FC – Fully Covered**  
  Core flows are implemented, have explicit use cases, and are backed by at least one meaningful test suite.

- **PC – Partially Covered**  
  Main paths are covered, but:
  - not all variants/roles are tested, or  
  - non-functional aspects are thin, or  
  - some edge/error flows remain untested.

- **NC – Not Covered**  
  Requirement has no explicit test coverage yet (or is only indirectly touched).

- **NA – Not Applicable**  
  Out of MVP scope, or validated via higher-level review (e.g., business sign-off only).

---
<br>

### 9.2 Summary by Module (MVP View)

The table below gives a **high-level summary** by module. Numbers are indicative, based on the SRS / Use Case Spec / Test Case Spec mappings.

| Module | Scope (MVP focus) | BR / SR / UC Coverage | Test Coverage | Overall Status | Main Gaps / Notes |
|--------|-------------------|------------------------|---------------|----------------|-------------------|
| **ACC** (Accounts & Auth) | Registration, login, password reset, roles, language switch. | All core ACC-related BR/SR/UC items are linked in Sections 4–6. | `TS-ACC-01` and `TS-ACC-02` cover main flows and negative cases. | **Mostly FC / some PC** | Edge security scenarios (brute-force, device fingerprinting, advanced session hardening) are outside MVP; password-reset edge timing cases are lightly tested. |
| **CAT** (Catalogue) | Game listing, game details, platform/variant metadata, UA flags, RU warnings. | Catalogue BRs are well-mapped; UA/RU flags linked to INT metadata SRs. | `TS-CAT-01`–`TS-CAT-03` plus discovery tests. | **PC** | Logic for flags and warnings is covered on curated examples; completeness of metadata across the full catalogue remains a risk and depends on external sources. |
| **SAV** (Save Management) | Upload/download, metadata, compatibility/legacy marking, reporting. | All key save BRs (`BR-SAV-01`–`03`) traced to SRs and UCs. | `TS-SAV-01`–`TS-SAV-05` cover happy paths and main errors. | **Mostly FC / some PC** | Not all combinations of game/platform/region are tested; community confirmation flows for compatibility and extreme edge formats are post-MVP. |
| **ACH** (Achievements) | Official Steam sets, RA sets, fan-made sets with evidence, stats. | Core ACH BRs traced to SRs and INT adapter SRs. | `TS-ACH-01`–`TS-ACH-04` + INT tests cover main flows. | **PC** | Complex RA-specific features, rare set types, and detailed dispute workflows for evidence are not fully tested; considered future enhancements. |
| **ALB** (Sticker Albums) | Templates, spoiler-safe hints, gradual reveal, completion & badges, reporting. | Album BRs link clearly to SRs and UCs in the RTM. | `TS-ALB-01`–`TS-ALB-05` cover creation, use, completion, and reporting. | **Mostly FC / some PC** | New-player-friendly guidance and nuanced spoiler handling are partly validated via structured tests and will rely on UAT feedback for fine-tuning. |
| **PRO** (Profiles & Progress) | Profile overview, stats, badges, privacy controls. | Profile BRs map to SR-PRO and multiple cross-module SRs. | `TS-PRO-01`–`TS-PRO-03` plus cross-checks from SAV/ACH/ALB. | **PC** | Very long histories and fine-grained privacy rules are only partially tested; heavier performance and UX checks are deferred. |
| **GOV** (Moderation & Governance) | Reporting, queues, decisions, policy banners, audit logging. | Governance BRs linked to SR-GOV and to content modules. | `TS-GOV-01`–`TS-GOV-03` plus report tests in SAV/ACH/ALB. | **PC** | Volume/scale scenarios, complex appeals, and advanced audit/reporting features are not fully covered in MVP tests. |
| **DSC** (Discovery & Search) | Global search, filters, recommendations, policy-aware views. | Discovery BRs map to SR-DSC and multiple modules. | `TS-DSC-01`–`TS-DSC-03`. | **PC / some FC** | Core search and filters are covered; ranking quality and sophisticated recommendation explainability are intentionally light for MVP. |
| **INT** (Integrations & Adapters) | Steam, RetroAchievements, metadata sources, evidence allowlist. | Integration BRs traced to SR-INT and linked UCs. | `TS-INT-01`–`TS-INT-04` plus negative-path checks in other suites. | **PC** | Behaviour on rare API changes, throttling limits, and long-running outages is only partly simulated; operational monitoring will be important. |

---
<br>

### 9.3 Key Partial / Uncovered Areas (Cross-Cutting)

Across all modules, the following themes are **partially covered** or explicitly left for future work:

1. **Deep Security Hardening**
   - Basic checks exist for auth, evidence links, and save uploads.  
   - More advanced topics (rate-limiting strategies, penetration testing, full audit analytics) are **not** fully represented in tests and sit outside this MVP.

2. **Data Quality & Metadata Completeness**
   - RTM shows that merging logic and flag application are tested for *curated examples*.
   - Full correctness of:
     - UA localisation flags,
     - UA-developed / RU-linked classifications,
     - platform/variant metadata  
     depends on external sources and manual curation and cannot be fully guaranteed by tests alone.

3. **Performance Under Heavy Load**
   - Some synthetic checks are mentioned (profiles with many items, basic search responsiveness).
   - There is no full-scale load or stress test coverage; this is a deliberate scope limitation.

4. **Moderation Volume & Nuanced Cases**
   - Reporting and decision flows are covered.  
   - High-volume moderation, complex appeals, and nuanced content-judgement rules are handled at the policy/process level, not fully modelled in the RTM.

5. **Recommendation Quality**
   - The existence of recommendation blocks and their basic behaviour is tested.
   - Actual **quality** of suggested games/content, and how well it matches user expectations, is beyond automated test coverage and must be evaluated via usage data and feedback.

6. **Long-Tail Localisation**
   - EN/UA for core flows is tested.
   - Labels and text on secondary or rarely used screens may initially be EN-only or less polished; this is accepted for MVP with the expectation of iterative improvement.

---
<br>

### 9.4 How to Use This Summary

- When planning **additional tests** or **next iteration scope**, start from:
  - modules with mostly **PC** coverage,
  - and cross-cutting themes in 9.3.

- When assessing **release readiness** for MVP, use this section to:
  - confirm that all **critical flows** are at least **partially covered** (no important BR should remain `NC`),  
  - explicitly acknowledge the **known limitations** above, and  
  - ensure these limitations are reflected in the **Risk & Assumptions Register** and **Project Plan**.

- When the product evolves beyond MVP:
  - update coverage statuses as new tests are added,
  - add rows for new BR/SR/UC/TC items,
  - and gradually move high-impact areas from **PC → FC**, or reclassify out-of-scope items as **NA** with clear justification.

This keeps the RTM as a **living summary** of where Dream Project is genuinely supported by requirements, use cases, and tests—and where it is still relying on conscious acceptance of risk or future work.

---
<br>
<br>

## 10. Change Impact & Maintenance Rules

This section defines **how to keep the RTM accurate** when anything changes in:

- **Business requirements** (BRD)  
- **System requirements** (SRS)  
- **Use cases** (UC Spec)  
- **Tests** (Test Case Specification)  
- or **external constraints** (risks, integrations, localisation rules)

The idea is simple: *if something important changes, at least one row in the RTM must change with it*.

---
<br>

### 10.1 When a Business Requirement Changes (BRD → RTM)

**Typical triggers**

- New BR added (new feature or non-functional expectation).  
- Existing BR reworded or its scope/priorities changed.  
- BR explicitly moved out of MVP or dropped altogether.

**Actions in the RTM**

1. **Section 4 – BR Traceability**
   - Add / update / remove the row for the BR in **4.1 Traceability Table**.
   - Check:
     - **Module(s)** affected,
     - **Related SR IDs** (Section 5),
     - **Related UC IDs** (Section 6),
     - **Related TS/TC IDs** and **Coverage**.

2. **Sections 5 & 6 – SRS / UC mappings**
   - For every `SR-…` and `UC-…` that references this BR:
     - Update the **BR Link(s)** column.
     - If a BR is removed, decide whether the SR/UC is still valid:
       - If **yes** → link it to another BR (or create a new BR).
       - If **no** → mark the SR/UC for deprecation/removal and update the SRS / UC Spec accordingly.

3. **Coverage & Risk**
   - Revisit **Section 9 (Coverage Summary & Gaps)**:
     - If the BR adds new scope, coverage may temporarily become **NC** or **PC**.
     - If the BR tightens expectations (e.g. stronger privacy), update NFR mapping in **Section 7**.
   - Check the **Risks & Assumptions Register**:
     - New BRs that change scope/complexity should be reflected as new/updated risk items.

---
<br>

### 10.2 When a System Requirement Changes (SRS → RTM)

**Typical triggers**

- New `SR-…` added for a module (e.g. new filter, extra metadata field).  
- Behaviour of an existing SR is adjusted.  
- SR removed or postponed beyond MVP.

**Actions in the RTM**

1. **Section 5 – System Requirements Traceability**
   - Add / update / remove the SR row in the relevant module subsection (ACC, CAT, SAV, etc.).
   - Ensure **BR Link(s)** are still correct and that the SR has at least one related UC (unless it is internal-only).

2. **Section 4 – BR Traceability**
   - For the BR row(s) that reference this SR:
     - Update the **Related SR IDs** list.
     - If a BR loses its only SR link, this indicates a **coverage gap** and should be highlighted.

3. **Section 6 – Use Case Traceability**
   - For any `UC-…` that explicitly relies on this SR:
     - Update the **SR Link(s)** column.
     - If the SR introduces new steps or variants, consider whether the UC description and alternate flows need updates.

4. **Testing & Coverage**
   - Adjust the **TS/TC links** in Sections 5 and 6:
     - If new behaviour is added, create or extend test cases and link them.
   - Re-assess coverage in **Section 9**:
     - Mark newly added behaviour as **PC** or **NC** until tests exist and are executed.

---
<br>

### 10.3 When a Use Case Changes (UC Spec → RTM)

**Typical triggers**

- New user flow added (e.g. new way to discover content).  
- Existing UC significantly reworked (different actors, new alternate flows).  
- UC split into multiple UCs, or retired.

**Actions in the RTM**

1. **Section 6 – Use Case Traceability**
   - Add / update / remove the relevant `UC-…` row.
   - Ensure:
     - **BR Link(s)** reflect the latest business intent.
     - **SR Link(s)** list all requirements that implement the flow.
     - **TS/TC Link(s)** include tests that exercise the updated behaviour.

2. **Sections 4 & 5 – BR / SR mappings**
   - If the UC is the **primary realisation** of a BR or SR, update:
     - **Related UC IDs** in Section 4 (BR table).
     - **UC Link(s)** in Section 5 (SR tables).
   - If a UC is removed and no other UC covers those SRs, mark the gap and decide:
     - Either create a replacement UC,
     - Or change the SR (or BR) to reflect that behaviour is no longer required.

3. **Testing & Coverage**
   - When a UC changes:
     - Confirm that linked tests still describe the actual flow (inputs, expected results, errors).
     - If not, update or add tests and keep **TS/TC Link(s)** in sync.
   - Re-check **Coverage** fields for relevant rows in Sections 4–6 and **Section 9**.

---
<br>

### 10.4 When Test Suites or Test Cases Change (Tests → RTM)

**Typical triggers**

- New tests added for previously uncovered behaviour.  
- Existing test cases refactored, merged, or split.  
- Tests deprecated because a feature moved out of scope.

**Actions in the RTM**

1. **Update Test References**
   - In Sections **4, 5, 6, 7, 8**:
     - Keep `TS-…` / `TC-…` IDs up-to-date in the **Test / Evidence** columns.
     - When a suite is renamed or restructured, reflect that change everywhere it is referenced.

2. **Update Coverage Status**
   - If new tests now cover a previously **PC** or **NC** requirement:
     - Re-assess and update the **Coverage** column (e.g. `PC → FC`).
   - If tests are removed (e.g. deprecated feature):
     - Re-check whether the corresponding requirement is still in scope; if it is, it may drop to **PC** or **NC**.

3. **Link to Evidence**
   - If you maintain test reports, CI pipelines, or UAT checklists outside this RTM:
     - Optionally add a short note in **Notes** columns (e.g. “Covered in smoke suite S1”, “See regression pack R2”).

---
<br>

### 10.5 When External or Cross-Cutting Rules Change

Some updates are not “one requirement” but **cross-cutting**:

- UA/RU policy rules,
- localisation approach,
- security/privacy baselines,
- integration contracts (Steam, RetroAchievements, metadata providers).

**Actions in the RTM**

1. **Non-Functional Mapping (Section 7)**
   - Update affected NFR rows and their **Affected Areas** and **Tests / Evidence**.
   - Re-classify coverage (`PC` → `NC` temporarily) if tests no longer match the new rules.

2. **Risk & Assumption Links (Section 8)**
   - Update risk and assumption mappings:
     - Add new risks or increase severity if rules got stricter or integrations more fragile.
     - Add new validation approaches where necessary (e.g. new monitoring, new test types).

3. **Module Summaries (Section 9)**
   - If a cross-cutting change hits several modules, revisit the module summaries to reflect:
     - updated scope,
     - updated coverage,
     - new or revised gaps.

---
<br>
<br>

## 11. Ownership & Review Cadence

This section defines **who owns the RTM**, who contributes to it, and **how often** it should be reviewed so it stays aligned with the real state of Dream Project.

The RTM only has value if it is **owned, maintained, and actually consulted** when changes are made.

---
<br>

### 11.1 Primary Owner

**Role:** Business Analyst (BA) / Requirements Lead  

**Responsibilities:**

- Maintain the **overall structure** of the RTM and keep sections 4–10 coherent.
- Ensure that every **in-scope BR** has:
  - at least one **SR** (or explicit justification why not),
  - at least one **UC** where it is realised,
  - and at least one **test suite** planned or executed (or clearly marked as a gap).
- Drive updates when:
  - new features are added or priorities change,
  - major assumptions are validated/invalidated,
  - or risks impact scope, requirements, or test focus.
- Coordinate with Tech Lead and QA Lead when changes cross module boundaries.

The BA does **not** have to edit every test ID or technical detail personally, but is responsible for making sure someone does and that RTM links stay meaningful.

---
<br>

### 11.2 Key Contributors

The RTM is a **shared artefact**. The following roles contribute to keeping it accurate:

- **Tech Lead / System Architect**
  - Confirms that SR mappings reflect the actual **architecture and design**.
  - Flags when implementation choices change traceability (e.g., a single SR now covers two BRs).
  - Helps to maintain sections **5 (SRS mapping)**, **7 (NFR mapping)**, and **10 (change impact)**.

- **QA Lead / Test Engineer**
  - Owns the mapping to **test suites and cases**:
    - creates and maintains `TS-…` / `TC-…` IDs,
    - keeps coverage flags (`FC`, `PC`, `NC`, `NA`) in sync with real execution.
  - Maintains sections **3 (coverage legend)**, **6 (UC → Tests)**, **8 (risk/test links)**, and **9 (coverage summary)**.

- **Product Owner / Project Lead**
  - Uses the RTM to **prioritise scope** and decide which gaps are acceptable for MVP.
  - Confirms that coverage levels and known gaps match **business expectations**.
  - Provides input to sections **4 (business traceability)** and **9 (coverage summary & gaps)**.

- **Security / Privacy Advisor** (if involved)
  - Reviews and updates NFR rows related to security and privacy.
  - Ensures sections **7 (NFR mapping)** and **8 (security-relevant risks)** stay aligned with current baselines.

---
<br>

### 11.3 Review Cadence

To keep the RTM **alive and trustworthy**, use the following minimum cadence:

1. **At Each Major Baseline**
   - When **BRD** or **SRS** versions are baselined (e.g., `v1.0` for MVP, `v1.1` for a major enhancement):
     - Run a **full RTM check**:
       - every new or changed BR has SR/UC/test links,
       - any dropped scope is reflected as `NA` or removed,
       - coverage summary (Section 9) is updated.

2. **Before Each Significant Release / Demo**
   - For each planned **MVP release** or major internal demo:
     - BA + QA Lead review:
       - test coverage for all BRs marked as **“in this release”**,
       - the most critical risks and assumptions (Sections 8 and 9),
       - whether any new gaps have appeared due to late changes.

3. **After Major Change Requests**
   - When a **big feature** is added, removed, or significantly changed:
     - Update the RTM in parallel with BRD/SRS/UC/Tests.
     - At minimum, revise:
       - affected rows in Sections 4–6,
       - NFR mapping (Section 7) if cross-cutting,
       - and coverage/risk notes (Sections 8–9).

4. **Lightweight Monthly Review (While Active)**
   - During active MVP development:
     - Perform a short **monthly RTM review**:
       - check that new tests are linked,
       - re-evaluate `PC` / `NC` areas,
       - ensure no “orphan” BR/SR/UCs appear without links.

---
<br>

### 11.4 Change Logging

To make RTM evolution transparent:

- Use the **Change History** block at the end of the document to record:
  - **Date**
  - **Version / Tag** (e.g., `RTM v1.0-MVP`, `RTM v1.1-PostMVP-1`)
  - **Author / Owner**
  - **Summary of changes** (e.g., “Added mapping for new achievement evidence rules”, “Updated INT coverage after Steam API v2”).

- For significant changes, also cross-link:
  - updated BRD/SRS/UC/Test versions,
  - relevant Risk & Assumptions Register entries,
  - and Project Plan sections (e.g., scope/phase changes).

---
<br>

### 11.5 When the RTM Can Be “Good Enough”

For an MVP like **Dream Project**, the RTM does **not** have to be mathematically perfect. It has to:

- cover all **critical business goals and flows**;  
- make gaps and limitations **explicit**;  
- be **easy to update** when scope or tests change.

If new work is added or priorities shift, this section is the reminder that someone (usually the BA with QA and Tech Lead) must:

1. Touch the RTM when they touch the BRD/SRS/UC/Tests.  
2. Keep ownership clear.  
3. Treat the RTM as a **living map**, not a one-off deliverable.

---
<br>
<br>

## 12. Change History

This section records **how the RTM has evolved over time**.  
Each entry captures what changed, when, and by whom, so anyone reviewing the document can quickly see which mappings and coverage statements are current.

| Version | Date       | Author / Role     | Summary of Changes                                                                                          |
|---------|------------|-------------------|-------------------------------------------------------------------------------------------------------------|
| 0.1     | 2025-XX-XX | BA (Initial Draft) | Initial RTM structure created (Sections 1–3). Basic BR → SR links captured for core modules (ACC, SAV, ACH). |
| 0.5     | 2025-XX-XX | BA + QA Lead      | Added UC and Test mappings (Sections 4–6). Introduced coverage legend (FC/PC/NC/NA) and first coverage pass. |
| 0.8     | 2025-XX-XX | BA + Tech Lead    | Extended NFR mapping (Section 7) and Risk/Assumption links (Section 8). Updated INT and UA/RU policy mapping. |
| 1.0     | 2025-XX-XX | BA + QA Lead      | MVP baseline: refreshed coverage summary (Section 9), added maintenance rules (Section 10) and ownership (Section 11). |

> **Maintenance rule:**  
> For any significant change to BRD, SRS, Use Case Spec, or Test Case Spec:
> - update the relevant RTM sections (4–10), and  
> - add a new row in this table with date, version tag, author, and a short description of what changed.

