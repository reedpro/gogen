PRODUCT REQUIREMENT DOCUMENT (PRD)
Project Name: gogen

Target Language: Japanese-first, other languages later

Architecture Paradigm: AI-First Development, Self-Hosted Local-First, Modular and Plugin-friendly

Deployment Environment: Proxmox LXC (Debian/Ubuntu Linux)
1. Executive Summary & Philosophy

gogen is an open-source, self-hosted, automated language learning ecosystem that eliminates the traditional friction of flashcard creation (Anki) by turning authentic native immersion into a dynamic, context-aware reading platform.
Core Pillars:

    Krashen's Input Hypothesis (n+1): Users are only served sentences where they know every element except exactly one new word or grammar structure.

    Implicit SRS (Spaced Repetition): Instead of pass/fail grading, learning progress is tracked implicitly via reading interactions, reducing "button-mashing" fatigue.

    Authentic Media Mining: Text is extracted directly from user media (Plex, subtitle files, scraped scripts) to maintain real-world contextual grounding.

    Cure Dolly Grammar Mechanics: Sentences are broken down into logical structural components (root forms + auxiliary attachments) rather than arbitrary textbook rules.

2. Tech Stack (AI-Optimized)

To maximize the efficacy of AI coding assistants (e.g., Cursor, Claude), the tech stack relies on highly typed, strictly structured, and widely documented frameworks.

    Backend: FastAPI (Python 3.11+). Handles type hints natively, offering optimal compatibility with Python's dominant Japanese NLP libraries.

    Database & ORM: PostgreSQL + Prisma ORM. Prisma definitions provide explicit relational constraints that AI models can generate queries for with near-zero syntax errors.

    Frontend: Vite + React + TypeScript + Tailwind CSS. Configured as a Progressive Web App (PWA) using vite-plugin-pwa for mobile access and push notifications.

    NLP Engine: SudachiPy (for multi-mode morphological tokenization) and GiNZA/spaCy (for dependency parsing).

    Local AI (Fallback Generator): Ollama running Llama 3 or Qwen3-14B hosted on the user's local Bazzite gaming GPU via API call.

gogen is a monorepo with core apps and plugins. The core apps, gogen-core and gogen-app are described here. Some plugins will be included in this repo, but it must be possible to install plugins 
from other repos, to enable user customization, and to prevent them from being required to run features that don't work for them, such as a plex tool when they don't use plex.

3. Functional Requirements

Flow 1: Install gogen

    Purpose: install local gogen stack. Once installed, gogen should be self-updating and managed through it's front end.

    Requirements:
        - Install script should be part of repo
        - Instructions must be outlined in the README
        - Admin user must be created securely during install
            - Admin user will be used to eventually create other accounts.
              This will allow other household members or friends to access gogen.
              However, if a gogen install is open to the web (NGINX), we don't want strangers creating accounts
        
    Considerations:
        - gogen should be ultra-versitile, and can be installed in multiple places
            - this can include PC, docker and proxmox. Proxmox will be the initial focus

Flow 2: Access gogen

    Purpose: User needs to access gogen app to use

    Requirements:
        - After install, should show user link to access gogen from browser
        - gogen shouldn't depend on certain domain. Accessing direct via local IP and remote using reverse proxy will both work
        - gogen will require user to log in with credentials
        - gogen will remember user's session
        - gogen will not allow unauthenticated access

Flow 3: Add User vocabulary

    Purpose: Build initial DB of user words + grammar

    Requirements:
        - Should be done through friendly wizard
        - Should support multiple methods
            - Import from anki (initial method, build more later)
            - Quiz mode (quiz user and estimate words based on level)
        - Added words should include these attributes, at least:
            - strength, how well the user knows the word
            - last review (if we have from anki or other tool)
            - next review (from anki)
            - times reviewed - important, since if we "guess" vocabulary, we should know to remove words if user fails

Flow 4: Add word list

    Purpose: Using a frequency list, we can understand what words user is missing, and teach them new words in an ideal order

    Requirements:
        - Should be flexible in format
        - Should suggest frequency lists to user (core2k, for example)

Flow 5: Install dictionary/dictionaries

    Purpose: To keep repo small, avoid stealing, and allow multiple languages, user should download a dictionary legally from a reputable source

    Requirements:
        - Should allow common dictionary formats
        - Should suggest common dictionaries
        - (maybe) Should support multiple dictionary types (Perhaps we would suggest monolingual dictionaries at a certain point?)

Flow 6: Forgot password

    Purpose: User should not worry about losing password, it should be possible to reset through CLI

    Requirements:
        - Should allow user to reset password through CLI
        - Should be as secure as possible

Flow 7: Backup

    Purpose: User should be able to backup their progress

    Requirements:
        - Only backup what is needed to restore
        - Should allow backup to save to local device, not on server
        - (eventually) Should allow automatic backups, and sync to cloud

Flow 8: Recovery

    Purpose: User should be able to restore from backup if the server is reset or replaced

    Requirements:
        - Allow backup from file
        - Should be possible from CLI or from setup wizard
        - Should be robust, and backwards compatible

Flow 9: Save PWA

    Purpose: User should be able to save as PWA for ease of use, offline capabilities, and notifications

    Requirements:
        - Should prompt PWA install

Flow 10: Review

    Purpose: User should be able to do daily reviews

    Requirements:
        - Enable nag notifications
        - Should use "tap-to-reveal" learning
            - Rather than self-scoring, we can present a sentence in the target language. 
                - If the user understands it, they click "next" and the app intuits they understand
                - The user can tap words they don't know. For Japanese, first tap reveals furigana, second reveals english definition
            - By keeping track of what words were tapped, the tool can decide if the user "learned" a word
            - This is more efficient as well, because we can keep track of more than one word at a time
        - During review, progress should dynamically adjust, following typical SRS 
        - Don't use static review sentences, instead pull from library

    Reference: See `review-system.md`

Flow 11: Reports

    Purpose: User should get a sense of progress

    Requirements:
        - See how many words user knows
        - See percent progress
        - Using word frequency, estimate fluency level

Flow 12: Grammar

    Purpose: When reading sentence, user won't know grammar

    Requirements:
        - Instead of tap-to-reveal, user can also choose "explain grammar"
        - Should also be tiered, could break down sentence by word type before doing full explanation
            - For Japanese, should use Cure Dolly's carriage system (Do we need plugins for this?)
        - Last step would be full translation

Flow 13: Mining

    Purpose: Need simple way to mine content for sentences

    Requirements:
        - Ability for 'automining'
            - Might be a plugin thing, one example would be mining from plex
        - Support combinations of text, images and audio
        - Should break down sentence
            - Grammar points
            - Each word's importance to understanding full sentence or other words
                - Example, in "The old car was fast", knowing the word "old" wouldn't help you learn "fast"

Flow 14: Content reccomendation

    Purpose: Language is learned through immersion. Give user homework without them having to think too much about it

    Requirements:
        - Should be able to find content from multiple sources
        - Should be able to use something like subtitles to see what percent user understands
        - Should reccomend ideal content for learning



Module 1: The Tap-to-Reveal Reading Interface (UI/UX)

    Behavior: Sentences are presented entirely in native Japanese.

    Furigana Management: Based on UserKanjiProgress.showFurigana, known Kanji have their Furigana hidden by default. Hitting a toggle globally overrides this.

    Interaction States:

        Tap 1 on Word: Reveals Furigana above the selected word.

        Tap 2 on Word: Opens a bottom sheet showing the English dictionary definition and its morphological breakdown (e.g., identifying its root verb).

        Next Button: Hitting "Next" without revealing any hints implicitly boosts the confidence scores of all words in that sentence. Hitting "Next" after interacting with a word flags that specific word for a confidence drop/review queue placement.

Module 2: The Core Content Sourcing Pipeline

When a user requires practice with a targeted word or grammar pattern, the system runs an automated hierarchy check to avoid AI hallucinations:

[Target Ingestion Request]
  └── Step 1: Query Local Mined Library (Plex extracted subtitles / Uploaded SRTs)
  └── Step 2: (Fallback) Web Scrape Verified Sentences (Massif.la, Tatoeba API)
  └── Step 3: (Last Resort) Query Local Ollama LLM to generate an n+1 context sentence

    Plex/Subtitle Processing: A nightly background cron job watches designated directories on the Proxmox file system. New .srt or .ass text tracks matching recently watched content hooks are tokenized via SudachiPy and appended to the Sentence database.

Module 3: Multi-Context Diagnostics (The "Anti-Cheat" Engine)

To prevent incorrect manual or implicit reporting:

    If a sentence contains a target n+1 word and the user interacts with definitions, the system isolates the target word from the background context (n).

    If the user struggles with the sentence but claims to know the target word, the system serves diagnostic sentences over the next 3 review positions containing only the surrounding context words inside simplified syntactic frames. This programmatically detects if the base n layer has eroded.

Module 4: "Cure Dolly" Syntactic Parsing

    Processing Rule: Instead of identifying compound phrases like 食べてみる (tabetemiru) as a single block conjugation, SudachiPy (Mode A) splits the phrase into its baseline structural roots: 食べ (Verb Stem) + て (Conjunctive Particle) + みる (Auxiliary Helper Verb).

    Grammar View: When a user requests a structural breakdown, the interface renders a visual stack explaining how the components interconnect logically ("To eat... and see how it goes").

Module 5: Media Readiness Scoring Engine

    Logic: The system aggregates all sentence elements associated with specific media files (e.g., individual anime episodes or movie scripts).

    Output: Generates a real-time sorting index showing the user exactly what percentage of unique tokens they master in that media instance. Shows containing high clusters of n+1 patterns are prominently recommended on the main dashboard interface.

4. Security & Mobile Deployment

    Authentication: Single-user JSON Web Token (JWT) workflow managed via FastAPI. Password encryption handled via passlib[bcrypt].

    Networking: The application exposes standard web ports behind an Nginx Proxy Manager instance or Cloudflare Tunnel running in a parallel Proxmox LXC container, routing traffic under SSL (HTTPS) to enable secure remote PWA installations on iOS and Android.