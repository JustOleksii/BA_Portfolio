# Business Analysis Document – Dream Project

## Document Structure

Below is an overview of the main sections of this Business Analysis Document.  
Use the links to jump directly to the part you’re interested in.

- **[1. Introduction & Purpose](#1-introduction--purpose)**  
  Explains why this document exists, how it relates to the Business Case, and who should use it. Sets expectations about scope and level of detail.

- **[2. Business Context & Background](#2-business-context--background)**  
  Describes the broader context in which Dream Project appears: market trends, player behaviour, relevant platforms and communities. Connects the product idea to the world around it.

- **[3. Stakeholder Analysis (Detailed)](#3-stakeholder-analysis-detailed)**  
  Presents a deeper view of stakeholder groups: their goals, pain points, influence, and expectations. Summarises how their needs shape the planned solution (with the communication details moved to the separate Stakeholder & Communication Plan).

- **[4. Current State (AS-IS Overview)](#4-current-state-as-is-overview)**  
  Describes how players currently manage saves, achievements, and screenshots without Dream Project. Highlights key pain points and workarounds that the new solution aims to improve.

- **[5. Future State (TO-BE Overview)](#5-future-state-to-be-overview)**  
  Provides a narrative view of how things should work once Dream Project exists, from a business and user perspective. Focuses on desired outcomes rather than technical implementation.

- **[6. Gap Analysis](#6-gap-analysis)**  
  Compares the AS-IS and TO-BE states, identifies gaps, and groups them into themes (e.g. data, process, experience, integrations). Helps justify which features are essential for the MVP.

- **[7. Business Processes](#7-business-processes)**  
  Describes the key processes that Dream Project needs to support (e.g. finding a game, sharing a save, completing a fan achievement set, filling a sticker album).  
  - **[7.1 AS-IS Process Summaries](#71-as-is-process-summaries)** – How users currently perform these processes using existing tools and platforms.  
  - **[7.2 TO-BE Process Summaries](#72-to-be-process-summaries)** – How the same processes should work within Dream Project at a high level.

- **[8. Business Rules](#8-business-rules)**  
  Lists the main business rules that govern how Dream Project operates: rules for saves, achievements, sticker albums, curators, catalogue policies, and Ukrainian-specific constraints. Focuses on decisions and policies rather than technical details.

- **[9. Data & Information View](#9-data--information-view)**  
  Provides a business-level view of the main information objects (e.g. Game, Platform Variant, Save, Achievement Set, Sticker Album, Profile) and how they relate to each other. Acts as a narrative companion to the formal data model.

- **[10. Assumptions & Constraints (BA-Level)](#10-assumptions--constraints-ba-level)**  
  Summarises the key assumptions and constraints that affect analysis and process design (e.g. available integrations, catalogue policies), with references back to the Business Case where appropriate.

- **[11. Non-Functional Considerations (Business View)](#11-non-functional-considerations-business-view)**  
  Describes non-functional needs from a business perspective: usability expectations, trust and safety, localisation, basic performance and availability targets important for stakeholders.

- **[12. Impact on Stakeholders & Organisation](#12-impact-on-stakeholders--organisation)**  
  Explains how Dream Project changes the situation for each major stakeholder group (players, curators, admins, creators) and what new responsibilities or opportunities arise for them.

- **[13. Glossary & Definitions](#13-glossary--definitions)**  
  Defines key terms used across Dream Project documentation (e.g. “Sticker Album Template”, “Curator”, “Fan Achievement Set”, “Platform Variant”), ensuring that all readers share a common vocabulary.

- **[14. References & Related Documents](#14-references--related-documents)**  
  Lists related artefacts such as the Business Case, BRD, SRS, Stakeholder & Communication Plan, Risk & Assumptions Register, and links this document to the rest of the BA package.
 
---
<br>
<br>

## 1. Introduction & Purpose

The Business Analysis Document (BAD) for **Dream Project** digs one level deeper than the Business Case.  
Where the Business Case explains *why* the product should exist and what we expect from the MVP, this document focuses on *how the business domain looks*, *who is involved*, *how things work today*, and *how they are expected to work once Dream Project is introduced*.

The primary purposes of this document are to:

- describe the **business context** around Dream Project (players, platforms, communities, catalogue policies, especially the Ukrainian context);
- provide a structured view of the **AS-IS situation**: how players currently handle saves, achievements, and screenshots using existing tools;
- define the **TO-BE vision** from a business and user perspective, without going into low-level technical design;
- highlight the **gaps** between AS-IS and TO-BE that justify the features planned for the MVP;
- record the **business rules and information structures** that will later be formalised in the BRD, SRS, and data models.

This document is intended for:

- **Product and business roles** (product owners, business analysts) who need a clear narrative about how Dream Project fits into the existing gaming and community ecosystem.
- **Design and engineering roles** (UX designers, developers, architects) who need a grounded understanding of the domain before making solution decisions.
- **Potential sponsors or stakeholders** who want more detail than the Business Case provides, but still in business language rather than implementation detail.

The Business Analysis Document does **not** define every screen or API, and it does not replace the System Requirements Specification or detailed process diagrams. Instead, it serves as a bridge:

- upwards, to the **Business Case**, by showing that the proposed scope follows from real business needs;  
- sideways, to documents like the **Stakeholder & Communication Plan** and **Risk & Assumptions Register**;  
- downwards, to the **BRD**, **SRS**, and detailed visual models that will use this analysis as their foundation.

By the end of this document, a reader should understand the world in which Dream Project operates, the problems it is actually solving in practice, and the business logic and rules that any concrete implementation must respect.

---
<br>
<br>

## 2. Business Context & Background

The gaming landscape that **Dream Project** enters is fragmented across time, platforms, and tools.  
Players move between retro consoles, emulators, and modern PC/console ecosystems, but their progress and memories remain scattered: save files live in local folders or emulator snapshots, achievements are split between official platform lists and fan-made sets, and screenshots sit in unstructured galleries or social feeds.

From a business analysis perspective, Dream Project sits **on top of** this ecosystem rather than trying to replace it. It focuses on preserving and organising what players have already done, especially for those who care about completion, history, and retro culture.

---
<br>

### 2.1 Current Ecosystem

Key elements of the current environment include:

- **Modern storefronts and platform accounts (Steam, PlayStation, Xbox, Switch, etc.)**  
  These ecosystems do their job well: they sell games, keep libraries together, and provide cloud saves and official achievements inside their own boundaries. For a single vendor’s world, they are usually “good enough”.  
  The limitation appears when a player’s history spans **multiple vendors and generations**. A Steam profile knows nothing about PlayStation trophies; a Switch account has no idea about retro emulators; older console saves may never have been online at all. Each platform shows a slice of the story, but none is responsible for stitching everything into one long-term view.

- **Library aggregators and launchers (e.g. GOG Galaxy 2.0, Playnite, LaunchBox)**  
  Aggregators solve a different problem: they help players *see and launch* games from many sources in one place. GOG Galaxy, Playnite, and similar tools improve everyday convenience by:
  - bringing multiple libraries into a single UI,
  - sometimes showing official achievements and basic playtime across services.  
  Their focus, however, is **current access**, not long-term preservation or interpretation. They do not try to:
  - give semantic meaning to save files beyond “this game has saves somewhere”;
  - host fan-defined structures like custom achievement sets or curated sticker albums;
  - act as a neutral “memory layer” where a player’s history can be read independently of where they own the game.

- **Save backup and transfer tools (e.g. GameSave Manager, Ludusavi)**  
  These utilities are excellent at what they are designed for: preventing loss of PC saves and simplifying migration between machines. They:
  - discover save locations;
  - back up and restore files;
  - sometimes sync with generic cloud storage.  
  
They do not aim to understand **what** the save represents (first clear, 100% run, special challenge) or to expose saves as part of a public, explainable gaming history. For most players, these tools remain a technical safeguard in the background, not a place to *browse* or *show* their progress.

- **Achievement tracking sites (e.g. Exophase, TrueAchievements, PSNProfiles)**  
  These services aggregate **official achievements and trophies** across platforms, giving players unified profiles, leaderboards, and stats. They:
  - reflect vendor-defined achievement data;
  - add social and competitive layers (rankings, rarity, comparisons).  
  
Their strength is in representing the *official* view of completion. Fan-made sets, alternative definitions of “100%”, and visual proof (screenshots tied to specific conditions) largely sit outside their scope. A player who cares about deeper, community-defined completion still has to look elsewhere.

- **Retro achievement ecosystems (e.g. RetroAchievements and emulator integrations)**  
  For classic systems, platforms like RetroAchievements come closest to the spirit of Dream Project. They:
  - host fan-made achievement sets for thousands of retro games;
  - integrate with emulators to detect progress automatically.  
  
However, they are intentionally focused on a specific slice of the world: retro titles and supported emulators. They are not designed to span modern consoles and PC storefronts, nor to manage saves and structured screenshots across all eras in a single profile.

- **Community-created content and guides**  
  Across forums, wikis, GameFAQs, YouTube and Discord servers, players publish:
  - detailed challenge lists;
  - route recommendations;
  - explanations of obscure mechanics and “true” completion paths.  
  
This content is rich, but it lives as **separate documents and posts**, not as part of a structured system that knows which challenges each individual player has actually completed.

- **Screenshots and “visual memories”**  
  Platforms and tools make capturing screenshots easy. Over time, though, most players accumulate:
  - folders full of unnamed images,
  - platform galleries that scroll endlessly,
  - social posts that quickly disappear in the feed.  

Screenshots rarely become **organised evidence** of specific achievements or milestones. They are memories, but not structured ones: there is no widely adopted practice of turning them into album-like “stickers” tied to explicit conditions.

  ---
<br>

### 2.2 Background: The Gap Dream Project Targets

Looking at this ecosystem as a whole, the gap is not that any particular tool is “bad”, but that each solves only **one slice** of a broader story:

- Storefronts show what you own and what you did **inside one vendor’s garden**.
- Aggregators help you **launch** games from many places but do not interpret saves, fan sets, or screenshots as part of a coherent history.
- Save tools keep data from being lost but treat it as **anonymous files**, not as milestones or memories.
- Achievement sites faithfully reflect **official** definitions of completion, but not community-invented ones.
- Retro achievement platforms excel in their niche, but do not carry over to modern ecosystems.
- Guides and videos explain how to do things, but are disconnected from whether *you personally* have done them.

For a player who cares about their **full gaming legacy**—including retro titles, multiple modern platforms, fan-made achievement sets, and visual proof—this means constantly switching between tools, sites, and local folders. No single place:

- ties saves, official achievements, and fan-defined goals together for each game;
- gives screenshots a structured role (as verifiable “stickers” in a curated album);
- presents all of this as a long-term, platform-agnostic record that can be revisited years later.

This is the specific gap that Dream Project targets: not replacing existing tools, but providing the **missing layer** that connects and preserves what they currently keep separate.

---
<br>

### 2.3 Positioning of Dream Project

In business terms, Dream Project aims to:

- **Complement the existing ecosystem**  
  It does not compete with stores, emulators, or launchers. Instead, it:
  - accepts that games will always be bought and run elsewhere;
  - assumes that official achievements and retro integrations will continue to live on their native platforms;
  - focuses on combining these sources into a structured view of *what was actually achieved* and *how it can be shown*.

- **Provide a neutral “completion and memory” layer**  
  Dream Project’s main value is to:
  - preserve saves with enough context to understand their meaning;
  - host both official and fan-made achievement structures for each game;
  - turn screenshots into curated, validated “stickers” in game-specific albums;
  - represent this information across vendors, generations, and formats as a single narrative per player.

- **Serve both global and Ukrainian audiences intentionally**  
  The platform is designed for:
  - a broad, mostly English-speaking audience that can benefit from a cross-platform progress layer;
  - a clearly visible Ukrainian segment, with catalogue and language policies that respect the local context (e.g. treatment of Russian-linked content) and highlight Ukrainian creators and curators.

Seen in this context, Dream Project is not “yet another launcher” or “one more achievement site”. It is positioned as a **memory and completion service** that stands alongside existing tools, specialising in preserving, structuring, and presenting the parts of gaming life that currently fall between them.

---
<br>
<br>

## 3. Stakeholder Analysis (Detailed)

This section expands the stakeholder overview from the Business Case into a more detailed view.  
It focuses on **who** is involved, **what they want**, and **how Dream Project affects them**.  
Specific communication channels, frequency, and responsibilities will be defined separately in the **Stakeholder & Communication Plan**.

---
<br>

### 3.1 Stakeholder Groups Overview

| Stakeholder Group                           | Short Description                                                                    | Primary Interest in Dream Project                                                 |
|---------------------------------------------|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Regular Players (Platform Users)            | Players using the platform for one or more features (saves, achievements, albums)   | A simple, trustworthy way to enhance and present their progress                   |
| Retro & Emulator-Focused Players            | Players focused on older systems and emulators                                      | Safe handling of retro saves and recognition of retro-centric achievements        |
| Curators & Achievement/Album Set Creators   | Community members who design fan achievement sets and sticker album templates       | Good tools, visibility, and stability for the content they create                 |
| Global Content Creators & Community Organisers (EN) | Streamers, writers, community leaders with a global/EN audience                   | Reliable profiles and stats to showcase in content and community discussions      |
| Ukrainian Content Creators & Community Organisers   | Creators and organisers focused on Ukrainian players and communities               | A platform aligned with Ukrainian context and catalogue policies                  |
| Platform Admins & Product Team              | People responsible for operating and evolving Dream Project                         | Stable, secure, maintainable product that delivers measurable value               |
| External Services & Integration Partners    | Third-party APIs and services used for progress or metadata integrations           | Predictable, compliant, low-risk use of their services                            |

---
<br>

### 3.2 Regular Players (Platform Users)

**Who they are**  
Players using modern platforms (PC and consoles) who come to Dream Project to **extend** what they already have: they may only care about one feature (e.g. finding saves), or they may eventually use all three (saves, achievements, sticker albums). They see Dream Project as a useful companion, not a replacement for Steam, PSN, Xbox Live, etc.

**Goals & Motivations**

- Enrich their experience with a specific game without necessarily replaying everything from scratch.
- Have a clear view of their progress that is easier to read than scattered platform dashboards.
- Show selected parts of their gaming history (certain games, runs, or albums) in a structured way.
- Optionally centralise data from multiple platforms into one readable profile.

Typical **save-related scenarios** for this group include:

- **Branch exploration** – using another player’s save that is just before a key decision (faction choice, ending route, major build lock-in) to explore alternative outcomes without replaying the entire game.
- **Skill or difficulty walls** – when stuck at a specific boss or section, loading a save from a player who has passed that point in order to continue and still see the rest of the game.
- **Skipping early grind** – starting from a save where basic grinding is done and the “interesting” part of the game is unlocked, especially in long RPGs or grind-heavy titles.
- **Experimenting with builds** – trying out different character builds or configurations using community saves instead of re-levelling from zero.

**Pain Points Today**

- Platform-native cloud saves help with hardware changes, but:
  - do not help when moving between **different ecosystems** (e.g. from Xbox to PC);
  - are not designed for sharing saves between players.
- Browsing alternative branches or “challenge-ready” save files generally requires digging through forums, file-sharing sites, or mod hubs.
- Personal screenshots and achievements feel buried inside vendor-specific UIs.

**What They Expect from Dream Project**

- The ability to use **only what they need**:
  - some will mostly browse and import saves,
  - others will primarily track achievements,
  - some will focus on sticker albums.
- Straightforward flows for discovering and using community saves that match their game, platform, and situation.
- For account linking and official achievement sync (where available): **non-intrusive, read-only integrations** that do not risk bans or violate the terms of service of the underlying platforms.
- Clear, friendly language instead of heavy technical jargon.

**Success Signals (from their perspective)**

- “I found exactly the kind of save I needed and could continue the game the way I wanted.”
- “My achievements and albums are easier to understand here than in the default platform dashboards.”
- “If I connect my accounts, I feel safe and in control of what data is read.”

---
<br>

### 3.3 Retro & Emulator-Focused Players

**Who they are**  
Players who primarily engage with older systems (e.g. PS1, SNES) using emulators or original hardware, often very invested in **preservation and authenticity**.

**Goals & Motivations**

- Keep long-running retro playthroughs safe and portable (across emulators, machines, or setups).
- Have their retro achievements recognised in a way that feels comparable to modern trophies.
- Participate in fan-made challenges that respect the depth of older games.

**Pain Points Today**

- Save states and memory cards are fragile: one corrupted file or mistaken overwrite can erase years of progress.
- Retro achievements may exist (e.g. via RetroAchievements), but are limited to specific emulators and profiles.
- Little to no unified view of “my retro history” across different systems and tools.

**What They Expect from Dream Project**

- Support for key retro platforms and formats (at least for a defined MVP subset).
- A place where retro saves and achievements are **not second-class** compared to modern games.
- The ability to show retro completions and albums alongside modern ones in a single profile.

**Success Signals**

- “My PS1 and SNES saves feel as safe and visible as my modern cloud saves.”
- “I can show people what I’ve done in retro games without sending them to three different sites.”

---
<br>

### 3.4 Curators & Achievement/Album Set Creators

**Who they are**  
Players who enjoy **designing structure** for games: fan achievement sets, challenge lists, and thematic sticker albums. They often have deep knowledge of specific titles or genres.

**Goals & Motivations**

- Express their understanding of a game through curated sets and albums.
- See other players actually use and complete their content.
- Build a reputation as trusted curators or experts for certain titles.

**Pain Points Today**

- Fan sets often live in scattered documents (spreadsheets, forum posts, Discord messages).
- It is hard to track who used which list and whether they completed it.
- No single place to keep such content updated and visible over time.

**What They Expect from Dream Project**

- Solid tools to:
  - define achievements and subsets,
  - build sticker album templates per game,
  - attach hints and external links (GameFAQs, YouTube, etc.).
- Clear attribution (names, profiles) and visibility for their work.
- Reasonable protection against low-effort copies or misuse of their sets.

**Success Signals**

- “People are using my sets and albums; I can see their progress and feedback.”
- “I can maintain and refine my content in one place instead of chasing updates across multiple platforms.”

---
<br>

### 3.5 Global Content Creators & Community Organisers (EN)

**Who they are**  
Streamers, writers, podcast hosts, Discord admins, and other community leaders who speak primarily to a **global, mostly English-speaking audience**.

**Goals & Motivations**

- Tell stories about games and playthroughs using concrete, verifiable progress.
- Highlight interesting challenges, completions, or albums from their community.
- Have stable links and visuals for articles, videos, and community posts.

**Pain Points Today**

- Progress screenshots and stats are spread over different platforms with different formats.
- Hard to link “this specific run with these specific rules” to a clear, visible record.
- No neutral place to point to when they want to showcase a community member’s long-term dedication.

**What They Expect from Dream Project**

- Stable URLs for profiles, games, sets, and albums that are safe to share.
- Clear, readable visuals and statistics that look good in screenshots or embedded views.
- Minimal friction when explaining “what this site is” to their audience.

**Success Signals**

- “I can point my viewers or readers to a single profile and they immediately understand what was completed.”
- “Community challenges can be expressed as sets/albums without custom spreadsheets.”

---
<br>

### 3.6 Ukrainian Content Creators & Community Organisers

**Who they are**  
Content creators and community organisers focused on the **Ukrainian gaming audience**: streamers, bloggers, Discord admins, local community leaders.

**Goals & Motivations**

- Provide Ukrainian players with tools and content that respect their language and context.
- Avoid normalising or promoting Russian-linked games and localisation.
- Highlight Ukrainian players, curators, and communities on equal terms with the global audience.

**Pain Points Today**

- Many gaming platforms prioritise English and Russian, with Ukrainian support often limited or absent.
- It is difficult to separate Russian-linked content from the broader catalogue on most platforms.
- Information about which games **support Ukrainian localisation** or are **Ukrainian-made** is often fragmented.

**What They Expect from Dream Project**

- Clear presence of Ukrainian language in the interface and documentation.
- Catalogue policies that:
  - avoid providing Russian as a default UI language;
  - allow them to filter or flag games developed/published by Russian companies;
  - clearly mark games with Ukrainian localisation or Ukrainian origins so they can promote them easily.
- Visibility for Ukrainian curators and content (e.g. featured sets, albums, profiles).

**Success Signals**

- “This platform feels like it understands and respects Ukrainian players.”
- “I can quickly find and highlight games with Ukrainian localisation or made by Ukrainian teams.”
- “I can confidently recommend it to my community without worrying about hidden Russian-linked promotion.”

---
<br>

### 3.7 Platform Admins & Product Team

**Who they are**  
The people responsible for building, maintaining, and evolving Dream Project: product owner, BA, designers, developers, QA, and operations.

**Goals & Motivations**

- Deliver a stable MVP that clearly demonstrates value.
- Keep scope under control and avoid being pulled into every possible feature idea.
- Make evidence-based decisions about what to improve next.

**Pain Points Today (for similar projects)**

- Pressure to expand features too quickly (full social network, complex tournaments, native apps).
- Difficulty tracking which features actually drive engagement.
- Balancing legal, technical, and ethical constraints (especially around integrations and catalogue policies).

**What They Expect from Dream Project (as a project)**

- A well-defined Business Case and analysis that justify decisions.
- Clear requirements and business rules that reduce ambiguity during implementation.
- Analytics and feedback channels that show how users really behave.

**Success Signals**

- “We can explain, in one page, why this MVP exists and whether it’s working.”
- “We know which games, sets, and albums are used—and which are noise.”

---
<br>

### 3.8 External Services & Integration Partners

**Who they are**  
Owners of APIs and services that Dream Project may integrate with: retro achievement platforms, metadata providers, identity providers, or similar.

**Goals & Motivations**

- Ensure their data and services are used within agreed limits and terms.
- Keep integration support effort low.
- Avoid reputational risk from misuse or misrepresentation of their data.

**Pain Points (from their perspective)**

- Third-party projects that overuse APIs, break terms of service, or create support burdens.
- Projects that make it unclear where data originated or who is responsible for its accuracy.

**What They Expect from Dream Project**

- Transparent, limited, read-only integrations that respect their terms and rate limits.
- Clear attribution or labelling where their data is shown.
- A stable integration story (no constant breaking changes).

**Success Signals**

- “We can see how our service is used in Dream Project and it aligns with our policies.”
- “Supporting this integration is predictable and low-risk.”

---
<br>

This stakeholder analysis will be used as input for:

- the **Stakeholder & Communication Plan** (how each group is engaged),
- the **BRD and SRS** (which needs and constraints translate into concrete requirements),
- and the **Risk & Assumptions Register** (where stakeholder-related risks are tracked explicitly).


---
<br>
<br>

## 4. Current State (AS-IS Overview)

This section describes how players currently manage saves, achievements, and screenshots **without** Dream Project.  
The focus is on real behaviours and workflows, not on what “should” exist.

---
<br>

### 4.1 Save Management Today

From a player’s point of view, save management is split across three main patterns:

1. **Modern platforms with built-in cloud saves**  
   - On PC and consoles, many games use platform cloud sync (e.g. Steam Cloud, console cloud storage).  
   - This works well for **“same platform, same account”** scenarios: reinstalling a game, moving between devices in the same ecosystem, or recovering after a local drive failure.  
   - However, players usually **cannot**:
     - browse other players’ saves directly through the platform;
     - easily export and share their own saves in a structured way;
     - move saves across vendor boundaries (e.g. from PS to PC) without manual work and unofficial tools.

2. **PC save folders and backup utilities**  
   - Technically experienced players often know where saves live on disk and can copy them manually.  
   - Some use backup tools or generic cloud storage (Dropbox, Google Drive, etc.) to keep saves safe.  
   - Sharing a save typically means:
     - zipping a folder,
     - uploading it somewhere,
     - explaining in text where it should be placed on another person’s system.  
   - There is no standard way to describe **what** a save represents (e.g. “before final boss with mage build”, “branch A before critical choice”), so context is easily lost.

3. **Retro consoles and emulators**  
   - On original hardware, saves are stored on cartridges, memory cards, or internal memory; backing them up often requires specialised hardware.  
   - On emulators, players use save states and memory card images stored in local directories.  
   - Some retro communities do share saves, but it is usually done through:
     - forum attachments,
     - file hosting links,
     - instructions tailored to a specific emulator.  
   - There is no shared, cross-game location where a player can say “I want a save for *this* game, *this* platform, *this* situation (route, build, difficulty).”

**Result:**  
For most players, saves are either **invisible and vendor-controlled** (cloud sync) or **visible but unstructured** (local folders, ad hoc sharing).  
Using someone else’s save to:
- explore another branch,
- skip a difficulty spike,
- try a different build  
usually requires searching through scattered posts, trusting random files, and following technical instructions.

---
<br>

### 4.2 Achievement & Goal Tracking Today

Achievement and challenge tracking currently sits in several loosely connected layers:

1. **Official achievement / trophy systems**  
   - Modern platforms track achievements/trophies per game and per account.  
   - They work well for:
     - basic completion metrics (“beat the game”, “finish chapter X”),
     - simple public comparison (“I have 100% on this game”).  
   - However, official lists are:
     - constrained by what developers decided to implement;
     - sometimes very minimal (only main story milestones);
     - not flexible enough to express fan-made ideas of “true 100%” or specialised runs.

2. **Cross-platform achievement sites**  
   - Sites aggregating official achievements/trophies allow players to:
     - see combined stats from multiple platforms,
     - compare themselves with others.  
   - They remain tightly bound to **official** achievement definitions; fan-made sets are usually outside their scope or handled informally (forums, custom challenges).

3. **Fan-made challenge and achievement lists**  
   - Communities create their own lists:
     - extra challenges,
     - “hardcore” or “low-level” routes,
     - completion checklists that cover every quest, secret, or item.  
   - These lists are stored as:
     - forum posts,
     - spreadsheets,
     - wiki pages,
     - Discord pins.  
   - Tracking completion is often manual and personal:
     - ticking boxes in a document,
     - updating a note file,
     - “just remembering” what was done.  
   - Even when platforms like RetroAchievements provide a structured and automated tracking system for retro games, this is limited to certain platforms and tools, not the whole gaming landscape.

**Result:**  
Players live in a hybrid world where:

- **official systems** show a partial, vendor-controlled definition of completion;
- **fan systems** are richer and more creative but are scattered and manually tracked;
- there is no single place where a player’s official achievements and fan-made progress for a game are **combined into one coherent picture**.

---
<br>

### 4.3 Screenshot & Visual Memory Handling Today

Screenshots are an important part of how players remember games, but their handling is mostly ad hoc:

1. **Platform galleries and local folders**  
   - PC and consoles provide built-in screenshot capture and simple galleries.  
   - On PC, images typically end up in:
     - game-specific folders,
     - generic “Screenshots” directories,
     - launcher-managed folders (e.g. per-store directories).  
   - On consoles, screenshots live inside the system gallery until the user manually exports or shares them.  
   - Over time, these collections turn into **long, unstructured lists of images**.

2. **Social and community sharing**  
   - Players share screenshots on:
     - social networks (Twitter/X, Instagram, etc.),
     - community platforms (Reddit, Discord, forums).  
   - The goal is usually short-lived: show a funny moment, a bug, a great view, or a rare achievement.  
   - The image leaves some trace in a timeline, but:
     - it is not clearly linked to a specific in-game condition;
     - it is hard to find again later among many posts.

3. **Lack of structured albums tied to progress**  
   - Very few tools allow players to define **structured templates** such as:
     - “slot for each boss of the game”,
     - “slot for each region or key story moment”,
     - “slot for each optional super-boss or ending”.  
   - Screenshots are almost never treated as **evidence** that a particular condition was met in a particular run; they are memories, but not structured milestones.

**Result:**  
Screenshots act as **raw material** for memories, not as a structured part of progress tracking.  
Players who want to build “albums” (e.g. one image per boss or per ending) generally need to:

- invent their own organisation system (folders, file names, collages),
- manually ensure they took the right images at the right times,
- explain the context to others every time they share.

---
<br>

### 4.4 Summary of AS-IS Situation

In the current state:

- Saves are either **hidden and vendor-bound** or **manual and scattered**.  
- Achievements are split between **official vendor lists** and **fan-made structures** with no unified tracker.  
- Screenshots are **abundant but unstructured**, rarely used as formal proof of anything.

Players who care about deep completion, alternative branches, and long-term preservation must **piece together their own ecosystem** of tools, documents, and habits.  
There is no single, neutral place that pulls these elements together into a unified, understandable record of “what I have actually done in this game and how I can show it to others.”


---
<br>
<br>

## 5. Future State (TO-BE Overview)

The TO-BE state describes how the world looks **from the user and business perspective** once Dream Project exists.  
It assumes the MVP scope defined in the Business Case and Section 5 of that document.

At a high level, Dream Project becomes a **web-based companion** where:

- saves are shared and reused with clear context,
- achievements (official and fan-made) are tracked together,
- screenshots form structured “sticker” albums,
- and all of this is visible in a coherent profile rather than scattered across tools.

---
<br>

### 5.1 TO-BE Experience – High-Level Narrative

In the future state, a typical interaction looks like this:

1. A user signs in, chooses a **game + platform** (e.g. “Final Fantasy X – PS2” or “Dark Souls – PC”), or accepts a recommendation from the system.
2. On that game’s page in Dream Project, they see **three pillars** relevant to that game:
   - **Saves** – community saves they can browse and import, plus their own uploaded saves.
   - **Achievements** – official achievement lists (where available) plus fan-made sets created by curators.
   - **Sticker Albums** – curated templates of “slots” for key scenes, bosses, endings, etc.
3. The user decides what they want to do **right now**:
   - pick a community save to explore another route,
   - track progress against a fan-made achievement set,
   - or start filling a sticker album with their own screenshots.
4. Whatever they do is reflected in their **profile and statistics**, which show:
   - which saves they used or contributed,
   - which official and fan achievements they have completed,
   - which albums they have started or finished and how long it took.

The user never has to think about save folders, spreadsheets, or scattered forum posts: Dream Project gives them **one structured place** for this part of their gaming life.

---
<br>

### 5.2 TO-BE Save Management

In the TO-BE state, save management is **intentional and annotated**, not just files in a folder.

- Players can **upload saves** for a supported game/platform and attach:
  - a short description (e.g. “before final choice between factions”, “ready for superboss X”),
  - basic metadata (story progress, difficulty, build summary where relevant).
- Other players can **search and filter** saves for that game/platform by:
  - situation (before/after certain story points),
  - difficulty or build characteristics (where provided),
  - popularity indicators (downloads, positive feedback).
- A simple **history view** lets users see versions of their own saves and when they were uploaded.
- Feedback mechanisms (e.g. likes, flags, basic comments) allow the community to surface **useful** saves and report broken or misleading ones.

Business-wise, this turns saves from anonymous technical artifacts into **reusable checkpoints** that help players:

- explore alternative branches,
- bypass difficulty walls,
- experiment with different builds and setups.

---
<br>

### 5.3 TO-BE Achievement & Goal Tracking

In the TO-BE state, achievements are presented as a **unified structure per game**, while acknowledging different sources:

- For each game, the user sees:
  - **Official achievements** (where available), reflecting what platforms define.
  - **Fan-made achievement sets** curated by the community, including subsets and “challenge branches”.
- Where integrations are possible, Dream Project can **read official achievement status** from linked profiles and pre-fill completion information.
- For fan-made sets (and for official sets where no sync is possible), the platform supports:
  - manual marking of completion,
  - linkage to **screenshots and saves** as evidence,
  - optional curator or community review in edge cases.
- Each achievement or challenge item has a dedicated view that can show:
  - description and criteria,
  - hints or external links (GameFAQs, YouTube),
  - related stickers or recommended screenshots,
  - basic discussion or feedback.

From a business perspective, the TO-BE state enables a player to answer:

- “What does **full completion** look like for this game, according to both developers and fans?”
- “Which of those goals have I personally met, and when?”

---
<br>

### 5.4 TO-BE Sticker Albums & Screenshots

In the TO-BE world, screenshots are turned into **structured visual evidence** rather than unlabelled images.

- Curators create **sticker album templates** for specific games and platforms:
  - each template defines a set of “slots” (e.g. one per boss, area, ending, or special event),
  - each slot has a description of what it represents.
- Players fill those slots by **uploading their own screenshots**:
  - the platform uses AI-based validation to check if the image matches the intent (without requiring pixel-perfect similarity),
  - if the screenshot is accepted, the slot is considered “filled”.
- Albums show **completion indicators**:
  - per-slot status (empty/filled),
  - overall percentage for that album,
  - timestamps indicating when the album (or specific stickers) were completed.
- Completing certain albums (or sets of albums) can award **profile badges** and visible milestones tied to that game.

Business-wise, this TO-BE state turns screenshots into part of a player’s **structured gaming record**, aligned with achievement sets and saves.

---
<br>

### 5.5 TO-BE Role-Specific Views

While Dream Project is not a full “dashboard-heavy” enterprise system, different roles still see **different slices of the same data**:

- **Regular Players**
  - See a simplified view focused on:
    - searching games,
    - using saves,
    - tracking achievements,
    - filling albums,
    - viewing their own profile and stats.
  - Most administrative and curator options are hidden to reduce noise.

- **Curators**
  - Access additional tools to:
    - create and edit fan achievement sets and album templates,
    - attach hints and external links,
    - view aggregate statistics on how their sets and albums are used.
  - Have a clear indication of which parts of the catalogue they “own” or maintain.

- **Platform Admins**
  - Have moderation and configuration views that show:
    - reported saves, screenshots, or sets,
    - catalogue-level information (e.g. flags for Russian-linked content),
    - basic system health and usage trends.
  - Can promote users to curator roles and manage high-level settings.

From a BA perspective, these role-specific views are **slices of the same underlying model**: they ensure that each type of stakeholder interacts with Dream Project in a way that matches their responsibilities and mental model.

---
<br>

### 5.6 Outcome of the TO-BE State

When the TO-BE state is realised at MVP level:

- Players have **one place** to:
  - reuse saves meaningfully,
  - track official and fan-made achievements together,
  - build visual albums that actually represent their playthroughs.
- Curators have a **proper home** for their achievement sets and albums, with visibility and feedback.
- Content creators (especially Ukrainian and global) gain **reliable references** for progress and completion stories.
- The product team gains **structured data** about how saves, sets, and albums are used, making future decisions more grounded.

The following sections (Gap Analysis, Business Processes, Business Rules, etc.) break this high-level vision into more detailed elements that can be implemented and validated.


---
<br>
<br>

## 6. Gap Analysis

This section compares the **current state (AS-IS)** with the **desired future state (TO-BE)** and identifies the main gaps that justify Dream Project’s MVP scope.  
The focus is on business and user perspective, not on technical implementation.

---
<br>

### 6.1 Scope of the Gap Analysis

The analysis is organised around four dimensions:

- the three core pillars:
  - **Save Management**
  - **Achievement & Goal Tracking**
  - **Screenshots & Sticker Albums**
- plus a cross-cutting dimension:
  - **Profiles, Curators, Catalogue & Governance**

For each area, we highlight what exists today, what Dream Project aims to provide, and what gap must be closed.

---
<br>

### 6.2 Save Management Gaps

**AS-IS**

- Saves are:
  - either hidden inside vendor-controlled cloud systems,
  - or exposed as raw files and save states in local folders.
- Sharing or reusing saves between players usually involves:
  - ad hoc file uploads (archives on forums, file hosts),
  - manual instructions (“copy here, rename this file”),
  - little or no description of what the save actually represents.
- There is no common, cross-game place where a player can say:
  - “I want a save **before this decision**,”
  - “I want a save with **this kind of build**,”
  - “I want a save that starts **after this difficult boss**.”

**TO-BE**

- Saves are uploaded to Dream Project **with context**:
  - game + platform,
  - short description and metadata (progress, route, build, difficulty).
- Other players can **search and filter** saves specifically for:
  - branching points,
  - post-boss checkpoints,
  - build-specific or challenge-ready states.
- Community feedback and simple moderation tools help surface **reliable, meaningful** saves.

**Gaps**

- **Context gap** – AS-IS saves are technical objects; TO-BE requires them to be **semantic checkpoints** (understandable to humans).
- **Discovery gap** – currently no central discovery layer for situation-specific saves; TO-BE requires **structured search and filters**.
- **Trust gap** – today players must rely on random files; TO-BE needs basic mechanisms for **quality signals and reporting**.

---
<br>

### 6.3 Achievement & Goal Tracking Gaps

**AS-IS**

- Official achievements/trophies:
  - are platform-bound,
  - sometimes shallow (main story only),
  - cannot be extended by the community.
- Cross-platform tracking sites:
  - aggregate official data,
  - focus on statistics and competition,
  - do not integrate fan-made sets as first-class objects.
- Fan-made lists:
  - are scattered across forums, wikis, spreadsheets, and chats,
  - require manual, personal tracking,
  - are hard to discover and hard to “prove” completed.

**TO-BE**

- For each game in Dream Project:
  - **official achievements** (where available) and **fan-made sets** are shown together.
- Fan-made sets:
  - are formally defined, versioned, and attributed to curators,
  - can include subsets and challenge branches,
  - can be linked to saves and screenshots as evidence.
- Where possible, official progress:
  - can be synchronised read-only from external profiles,
  - gives an initial baseline for the user’s completion state.

**Gaps**

- **Structural gap** – fan-made sets today lack a **shared structure** and place; TO-BE introduces a unified model per game.
- **Traceability gap** – today there is no robust, platform-wide way to show **which fan sets a user has completed**; TO-BE requires that fan progress is tracked like official achievements.
- **Evidence gap** – completion of fan lists is often “trust me, I did it”; TO-BE introduces **connections to screenshots/saves** as optional proof.
- **Extensibility gap** – official systems are closed; TO-BE allows community-driven **extension of what “completion” means** without replacing the official view.

---
<br>

### 6.4 Screenshot & Sticker Album Gaps

**AS-IS**

- Screenshots:
  - are easily captured but poorly organised,
  - live in local folders, platform galleries, or social feeds.
- There is no common practice of:
  - defining “one screenshot per boss/ending/area” as a structured album,
  - using screenshots as **formal proof** tied to achievement-like conditions.
- Players who want albums must create their own manual systems (folders, collages, notes).

**TO-BE**

- Each supported game can have **sticker album templates**:
  - designed by curators,
  - describing slots for bosses, locations, endings, special events.
- Players fill these slots:
  - by uploading screenshots,
  - with AI-based validation helping check whether the image matches the intent.
- Albums:
  - show clear completion status,
  - integrate into profiles and stats,
  - act as visual evidence aligned with achievements and game history.

**Gaps**

- **Organisation gap** – AS-IS screenshots are unstructured; TO-BE requires **templates and slots** per game.
- **Validation gap** – currently no shared mechanism to decide if a screenshot “counts”; TO-BE introduces **automated checks** plus human judgement.
- **Narrative gap** – screenshots now tell fragmented stories; TO-BE uses albums to present a **coherent visual narrative** for each playthrough.

---
<br>

### 6.5 Profiles, Curators, Catalogue & Governance Gaps

**AS-IS**

- Player histories are fragmented:
  - each platform shows only its part (Steam, PSN, Xbox, emulators, retro sites).
- Curators:
  - use many disconnected tools (spreadsheets, documents, forum posts),
  - lack a stable place where their sets/albums live and evolve.
- Catalogue and localisation context:
  - information about Ukrainian localisation and Ukrainian-made games is scattered,
  - most platforms do not provide strong controls around Russian-linked content,
  - Ukrainian creators often operate as a marginalised niche.

**TO-BE**

- Dream Project profiles:
  - aggregate a user’s activity across games and pillars (saves, achievements, albums),
  - provide readable stats like time to completion or filled albums.
- Curators:
  - get dedicated tools and recognition for their sets and templates,
  - can see how their content is used and improved over time.
- The catalogue:
  - can flag Russian-linked content and allow users to avoid or treat it with context,
  - can highlight games with Ukrainian localisation or Ukrainian origin,
  - supports both global and Ukrainian audiences intentionally.

**Gaps**

- **Profile gap** – AS-IS there is no unified, platform-agnostic **“progress identity”**; TO-BE requires such a profile within Dream Project.
- **Curator lifecycle gap** – today, curator work is fragile and scattered; TO-BE introduces a **formal role, tools, and recognition**.
- **Catalogue policy gap** – current platforms rarely reflect the Ukrainian context; TO-BE incorporates **explicit catalogue rules and labels** around localisation and Russian-linked content.

---
<br>

### 6.6 Summary

The gaps identified above explain why Dream Project is not “just another website” on top of existing tools:

- It addresses **context and meaning** of saves, not only their storage.
- It unifies **official and fan-made completion** in one place.
- It transforms screenshots into **structured, validated albums**, not loose files.
- It creates a **platform-agnostic progress profile** for players and a stable home for curators, with catalogue policies that explicitly account for Ukrainian needs.

These gaps feed directly into the next sections of the document:

- **Business Processes** – how users will move through the TO-BE world.
- **Business Rules** – what must be true for the system to behave consistently.
- **Data & Information View** – which information objects are needed to close these gaps.

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
