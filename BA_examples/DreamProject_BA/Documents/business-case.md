# Business Case

## Document Structure

Below is an overview of the main sections of this Business Case. This document walks you through the reasoning behind the project — from understanding the problem and defining the objectives to outlining the proposed solution and its expected value.    
Use the links to jump directly to the part you’re interested in and see how the project rationale is built step by step.

- **[1. Introduction](#1-introduction)**  
  Short context for the document: why this Business Case exists, who it’s intended for, and how stakeholders can use it as a reference for decision-making.

- **[2. Problem Statement](#2-problem-statement)**  
  Outlines the main business or user pain points that Dream Project is designed to address. Helps everyone align on *what exactly* we are trying to solve before discussing solutions.

- **[3. Business Objectives & Success Criteria](#3-business-objectives--success-criteria)**  
  Defines the key business goals for Dream Project and how success will be measured (qualitative and quantitative indicators).

- **[4. Solution Overview](#4-solution-overview)**  
   Describes the proposed solution at a high level: its core capabilities, intended users, and how it responds to the defined objectives — without delving into technical implementation.

- **[5. Scope](#5-scope)**  
  Clarifies what will be delivered in the initial phase and what is intentionally excluded. This helps prevent scope creep and sets realistic expectations from the start. 
  - **[5.1 In Scope](#51-in-scope)** – Lists the main functional and business areas that will be covered.  
  - **[5.2 Out of Scope](#52-out-of-scope)** – Notes areas or features that are excluded from the MVP or deferred to future phases.

- **[6. Stakeholder Overview](#6-stakeholder-overview)**  
  Summarises the main stakeholder groups, their interests, and their roles in the project. This shows who will be impacted by Dream Project and whose needs shape its requirements (detailed analysis is available in the different BA artifact).

- **[7. Benefits & Value Proposition](#7-benefits--value-proposition)**  
  Highlights the expected benefits for different stakeholder groups — including players, platform maintainers, and partners — and defines the value proposition that makes Dream Project worth pursuing.

- **[8. Assumptions & Constraints](#8-assumptions--constraints)**  
  Lists the assumptions and limitations shaping the project, such as technology choices, time, budget, or dependencies. Making these explicit ensures transparent and informed decision-making.

- **[9. Risks Summary](#9-risks-summary)**  
  Provides an overview of key risks identified at this stage, along with their potential impacts. Detailed mitigation plans can be found in the dedicated **Risk & Assumptions Register** file.

- **[10. High-Level Timeline & Dependencies](#10-high-level-timeline--dependencies)**  
  Presents an approximate delivery timeline, major milestones, and external dependencies. This helps understand when value can be expected and what may delay it.

- **[11. Financial & Effort Overview](#11-financial--effort-overview)**  
  Offers a high-level view of expected effort and cost ranges, together with benefits at a conceptual level. In this portfolio, figures are presented as relative estimates rather than exact budgets.

- **[12. Approval & Next Steps](#12-approval--next-steps)**  
  Lists the expected approvers and outlines what happens once the Business Case is approved — typically, the move into detailed analysis, planning, and early delivery stages.

---
<br>
<br>


## 1. Introduction

Dream Project is conceived as a product that turns scattered gaming progress into something structured, persistent, and meaningful. Modern players move between platforms, devices, and even living situations, but their stories are still locked inside local save files, fragmented achievement systems, and screenshots buried in folders. This Business Case explains why it is worth investing effort into solving that problem as a coherent product rather than leaving it to ad-hoc tools and manual workarounds.

The document sets the stage for all further analysis and design. It clarifies the context in which Dream Project appears, the business problem it addresses, and the value it aims to generate for its primary stakeholders: players, platform owners, and potential partners in the gaming ecosystem. The intention is not to describe every technical detail, but to provide enough structure so that later decisions about scope, requirements, and implementation can be traced back to a clear rationale.

Different people will look at this Business Case with different expectations. Sponsors and decision-makers will focus on the problem/benefit balance, the risks, and whether the proposed solution is realistic. Product and delivery roles (business analysts, product managers, designers, developers, QA) will use it as a reference point: a compact narrative that explains what “success” means and why certain trade-offs are acceptable while others are not.

You can think of this document as the entry point into the Dream Project artefact set:

- it **defines the “why” and the high-level “what”**, while leaving detailed behaviour and scenarios to the Business Requirements Document and System Requirements Specification;
- it **summarises key stakeholders, risks, and constraints**, with more granular tracking maintained in the Business Analysis Document and the Risk & Assumptions Register;
- it **sketches the high-level timeline and effort**, which is then expanded in the dedicated Project Plan.

By reading this introduction and the sections that follow, a stakeholder should be able to answer a few simple but critical questions: *What problem are we solving? For whom? Why now? And what kind of solution are we willing to invest in to address it?*

---
<br>
<br>


## 2. Problem Statement

Retro gamers often juggle physical hardware, memory cards, backup drives, and third-party emulators just to keep their progress alive. On modern platforms such as Steam, GOG, or EGS, cloud sync partially solves this: save files can be restored even after a game is uninstalled locally. However, this safety net is limited to a specific platform profile and a specific ecosystem. Sharing a save file with another player, moving progress between platforms, or preserving it independently of the vendor is still awkward, fragile, and highly technical for most users.

For achievements, the situation is similarly fragmented, but in a different way. For older titles that are not available in modern game stores, fans often create their own achievement sets, as seen on platforms like RetroAchievements. For games on modern PC and console platforms (PlayStation, Switch, Xbox), official achievements are the responsibility of the developers or publishers. Not all of them are willing—or able—to design rich, detailed sets that encourage players to explore the full depth of a game. As a result, many achievement lists are limited to broad milestones such as beating specific levels or unlocking endings, while whole systems, mechanics, and hidden content remain unrepresented.

Fans, on the other hand, tend to be far more creative. They can design additional sets or subsets of achievements that reward careful exploration, use of optional mechanics, or discovery of secrets. These fan-made sets can motivate players to approach games more thoughtfully and to engage with content that would otherwise be ignored. However, this layer of creativity usually lives outside official platforms, scattered across separate websites, communities, or tools. Tracking completion of such fan-made achievements is often “in your head” or in personal notes: there is no unified, reliable tracker that treats these sets as first-class citizens alongside official achievements.

The problem also touches how players treat screenshots. Modern platforms already make it easy to capture and share images from games, and even for retro titles it is possible to obtain screenshots through emulators, capture cards, or other workarounds. These images, however, usually exist as loose files or temporary social posts. They are not organised in a structured way and rarely serve as a consistent, long-term record of what a player has actually accomplished.

The core idea behind screenshots in Dream Project is different: they are treated as **visual proof of completion**, similar to stickers in a Panini album. Curators define album templates for each game with specific “slots” (key scenes, bosses, locations, events). Players fill these slots with their own screenshots, turning unstructured images into a visual, verifiable record of progress. At the moment, there is no widely adopted platform that uses screenshots in this structured, album-based way tied directly to achievement-like progress.

Overall, today, there is no single, structured place where a player can:

- bring together platform-specific saves, official achievements, and fan-made achievement sets for a game;
- track completion of both official and community-created achievements in a unified, platform-agnostic way;
- use screenshots as organised, template-based “stickers” that visually confirm specific in-game moments or conditions;
- keep this combined progress view persistent over time, independent of any one storefront, device, or social network.

As a result:

- personal gaming histories remain fragmented and fragile, spread across vendor accounts, community sites, local folders, and ad-hoc notes;
- players cannot easily demonstrate “full completion” of a game in a way that combines official achievements, richer fan-made sets, and visual evidence;
- communities that invest effort into designing better achievement sets have no central, durable home where those sets, their completion statistics, and related screenshots exist together.

Dream Project is proposed as a response to this fragmentation: a platform-agnostic layer for preserving saves, achievements, and screenshot-based “stickers” as a coherent, long-term gaming legacy.

---
<br>
<br>


## 3. Business Objectives & Success Criteria

### 3.1 Business Objectives

Over the first four months after launching the MVP, Dream Project is expected to achieve four key business objectives:

1. **Ship a usable MVP to real platform users**  
   Deliver a minimum viable product that includes the core pillars of the platform:  
   - **Save Management** – uploading, securely storing, and organising saves, plus the ability to discover saves shared by other users for specific games and supported platforms.  
   - **Achievement Tracker** – support for both official and fan-made achievement sets so that community-designed lists can live alongside platform-native ones.  
   - **Sticker Album** – screenshot-based “sticker” albums per game, where screenshots act as visual proof of specific in-game moments. As part of this, an AI-based screenshot validation layer should be in place to check whether uploaded images match the intent of each “sticker”, with reasonable tolerance for variations between users’ screenshots.  

The goal is not to cover every planned feature, but to offer a coherent, end-to-end experience that demonstrates the core value of Dream Project.

2. **Build an initial cross-language community**  
   Attract a small but active early community to validate the concept and gather feedback.  
   Target: around **100 active users** within the first four months, with a mix of English-speaking and Ukrainian-speaking users. 

3. **Validate engagement with progression and “sticker” features**  
   Confirm that users are willing to invest time in recording their progress, not just registering once and leaving.  
   Initial targets include:  
   - at least **50 sticker album templates** created by curators for **50 different games**, actively used by players;  
   - at least **400 achievement entries** (completions) recorded within three months of launch.  

These numbers are not final success on their own, but they show whether the platform’s core loop—saving progress, tracking achievements, filling albums—is actually used. 

4. **Form a core group of curators and achievement set creators**  
   Identify and support a small group of motivated users who create and maintain content: fan-made achievement sets, structured album templates, and practical guidance on how to complete them. Curators should be able to enrich their sets with short text tips or external links (for example, to GameFAQs, YouTube videos, or detailed guides) that explain how to obtain specific achievements or fill certain stickers.  

Curators are not chosen through private invitations only. Instead, the platform will:  
   - highlight active contributors inside the product and invite them to curator roles;  
   - publish targeted calls for curators in relevant communities (e.g. RetroAchievements, retro-gaming forums, Discord servers, social media);  
   - where technically and legally feasible, seed the platform by importing or mirroring existing public fan-made achievement sets so that their authors can claim, maintain, and extend them.  

Target: at least **5 volunteer curators** within four months, with **at least 3** coming from the Ukrainian community.

---
<br>
<br>   

### 3.2 Success Criteria

To understand whether these objectives are being met, success criteria are defined as measurable outcomes that can be reviewed after the first four months of operation:

1. **MVP Delivery Success**  
   - The MVP including Save Management, Achievement Tracker, and Sticker Album modules is deployed and accessible to external users.  
   - Core flows (save upload and discovery, achievement tracking, album creation and filling) are usable end-to-end with no critical defects blocking normal use.  
   - An optional onboarding flow or in-product hints are available to help new users understand key actions.

2. **Community Growth & Activity**  
   - At least **100 registered users** have performed meaningful actions (e.g. added saves, tracked achievements, or filled album slots) within a rolling 30-day period by month four.  
   - At least **half** of these active users return at least once after their first week of registration (an early indicator of retention).  
   - The community is mixed, with a target of roughly **75% English-speaking** and **25% Ukrainian-speaking** users, ensuring both a global reach and a clear Ukrainian presence.

3. **Engagement with Achievements & Sticker Albums**  
   - At least **50 distinct sticker album templates** exist, each tied to a different game and created by curators, with player-filled albums showing more than one completed slot for each template.  
   - At least **400 achievement completion records** are stored in the system within three months after launch, across both official and fan-made sets.  
   - A meaningful portion of users (for example, 30–40% of active users) have used both achievement tracking and sticker albums, indicating that these features support each other rather than being isolated tools.  
   - The AI-based screenshot validation mechanism is active in production and used in normal flows (users receive feedback when their screenshots do not appear to match the intended sticker), while still allowing natural variation between different players’ screenshots.

4. **Curator & Content Ecosystem**  
   - A basic curator flow exists: active contributors are identified through their activity and can be elevated to curator status via in-product prompts or invitations.  
   - At least **5 users** are recognised as regular curators (e.g. creators of achievement sets or album templates) and actively maintain or update their content.  
   - At least **10 fan-made achievement sets or subsets** are available for different games, with curators adding short written hints or external links (GameFAQs, YouTube, etc.) where helpful.  
   - At least **3 of these curators** are members of the Ukrainian community, confirming that the product also serves its intended local audience and not only the broader English-speaking base.
---
<br>
<br>   

These criteria are not meant to be rigid contractual targets. They show that Dream Project has clear, time-bound expectations for its first phase and that success is defined not only by “shipping a website,” but by actual usage, engagement, and the emergence of a small but healthy curator-driven ecosystem around it.

---
<br>
<br>

## 4. Solution Overview

Dream Project is proposed as a **platform-agnostic web application** that sits on top of existing gaming ecosystems and connects three currently separate aspects of a player’s progress: save files, achievements (official and fan-made), and screenshots used as “stickers”. Instead of replacing game stores or consoles, it acts as a neutral layer where this information is collected, structured, and preserved over time for many different games and platforms.

The platform maintains a **catalogue of games and platform-specific variants** (e.g. the same title on PC, PS1, SNES and other supported systems). A user does not have to “register all their games” at once. Instead, they search or browse for a particular game and platform when they want to use at least one of the three main features. For users who do not yet know what to play, the system can also suggest a game automatically based on their history or general criteria.

---
<br>

### 4.1 Core Concept

The core idea behind Dream Project is to turn scattered, platform-bound progress into a **coherent, long-lived record** that is organised per game and per user.

A typical interaction looks like this:

1. The user selects a game and specific platform from the catalogue (or accepts a recommendation).  
2. For that game/platform pair, the platform offers:
   - save management actions (upload, organise, and discover saves),
   - achievement tracking (official sets where available, plus fan-designed sets),
   - sticker album actions (filling screenshot-based albums defined for that game).
3. All resulting data—saves, achievement completions, and filled stickers—contributes to the user’s overall profile and statistics.

The user does not have to use all three feature areas at once. Dream Project works even if they only care about, for example, finding a specific save for a retro game, or filling a sticker album for a single favourite title. The platform simply exposes whichever features are available for the selected game and platform.

---
<br>

### 4.2 Game Discovery & Recommendations

For users who “don’t know what to play right now”, Dream Project includes a **game recommendation flow**:

- The user can specify at least a target platform (e.g. “PS1”, “SNES”, “PC”) and optionally basic preferences.
- The system proposes a game and variant from its catalogue, using:
  - the user’s own history (games they searched for, which features they used, what they completed);
  - general signals such as community ratings, popularity, or “anniversary” dates (e.g. games released around the current date);
  - curated lists prepared by the community or platform owners.

If the suggestion does not fit, the user can reject it (for example by clicking *“This is not suitable for me”*), and the system offers alternative options. Over time, this helps users discover titles that match their interests and that also have meaningful save, achievement, and sticker structures within Dream Project.

---
<br>

### 4.3 Target Users

The solution primarily targets:

- **retro players** who want a safe, organised way to keep and share saves for older platforms;
- **completion-focused players** who care about more than the default official achievement list and want deeper tracking;
- **curators and community authors** who design fan achievement sets and sticker album templates and want a stable home for their work.

Secondary beneficiaries include content creators, community organisers, and potentially platform owners who can refer to Dream Project as an external layer for tracking long-term progress.

---
<br>

### 4.4 Key Capabilities

From a business analysis perspective, Dream Project provides the following key capabilities at MVP level:

#### Save Management Layer

Users can upload and organise save files for supported platforms, associate them with specific games and playthroughs, and search for saves shared by other users based on criteria such as:

- game title and platform,
- game version or region (where relevant),
- basic metadata (e.g. story progress level, difficulty, specific checkpoints).

The goal is to make transferring or reusing a save file straightforward without forcing players into low-level file operations or informal one-off transfers.

#### Unified Achievement Tracking

Dream Project offers a unified achievement view for each game, while recognising technical and legal constraints.

- **Official achievements:**  
  Where platform APIs or integration options are available (for example, for certain PC ecosystems or retro services), users can **link external profiles** and allow Dream Project to read their official achievement status. This synchronisation is read-only: the platform reflects what has already been earned elsewhere. Where automated sync is not feasible, official achievements can still be represented, but completion may need to rely on self-reporting and/or evidence such as screenshots.

- **Fan-made achievement sets:**  
  Fan sets are treated as first-class structures with their own criteria, subsets, and progress. For retro games running in compatible emulators, Dream Project can follow the model of services like RetroAchievements:  
  - integrate with emulators or existing APIs where technically and legally possible;  
  - use those integrations to detect when an achievement condition is met.  

  For platforms where such integration is not realistic (especially closed console ecosystems), the MVP accepts that full automation is not always possible. In those cases, completion tracking can combine:  
  - manual marking of completion by the user;  
  - screenshot-based evidence tied to specific stickers or achievements;  
  - optional community validation or curator review.  

The system is not positioned as an enforcement mechanism; its main value is to **preserve and present** a structured record of what players and communities agree has been accomplished, rather than to police every single unlock.

#### Screenshot-Based Sticker Albums

For each supported game, curators can define **sticker album templates**: structured collections of “slots” that correspond to key scenes, bosses, locations, endings, or other notable events. Players fill these slots with their own screenshots.

An AI-based validation layer checks whether uploaded screenshots broadly match the intent of each sticker (for example, checking that the right character, location, or UI elements are present), without requiring pixel-perfect similarity. This makes stickers:

- meaningful enough to act as visual proof of progress,
- flexible enough to allow different playstyles, resolutions, and platforms.

Sticker albums are tightly linked to achievements and events inside the game world. A filled sticker slot can correspond, for example, to the completion of a particular fan achievement, reaching a key story point, or defeating a specific boss.

#### Curator & Content Tools

Curators have dedicated tools to:

- create and maintain **fan achievement sets** and **sticker album templates**;
- define subsets or “challenge branches” for specific playstyles;
- add short text hints or external links (for example, to GameFAQs guides or YouTube videos) that explain how to obtain certain achievements or fill specific stickers.

Active contributors can gradually be promoted to curator roles based on their activity and community feedback. Where technically and legally feasible, existing public fan-made sets can be imported to seed initial content so that their authors can continue maintaining them inside Dream Project.

#### Profile & Progress View

Each user has a profile that aggregates their activity across games:

- saves contributed and used,
- official and fan achievement completions,
- sticker albums started and completed.

The profile includes **automatically generated statistics** showing recent activity and overall completion trends. For example, the platform can display:

- how long it took a user to complete a particular achievement set (from the first recorded action to the final required achievement);
- how long it took them to reach the effective “end” of a game (e.g. time between the first recorded progress and obtaining a designated “game completion” achievement or sticker);
- badges earned for finishing curated sets or albums, or for reaching specific progression thresholds.

Conceptually, this mirrors the familiar profile and stats layer of services such as Steam or RetroAchievements, but is aligned with Dream Project’s own three pillars: saves, achievements, and sticker-based albums.

---
<br>

### 4.5 Relationship to the Problem and Objectives

By combining these capabilities, Dream Project directly addresses the fragmentation described in the Problem Statement:

- save files are preserved and shared independently of any single platform account;
- official and fan-made achievements are brought together into one structured tracker per game;
- screenshots are transformed from loose images into organised, validated “stickers” that document concrete in-game moments;
- users can discover what to play next through recommendations that are aware of these structures.

At the same time, the solution remains scoped as a focused MVP: it aims to deliver enough functionality to validate the concept, grow a small but active community, and support a core group of curators—without overextending into full-fledged storefront or social network territory.

---
<br>
<br>


## 5. Scope

This section defines what the initial version of Dream Project is expected to include and, just as importantly, what will be intentionally left out of the first release. The goal is to deliver a focused MVP that demonstrates the value of the three core pillars—save management, achievement tracking, and sticker albums—without trying to cover every possible platform or social feature from day one.

---
<br>

### 5.1 In Scope

The MVP for Dream Project includes the following functional areas:

#### Game Catalogue & Platform Variants
- Central catalogue of games with entries for specific platform variants (e.g. PC, PS1, SNES, other supported systems).
- Basic search and browse by game title and platform.
- Game detail pages that act as entry points into saves, achievements, and sticker albums for that specific game/platform pair.

#### Save Management Module
- Secure upload and storage of save files for supported platforms.
- Association of each save with a game, platform variant, and optional metadata (e.g. rough progress level, difficulty, notable events reached).
- Search and filtering of community-shared saves by game, platform, and basic metadata.
- Simple community feedback and moderation signals (e.g. reporting problematic saves, basic quality indicators).

#### Achievement Tracking Module
- Representation of **official achievement sets** for selected platforms, configured in the Dream Project catalogue (either imported where APIs allow it or manually defined).
- Ability for users to **link external profiles** and, where technically and legally feasible, read their official achievement status from at least one external ecosystem (for example, a PC platform or an existing retro-focused service).
- Support for **fan-made achievement sets**: curators can define, publish, and update sets and subsets for individual games.
- Basic completion tracking for both official and fan sets, combining:
  - automated updates where integrations exist (e.g. for retro emulators via external services);
  - manual marking and/or screenshot-based confirmation when full automation is not realistic.

#### Sticker Album Module
- Creation of **sticker album templates** for specific games and platforms, managed by curators.
- Each template consists of defined “slots” tied to concrete in-game situations (bosses, locations, endings, special events).
- Players can upload screenshots to fill these slots.
- Initial AI-based validation that checks whether a screenshot roughly matches the intended sticker (e.g. correct scene or character), with reasonable tolerance for differences in resolution, HUD elements, or visual style.

#### Curator & Content Management Tools
- Role and interface for curators to:
  - create and edit fan achievement sets and sticker album templates;
  - define subsets or specialised branches (e.g. “low-level completion subset”);
  - attach short textual hints or external links (GameFAQs, YouTube, etc.) for specific achievements or stickers.
- Basic curator lifecycle: recognising high-activity users and promoting them to curator roles via admin decisions or automated suggestions.

#### Profile & Progress View
- Per-user profile page aggregating:
  - saves contributed and used,
  - official and fan achievement completions,
  - sticker albums started and completed.
- Automatically generated statistics, including:
  - time taken to complete specific achievement sets (from first recorded action to final achievement),
  - time to reach effective “game completion” where such a marker exists,
  - badges for finishing curator-defined sets or albums.

#### Game Discovery & Recommendations
- Simple recommendation flow where a user can request a suggestion for a game on a chosen platform.
- Recommendation logic combining:
  - the user’s own history (searched games, used features, completed sets/albums),
  - general signals such as ratings, popularity, or notable dates (e.g. anniversaries),
  - optional curated lists.
- Ability for the user to reject a suggestion and request another until a suitable option is found.

#### Core Platform Services
- User registration, authentication, and session management.
- Basic role-based access control (standard users, curators, admins).
- Reporting and moderation tools for content (saves, screenshots, achievement sets, albums).
- RESTful API endpoints for major modules to support future integration and tooling.

---
<br>

### 5.2 Out of Scope

The following items are explicitly out of scope for the MVP and may be considered in later phases:

- **Full coverage of all gaming ecosystems**  
  Deep, official integrations with closed console platforms (PlayStation, Xbox, Nintendo) for real-time achievement sync or direct save manipulation are not targeted for the initial release.

- **Custom emulator development or invasive tooling**  
  Dream Project will not develop its own emulators, game hooks, or tools that interfere with game processes in ways that could violate platform rules or risk user bans. Where integrations exist (e.g. via established retro services), the platform will rely on their APIs rather than low-level modifications.

- **Strict anti-cheat and enforcement systems**  
  The MVP does not attempt to guarantee that every recorded achievement or sticker is “legitimate” beyond basic validation (AI checks, screenshots, community signals). Complex anti-cheat mechanisms and fully tamper-proof verification are out of scope.

- **Competitive multiplayer features and tournaments**  
  No global leaderboards, PvP ranking systems, official competitions, or timed tournaments are included. Progress is tracked per user, not as part of competitive events between users.

- **Rich social network functionality**  
  Features such as private messaging, complex friend systems, public activity feeds, or in-depth social graphs are not part of the MVP.

- **Video capture and hosting**  
  The platform will not handle game video clips, streaming features, or integrated video hosting in the first release. The focus remains on saves, achievements, and screenshots.

- **Native mobile applications**  
  Dedicated iOS/Android apps are not planned for the MVP. The initial release assumes a web-based interface accessible from desktop and mobile browsers.

- **Monetisation and payment features**  
  Subscription management, in-app purchases, or other payment-related flows are not included in the initial scope.

By clearly separating what is and isn’t included, the MVP can focus on delivering a stable, demonstrable core experience around saves, achievements, and sticker-based albums, while leaving more complex integrations and social features to later phases.

---
<br>
<br>

## 6. Stakeholder Overview

This section highlights the main stakeholder groups for Dream Project, what roles they play, and what they care about at a high level. Detailed personas, communication plans, and responsibilities will be expanded in the dedicated **Stakeholder & Communication Plan**; here the goal is simply to show whose needs drive the product decisions.

| Stakeholder Group                           | Role / Description                                                                 | What They Care About                                                                                                                                        | Influence Level |
|---------------------------------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|
| Regular Players (Platform Users)            | People who use the platform to find saves, track achievements, and fill albums    | Simple and stable UX, safe and trustworthy save sharing, clear achievement status, meaningful sticker albums, useful game recommendations                    | High            |
| Retro & Emulator-Focused Players            | Players who use older platforms and emulators                                     | Support for retro platforms and formats, reliable handling of saves, recognition of retro-focused achievements, low friction with emulators and tools       | High            |
| Curators & Achievement/Album Set Creators   | Community members who design fan achievement sets and sticker album templates     | Good creation tools, clear structure for sets and albums, long-term stability of their content, visible attribution, basic protection against misuse         | Medium–High     |
| Global Content Creators & Community Organisers (EN) | Streamers, writers, community leaders in the broader, mostly English-speaking audience | Ways to showcase progress (profiles, stats, albums) in their content, stable links to games and profiles, readable visualisations, predictable platform behaviour | Medium          |
| Ukrainian Content Creators & Community Organisers   | Creators and community leads focused on the Ukrainian audience                    | Strong visibility of Ukrainian language and games, absence of Russian-language localisation by default, clear handling of titles linked to Russian companies (e.g. ability to exclude them or mark them with contextual information so users can make informed choices), fair representation in curator roles and featured content | Medium          |
| Platform Admins & Product Team              | People responsible for operating and evolving Dream Project                       | Security, moderation tools, system stability, clear MVP scope, maintainable architecture, ability to prioritise features based on real usage and feedback   | High            |
| External Services & Integration Partners    | Third-party platforms or services (e.g. emulator/achievement APIs) used for sync | Transparent and limited use of their APIs, compliance with their terms of service, predictable integration patterns, minimal operational and support overhead | Medium          |

---
<br>
<br>

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
