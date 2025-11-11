# Business Analysis Document

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

## 7. Business Processes

This section describes the key business processes around saves, achievements, screenshots, and curation.  
It is not a technical workflow specification, but a **domain-level view** of how players and curators act today (AS-IS) and how they are expected to act with Dream Project (TO-BE).

The processes covered here are:

1. Choosing a game and deciding “what to do next”.
2. Preserving and sharing saves.
3. Tracking achievements and challenges.
4. Capturing and using screenshots as memories.
5. Creating and maintaining community structures (fan sets, albums).

---
<br>

### 7.1 AS-IS Process Summaries

#### 7.1.1 Choosing a Game / Deciding What to Play

1. Player opens a storefront, launcher, or physical collection.
2. Scrolls through an often long list of owned or installed games.
3. Makes a choice based on:
   - mood,
   - what friends are playing,
   - current marketing or sales,
   - or random impulse.
4. If they want “something fitting my current mood / platform”, they:
   - search online for recommendation lists,
   - watch YouTube or read articles,
   - manually cross-check availability on their platform.
5. Any decision about “I want a game I can play in this specific way (route, challenge, completion style)” is handled mentally or through scattered notes.

---
<br>

#### 7.1.2 Preserving and Sharing Saves

1. Progress is saved automatically by the game/platform (cloud) or manually (local saves/save states).
2. For personal backup:
   - either the platform’s cloud handles it,
   - or the player manually copies save folders or uses a backup tool.
3. To share a save with someone:
   - the player locates the save folder or memory card image,
   - compresses it,
   - uploads it somewhere (file host, forum),
   - sends instructions on where and how to place it.
4. The recipient:
   - downloads the file,
   - follows the manual instructions,
   - hopes it matches their game version/platform and works correctly.
5. There is no central place to browse saves for a particular situation; discovery is done via search engines, forum threads, or word of mouth.

---
<br>

#### 7.1.3 Tracking Achievements and Challenges

1. The player unlocks **official achievements/trophies** by playing on modern platforms.
2. To see their status:
   - they open platform-specific overlays or profiles,
   - sometimes use third-party sites to aggregate across platforms.
3. If they want more than official lists:
   - they search for fan-made challenge lists, checklists, or “true 100%” guides.
4. Tracking completion of fan lists is usually done by:
   - marking entries in a personal spreadsheet or printed checklist,
   - updating a wiki account or forum signature manually,
   - or simply remembering what was done.
5. There is no unified view of “my official + fan-made progress” per game.

---
<br>

#### 7.1.4 Capturing & Using Screenshots

1. During play, the player occasionally takes screenshots via platform shortcuts or emulator hotkeys.
2. Screenshots are stored:
   - in local directories,
   - in a console gallery,
   - or in the launcher’s screenshot system.
3. When they want to share a moment:
   - they post the screenshot on social media, forums, or messaging apps,
   - add ad hoc text to explain context.
4. Over time, screenshots accumulate and:
   - become difficult to search or re-contextualise,
   - are rarely tied to specific achievements, bosses, or story beats in any structured way.
5. If players want a “collection” (e.g. one shot per boss), they manually organise folders or create their own collages.

---
<br>

#### 7.1.5 Creating & Maintaining Community Structures

1. A passionate player decides to create:
   - a fan achievement list,
   - a list of challenges,
   - or a thematic screenshot idea for a game.
2. They publish it via:
   - forum post,
   - Google Sheet or document,
   - wiki page,
   - Discord channel.
3. Other players:
   - discover it through links or recommendations,
   - possibly copy or adapt it in their own files.
4. Progress is tracked individually, not centrally:
   - each player manages their own version of the list,
   - creators do not have a clear view of how many people used or completed it.
5. Updating or versioning the list:
   - requires editing the original post/document,
   - may lead to multiple inconsistent copies existing simultaneously.

---
<br>

### 7.2 TO-BE Process Summaries

#### 7.2.1 Choosing a Game / Deciding What to Play (with Dream Project)

1. The user signs in to Dream Project and selects a **target platform** (e.g. PC, PS1, SNES).
2. They can:
   - search directly for a known game, *or*
   - ask for a **recommendation**.
3. Dream Project recommends a game+platform variant based on:
   - the user’s previous activity (saved games, completed sets, filled albums),
   - general popularity and curated suggestions,
   - possibly special criteria (anniversary of release, notable events).
4. The user can:
   - accept the suggestion,
   - or reject it and ask for another until something fits.
5. When a game is selected, its **game page** presents:
   - available saves,
   - official + fan achievement structures,
   - sticker albums.
6. The user decides what to do *for this game* right now (use a save, follow a set, fill an album) from a single, focused entry point.

---
<br>

#### 7.2.2 Preserving and Sharing Saves (with Dream Project)

1. A user finishes or reaches an interesting point in a game and wants others to use that state.
2. They open the game page in Dream Project, go to the **Saves** section, and click “Upload Save”.
3. They provide:
   - the save file,
   - target platform variant (e.g. PC, PS1),
   - a short description (“before final route choice”, “post-boss, level 60 mage build”),
   - optional tags (difficulty, route, build notes).
4. Dream Project:
   - stores the save,
   - associates it with the game and platform,
   - indexes metadata for search.
5. Another user:
   - visits the same game page,
   - filters saves by situation (e.g. “before final choice”) or tags (difficulty, build),
   - picks a save that fits their intention.
6. They download and follow platform-specific instructions (where needed), provided in a standardised, non-technical way.
7. Feedback (e.g. “useful/not useful”, report) helps the system and admins understand which saves are reliable and which require moderation.

---
<br>

#### 7.2.3 Tracking Achievements and Challenges (with Dream Project)

1. On the game page, the user switches to the **Achievements** section.
2. They see:
   - the **official achievement list** (if applicable),
   - one or more **fan-made sets** curated by the community.
3. If they choose to link external profiles (where allowed):
   - Dream Project reads their official achievement status,
   - marks already completed items automatically.
4. The user selects a fan-made set (or subset) they want to follow (e.g. “true 100% route”, “low-level completion subset”).
5. During play:
   - completion may be updated automatically via integrations (for retro/emulator scenarios),
   - or manually by the user, optionally adding:
     - screenshots,
     - references to saves that reflect the completed condition.
6. The set overview in Dream Project shows:
   - what is completed,
   - what remains,
   - optional completion time (from first recorded progress to final item).
7. Curators see aggregated, anonymised stats about how many players are using and completing their sets.

---
<br>

#### 7.2.4 Capturing & Using Screenshots as Stickers (with Dream Project)

1. While playing, the user takes screenshots as usual (via platform or emulator tools).
2. When they want to “turn them into something”, they:
   - open the game page in Dream Project,
   - go to the **Sticker Albums** section,
   - choose an album template (e.g. “main bosses”, “secret areas”, “endings”).
3. For each empty slot in the album:
   - they upload one of their screenshots,
   - optionally add a short caption.
4. Dream Project runs **AI-based validation** to check whether:
   - the screenshot roughly matches the described slot (boss, area, event),
   - the image is not obviously wrong (different game, menu screen, unrelated content).
5. If accepted:
   - the slot is marked as filled,
   - album completion percentage updates,
   - the user’s profile stats are updated (e.g. “Album X: 7/10 stickers filled”).
6. Completed albums (or key milestones) can grant:
   - profile badges,
   - visible markers that can be shared or referenced by content creators.

   ---
<br>

#### 7.2.5 Creating & Maintaining Community Structures (with Dream Project)

1. A curator decides to formalise their knowledge of a game into:
   - a fan achievement set,
   - and/or a sticker album template.
2. In Dream Project, they open curator tools for that game and:
   - define achievement/challenge items with names, descriptions, and categories,
   - group them into subsets or challenge branches where necessary,
   - or create an album template with defined sticker slots.
3. They can attach:
   - short hints or notes,
   - external links (GameFAQs, videos) to help players.
4. Once published:
   - the set or album appears on the game page,
   - players can start using it immediately.
5. Over time, the curator can:
   - review usage stats (how many started/completed),
   - refine descriptions or balance,
   - publish new versions if needed (with version history preserved).
6. For Ukrainian-focused curators and communities:
   - they can explicitly tag content as Ukrainian,
   - prioritise games with Ukrainian localisation or origin,
   - and avoid or flag Russian-linked titles according to catalogue policies.

---
<br>

These TO-BE process summaries will be expanded into more formal **use cases**, **user stories**, and **process diagrams** in later artefacts (Use Case Specifications, SRS, and visual models). Here, they provide the business narrative that those detailed specifications must support.

---
<br>
<br>

## 8. Business Rules

This section captures the main **business-level rules** that govern how Dream Project should behave.  
They will later be refined and extended in the BRD and SRS, but the principles here must remain valid regardless of technical implementation.

---
<br>

### 8.1 User Accounts & Roles

- **Roles and Permissions**
  - **Regular Users**
    - May register an account, manage their profile and preferences.
    - May search and view game pages, saves, achievement sets, and sticker albums.
    - May upload their own saves and screenshots, and decide whether they are private or shared.
    - May join and track fan achievement sets and fill sticker albums.
    - May rate or report content and leave feedback where enabled.
    - May link and unlink external gaming profiles (where integrations exist).
  - **Curators**
    - Have all capabilities of Regular Users.
    - May create, edit, and publish **fan achievement sets** for specific games.
    - May create, edit, and publish **sticker album templates** for specific games.
    - May deprecate or update their own sets and templates, with version history preserved.
    - May propose catalogue tags and corrections (e.g. localisation info, Ukrainian origin flags), subject to admin review.
  - **Platform Administrators**
    - Have all capabilities of Curators.
    - May manage user roles (promote/demote curators, suspend accounts when required).
    - May configure catalogue policies (e.g. flags for Russian-linked content).
    - May review and act on reported content (hide, delete, warn, restrict).
    - May configure high-level system settings (e.g. retention defaults, feature toggles).

- **Account Linking**
  - Linking external gaming profiles (Steam, retro services, etc.) is **optional** and requires explicit user consent.
  - Linked accounts are used in a **read-only** way for progress synchronisation; Dream Project does not modify data in external systems.
  - Users can revoke linked accounts at any time; after revocation, no new data is fetched and existing imported data is clearly marked as “last synchronised at \<date\>”.

- **Progress Visibility**
  - Each user profile shows high-level statistics about their activity (e.g. completed sets, completed albums, notable badges).
  - Users can control whether their profile is:
    - public,
    - visible only to logged-in users,
    - or limited to “private with shareable link” where feasible.

---
<br>

### 8.2 Save Management Rules

- **Save Identification**
  - Every shared save must be associated with:
    - a specific **game** entry,
    - a specific **platform variant** (e.g. PC, PS1, SNES),
    - and, where relevant, a version/region tag.
  - Each save must include a short human-readable description of its situation (e.g. “before final faction choice”, “post-boss X, level 60 mage build”).

- **Ownership & Visibility**
  - By default, uploaded saves are private to the uploader until they explicitly mark them as **shared**.
  - The uploader can:
    - change a shared save back to private,
    - or delete their own saves.
  - Administrators may hide or remove saves that violate policies or are clearly misleading or harmful.

- **Discovery & Use**
  - Shared saves are searchable by game, platform, and basic metadata (e.g. route, difficulty, key story points).
  - Dream Project must present **clear, platform-specific instructions** for how to use a downloaded save.
  - Where technical risk exists (e.g. overwriting local progress), the interface must display a **prominent warning**.

- **Versioning & Retention**
  - Users may keep multiple versions of saves per game; older versions may be archived or compressed according to configurable platform policies.
  - Saves that have not been accessed for a long period may be archived, with prior notice to the owner where feasible.

- **Reporting & Moderation**
  - Any signed-in user may report a save for:
    - corruption or non-functioning file,
    - malware suspicion,
    - misleading description,
    - policy violations.
  - Reported saves are marked with a warning until reviewed by an administrator.
  - Administrators may:
    - remove the save,
    - correct its metadata,
    - or leave it visible with an explanatory note if the report is not confirmed.

---
<br>

### 8.3 Achievement & Goal Tracking Rules

- **Official vs Fan-Made Structures**
  - Where supported, **official achievements** for a game are imported or represented as a read-only reference list.
  - **Fan-made achievement sets** are first-class objects in the system and:
    - can be created and maintained by Curators,
    - are clearly labelled as fan-made, with author attribution,
    - can coexist with each other and with official lists for the same game.

- **Integrations for Retro / Emulator Sets**
  - For retro games supported by external achievement services (e.g. RetroAchievements or similar):
    - Dream Project may mirror those achievement sets as structured lists.
    - Completion status is imported from the linked external profile in a **read-only** way.
    - Users cannot override externally synchronised completion flags for these items inside Dream Project.
    - Any changes in completion status must originate from the external service and be refreshed by synchronisation.

- **Set Structure & Variants**
  - Multiple fan-made sets per game are allowed.
  - Sets may contain:
    - base lists,
    - optional subsets (e.g. “low-level subset”, “no-death subset”),
    - tags indicating their focus (exploration, challenges, collectibles, etc.).
  - Each achievement item must have:
    - a clear name,
    - a description of the condition,
    - optional notes or hints.

- **Completion Tracking**
  - For **official lists**:
    - where integrations exist, completion status may be updated automatically from external profiles (read-only).
    - where integrations do not exist, users may **optionally self-report** completion.
  - For **external retro / emulator sets** (e.g. mirrored from RetroAchievements):
    - completion status is taken exclusively from the external profile once linked,
    - users cannot manually mark these items complete or incomplete in Dream Project,
    - if the user unlinks the external profile, the last known completion state remains visible but is clearly marked as “from external source, last updated \<date\>”.
  - For **Dream Project–native fan-made sets**:
    - users can mark items as completed.
    - for each completed item, users may attach **one or more pieces of evidence**, such as:
      - a screenshot (or reference to an existing sticker slot),
      - a save file or link to a save hosted on Dream Project,
      - a link to an external video or game clip (e.g. file-sharing or clip platforms).
    - attaching evidence is optional for normal use but may be required for any future “verified completion” statuses if such features are introduced.
  - The system should calculate completion status per set and may store:
    - completion timestamps,
    - approximate time taken from first recorded action to full completion.

- **Verification & Trust Model**
  - Dream Project’s default model is **trust with light verification**:
    - self-reported completion is accepted for native fan sets and non-integrated official lists,
    - attached evidence is primarily for social proof and community trust.
  - For evidence (screenshots, videos) the system may:
    - use AI-based checks to detect obviously unrelated content (wrong game, menus, unrelated footage),
    - highlight questionable items for manual review only in specific cases (e.g. suspected abuse, special badges, or repeated reports).
  - The platform does **not** aim to provide strict anti-cheat guarantees; it aims to keep data reasonably trustworthy without requiring a large manual moderation team.

- **Quality & Feedback**
  - Users may:
    - rate fan-made sets (e.g. helpfulness, clarity),
    - report items that are unclear, impossible, or in violation of policy.
  - Low-rated or frequently reported sets may be flagged for curator or admin review.
  - Curators can publish revised versions of sets; older versions remain reference-only.

- **Content Rules**
  - Achievement sets must:
    - describe goals that can be achieved within normal game use (no requirement for cheating, exploiting platform vulnerabilities, or using prohibited tools),
    - respect content and community guidelines (no hate, harassment, or prohibited themes).
  - Administrators may remove or require changes to sets that violate these rules.

---
<br>

### 8.4 Sticker Album & Screenshot Rules

- **Album Templates**
  - Sticker album templates are defined per game (and, where relevant, platform variant) by Curators.
  - Each template:
    - defines a finite set of **slots** (e.g. one per boss, area, ending, secret),
    - includes descriptions and optional spoiler warnings for each slot,
    - may include tags (e.g. “main story album”, “secret endings album”).

- **Album Types & Spoiler Levels**
  - Each album template must be assigned a **type**, for example:
    - **“First Playthrough Friendly”** – designed for new players; descriptions avoid heavy spoilers and focus on gentle hints.
    - **“Post-Completion / Deep Dive”** – intended for players who have already finished the game; descriptions may be explicit about bosses, endings, and secrets.
  - For each slot, curators may define:
    - a short **hint text** (minimal spoilers, intended for first-playthrough use),
    - a full **detailed description** (which may contain spoilers).
  - For “First Playthrough Friendly” albums:
    - the UI should initially show only the hint text,
    - the full description is hidden behind a spoiler toggle so users can choose how much information they want.
  - For “Post-Completion / Deep Dive” albums:
    - the full description may be shown by default, with spoiler labelling where appropriate.

- **Guidance So Players Don’t Miss Key Moments**
  - For each album template, Dream Project may provide a **“next suggested slot”** view that:
    - lists upcoming slots in a recommended order (e.g. main story sequence),
    - shows only the non-spoiler hints for first-playthrough albums (e.g. “First major boss encounter – remember to capture something during the fight”),
    - lets users mark slots as “planned” or “already passed” to help them track what they still want to capture.
  - This guidance is provided **inside Dream Project’s UI** (web/mobile) before or between play sessions; it does not require or assume any in-game overlay on consoles or external platforms.
  - It is understood that some players may still miss moments or use internet images instead of personal screenshots; the system aims to reduce accidental misses without trying to enforce perfect behaviour.

- **Filling Albums**
  - Users fill slots by:
    - uploading their own screenshots,
    - or, where supported, reusing screenshots they previously uploaded for that game.
  - Each uploaded screenshot is associated with:
    - a specific slot,
    - the game and platform variant,
    - the user who uploaded it.

- **Validation & Spoilers**
  - An AI-based validation step is used to check whether a screenshot reasonably matches the intended slot (scene, character, location).
  - If validation clearly fails, the user is informed and may:
    - retry with a different screenshot,
    - or, for borderline cases, request manual review (subject to platform capacity).
  - Images marked as potentially containing spoilers or sensitive content are:
    - auto-blurred or hidden behind a spoiler layer,
    - only revealed when the viewer explicitly chooses to see them.

- **Completion & Statistics**
  - Album completion is calculated based on filled slots.
  - Completing an album may:
    - unlock a game-specific badge on the user profile,
    - contribute to overall completion statistics for that game.
  - The system may display aggregated statistics such as:
    - percentage of users who completed an album,
    - typical time from first sticker to full completion (in an anonymised way).

- **Content Ownership & Authenticity**
  - Users remain the owners of the screenshots they upload.
  - Users may delete their own screenshots and filled slots; albums will reflect such changes.
  - Administrators may remove screenshots that violate content policies.
  - The system cannot guarantee that every screenshot was taken personally by the uploader, but:
    - AI validation and community reporting should be used to discourage clearly mismatched or deceptive content,
    - any stricter “verified” status (if introduced) must rely on stronger checks than regular slot filling.

---
<br>

### 8.5 Catalogue & Localisation Rules

- **Game Entries**
  - Each game entry must include:
    - title and basic metadata,
    - one or more platform variants,
    - available language/localisation information where known.
  - Where possible, entries should record:
    - developer and publisher,
    - country/region information.

- **Russian-Linked Content (Metadata & Visibility)**
  - The platform must support explicit metadata fields for Russian-linked content (e.g. developer or publisher registered in Russia, or other clear associations).
  - How this metadata is **displayed** can depend on the user’s locale and preferences:
    - For users who choose a Ukrainian language interface (or explicitly opt in), Russian-linked flags and explanatory notes should be clearly visible and filterable.
    - For other users, Russian-linked metadata may be:
      - less prominent by default,
      - still available via filters or info panels for those who want to see it.
  - Global catalogue policies (e.g. exclusion from certain views) are configured by Platform Administrators; regular users and curators do not decide policy, but can see and use the results.

- **Ukrainian Localisation & Origin**
  - Games with official Ukrainian localisation must be:
    - clearly marked in the catalogue,
    - filterable for users who want to focus on such titles.
  - Games developed by Ukrainian teams or strongly associated with Ukraine should:
    - be tagged accordingly,
    - be eligible for special highlighting in Ukrainian-focused views or campaigns.
  - These tags are primarily maintained by Platform Administrators or dedicated catalogue maintainers; users and curators can suggest corrections, but are not responsible for maintaining this data.

- **Responsibility for Classification**
  - Responsibility for classifying games as Russian-linked or Ukrainian-origin lies with:
    - the platform’s catalogue/data maintainers,
    - and Platform Administrators who approve or adjust sensitive metadata.
  - Curators:
    - are **not required** to research or decide on Russian/Ukrainian classifications as part of their normal curation work,
    - may optionally propose corrections if they notice obvious mistakes, via a simple suggestion mechanism.
  - Sensitive changes (e.g. adding or removing a Russian-linked flag, marking a game as Ukrainian origin) must go through an admin review process.

- **Curator Proposals**
  - Curators and regular users may propose corrections or additions to catalogue metadata (e.g. incorrect localisation info, missing Ukrainian language support).
  - All proposals affecting sensitive fields (Russian-linked status, Ukrainian origin) require explicit approval from Platform Administrators or catalogue maintainers before becoming visible to all users.

---
<br>

### 8.6 Moderation & Reporting Rules

- **Reportable Items**
  - The following content types can be reported by users:
    - saves,
    - screenshots and album slots,
    - achievement sets and individual items,
    - album templates,
    - user comments or profile content (where applicable).

- **Reporting Workflow**
  - Any signed-in user can submit a report with:
    - a reason category (e.g. corruption, misleading, offensive content),
    - optional free-text explanation.
  - Reported items are:
    - marked as “under review” or shown with a warning,
    - still visible unless they pose clear legal or safety risks, in which case admins may hide them immediately.

- **Administrative Actions**
  - Admins may:
    - keep content as-is (closing the report),
    - edit metadata or mark items as deprecated,
    - hide or delete content,
    - apply user-level actions (warnings, temporary restrictions, permanent bans) for repeated or severe violations.
  - All moderation actions should be logged for audit purposes.

- **Appeals**
  - Content owners may request a review of moderation decisions within a defined time window (e.g. 7 days).
  - Where possible, a different admin should handle appeals to avoid direct self-review.

---
<br>

### 8.7 Data & Privacy Rules (Business View)

- **User Control**
  - Users must be able to:
    - download or export their own content and basic profile data upon request,
    - delete their account (subject to legal and safety constraints),
    - remove individual uploads (saves, screenshots) they own.

- **Minimal Data Principle**
  - Dream Project collects and stores only the personal data necessary to:
    - operate accounts and roles,
    - provide progress tracking features,
    - comply with legal and security obligations.
  - Linking external profiles is optional and limited to the minimum scope needed for read-only progress synchronisation.

- **Retention**
  - Public saves and content that have been inactive for a long period may be archived or removed according to platform policies, with reasonable attempts to notify content owners beforehand.
  - Core profile and progress data are stored as long as the account is active or as required by applicable regulations; after account deletion, remaining data should be anonymised or removed where feasible.

- **Security & Compliance**
  - Personal data and user-generated content must be protected with appropriate security measures (e.g. encryption in transit and at rest, access controls).
  - The platform must follow privacy regulations applicable to its target regions and clearly communicate key terms in its public-facing policies.

---
<br>
<br>

## 9. Data & Information View

This section describes the main **business information objects** used by Dream Project and how they relate to each other.  
It is a **conceptual view**, not a database design: the goal is to clarify *what information the platform cares about* and *how it hangs together*.

---
<br>

### 9.1 Conceptual Overview

At a high level, Dream Project’s data model revolves around four groups of objects:

1. **Catalogue & Context** – *what* exists in the world of games:
   - Game, Platform Variant, Localisation & Origin metadata, Catalogue Tags.

2. **Users, Roles & Profiles** – *who* is using the platform:
   - User Account, User Profile, Role Assignments, External Profile Links.

3. **Progress & Evidence** – *what users have actually done*:
   - Saves, Achievement Sets & Items, Completion Records, Sticker Albums, Screenshots, Evidence Links.

4. **Governance & Moderation** – *how the platform stays safe and compliant*:
   - Reports, Moderation Decisions, Catalogue Policy Flags (e.g. Russian-linked, Ukrainian-origin).

The following subsections describe these objects in business terms.

---
<br>

### 9.2 Catalogue & Context Entities

#### 9.2.1 Game

Represents a distinct game title as understood by players (e.g. *Final Fantasy X*, *Silent Hill 2*, *Hollow Knight*).

**Key aspects:**

- Name, alternative titles (regional names, abbreviations).
- Generic description / summary.
- High-level genre and tags (RPG, JRPG, Metroidvania, horror, etc.).
- Canonical release year (for reference and recommendations).

**Relationships:**

- A **Game** has one or more **Platform Variants**.
- A **Game** can have multiple **Sticker Album Templates** and multiple **Fan Achievement Sets**.
- A **Game** is linked to localisation and origin metadata (e.g. “has official Ukrainian localisation”, “developed by Ukrainian studio”).

---
<br>

#### 9.2.2 Platform Variant

Represents a specific combination of **game + platform** (e.g. *Final Fantasy X – PS2*, *Final Fantasy X – PS4 Remaster*, *Hollow Knight – PC*).

**Key aspects:**

- Platform type (PC, PS1, PS2, SNES, Switch, etc.).
- Specific edition/remaster identifiers if needed.
- Compatibility and version notes relevant to saves and screenshots (e.g. saves from PS2 NTSC may not work with PS2 PAL).

**Relationships:**

- Each **Platform Variant** belongs to exactly one **Game**.
- Saves, screenshots, albums and some achievement sets may be scoped to a specific **Platform Variant**.

---
<br>

#### 9.2.3 Localisation & Origin Metadata

Business-level metadata describing **language and origin context**, especially for Ukrainian users.

**Key aspects:**

- Localisation info:
  - “Has official Ukrainian localisation” (yes/no/unknown).
  - Available languages list.
- Origin info:
  - Developer and publisher,
  - Country/region associations (e.g. Ukrainian developer, Russian publisher).

**Relationships:**

- Linked to **Game** (generic info) and/or **Platform Variant** (platform-specific localisation differences).
- Provides inputs for catalogue rules:
  - Ukrainian highlighting,
  - Russian-linked flags and filters.

---
<br>

#### 9.2.4 Catalogue Tag

Represents a reusable **tag** used throughout the catalogue.

**Examples:**

- Content tags: “retro”, “JRPG”, “survival horror”.
- Context tags: “Ukrainian localisation”, “Ukrainian developer”.
- Policy tags: “Russian-linked”.

**Relationships:**

- Tags may be attached to **Games**, **Platform Variants**, **Album Templates**, **Fan Sets**, etc.
- Tag assignments may be proposed by Curators or users, but sensitive tags are approved by Platform Administrators.

---
<br>

### 9.3 User, Role & Profile Entities

#### 9.3.1 User Account

Represents the authenticated identity of a person using Dream Project.

**Key aspects:**

- Login credentials / authentication method (not detailed here).
- Basic contact/notification preferences.
- Status (active, suspended, deleted).

**Relationships:**

- Each **User Account** is associated with exactly one **User Profile**.
- A **User Account** may have one or more **Role Assignments**.
- A **User Account** may have multiple **External Profile Links**.

---
<br>

#### 9.3.2 User Profile

Represents the visible “face” of a user on the platform (nickname, avatar, basic stats).

**Key aspects:**

- Display name, avatar, optional bio.
- Visibility settings (public / members-only / restricted).
- High-level stats:
  - number of completed achievement sets,
  - number of completed sticker albums,
  - badges (e.g. “completed X albums”, “curator for Y game”).

**Relationships:**

- A **User Profile** belongs to one **User Account**.
- A **User Profile** is the subject of:
  - **Completion Records** (for achievements and sets),
  - **Sticker Album Instances**,
  - **Save Entries** (created or shared),
  - **Screenshots** uploaded.

---
<br>

#### 9.3.3 Role Assignment

Represents the link between a **User Account** and a **role** (Regular User, Curator, Platform Administrator).

**Key aspects:**

- Role type (e.g. Regular User, Curator, Platform Administrator).
- Date when the role became effective (e.g. when a user was promoted to Curator or Admin).
- Current status (active / revoked) if roles can change over time.

**Relationships & Visibility:**

- A **User Account** may have multiple **Role Assignments** over its lifetime (e.g. Regular User → Curator).
- Technical details of who changed which role are kept internally for admin/audit purposes, but not exposed in business-facing documentation.
- On the **user-facing profile**, the main visible effects of role assignments are:
  - role badges or labels (e.g. “Curator”),
  - summary of contributions relevant to that role (e.g. number of achievement sets or sticker albums created by that curator).

---
<br>

#### 9.3.4 External Profile Link

Represents a connection between a **User Account** and an external gaming or achievement service (e.g. Steam, RetroAchievements).

**Key aspects (internal view):**

- External service identifier (Steam, RetroAchievements, etc.).
- External account reference (ID or username) stored securely and used only for synchronisation.
- Status (linked, revoked).
- Date of last synchronisation and basic scope of imported data (e.g. “official achievements only”).

**Relationships & Visibility:**

- Each **External Profile Link** belongs to one **User Account**.
- External links are used by:
  - achievement import processes,
  - retro set completion synchronisation.

**User-facing behaviour & privacy:**

- Only the **account owner** sees detailed information about linked external accounts (which services are linked, and controls to unlink them).
- Other users:
  - **do not** see external usernames or IDs,
  - only see aggregated results (e.g. which official achievements are completed) as part of normal profile and game views.
- Dream Project must avoid exposing information that would help someone compromise external accounts; external identifiers are used internally for read-only sync and are not shown publicly.

---
<br>

### 9.4 Saves & Progress Evidence Entities

#### 9.4.1 Save Entry

Represents an **uploaded save** that Dream Project knows about (private or shared).

**Key aspects:**

- Game and Platform Variant.
- Human-readable description (situation, route, build).
- Ownership (which user uploaded it).
- Visibility (private/shared).
- Basic metadata (e.g. in-game progress markers when available: chapter, level, difficulty).

**Relationships:**

- Each **Save Entry** belongs to one **User Profile** (owner).
- Each **Save Entry** is tied to a specific **Platform Variant**.
- A **Save Entry** may be linked as **evidence** for:
  - Completion of one or more **Achievement Items**,
  - Specific slots in a **Sticker Album** (if relevant).

---
<br>

#### 9.4.2 Save Version (optional conceptual object)

In some designs, multiple file versions or updates of the same logical save are stored.

**Key aspects:**

- Version identifier, creation date.
- Technical file details (not detailed at BA level).

**Relationships:**

- A **Save Entry** may have many **Save Versions**, with one marked as “current”.

---
<br>

#### 9.4.3 Screenshot / Media Asset

Represents an uploaded image (or, later, potentially short clips).

**Key aspects:**

- Owner (User Profile).
- Associated Game and Platform Variant.
- Basic description/caption.
- Flags (e.g. potential spoiler, NSFW, policy risk).

**Relationships:**

- A **Screenshot** may be:
  - linked to one or more **Sticker Slots** (as part of album filling),
  - linked as evidence for **Achievement Item** completion.
- A **Screenshot** belongs to one **User Profile**.

---

### 9.5 Achievement & Completion Entities

#### 9.5.1 Achievement Source

Represents the **origin type** of an achievement structure:

- Official platform list (Steam/console achievements).
- External retro services (e.g. RetroAchievements).
- Dream Project–native fan-made sets.

This can be a conceptual attribute applied to sets/items rather than a separate entity in implementation, but is important to distinguish in analysis.

---
<br>

#### 9.5.2 Achievement Set

Represents a **group of achievements or challenges** related to a Game (and optionally a Platform Variant).

**Examples:**

- Official list: “Steam Achievements for Game X”.
- Retro external set: “RetroAchievements set for Game Y (SNES)”.
- Fan-made sets: “True 100% Route”, “Low-Level Challenge Subset”.

**Key aspects:**

- Game (and optionally Platform Variant).
- Source type (official / external retro / fan-made).
- Author/owner (for fan-made sets).
- Status (active, deprecated, superseded).
- Descriptive metadata (theme, difficulty, recommended audience).

**Relationships:**

- A **Game** may have multiple **Achievement Sets**.
- A **Platform Variant** may narrow which sets apply (especially for retro sets).
- Each **Achievement Set** contains multiple **Achievement Items**.
- Completion for a set is tracked via **Completion Records** (aggregated over items).

---
<br>

#### 9.5.3 Achievement Item

Represents a **single achievement or challenge condition**.

**Key aspects:**

- Name and description.
- Optional notes/hints.
- Category tags (story, collectible, challenge, etc.).
- For external sources:
  - external ID,
  - read-only status.

**Relationships:**

- Belongs to exactly one **Achievement Set**.
- May have many **Completion Records** (one per user who interacts with it).
- May have associated **Evidence Links** (screenshots, saves, external clips).

---
<br>

#### 9.5.4 Completion Record

Represents a **user’s relationship** to an Achievement Item and, by aggregation, to an Achievement Set.

**Key aspects:**

- User Profile.
- Achievement Item (and its set).
- Completion status (not started / in progress / completed).
- Timestamps (first recorded activity, completion time).
- Flags indicating source of completion:
  - imported from external official/retro service,
  - self-reported with evidence,
  - self-reported without evidence.

**Relationships:**

- Each **Completion Record** belongs to one **User Profile** and one **Achievement Item**.
- May be linked to one or more **Evidence Links**.

---
<br>

#### 9.5.5 Evidence Link

Represents a connection between a **Completion Record** and the pieces of proof a user chooses to attach.

**Key aspects:**

- Evidence type:
  - Screenshot (internal),
  - Save Entry (internal),
  - External clip URL (e.g. video hosting, file-sharing),
  - Other future types if introduced.
- Optional short description.

**Relationships:**

- Each **Evidence Link** belongs to one **Completion Record**.
- Points to one **Screenshot**, **Save Entry**, or external URL.

---
<br>

### 9.6 Sticker Albums & Visual Progress Entities

#### 9.6.1 Sticker Album Template

Represents the **design** of a sticker album for a specific game (and optionally platform variant).

**Key aspects:**

- Game (and optional Platform Variant).
- Album type:
  - “First Playthrough Friendly”,
  - “Post-Completion / Deep Dive”, etc.
- Description (what this album is about).
- Author/curator and version status.

**Relationships:**

- A **Game** may have multiple **Sticker Album Templates**.
- A **Sticker Album Template** contains multiple **Sticker Slot Definitions**.
- Users create **Sticker Album Instances** based on templates.

---
<br>

#### 9.6.2 Sticker Slot Definition

Represents a **single slot** in a template that needs to be filled with a screenshot.

**Key aspects:**

- Name or identifier (e.g. “Boss 1”, “Secret Area – Hidden Garden”).
- Hint text (non-spoiler guidance).
- Detailed description (possibly spoiler-heavy).
- Spoiler flags and content tags.

**Relationships:**

- Belongs to exactly one **Sticker Album Template**.
- Instances of this slot for individual users are represented as **Sticker Slot Fills**.

---
<br>

#### 9.6.3 Sticker Album Instance

Represents one user’s **personal album** based on a template.

**Key aspects:**

- Owner (User Profile).
- Template used.
- Completion status and percentage.
- Start and completion timestamps.

**Relationships:**

- Created from one **Sticker Album Template**.
- Contains multiple **Sticker Slot Fills** (one per slot in the template).
- Contributes to User Profile stats and potentially badges.

---
<br>

#### 9.6.4 Sticker Slot Fill

Represents a **user’s screenshot** filling a specific slot in their album instance.

**Key aspects:**

- Status (empty / filled / under review).
- Associated screenshot.
- Timestamps (when filled, last updated).
- Validation state (accepted by AI, flagged, manually reviewed).

**Relationships:**

- Each **Sticker Slot Fill** belongs to one **Sticker Album Instance** and one **Sticker Slot Definition**.
- References exactly one **Screenshot** (or is empty).
- May indirectly serve as evidence for **Completion Records** if linked.

---
<br>

### 9.7 Governance, Reporting & Policy Entities

#### 9.7.1 Content Report

Represents a **user-initiated report** about an item that may violate policy or be problematic.

**Key aspects:**

- Reported item reference (save, screenshot, achievement item/set, template, comment, profile).
- Reporting user.
- Reason category (e.g. corruption, inappropriate content, misleading, spam).
- Optional free-text explanation.
- Status (open, under review, resolved).

**Relationships:**

- Each **Content Report** refers to one target object.
- Moderation actions are linked via **Moderation Decisions**.

---
<br>

#### 9.7.2 Moderation Decision

Represents an action taken by an administrator in response to a report or proactive review.

**Key aspects:**

- Target object reference.
- Action taken (keep, edit metadata, hide, delete, warn user, restrict user, ban user).
- Responsible administrator.
- Date/time and rationale.

**Relationships:**

- May be linked to one or more **Content Reports**.
- Influences state of the reported item and possibly **User Account** status.

---
<br>

#### 9.7.3 Catalogue Policy Flag

Represents specific **policy-related metadata** on catalogue objects.

**Examples:**

- “Russian-linked developer/publisher”.
- “Ukrainian-origin game”.
- “Contains Ukrainian localisation”.

**Key aspects:**

- Flag type.
- Source of information (internal research, trusted external database, user suggestion reviewed by admin).
- Approval status and audit trail.

**Relationships:**

- Attached primarily to **Game** and **Platform Variant** entities.
- Used by:
  - filters in the user interface (e.g. hide/show Russian-linked content),
  - special Ukrainian-focused views or highlights.

---
<br>

### 9.8 Traceability View

From a business perspective, the information model must support **end-to-end traceability**, such as:

- From a **User Profile** to:
  - which **Games** they interacted with,
  - which **Achievement Sets** and **Sticker Albums** they completed,
  - which **Saves** they contributed.

- From a **Game** to:
  - its **Platform Variants**, **Album Templates**, **Fan Sets**,
  - all **user activities** related to it (saves, completions, filled albums).

- From a single **Achievement Item** or **Album Slot** to:
  - every **Completion Record** or **Sticker Slot Fill** associated with it,
  - evidence provided (screenshots, saves, clips).

- From a **Report** or **Policy Flag** to:
  - all affected content and users,
  - decisions taken and their rationale.

This traceability is essential for:

- building trust in the data (users can see what their progress is based on),
- supporting moderation and policy enforcement,
- enabling meaningful analytics and product decisions based on real usage.

---
<br>
<br>


## 10. Assumptions & Constraints (BA-Level)

This section lists the **assumptions** and **constraints** that directly influence how requirements, processes, and data structures are defined for Dream Project.  
More detailed, evolving items will be maintained in the separate **Risks, Assumptions & Constraints Register**; here we capture the stable, BA-level view.

---
<br>

### 10.1 Key Business Assumptions

#### 10.1.1 Users & Community

- There is a niche but motivated audience of:
  - players who care about completion, retro gaming, and preserving their progress;
  - community-minded people willing to act as curators and set creators.
- Users will often adopt Dream Project for **one main pillar** first (saves, achievements, or sticker albums) and only later explore others.
- A small group of early curators (global EN + Ukrainian) will be reachable via:
  - existing communities (Discord servers, forums, RetroAchievements community, etc.),
  - simple outreach (social posts, personal contacts),
  - without needing a large marketing budget.
- Users accept that Dream Project is an **overlay/companion**, not a replacement for stores or platforms:
  - games are still purchased, installed, and launched elsewhere;
  - Dream Project is primarily about tracking, preserving, and presenting progress.

  ---
<br>

#### 10.1.2 Ecosystem & Integrations

- Major platforms (Steam, console ecosystems) and retro services (e.g. RetroAchievements) will continue to:
  - expose some level of profile/achievement data through APIs or export mechanisms;
  - allow **read-only** use of that data within the limits of their terms.
- For MVP, only a **small number** of integrations will be supported:
  - at least one modern platform for official achievements (if feasible),
  - at least one retro achievement ecosystem.
- There is no guarantee of long-term stability for third-party APIs; the system must tolerate integrations being temporarily unavailable or permanently removed (e.g. by degrading gracefully to manual tracking).

---
<br>

#### 10.1.3 Catalogue & Content

- A usable initial **game catalogue** (titles + platform variants) can be assembled using:
  - one or more public or licensed data sources,
  - manual curation for key titles, especially retro and “flagship” games.
- Localisation and origin metadata (e.g. “has Ukrainian translation”, “Ukrainian developer”, “Russian-linked publisher”) can be identified for at least:
  - major modern titles,
  - selected classics/retro games important to the target audience.
- Platform Administrators or dedicated catalogue maintainers, not curators in general, are responsible for:
  - approving sensitive flags (Russian-linked, Ukrainian-origin),
  - keeping high-impact catalogue metadata consistent over time.

  ---
<br>

#### 10.1.4 Data, AI & Verification

- AI-based checks (for screenshot validation, evidence plausibility, content risk) are treated as **assistance**, not as absolute truth:
  - they may flag obviously wrong or suspicious content,
  - they may occasionally misclassify borderline cases.
- The platform’s **default trust model** is:
  - self-reporting for fan-made sets + optional evidence,
  - automatic imports where external services provide reliable completion data,
  - light AI and community reporting to discourage abuse.
- Dream Project is not positioned as an anti-cheat authority; minor inaccuracies in reported progress are acceptable as long as users generally feel the data is trustworthy and transparent.

---
<br>

#### 10.1.5 Team, Operations & Moderation

- The core team for MVP is **small** (1 product/BA, 1–2 engineers, 1 designer, 1 QA, part-time admins), which implies:
  - moderation capacity is limited,
  - manual reviews must focus on clearly prioritised cases (e.g. multiple reports, severe policy issues),
  - self-service flows and automated checks are preferred where reasonable.
- There will be enough available admin time to:
  - handle critical moderation,
  - review sensitive catalogue flags,
  - manage curator promotions and role changes,
  without offering 24/7 real-time support.

---
<br>

### 10.2 Key Constraints

#### 10.2.1 Scope & MVP Constraints

- **MVP focus**:
  - Web-based application (desktop and mobile web).
  - No native mobile apps in the first release.
  - No direct in-game overlays, hooks, or unofficial injectors for proprietary platforms.
- Feature scope is constrained to:
  - save upload/search/use for a **limited number of platforms** and formats,
  - achievement tracking for official lists (where feasible) + a first wave of fan-made sets,
  - basic sticker albums with AI-assisted validation,
  - simple profiles and stats,
  - essential moderation and reporting.
- Out of scope for MVP:
  - complex social features (friends lists, global chat, tournaments),
  - leaderboards beyond basic aggregated statistics,
  - automatic cross-platform save conversion,
  - tournament-style competitions or prize systems.

These constraints mean that some user needs will be deferred to later phases even if they are valid and aligned with the overall vision.

---
<br>

#### 10.2.2 Technical & Integration Constraints

- All integrations with external platforms and services must:
  - be **read-only** from Dream Project’s perspective,
  - comply with published terms and rate limits,
  - avoid any behaviour that could be interpreted as cheating, scraping, or API abuse.
- External usernames, IDs, and tokens are treated as **sensitive internal data**:
  - they are stored securely,
  - they are not displayed to other users,
  - they are used only to fetch progress data and then reduced to aggregated, in-platform progress views.
- The MVP infrastructure is sized for **modest traffic**:
  - not engineered for tens of millions of users from day one,
  - performance and scalability must be “good enough” for early adopters and testers, with clear upgrade paths later.

  ---
<br>

#### 10.2.3 Legal, Policy & Localisation Constraints

- The platform must operate under **GDPR-like privacy expectations**:
  - clear consent for external account linking,
  - ability for users to request export or deletion of their data (subject to legal/safety constraints),
  - minimal collection and retention of personal information.
- Catalogue policy around **Russian-linked content** and **Ukrainian-origin games** must:
  - be centrally defined and configurable by Platform Administrators,
  - avoid forcing political context on users who do not opt in, while still:
    - clearly exposing this information to Ukrainian-language users and others who request it,
    - allowing Ukrainian users to filter or avoid Russian-linked titles.
- Content guidelines (for screenshots, achievement names/descriptions, comments) are a hard constraint:
  - illegal, hateful, or otherwise prohibited content must be removable,
  - sticker and achievement descriptions must avoid inciting cheating, harassment, or platform policy violations.

  ---
<br>

#### 10.2.4 UX & Accessibility Constraints

- Dream Project must remain usable for:
  - players who are not technically inclined (no assumption that they know file paths or emulator internals),
  - players accessing the site from a range of devices and screen sizes.
- Documentation and UI text must:
  - be understandable to both global EN users and Ukrainian users,
  - avoid overloading users with BA or developer jargon,
  - clearly explain when something is an **assumption** (“we can only detect X in this way”) versus a guaranteed system behaviour.
- There is limited capacity for custom workflows per game; most flows should follow a **consistent pattern** across titles, with only metadata and templates varying.

---
<br>

### 10.3 How This Section Is Used

- When writing or reviewing requirements, we should ask:
  - “Does this requirement rely on one of the assumptions above? If that assumption fails, what breaks?”
  - “Is this requirement violating any of the constraints (scope, legal, technical, moderation capacity)?”
- If critical assumptions are later proven false (e.g. an external API is removed, curator engagement is lower than expected), this section should be updated and linked to:
  - the **Risks, Assumptions & Constraints Register**,
  - any impacted requirements, processes, or data structures.

This ensures that the Business Analysis Document remains honest about the environment in which Dream Project is being designed, and helps the team make consistent, realistic decisions about scope and behaviour.


---
<br>
<br>

## 11. Non-Functional Considerations (Business View)

This section outlines the key **non-functional qualities** that Dream Project should achieve at MVP and beyond.  
Technical details (specific SLAs, architecture choices, tools) will be defined in the SRS and implementation documents; here we focus on the **business expectations** behind them.

---
<br>

### 11.1 Performance & Scalability

From a user’s perspective:

- Pages for **games, saves, achievements, and albums** should load quickly enough that the site feels responsive, even on average home connections.
- Common interactions (searching games, filtering saves, marking achievements, filling album slots) should complete without noticeable delays in normal conditions.
- The platform should comfortably support:
  - a growing but still niche community (early adopters, curators, content creators),
  - occasional spikes in traffic (e.g. when a specific game or feature becomes popular).

From a business view, this means:

- Prioritising fast responses for **read-heavy** operations (browsing, viewing profiles, game pages).
- Accepting that some heavier operations (e.g. large AI validations, full re-sync from external APIs) may be slightly slower or handled asynchronously, as long as the UI communicates clearly what is happening.
- Designing for **gradual scaling**: the MVP does not need “massive platform” capacity, but should not rely on choices that become impossible to scale later.

---
<br>

### 11.2 Security & Privacy

Non-functionally, Dream Project must be seen as **safe and trustworthy**:

- User accounts:
  - Protected with modern authentication practices.
  - Guarded against common attacks (credential stuffing, simple brute force).
- External account links:
  - Treated as sensitive; tokens and identifiers must never be exposed to other users.
  - Used strictly in **read-only** mode for progress synchronisation.
- Personal data and user-generated content:
  - Protected with appropriate encryption in transit and at rest.
  - Access limited by role (Regular User, Curator, Admin) and only where necessary.

Privacy expectations:

- Clear explanation of:
  - what data is collected,
  - how it is used (e.g. progress tracking, statistics),
  - how external accounts are linked and can be revoked.
- Ability for users to:
  - control profile visibility,
  - remove their own uploads,
  - request export or deletion of their data (within legal and safety limits).

Business implication: users should **feel safe** linking their progress and uploading their content; lack of trust here would undermine the entire concept of long-term progress preservation.

---
<br>

### 11.3 Reliability & Availability

From the user’s point of view:

- Dream Project should be reliably available for everyday use; unexpected downtime should be rare.
- Critical flows (sign-in, viewing game pages, accessing own saves/achievements/albums) should work consistently.

Business-level expectations:

- Occasional maintenance windows are acceptable, but:
  - they should be announced where possible,
  - they should avoid data loss.
- In case of failures:
  - saves and progress records should not be corrupted or silently discarded,
  - users should see clear error messages rather than broken pages.

Backups & recovery (conceptual):

- Regular backups of core data (catalogue, profiles, saves metadata, progress records).
- A defined recovery approach so that accidental data loss can be minimised and corrected.

---
<br>

### 11.4 Usability & Accessibility

Dream Project targets players with **mixed technical skills**. Non-functionally:

- Interfaces for:
  - uploading saves,
  - linking external profiles,
  - filling albums,
  must be understandable without requiring detailed knowledge of file systems, emulators, or APIs.
- Controls and texts should:
  - explain clearly what an action will do (“Share save”, “Mark as completed”, “Attach evidence”),
  - avoid BA/dev jargon where possible.

Accessibility considerations:

- Layouts and controls should work on common desktop and mobile browsers.
- Basic accessibility principles should be respected:
  - text that is readable without zoom tricks,
  - sensible contrast,
  - navigation that does not rely solely on complex gestures.

For Ukrainian and global EN users:

- Key flows should be fully usable in both languages over time.
- Where content is not fully localised yet, the UI should fail gracefully (e.g. not mixing random English fragments into Ukrainian text in a confusing way).

---
<br>

### 11.5 Maintainability & Extensibility

The platform is expected to evolve:

- New integrations (new platforms, new retro services) may be added.
- New types of albums, sets, or progress views may be introduced later.
- Catalogue policies (especially around origin and localisation) may change over time.

Non-functional expectations:

- The core model (Games, Platform Variants, Sets, Albums, Profiles) should be stable enough that:
  - new features can attach to it without major rewrites,
  - most changes are additive rather than disruptive.
- Configuration (e.g. catalogue flags, policy toggles) should be:
  - editable by Platform Administrators without code changes where possible,
  - traceable (who changed what, when).

This is essential so that Dream Project can respond to community feedback and ecosystem changes without constantly “starting from scratch”.

---
<br>

### 11.6 Localisation, Catalogue Policy & Ethical Behaviour

Because Dream Project explicitly cares about Ukrainian users and catalogue context:

- Localisation:
  - The UI should support at least English and Ukrainian, with a clear path to add more languages later.
  - Language settings should be explicit; system language should not silently override the user’s choice.
- Catalogue policy:
  - Sensitive metadata (Russian-linked publishers, Ukrainian origin, Ukrainian localisation) must be handled consistently.
  - Filters and flags around this information should behave predictably and not “disappear” across different pages.
- Ethical positioning:
  - The platform should be transparent about what it shows and why (e.g. why certain games are flagged or filtered).
  - Users who are not interested in this aspect should not be forced into it, but Ukrainian and other interested users must have reliable access to that information.

Non-functionally, this is about **predictability and clarity**: the rules behind what appears in search, filters, and highlights should not feel arbitrary or opaque.

---
<br>

### 11.7 Observability & Analytics (Business View)

To steer the product, the team needs a **clear picture of usage**:

- Basic analytics should show:
  - which features are actually used (saves vs sets vs albums),
  - which games and templates attract activity,
  - how many users complete sets and albums vs abandon them.
- Moderation metrics:
  - volume and type of reports,
  - patterns around problematic content.

Non-functional expectations:

- Analytics must respect privacy (no unnecessary tracking or invasive profiling).
- Aggregated statistics, not per-user spying, should guide decisions.
- The platform should provide enough internal visibility that:
  - issues can be detected (e.g. a broken integration causing all imports to fail),
  - new versions can be evaluated based on real user behaviour.

---
<br>
<br>

## 12. Impact on Stakeholders & Organisation

This section summarises how Dream Project, even at MVP stage, **changes the world** for each key stakeholder group and for the organisation operating the platform.  
It focuses on **practical impact**: new capabilities, responsibilities, and potential concerns.

---
<br>

### 12.1 High-Level Impact Summary

| Stakeholder Group                                   | Key Positive Impact                                             | Key New Responsibilities / Changes                          |
|-----------------------------------------------------|------------------------------------------------------------------|-------------------------------------------------------------|
| Regular Players (Platform Users)                    | Better ways to reuse saves, track goals, and show progress      | Learning a new companion platform and its basic flows       |
| Retro & Emulator-Focused Players                    | Centralised recognition for retro progress and saves            | Linking retro profiles; managing more structured data       |
| Curators & Achievement/Album Set Creators           | Stable home for sets/albums, visibility and feedback            | Designing and maintaining structured content                |
| Global Content Creators & Community Organisers (EN) | Clear references and stats for stories and community events     | Learning how to embed/point to Dream Project objects        |
| Ukrainian Content Creators & Community Organisers   | Tools aligned with Ukrainian context & catalogue policies       | Using localisation/origin flags in their work               |
| Platform Admins & Product Team                      | Concrete data for decisions; structured moderation and catalog  | Operating the platform, moderation, catalogue maintenance   |
| External Services & Integration Partners            | Additional value/use of their data in a niche companion layer   | Monitoring integrations and enforcing their own policies    |

---
<br>

### 12.2 Impact on Players

#### 12.2.1 Regular Players (Platform Users)

**Positive impact**

- Gain a **single place** to:
  - find situation-specific saves (e.g. branch points, post-boss checkpoints),
  - see both official and fan-made completion structures for a game,
  - turn screenshots into structured albums rather than raw image dumps.
- Can selectively use the features they care about (only saves, only albums, only achievements) without committing to a full “lifestyle platform”.

**Changes & responsibilities**

- Need to:
  - create and manage an additional account (Dream Project),
  - learn how to upload/download saves safely,
  - understand the difference between official progress, retro/external data, and fan-made tracking.
- Are gently invited to attach **evidence** (screenshots, saves, clips) if they want more trust and richer profiles, which is extra effort compared to doing nothing.

---
<br>

#### 12.2.2 Retro & Emulator-Focused Players

**Positive impact**

- Receive a **central, neutral place** where retro progress can live alongside modern games.
- Can:
  - import progress from retro achievement services,
  - share and reuse retro saves in a more structured way,
  - represent long retro runs via sticker albums and curated sets.

**Changes & responsibilities**

- Need to:
  - link external retro profiles if they want automatic progress synchronisation,
  - be slightly more disciplined about saves and screenshots (naming, descriptions) to benefit fully.
- May adjust existing habits (forum posts, plain save-sharing) to take advantage of Dream Project’s structure.

---
<br>

### 12.3 Impact on Curators & Content Creators

#### 12.3.1 Curators & Achievement/Album Set Creators

**Positive impact**

- Gain **proper tools** for:
  - defining structured achievement sets and subsets,
  - designing sticker album templates,
  - attaching hints and external resources.
- Their work becomes:
  - more **visible**, via attribution and statistics,
  - more **persistent**, instead of being buried in chats or threads.

**Changes & responsibilities**

- Need to:
  - learn the curator tooling and templates,
  - maintain sets and albums as games are patched or better knowledge appears,
  - respond (at least occasionally) to feedback or reports about their content.
- Take on a semi-formal role within the community, influencing how many players experience a game’s “completion” path.

---
<br>

#### 12.3.2 Global Content Creators & Community Organisers (EN)

**Positive impact**

- Gain **stable, shareable references** for:
  - specific runs (achievement sets + evidence + albums),
  - community challenges (“complete this fan set”),
  - notable players’ profiles.
- Can enrich content (articles, streams, videos) with:
  - verifiable progress links,
  - visual albums that summarise a playthrough.

**Changes & responsibilities**

- Need to:
  - integrate Dream Project links into their existing communication channels,
  - understand at a basic level how sets and albums work so they can explain them.
- May become informal “ambassadors” of Dream Project if it works well for their communities.

---
<br>

#### 12.3.3 Ukrainian Content Creators & Community Organisers

**Positive impact**

- Gain a platform that:
  - explicitly supports Ukrainian language and context,
  - lets them highlight Ukrainian-localised or Ukrainian-origin games,
  - respects catalogue policies around Russian-linked content.
- Can create **Ukrainian-focused sets and albums** and see them treated as first-class content.

**Changes & responsibilities**

- Need to:
  - use and promote localisation/origin filters in their materials,
- May be asked for feedback on how well the platform meets Ukrainian players’ expectations, influencing the roadmap.

---
<br>

### 12.4 Impact on Platform Admins & Product Team

**Positive impact**

- Get a **clear business structure**:
  - stakeholder groups, processes, data model, rules and policies.
- Have access to:
  - structured analytics on what users actually do (which games, sets, albums matter),
  - moderations tools that form a traceable workflow rather than ad hoc actions.
- Can make more grounded decisions about:
  - which integrations are worth maintaining,
  - which catalogue areas deserve deeper curation,
  - which features to extend beyond MVP.

**Changes & responsibilities**

- Take on ongoing **operational duties**:
  - managing curator roles,
  - reviewing reports and sensitive catalogue flags,
  - ensuring policy consistency (especially around Russian-linked and Ukrainian content).
- Need to maintain and evolve:
  - documentation (policies, help content),
  - internal playbooks (e.g. how to handle abuse, how to react if an external API disappears).

Organisationally, this means the project is not “build once and forget”, but an ongoing **service** with continuous BA input and refinement.

---
<br>

### 12.5 Impact on External Services & Integration Partners

**Positive impact**

- Their data (official achievements, retro sets, metadata) becomes part of a **richer narrative**:
  - long-term progress representations,
  - visual albums,
  - curated fan structures.
- This may:
  - improve perceived value of their own platforms,
  - channel more engaged users towards them.

**Changes & responsibilities**

- Need to:
  - ensure that Dream Project’s use of their data remains compliant with terms,
  - possibly handle occasional support or communication about API changes.
- May need to react if:
  - Dream Project reveals edge cases or inconsistencies in their APIs or data.

Dream Project, in turn, must be ready to **adjust quickly** if any partner tightens rules or changes API behaviour.

---
<br>

### 12.6 Organisational Impact & Ways of Working

Internally, adopting Dream Project as a real product (even if initially built for a portfolio) implies:

- A more **structured BA practice**:
  - clear separation between business case, BA document, SRS, and visual artefacts,
  - traceability from stakeholder needs and gaps through to data and rules.
- A **product mindset**:
  - continuous iteration based on user behaviour,
  - decisions grounded in actual usage and feedback rather than assumptions alone.
- A lightweight but real **operational model**:
  - defined responsibilities for catalogue maintenance, moderation, curator management,
  - simple escalation paths when something goes wrong (abuse, data issues, integration failures).

From a business analysis perspective, this section acts as a reminder that Dream Project is not just a set of features, but a **change in how different groups work and interact** around gaming progress.

---
<br>
<br>

## 13. Glossary & Definitions

This glossary explains how key terms are used **within Dream Project** and this Business Analysis Document.  
Where a term has a common industry meaning and a specific Dream Project meaning, the definition below refers to the **Dream Project** usage.

---
<br>

### 13.1 Core Product Terms

- **Dream Project**  
  The web-based platform described in this documentation. It acts as a companion layer around games, focused on saves, achievements, and screenshot-based sticker albums.

- **MVP (Minimum Viable Product)**  
  The first usable version of Dream Project that delivers the three core pillars (save sharing, achievement tracking, sticker albums) in a limited but coherent scope.

- **AS-IS**  
  The current state of the world without Dream Project: how players currently manage saves, achievements, and screenshots using existing tools.

- **TO-BE**  
  The desired future state once Dream Project exists: how players will ideally interact with saves, achievements, and albums through the platform.

---
<br>

### 13.2 Catalogue & Game Terms

- **Game**  
  A distinct title as understood by players (e.g. *Final Fantasy X*, *Silent Hill 2*). In the model, a Game groups all platform-specific variants and related content.

- **Platform Variant**  
  A specific combination of game and platform or edition (e.g. *FFX – PS2*, *FFX – PS4 Remaster*, *Hollow Knight – PC*). Important for save compatibility and platform-specific metadata.

- **Catalogue**  
  The structured list of Games and Platform Variants known to Dream Project, including tags, localisation info, and policy flags.

- **Catalogue Tag**  
  A reusable label applied to catalogue items (e.g. “JRPG”, “retro”, “Ukrainian localisation”). Some tags are purely descriptive; others have policy implications.

- **Localisation Metadata**  
  Information about which languages a game supports (including whether Ukrainian is officially supported).

- **Origin Metadata**  
  Information about where a game comes from (e.g. developer country, publisher country, “Ukrainian-origin game”, “Russian-linked publisher”).

---
<br>

### 13.3 User, Role & Account Terms

- **User Account**  
  The authenticated identity of a person using Dream Project (credentials, settings, status).

- **User Profile**  
  The public-facing part of a user’s presence on the platform: nickname, avatar, visibility settings, high-level stats and badges.

- **Regular User**  
  Default role assigned at registration. Can browse content, upload their own saves/screenshots, track achievements, use albums, and report content.

- **Curator**  
  A user with additional permissions to create and maintain fan-made achievement sets and sticker album templates, and to propose certain catalogue improvements.

- **Platform Administrator (Admin)**  
  A user with elevated permissions to manage roles, moderation, and catalogue policies, including sensitive metadata (e.g. Russian-linked flags).

- **Role Assignment**  
  The relationship between a User Account and a role (Regular User, Curator, Admin), including when that role became active.

- **External Profile Link**  
  A secure internal link between a Dream Project account and an external service account (e.g. Steam, RetroAchievements), used for read-only progress synchronisation. External IDs are not shown to other users.

---
<br>

### 13.4 Progress, Saves & Evidence Terms

- **Save / Save File**  
  A game’s internal representation of progress, typically stored as files, memory card images, or emulator states.

- **Save Entry**  
  Dream Project’s representation of an uploaded save, with metadata such as game, platform variant, description, and visibility (private/shared).

- **Screenshot / Media Asset**  
  An image uploaded by a user, usually a game screenshot. It can be used in sticker albums or as evidence for achievements.

- **Achievement (General)**  
  A discrete goal or condition associated with a game (e.g. “Defeat Boss X”, “Collect all items in area Y”).

- **Official Achievement**  
  An achievement defined and managed by a game platform or developer (e.g. Steam achievements, console trophies).

- **Fan-Made Achievement / Challenge**  
  A custom goal defined by the community (curators), not part of the official platform list, usually grouped into a fan-made set.

- **Achievement Set**  
  A structured group of achievements or challenges for a given game (and optionally platform). Can be official, external retro, or Dream Project–native fan-made.

- **Achievement Item**  
  A single achievement or challenge inside an Achievement Set, with its own name, condition, and optional hints.

- **Retro / External Achievement Set**  
  An Achievement Set imported from an external retro service (e.g. RetroAchievements) and synchronised read-only from the linked external profile.

- **Completion Record**  
  A record representing a user’s progress on a specific Achievement Item (e.g. not started/in progress/completed, timestamps, source of completion).

- **Evidence Link**  
  A link between a Completion Record and supporting evidence (e.g. screenshot, save entry, external clip URL).

---
<br>

### 13.5 Sticker Albums & Visual Progress Terms

- **Sticker Album (Concept)**  
  A conceptual visual collection where each “sticker” corresponds to a specific game moment (boss, area, ending, etc.), filled by screenshots.

- **Sticker Album Template**  
  A curated design for an album for a specific game (and optionally platform), defined by a curator. It describes which stickers/slots exist and what they represent.

- **Sticker Slot Definition**  
  A single slot in a Sticker Album Template, with a hint and detailed description of what the screenshot should show (e.g. a particular boss or scene).

- **Sticker Album Instance**  
  One user’s personal copy of an album template, showing which slots they have filled, completion status, and timestamps.

- **Sticker Slot Fill**  
  A user’s screenshot filling a particular slot in their album instance, including its validation state (accepted, flagged, under review).

- **First Playthrough Friendly Album**  
  An album template type aimed at new players; slot descriptions are written with minimal spoilers and more subtle hints.

- **Post-Completion / Deep Dive Album**  
  An album template type aimed at players who already finished the game; descriptions can be explicit and spoiler-heavy.

---
<br>

### 13.6 Governance, Policy & Integration Terms

- **Content Report**  
  A user-submitted report flagging a save, screenshot, achievement item/set, album template, comment, or profile as problematic (corrupt, misleading, offensive, etc.).

- **Moderation Decision**  
  An admin action taken in response to reports or proactive checks (e.g. keep, edit metadata, hide, delete, warn or restrict user).

- **Catalogue Policy Flag**  
  A specific metadata flag with policy implications (e.g. “Russian-linked publisher”, “Ukrainian-origin game”, “contains Ukrainian localisation”).

- **Russian-Linked Content**  
  Content (usually games) that is associated with Russian developers or publishers according to catalogue policy. Handling and visibility may depend on user locale and settings.

- **Ukrainian-Origin Game**  
  A game that is developed by a Ukrainian studio or strongly associated with Ukraine, used for highlighting and filtering.

- **GDPR-Like Expectations**  
  A shorthand for privacy and data-protection obligations similar to GDPR (clear consent, minimal data, right to access/delete where feasible).

---
<br>

### 13.7 BA & Documentation Terms (Context)

- **Business Case**  
  The document that explains why Dream Project is worth doing at all: objectives, value, high-level scope, risks, and rough effort/cost.

- **Business Analysis Document (this document)**  
  The analysis-focused document that connects business needs, AS-IS/TO-BE, processes, rules, and data into a coherent model for requirements.

- **SRS (System Requirements Specification)**  
  A more technical document (not this one) that will translate business analysis into detailed functional and non-functional requirements for implementation.

- **Business Rule**  
  A constraint or policy that must always hold true in the system (e.g. “external achievements are read-only”, “only admins can approve Russian-linked flags”).

- **Non-Functional Considerations / Non-Functional Requirements**  
  Qualities of the system that are not about specific features but about how it behaves (e.g. performance, security, reliability, usability).

This glossary is intended as a living reference; if new recurring terms appear in future artefacts, they should be added here to keep communication clear for all stakeholders.

---
<br>
<br>