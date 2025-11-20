# Risks & Assumptions Register

This register centralises **project risks** (threats to scope, schedule, quality, or compliance) and **assumptions** (facts we’re treating as true until validated) for the **Dream Project**. It complements summaries in the Business Case and BRD and is the day-to-day source of truth for mitigation and follow-up.

## Table of Contents

- [1. Introduction & Purpose](#1-introduction--purpose)  
  What this register is for, how it differs from summaries in other docs, and who owns it.

- [2. How to Use This Register](#2-how-to-use-this-register)  
  Workflow for adding/updating entries, roles and responsibilities (owner, reviewer), and status lifecycle.

- [3. Scoring Model & Categories](#3-scoring-model--categories)  
  Likelihood × Impact scoring (1–5), severity bands (Low/Moderate/High/Critical), and risk categories (Product, Technical, Security/Privacy, Legal/IP, Community/Moderation, Integration, Delivery).

- [4. Risk Register (Active)](#4-risk-register-active)  
  **Canonical table** of current risks with: `RISK-ID`, Title, Description, Triggers/Signals, Impact, Likelihood, **Score**, Mitigation, Contingency/Fallback, Owner, Due/Review Date, Status, Cross-links (BC/BRD/SRS/UC).

- [5. Closed / Accepted Risks](#5-closed--accepted-risks)  
  Risks that were mitigated, accepted, or are no longer applicable, with final outcome notes.

- [6. Assumptions Log (Active)](#6-assumptions-log-active)  
  **Canonical table** of current assumptions with: `ASM-ID`, Statement, Rationale, Confidence (High/Med/Low), Validation Method, Evidence Needed, Owner, **Validation Due**, Status, Cross-links.

- [7. Assumptions Validation Plan](#7-assumptions-validation-plan)  
  How each active assumption will be tested (experiments, spikes, outreach), success/fail criteria, and what changes if disproven.

- [8. Monitoring & Reporting Cadence](#8-monitoring--reporting-cadence)  
  Check-in rhythm (weekly), dashboards/alerts, and what gets escalated to whom and when.

- [9. Cross-References](#9-cross-references)  
  Pointers to related decisions and artefacts (Business Case §9 Risk Summary, BRD §9 Assumptions & Constraints, SRS NFRs, Use Cases with governance impacts).

- [10. Change History](#10-change-history)  
  Versioned log of edits to this register (who/when/what changed).

---
<br>
<br>

## 1. Introduction & Purpose

This register is the **living source of truth** for uncertainties around the Dream Project. It captures:
- **Risks** — potential events or conditions that could harm scope, schedule, quality, security/privacy, community trust, or compliance.
- **Assumptions** — statements we’re treating as true for now, which must be **validated** or **retired** by a due date.

Unlike the brief summaries in the **Business Case** and **BRD**, this register is **operational**: every entry has an owner, status, due/review dates, and a concrete mitigation or validation plan. It is updated continuously and referenced during planning, estimation, and go/no-go checks.

---
<br>

### Why we keep this register
- **Focus:** Make uncertainties explicit so they can be managed, not discovered late.
- **Alignment:** Give all stakeholders (product, engineering, moderators, community) a single view of current threats and working assumptions.
- **Actionability:** Tie each item to clear **owners**, **signals/triggers**, **mitigations/contingencies** (for risks), and **validation methods** (for assumptions).
- **Traceability:** Link risks/assumptions back to **BRD**, **SRS**, and **Use Cases**, so changes in one place trigger updates elsewhere.

---
<br>

### What qualifies for inclusion
- **Product/Delivery:** scope creep, capacity gaps, timeline slippage.
- **Technical/Integrations:** third-party outages/quotas, version/manifest mapping, emulator facets, AI triage accuracy.
- **Security/Privacy/Legal:** data handling, evidence-link allowlist integrity, content takedowns.
- **Community/Moderation:** abuse load, curation quality, regional policy sensitivities (e.g., UA/RU visibility rules) affecting discovery only.
- **Cost/Operations:** hosting costs, storage growth, moderation throughput.

---
<br>

### How this register is structured
- **Scoring model:** Likelihood × Impact (1–5) → **Severity** bands (Low/Moderate/High/Critical). See §3.
- **Risk entries:** `RISK-###` with title, description, triggers, score, mitigation, contingency, owner, review date, status. See §4.
- **Assumption entries:** `ASM-###` with statement, rationale, confidence, validation method, evidence needed, owner, validation due, status. See §6–7.
- **Lifecycle & cadence:** Add/update any time; **weekly** review cycle with owners; escalation rules in §8.

---
<br>

### Ownership & governance
- **Register Owner:** Business Analyst (working with Project Lead) — maintains format, runs weekly review, ensures cross-links.
- **Item Owner:** The person best positioned to act (e.g., Tech Lead for build/manifest mapping risk, Moderation Lead for abuse load).
- **Escalation:** High/Critical items with missed dates escalate to Project Lead; blocking items surface in release go/no-go.

---
<br>

### Scope & boundaries
- **In scope:** MVP through early growth (features, data, moderation, integrations, infrastructure, costs).
- **Out of scope:** Pure HR issues or unrelated org risks unless they directly affect delivery.
- **Policy discipline:** UA/RU policy matters are recorded here **only** as they affect **catalogue/discovery** behaviour, never profile/achievement views.

> Success looks like this: no unowned High/Critical items, all mitigations/validations tracked to a date, and clear links from each risk/assumption to the requirements they influence.

---
<br>
<br>

## 2. How to Use This Register

This section explains **who updates what, when, and how**. It is the operational playbook for keeping risks and assumptions current and actionable.

### 2.1 Workflow at a Glance

1. **Spot it → Log it**  
   - Anyone may propose a new entry. Use the templates in §2.6.  
   - Assign a **provisional ID** (`RISK-TBD` / `ASM-TBD`) if you don’t know the next number.

2. **Triage (within 24h)** — *Register Owner (BA)*  
   - Deduplicate or merge, assign the next numeric ID (`RISK-021`, `ASM-014`).  
   - Nominate an **Item Owner** and an **initial review date**.  
   - For risks: set **Likelihood** and **Impact** (1–5) → **Score** (see §3).  
   - For assumptions: set **Confidence** (High/Med/Low) and **Validation Due**.

3. **Plan**  
   - **Risks:** Item Owner proposes **Mitigation** (reduce Likelihood/Impact) and **Contingency** (what we do if it happens).  
   - **Assumptions:** Item Owner defines a **Validation Method** and **Evidence Needed**.

4. **Execute & Update (weekly)**  
   - Owners update **Status**, **Signals/Triggers**, progress on mitigations/validations, and adjust dates as needed.  
   - Register Owner checks for stale items and pings owners.

5. **Decide**  
   - **Risks:** Close (mitigated / no longer applicable), **accept** (document rationale), or **transfer** (e.g., to vendor).  
   - **Assumptions:** Mark **Validated (True)** or **Invalidated (False)** and record knock-on changes (links to BRD/SRS/UCs).

6. **Report & Escalate** — *Weekly*  
   - High/Critical risks and overdue validations appear on the weekly summary (see §8).  
   - Escalation rules in §2.5.

---
<br>

### 2.2 Roles & Responsibilities

- **Register Owner (BA):** Intake, triage, ID assignment, meeting cadence, quality control, cross-link hygiene.  
- **Item Owner (by domain):** Writes/executes mitigation or validation plan; keeps dates and status current.  
- **Project Lead:** Arbitrates contention, accepts residual risk, approves scope/schedule impacts.  
- **Tech Lead / Mod Lead / Ops Lead:** Own domain-specific items (integrations, moderation load, infra).  
- **All Contributors:** Propose entries, supply evidence, signal changes quickly.

---
<br>

### 2.3 Status Lifecycle

**Risks**

| Status | Meaning | Typical Next |
|---|---|---|
| *Open* | Logged, scored, owner assigned | Mitigating / Monitoring |
| *Mitigating* | Mitigation actions in progress | Monitoring / Closed / Accepted / Transferred |
| *Monitoring* | Watching signals; mitigation in place | Closed / Accepted |
| *Blocked* | Owner cannot proceed (dependency) | Mitigating (unblocked) |
| *Accepted* | Residual risk intentionally kept (rationale recorded) | Review by date / Closed |
| *Transferred* | Handed to vendor/partner/insurance | Monitoring / Closed |
| *Closed* | No longer relevant or fully mitigated | — |

**Assumptions**

| Status | Meaning | Typical Next |
|---|---|---|
| *Proposed* | Drafted, awaiting triage | Active |
| *Active* | Tracked with owner & due date | Validating |
| *Validating* | Experiment/outreach/spike running | Validated / Invalidated |
| *Validated (True)* | Confirmed; keep as constraint or fact | Archived |
| *Invalidated (False)* | Disproven; follow-up required | Change actions logged |
| *Superseded* | Replaced by a newer statement | Archived |

---
<br>

### 2.4 Scoring & Review Cadence

- **Risks:** Likelihood × Impact (1–5) → **Score** (see bands in §3).  
  - **Review cadence:**  
    - *Critical (≥16):* **Daily** until mitigated/accepted.  
    - *High (12–15):* **Weekly**.  
    - *Moderate (6–11):* **Bi-weekly**.  
    - *Low (≤5):* **Monthly**.
- **Assumptions:** Confidence drives urgency.  
  - *Low confidence* or *high impact if false* ⇒ validate **within 2–4 weeks**.

> Owners update entries **before** the weekly sync; the Register Owner flags anything stale (no updates in 14 days).

---
<br>

### 2.5 Escalation Rules

Escalate to the **Project Lead** when any of the following occurs:

- A **Critical** risk remains *Open/Mitigating* beyond **7 days**.  
- A **High** risk’s mitigation slips twice or impacts the MVP scope/NFRs.  
- An **assumption validation** overdue by > **14 days** with potential scope or architectural consequences.  
- A provider (Steam/RA/object storage) change threatens compliance with **NFR-INT-01** or **NFR-AVL-01**.

Escalation outcomes may include scope trade-offs, timeline adjustments, or temporary feature flags.

---
<br>

### 2.6 Templates (copy & fill)

**Risk template**

    RISK-### — <Short Title>
    Category: <Product / Technical / Security/Privacy / Legal/IP / Community/Moderation / Integration / Delivery>
    Owner: <Name/Role> | Status: <Open/Mitigating/...> | Review: <YYYY-MM-DD>
    Likelihood: <1–5> | Impact: <1–5> | **Score**: <L×I> (see §3 band)

    **Description**  
    What can happen and why it matters.

    **Signals / Triggers**  
    Early warning signs, thresholds, metrics, provider notices.

    **Mitigation (reduce Likelihood/Impact)**  
    Concrete steps, owners, dates.

    **Contingency (if it happens)**  
    Fallback plan, feature flags, comms.

    **Cross-links**  
    BRD: BR-… | SRS: SR-… | UC: UC-… | NFR: NFR-…

**Assumption template**

    ASM-### — <Statement>
    Owner: <Name/Role> | Status: <Active/Validating/...> | Validation Due: <YYYY-MM-DD>
    Confidence: <High/Med/Low>

    **Rationale**  
    Why we think this is true.

    **Validation Method & Evidence Needed**  
    Experiment/spike/outreach; what evidence proves/disproves it.

    **Impact if False**  
    What we change (scope/architecture/NFRs).

    **Cross-links**  
    BRD: BR-… | SRS: SR-… | UC: UC-…

**Quality Bar:** Specific · Owned · Linked · Current · Decisive.

---
<br>

### 2.7 Tooling & Hygiene

- **Single source:** Maintain this register in version control; edits via PRs or tracked changes.  
- **ID discipline:** Never reuse IDs; mark withdrawn items as **Closed** with rationale.  
- **Cross-link checks:** Each new/edited entry must include BRD/SRS/UC references (where applicable).  
- **Audit trail:** Keep change history succinct (who/when/what).  
- **Language:** EN/UA as per project standard; plain, non-judgmental phrasing.

---
<br>
<br>

## 3. Scoring Model & Categories

This section defines how we **score risks** and **prioritise assumptions** so decisions are consistent across the project.

---

### 3.1 Dimensions & Scales

**Likelihood (L)** — probability the risk will occur in the MVP horizon.

| Score | Label | Anchor examples |
|---:|:---|:---|
| 1 | Rare | Only in edge cases; no prior occurrences in similar projects. |
| 2 | Unlikely | Possible but not expected; sporadic in the wild. |
| 3 | Possible | Could happen; seen before under normal conditions. |
| 4 | Likely | Expected at least once unless actively mitigated. |
| 5 | Almost certain | Will happen without intervention (e.g., known quota limits). |

**Impact (I)** — effect if the risk occurs. Score the **highest applicable** column.

| Score | Users & Community | Scope/Timeline | Security/Legal | Cost/Operations |
|---:|:---|:---|:---|:---|
| 1 | Minor annoyance; quick workaround | < 1 day slip | No sensitive data or policy touch | Negligible |
| 2 | Noticeable friction for a subset | 1–3 days slip | Low-risk policy confusion | Small, absorbed in buffer |
| 3 | Visible degradation for many | 4–10 days slip | Privacy complaint risk | Medium unplanned spend |
| 4 | Core flow blocked for many | 2–4 weeks slip | Potential breach/takedown | Significant cost or rework |
| 5 | Platform trust at risk | > 4 weeks slip / replan | Breach/regulatory exposure | Budget threat / service disruption |

**Risk Score = L × I** (range 1–25)

---
<br>

### 3.2 Severity Bands & Actions

| Score | Severity | What we do |
|---:|:---|:---|
| 1–5 | Low | Track; lightweight mitigation; **monthly** check. |
| 6–11 | Moderate | Owner plan required; **bi-weekly** review. |
| 12–15 | High | Active mitigation + contingency; **weekly** review and visible on status. |
| 16–25 | Critical | Daily attention; feature flags/rollback paths ready; escalate to Project Lead. |

> Use **real signals** (metrics, queues, provider statuses) to justify changes to L or I. If uncertainty is high, bias upward and revisit after one week.

---
<br>

### 3.3 Assumptions: Confidence & Validation Priority

Assumptions use **Confidence × Consequence** to set validation urgency.

- **Confidence**: High (evidence exists) · Medium (reasonable but unproven) · Low (speculative).
- **Consequence** (if false): Low (minor copy/UX change) · Medium (feature behaviour change) · High (architecture/scope/NFR impact).

| Confidence \ Consequence | Low | Medium | High |
|:--|:--:|:--:|:--:|
| **High** | Validate when convenient | ≤ 6 weeks | ≤ 4 weeks |
| **Medium** | ≤ 6 weeks | ≤ 4 weeks | ≤ 2–3 weeks |
| **Low** | ≤ 4 weeks | ≤ 2–3 weeks | **≤ 2 weeks** |

Record the **Validation Method** and **Evidence Needed** in the log (see §2.6 template).

---
<br>

### 3.4 Categories (with project-specific examples)

| Category | Typical Owner | Examples in Dream Project |
|---|---|---|
| **Product/Adoption** | BA / Product | Curator recruitment shortfall; UA/EN policy banner perception in discovery; unclear value for album badges. |
| **Technical/Architecture** | Tech Lead | Build/manifest mapping for saves; AI triage thresholds; search/typeahead budgets. |
| **Security/Privacy** | Tech Lead / Admin | Token storage & rotation; evidence link abuse; AV scan coverage. |
| **Legal/IP & Policy** | Project Lead | Takedowns for screenshots; account-bound save restrictions; discovery-only policy banners accuracy. |
| **Community/Moderation** | Mod Lead | Report queue overload; toxicity in comments/ratings; appeal turn-around. |
| **Integrations** | Tech Lead | Steam/RA quota/outage handling; cooldowns; mapping drift. |
| **Delivery/Operations/Cost** | Ops Lead | Storage/egress spikes; backup/DR targets; on-call coverage. |

> Choose the **single best category** to avoid double-counting; cross-link secondary impacts in the entry’s description.

---
<br>

### 3.5 Scoring Guidance & Edge Cases

- **Score the real outcome, not the annoyance.** If the only effect is a small copy tweak, Impact ≤ 2.  
- **Don’t average impacts.** Use the **worst credible** impact for the audience affected.  
- **Compound risks.** If two risks must co-occur to cause harm, score the combined likelihood; otherwise log them separately.  
- **Risk vs. Issue.** If it **already happened**, log it as an **issue** in delivery tracking, and (optionally) keep a residual risk to prevent recurrence.  
- **Re-scoring cadence.** Revisit L and I after each mitigation milestone or when signals change (queues, p95, provider status, cost dashboards).

---
<br>
<br>

## 4. Risk Register (Active)

> Headline table first; detailed cards follow. Scores use **Likelihood (L)** × **Impact (I)** per §3.

| ID | Title | Category | L | I | Score | Severity | Owner | Next Review | Status |
|---|---|---|---:|---:|---:|:---|:---|:---:|:---|
| RISK-001 | Provider outage/quotas (Steam/RA) | Integration | 3 | 4 | 12 | High | Tech Lead | 2025-12-04 | Mitigating |
| RISK-002 | Wrong build/manifest mapping → bad save compatibility | Technical | 3 | 5 | 15 | High | Tech Lead | 2025-12-04 | Mitigating |
| RISK-003 | Evidence allowlist abuse (malicious/low-quality links) | Security/Privacy | 3 | 4 | 12 | High | Admin | 2025-12-04 | Mitigating |
| RISK-004 | AI triage accuracy too low (sticker validation) | Technical | 3 | 4 | 12 | High | Tech Lead | 2025-12-04 | Mitigating |
| RISK-005 | Moderation overload (reports, pending reviews) | Community/Moderation | 3 | 4 | 12 | High | Mod Lead | 2025-12-04 | Monitoring |
| RISK-006 | Storage/egress cost spikes (images/saves) | Delivery/Operations/Cost | 3 | 4 | 12 | High | Ops Lead | 2025-12-04 | Mitigating |
| RISK-007 | Legal/IP concerns for screenshots/saves | Legal/IP & Policy | 2 | 5 | 10 | Moderate | Project Lead | 2025-12-11 | Mitigating |
| RISK-008 | Low curator adoption / not enough fan sets | Product/Adoption | 3 | 4 | 12 | High | BA | 2025-12-11 | Mitigating |
| RISK-009 | Toxicity/abuse in comments/ratings | Community/Moderation | 3 | 3 | 9 | Moderate | Mod Lead | 2025-12-11 | Monitoring |
| RISK-010 | Security breach / token leakage | Security/Privacy | 2 | 5 | 10 | Moderate | Tech Lead | 2025-12-04 | Mitigating |
| RISK-011 | UA/RU policy flags inaccurate (discovery-only) | Product/Policy | 2 | 4 | 8 | Moderate | BA | 2025-12-11 | Mitigating |
| RISK-012 | Performance budgets missed under load | Technical | 3 | 4 | 12 | High | Tech Lead | 2025-12-04 | Mitigating |

---
<br>

### RISK-001 — Provider outage/quotas (Steam/RA)

**Description**  
Read-only achievement syncs can fail or be throttled, leaving progress displays **stale** and confusing.

**Signals / Triggers**  
Provider 429/5xx; retry/backoff counts ↑; sync success rate < 95%; cooldown hits ↑.

**Mitigation**  
User/provider **cooldowns**; exponential backoff; circuit breakers; stale badge on UI; queue and retry off-peak.

**Contingency**  
Temporarily disable manual sync; banner on affected pages; RA-only or Steam-only fallback if one provider is degraded.

**Owner** Tech Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
BRD: BR-INT-02/03 · SRS: SR-INT-05/06, SR-ACH-12 · UCS: UC-INT-01/UC-ACH-02 · NFR: NFR-INT-01, NFR-AVL-01

---
<br>

### RISK-002 — Wrong build/manifest mapping → bad save compatibility

**Description**  
Incorrect build/manifest or region mapping mislabels saves (**confirmed/untested/legacy/incompatible**) and frustrates users.

**Signals / Triggers**  
Spike in “Didn’t work” confirmations; mismatch between declared and detected build; moderator overrides ↑.

**Mitigation**  
Multiple sources (Steam build IDs, community confirmations, checksum heuristics); **admin override**; auto-downgrade to *untested* when in doubt.

**Contingency**  
Fast triage queue; hide “confirmed” chip until quorum; add variant-specific guidance.

**Owner** Tech Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
BRD: BR-SAV-02/06 · SRS: SR-SAV-24..27, SR-CAT-04 · UCS: UC-CAT-02 · NFR: NFR-UX-03

---
<br>

### RISK-003 — Evidence allowlist abuse (malicious/low-quality links)

**Description**  
Fan achievement completions may include unsafe or spammy links, eroding trust or risking user safety.

**Signals / Triggers**  
Report spikes; domain-specific abuse; validation failures; appeal rate ↑ tied to rejected links.

**Mitigation**  
Tight **allowlist** with per-domain rules and test-URL tool; rate limits; automated checks for redirects/trackers.

**Contingency**  
**Suspend** offending domain; mark legacy evidence “Needs review”; explanatory UI copy.

**Owner** Admin · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
BRD: BR-ACH-05 · SRS: SR-ACH-18/19, SR-GOV-14 · UCS: UC-INT-02, UC-ACH-04 · NFR: NFR-SEC-05/07

---
<br>

### RISK-004 — AI triage accuracy too low (sticker validation)

**Description**  
False rejects delay users; false accepts pollute albums and undermine the badge system.

**Signals / Triggers**  
>10% manual appeals; high disagree rate versus moderator decisions; curator complaints.

**Mitigation**  
Conservative thresholds; *Pending* path; sampled human review loop; model re-tuning.

**Contingency**  
Route affected templates to manual review temporarily; publish expected turnaround in UI.

**Owner** Tech Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
BRD: BR-ALB-03 · SRS: SR-ALB-03..05 · UCS: UC-ALB-03 · NFR: NFR-PERF-06

---
<br>

### RISK-005 — Moderation overload (reports, pending reviews)

**Description**  
Queues for reports, evidence checks, and pending stickers may exceed SLA, hurting trust and throughput.

**Signals / Triggers**  
Queue age > 72h; time-to-first-action > 24h; open appeals backlog ↑.

**Mitigation**  
Priority routing (malware first); bulk actions; volunteer moderators; weekly capacity planning.

**Contingency**  
Throttle new submissions; freeze comments on hot threads; temporarily relax “pending” SLA messaging.

**Owner** Mod Lead · **Next Review** 2025-12-04 · **Status** Monitoring

**Cross-links**  
BRD: BR-GOV-01/02 · SRS: SR-GOV-12..18 · UCS: UC-GOV-01/02 · NFR: NFR-OBS-03

---
<br>

### RISK-006 — Storage/egress cost spikes (images/saves)

**Description**  
Rapid growth in screenshots and saves increases object storage and CDN egress beyond budget.

**Signals / Triggers**  
Monthly storage/egress > budget by 20%; cache miss rate ↑ for hot objects.

**Mitigation**  
Lifecycle policies (archive cold saves), image resizing, dedup by checksum, CDN caching.

**Contingency**  
Tighten quotas; defer high-res previews; seek credits/sponsorships.

**Owner** Ops Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
BRD: BR-OPS-01 · SRS: SR-DATA-07..10 · NFR: NFR-REL-02

---
<br>

### RISK-007 — Legal/IP concerns for screenshots/saves

**Description**  
Some titles restrict redistribution of assets or saves; takedowns create operational risk.

**Signals / Triggers**  
Publisher complaints; ambiguous EULAs; DMCA notices.

**Mitigation**  
Clear ToS; DMCA/takedown process; per-title policy switches (disable Saves where account-bound).

**Contingency**  
Remove affected content; enable per-title feature flags; publish correction notes.

**Owner** Project Lead · **Next Review** 2025-12-11 · **Status** Mitigating

**Cross-links**  
BC §9 · BRD: BR-LEG-01 · SRS: SR-GOV-18 · NFR: NFR-PRIV-04

---
<br>

### RISK-008 — Low curator adoption / not enough fan sets

**Description**  
Without quality fan sets, the Achievement pillar under-delivers.

**Signals / Triggers**  
< 10 active curators by Month 2; < 20 published sets by Month 3.

**Mitigation**  
Curator application flow; outreach to RA contributors; recognition/badges; lightweight authoring tools.

**Contingency**  
Seed internal sets; prioritise RA mapping; highlight official/retro first.

**Owner** BA · **Next Review** 2025-12-11 · **Status** Mitigating

**Cross-links**  
BC §3 · BRD: BR-PRO-04, BR-ACH-03 · UCS: UC-PRO-04

---
<br>

### RISK-009 — Toxicity/abuse in comments/ratings

**Description**  
Harassment or spam degrades community health and deters curators.

**Signals / Triggers**  
Flag rate ↑; blocked-user count ↑; repeat-offender patterns.

**Mitigation**  
Rate limits; AI toxicity filters; report/appeal clarity; moderator tools.

**Contingency**  
Lock threads; temporary comment freeze per set; targeted bans.

**Owner** Mod Lead · **Next Review** 2025-12-11 · **Status** Monitoring

**Cross-links**  
BRD: BR-ACH-06 · SRS: SR-GOV-11..14 · UCS: UC-GOV-01 · NFR: NFR-SEC-07

---
<br>

### RISK-010 — Security breach / token leakage

**Description**  
Compromise of tokens/PII would be severe (provider links, account trust).

**Signals / Triggers**  
Vault/IAM alerts; anomalous access; IDS signals; secrets in logs (should be impossible).

**Mitigation**  
KMS-backed secrets; least privilege; secret scanning; pen tests; no tokens in logs.

**Contingency**  
Revoke/rotate tokens; incident comms; forensics; regulatory steps.

**Owner** Tech Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
SRS: SR-SEC-01..05 · NFR: NFR-SEC-01..04

---
<br>

### RISK-011 — UA/RU policy flags inaccurate (discovery-only)

**Description**  
Misclassification harms trust for UA users or annoys others if over-applied. (Per rules, banners are **catalogue/discovery only**, never on profiles/achievements.)

**Signals / Triggers**  
User reports; mismatch with trusted sources; toggle-off rate ↑.

**Mitigation**  
Curated sources; admin overrides; clear rationale; UA default ON / others OFF but toggleable.

**Contingency**  
Disable banner for affected titles; publish correction notes; adjust source weights.

**Owner** BA · **Next Review** 2025-12-11 · **Status** Mitigating

**Cross-links**  
BA Doc §8 (policy scope) · BRD: BR-CAT-04 · SRS: SR-CAT-12 · UCS: UC-DSC-02

---
<br>

### RISK-012 — Performance budgets missed under load

**Description**  
p95 targets not met for search, game pages, uploads; UX regresses.

**Signals / Triggers**  
RUM/APM red; queue saturation; autoscale thrash.

**Mitigation**  
CDN cache; indexed aggregates; async panels; backpressure on heavy routes.

**Contingency**  
Feature-flag non-critical panels; scale out; temporarily relax budgets (with rationale & timebox).

**Owner** Tech Lead · **Next Review** 2025-12-04 · **Status** Mitigating

**Cross-links**  
SRS: SR-DSC-*, SR-ALB-*, SR-SAV-* · NFR: NFR-PERF-01..07, NFR-REL-01

---
<br>
<br>

## 5. Closed / Accepted Risks

This log captures risks that have been **retired** (closed) or **explicitly accepted** with rationale, so future readers understand past decisions and don’t re-litigate them.

### 5.1 Summary Table

| ID | Title | Category | Outcome | Rationale (short) | Owner | Decision Date | Follow-ups |
|---|---|---|:---:|---|---|:---:|---|
| RISK-013 | Native mobile apps in MVP | Delivery/Scope | **Accepted** | Out of scope for MVP; mobile web is sufficient; reduces build/test surface. | Project Lead | 2025-11-20 | Revisit in Phase II; track PWA enhancements & mobile RUM. |
| RISK-014 | Write-back to Steam/RA | Legal/Integration | **Closed** | Non-goal: integrations are read-only; avoids ToS and security exposure. | Tech Lead & Project Lead | 2025-11-20 | Periodic ToS review; keep UI copy explicit about read-only sync. |

---
<br>

### 5.2 Decision Cards

#### RISK-013 — Native mobile apps in MVP *(Accepted)*
**Context**  
Building iOS/Android apps now would slow web delivery and expand maintenance/testing scope without adding must-have MVP value.

**Decision & Rationale**  
**Accepted** as a residual risk: we may lose some native-only UX, but **mobile web** covers core flows (search, saves, sets, albums). Scope focus improves schedule confidence.

**Conditions / Follow-ups**  
- Measure mobile RUM; prioritise PWA features if needed (install prompt, offline shells).  
- Re-evaluate native apps post-MVP adoption (≥10k MAU, clear mobile usage patterns).

**Cross-links**  
BC §5 Scope; BRD §4 Scope; SRS NFRs (compatibility, performance).

---
<br>

#### RISK-014 — Write-back to Steam/RA *(Closed)*
**Context**  
Allowing our platform to push data back to providers creates ToS, security, and trust risks.

**Decision & Rationale**  
**Closed** as not applicable: integrations are **read-only** by design (user-initiated sync, no provider mutations).

**Conditions / Follow-ups**  
- Quarterly ToS/legal check.  
- UI copy and docs clearly state “read-only sync”; error states use “stale” badges rather than retries beyond quotas.

**Cross-links**  
BRD §10 Integrations; SRS Integrations (read-only); UCS UC-INT-01/UC-ACH-02.

---
<br>
<br>

## 6. Assumptions Log (Active)

Active working assumptions that guide planning and design. Each item has an owner and a target date for validation (see §7 for the validation plan).

### 6.1 Summary Table

| ID | Statement | Confidence | Owner | Validation Due | Status |
|---|---|---|:---:|:---:|:---|
| **ASM-001** | Users accept **read-only** Steam/RetroAchievements sync (no write-back). | High | BA | 2025-12-18 | Active |
| **ASM-002** | Curators will publish **≥10** fan sets by Month 2. | Medium | BA | 2026-01-08 | Active |
| **ASM-003** | AI sticker-triage can reach **≥85%** agreement with moderators after tuning. | Medium | Tech Lead | 2026-01-08 | Validating |
| **ASM-004** | Evidence links from an **allowlisted set of domains** are acceptable to users. | Medium | Admin | 2025-12-18 | Active |
| **ASM-005** | Build/manifest mapping is maintainable via mixed sources + **community confirmations**. | Medium | Tech Lead | 2025-12-18 | Active |
| **ASM-006** | Storage/egress stays within MVP budget using **lifecycle rules** and caching. | Medium | Ops Lead | 2026-01-08 | Active |
| **ASM-007** | Early adopter mix ≈ **UA 25% / EN 75%**. | Medium | BA | 2026-01-08 | Active |
| **ASM-008** | Users will manually mark fan achievements (with evidence links) in MVP. | Medium | BA | 2025-12-18 | Active |
| **ASM-009** | Discovery-scoped UA/RU policy banners **won’t deter** non-UA users when toggleable. | Medium | BA | 2026-01-08 | Active |
| **ASM-010** | Steam/RA ToS **permit read-only sync** as designed. | High | Tech Lead | 2025-12-04 | Validating |

---
<br>

### 6.2 Assumption Cards (Details)

#### ASM-001 — Users accept read-only Steam/RA sync
**Rationale**  
Write-back is unnecessary for our value prop and increases ToS/security risk; read-only keeps things simple.

**Validation Method & Evidence Needed**  
Onboarding survey + telemetry: % linking providers; manual sync usage; drop-off near sync prompts.

**Impact if False**  
De-emphasise provider panels; prioritise local features (saves/albums); revisit copy and placement.

**Cross-links**  
BRD: BR-INT-02/03 · SRS: SR-INT-05/06 · UCS: UC-INT-01/UC-ACH-02

---
<br>

#### ASM-002 — Curators will publish ≥10 fan sets by Month 2
**Rationale**  
Outreach + lightweight authoring lowers friction; RA community provides seed talent.

**Validation Method & Evidence Needed**  
Curator application funnel; # approved curators; # published sets.

**Impact if False**  
Seed internal sets; expand outreach; improve authoring UX; highlight official/retro sets more prominently.

**Cross-links**  
BRD: BR-ACH-03, BR-PRO-04 · UCS: UC-PRO-04

---
<br>

#### ASM-003 — AI sticker-triage can reach ≥85% agreement with moderators
**Rationale**  
Conservative thresholds + feedback loop should achieve reliable pre-filtering.

**Validation Method & Evidence Needed**  
A/B thresholds vs. moderator labels; appeal rates; precision/recall on sampled batches.

**Impact if False**  
More manual review; adjust thresholds; tighter template guidance; queue UX messaging.

**Cross-links**  
BRD: BR-ALB-03 · SRS: SR-ALB-03..05 · UCS: UC-ALB-03

---
<br>

#### ASM-004 — Allowlisted evidence links are acceptable to users
**Rationale**  
Scope-limiting domains reduces risk while still covering popular hosts (video, paste, docs).

**Validation Method & Evidence Needed**  
Funnel analysis around evidence step; complaints; rejection/appeal patterns by domain.

**Impact if False**  
Expand allowlist with per-domain rules; add “text note” fallback (stricter review).

**Cross-links**  
BRD: BR-ACH-05 · SRS: SR-ACH-18/19, SR-GOV-14 · UCS: UC-INT-02

---
<br>

#### ASM-005 — Build/manifest mapping is maintainable with mixed sources + confirmations
**Rationale**  
Combining Steam build IDs/checksums with community confirmations should keep labels accurate.

**Validation Method & Evidence Needed**  
Time-to-confirm per popular variant; “didn’t work” rate; moderator override frequency.

**Impact if False**  
Label more variants **untested** by default; invest in checksum heuristics; expand admin tooling.

**Cross-links**  
BRD: BR-SAV-02/06 · SRS: SR-SAV-24..27, SR-CAT-04 · UCS: UC-CAT-02

---
<br>

#### ASM-006 — Storage/egress within budget using lifecycle rules
**Rationale**  
Cold-storage archiving, resizing, and CDN caching reduce hot bytes and egress.

**Validation Method & Evidence Needed**  
Month-1/2 bills vs. forecast; cache hit ratio; GB stored/egressed per 1k actions.

**Impact if False**  
Tighten quotas; more aggressive lifecycle; defer high-res previews; seek credits.

**Cross-links**  
BRD: BR-OPS-01 · SRS: SR-DATA-07..10 · NFR: NFR-REL-02

---
<br>

#### ASM-007 — UA 25% / EN 75% early adopter mix
**Rationale**  
Community focus + global reach; UA default banners are relevant locally.

**Validation Method & Evidence Needed**  
Opt-in locale/country telemetry; UA/EN ratios among actives.

**Impact if False**  
Adjust outreach, default locales, and discovery messaging weights.

**Cross-links**  
BA Doc §8 (policy scope) · BRD: BR-CAT-04 · SRS: SR-CAT-12

---
<br>

#### ASM-008 — Users will manually mark fan achievements (with evidence) in MVP
**Rationale**  
Curators and early adopters are motivated; manual flows unblock value before full automation.

**Validation Method & Evidence Needed**  
Completion funnel: % submissions with acceptable evidence; drop-off at evidence step.

**Impact if False**  
Clearer examples; lighter review; add optional guidance links; explore semi-automation.

**Cross-links**  
BRD: BR-ACH-04/05 · SRS: SR-ACH-15..20 · UCS: UC-ACH-04

---
<br>

#### ASM-009 — Discovery banners won’t deter non-UA users when toggleable
**Rationale**  
Scoped to catalogue/discovery only; opt-out preserves neutrality for others.

**Validation Method & Evidence Needed**  
Toggle usage; feedback; click-through vs. baseline.

**Impact if False**  
Default banners OFF globally (UA remains ON); refine copy; reduce surface area.

**Cross-links**  
BRD: BR-CAT-04 · SRS: SR-CAT-12 · UCS: UC-DSC-02

---
<br>

#### ASM-010 — Steam/RA ToS permit read-only sync
**Rationale**  
We only call read endpoints with user consent and respect quotas/cooldowns.

**Validation Method & Evidence Needed**  
Legal/ToS review; spike with compliant headers/rates; provider responses.

**Impact if False**  
Disable affected provider; RA-only or Steam-only path; adjust UI copy.

**Cross-links**  
BRD: BR-INT-02/03 · SRS: SR-INT-05/06 · UCS: UC-INT-01

---
<br>
<br>

## 7. Assumptions Validation Plan

This section defines **how** we will test each active assumption from §6, the **signals** we will collect, and the **decision thresholds** that trigger follow-up changes to scope, design, or NFR budgets.

---
<br>

### 7.1 Validation Matrix

| ASM | Method (primary) | Key Signals / Metrics | Success Criteria | Owner | Due |
|---|---|---|---|:---:|:---:|
| **ASM-001** Read-only provider sync is acceptable | Onboarding survey + telemetry | % users linking Steam/RA; manual sync usage; drop-off near sync prompts | ≥50% of engaged users link ≥1 provider; <5% drop-off at sync prompts | BA | 2025-12-18 |
| **ASM-002** ≥10 fan sets by Month 2 | Curator outreach + application funnel | # approved curators; # published sets | ≥10 sets by M2; ≥5 active curators | BA | 2026-01-08 |
| **ASM-003** AI sticker triage ≥85% agreement | A/B thresholds vs. moderator labels | Agreement %, precision/recall; appeal rate | ≥85% agreement; appeals <10% of triaged items | Tech Lead | 2026-01-08 |
| **ASM-004** Allowlisted evidence links acceptable | UX funnel + feedback review | Evidence-step abandonment; complaints by domain | <5% step drop-off; low complaint rate | Admin | 2025-12-18 |
| **ASM-005** Build/manifest mapping is maintainable | Pilot mapping + confirmations | Time-to-confirm per variant; “didn’t work” rate | 2+ confirmations per top variant in ≤14 days; failure rate <10% | Tech Lead | 2025-12-18 |
| **ASM-006** Storage/egress within budget | Cost sim + month-1/2 bills | $/GB/mo; egress per 1k downloads; cache hit-rate | ≤ budget +10%; cache hit ≥80% on hot paths | Ops Lead | 2026-01-08 |
| **ASM-007** UA 25% / EN 75% early mix | Opt-in locale telemetry | UA/EN share among actives | 20–35% UA | BA | 2026-01-08 |
| **ASM-008** Users will manually mark fan achievements | Completion funnel analysis | % completions w/ acceptable evidence; time-to-review | ≥60% submit acceptable evidence; median review ≤48h | BA | 2025-12-18 |
| **ASM-009** Discovery banners don’t deter non-UA users | Toggle analytics + feedback | Toggle-off rate; negative feedback; CTR deltas | Toggle-off <30%; minimal negative feedback; neutral CTR | BA | 2026-01-08 |
| **ASM-010** Steam/RA ToS permit read-only sync | Legal review + API spike | ToS memo; compliant headers/rates; provider responses | Pass legal review; spike logs clean; no provider objections | Tech Lead | 2025-12-04 |

---
<br>

### 7.2 Experiment Playbooks (per Assumption)

Each playbook includes **Setup → Steps → Data → Analysis → Decision**. All experiments run with opt-in telemetry and anonymised aggregates unless personally necessary for support.

#### ASM-001 — Read-only sync is acceptable
- **Setup:** Add gentle provider-link prompt in onboarding; enable manual “Sync now”.
- **Steps:** 2-week cohort observes linking rate; run a 50/50 copy test (“Sync your progress” vs “Show your achievements here”).
- **Data:** Link rate, manual sync clicks, abandonment at prompt, help-desk questions.
- **Analysis:** Compare cohorts; segment by country/locale.
- **Decision:** If link rate <50% or drop-off ≥5%, reduce prompt surface and emphasise local value (saves, albums); keep read-only design.

#### ASM-002 — Curator output by Month 2
- **Setup:** Curator application form; lightweight authoring UI; RA outreach list.
- **Steps:** Weekly outreach; public “call for curators” post; seed 2 internal sets.
- **Data:** Applicants → approved → published sets; time-to-first-publish.
- **Analysis:** Funnel conversion; reasons for drop-off.
- **Decision:** If <10 sets by M2, add incentives, improve authoring help, or extend outreach.

#### ASM-003 — AI triage ≥85% agreement
- **Setup:** Shadow-mode model; curated labelled set from moderators.
- **Steps:** Run A/B thresholds; route small % to manual review regardless.
- **Data:** Agreement %, precision/recall, appeal rate, time-to-decision.
- **Analysis:** Confusion matrix by album template and game genre.
- **Decision:** If <85% or appeals ≥10%, lower thresholds and widen manual path; plan retraining.

#### ASM-004 — Allowlisted evidence links acceptable
- **Setup:** Evidence step supports only allowlisted domains (+ preview).
- **Steps:** Track funnel; collect feedback; sample problematic links for review.
- **Data:** Drop-off at evidence step; complaint volume by domain; rejection reasons.
- **Analysis:** Domain-level friction; user intent vs. allowed hosts.
- **Decision:** If drop-off ≥5% or complaints cluster on a reputable host, add that host with per-domain rules; consider “text note” fallback (stricter review).

#### ASM-005 — Build/manifest mapping maintainable
- **Setup:** Mapping service fed by Steam build IDs + checksum heuristics + community confirmations.
- **Steps:** For top 50 variants, request confirmations after uploads/downloads.
- **Data:** Time-to-confirm; “didn’t work” rate; admin override frequency.
- **Analysis:** Coverage vs. effort; drift by title/version.
- **Decision:** If confirmations lag (>14 days) or failure >10%, widen “untested” default, invest in heuristics, add more admin tooling.

#### ASM-006 — Storage/egress within budget
- **Setup:** Lifecycle rules (archive cold saves), image resizing, CDN on hot paths.
- **Steps:** Compare forecast to real Month-1/2 bills; tune cache TTLs.
- **Data:** $/GB, egress/1k downloads, cache hit-rate, object count growth.
- **Analysis:** Cost per action; top cost drivers.
- **Decision:** If cost > budget +10% or hit-rate <80%, tighten quotas and lifecycle, defer high-res previews.

#### ASM-007 — UA/EN adopter mix
- **Setup:** Opt-in locale/country capture; UA default banners ON, others OFF (toggleable).
- **Steps:** Track locale split; gather feedback.
- **Data:** UA vs EN actives; banner toggle usage.
- **Analysis:** Compare to target 20–35% UA.
- **Decision:** If below, increase UA outreach and localisation efforts; if above, ensure EN onboarding remains clear and neutral.

#### ASM-008 — Manual marking of fan achievements
- **Setup:** Completion form with evidence link; clear examples.
- **Steps:** Observe completion funnel; sample reviews for clarity.
- **Data:** % with acceptable evidence; time-to-review; appeal rate.
- **Analysis:** Identify friction points.
- **Decision:** If <60% acceptable or review >48h median, add examples, loosen thresholds with spot checks, or expand reviewers.

#### ASM-009 — Discovery banners do not deter
- **Setup:** Banner appears **only** in catalogue/discovery; toggle shown to non-UA users.
- **Steps:** Measure CTR and toggle-off; collect feedback.
- **Data:** Toggle-off %, CTR change vs baseline, negative feedback count.
- **Analysis:** Segment by locale and game type.
- **Decision:** If toggle-off ≥30% or CTR suffers, default banners OFF globally (UA remains ON); refine copy or reduce surface.

#### ASM-010 — ToS permit read-only sync
- **Setup:** Legal memo; limited API spike with compliant headers/rates.
- **Steps:** Run spike; record responses, rate limits, headers; no data stored long-term.
- **Data:** ToS excerpts, spike logs, provider responses.
- **Analysis:** Counsel review.
- **Decision:** If any conflict, disable affected provider; adjust UI copy and flows.

---
<br>

### 7.3 Instrumentation & Data Sources

- **Product analytics:** onboarding/link prompts, evidence step, completion funnels, banner toggles.  
- **Moderation tooling:** appeal rate, decision outcomes, time-to-first-action.  
- **Sync logs:** provider response codes, backoff/cooldown counts (aggregated).  
- **Cost dashboards:** object storage, CDN egress, cache hit-rate.  
- **Surveys/feedback:** short in-product prompts, help-desk tags.  
- **Legal/ToS repository:** memos and spike results.

> All telemetry is **opt-in** where required and aggregated/anonymised by default.

---
<br>

### 7.4 Privacy & Ethics Guardrails

- Minimise PII; store only what is essential for validation.  
- Pseudonymise user IDs in analysis; redact tokens/links in logs.  
- Respect **GDPR-like** rights (export/delete on request).  
- For evidence links, never fetch private content server-side without user consent.

---
<br>

### 7.5 Reporting & Decisions

- **Weekly BA-led review:** status of each ASM against its success criteria.  
- **Decision thresholds:** failure to meet criteria by due date → propose scope/UX/NFR adjustments within **one week**.  
- **Visibility:** summary added to project status; cross-links to BRD/SRS/UCS updated.

---
<br>

### 7.6 Change Control When an Assumption Fails

1. Mark **Invalidated (False)** in §6.  
2. Create follow-up items (BRD/SRS/UC edits, NFR changes).  
3. If user-facing, update copy/tooltips and help docs.  
4. Log decision in **Change History** with rationale and owner.

---
<br>

### 7.7 Validation Risks & Mitigations

- **Sampling bias** (early power users): include general audience cohorts; segment by locale.  
- **Seasonality** (holidays, releases): extend window or normalise metrics.  
- **Telemetry gaps:** add redundant events and manual spot checks.  
- **Moderator variance:** double-label a sample to calibrate agreement metrics.  
- **Provider policy shifts:** schedule quarterly ToS checks; keep read-only stance.

---
<br>
<br>

## 8. Monitoring & Reporting Cadence

This cadence keeps the register **current, owned, and actionable**. It defines when we look, what we look at, who attends, and how we escalate.

---
<br>

### 8.1 Cadence at a Glance

| Rhythm | What happens | Inputs | Outputs | Owner(s) |
|---|---|---|---|---|
| **Daily (workdays)** | Check any **Critical** risks; scan alerts (integrations, perf, security). | Alert feed, provider status, APM/RUM, moderation queue | Acknowledged/assigned incidents; register notes on affected risks | Tech Lead (primary), Ops Lead (infra), Mod Lead (community) |
| **Weekly** | **BA-led review** of High/Critical risks + overdue validations; update statuses/dates; agree next actions. | §4 risks, §6 assumptions, dashboards (§8.2) | 1-page digest (decisions, owners, due dates); register updates | BA (facilitator), Project Lead (decisions) |
| **Bi-weekly** | Moderate risks & cost/perf trends; scope trade-offs if needed. | Trend charts, storage/costs, p95 metrics | Tweaks to mitigations; NFR tuning proposals | Tech Lead, Ops Lead, BA |
| **Monthly** | Portfolio view: trends, budget vs. actuals, residual risk acceptance. | Month-end reports, validation outcomes | Accepted/closed items; plan updates; budget notes | Project Lead, BA, Leads |
| **Release gate** | Go/No-Go: open High/Critical risks, contingencies ready, validation blockers. | Risk list, test exit criteria, rollback plans | Go/No-Go decision; flagged follow-ups | Project Lead (chair), BA, Tech Lead |

---
<br>

### 8.2 Dashboards & KPIs (what we watch)

- **Integrations (Steam / RetroAchievements)**  
  Sync success rate; 429/5xx counts; cooldown hits; **time since last successful sync**; read-only compliance checks.
- **Save compatibility & catalogue mapping**  
  Confirmed/Untested/Legacy/Incompatible ratios; “Didn’t work” confirmations; admin overrides; time-to-confirm per variant.
- **Moderation & community health**  
  Time-to-first-action; queue age; appeals rate; toxicity flags; domain allowlist rejections.
- **Sticker albums & AI triage**  
  AI↔Moderator agreement %; pending backlog; auto-accept/auto-deny rates; appeal outcomes per template.
- **Discovery & search**  
  Typeahead p95; results p95; index freshness; UA/RU **discovery-only** banner toggle-off rate; mis-flag reports.
- **Storage & cost**  
  GB stored; CDN egress; cache hit-rate; top hot objects; cost per 1k actions.
- **Adoption & curation**  
  Active curators; fan sets published; curator time-to-first-publish; album templates created/used.
- **Security & privacy**  
  Secret scan results; token anomalies; rate-limit abuse; evidence-link validation failures.

> KPI owners keep thresholds current and document runbooks for each alert (see §8.3).

---
<br>

### 8.3 Alert Thresholds & Runbooks (examples)

| KPI | Threshold (initial) | Alert to | First steps (runbook) |
|---|---|---|---|
| Sync success rate | **< 95%** for 30 min | Tech Lead, Ops | Check provider status; enable backoff/cooldowns; show **stale** badge; queue retries off-peak. |
| “Didn’t work” confirmations | **> 10/day** per title/variant | Tech Lead, BA | Downgrade to *untested*; request confirmations; verify build/manifest mapping; notify admins if overrides spike. |
| Moderation time-to-first-action | **> 24h** median | Mod Lead | Rebalance queues; recruit volunteers; throttle new submissions if >72h backlog. |
| AI↔Moderator agreement | **< 85%** weekly | Tech Lead, Mod Lead | Lower thresholds; widen manual path; sample review; update template guidance. |
| Typeahead p95 | **> 300 ms** for 30 min | Tech Lead, Ops | Inspect indices/caches; degrade non-critical panels; scale out; open perf ticket. |
| Storage/egress | **> budget +10%** MoM | Ops Lead, Project Lead | Tighten quotas; adjust lifecycle; defer hi-res previews; seek credits. |
| Security anomaly | Any token/secret alert | Tech Lead, Admin | Rotate/revoke; incident comms; forensics; update register as High/Critical. |

---
<br>

### 8.4 Weekly Digest (structure)

- **Top risks (High/Critical):** status, owner, next date, change since last week.  
- **Overdue validations:** assumption, method, new due date, blockages.  
- **Alerts triggered:** what happened, actions taken, remaining risk.  
- **Trends:** perf (p95), costs, moderation SLAs, adoption/curation.  
- **Decisions needed:** explicit asks for the Project Lead.  
- **Links:** updated register, relevant BRD/SRS/UC edits.

---
<br>

### 8.5 Ownership & Attendance

| Area | Primary | Backup | Notes |
|---|---|---|---|
| Register & cadence | **BA** | Project Lead | Runs weekly; maintains cross-links. |
| Integrations & mapping | **Tech Lead** | Senior Dev | Steam/RA sync, build/manifest, compatibility layer. |
| Moderation & community | **Mod Lead** | Admin | Reports, appeals, allowlist, toxicity signals. |
| Perf/infra & costs | **Ops Lead** | Tech Lead | APM/RUM, storage/egress, scaling, SLOs. |
| Policy & legal/IP | **Project Lead** | BA | Takedowns, ToS reviews, discovery-only banner policy. |

Attendees for the **weekly**: BA (chair), Project Lead, Tech Lead, Ops Lead, Mod Lead; others by invite.

---
<br>

### 8.6 Tooling & Data Hygiene

- **Single source of truth** dashboards; versioned definitions for KPIs and thresholds.  
- **Event taxonomy** documented (names, params); avoid PII; pseudonymise IDs.  
- **Sampling & retention:** keep only what’s needed; rotate logs; respect deletion requests.  
- **Consistency checks:** automated tests for event payloads; contract tests for provider responses.  
- **Labels & tags:** every alert links to a playbook and the relevant risk/assumption ID.

---
<br>

### 8.7 Escalation & Communications

- Follow §2.5 rules (Critical >7 days, High slips twice, overdue validations >14 days).  
- Escalations go to **Project Lead** with a one-paragraph context + options + recommendation.  
- If user-visible impact is likely, prepare: status banner copy, help-desk macro, and social post stub (if needed).

---
<br>

### 8.8 Cross-Links

- **BRD:** §11 Acceptance Criteria (reporting expectations), §10 Integrations dependencies.  
- **SRS:** NFRs for availability, performance, operations; module-specific monitoring hooks.  
- **Use Cases:** INT/ACH/SAV/ALB/GOV events instrumented as part of acceptance.

---
<br>
<br>

## 9. Cross-References

This section points from the **Risks & Assumptions Register** to the most relevant project artefacts. Use these links when adding/changing entries in §4 (Risks) and §6–7 (Assumptions & Validation).

### 9.1 Primary Documents

- **Business Case** — objectives, risk summary, timeline, budget, approvals  
  [`./business-case.md`](./business-case.md)

- **Business Analysis Document (BAD)** — business rules, data view, BA-level assumptions/NFRs  
  [`./business-analysis-document.md`](./business-analysis-document.md)

- **Stakeholder & Communication Plan** — owners, escalation paths, meeting cadence  
  [`./stakeholder-and-communication-plan.md`](./stakeholder-and-communication-plan.md)

- **Business Requirements Document (BRD)** — scope, catalogue of business requirements, dependencies, acceptance criteria  
  [`./brd.md`](./brd.md)

- **System Requirements Specification (SRS)** — module behaviour, external interfaces, system NFRs, data/ops, traceability to BRD  
  [`./srs.md`](./srs.md)

- **Use Case Specification (UCS)** — actor flows and alternative/error paths by module  
  [`./use-case-specification.md`](./use-case-specification.md)

---
<br>

### 9.2 What to Link From Each Register Entry

For **Risks** (see §4 template):
- **BRD** requirement IDs affected (e.g., `BR-ACH-04`, `BR-SAV-06`).
- **SRS** system requirement IDs (e.g., `SR-ALB-03`, `SR-INT-06`, `SR-SAV-24..27`).
- **UCS** use cases touched (e.g., `UC-ACH-02`, `UC-SAV-04`, `UC-INT-01`).
- **NFRs** that constrain the fix or fallback (e.g., `NFR-INT-01`, `NFR-PERF-06`, `NFR-SEC-03`).

For **Assumptions** (see §6–7 templates):
- The **BRD** objective/requirement the assumption enables.
- The **SRS** area that would change if false (interface, data, ops).
- The **UCS** flows impacted by validation outcomes.

---
<br>

### 9.3 Quick Map for Frequent Topics

- **Provider sync & quotas (Steam/RA)**  
  BRD: Integrations/Dependencies · SRS: External Interfaces (`SR-INT-*`) · UCS: `UC-INT-01/02`

- **Save compatibility (build/manifest mapping; legacy/untested)**  
  BRD: Save features · SRS: Save Management (`SR-SAV-*`) & Catalogue (`SR-CAT-*`) · UCS: `UC-SAV-*`, `UC-CAT-*`

- **Sticker validation (AI triage, appeals)**  
  BRD: Albums · SRS: Albums (`SR-ALB-*`) & Governance (`SR-GOV-*`) · UCS: `UC-ALB-*`, `UC-GOV-*`

- **Evidence allowlist for fan achievements**  
  BRD: Achievements · SRS: Achievements/Governance (`SR-ACH-*`, `SR-GOV-*`) · NFR: Security/Privacy · UCS: `UC-ACH-*`

- **UA/RU discovery-only policy banners**  
  BAD §8 (policy scope) · BRD: Catalogue policies · SRS: Catalogue/Discovery (`SR-CAT-*`, `SR-DSC-*`) · UCS: `UC-DSC-*`

---
<br>

### 9.4 Link Hygiene Checklist

- Every **High/Critical** risk has at least one **BRD** and one **SRS** cross-link.  
- Assumptions in §6–7 reference the **validation owner** and the **downstream doc** that will change if false.  
- When a link target changes, update the cross-links **and** the change history in this register.  
- Prefer **specific IDs** over generic section names (e.g., `SR-SAV-26`, not just “Saves”).  
- If an item spans multiple modules, choose **one primary** and list others under “Secondary impacts” in the entry.

---
<br>
<br>

## 10. Change History

This log records **what changed**, **why**, and **where it links**—so future readers can trace decisions without digging through commits.

### 10.1 Versioning Rules
- **Schema:** `v<major>.<minor>[.<patch>]`  
  - **Major:** structural changes to sections/templates.  
  - **Minor:** new/retired risks or assumptions; scoring policy changes.  
  - **Patch:** copy/clarity tweaks that don’t alter meaning.
- **Entry format:** one row per decision set; include affected IDs and links.

### 10.2 Log

| Version | Date | Editor | Type | Summary of Change | Affected IDs | Links |
|---|---|---|---|---|---|---|
| v0.1 | 2025-11-20 | BA | **Create** | Initial register established (Sections 1–10); active risks and assumptions recorded; validation cadence defined. | RISK-001..012, ASM-001..010 | BRD `./brd.md`, SRS `./srs.md` |
| v0.1.1 | 2025-11-20 | BA | **Update** | Clarified **discovery-only** scope for UA/RU policy banners; tightened cross-links. | RISK-011, ASM-007, §9.3 | BAD `./business-analysis-document.md`, SRS `./srs.md` |
| v0.1.2 | 2025-11-21 | Tech Lead | **Thresholds** | Set initial KPIs & alert thresholds (sync success, AI agreement, moderation SLA, p95 search). | §8.2–8.3, RISK-001, RISK-004, RISK-005, RISK-012 | SRS NFRs in `./srs.md` |
| v0.1.3 | 2025-11-21 | Admin | **Policy** | Finalised evidence **allowlist** approach; added guidance and appeal path. | RISK-003, ASM-004, §7.2 (ASM-004) | UCS `./use-case-specification.md` |
| v0.1.4 | 2025-11-22 | Project Lead | **Decision** | Marked native mobile apps **Accepted risk** (out of MVP); write-back to Steam/RA **Closed** (non-goal). | RISK-013, RISK-014 | Business Case `./business-case.md` |
| v0.1.5 | 2025-11-23 | Tech Lead | **Mapping** | Added build/manifest **fallbacks** and community confirmation quorum language. | RISK-002, ASM-005, §4 (cross-links) | SRS Saves/Catalogue in `./srs.md` |

> When you edit any risk/assumption, **add a row** here with the IDs touched and update cross-links in §9.
