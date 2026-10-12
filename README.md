# Canvas Co-Pilot

Canvas Co-Pilot turns Canvas from a passive course portal into an active academic support system. Instead of forcing students to constantly check for changes, interpret grades, read every file manually, and decide what to study next, the platform monitors course state, understands what changed, and generates useful learning support automatically.

## Demo

- Demo video: https://youtu.be/w_5ZtJjuY3o

## Problem

Students spend too much time managing coursework before they even start learning.

Pain points we targeted:

- checking Canvas repeatedly for new content
- reading long PDFs and slides end to end
- understanding why grades dropped
- figuring out what to study next
- converting raw course material into something actually useful for revision

Canvas Co-Pilot reduces that overhead by turning course updates into guided study actions.

## Solution

Canvas Co-Pilot is a multi-agent study assistant built around Canvas LMS data.

It can:

- detect newly posted course content
- summarize materials in a more learnable format
- generate flashcards and quizzes
- create structured study plans
- generate lesson and video-style learning flows
- react to performance signals such as weak grades
- support both autonomous workflows and manual student requests

The result is a system that moves students from "something changed" to "here is exactly what to do next."

## Key Features

- Autonomous mode that watches Canvas state and decides when help is needed
- Manual mode where students can directly generate quizzes, flashcards, study plans, and lesson flows
- Multi-agent workflow with specialized agents for parsing, summarization, quizzes, flashcards, video planning, and intervention
- Personalized outputs shaped by course content and student preferences
- Audio-backed lesson experience for guided study playback

## Tech Stack

### Frontend

- React 19
- Vite
- Framer Motion
- CSS

### Backend

- Node.js
- Express
- dotenv

### AI and Agent Orchestration

- LangGraph
- LangChain Core
- Gemini API
- OpenAI-compatible API support

### Data and Storage

- SQLite
- JSON file storage for saved artifacts

### External Integrations

- Canvas LMS API
- ElevenLabs for lesson narration audio
- Pexels for visual/media support

## How It Works

Canvas Co-Pilot supports two operating modes.

### Autonomous Mode

The system acts like a monitoring and intervention layer:

1. fetch Canvas state
2. detect meaningful changes
3. decide which workflow should run
4. coordinate multiple agents
5. return learning artifacts to the UI

This mode is designed for continuous support without requiring the student to manually orchestrate every step.

### Manual Mode

Students can directly open courses, modules, files, and workflows and request outputs such as:

- flashcards
- quizzes
- study plans
- lesson and video flows

This mode gives students direct control while still using the same backend intelligence.

## Architecture

The project is a full-stack monorepo:

```text
frontend/   React + Vite student interface
backend/    Express API, Canvas integration, LangGraph orchestration, storage
```

### Frontend

The frontend provides the student-facing workspace, including:

- autonomous workflow dashboard
- study plan workspace
- quiz workspace
- manual student interaction workspace
- lesson player experience

Important files:

- [frontend/src/App.jsx](frontend/src/App.jsx)
- [frontend/src/manual/ManualStudentInteractionView.jsx](frontend/src/manual/ManualStudentInteractionView.jsx)
- [frontend/src/study-plan/StudyPlanWorkspace.jsx](frontend/src/study-plan/StudyPlanWorkspace.jsx)
- [frontend/src/quiz/QuizWorkspace.jsx](frontend/src/quiz/QuizWorkspace.jsx)

### Backend

The backend coordinates Canvas, workflow state, AI services, and artifact generation.

Main responsibilities:

- authenticate students with a Canvas Personal Access Token
- fetch and normalize Canvas courses, modules, files, assignments, and messages
- store runtime data and user preferences
- run AI-powered workflows
- coordinate LangGraph-based agent execution
- generate lesson audio and video-related outputs

Important files:

- [backend/server.js](backend/server.js)
- [backend/lib/db.js](backend/lib/db.js)
- [backend/lib/langgraph-runtime.js](backend/lib/langgraph-runtime.js)

## Agentic Workflow

The agent system is built around a central orchestrator and shared runtime state.

High-level flow:

1. Canvas state is collected and normalized.
2. A change-detection layer determines whether something meaningful happened.
3. The orchestrator selects which agents should run.
4. Agents exchange outputs through shared workflow state.
5. The frontend receives a coherent result instead of disconnected tool calls.

Main orchestration files:

- [backend/agents/agentic-workflow/agent-6-orchestrator.js](backend/agents/agentic-workflow/agent-6-orchestrator.js)
- [backend/agents/agentic-workflow/index.js](backend/agents/agentic-workflow/index.js)
- [backend/agents/agentic-workflow/contracts/runtime-state-contract.js](backend/agents/agentic-workflow/contracts/runtime-state-contract.js)

### Agent Roles

#### Agent 1: Parse and Research

- reads selected topic or module content
- grounds the workflow in source material
- prepares the content package for downstream agents

#### Agent 2: Summarize by Preference

- adapts explanations to student preferences
- converts raw source material into a more learnable format

#### Agent 3: Create Flashcards

- transforms summary content into recall-oriented study cards

#### Agent 4: Create Quizzes

- generates checks-for-understanding from the learning package

#### Agent 5: Create Video Plan

- prepares a lesson or video-oriented explanation structure
- organizes examples, scenarios, and teaching cues

#### Agent 6: Create Study Plan

- turns deadlines, scope, and learning needs into sessions and milestones

#### Agent 7: State Change Decider

- detects important Canvas events
- decides what workflow should run next
- acts as the policy layer for autonomous behavior

#### Agent 8: Grade Intervention

- reasons about poor performance
- identifies recovery direction
- triggers reinforcement and study-plan support

For a deeper breakdown, see:

- [backend/agents/agentic-workflow/README.md](backend/agents/agentic-workflow/README.md)

## Why This Is Hackathon-Worthy

- It reframes LMS platforms from passive dashboards into active support systems.
- It combines real educational workflow pain points with agent-based decision making.
- It goes beyond summarization and builds actionable outputs such as quizzes, plans, and guided lessons.
- It blends automation with student control instead of forcing a one-mode experience.

## What We Built During The Hackathon

- Canvas course and content ingestion
- manual and autonomous workflow modes
- multi-agent orchestration for study support generation
- quiz, flashcard, study-plan, and lesson generation
- audio-backed lesson playback experience
- backend persistence for saved artifacts and preferences

## Challenges We Ran Into

- integrating with Canvas using a practical authentication flow without institutional OAuth setup
- coordinating multiple agents while keeping outputs structured and state-aware
- converting raw course data into workflows that feel genuinely useful to students
- generating and playing lesson audio reliably inside browser autoplay restrictions
- balancing autonomous behavior with manual user control

## Future Work

- institutional OAuth instead of Canvas Personal Access Token login
- stronger personalization based on long-term student learning history
- push notifications and proactive reminders for high-priority course changes
- broader autonomous intervention workflows for grades, deadlines, and missing work
- richer lesson generation with stronger visuals and more adaptive pacing
- mobile-first delivery experience

## Authentication

The project currently uses a Canvas Personal Access Token for authentication.

That means:

- a student logs in with a Canvas token through the UI
- the backend uses that token to fetch Canvas data

This approach was used because full institutional SSO and OAuth approval typically requires admin-level setup.

## Storage

The backend currently uses a mix of:

- SQLite for runtime and preference data
- JSON files for some saved study-plan and quiz state
- generated asset directories for lesson audio and video outputs

Important files:

- [backend/data/copilot.db](backend/data/copilot.db)
- [backend/data/study-plans.json](backend/data/study-plans.json)
- [backend/data/quizzes.json](backend/data/quizzes.json)

## Local Setup

### 1. Install dependencies

From the repo root:

```bash
npm run install:backend
npm run install:frontend
```

### 2. Configure environment variables

Create `backend/.env` with values like:

```env
CANVAS_BASE_URL=https://canvas.asu.edu/api/v1
OPENAI_API_KEY=your_openai_api_key
GEMINI_API_KEY=your_gemini_api_key
ELEVENLABS_API_KEY=your_elevenlabs_api_key
FRONTEND_URL=http://localhost:5173
BACKEND_URL=http://localhost:3001
```

Optional variables can include:

- `ELEVENLABS_VOICE_ID`
- `ELEVENLABS_MODEL_ID`
- `PEXELS_API_KEY`
- `HOST`
- `PORT`

You do not need to hardcode a Canvas token into `.env`. The student provides it during login.

## How To Run

Use two terminals from the repo root.

### Terminal 1: backend

```bash
npm run dev:backend
```

### Terminal 2: frontend

```bash
npm run dev:frontend
```

App URLs:

- Frontend: `http://localhost:5173`
- Backend API: `http://127.0.0.1:3001`

## Production Build Commands

From the repo root:

```bash
npm run build
npm run lint
```

## Team

- Niharika Ravilla
- Ram Reddy
- Suraj Shinde 

## Summary

Canvas Co-Pilot is not just a Canvas dashboard. It is a multi-agent student support system that watches course activity, understands changes, decides what action is needed, and generates learning support automatically while still allowing manual control when students want it.
