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
