
# gogen - Implementation Plan

This document outlines the step-by-step roadmap for building **gogen**, an open-source, self-hosted, automated language learning ecosystem. This plan is designed to be highly structured, modular, maintainable, and testable from day one.

---

## 📂 Project Architecture & Directory Layout

To keep the project clean, we will structure gogen as a monorepo containing the following layout:

```text
/workspaces/gogen/
├── compose.yaml              # Docker Compose for PostgreSQL, pgAdmin, and Ollama
├── docs/                     # Product specs, PRD, and design docs
├── plan.md                   # This implementation roadmap
├── backend/                  # FastAPI Core Backend & NLP Engine (gogen-core)
│   ├── app/
│   │   ├── api/              # API Endpoints (v1)
│   │   ├── core/             # Config, security, auth, database sessions
│   │   ├── models/           # SQLAlchemy or Prisma models / schemas
│   │   ├── services/         # Business logic (SRS, Dictionary, NLP, Mining)
│   │   └── main.py           # FastAPI Application entry point
│   ├── prisma/               # Prisma Schema & migrations
│   ├── tests/                # Comprehensive unit/integration tests
│   ├── pyproject.toml        # Backend dependencies & configuration
│   └── README.md
└── frontend/                 # React + Vite + Tailwind PWA (gogen-app)
    ├── src/
    │   ├── components/       # Reusable UI components (Bottom sheets, cards, layout)
    │   ├── hooks/            # Custom React hooks (auth, SRS state, PWA install)
    │   ├── pages/            # View pages (Login, Dashboard, Review, Settings, Wizard)
    │   ├── services/         # API integration client
    │   ├── utils/            # Helper functions for Japanese parsing/rendering
    │   ├── App.tsx
    │   └── main.tsx
    ├── tailwind.config.js
    ├── vite.config.ts        # PWA plugin configuration
    ├── package.json
    └── README.md
```

---

## 🗓️ Phase-by-Phase Roadmap

### Phase 1: Environment Scaffolding & Initial Setup
Initialize the container environment, database, and setup standard boilerplate for backend and frontend.

- [ ] **Step 1.1: Docker Compose Configuration (`compose.yaml`)**
  - Setup a PostgreSQL 15 database service.
  - Setup PGAdmin for easier database inspection during development.
  - Setup an Ollama service (initially commented out or optional) for local fallback LLM execution.
- [ ] **Step 1.2: Backend Monorepo Setup**
  - Initialize the `backend/` directory using Python 3.11+.
  - Setup `pyproject.toml` (Poetry or standard packaging tools) with essential dependencies:
    - FastAPI, Uvicorn, Prisma Python CLI, Passlib (bcrypt), PyJWT, SudachiPy, spaCy/GiNZA, Pytest.
- [ ] **Step 1.3: Frontend Monorepo Setup**
  - Initialize the `frontend/` directory with Vite, React, TypeScript, and Tailwind CSS.
  - Add basic PWA template setup with `vite-plugin-pwa` for offline assets and mobile optimization.
- [ ] **Step 1.4: Scaffolding Verification**
  - Write a health check endpoint `/api/health` in FastAPI.
  - Verify that both the backend server and frontend development server run smoothly.

---

### Phase 2: Database Schema & Prisma Configuration
Design and establish relational constraints reflecting vocabulary, kanji, sentences, reviews, and content analysis.

- [ ] **Step 2.1: Design `prisma/schema.prisma`**
  - **User**: ID, email, hashed_password, is_admin, created_at.
  - **Word**: ID, surface (written form), reading (kana), dictionary_form, part_of_speech, strength (float/int representing retention score), last_reviewed, next_review, times_reviewed, is_known.
  - **KanjiProgress**: Character, level, show_furigana (Boolean).
  - **Sentence**: ID, raw_text, parsed_tokens (JSON representation of tokens & part-of-speech relationships), source_media_id (optional).
  - **Media**: ID, title, file_path, category (Anime, Book, etc.), parsed_token_frequency (JSON), unique_token_count, user_readiness_score (float).
  - **ReviewLog**: ID, word_id, sentence_id, revealed_furigana (Boolean), revealed_definition (Boolean), timestamp, rating (success/fail).
- [ ] **Step 2.2: Prisma Client Generation & Database Migrations**
  - Run initial database migration via Prisma.
  - Generate the Python Prisma Client and verify DB connections via standard integration tests.

---

### Phase 3: Morphological NLP & Cure Dolly Parsing Engine
Implement Japanese-specific lexical processing utilizing SudachiPy and spaCy.

- [ ] **Step 3.1: Tokenization Service**
  - Setup SudachiPy with `Mode A` split mode.
  - Develop tokenization methods to dissect sentences and produce clean JSON structures detailing:
    - Base Form / Dictionary Form
    - Surface Form
    - Reading (Kana)
    - Part of Speech (POS)
- [ ] **Step 3.2: "Cure Dolly" Conjugation Breakdown**
  - Implement parsing heuristics that break complex conjugations (e.g., 食べてみる) into:
    - Root verb (食べる)
    - Conjugation suffix/particle (て)
    - Auxiliary/Helper verb (みる)
- [ ] **Step 3.3: NLP Engine Unit Testing**
  - Write robust test cases with specific Japanese sentences (polite, casual, compound verbs) to verify token, reading, and structural root breakdown accuracy.

---

### Phase 4: Backend API Services
Build authentication, onboarding, dictionary, and the SRS review algorithms.

- [ ] **Step 4.1: Authentication & Admin CLI**
  - Secure FastAPI JWT auth endpoints.
  - Develop a CLI script/utility to reset or generate an admin user's password (`tools/cli_reset_password.py`).
- [ ] **Step 4.2: Vocabulary Ingestion & Setup Wizard APIs**
  - Implement Anki `.apkg` or text/JSON parser to bulk-import user vocabulary.
  - Develop an interactive "Quiz Mode" API to dynamically test user comprehension of high-frequency words and bootstrap the database with estimated word strengths.
- [ ] **Step 4.3: Dictionary Management & Imports**
  - Build a dictionary service capable of parsing open formats (such as JMdict/Yomichan JSON files).
  - Include an API endpoint to download and index a starter dictionary (e.g., JMdict) legally.
- [ ] **Step 4.4: SRS Review Engine & Diagnostics**
  - Implement standard SRS scheduling (Interval/Factor system inspired by SM2, but modified to track implicit sentence reviews).
  - Calculate word requirements inside sentences.
  - Implement the **"Anti-Cheat" Diagnostic Mode**:
    - If user claims to know an n+1 target word but revealed context-word definitions, queue simplified diagnostic sentences containing only the surrounding n-words over the next 3 review slots to verify background retention.

---

### Phase 5: Frontend Interface (The PWA App)
Develop the user experience and PWA features.

- [ ] **Step 5.1: App Layout & Authentication**
  - Create standard dashboard layout with mobile responsiveness.
  - Implement onboarding / setup wizard screens (import Anki deck, download dictionary, initial vocabulary quiz).
- [ ] **Step 5.2: "Tap-to-Reveal" Review Screen**
  - Render Japanese sentences with customizable Furigana toggling based on individual word or global preference.
  - Add tap interaction states:
    - **Tap 1**: Show Furigana.
    - **Tap 2**: Show English dictionary definitions and "Cure Dolly" structural stacked visual explanations.
  - Handle implicit SRS responses via "Next" button (promoting unmodified words, scheduling review adjustments for revealed words).
- [ ] **Step 5.3: Reports, Statistics & PWA Config**
  - Design visual charts displaying:
    - Estimated fluency level (percentage of standard frequency lists matched).
    - Media readiness scoring list.
  - Activate full PWA capabilities including browser installation prompt and push notifications.

---

### Phase 6: Mining, Automation & Local Fallback Generation
Connect real-world media assets and automate sentence enrichment.

- [ ] **Step 6.1: Subtitle & SRT Mining Pipeline**
  - Build a background worker/service that scans designated directories for `.srt` or `.ass` subtitle tracks.
  - Parse subtitle segments, run NLP tokenization, cross-reference words against known vocabulary databases, and store them as high-quality candidate review sentences.
- [ ] **Step 6.2: Ollama Local Fallback LLM Generator**
  - Implement LLM prompt engine to generate clean *n+1* sentence options if the local mined database is missing examples for a specific target vocabulary word.
  - Verify that fallback generation adheres to strict grammatical schemas without hallucinating unknown structures.
- [ ] **Step 6.3: Database Backup & Recovery Utilities**
  - Implement simple backup scripts/endpoints to archive database states (vocabulary strengths and logs) into compact JSON formats downloadable directly on local machines.

---

### Phase 7: Deployment Scripts & Final Integration Testing
Package gogen for seamless self-hosting.

- [ ] **Step 7.1: Proxmox LXC Installer & Deployment Scripts**
  - Write a shell script `install.sh` to scaffold and run the stack automatically on Debian/Ubuntu systems.
  - Populate `README.md` with foolproof setup instructions.
- [ ] **Step 7.2: End-to-End Test Suite**
  - Build automated integration tests checking a full review cycle: Login -> Load Reviews -> Perform Implicit Next -> Confirm Database Strengths Updated.

---

## 🧪 Testing & Verification Strategy

- **Backend Unit Testing**: Execute `pytest` inside the `backend` directory.
- **Frontend Component/Unit Testing**: Set up Vitest + React Testing Library for verifying interactions (such as Tap-to-Reveal and the bottom sheet).
- **Linter & Type Checking**:
  - Run Python checks: `black .`, `ruff check .`, and `mypy .`
  - Run Frontend checks: `npm run lint` and `tsc --noEmit`.
