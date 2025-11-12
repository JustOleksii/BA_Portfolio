# Business Requirements Document (BRD)

## Document Structure

Below is an overview of the sections in this BRD.  
Use the links to jump directly to the part you need.

- **[1. Introduction & Purpose](#1-introduction--purpose)**  
  Explains what the BRD is for, how it relates to the Business Case and Business Analysis Document, and how it should be used by stakeholders (product, dev, QA, curators, admins).

- **[2. Business Context & Background](#2-business-context--background)**  
  Summarises the key elements of the gaming ecosystem and current pain points (saves, achievements, screenshots) that Dream Project addresses, referencing the deeper analysis in the BA document.

- **[3. Business Objectives & Success Criteria](#3-business-objectives--success-criteria)**  
  Restates the main business goals for Dream Project and the high-level success metrics that the solution must support, based on the Business Case.

- **[4. Solution Scope (In Scope / Out of Scope)](#4-solution-scope-in-scope--out-of-scope)**  
  Defines the functional and business scope of the MVP from a requirements perspective: what Dream Project will cover, what is explicitly excluded, and which items are deferred to later phases.

- **[5. Stakeholder & User Overview](#5-stakeholder--user-overview)**  
  Provides a concise view of the key stakeholder and user groups relevant to the BRD (players, retro users, curators, creators, admins, catalogue maintainers), focusing on how they drive or consume requirements.

- **[6. High-Level Product Features & Capabilities](#6-high-level-product-features--capabilities)**  
  Describes the major capability areas of Dream Project (Save Management, Achievement Tracking, Sticker Albums, Profiles & Stats, Governance & Policy) at a business level, as a bridge to detailed requirements.

- **[7. Business Requirements Catalogue](#7-business-requirements-catalogue)**  
  Lists the concrete **business-level requirements** (BR-XX) grouped by theme (e.g. Saves, Achievements, Albums, Profiles, Moderation, Localisation/Policy). Each requirement is stated in business terms, not technical design.

- **[8. Non-Functional Requirements (Business View)](#8-non-functional-requirements-business-view)**  
  Captures the key non-functional expectations that matter to the business (performance, security & privacy, reliability, usability, localisation, maintainability), summarising and structuring them for later technical specification.

- **[9. Assumptions & Constraints](#9-assumptions--constraints)**  
  Lists the assumptions and constraints that directly affect the feasibility or interpretation of the business requirements (team size, integrations, catalogue policy rules, language focus), with pointers back to the BA-level view.

- **[10. Dependencies & External Interfaces](#10-dependencies--external-interfaces)**  
  Identifies external systems, services, and data sources (e.g. achievement platforms, retro services, game metadata sources) that the business requirements rely on, and the nature of those dependencies.

- **[11. Business Acceptance Criteria](#11-business-acceptance-criteria)**  
  Defines the high-level conditions under which key stakeholders (product owner, admins, curators) would consider the MVP acceptable from a business standpoint, independent of low-level technical testing.

- **[12. Traceability & Related Artefacts](#12-traceability--related-artefacts)**  
  Explains how this BRD links to other artefacts (Business Case, BA Document, Requirements Specification, RTM) and how changes in one should propagate to others, without duplicating the full list from the README.

---
<br>
<br>


## 1. Introduction & Purpose

The **Business Requirements Document (BRD)** defines *what the business expects* Dream Project to deliver at MVP stage and in its immediate evolution.  
It translates the vision and analysis from the **Business Case** and **Business Analysis Document** into a structured list of **business-level requirements** that guide design, development, and testing.

Dream Project is a web-based companion platform that helps players:

- preserve and share **game saves**,
- track **official and fan-made achievements**,
- curate **screenshot-based sticker albums** tied to specific games.

This BRD focuses on what the solution must do from a **business and user standpoint**, not on how it will be implemented technically.

---
<br>

### 1.1 Document Context

This BRD sits in the middle of the Dream Project documentation stack:

- The **Business Case** explains *why* the project is worth doing and what success looks like at a high level.
- The **Business Analysis Document** describes *how the domain works*: stakeholders, AS-IS / TO-BE processes, rules, and data.
- The **BRD (this document)** specifies *what the solution must deliver* in terms of capabilities and behaviour that satisfy the business.
- The **System Requirements Specification (SRS) / detailed requirements** will later turn these business requirements into more granular functional and non-functional specifications (user stories, API details, UI rules, etc.).

When requirements change, this BRD is one of the key artefacts that should be updated and then reflected downstream in more detailed specifications and tests.

---
<br>

### 1.2 Purpose of the BRD

The BRD aims to:

- Provide a **single, structured list** of business requirements for Dream Project MVP, grouped by capability (saves, achievements, albums, profiles, governance, localisation/policy).
- Make explicit the **minimum behaviour** the solution must support so that:
  - players can actually use the platform for real playthroughs,
  - curators and creators can meaningfully contribute content,
  - admins can operate and moderate the platform,
  - catalogue policy and localisation goals are respected.
- Clarify **non-functional expectations** that matter to the business (e.g. trust, usability, localisation, moderation feasibility).
- Serve as a reference for:
  - product and design decisions,
  - development planning,
  - test planning and UAT criteria.

The BRD is deliberately written in **business language** so that non-technical stakeholders can review it and confirm that the described behaviour matches their expectations.

---
<br>

### 1.3 Intended Audience & Usage

The primary audience of this document is:

- **Product Owner / Business Analyst** – to manage scope and priorities and ensure traceability back to objectives.
- **Design, Development & QA Team** – to understand what the product must achieve from a business point of view before designing solutions and writing user stories.
- **Platform Administrators & Moderation Leads** – to validate that operational and policy needs are reflected in requirements.
- **Curators & Content Creators (representatives)** – to confirm that tools and workflows they rely on are covered at the business level.
- **Potential reviewers** (e.g. mentors, hiring managers) – to see how requirements are structured and justified within the overall documentation set.

Typical uses:

- As a **baseline** when creating or refining detailed requirements and user stories.
- As a **checklist** during planning and release reviews (“Which BRs are satisfied by this release?”).
- As a **reference** when new ideas appear (“Does this fit within existing business requirements, or is it a new one?”).

---
<br>

### 1.4 Scope of This Document

**In scope:**

- Business-level requirements for Dream Project MVP:
  - core capabilities (Save Management, Achievement Tracking, Sticker Albums),
  - supporting capabilities (Profiles & Stats, Governance & Moderation, Catalogue & Localisation).
- Non-functional requirements expressed in terms of **business expectations**, not low-level technical metrics.
- Dependencies on external services and policies **as they affect business behaviour**.

**Out of scope:**

- Detailed technical specifications (APIs, database schemas, infrastructure design).
- Low-level UI design (pixel-perfect layouts) beyond what is implied by requirements.
- Full test cases; only high-level **business acceptance criteria** are captured here.

---
<br>
<br>

## 2. Business Context & Background

Dream Project exists in a world where players already have **many tools** for buying, launching, and discussing games – but very few for **preserving and structuring their long-term progress** in one place.

---
<br>

### 2.1 Gaming Ecosystem Snapshot

From a business perspective, the relevant ecosystem looks roughly like this:

- **Modern PC / Console Platforms**  
  - Steam, GOG, console stores, etc. provide:
    - cloud saves tied to a specific account and platform,
    - official achievement / trophy systems,
    - screenshot capture and basic galleries.
  - They generally do **not** provide:
    - a way to browse or reuse **other players’ saves** in a structured, safe way,
    - tools to extend official achievements with **community-defined sets**,
    - album-like views that treat screenshots as structured “collectibles”.

- **Launchers & Library Aggregators (e.g. multi-store launchers)**  
  - Help users see “all my games in one place” and start them easily.  
  - Focus on **launching and storefront integration**, not on long-term progress archiving or fan-made structures.

- **Retro / Emulator Ecosystem**  
  - Emulators and retro platforms rely on:
    - local save files, memory card images, emulator states,
    - community-driven achievement ecosystems (e.g. RetroAchievements) for some titles.
  - Save sharing is typically ad hoc (forums, file hosts); achievements are tied to specific emulator setups and profiles.

- **Community Platforms (forums, Discord, guides, wikis)**  
  - Excellent for **discussion, guides, and social interaction**.  
  - Not designed as a structured, persistent store of:
    - reusable saves,
    - unified achievement structures,
    - album-like representations of playthroughs.

Taken together, the ecosystem provides **good ways to play and talk**, but no single, neutral layer that **preserves progress and fan-made structures across platforms and over time**.

---
<br>

### 2.2 Problem Recap (from a Business Perspective)

Key pain points that drive the need for Dream Project:

1. **Saves are fragile and siloed**
   - Cloud saves help individual players on a single platform, but:
     - they are not designed for sharing scenarios (e.g. “give me a save just before this route split”),
     - retro and emulator saves remain mostly local and scattered,
     - players who switch platforms or hardware often cannot reuse old progress.
   - As a result, there is no trusted, searchable “library” of **contextual saves** that players can tap into.

2. **Achievements do not fully reflect how communities play**
   - Official achievement systems often:
     - cover only basic milestones (main story, a few side activities),
     - lack detailed coverage of all mechanics, routes, and challenges.
   - Fan-made sets (especially in retro ecosystems) are richer, but:
     - live on separate sites or tools,
     - are not consistently combined with official achievements,
     - rely heavily on per-platform or per-emulator setups.
   - There is no neutral place where:
     - official progress and fan-made structures are viewed together,
     - players can see **completion stats** and personal history across sets.

3. **Screenshots are not treated as structured “collectibles”**
   - Modern platforms allow capturing and storing screenshots, but mostly as:
     - unsorted galleries or timelines.
   - Retro players share images via forums, chats, or generic image hosts.
   - There is **no widely used concept** of:
     - screenshot-based albums where each “slot” has a clear meaning,
     - completion logic based on filling those slots,
     - light spoiler handling for new players.

From a business standpoint, this means:

- Valuable player effort (saves, challenges, screenshots) is **hard to reuse**, **easy to lose**, and **difficult to present** in a coherent way.
- Communities and curators lack a stable, structured platform for the **fan-made layer** that lives on top of games.

---
<br>

### 2.3 Why Dream Project & Why Now

Dream Project’s role in this context is to be a **specialised companion layer** that:

- **Unifies progress signals** from different sources into a single view per player and per game:
  - saves (original and shared),
  - official achievements (where integrations allow),
  - retro / external achievements (via read-only links),
  - fan-made sets and sticker albums.

- **Provides tools for communities and curators** to:
  - define richer completion structures (achievement sets and album templates),
  - rely on consistent rules (spoilers, evidence, policies),
  - see usage and completion statistics for their creations.

- **Respects platform boundaries and policies** by:
  - treating external platforms as **sources of data**, not something to be modified or automated against in risky ways,
  - focusing on read-only imports and self-reported progress with optional evidence.

For the BRD, this context implies that business requirements must:

- Support **realistic MVP usage** within this ecosystem (no assumptions of deep, proprietary integrations that may not be feasible).
- Capture the needs of **players**, **curators**, **admins**, and **Ukrainian-focused catalogue policies** in a way that can be implemented step by step.
- Provide a solid basis for later phases without over-promising what the first release can do.

---
<br>
<br>

## 3. Business Objectives & Success Criteria

This section restates the main business goals for Dream Project and the success criteria the MVP should meet.  
It is aligned with the **Business Case** but framed here specifically as inputs to the business requirements.

---
<br>

### 3.1 Business Objectives

Over roughly the first **four months after launching the MVP**, Dream Project aims to achieve four primary business objectives:

1. **Deliver a usable MVP around three core pillars**  
   Provide a minimum viable product that demonstrates the value of:
   - **Save Management** – players can upload, store, describe, and reuse their own saves and safely discover saves shared by others for specific games and supported platforms.
   - **Achievement Tracking** – players can view official achievements (where integrations allow) and interact with curated fan-made achievement sets in a structured way.
   - **Sticker Albums** – players can fill screenshot-based “sticker” albums for specific games, using curated templates that treat screenshots as meaningful collectibles rather than random images.

   The goal is not to cover every future idea, but to deliver a coherent end-to-end experience that players can actually use in real playthroughs.

2. **Build an initial cross-language early community**  
   Attract a small but active set of early adopters who:
   - regularly use at least one of the three pillars (saves, achievements, albums),
   - include both English-speaking and Ukrainian-speaking users,
   - are willing to share feedback and help refine the platform.

3. **Validate engagement with achievements and sticker albums**  
   Demonstrate that players:
   - interact with curated sticker album templates (not just raw screenshots),
   - log meaningful achievement progress (official, retro, or fan-made sets),
   - use Dream Project as a place to **return** to for tracking and visualising completion, not just as a one-off curiosity.

4. **Form a core group of curators and set/album creators**  
   Establish a small group of motivated curators who:
   - create and maintain fan-made achievement sets and sticker album templates,
   - help define quality standards and best practices,
   - represent both global/English-speaking and Ukrainian communities.

---
<br>

### 3.2 Success Criteria (MVP)

The following criteria summarise what “success” looks like for the MVP from a business perspective.  
Numbers are indicative and may be refined as more information appears, but they should be concrete enough to guide planning.

---
<br>

#### 3.2.1 MVP Delivery & Usability

- The MVP including **Save Management**, **Achievement Tracking**, and **Sticker Albums** is deployed and accessible to external users (not only the internal team).
- Core flows are usable end-to-end without critical defects:
  - upload and manage personal saves,  
  - discover and use shared saves for selected games,  
  - browse achievement sets and record progress,  
  - start and progress at least one album per supported game.
- A lightweight onboarding or in-product hinting mechanism exists to help new users understand at least **one clear starting action** (e.g. “pick a game and see what you can do”).

---
<br>

#### 3.2.2 Community Growth & Activity

Within approximately four months after launch:

- Around **100 registered users** have performed at least one meaningful action (e.g. uploading a save, marking achievement progress, filling album slots) within a rolling 30-day period.
- At least **half** of these active users return at least once after their first week (early sign of retention).
- The community mix is roughly:
  - **≈75% English-speaking**,  
  - **≈25% Ukrainian-speaking**,  
  showing both global reach and a tangible Ukrainian presence.

---
<br>

#### 3.2.3 Engagement with Achievements & Sticker Albums

Within the same period:

- At least **50 sticker album templates** exist for **distinct games**, created primarily by curators (not just the core team).
- A non-trivial portion of these templates show real use:
  - multiple users have filled several slots,  
  - at least some users have completed full albums and earned corresponding badges.
- At least **400 achievement completion records** are stored across:
  - official achievements (where integrations allow),
  - imported retro/external sets (where supported),
  - and Dream Project–native fan-made sets.
- A visible share of active users (for example, on the order of 30–40%) interact with **both** achievements and albums, indicating that the pillars reinforce each other rather than living in isolation.

---
<br>

#### 3.2.4 Curator & Creator Ecosystem

- A **basic curator flow** exists:
  - active contributors can be identified and promoted to curator status,
  - they receive onboarding material and have the tools needed to create and maintain sets/albums.
- At least **5 users** are recognised as regular curators (beyond the core team) and have created or significantly updated content (sets or albums).
- At least **10 fan-made achievement sets or subsets** are available for different games, with:
  - clear descriptions,
  - optional hints or linked resources (e.g. GameFAQs, YouTube tutorials),
  - tagging that allows players to understand what type of run they represent.
- At least **3 of these curators** are active within or close to the Ukrainian community, ensuring that Dream Project supports both global and Ukrainian audiences.

---
<br>

These objectives and criteria are guidance rather than strict contractual targets.  
They define what “good enough” looks like for an MVP that is worth iterating on, and they provide a baseline against which the detailed business requirements in later sections can be prioritised and validated.

---
<br>
<br>

## 4. Solution Scope (In Scope / Out of Scope)

This section defines what the **Dream Project MVP** is expected to cover from a business perspective, and what is explicitly excluded or deferred to later phases.

---
<br>

### 4.1 Overall Scope

The MVP focuses on delivering a **usable, end-to-end slice** of the Dream Project vision across three pillars:

1. **Save Management**
2. **Achievement Tracking**
3. **Sticker Albums**

with supporting capabilities for:

- **Profiles & basic statistics**
- **Governance, moderation & catalogue policy**
- **Lightweight discovery and navigation**

The goal is not to be feature-complete, but to provide enough functionality that real players and curators can use the platform in actual playthroughs and give meaningful feedback.

---
<br>

### 4.2 In Scope (MVP)

#### 4.2.1 Save Management

- Users can:
  - upload and manage their own save files for supported games and platform variants,
  - add basic metadata (game, platform, short description, approximate position in the game),
  - choose whether saves are private or shared.
- Other users can:
  - search and filter shared saves for supported games (e.g. by platform, rough progress point),
  - download and use those saves with appropriate warnings and guidance.
- The system:
  - stores save entries with essential metadata and basic versioning (e.g. multiple saves per game),
  - supports reporting of problematic saves (corrupt, malicious, misleading).

  ---
<br>

#### 4.2.2 Achievement Tracking

- **Official achievements:**
  - Where feasible for MVP, players can view official achievements for selected games, either:
    - imported via read-only integrations, or
    - configured using structured metadata if direct integration is not available.
- **Retro / external sets:**
  - For at least one retro ecosystem, players can:
    - link their external profile,
    - see completion status for imported sets in a read-only way.
- **Dream Project fan-made sets:**
  - Curators can create and maintain structured achievement sets and subsets for specific games.
  - Players can:
    - browse these sets,
    - mark progress or completion on individual items,
    - optionally attach evidence (screenshots, saves, external clip links).

---
<br>

#### 4.2.3 Sticker Albums

- Curators can:
  - define **sticker album templates** for specific games (and optionally platform variants),
  - create different album “types” (e.g. first-playthrough-friendly vs. post-completion / deep dive),
  - specify sticker slots with descriptions and spoiler sensitivity.
- Players can:
  - start an album instance based on a template,
  - upload screenshots into specific slots,
  - see their completion status and earned album-specific badges.
- The system:
  - applies basic AI-assisted checks (e.g. consistency with slot description, content safety),
  - supports spoiler handling (e.g. blur or warnings for spoiler-marked slots).

---
<br>

#### 4.2.4 Profiles & Basic Statistics

- Each user has a **profile** showing:
  - nickname, avatar, and basic visibility settings,
  - high-level stats (e.g. number of completed sets, number of completed albums),
  - a history or summary of recent notable actions (e.g. completed album, completed set).
- For achievements and albums, the system maintains and can display:
  - aggregate completion percentages,
  - lists of users who completed a set/album (with nicknames and completion timestamps),
  - approximated “time to completion” where data is available.

---
<br>

#### 4.2.5 Governance, Moderation & Catalogue

- **Roles and permissions**:
  - Regular users, Curators, and Platform Administrators are supported roles with distinct capabilities.
- **Moderation workflows**:
  - Users can report saves, screenshots, achievements, sets, albums, and comments.
  - Admins can review reports and apply actions (hide/remove content, warn or restrict user).
- **Catalogue & policy-related scope**:
  - A basic game catalogue exists with:
    - games and platform variants,
    - tags for genre, retro/modern, and other simple descriptors.
  - A first version of localisation and origin metadata:
    - flags for Ukrainian localisation where known,
    - flags for Ukrainian-origin games,
    - flags for Russian-linked publishers/developers where policy defines this.
  - Filters and indicators that:
    - allow Ukrainian users to highlight UA-relevant content,
    - allow users to be informed about Russian-linked content according to platform policy.

---
<br>

#### 4.2.6 Discovery & Navigation (Basic)

- Users can:
  - search for games within the catalogue,
  - navigate to game-specific pages that show:
    - available saves,
    - achievement sets (official, retro, fan-made),
    - sticker album templates.
- A **simple recommendation mechanism** is in scope conceptually (even if basic), such as:
  - suggesting a game and feature (save/set/album) based on:
    - user history, or
    - general popularity and curated picks.
- Users can decline recommendations and choose another suggested game.

---
<br>

### 4.3 Out of Scope (MVP)

The following items are explicitly **out of scope for the MVP**, even if they may be considered later:

#### 4.3.1 Deep or Risky Integrations

- Direct hooks into running games on proprietary platforms (e.g. overlays, injections, unofficial SDKs).
- Automatic “live” detection of achievements or in-game events on consoles or PC without official support.
- Any behaviour that risks breaching the terms of service of external platforms or triggering user bans.

---
<br>

#### 4.3.2 Complex Social & Competitive Features

- Full social graph (friends/following system) beyond basic profile viewing.
- Real-time chat, global activity feeds, or complex notification systems.
- Tournaments, contests, or competitive events with prizes between users.
- Global leaderboards outside of simple completion statistics.

---
<br>

#### 4.3.3 Advanced Save Manipulation

- Automatic cross-platform save conversion (e.g. transforming a PS2 save into PC format).
- Emulation front-end features (launching games directly from Dream Project).
- Automated backup/restore tools integrated into game launchers or OS-level save management.

---
<br>

#### 4.3.4 Native Applications & Heavy Offline Support

- Native mobile or desktop applications (MVP is **web-based only**).
- Offline use cases beyond basic caching of static assets.

---
<br>

#### 4.3.5 Full Analytics / Enterprise-Grade Reporting

- Deep segmentation, cohort analysis, or complex BI dashboards.
- Exportable enterprise reports and extensive custom analytics tooling.

---
<br>

### 4.4 Phase-II+ Candidates (Not Committed for MVP)

These items are **not in scope for MVP**, but may be relevant for future phases if MVP engagement validates them:

- Richer **recommendation engines** (e.g. personalised game and route suggestions based on behaviour).
- Additional roles (e.g. dedicated “Community Manager” role inside the platform).
- More advanced integrations with multiple external achievement services and launchers.
- More sophisticated AI usage (e.g. auto-detecting game moments in uploaded clips, advanced fraud detection).

For BRD purposes, the requirements in later sections should be read with this scope in mind: MVP must support solid, realistic functionality around the three pillars and supporting governance, **without** drifting into these more complex or risky areas in the first release.

---
<br>
<br>

## 5. Stakeholder & User Overview

This section gives a **concise view** of the key stakeholder and user groups relevant to the BRD.  
Detailed profiles are maintained in the Business Analysis Document and Stakeholder & Communication Plan.

---
<br>

### 5.1 Stakeholder Summary

| Group                                             | Type        | Primary Role in Requirements                                                   | Priority |
|---------------------------------------------------|------------|--------------------------------------------------------------------------------|----------|
| Regular Players (Platform Users)                  | External    | Everyday users of saves, achievements, and albums                             | High     |
| Retro & Emulator-Focused Players                  | External    | Early adopters for retro integrations and advanced save use                   | High     |
| Curators & Achievement/Album Set Creators         | External    | Power users who create and maintain sets and album templates                  | Very High|
| Global Content Creators & Community Organisers    | External    | Use Dream Project as evidence/context in events and content                   | Medium   |
| Ukrainian Content Creators & Community Organisers | External    | Drive UA-language adoption and UA-specific catalogue expectations             | High     |
| Platform Administrators & Moderation Leads        | Internal    | Operate moderation, roles, and catalogue policy                               | Very High|
| Catalogue / Data Maintainers                      | Internal    | Maintain game catalogue and sensitive metadata (origin, localisation, flags)  | High     |
| Product Owner / Business Analyst                  | Internal    | Owns product vision, requirements, and trade-offs                             | Very High|
| Design, Development & QA Team                     | Internal    | Implement and test requirements                                               | Very High|
| External Services & Integration Partners          | External    | Provide achievement/metadata APIs; constrain and enable integrations          | Medium   |

---
<br>

### 5.2 Primary User Segments (External)

#### 5.2.1 Regular Players (Platform Users)

- **Who**  
  - Players mainly on modern platforms (Steam, GOG, consoles) who try Dream Project as a companion.

- **What they need from the product**  
  - Simple, safe flows to:
    - upload and reuse their own saves,
    - find and use useful shared saves for supported games,
    - track progress in official / basic fan-made achievement sets,
    - fill straightforward sticker albums without complex setup.
  - Clear explanations about:
    - what happens to their data,
    - how to avoid losing progress when using shared saves.

- **Implications for requirements**  
  - Requirements must favour **clarity and simplicity** in core flows.  
  - MVP cannot assume deep technical knowledge of file systems, emulators, or APIs.

---
<br>

#### 5.2.2 Retro & Emulator-Focused Players

- **Who**  
  - Players invested in retro systems, emulation, and retro achievement ecosystems.

- **What they need from the product**  
  - Read-only integrations with at least one retro achievement service.
  - Structured ways to:
    - upload/share retro saves,
    - see retro achievement progress alongside Dream Project content,
    - document and show off long, difficult runs.

- **Implications for requirements**  
  - Requirements must define:
    - how external retro profiles are linked and synchronised,
    - how imported progress is displayed,
    - how retro saves and sets are distinguished from official and Dream Project–native ones.
  - The product must avoid any behaviour that risks bans or ToS violations.

---
<br>

#### 5.2.3 Curators & Achievement/Album Set Creators

- **Who**  
  - Power users who create and maintain fan-made achievement sets and sticker album templates.

- **What they need from the product**  
  - Tools to:
    - define, edit, and version sets and templates,
    - mark spoiler sensitivity,
    - configure hints and optional external resources (e.g. guides, videos),
    - see basic usage stats (starts/completions).
  - Clear rules about:
    - quality expectations,
    - evidence for achievement completion,
    - how their content is surfaced and attributed.

- **Implications for requirements**  
  - Requirements must cover:
    - curator role and permissions,
    - templates and versioning behaviour,
    - statistics and visibility for curated content,
    - workflows for dealing with reports and corrections.

---
<br>

#### 5.2.4 Global & Ukrainian Content Creators / Community Organisers

- **Who**  
  - EN creators: streamers, writers, organisers for global audiences.  
  - UA creators: similar roles, but focused on Ukrainian-language communities and context.

- **What they need from the product**  
  - Stable links and views to:
    - specific achievement sets and albums,
    - players’ progress and completion stats.
  - For Ukrainian creators specifically:
    - filters and flags for UA localisation and UA-origin games,
    - clear handling of Russian-linked content,
    - key elements of the UI and docs available in Ukrainian.

- **Implications for requirements**  
  - Requirements must include:
    - shareable URLs for game pages, sets, albums, and profiles (respecting privacy),
    - basic UA-specific catalogue and localisation features,
    - predictable behaviour of filters and flags across the product.

---
<br>

### 5.3 Internal Stakeholders (Operators & Implementers)

#### 5.3.1 Platform Administrators & Moderation Leads

- **Who**  
  - People responsible for user roles, content moderation, and policy enforcement.

- **What they need from the product**  
  - Moderation tools:
    - queues for reported content,
    - quick actions (hide/remove/warn),
    - ability to see context around reports.
  - Policy support:
    - consistent application of catalogue rules,
    - visibility into Russian-linked and UA-origin flags,
    - logs of decisions for future reference.

- **Implications for requirements**  
  - Requirements must explicitly define:
    - reporting flows for users,
    - moderation actions and states,
    - how policy-related flags affect visibility and filters.

---
<br>

#### 5.3.2 Catalogue / Data Maintainers

- **Who**  
  - People curating the game catalogue and key metadata.

- **What they need from the product**  
  - Tools to:
    - create and edit game and platform entries,
    - manage tags and sensitive flags (localisation, origin, Russian-linked),
    - import and correct data from external sources where applicable.

- **Implications for requirements**  
  - Requirements must cover:
    - game/platform maintenance flows,
    - validation rules for sensitive fields,
    - minimal auditability (who changed what and when) for high-impact metadata.

---
<br>

#### 5.3.3 Product Owner / BA, Design, Development & QA

- **Who**  
  - Core team responsible for defining, designing, implementing, and testing Dream Project.

- **What they need from the product (in BRD terms)**  
  - A clear set of **business requirements** that:
    - align with the Business Case and BA analysis,
    - are testable from a business point of view,
    - are stable enough to support sprint-level planning.

- **Implications for requirements**  
  - Requirements must be:
    - written in business language but precise,
    - structured (e.g. BR-IDs) for traceability,
    - linked to objectives and later to detailed SRS and tests.

---
<br>

### 5.4 External Services & Integration Partners

- **Who**  
  - Operators of external achievement systems and game metadata sources.

- **What they need from the product**  
  - Predictable, respectful use of their APIs and data:
    - read-only access patterns,
    - adherence to rate limits and attribution requirements.
  - Simple explanation of:
    - how their data appears in Dream Project,
    - what users will see and expect.

- **Implications for requirements**  
  - Requirements must:
    - describe the expected business behaviour when integrations are available, degraded, or removed,
    - ensure there is a graceful fallback (manual or partial data) rather than hard failure where possible.

---
<br>

This overview keeps the focus on **how each group shapes what the product must do**, without repeating the full stakeholder analysis.  
The next sections translate these needs into high-level features and concrete business requirements.

---
<br>
<br>

## 6. High-Level Product Features & Capabilities

This section describes the major **capability areas** of Dream Project at a business level.  
Later, Section 7 will break these into concrete business requirements (BR-XX).

---
<br>

### 6.1 Save Management

**Goal:**  
Allow players to **preserve**, **describe**, and **reuse** game progress – their own and, where they choose, that of other players – for games where this is realistically possible.

**Key capabilities (business view):**

- **Personal save library**
  - Users can upload save files for supported games and platform variants.
  - Saves are stored with essential metadata (game, platform, rough progress point, short description).
  - Users control whether each save is private or shared (where the game/platform actually supports meaningful save sharing).

- **Shared saves discovery**
  - For games and platform variants where saves can be safely exchanged, users can:
    - browse and search saves shared by other players,
    - filter by basic criteria (e.g. early/mid/late game, “before boss”, “after route choice”).
  - Game pages clearly indicate when **save sharing is supported** and when it is **not**.

- **Clear indication when save sharing is not viable**
  - Some games will **not support practical or safe save transfer**, for example:
    - online-only or strongly multiplayer-focused titles where progress is stored server-side,
    - games where progress is tightly bound to a specific online profile and cannot be reused on another account,
    - titles that use platform-level encryption or mechanisms that make third-party saves invalid or unsafe,
    - games whose design or anti-cheat protections explicitly forbids or breaks external save usage.
  - For such games and platform variants, Dream Project:
    - may still allow **personal save backup** (if meaningful), but
    - clearly tells the user that **save sharing with others is unavailable or limited** for that game,
    - emphasises that other pillars (achievements, albums) are still available where supported.

- **Guided, safe usage**
  - The platform provides basic guidance on:
    - how to download and use a save (for supported games),
    - what the risks are (e.g. overwriting local progress, incompatibility across versions/platforms),
    - how to keep their own progress backed up before experimenting with shared saves.

- **Community quality signal**
  - Users can report problematic saves (corrupt, misleading, harmful).
  - Administrators can review, hide, or remove saves based on these reports and policies.
  - Where feasible, saves for a given game/platform can be labelled as:
    - “community-verified” (widely used without issues),
    - “use with caution” (known quirks or limitations).

---
<br>

### 6.2 Achievement Tracking

**Goal:**  
Provide a place where players can see and track their **official**, **retro**, and **fan-made** achievement progress in one structured view.

**Key capabilities:**

- **Official achievement overview**
  - For selected games, Dream Project can show official achievements / trophies:
    - imported via read-only integration where possible, or
    - configured using structured metadata if direct integration is not available.
  - Players see which official achievements they have completed (where data is available).

- **Retro / external set integration**
  - For at least one retro achievement ecosystem, players can:
    - link their external profile,
    - see up-to-date progress on imported sets,
    - understand which achievements and sets originate from that external service.

- **Dream Project fan-made sets**
  - Curators can define structured achievement sets and specialised subsets for specific games (e.g. “low-level run”, “no-death route”).
  - Players can:
    - view and join these sets,
    - mark achievements as in progress or completed,
    - optionally attach evidence (screenshot, save, external video link).

- **Completion and statistics**
  - For each set:
    - completion percentage per user,
    - list of users who completed the set (nickname, completion timestamp),
    - approximate time from starting to completing the set where data is available.

---
<br>

### 6.3 Sticker Albums

**Goal:**  
Turn screenshots into **structured, collectible albums**, where each slot represents a specific game moment or theme.

**Key capabilities:**

- **Curated album templates**
  - Curators can create templates for sticker albums per game (and optionally platform variant).
  - Templates define:
    - which slots exist,
    - what each slot represents (boss, location, ending, secret, etc.),
    - spoiler sensitivity and hints.

- **Album instances for players**
  - Players can start an album based on a template.
  - For each slot, they can upload a screenshot they believe matches the description.
  - Players see their personal completion status (e.g. 7/20 slots filled).

- **Spoiler-aware design**
  - Templates may be:
    - “first-playthrough-friendly” (minimal spoilers, subtle hints),
    - “post-completion / deep dive” (explicit descriptions).
  - Spoiler-marked slots can be blurred or hidden until the user chooses to reveal them.

- **Validation & safety**
  - Basic AI-assisted checks help:
    - detect obviously wrong or unrelated images,
    - flag potentially inappropriate content.
  - Admins can review flagged content and act accordingly.

- **Album completion & recognition**
  - Completing an album grants:
    - a game-specific badge on the user’s profile,
    - inclusion in completion statistics for that album (who completed it and when).

---
<br>

### 6.4 Profiles & Progress View

**Goal:**  
Provide players with a **coherent view of their own progress** across saves, achievements, and albums, and allow others to see selected highlights.

**Key capabilities:**

- **User profile**
  - Each user has a profile with:
    - nickname and avatar,
    - visibility settings (e.g. public profile vs limited visibility),
    - high-level progress stats (completed sets, completed albums, notable milestones).

- **Progress summary**
  - For individual games:
    - combined view of saves, achievement sets, and sticker albums for that user.
  - For the user overall:
    - recent actions feed (e.g. “Completed album X”, “Finished set Y”),
    - badges and notable completions.

- **Privacy-aware display**
  - Users can control:
    - which parts of their progress and content are visible to others,
    - whether certain items (e.g. specific saves or screenshots) remain private.

---
<br>

### 6.5 Governance, Moderation & Catalogue / Localisation

**Goal:**  
Ensure that the platform remains **safe, consistent, and aligned** with catalogue and localisation policies, including the Ukrainian context.

**Key capabilities:**

- **Roles & permissions**
  - Distinct roles for:
    - Regular Users (default),
    - Curators,
    - Platform Administrators.
  - Each role has clearly defined permissions (content creation, moderation, catalogue changes).

- **Moderation workflow**
  - Users can report:
    - saves, screenshots, sets, albums, comments, profiles.
  - Admins have:
    - a moderation queue,
    - actions (keep, hide, remove, warn or restrict user),
    - minimal logging of decisions for future reference.

- **Catalogue management**
  - Basic game catalogue with:
    - game entries and platform variants,
    - key tags (genre, retro/modern).
  - Additional metadata for:
    - Ukrainian localisation availability,
    - Ukrainian-origin games,
    - Russian-linked publishers/developers (according to policy).

- **Localisation & policy behaviour**
  - Ukrainian users can:
    - filter or highlight games with UA localisation or UA origin,
    - see contextual information about Russian-linked content based on platform settings.
  - Policy rules for these flags are applied consistently throughout search and game pages.

---
<br>

### 6.6 Discovery & Lightweight Recommendations

**Goal:**  
Help users **find a game and a feature to try**, even if they arrive without a specific plan.

**Key capabilities:**

- **Game search & navigation**
  - Users can search and browse the catalogue.
  - Game pages show, in one place:
    - available saves (or a clear indication if save sharing is not supported for that game/platform),
    - achievement sets (official, retro, fan-made),
    - sticker album templates.

- **Simple recommendation flow**
  - For users unsure of what to do:
    - the platform can suggest a game and a starting action (e.g. “Try this album for Game X” or “Check shared saves for Game Y”, only if those pillars are actually available).
    - suggestions can be based on:
      - basic popularity,
      - curated picks,
      - or simple signals from the user’s past activity.
  - Users can reject a suggestion and ask for another until they find something that fits.

  ---
<br>

These high-level capabilities define **what Dream Project must be able to do** at MVP level.  
In the next section, they will be transformed into **explicit business requirements** grouped by area (saves, achievements, albums, profiles, governance, localisation, and discovery).

---
<br>
<br>

## 7. Business Requirements Catalogue

This section lists the **business-level requirements (BRs)** for Dream Project MVP, grouped by capability area.  
Each requirement is written in business language and marked with an indicative **Priority**:

- **M** – Must-have for MVP  
- **S** – Should-have for MVP (valuable; MVP can work in a simpler form)  
- **N** – Nice-to-have (candidate for later phases)

IDs are structured as `BR-<AREA>-NN`.

---
<br>

### 7.1 User Accounts, Registration & Roles

**BR-ACC-01 – Account Registration & Sign-in**  
**Priority:** M  
The system shall allow users to create an account, sign in, and sign out using a secure authentication mechanism (e.g. email + password or supported identity providers), and shall provide a way to recover access if credentials are lost.

**BR-ACC-02 – Default Role & Profile Creation**  
**Priority:** M  
When a new account is successfully registered, the system shall automatically:

- create a user profile, and  
- assign the **Regular User** role as the default primary role.

All features that involve uploading content, tracking progress, or linking external profiles shall require a signed-in user account.

**BR-ACC-03 – Role Visibility & Management**  
**Priority:** M  
The system shall:

- store exactly one **primary role** per user (Regular User, Curator, or Admin),  
- display the current role to the user within their profile or account settings, and  
- allow Administrators to change a user’s role (promotion or revocation) via an internal management interface.

**BR-ACC-04 – Curator Role Request & Approval Workflow**  
**Priority:** S  
Regular Users should be able to submit a **request to become a Curator**. The system should:

- provide a simple application form (e.g. motivations, examples of contributions),  
- send these requests to Administrators,  
- allow Admins to approve or reject the request, changing the user’s role accordingly, and  
- notify the user of the decision.

**BR-ACC-05 – Activity-Based Curator Suggestions**  
**Priority:** N  
Optionally, the system may surface “candidate curators” to Administrators based on criteria such as content quality, volume of contributions, and community feedback, to support proactive curator recruitment.

---
<br>

### 7.2 Save Management Requirements

**BR-SAV-01 – Personal Save Upload & Metadata**  
**Priority:** M  
The system shall allow signed-in users to upload save files for supported games and platform variants and record essential metadata (game, platform variant, short description, approximate progress/segment, upload date).

**BR-SAV-02 – Save Visibility & Ownership**  
**Priority:** M  
The system shall allow users to classify each save as *private* or *shared*, and only the save owner (and Admins) shall be able to change its metadata or visibility.

**BR-SAV-03 – Multiple Saves per Game & Basic Versioning**  
**Priority:** M  
The system shall allow users to maintain multiple saves per game/platform variant and present them in a chronological list so that players can recognise different points in their playthrough.

**BR-SAV-04 – Shared Saves Discovery (Supported Games Only)**  
**Priority:** M  
For games and platform variants where save transfer is viable, the system shall allow users to browse and search shared saves, with filters such as platform variant and rough progress segment (e.g. early/mid/late game, before/after key points).

**BR-SAV-05 – Save Sharing Availability Indicator**  
**Priority:** M  
The system shall clearly indicate on game/platform pages whether **save sharing is supported, limited, or not viable**, based on known characteristics (e.g. online-only/server-side progression, strong account binding, anti-cheat constraints).

**BR-SAV-06 – Save Usage Guidance & Warnings**  
**Priority:** M  
When users download or prepare to use a shared save, the system shall present concise guidance on safe usage (e.g. backing up current saves, version compatibility) and highlight any known limitations or risks for that game/platform.

**BR-SAV-07 – Save Reporting & Moderation**  
**Priority:** M  
Users shall be able to report shared saves as corrupt, misleading, malicious, or otherwise problematic; reported saves shall enter a moderation workflow where Admins can review, hide/remove them, and optionally warn or restrict the uploader.

**BR-SAV-08 – Optional Save Quality Signals**  
**Priority:** S  
Where sufficient usage data exists, the system should surface simple quality signals for shared saves (e.g. number of successful downloads, absence of issues, “community-verified” or “use with caution” labels).

---
<br>

### 7.3 Achievement Tracking Requirements

**BR-ACH-01 – Official Achievement Overview (Steam & Similar Platforms Where Feasible)**  
**Priority:** M  
For selected games on modern platforms (with **Steam** as a primary example), the system shall allow users to view a structured list of official achievements/trophies and, where integration or configuration allows, see which ones they have completed.

**BR-ACH-02 – Distinguishing Achievement Origins**  
**Priority:** M  
The system shall clearly distinguish between **official achievements** (e.g. Steam trophies), **retro/external achievements**, and **Dream Project fan-made achievements** in both UI and data structures.

**BR-ACH-03 – External Gaming & Achievement Profile Linking (Steam and Retro/External)**  
**Priority:** M  
The system shall allow users to link accounts or profiles from at least one modern gaming platform (e.g. Steam) and at least one retro/external achievement ecosystem (e.g. RetroAchievements) in a secure, read-only way, so that their official and retro achievement progress can be reflected within Dream Project.

**BR-ACH-04 – External Progress Display (Official & Retro)**  
**Priority:** M  
For linked external profiles (modern platforms like Steam and retro/external services), the system shall display imported achievement sets and completion status as read-only data and make clear to users which data originates from which external service.

**BR-ACH-05 – Dream Project Fan-Made Set Creation & Maintenance**  
**Priority:** M  
Curators shall be able to create, edit, and version fan-made achievement sets for specific games (and optionally platform variants). For each set, Curators shall be able to define:

- set name and description,  
- grouping of individual achievements,  
- optional tags (e.g. “low-level run”, “no-death route”, “collectibles”),  
- optional hints or external resources (e.g. links to GameFAQs, YouTube guides),  
- whether the set is intended for first playthroughs or advanced/challenge play.

Curators may share maintenance of a set (e.g. co-maintainers) under platform policies.

**BR-ACH-06 – Achievement Item Structure**  
**Priority:** M  
Each achievement item (official, retro/external, or fan-made) shall have at least:

- a name,  
- a description and/or criteria text,  
- an origin/source label (official, external service X, Dream Project fan-made),  

and may support optional attributes such as:

- flags (e.g. “storyline”, “missable/irreversible”, “final completion indicator”),  
- tags (e.g. “combat”, “exploration”, “speedrun”),  
- linked resources (guides, videos).

**BR-ACH-07 – Player Progress Tracking in Fan-Made Sets**  
**Priority:** M  
Users shall be able to track their progress in Dream Project fan-made sets by marking individual achievements as not started, in progress, or completed.

**BR-ACH-08 – Evidence Attachment for Achievements**  
**Priority:** S  
For selected achievements in fan-made sets, the system should allow users to attach evidence (e.g. screenshot, save entry, external clip URL) to their completion record to support verification where desired.

**BR-ACH-09 – Dedicated Achievement Item View**  
**Priority:** S  
The system should provide a dedicated view for each achievement item that can display:

- description and criteria,  
- flags and tags,  
- attached evidence (where linked),  
- curator-provided hints and external resources,  
- related sticker slots or recommended screenshots (where applicable).

**BR-ACH-10 – Comments on Achievements & Sets**  
**Priority:** S  
Users should be able to comment on:

- entire achievement sets, and  
- individual achievement items,

with comments appearing as threaded discussions subject to moderation rules.

**BR-ACH-11 – Rating & Quality Signals for Fan-Made Sets**  
**Priority:** S  
The system should allow users to rate fan-made achievement sets and surface basic quality signals (e.g. average rating, number of ratings), and low-rated or heavily flagged sets should be more prominent in moderation workflows.

**BR-ACH-12 – Completion Statistics per Set**  
**Priority:** M  
For each achievement set (official, external/retro where data allows, and fan-made), the system shall be able to display:

- percentage of users who have completed the set,  
- list of users who completed it (nickname and completion timestamp),  
- where possible, an approximate “time to completion” between first engagement and completion.

---
<br>

### 7.4 Sticker Album Requirements

**BR-ALB-01 – Sticker Album Template Management**  
**Priority:** M  
Curators shall be able to create and maintain sticker album templates for specific games (and optionally platform variants), defining the album title, description, type (e.g. first-playthrough-friendly vs post-completion), and overall structure.

**BR-ALB-02 – Sticker Slot Definitions**  
**Priority:** M  
Within a template, Curators shall be able to define individual sticker slots with:

- a name or label,  
- a description of the moment or theme,  
- spoiler sensitivity level,  
- optional hints to guide screenshot capture,  
- optional tags (e.g. “boss”, “location”, “ending”, “secret”).

**BR-ALB-03 – Player Album Instances**  
**Priority:** M  
Users shall be able to create their own album instance from a template, see all defined slots, and track which slots are empty, filled, or accepted.

**BR-ALB-04 – Screenshot Upload & Slot Filling**  
**Priority:** M  
Users shall be able to upload one or more screenshots to fill a particular slot in their album instance and replace or update that screenshot later if needed.

**BR-ALB-05 – Spoiler-Aware Presentation**  
**Priority:** M  
The system shall respect spoiler sensitivity defined by Curators by:

- blurring or hiding spoiler slots and their descriptions until a user explicitly opts to reveal them, and  
- offering non-spoiler hints in first-playthrough-friendly templates where applicable.

**BR-ALB-06 – Basic AI-Assisted Validation & Safety Checks**  
**Priority:** S  
The system should apply basic AI-assisted or rule-based checks on uploaded screenshots to:

- detect obviously unrelated or inappropriate images,  
- flag questionable content for moderator review.

**BR-ALB-07 – Album Completion & Badges**  
**Priority:** M  
When a user fills all required slots in an album (subject to validation rules), the system shall:

- mark the album as completed,  
- award a game-specific badge on the user’s profile,  
- include the completion in album statistics.

**BR-ALB-08 – Album Completion Statistics**  
**Priority:** M  
For each album template, the system shall provide:

- completion percentage across users,  
- a list of users who completed the album (nickname and completion timestamp),  
- basic counts (e.g. number of started vs completed instances).

---
<br>

### 7.5 Profile & Progress View Requirements

**BR-PRO-01 – User Profile & Basic Settings**  
**Priority:** M  
Each user shall have a profile with:

- nickname and avatar,  
- language/locale preferences,  
- profile visibility settings (e.g. public, visible only to logged-in users, or private/limited).

**BR-PRO-02 – Role & Curator Attribution on Profile**  
**Priority:** S  
The profile should display the user’s current role (Regular User, Curator, Admin), and for Curators, should highlight key contributions (e.g. number of achievement sets and sticker album templates they maintain).

**BR-PRO-03 – Per-Game Progress Overview**  
**Priority:** M  
For each game the user interacts with, the system shall provide a per-game overview summarising:

- user’s saves for that game,  
- user’s achievement set progress (official/external/fan-made where applicable),  
- user’s album instances and completion status.

**BR-PRO-4 – Cross-Game Progress Summary**  
**Priority:** S  
The system should provide a high-level view of a user’s overall progress across games, including:

- total completed achievement sets,  
- total completed sticker albums,  
- notable badges and milestones.

**BR-PRO-05 – Privacy Controls for Content**  
**Priority:** M  
Users shall be able to control visibility for key content types (e.g. saves, screenshots, album instances, certain progress information), and profiles displayed to others shall respect these settings.

**BR-PRO-06 – External Profile References (Safe & Minimal)**  
**Priority:** S  
Where external profiles (e.g. Steam, retro achievement services) are linked, the system should allow the user to see which external services are connected and, if applicable, provide a safe link or reference, while not exposing sensitive identifiers publicly to other users.

---
<br>

### 7.6 Governance & Moderation Requirements

**BR-GOV-01 – Role Model (User, Curator, Admin)**  
**Priority:** M  
The system shall support at least three roles with distinct capabilities:

- Regular User,  
- Curator,  
- Platform Administrator (Admin).

**BR-GOV-02 – Reporting Mechanism**  
**Priority:** M  
Users shall be able to report:

- saves, screenshots, achievement items and sets, albums, comments, and profiles,

with a simple reason selection and optional free-text comment.

**BR-GOV-03 – Moderation Queue & Actions**  
**Priority:** M  
Admins shall have access to a moderation queue where reported items can be reviewed and actions applied (e.g. keep, hide, remove, warn or restrict user), with minimal logging of decisions.

**BR-GOV-04 – Curator-Specific Responsibilities & Limits**  
**Priority:** M  
Curators shall have extended content-creation capabilities (sets, albums, suggested catalogue corrections) but shall:

- not have full Admin powers, and  
- remain subject to moderation and platform policies, especially regarding sensitive metadata and policy flags.

Admins must be able to revoke a Curator role if needed.

**BR-GOV-05 – Decision Logging for Sensitive Cases**  
**Priority:** N  
For sensitive moderation or policy decisions (e.g. controversial content, catalogue flags), the system may allow brief decision logging so that future similar cases can reuse or reference past reasoning.

---
<br>

### 7.7 Catalogue, Localisation & Policy Requirements

**BR-CAT-01 – Game & Platform Catalogue Management**  
**Priority:** M  
The system shall provide a catalogue of games and platform variants, with the ability for authorised users to create and edit entries, including titles, basic descriptions, and key attributes (e.g. retro vs modern).

**BR-CAT-02 – Localisation Metadata (Including Ukrainian)**  
**Priority:** M  
Catalogue entries shall be able to store localisation metadata, including whether a game supports **official Ukrainian localisation**, and this information shall be visible on game pages and usable in filters.

**BR-CAT-03 – Origin & Policy Flags (Ukrainian-Origin, Russian-Linked)**  
**Priority:** M  
Catalogue entries shall support policy-related flags, including but not limited to:

- “Ukrainian-origin game” (developer/studio or strong UA association),  
- “Russian-linked publisher/developer” according to platform policy.  

These flags shall influence how games are highlighted or annotated for users, especially those using the Ukrainian locale.

**BR-CAT-04 – Filters & Indicators for UA Users**  
**Priority:** M  
For users using the platform in Ukrainian locale, the system shall allow:

- filtering or highlighting games with Ukrainian localisation and/or Ukrainian origin,  
- displaying clear indicators or contextual notes for Russian-linked content according to defined policy.

**BR-CAT-05 – Save & Feature Availability Indicators per Game/Platform**  
**Priority:** M  
For each game/platform variant, the system shall indicate which Dream Project pillars are available (save sharing, achievements, albums) and whether any are restricted due to technical or policy reasons.

**BR-CAT-06 – Catalogue Change Audit (Minimal)**  
**Priority:** N  
For high-impact metadata (localisation, origin, Russian-linked flags), the system may maintain minimal audit information (who changed what and when) for internal reference.

---
<br>

### 7.8 Discovery & Recommendation Requirements

**BR-DIS-01 – Game Search & Browsing**  
**Priority:** M  
Users shall be able to search and browse the game catalogue using basic criteria (e.g. title, platform, tags such as retro/modern) and navigate to a dedicated game page.

**BR-DIS-02 – Consolidated Game Page**  
**Priority:** M  
Each game’s page shall consolidate, where applicable:

- available saves and a link to search shared saves (or a clear note if save sharing is not supported),  
- official/external/fan-made achievement sets,  
- sticker album templates,  
- key localisation and origin flags.

**BR-DIS-03 – Simple Recommendation Entry Point**  
**Priority:** S  
The system should provide an optional “help me choose” or similar entry point that suggests:

- a game from the catalogue, and  
- one relevant pillar to start with (e.g. try an album, browse shared saves),

based on popularity, curated picks, or basic past activity.

**BR-DIS-04 – Respecting Feature Availability in Recommendations**  
**Priority:** M  
Recommendations shall only suggest games and actions where the underlying pillar (saves, sets, albums) actually has usable content or support for the relevant platform variant.

**BR-DIS-05 – User Control over Recommendations**  
**Priority:** S  
Users should be able to reject a suggestion and request another, and the system should avoid repeatedly proposing items that were explicitly declined in the same session.

---
<br>

### 7.9 Integration & External Services Requirements

**BR-INT-01 – Respectful API Usage & Attribution**  
**Priority:** M  
Any integration with external services (e.g. Steam, retro achievement platforms, metadata providers) shall:

- use APIs in a way consistent with provider terms and rate limits,  
- clearly attribute data origin to the external service,  
- avoid any behaviour that could be interpreted as attempting to bypass rules or imitate official clients.

**BR-INT-02 – Graceful Degradation on Integration Failure**  
**Priority:** M  
If an external integration is unavailable or degraded, the system shall:

- avoid blocking unrelated Dream Project functionality,  
- show clear, non-technical messages to affected users,  
- fall back to partial or cached information where reasonable.

**BR-INT-03 – User Control Over Linked External Profiles**  
**Priority:** M  
Users shall be able to link and unlink external profiles (e.g. Steam accounts, retro achievement accounts) through controlled flows, and see which integrations are active.

---
<br>

### 7.10 Basic Business Metrics & Insight Requirements

**BR-MET-01 – Pillar Usage Metrics**  
**Priority:** N  
The system may capture basic metrics per pillar (saves, achievements, albums), such as number of active users per period, number of uploads, completions, and started items, to support evaluation of MVP success.

**BR-MET-02 – Curator Activity Overview**  
**Priority:** N  
The system may provide simple internal views or exports showing Curator activity (e.g. number of sets/albums created or updated, usage of their content) to help prioritise communication and support.

**BR-MET-03 – UA vs Global Adoption Signals**  
**Priority:** N  
Where feasible, the system may allow internal tracking of approximate language/locale distribution and usage of UA-specific features (filters, flags) to verify that Ukrainian-related goals are being addressed.

---
<br>
<br>

## 8. Non-Functional Requirements (Business View)

This section describes **non-functional requirements (NFRs)** that matter to Dream Project from a business perspective.  
They do not prescribe specific technologies but define **how the platform should behave and feel** for users and operators.

As with business requirements, each NFR has a priority:

- **M** – Must-have for MVP  
- **S** – Should-have for MVP (valuable; can be simplified)  
- **N** – Nice-to-have (strong candidate for later phases)

IDs are structured as `NFR-<AREA>-NN`.

---
<br>

### 8.1 Usability & User Experience

**NFR-UX-01 – Clear First Action**  
**Priority:** M  
For new signed-in users, the platform shall make it clear **what to do first** (e.g. “Pick a game” or “Start an album”) within one or two screens, using short copy and onboarding hints instead of long manuals.

**NFR-UX-02 – Consistent Navigation**  
**Priority:** M  
Navigation patterns (menus, game pages, profile pages) shall be **consistent across pillars** (saves, achievements, albums) so that once a user understands navigation for one area, the others feel familiar.

**NFR-UX-03 – Plain Language with Light Jargon**  
**Priority:** M  
User-facing text (EN and UA):

- shall avoid heavy technical jargon (e.g. file system paths, low-level emulator terms) in main flows,  
- may include more technical details in expandable help or advanced sections for experienced users.

**NFR-UX-04 – Error Messages that Explain Next Steps**  
**Priority:** M  
When something goes wrong (e.g. invalid save upload, missing integration, unavailable feature), the platform shall show short, understandable messages that:

- state what happened in simple terms, and  
- suggest **what the user can do next** (e.g. “Try again later”, “Use this feature instead”, “Check our help article”).

**NFR-UX-05 – Minimal Friction for Core Flows**  
**Priority:** S  
Core user flows (uploading a save, joining an achievement set, starting an album) should usually be completed in **no more than 3–5 steps** from the relevant game page, without forcing unnecessary data entry.

---
<br>

### 8.2 Performance & Responsiveness

**NFR-PERF-01 – Page Load Perception**  
**Priority:** M  
For typical MVP loads and reasonable consumer internet connections, key pages (home, game pages, profile, set/album view) shall **feel responsive**, with:

- initial content visible within ~2–3 seconds in normal conditions, and  
- navigation between major sections generally under ~2 seconds once assets are cached.

Numbers are indicative; the intent is to avoid “sluggish” behaviour that discourages use.

**NFR-PERF-02 – Heavy Operations Boundaries**  
**Priority:** S  
Operations that may naturally be slower (e.g. large search results, aggregate statistics, image-heavy album pages) should:

- make progress obvious (loading indicators), and  
- aim to complete in **under ~5–7 seconds** for typical scenarios.

**NFR-PERF-03 – Retro/External Sync Timing Expectations**  
**Priority:** S  
When updating progress from external services (e.g. Steam, retro achievements), the platform should:

- avoid implying “instant” sync if that is not realistic, and  
- present clear expectations (e.g. “Updates may take a few minutes”, “Last updated X minutes/hours ago”).

---
<br>

### 8.3 Availability & Reliability

**NFR-REL-01 – MVP Availability Target**  
**Priority:** M  
Dream Project is a small-scale MVP, not a 24/7 enterprise service, but it shall:

- be generally available outside short maintenance windows, and  
- avoid long unexplained outages for core user-facing functionality.

No formal uptime SLA is required, but the intent is **“usually usable when someone tries to access it”**.

**NFR-REL-02 – Controlled Maintenance Windows**  
**Priority:** S  
Planned maintenance should:

- be scheduled for low-usage periods where possible, and  
- be accompanied by a short notice (status message or banner) when work is ongoing and core features are unavailable.

**NFR-REL-03 – Graceful Failure for Non-Critical Features**  
**Priority:** M  
If non-critical components fail (e.g. metrics collection, optional recommendations, some statistics), the platform shall **degrade gracefully** and keep core flows (login, viewing game pages, uploading saves, tracking achievements/albums) operational where technically possible.

---
<br>

### 8.4 Security, Privacy & Data Protection

**NFR-SEC-01 – Secure Transmission & Storage of Sensitive Data**  
**Priority:** M  
User credentials, linked external tokens, and any personal data shall:

- be transmitted over secure channels (e.g. HTTPS), and  
- be stored in a way that follows standard security practices for an online application (e.g. hashed passwords, restricted access to sensitive keys).

The MVP is not expected to implement every enterprise control, but **basic good security hygiene** is mandatory.

**NFR-SEC-02 – Minimal Personal Data Collection**  
**Priority:** M  
The platform shall collect and store **only the personal data needed** to operate core features:

- e.g. email or similar for login/recovery, nickname, optional avatar, language preference, external profile links.  

Sensitive personal attributes beyond this are out of scope for MVP.

**NFR-SEC-03 – External Profile Linking Transparency**  
**Priority:** M  
When linking external accounts (Steam, retro achievements, etc.), the platform shall clearly explain:

- what data will be accessed,  
- how it will be used (read-only, for progress display only),  
- how the user can revoke access or unlink the account later.

**NFR-SEC-04 – Content & Evidence Handling**  
**Priority:** M  
Uploaded content (saves, screenshots, evidence for achievements):

- shall be treated as user-owned content,  
- shall not be modified by Dream Project except for safety checks (e.g. thumbnailing, basic AI-based validation),  
- may be removed or restricted by Admins under moderation policies.

**NFR-SEC-05 – Privacy & Visibility Defaults**  
**Priority:** M  
Visibility defaults shall favour **safety and privacy**:

- profiles visible in a limited form by default (configurable),  
- saves and screenshots defaulting to private or limited visibility, unless the user explicitly chooses to share.

---
<br>

### 8.5 Localisation & Language Support

**NFR-LOC-01 – Primary Languages: EN & UA**  
**Priority:** M  
The MVP shall support at least:

- English as the primary interface language, and  
- Ukrainian as a key second language for player-facing pages important to UA users (e.g. core navigation, game pages, policy-related texts, UA-specific features).

**NFR-LOC-02 – UA-Critical Content Coverage**  
**Priority:** M  
For Ukrainian users, the following must have UA versions (even if other parts of the site remain EN-only at MVP):

- key UI labels and navigation elements,  
- catalogue policy explanations (UA-origin, Russian-linked flags),  
- core help/FAQ about how to interpret these flags.

**NFR-LOC-03 – Partial Localisation Strategy**  
**Priority:** S  
Where full localisation is not yet available, the platform should:

- avoid mixing languages in the same sentence or label,  
- clearly indicate when a section is only available in English,  
- prioritise adding Ukrainian translations for the most frequently used player journeys.

---
<br>

### 8.6 Accessibility & Inclusivity

**NFR-ACC-01 – Basic Web Accessibility Practices**  
**Priority:** M  
The UI shall follow basic accessibility practices suitable for an MVP, including:

- meaningful text alternatives for key images and icons,  
- logical heading structure,  
- keyboard-focusable interactive elements where feasible.

**NFR-ACC-02 – Colour & Contrast Considerations**  
**Priority:** S  
The visual design should:

- avoid relying solely on colour to convey critical information (e.g. red/green without icons or text),  
- aim for sufficient contrast for main text and buttons to be readable on common displays.

**NFR-ACC-03 – Avoiding Overly Flashy or Distracting Elements**  
**Priority:** S  
Core product areas (game pages, sets, albums) should avoid:

- rapid animations or flashing elements that could distract or overwhelm users,  
- unnecessary visual noise that makes it hard to focus on content (saves, achievements, screenshots).

---
<br>

### 8.7 Maintainability & Evolvability

**NFR-MAINT-01 – Clear Separation of Business Logic & External Integrations**  
**Priority:** M  
The solution architecture shall separate:

- internal business logic (saves, sets, albums, catalogue) from  
- external integration details (Steam, retro achievements, metadata sources),

so that integrations can be changed or disabled with limited impact on core functionality.

**NFR-MAINT-02 – Configurable Feature Flags (Where Reasonable)**  
**Priority:** S  
Where technically feasible, key features (e.g. specific integrations, recommendation module) should be controllable via configuration or feature flags, enabling:

- safe roll-out,  
- easy disabling in case of issues.

**NFR-MAINT-03 – Minimal but Structured Documentation for Operators**  
**Priority:** M  
There shall be minimal, structured documentation for:

- Admin and moderation workflows,  
- catalogue maintenance rules,  
- integration configuration and basic troubleshooting.

This documentation may live alongside BA artefacts but must be discoverable by operators.

**NFR-MAINT-04 – Logging for Debugging**  
**Priority:** S  
The system should produce enough internal logs for developers and Admins to:

- investigate issues (e.g. errors in external calls, moderation actions, failed uploads),  
- diagnose problems without relying only on user reports.

---
<br>

### 8.8 Legal, Policy & Community Considerations

**NFR-LAW-01 – Respect for External Platform Terms**  
**Priority:** M  
All interactions with external platforms (Steam, retro achievement services, etc.) shall be designed to **respect their terms of service**:

- read-only where required,  
- no scraping or impersonation of official clients,  
- no automated behaviour that risks account penalties for users.

**NFR-LAW-02 – Clear Terms & Policy Communication (MVP-Scaled)**  
**Priority:** M  
Dream Project shall provide:

- a clear, concise terms-of-use or rules page,  
- a brief content policy describing what is acceptable,  
- short, user-friendly explanations for UA-related catalogue policies (UA-origin, Russian-linked flags and visibility).

This can be simpler than a full legal department version, but must be understandable and visible.

**NFR-LAW-03 – Content Ownership & Takedown**  
**Priority:** M  
The platform shall:

- state that users retain ownership of their uploads (saves, screenshots),  
- explain under what conditions content may be removed (e.g. copyright, policy violations),  
- provide a basic mechanism for users to request removal of their content or to report suspected copyright issues.

---
<br>

These non-functional requirements provide the **behavioural and quality frame** for Dream Project MVP.  
Together with the business requirements from Section 7, they define not only *what* the platform should do, but also *how* it should feel and operate for players, curators, admins, and the Ukrainian-focused aspects of the catalogue and community.

---
<br>
<br>

## 9. Assumptions & Constraints

This section summarises the **assumptions** and **constraints** that directly affect how the business requirements in this BRD should be interpreted and implemented.  
Detailed risk-level discussion lives in other artefacts; here we focus on what shapes the **scope and realism** of the MVP.

---
<br>

### 9.1 Key Assumptions (BRD-Level)

These assumptions are considered **true for the MVP**.  
If they change, some requirements in Section 7 may need to be revisited or re-prioritised.

**A1 – Small but steady core team**

- The product is built and operated by a **small team** (e.g. 1 PO/BA, 1–2 developers, 1 QA, 1UX/UI designer, part-time AI/ML Engineer and/or DevOps, 1 admin/moderator role, possibly combined).
- This limits:
  - how many complex features can be delivered in the MVP,
  - how intensive manual moderation and manual data entry can be.

**Impact on BRD:**  
Requirements must avoid heavy manual workflows (e.g. purely manual verification for every achievement) as the primary mechanism and favour scalable patterns (templates, basic automation, sampling, prioritised queues).

---

**A2 – Limited but motivated early user base**

- Early adoption is expected to be **hundreds**, not tens of thousands, of active users.
- A subset of users (especially retro and dedicated players) will be willing to:
  - link external profiles (Steam, retro achievements),
  - contribute content as Curators (sets, albums).

**Impact on BRD:**  
Requirements assume that feature usage is **non-zero but not massive**, so:

- some manual curation and moderation is still acceptable,
- complex scalability and enterprise-grade SLAs are not required for MVP.

---

**A3 – External services are available in a reasonable, read-only form**

- At least one modern gaming platform (e.g. Steam) and one retro/external achievement ecosystem (e.g. RetroAchievements) provide:
  - documented ways to retrieve achievement or profile data in a **read-only** manner, or
  - stable pages/feeds from which progress can be derived without violating terms.
- These services remain generally available and do not drastically change their terms during the MVP period.

**Impact on BRD:**  
Achievement-related requirements (BR-ACH-01, BR-ACH-03, BR-ACH-04, BR-INT-01/02) assume:

- integrations can be implemented and maintained at MVP scale,
- Dream Project can safely show official/retro progress without scraping or risky automation.

---

**A4 – Basic game metadata is obtainable**

- Game and platform metadata can be sourced from:
  - public databases or APIs, **or**
  - curated manually by Catalogue Maintainers for the initial set of titles.
- For the MVP scope of games, this is manageable with the available team.

**Impact on BRD:**  
Catalogue requirements (BR-CAT-01..05) assume that:

- the initial games catalogue can be populated without full automation,
- high-quality metadata for a **limited set** of games is more important than broad coverage.

---

**A5 – Two primary interface languages (EN & UA) are sufficient for MVP**

- English and Ukrainian are the only fully supported UI languages for the MVP.
- Other languages may appear in user-generated content (e.g. comments, descriptions) but are not first-class UI locales.

**Impact on BRD:**  
Localisation requirements (BR-CAT-02, NFR-LOC-01..03) are scoped around:

- prioritising EN + UA UI,
- ensuring UA users can fully understand catalogue policy and core flows,
- treating additional languages as out of scope for now.

---

**A6 – The product is used via web only (no native apps in MVP)**

- The MVP is accessed via a modern web browser on desktop or mobile.
- No native mobile/desktop clients are required for MVP.

**Impact on BRD:**  
Non-functional and UX requirements assume:

- responsive web UI only,
- no offline app mode beyond basic browser caching.

---

**A7 – Content creators and Curators accept basic tooling**

- Curators and creators are prepared to work with:
  - form-based editors for sets and albums,
  - simple dashboards and lists,
  - links to external resources (guides, videos).
- Highly advanced “pro” tools (rich WYSIWYG layout editors, complex scripting, etc.) are not expected at MVP.

**Impact on BRD:**  
Curator-focused requirements (BR-ACH-05, BR-ALB-01/02, BR-ACC-04) assume tooling can be **simple but structured**, not full-featured content-authoring environments.

---
<br>

### 9.2 Business & Product Constraints

These are **known boundaries** within which the MVP must operate.

**C1 – MVP timebox & scope discipline**

- The MVP is assumed to be built within a **limited timeframe** (a few months) with a junior-leaning team.
- Not all “Should” and “Nice-to-have” requirements in this BRD will necessarily be delivered in the first public release.

**Implication:**  
Priorities (M/S/N) are meaningful:

- **Must** items define the minimum acceptable platform,
- **Should** items are strong candidates if capacity allows,
- **Nice-to-have** items are explicit scope for later if time is too tight.

---

**C2 – Respect for external platform terms**

- Dream Project must not:
  - scrape or automate against services in ways that break ToS,
  - simulate official clients where forbidden,
  - encourage behaviour that risks bans or penalties for users.

**Implication:**  
Integration requirements (BR-ACH-01..04, BR-INT-01..03) and relevant NFRs constrain:

- how “automatic” official/retro achievement syncing can be,
- how often and how aggressively external APIs can be called,
- what kind of UI promises are made (“live sync” vs “periodic updates”).

---

**C3 – Limited moderation and catalogue staffing**

- Moderation and catalogue maintenance will be done by a **very small group** of Admins and maintainers.

**Implication:**  

- Business requirements must avoid processes that require manual review of every single item or evidence by default.
- Features like evidence attachments, comments, and rating systems must be designed to:
  - support targeted moderation (e.g. via reports and flags),
  - not overload the team with constant manual checks.

---

**C4 – Policy stance on Ukrainian context and Russian-linked content**

- The platform intentionally implements **special handling** of:
  - games with Ukrainian localisation or Ukrainian origin,
  - Russian-linked publishers/developers, as defined in platform policy.
- Policy must be consistently applied across catalogue and UI.

**Implication:**  

- Catalogue and localisation requirements (BR-CAT-02..05, NFR-LAW-02) constrain how content is presented, filtered, and annotated.
- Some games or content may be **de-emphasised, annotated or discouraged** for UA users when flagged as Russian-linked.

---

**C5 – Web-friendly content sizes and formats**

- Saves and screenshots are expected to be within **reasonable size limits** so that:

  - storage and bandwidth remain manageable for an MVP,
  - uploads/downloads do not significantly break UX.

**Implication:**  

- Requirements implicitly assume some maximum file sizes and supported formats (to be specified in SRS).
- Very large or exotic formats may not be supported at first.

---

**C6 – No guaranteed data migration from other services**

- The MVP does **not** promise:
  - full import of historical saves from all platforms/emulators,
  - automatic migration of entire external libraries.

**Implication:**  

- BRs around saves and achievements focus on **forward-looking use** (from the time users adopt Dream Project),
- any bulk import features are considered beyond MVP unless clearly defined as limited pilots.

---
<br>

### 9.3 Interpretation & Change Handling

- If **assumptions fail** (e.g. an external API becomes unusable, team size is much smaller than expected), some requirements – especially “Should” and “Nice-to-have” – may need to be:
  - postponed,
  - simplified,
  - or replaced with manual/partial alternatives.
- If **constraints change** (e.g. additional funding, official partnerships, expanded moderation capacity), this BRD should be updated to:
  - relax constraints where appropriate, and  
  - potentially promote some “Should/Nice-to-have” items to “Must” in future phases.

---
<br>

This section should be reviewed alongside the Business Case and BA-level assumptions whenever the scope or context of Dream Project **shifts in a significant way**.

---
<br>
<br>

## 10. Dependencies & External Interfaces

This section identifies the **specific external systems and relationships** that Dream Project MVP depends on, and describes at a business level how those dependencies affect the product.  
Technical details (API endpoints, protocols, data formats) will be defined later in the SRS.

---
<br>

### 10.1 Overview

For the MVP, Dream Project intentionally relies on a **small, concrete set** of external services and information sources:

1. **Steam** – official achievements for selected modern PC games.  
2. **RetroAchievements** – achievements for retro/emulator titles and basic conventions.  
3. **IGDB** – primary general-purpose game metadata/catalogue source.  
4. **UA-focused catalogues & localisation trackers** – additional sources for Ukrainian localisation/origin and Russia-linked information (e.g. *Kuli*, *Artemiano*).  
5. **SendGrid (or equivalent single transactional email provider)** – registration and account emails.  
6. **YouTube + GameFAQs (and similar)** – typical external destinations for clips/guides, referenced via links only.

In addition, Dream Project depends on:

- **Curators & creators**,  
- **Platform Admins & catalogue maintainers**,  
- **Ukrainian community stakeholders**  

as human/organisational dependencies.

All business requirements in Section 7 should be interpreted assuming *these* concrete services and sources are used for MVP.

---
<br>

### 10.2 Gaming & Achievement Platforms

#### 10.2.1 Steam (Modern PC Platform – Official Achievements)

**Purpose (Business View):**

- Provide **official achievement data** for selected modern PC games.  
- Allow Dream Project users to **link their Steam account** and see their official progress reflected in Dream Project.

**Used For:**

- Displaying official achievement lists and completion status  
  → Supports BR-ACH-01, BR-ACH-02, BR-ACH-03, BR-ACH-04, BR-INT-01..03.  
- Optionally helping to disambiguate specific PC versions of games in the catalogue.

**Interaction Type (MVP):**

- **Read-only** access to Steam data, via documented/authenticated mechanisms only.  
- No write-back to Steam, no automation that imitates the official client.

**Business Impact if Unavailable or Restricted:**

- For affected games:
  - official achievements may temporarily disappear from Dream Project,  
  - or fall back to a small, manually maintained achievement definition for those titles,  
  - or be disabled entirely for that game.
- Dream Project remains fully usable for:
  - fan-made achievement sets,  
  - sticker albums,  
  - save management (where PC save sharing is technically possible).

Steam is the **only modern platform** assumed for official achievement integration in MVP.

---
<br>

#### 10.2.2 RetroAchievements (Retro / Emulator Achievement Ecosystem)

**Purpose (Business View):**

- Provide **retro achievement sets and progress data** for supported retro/emulator titles.  
- Allow Dream Project users to **link their RetroAchievements profile** and see retro progress inside Dream Project.
- Act as a **reference for emulator usage and save handling conventions** for supported retro platforms.

**Used For:**

- Displaying imported retro achievement sets and completion status  
  → BR-ACH-02, BR-ACH-03, BR-ACH-04.  
- Providing examples of:
  - how retro achievements are structured,  
  - how progress is tracked across emulators,  
  - where and how retro saves are typically stored and synced.

**Interaction Type (MVP):**

- **Read-only synchronisation** of profile and achievement data, respecting RetroAchievements’ API rules and rate limits.  
- No modification of RetroAchievements data.

**Business Impact if Unavailable or Restricted:**

- Retro progress views may:
  - be outdated,  
  - be temporarily missing,  
  - or require users to rely entirely on Dream Project–native achievement sets for those games.
- Retro players can still:
  - upload saves,  
  - use Dream Project fan-made sets,  
  - fill sticker albums.

---
<br>

### 10.3 Game Metadata, Localisation Sources & Supporting Services

#### 10.3.1 IGDB (Primary General Game Metadata Provider)

**Purpose (Business View):**

- Serve as the **primary external source** of general game metadata for Dream Project MVP, including:
  - game titles, release dates, platforms,  
  - basic descriptions, genres/tags,  
  - sometimes cover images.

**Used For:**

- Initial population of the Dream Project game catalogue  
  → BR-CAT-01..05.  
- Reducing manual work for Catalogue Maintainers when adding games.

**Interaction Type (MVP):**

- Periodic or one-off **imports** from IGDB for the games that Dream Project decides to support.  
- Manual review/correction by Catalogue Maintainers where necessary.  
- No guarantee of live, continuous sync for every field.

**Business Impact if Unavailable or Limited:**

- The catalogue will:
  - focus on a **smaller curated set** of games,  
  - rely more heavily on manual data entry for those titles.
- Requirements around catalogue *quality* remain; **breadth of coverage** is what gets constrained.

---
<br>

#### 10.3.2 UA-Focused Catalogues & Localisation Trackers  
*(e.g. Kuli, Artemiano’s UA localisation lists)*

**Purpose (Business View):**

- Provide **Ukrainian-specific information** that IGDB does not cover, such as:
  - whether a game has **official Ukrainian localisation**,  
  - whether a game is considered **Ukrainian-origin** (UA developers, publishers, or strong UA association),  
  - basic signals about **Russian-linked publishers/developers** (directly or indirectly).

**Used For:**

- Determining and verifying catalogue flags used in BR-CAT-02..05:
  - “Has Ukrainian localisation”,  
  - “Ukrainian-origin game”,  
  - “Russian-linked publisher/developer”.  
- Supporting UA-specific filters and annotations for Ukrainian users.

**Interaction Type (MVP):**

- **Reference-based** use:
  - Catalogue Maintainers consult Kuli, Artemiano and similar UA-focused resources when creating or updating game entries.  
  - No deep technical integration is required for MVP; these sites are used as **authoritative references** plus manual research and community feedback.
- For some titles, decisions may require:
  - cross-checking multiple sources,  
  - input from UA Curators or players.

**Business Impact if Unavailable or Limited:**

- Some flags (UA localisation, UA-origin, Russian-linked) may:
  - be slower to establish,  
  - rely more on manual research and community knowledge.
- Dream Project must be prepared to:
  - adjust flags over time if new information appears,  
  - be transparent that classification is based on best available UA-focused sources, not a perfect global database.

---
<br>

#### 10.3.3 External Media & Clip/Guide Services (YouTube, GameFAQs, etc.)

**Purpose (Business View):**

- Provide **external evidence and guide content** that Curators and users can reference in Dream Project, such as:
  - YouTube videos or clips showing how to obtain a specific achievement,  
  - recorded playthrough segments that prove completion of fan-made achievements,  
  - GameFAQs pages or similar guides explaining routes, secrets, or advanced challenges.

**Used For:**

- Evidence attachment for achievements (BR-ACH-08).  
- Curator-provided hints and resources in achievement items and sets (BR-ACH-05, BR-ACH-06, BR-ACH-09).  

**Storage Model (Business View):**

- Dream Project MVP does **not host full gameplay clips** itself.  
- Users are expected to:
  - create clips using their preferred tools/platforms (e.g. capturing via console/PC, uploading to YouTube or another video host),  
  - paste the **link/URL** into Dream Project as evidence or guide reference.
- Dream Project stores:
  - the link,  
  - minimal metadata (e.g. title/thumbnail if easily retrievable),  
  - association with the specific achievement item or set.

**Interaction Type (MVP):**

- Link-only integration:
  - Where embedding is allowed and technically simple, videos may be previewed; otherwise, the user is sent to the external site.  
  - Dream Project does not control or guarantee the availability of external content.

**Business Impact if Unavailable or Changed:**

- Linked content may:
  - disappear,  
  - become private,  
  - or change its content.  
- The platform must:
  - handle broken or unreachable links gracefully,  
  - never rely on a clip as the **only** way to understand an achievement or set (Curators should still provide textual criteria/hints).

---
<br>

#### 10.3.4 Emulator & Save-Handling Guidance Sources (Retro & PC/Steam)

**Purpose (Business View):**

- Provide **reference information** about how saves are stored, backed up, and restored for:
  - retro/emulator-based titles, and  
  - **modern PC games on Steam** (local saves + Steam Cloud behaviour).
- Help users understand when and how **save sharing** is realistic and safe for a specific game/platform variant.

**Likely Sources (Conceptual, not deeply integrated):**

- Official or community documentation for major **emulators**  
  (e.g. PCSX2, Dolphin, SNES emulators), describing:
  - default save locations,  
  - recommended backup/restore processes,  
  - limitations when moving saves between devices or emulator versions.
- Official Steam and game-specific documentation for **PC titles**, including:
  - whether Steam Cloud is supported,  
  - where local save files are stored,  
  - whether saves are profile-bound or freely copyable.
- Selected community wikis, guides, and forum posts dedicated to:
  - save locations for specific games,  
  - known compatibility issues when sharing saves,  
  - “safe practices” for experimenting with external saves.

**Used For:**

- Creating and maintaining **help pages and knowledge articles** inside Dream Project that explain, for both retro and modern PC games:
  - “Where are saves stored for platform X or for this Steam game?”  
  - “How do I safely use a downloaded save with emulator Y or Steam game Z?”  
  - “What are typical limitations when sharing saves for this system or title?”
- Providing **inline hints** in UI (e.g. short info blocks on game pages) that:
  - summarise key save-handling points,  
  - link to more detailed internal guides for users who want extra context.

**Interaction Type (MVP):**

- No direct technical integration; these resources are used as **research and reference material** by:
  - Catalogue Maintainers,  
  - Curators who specialise in specific systems or games.
- Knowledge is distilled into Dream Project’s **own help content**, rather than copying external text verbatim.

**Business Impact if Unavailable or Outdated:**

- Some help pages for saves (retro or PC/Steam) may become outdated and need occasional refresh based on:
  - new emulator versions,  
  - changes in Steam or game-specific save behaviour.
- MVP assumes:
  - a **limited set of emulators and PC games** is targeted initially,  
  - documentation is curated manually for those systems/titles rather than trying to cover everything at once.

---
<br>

#### 10.3.5 Transactional Email Provider (e.g. SendGrid)

**Purpose (Business View):**

- Deliver essential **account-related emails** for Dream Project, such as:
  - registration confirmation,  
  - password reset,  
  - critical security or role-change notifications (e.g. curator approval).

**Used For:**

- Fulfilling BR-ACC-01 (Account Registration & Sign-in).  
- Supporting basic account lifecycle and security expectations.

**Interaction Type (MVP):**

- Outbound email via one configured transactional email service (assumed **SendGrid** for this BRD).  
- No complex marketing automation; only minimal transactional traffic.

**Business Impact if Unavailable or Limited:**

- New registrations or password resets may:
  - be delayed,  
  - require temporary manual support or alternative flows.
- MVP must be prepared to:
  - briefly limit new registrations during severe outages, or  
  - present clear error messages and fallback instructions.

---
<br>

### 10.4 Human & Organisational Dependencies

#### 10.4.1 Curators & Content Creators

**Purpose (Business View):**

- Provide the **fan-made layer**:
  - Dream Project achievement sets and subsets,  
  - sticker album templates,  
  - curated descriptions, hints, and resource links.

**Used For:**

- Fulfilling BR-ACH-05..11, BR-ALB-01..02, BR-PRO-02, BR-ACC-04.  
- Giving the platform depth beyond what IGDB/Steam/RetroAchievements alone can provide.

**Dependency Nature:**

- Curators are external or semi-external, often unpaid contributors.
- Their engagement depends on:
  - clear expectations and tools,  
  - visible attribution,  
  - a sense that their work is respected and discoverable.

**Business Impact if Engagement is Low:**

- Fewer curated sets and albums; the platform leans more heavily on:
  - official Steam achievements,  
  - RetroAchievements sets,  
  - internal seed content.
- Product roadmap may pivot towards:
  - improving curator UX,  
  - adding incentives,  
  - or narrowing initial focus to titles with strong existing support.

---
<br>

#### 10.4.2 Platform Administrators, Moderation & Catalogue Maintainers

**Purpose (Business View):**

- Operate and safeguard the platform by:
  - managing roles and curator approvals,  
  - reviewing reports and applying moderation actions,  
  - maintaining catalogue entries and policy flags (UA-origin, Russian-linked, etc.).

**Used For:**

- Fulfilling BR-GOV-01..05, BR-CAT-01..06, NFR-LAW-02..03.  
- Ensuring Dream Project does not become unsafe or misaligned with its policies.

**Dependency Nature:**

- Small group, sometimes overlapping with other roles (e.g. the developer or BA may also act as Admin).  
- Need simple tooling and concise internal documentation.

**Business Impact if Capacity is Limited or Unstable:**

- Longer response times for:
  - reported content,  
  - curator role requests,  
  - catalogue corrections and UA/Russia-related flag updates.
- Some ambitious features (e.g. intensive manual verification of evidence) may need to remain optional or sampling-based.

---
<br>

#### 10.4.3 Ukrainian Community & UA-Focused Stakeholders

**Purpose (Business View):**

- Ensure the platform meaningfully supports **Ukrainian players and creators**, especially around:

  - UA localisation indicators,  
  - UA-origin games,  
  - visibility and annotation of Russian-linked content,  
  - communication of the reasoning behind catalogue policy.

**Used For:**

- Validating that UA-specific features and messaging:
  - are clear,  
  - feel respectful and accurate,  
  - help users make informed decisions about what they play and support.

**Dependency Nature:**

- Informal feedback loop through UA Curators, creators, and communities (Discords, forums, etc.).  
- No formal contracts, but high reputational importance.

**Business Impact if Engagement is Low or Misaligned:**

- Risk that UA-specific features are misunderstood or seen as insufficient.  
- May require:
  - revisiting how flags are defined and displayed,  
  - improving UA documentation and communication,  
  - seeking direct feedback from representative UA players/creators.

---
<br>

### 10.5 Degradation & Fallback Expectations (Business View)

Given these specific dependencies, Dream Project is expected to behave as follows when something goes wrong:

- **Steam or RetroAchievements integrations fail or change**  
  → Dream Project:
  - keeps internal features (saves, fan-made sets, albums) fully usable,  
  - clearly indicates that official/retro progress is temporarily unavailable or outdated,  
  - avoids promising real-time sync.

- **IGDB and/or UA-focused sources are unavailable or incomplete**  
  → Dream Project:
  - slows or pauses catalogue expansion,  
  - focuses on maintaining and curating existing titles,  
  - may add a limited number of new games via manual research and UA community input.

- **External clip/guide hosts (YouTube, GameFAQs) change or remove content**  
  → Dream Project:
  - continues to display textual achievement criteria and hints,  
  - treats linked videos/guides as optional enrichment, not the only source of truth,  
  - handles broken links gracefully.

- **Transactional email provider issues**  
  → Dream Project:
  - may temporarily limit new registrations or password resets,  
  - shows clear guidance to users on when to retry or how to seek help.

- **Curator/Admin capacity is low**  
  → Dream Project:
  - relies heavily on report-driven moderation rather than pre-approval,  
  - keeps tools simple and focused on the most important queues,  
  - may postpone or simplify features that require heavy manual review.

---
<br>
<br>

## 11. Business Acceptance Criteria

This section defines **when the Dream Project MVP can be considered “accepted” from a business point of view**.  
It does **not** describe low-level test cases or technical QA; instead, it focuses on:

- whether the **core pillars and flows** work end-to-end for real users;  
- whether the **most important business and UX expectations** are met;  
- whether **Ukrainian-focused goals** and **curator ecosystem** expectations are satisfied.

Business acceptance is based on a combination of:

- coverage of **Must-have (M)** business requirements from Section 7;  
- a minimal set of **non-functional expectations** from Section 8;  
- **early usage and engagement signals** observed within the first months after launch.

---
<br>

### 11.1 Acceptance Scope & Timeframe

- These criteria apply to the **Dream Project MVP** as defined in this BRD and the Business Case.  
- Unless explicitly stated otherwise, the quantitative signals are evaluated over the **first four months after MVP launch**.
- Acceptance is considered at the level of the **whole product**, not individual stories.

---
<br>

### 11.2 Functional Acceptance – Core Pillars

#### 11.2.1 Save Management

Business acceptance requires that:

1. **End-to-end save flows work for supported games**  
   - A signed-in user can:
     - upload a save file for a **supported game/platform variant**,  
     - see it appear in their personal list with basic metadata (BR-SAV-01, BR-SAV-03),  
     - optionally mark it as shared (BR-SAV-02).  
   - For games where save sharing is **not viable**, the game page clearly reflects this (BR-SAV-05).

2. **Shared saves can be discovered and used where supported**  
   - For at least a small but non-trivial subset of games where save sharing is possible, users can:
     - search/browse shared saves by game and platform variant (BR-SAV-04),  
     - understand from metadata roughly **where in the game** the save is (early/mid/late or key point).  
   - Save usage guidance and warnings are present when a user downloads/uses a shared save (BR-SAV-06).

3. **Basic quality and safety controls are in place**  
   - Users can report problematic saves (corrupt/misleading/malicious) and see that reports are acknowledged in the UI (BR-SAV-07).  
   - At least minimal moderation actions (hide/remove) can be performed by Admins, and visible outcomes are reflected in the product.

If these conditions are met for a representative sample of games (retro and modern PC where technically possible), the **Save Management pillar** is considered functionally acceptable.

---
<br>

#### 11.2.2 Achievement Tracking

Business acceptance requires that:

1. **Official and retro achievements can be viewed and linked**  
   - For selected Steam titles, users with linked Steam accounts can:
     - view the official achievement list, and  
     - see which achievements are marked as completed for them (BR-ACH-01, BR-ACH-03, BR-ACH-04).  
   - For selected RetroAchievements-supported games, users with linked RA profiles can:
     - see retro sets and completion status inside Dream Project (BR-ACH-02, BR-ACH-03, BR-ACH-04).

2. **Fan-made (Dream Project) achievement sets exist and are usable**  
   - For a meaningful number of games (at least **10 fan-made sets or subsets** as per Business Case signals), Curators can:
     - define and edit sets with named achievements, descriptions and tags (BR-ACH-05, BR-ACH-06),  
     - optionally add hints and external resources (e.g. GameFAQs, YouTube) for at least some achievements.  
   - Regular users can:
     - join a fan-made set,  
     - mark achievements as not started / in progress / completed (BR-ACH-07).

3. **Evidence and discussion are supported (even if partial)**  
   - At least some achievements in fan-made sets support **evidence links** (screenshots, save entries or clip URLs) (BR-ACH-08).  
   - Users can comment on at least the **set level**, and ideally on individual achievements (BR-ACH-10), with comments subject to moderation.

4. **Completion statistics are visible and meaningful**  
   - For official, retro and fan-made sets where enough data exists, the platform shows:
     - percentage of users who have completed the set,  
     - list of users (nickname + timestamp) who completed it,  
     - basic “time to completion” where it can reasonably be computed (BR-ACH-12).

If users can **see, track and talk about their achievements** in these three dimensions (official, retro, fan-made), Achievement Tracking is considered functionally acceptable.

---
<br>

#### 11.2.3 Sticker Albums

Business acceptance requires that:

1. **Curators can define album templates for real games**  
   - For at least **50 album templates across 50 distinct games** (Business Case signal), Curators can:
     - define album metadata (title, description, type),  
     - configure sticker slots with names, descriptions, spoiler flags and optional hints (BR-ALB-01, BR-ALB-02).

2. **Users can actually fill albums with screenshots**  
   - Regular users can:
     - create an album instance from a template,  
     - upload and assign screenshots to specific slots (BR-ALB-03, BR-ALB-04),  
     - see which slots are empty, filled, or accepted.  
   - Spoiler-sensitive slots behave as expected (blurred/hidden until revealed and clearly labelled) (BR-ALB-05).

3. **Album completion and badges work**  
   - When a user fills all required slots for a template:
     - the album is marked as completed,  
     - a game-specific badge appears in their profile (BR-ALB-07),  
     - completion appears in album-level statistics (BR-ALB-08).

4. **Safety checks are at least minimally active**  
   - An initial form of **AI/rule-based validation** exists for uploaded screenshots (e.g. detecting obviously unrelated/inappropriate content) (BR-ALB-06, even if in a simple pilot form).  
   - Inappropriate screenshots can be reported and moderated.

If these behaviours are available and used by real users, the **Sticker Album pillar** is functionally acceptable.

---
<br>

### 11.3 Functional Acceptance – Profiles, Governance & Catalogue

#### 11.3.1 Profiles, Roles & Progress Views

Business acceptance requires that:

- New users can **register, sign in and sign out**; a profile is created with at least nickname, language preference and basic visibility settings (BR-ACC-01, BR-ACC-02, BR-PRO-01).  
- A user’s **role (Regular / Curator / Admin)** is visible in their profile or settings, and Admins can change roles (BR-ACC-03, BR-PRO-02).  
- At least one working path for **Curator promotion** exists:
  - either via a request/approval workflow, or manual elevation of clearly active users (BR-ACC-04).  
- For games a user interacts with, there is a **per-game progress page** summarising:
  - their saves,  
  - their progress in official/retro/fan-made achievement sets,  
  - their album instances and completion status (BR-PRO-03).  
- Basic privacy controls work: setting content to private/limited prevents it from being visible publicly (BR-PRO-05).

---
<br>

#### 11.3.2 Governance & Moderation

Business acceptance requires that:

- There is a clear **role model** in the running system (Regular User, Curator, Admin) with permissions aligned to BR-GOV-01/04.  
- Users can **report** problematic saves, screenshots, achievement sets/items, albums, comments or profiles (BR-GOV-02).  
- Admins see a **moderation queue** and can take actions (keep/hide/remove, warn or restrict) with visible effects in the product (BR-GOV-03).  
- Curators have extended **content-creation powers** but not full Admin privileges, and their actions remain subject to moderation and policy (BR-GOV-04).

---
<br>

#### 11.3.3 Catalogue, Localisation & UA/Russia Policy Flags

Business acceptance requires that:

- A **game catalogue** exists for a curated subset of games with:
  - entries for game + platform variants,  
  - basic descriptions and tags (BR-CAT-01).  
- For relevant titles, catalogue entries hold correctly configured flags for:
  - “Has Ukrainian localisation”,  
  - “Ukrainian-origin game”,  
  - “Russian-linked publisher/developer” (BR-CAT-02, BR-CAT-03).  
- For users using the **Ukrainian locale**:
  - UA-localised and UA-origin games can be **highlighted or filtered**,  
  - Russian-linked games display appropriate indicators or notes according to platform policy (BR-CAT-04).  
- Game pages clearly show **which pillars are available** (save sharing, achievements, albums) and where there are technical or policy restrictions (BR-CAT-05).

---
<br>

### 11.4 Non-Functional Acceptance (Business View)

For MVP acceptance, the following **non-functional expectations** from Section 8 must be demonstrably met at a reasonable level:

1. **Usability & Onboarding**  
   - New signed-in users can understand **what to do first** (e.g. pick a game, start an album) without reading long documentation (NFR-UX-01, NFR-UX-05).  
   - Core flows (upload save, join set, create/fill album) can be completed in a small number of steps with clear, human-readable messages (NFR-UX-02..04).

2. **Performance & Responsiveness**  
   - In normal conditions, key pages (home, game pages, profile, set/album views) feel responsive; initial content appears within a few seconds and navigation is not sluggish (NFR-PERF-01, NFR-PERF-02).  
   - External sync (Steam/RetroAchievements) does not block normal navigation and presents realistic expectations about update timings (NFR-PERF-03).

3. **Security & Privacy Basics**  
   - The product is deployed over HTTPS; user credentials are stored securely; only minimal personal data is collected (NFR-SEC-01, NFR-SEC-02).  
   - External linking (Steam, RetroAchievements) is clearly explained to the user, with visible unlink options (NFR-SEC-03).  
   - Visibility defaults are conservative: content is not unexpectedly exposed (NFR-SEC-05).

4. **Localisation & UA-Specific Content**  
   - The UI is usable in **English and Ukrainian** for core flows (navigation, game pages, key policy explanations) (NFR-LOC-01, NFR-LOC-02).  
   - UA users can understand:
     - what UA-origin and Russian-linked flags mean,  
     - how to filter or interpret them in the catalogue.

If significant gaps exist in these areas, business acceptance is deferred until a corrective plan is agreed.

---
<br>

### 11.5 Early Usage & Engagement Signals

While MVP acceptance is not purely driven by metrics, the following **early signals** are expected within roughly the first four months after launch (aligned with the Business Case):

1. **Core Community Activity**  
   - Roughly **100 registered users** have performed meaningful actions (e.g. saves uploaded, achievements tracked, album slots filled) within a 30-day window by month four.  
   - A mixed audience is present, with around **75% English-speaking and 25% Ukrainian-speaking** users among active accounts.

2. **Use of Achievements & Albums**  
   - At least **50 distinct sticker album templates** exist for **50 different games**, and a non-trivial number of users have started or completed them.  
   - At least **400 achievement completion records** are stored (official, retro, fan-made combined), indicating real use of tracking features.

3. **Curator & Content Ecosystem**  
   - At least **5 users** are acting as active Curators (creating or maintaining sets and albums), with at least **3 from the Ukrainian community**, as visible in the live system.  
   - At least **10 fan-made achievement sets or subsets** are available and have some level of player interaction (joins, completions, comments).

If these thresholds are **not fully met** but the trends are clearly positive, acceptance may still be granted provided there is a realistic plan to improve adoption.  
If activity is near-zero despite the features working technically, the product may be considered technically acceptable but **not yet validated** from a business perspective.

---
<br>

### 11.6 Acceptance Decision & Follow-Up

- The **final business acceptance decision** is made jointly by:
  - the Product Owner / Founder,  
  - a representative Admin / Operator,  
  - at least one Curator representative (informal, but valuable input).

- Acceptance may be:
  - **Full** – criteria broadly met; MVP considered live and stable.  
  - **Conditional** – key criteria met, but with specific actions agreed (e.g. improve UA documentation, refine curator tools).  
  - **Deferred** – critical gaps in core pillars, governance, or UA policy handling prevent acceptance.

All acceptance outcomes should be documented briefly, with:

- which criteria were met or missed,  
- agreed follow-up actions,  
- a target timeframe for reassessment where needed.

This ensures that Dream Project MVP is not only **functionally complete**, but also aligned with its **business goals, UA focus, and community-driven vision** before being treated as a stable baseline for future iterations.

---
<br>
<br>

## 12. Traceability & Related Artefacts

This section describes **how the requirements in this BRD connect to other artefacts** in the Dream Project documentation set and how they will be traced forward into design, implementation and testing.

The goal is that any stakeholder can answer:

- “Where did this requirement come from?”  
- “Where is it refined into more detail?”  
- “How will we know it was implemented and tested?”

---
<br>

### 12.1 Traceability Approach

Dream Project uses **stable IDs** for requirements and non-functional expectations:

- **Business Requirements (BR)** – e.g. `BR-SAV-01`, `BR-ACH-05`, `BR-CAT-03`.  
- **Non-Functional Requirements (NFR)** – e.g. `NFR-UX-01`, `NFR-SEC-02`.

These IDs are intended to appear consistently in:

- this **BRD** (origin and business intent),  
- the **System Requirements Specification (SRS)** – detailed, more technical requirements,  
- the **Requirements Traceability Matrix (RTM)** – mapping BR/NFR → SRS → user stories → test cases,  
- design and diagram artefacts, where relevant,  
- test cases and test reports.

Each new requirement introduced later (e.g. during change requests) should receive a new ID or a clearly versioned variant, so changes can be traced over time.

---
<br>

### 12.2 Upstream Links – Where the BRD Comes From

The requirements in this BRD are **not standalone**; they are derived from earlier BA artefacts:

- **Business Case**  
  - Defines the high-level problem, target users, business objectives and success metrics.  
  - Key links:  
    - Section 3 of this BRD (Business Objectives & Success Criteria) reflects the Business Case goals.  
    - Section 11 (Business Acceptance Criteria) translates those goals into acceptance conditions.

- **Business Analysis Document**  
  - Provides detailed **business context, stakeholder analysis, AS-IS/TO-BE views, business rules and data view**.  
  - Key links:  
    - Stakeholder needs and pain points → Section 5 (Stakeholder & User Overview) and Section 7 (BR catalogue).  
    - Business rules around roles, achievements, albums, UA/Russia flags → Sections 7 and 8 (Non-functional view).

- **Stakeholder & Communication Plan**  
  - Identifies stakeholder groups, their influence/interest, and how they are engaged.  
  - Key links:  
    - Explains **who** needs to review and sign-off this BRD and later changes.  
    - Informs prioritisation and acceptance (e.g. Ukrainian community expectations for catalogue policies).

These artefacts answer **“Why do these requirements exist at all?”**.  
If a BR or NFR appears misaligned with them, it should be reviewed.

---
<br>

### 12.3 Downstream Links – Where the BRD Leads To

The BRD serves as the **business-level source** for more detailed and technical artefacts:

- **System Requirements Specification (SRS)**  
  - Will break each relevant `BR-…` and `NFR-…` into:
    - functional requirements,  
    - interface definitions,  
    - data models,  
    - error and edge-case behaviour.  
  - Each SRS requirement should reference at least one BR/NFR ID from this document.

- **Requirements Traceability Matrix (RTM)**  
  - A separate artefact that will explicitly map:
    - `BR-…` / `NFR-…` → SRS requirements → user stories/tasks → test cases.  
  - Also used to track:
    - which BRs are fully implemented,  
    - which have partial coverage or are deferred,  
    - which tests validate each BR/NFR.

- **Visual & Design Artefacts (Diagrams & Wireframes)**  
  - Context diagrams, process diagrams, ERD, wireframes/low-fi mockups, etc.  
  - Where appropriate, key diagrams should reference BRD sections or IDs (e.g. notes like “covers BR-SAV-01..05”).

- **Test Cases & Test Reports**  
  - Higher-level test cases (especially UAT / business tests) should explicitly reference:
    - the BR/NFR IDs they validate, and  
    - the acceptance criteria in Section 11.

---
<br>

### 12.4 Change Management & Versioning

To keep traceability useful over time:

- Any **new or changed business requirement** should:
  - be added or updated in this BRD with a clear ID (and, if relevant, a short note about the change),  
  - be reflected in the SRS and RTM,  
  - be communicated to key stakeholders as defined in the Stakeholder & Communication Plan.

- When requirements are **re-scoped or de-prioritised** (e.g. a “Should” becomes a “Nice-to-have” or is moved out of MVP), the RTM and any affected test plans should be updated accordingly.
