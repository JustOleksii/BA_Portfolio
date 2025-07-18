# BA Documentation Deliverables

Hello and welcome to the End of Time ⏳
Or, if seriously, it's the text part of the Business Analysis essential documentation for **Dream Project**. Here, you’ll find every crucial artifact that defines my project’s goals, scope, and roadmap. Just click any heading below to jump straight to the details you need.

## 📑 What’s Inside

* 🔹 [Business Case](#business-case)  
* 🔹 [Business Analysis Document](#business-analysis-document)  
* 🔹 [Business Requirements Document (BRD)](#business-requirements-document-brd)  
* 🔹 [Use Cases](#use-cases)  
* 🔹 [System Requirements Specification (SRS)](#system-requirements-specification-srs)  
* 🔹 [Functional Requirements](#functional-requirements)  
* 🔹 [Non-Functional Requirements](#non-functional-requirements)  
* 🔹 [Data Mapping Requirements](#data-mapping-requirements)  
* 🔹 [Risk Analysis](#risk-analysis)  
* 🔹 [Project Plan](#project-plan)  
* 🔹 [Gap Analysis](#gap-analysis)  
* 🔹 [Test Cases](#test-cases)  

---

*Feel free to scroll down or click any link above to navigate directly to the section you’re looking for.*  


# Business Case

## Executive Summary

Dream Project is a passion-driven web platform for gamers, first of all for retro-console and legacy-game fans, but not forgetting all the other games that were officially released on PC. It brings together four user roles—Regular Players, List Creators, Curators, and Admins—to:

- Securely back up and restore save files  
- Track both official and fan-made achievements  
- Craft collectible screenshot “sticker” albums  

By merging these features, we’re planning to preserve gaming history and foster creativity and community among players worldwide.

## Background & Problem Statement

Retro gamers often juggle hardware backups or third-party emulators to save progress—and even then, achievements are scattered across platforms or hidden in niche sites. The same issue applies to modern games, which, for example, do not have enough attention or popularity, so they also do not have an easy search for saves. 
There’s no single place to organize screenshots, list fan challenges, and share memorable playthrough moments. This fragmentation risks losing personal gaming legacies and makes it harder for communities to celebrate and build on shared experiences.

## Business Objectives

Over the next six months, we aim to:

1. **Ship the MVP**  
   - Launch Save Management, Achievement & Milestone Tracker, and Sticker Album modules.

2. **Grow Our Community**  
   - Onboard 200 active players (≈150 English-speaking, ≈50 Ukrainian)  
   - Recruit 20 challenge creators (≈15 EN, ≈5 UA)

3. **Measure Early Engagement**  
   - Support 50 sticker albums  
   - Capture 400 achievement entries within three months of launch

4. **Build Leadership**  
   - Enlist 5 volunteer curators (with at least 3 from the Ukrainian community) by month four

## Scope & Deliverables

### 🚀 In Scope (MVP)

#### Save Management Module
- Encrypted upload/download and version control for save files  
- Search by game title; filter by playtime, chapter, etc.  
- Community feedback (likes/dislikes) and manual reporting for moderation

#### Achievement & Milestone Tracker Module
- Auto-import of official achievements from Steam; manual support for fan lists.
- One “primary” manual list per game, plus optional variants (e.g., low-level challenges).
- Comments, ratings, and admin review workflows on each achievement set.
- Dedicated pages with instructions, feedback, and media

#### Sticker Album Module
- Create themed screenshot collections unlocked by in-game achievements

#### Profile Management
- Track completion progress and award profile badges  
- Auto-generated user profiles stats showcasing recent activity, achievements, and album stickers

### Core Platform Services
- Role-based authentication and permissions  
- RESTful API endpoints for every module

### 🚧 Out of Scope (Phase II+)
- Direct emulator or proprietary SDK integrations  
- Automatic PC/Steam achievement syncing (deferred to manual entry)  
- Native mobile applications

## Who’s Involved (Stakeholder Analysis)

| Who               | Role                         | What They Care About                             | Influence Level |
|-------------------|------------------------------|--------------------------------------------------|-----------------|
| Regular Players   | Platform users               | Safe saves, unified achievement & album view     | High            |
| List Creators     | Fan challenge designers      | Publishing challenges, community recognition     | Medium          |
| Curators          | Content organizers           | Quality control, highlighting top collections    | Medium          |
| Platform Admins   | System maintainers           | Security, policy enforcement, system uptime      | High            |
| Community Reps    | UA/EU local advocates        | Regional engagement, localized support           | Low             |

## Why It Matters (Value Proposition)

- **Regular Players** You never lose progress; enjoy a central dashboard for achievements; express creativity through sticker albums.  
- **List Creators** Craft and share fan-made challenges; build reputation within the community.  
- **Curators** Discover, organize, and spotlight the best albums; drive deeper engagement.  
- **Admins** Manage content and security from a unified control panel.

## Cost Estimate & Break-even Analysis

- **Team (6 months):**  
  - 2 Developers (Ukraine/Poland) at \$2,000/mo each  
  - 1 Business Analyst & 1 QA Engineer at \$1,500/mo each  
- **Infrastructure:** \$3,000 total  
- **Total Budget:** \$45,000 (Developers \$24k + BA/QE \$18k + Infra \$3k)

**Break-even Plan:**  
Platform access is free at launch. We’ll rely on community donations initially and introduce premium API offerings in Phase II. Break-even targets will be refined based on post-MVP user engagement metrics.

## Risks & Assumptions

### Assumptions
- Game metadata is accessible and accurate.   
- Volunteers (especially from Ukraine) will engage as curators.  
- Users are comfortable sharing screenshots and save data. 

### Risks & Mitigations
- **Moderation Overload.** Start with manual reviews; plan automated flagging in Phase II.  
- **Data Compatibility.** Build modular adapters for different save formats.  
- **Legal/Copyright.** Draft clear Terms of Service and DMCA takedown processes.  
- **Low Local Adoption.** Target outreach to Ukrainian gaming communities.  
- **Security Threats.** Encrypt data at rest/in-transit; schedule frequent security audits.

---
<br>
<br>
<br>
<br>
<br>
<br>

# Business Analysis Document

Welcome to the heart of Dream Project - **The BA document**. This document brings together all the critical BA artifacts you need to understand our project’s purpose, the people it serves, and how we’ll get there. Here you’ll find:

* [Business Context & Objectives.](#business-context--objectives) 
Why our project exists, the problems we’re solving, and the goals we’re aiming for.
* [Stakeholder Analysis Summary.](#stakeholder-analysis-summary) 
  A snapshot of who’s involved, what they care about, and how they influence the project.
* [Process Analysis.](#process-analysis)
  A deep dive into how things work today and how they should work tomorrow.
   * [Current State (As-Is).](#current-state-as-is)
  A realistic look at our starting point—systems, workflows, and pain points.
   * [Future State (To-Be).](#future-state-to-be)
  The vision for streamlined processes, new capabilities, and improved experiences.
* [Business Rules.](#business-rules)
  The guiding principles and policies that keep everything running smoothly.
* [Assumptions & Constraints.](#assumptions--constraints)
  What we’re taking for granted and the limits we need to work within.
* [High-Level Business Requirements.](#high-level-business-requirements)
  The must-have features and outcomes that will drive success.
* [Glossary.](#glossary)
  Definitions of key terms to keep everyone on the same page.

---

*(Scroll down for detailed content on each section.)*  

---
<br>
<br>
<br>

## Business Context & Objectives

### Business Context  
**Dream Project** serves both retro-console classics and modern PC/console titles by unifying progress preservation, achievement tracking, and visual storytelling. Currently, preservation of save states relies on disparate fan-developed tools/emulator snapshots and custom challenge lists, while official achievement sets often neglect side-quests, collectibles, and other nuanced milestones. Enthusiasts may curate extensive challenge lists and screenshot galleries, yet they lack a centralized platform to host, organize, and share this content. The **Dream Project** unifies these capabilities—consolidating official and community-created achievements, offering robust, cross-platform save-file management, and introducing a themed screenshot “sticker” album feature—within a single, web-based environment.

### Business Objectives  
Over the next six months, our primary objectives are to:

1. **Deliver the MVP**  
   - Implement and launch the Save Management, Achievement & Milestone Tracker, and Sticker Album modules.

2. **Expand User Base**  
   - Enroll 200 active users (approximately 150 English-speaking and 50 Ukrainian-speaking).  
   - Recruit 20 dedicated challenge creators (approximately 15 English-speaking and 5 Ukrainian-speaking).

3. **Foster Early Engagement**  
   - Facilitate the creation of at least 50 sticker albums.  
   - Capture a minimum of 400 achievement entries within three months of product release.

4. **Establish Community Leadership**  
   - Appoint 5 volunteer curators—ensuring at least 3 are members of the Ukrainian gaming community—by the end of month four.

### Success Criteria & KPIs
To monitor our progress and validate success, we will track:

- **Monthly Active Users (MAU):** Number of unique regular users engaging with core features.  
- **Content Volume:** Totals of save-file uploads, achievement entries recorded, and sticker-album completions.  
- **Engagement Ratio:** Proportion of active content creators (list creators and curators) relative to regular users.  
- **Regional Adoption:** Breakdown of user registrations by language/locale, to assess local (Ukrainian) versus global uptake.

---
<br>
<br>

## Stakeholder Analysis Summary

To build a platform that truly resonates with our community, we’ve mapped out the key players, what they care about most, and how much sway they have in shaping **Dream Project**.
| Stakeholder             | Role                             | Needs/Concerns                                        | Influence |
|-------------------------|----------------------------------|-------------------------------------------------------|-----------|
| Regular Users           | Primary platform participants    | Reliable save backups; complete achievement tracking; easy album creation | High      |
| List Creators           | Fan challenge designers          | Robust list tools; clear version control; community visibility  | Medium    |
| Curators                | Content organizers & moderators  | Efficient curation tools; featured album showcases; moderation controls | Medium    |
| Platform Administrators | System operators & enforcers     | Platform stability; security compliance; user support workflows    | High      |
| Local Community Reps    | Regional gaming ambassadors      | Localized UI & language support; community engagement; accessibility   | Low       |

---
<br>
<br>

## Process Analysis

### Current State (As-Is)

- **Save Management**  
   Today, if you’re playing on Steam or a similar modern platform, your game progress quietly syncs to the cloud—but there’s no user-friendly way to browse or share someone else’s save files. Retro-console fans are even more left out: their saves live on local files, and while tools like RetroAchievements let them export snapshots, there’s no central cloud repository or unified interface.

- **Achievement & Milestone Tracking**  
  Official achievement systems on PC and console platforms are generally comprehensive; however, they frequently omit side-quests, collectibles, and custom milestones. Fan-driven challenge lists—such as those on RetroAchievements—offer extensive coverage for legacy titles, yet no single platform consolidates official achievements with community-created lists.

- **Screenshot Curation**  
  Although modern platforms store screenshots in the cloud, they do not facilitate thematic organization or album creation. Retro-gaming communities share images via forums, Reddit threads, or ad hoc image hosts—without any structured way to assemble or showcase a “best-of” album.

---

### Future State (To-Be)

- **Save Management Module**  
  A unified web interface enabling users to upload save files, view version history, and filter by title, playtime, or popularity. Community feedback features (likes/dislikes) and reporting workflows will support content moderation.

- **Achievement & Milestone Tracker Module**  
  An integrated view of official achievements and community-created challenge lists, underpinned by standardized templates for list creation. Inline commenting and rating mechanisms will allow users and administrators to ensure list quality.

- **Sticker Album Curation**  
  A drag-and-drop album builder for organizing screenshots into themed collections. Automatic progress indicators and profile badges will denote completion status.

- **Role-Specific Dashboards**  
  Tailored interfaces for Regular Users, List Creators, Curators, and Administrators. The Administrator dashboard will include moderation queues for flagged save files and achievement lists.  

---
<br>
<br>

## Business Rules

### User Accounts & Roles
- **Role-Based Access**  
  - **Regular Users** can browse and search save files, achievement lists, and sticker albums—and like, dislike, or report any content.  
  - **List Creators** design, edit, and publish fan-made achievement lists.  
  - **Curators** define album templates, organize featured collections, and add descriptions or spoiler warnings.  
  - **Platform Administrators** have full moderation controls and system-management interfaces.  
- **Progress Tracking**  
  Each user profile automatically shows recent game activity, playthrough progress (level, chapter, playtime), counts of completed achievement sets and sticker albums, plus any earned badges, and overall completion percentages.

### Save Management Rules
- **Unique File Metadata**  
  Every save file entry includes game title, platform, version, playtime, and chapter/level.  
- **Version Control**  
  Users can keep up to 25 versions per game; older versions are archived but can be restored or deleted at will.  
- **Reporting & Moderation**  
  Any user may report save files for corruption, malware, or mismatches. Reported items display a warning banner until an admin reviews them.

### Achievement Tracking Rules
- **Official vs. Fan-Made**  
  Official achievements are auto-imported and clearly labeled. Fan-made lists can be created, versioned, and collaboratively edited.  
- **Collaborative Lists**  
  Multiple active fan-made challenge lists per game are allowed, including themed subsets (e.g., difficulty modes). Creators can collaboratively add, remove, or edit achievements. 
- **Commenting & Rating**  
  Users can start threaded discussions on individual achievements and rate full lists. Flagged or low-rated items enter the moderation queue.  
- **Dedicated Achievement Pages**  
 Each achievement has a dedicated page with criteria description, media attachments, comment threads, and optional flags (e.g., storyline achievement, irreversible unlocks, final completion indicator).
 - **Completion Statistics**: Display percentage of users who have completed each achievement and lists of users (with nicknames, avatars, and timestamps) who achieved completion. Show real time taken to unlock achievement sets.

### Sticker Album Rules
- **Custom Templates**  
Curators define album templates (locations, characters, plot elements, etc.) with descriptions and optional spoiler warnings. Multiple themes per game are supported. 
- **Upload & Validation**  
  Screenshots are dragged into album slots; an AI-driven system verifies consistency and filters out inappropriate content.  
- **Spoiler Protection**  
  Images flagged as spoilers are auto-blurred until a user opts to reveal them.  
- **Completion Badges**  
  Completing an album grants a game-specific badge on the user’s profile. 
 - **Completion Statistics**: Display percentage of users who have completed each sticker album and list of users (nicknames, avatars, timestamps) who completed it, along with real time taken. 
- **Content Ownership**  
  Users retain full rights to their uploads; only the uploader or admins can remove screenshots.

### Moderation & Reporting
- **Reporting Workflow**  
  Items reported by users (saves, achievements, screenshots) are flagged and remain visible with cautionary graphics until reviewed.  
- **Administrative Review**  
  Admins prioritize content in a moderation queue based on report severity and volume.  
- **Appeals Process**  
  Content owners may appeal removal decisions within seven days.

### Data Retention & Privacy
- **Save Retention**  
  Saves not accessed for one year are archived, with advance notification to the user before deletion.  
- **Screenshot Retention**  
  Screenshots in sticker albums are stored indefinitely.  
- **Privacy Compliance**  
  All personal data is encrypted in transit and at rest, following GDPR-like standards.  
- **API Rate Limits**  
  Public API access is capped at 1,000 requests per hour per registered key to ensure system stability.  

---
<br>
<br>

## Assumptions & Constraints

### Assumptions
- **Access to Game Metadata**  
  We’re counting on modern platforms (Steam, Epic Games, etc.) to expose achievement and save data via their APIs, and on popular retro emulators (RetroArch, Dolphin, PCSX2) to offer plugins or memory-reader hooks for extracting save snapshots and custom triggers.

- **Emulator Integration**  
  A set of popular emulators will be supported at launch, with community‑maintained plugins available for data parsing.

- **Active Community Participation**  
  Curators, list creators, and regular users within both English‑speaking and Ukrainian communities will actively contribute to content creation, moderation, and validation processes.

- **User Technical Competence**  
  Users know how to export saves from their retro emulators and capture/upload screenshots without hand-holding.

- **Reliable AI Validation**  
  Automated image-verification service should hit at least 90% accuracy in matching uploads to the templates defined by curators.

- **Stable Internet Connectivity**  
  End users have a dependable connection for uploading and downloading saves, achievements, and screenshots.

### Constraints
- **Team & Timeline**  
  MVP development is limited to a four-person team (two developers, one business analyst, one QA engineer) over six months.

- **Fixed Budget**  
  Total MVP budget is capped at $45K, covering personnel, infrastructure, and essential tooling. Anything beyond the MVP features needs new funding.

- **Emulator Support Scope**  
  Only pre-selected list of emulators will be integrated in Phase I; support for others or proprietary tools comes later in Phase II.

- **API Limitations**  
  External APIs may throttle requests or require licensing—so manual data-entry must be available as a fallback.

- **Browser Compatibility**  
  Support the latest versions of Chrome, Firefox, and Edge. Legacy browsers and mobile-only interfaces are out of scope for the MVP.

- **Storage & Retention**  
  User storage quotas and a global cap keep us within budget. Saves not accessed for a year will be archived, while sticker-album images stay indefinitely (within quota limits).

- **Regulatory Compliance**  
  Must adhere to GDPR-style privacy rules and offer DMCA takedown processes for copyrighted content.

- **Security & Performance**  
  All data must be encrypted in transit and at rest. Public APIs will be rate-limited to 1,000 requests/hour per key, and the system must support at least 500 concurrent users without slowing down.  

---
<br>
<br>

## High-Level Business Requirements

Below are the must-have capabilities (what the platform does) and quality attributes (how it performs) for Dream Project.

### Functional Requirements

**Save Management**  
- Players can upload, download, and securely store game save files with full metadata (title, platform, version, playtime, chapter).  
- Browse, filter, and sort saves by game, progress markers, popularity, or creator.  
- Keep up to 25 versions per game, with options to archive, restore, or delete older saves.  
- Community feedback is built in—anyone can like, dislike, or report a save, and flagged items display a warning banner until reviewed.

**Achievement & Milestone Tracking**  
- Automatically import and label official achievements via platform APIs.  
- Create, edit, and collaboratively version multiple fan-made achievement lists per game.  
- Comment on and rate lists; low-rated or flagged lists enter a moderation queue.  
- Each achievement has its own page with criteria descriptions, media attachments, tags (e.g., “storyline,” “irreversible”), and completion stats (users and timestamps).

**Sticker Album Collection**  
- Curators design custom album templates (locations, characters, plot elements) with optional spoiler notices.  
- Players drag-and-drop screenshots into template slots once they meet in-game criteria; an AI check verifies content consistency.  
- Albums display completion percentages, award badges, and feature leaderboards showing avatars, nicknames, and timestamps.  
- Spoiler images stay blurred until a user chooses to reveal them.

**User Profiles & Progress Tracking**  
- Profiles automatically showcase recent game activity, overall completion stats, counts of completed achievement sets and sticker albums, badges earned, and time-to-complete metrics.

**Moderation & Reporting**  
- Role-based access for Regular Users, List Creators, Curators, and Administrators.  
- A centralized moderation queue handles all reported content, and content owners can appeal removal decisions within seven days.

**API & Integration**  
- Provide RESTful endpoints for save management, achievements, albums, and user profiles.  
- Enforce a 1,000-requests/hour rate limit per API key, with manual data-entry fallbacks when external APIs hit their limits.

### Non-Functional Requirements

**Performance & Scalability**  
- Support a minimum of 500 concurrent users without noticeable lag.  
- Automatically scale storage and compute resources based on demand.

**Security & Compliance**  
- Encrypt all data in transit and at rest; comply with GDPR-like privacy standards and offer DMCA takedown workflows.  
- Implement OAuth2 (or an equivalent) for authentication and authorization.

**Availability & Reliability**  
- Maintain at least 99.5% uptime, with fault-tolerant design and disaster recovery plans.

**Localization & Accessibility**  
- Offer full English and Ukrainian UI localization; meet WCAG 2.1 AA accessibility guidelines.

**Maintainability**  
- Modular architecture with clear separation of concerns; comprehensive logging and monitoring.

---
<br>
<br>

## Glossary

- **Save File**  
  A snapshot of your game right where you left off (level, chapter, playtime, and so on).

- **Achievement**  
  A milestone you unlock in-game. It can be an official trophy imported from the platform or a fan-made challenge dreamed up by the community.

- **Fan-Made List**  
  A custom collection of achievements created and maintained by players—perfect for adding extra challenges to your favorite games.

- **Sticker Album**  
  A themed digital scrapbook where you “stick” your best screenshots into slots defined by curators. Complete the album and earn badges!

- **Curator**  
  A user role responsible for designing album templates, organizing content, and enforcing quality standards.

- **List Creator**  
  The architect of fan challenges—this role builds and edits custom achievement lists for the community to tackle.

- **Regular User**  
  Anyone who plays, backs up saves, tracks achievements, and builds sticker albums—without the extra powers of a curator or list creator.

- **Platform Administrator**  
  The gatekeeper and moderator—this role has full control over system settings, user management, and content moderation.

- **MVP (Minimum Viable Product)**  
  The first fully working version of Dream Project, including the core features: save management, achievement tracking, and sticker albums.

- **Metadata**  
  The “data about data” for your saves and achievements—details like game title, platform, version, playtime, and chapter.

- **AI Validation**  
  An automated check that uses machine learning or image recognition to make sure your uploaded screenshots match the curator’s template.

- **API (Application Programming Interface)**  
  A set of RESTful endpoints that allow external applications and services to interact with Dream Project’s core modules.

- **WCAG (Web Content Accessibility Guidelines)**  
  The rulebook for making our platform accessible to everyone, including people with disabilities (we follow WCAG 2.1 AA standards).

---
<br>
<br>
<br>
<br>
<br>
<br>

# Business Requirements Document (BRD)

The Business Requirements Document (BRD) formalizes the detailed business needs, objectives, scope boundaries, and stakeholder expectations for Dream Project / Zanarkand Project. Consider it the single source of truth that keeps everyone—from developers to stakeholders—aligned on our goals and deliverables.

## Document Structure <a name="document-structure"></a>
Jump to any section below:

- [Purpose & Scope](#purpose-and-scope)  
- [Stakeholder Requirements](#stakeholder-requirements)  
- [Business Needs & Objectives](#business-needs-and-objectives)  
- [Detailed Business Requirements](#detailed-business-requirements)  
- [Acceptance Criteria](#acceptance-criteria)  
- [Constraints & Assumptions](#constraints-and-assumptions)  
- [Approval & Sign-off](#approval-and-sign-off)  

---

