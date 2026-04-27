# 🎓 EduAI — AI-Powered Teacher Assistant

EduAI is a web application that helps teachers analyze student performance, generate personalized study notes, plan budgets, run live video classes, and track progress — all powered by **Google's Gemini AI** and deployed on modern cloud infrastructure.

---

## ✨ Features

- 📊 **AI Student Analysis** — Upload report cards and get instant AI-generated performance insights
- 📝 **Auto Study Notes** — Generate personalized study notes for each student using Gemini
- 🎯 **Resource Recommendations** — AI suggests learning resources tailored to each student's needs
- 💰 **Budget Planning** — Plan and track educational resource budgets with AI assistance
- 👥 **Intervention Groups** — Identify struggling students and plan group interventions
- 📹 **Live Video Classes** — Schedule and host real-time video sessions with students
- 📈 **Progress Tracking** — Monitor student progress over time with AI-generated checkpoints
- 🧑‍🎓 **Student Portal** — Students can log in, view their notes, resources, and join live classes
- 📥 **Bulk Import** — Import multiple students at once via CSV

---

## 🤖 Google Technologies & Services

### [Google Gemini 2.5 Flash](https://aistudio.google.com) — Core AI Engine
The entire AI brain of EduAI runs on **Gemini 2.5 Flash** via the official `google-genai` Python SDK.

Every AI feature in the app is powered by Gemini:

| Feature | What Gemini Does |
|---|---|
| Report Card Analysis | Reads uploaded PDF report cards and extracts structured performance data |
| Student Analysis | Takes marks, attendance, and assignment scores → generates detailed insights |
| Study Notes | Generates subject-specific, student-personalized revision notes |
| Resource Recommendations | Suggests books, videos, and tools based on student weaknesses |
| Budget Planning | Creates cost-optimized resource procurement plans |
| Group Intervention | Identifies common weaknesses across students and plans group sessions |
| Progress Analysis | Compares checkpoints over time and generates progress narratives |

**Model used:** `gemini-2.5-flash` — chosen for its speed, long context window (handles full report card PDFs), and multimodal capability.

**SDK:** [`google-genai`](https://pypi.org/project/google-genai/) (official Python client)

**Get your API key:** https://aistudio.google.com → API Keys → Create

---

### [Google AI Studio](https://aistudio.google.com) — API Key Management
API keys for Gemini are issued and managed through Google AI Studio. The free tier is generous enough to run EduAI in development and small deployments.

---
For coding help is taken from Google Gemini

## 🧱 Full Tech Stack

### 🤖 AI & Machine Learning

| Technology | Version | Purpose |
|---|---|---|
| **Google Gemini 2.5 Flash** | Latest | Core LLM — powers all AI features (analysis, notes, recommendations, budgets, interventions, progress) |
| **google-genai** (Python SDK) | ≥1.0.0 | Official Google client to call Gemini API |
| **Pinecone** | v3.2.2 | Vector database — stores student performance embeddings for similarity search ("find students like this one") |

### 🐍 Backend

| Technology | Version | Purpose |
|---|---|---|
| **Python** | 3.11 | Primary programming language |
| **Flask** | 3.0.3 | Web framework — handles all routes, requests, and responses |
| **Werkzeug** | 3.0.3 | WSGI utilities, secure file uploads, request handling |
| **Gunicorn** | 22.0.0 | Production WSGI server — runs the Flask app on Render |
| **python-dotenv** | 1.0.1 | Loads environment variables from `.env` file in local dev |
| **requests** | 2.32.3 | HTTP client — used to call Daily.co API for video rooms |

### 🗄️ Database & ORM

| Technology | Version | Purpose |
|---|---|---|
| **PostgreSQL** | Latest | Production database on Render — persists all users, students, reports |
| **SQLite** | Built-in | Local development database (zero setup) |
| **SQLAlchemy** | 2.0.30 | ORM — defines and queries all database models |
| **Flask-SQLAlchemy** | 3.1.1 | SQLAlchemy integration with Flask |
| **psycopg2-binary** | 2.9.9 | PostgreSQL driver for Python |

### 🔐 Authentication & Security

| Technology | Version | Purpose |
|---|---|---|
| **Flask-Login** | 0.6.3 | Session management — keeps teachers and students logged in |
| **Flask-Bcrypt** | 1.0.1 | Password hashing — all passwords stored as bcrypt hashes, never plain text |

### 📄 File & Document Handling

| Technology | Version | Purpose |
|---|---|---|
| **pdfplumber** | 0.11.1 | Extracts text from uploaded report card PDFs |
| **PyPDF2** | 3.0.1 | PDF utilities and fallback parsing |
| **Pillow** | 10.3.0 | Image processing for uploaded files |

### 🎨 Frontend

| Technology | Purpose |
|---|---|
| **Jinja2** | Server-side HTML templating engine built into Flask — renders all 20+ pages dynamically |
| **Custom CSS (Vanilla)** | Fully hand-written dark-theme UI with CSS variables, glassmorphism cards, responsive grid layout — no CSS framework used |
| **Google Fonts** | Typography — **Syne** (headings, bold UI elements) + **DM Sans** (body text), loaded via Google Fonts CDN |
| **Chart.js 4.4.1** | Interactive charts — used for student score trend graphs and progress tracking visualizations (loaded via cdnjs CDN) |
| **HTML5 Canvas API** | Native browser canvas used for custom progress charts in student portal |
| **Daily.co JS SDK** (`@daily-co/daily-js`) | Embeds the live video call iframe directly inside the classroom page — loaded via unpkg CDN |
| **Vanilla JavaScript** | Dynamic form rows (bulk import), chart rendering, video room embedding — no frontend framework needed |

### 📹 Video Conferencing

| Technology | Purpose |
|---|---|
| **Daily.co API** | Creates WebRTC video rooms for live classes — embedded directly in the browser, no app download needed |

### ☁️ Infrastructure & Deployment

| Technology | Purpose |
|---|---|
| **Render** | Cloud hosting — runs the web service and manages the PostgreSQL database |
| **Render PostgreSQL** | Managed, persistent database — survives restarts and deploys |
| **GitHub** | Source control — Render auto-deploys on every push to `main` |

---

## 🚀 Deploy to Render (Recommended)

### Prerequisites
- A GitHub account
- A [Render](https://render.com) account (free)
- A [Google AI Studio](https://aistudio.google.com) API key
- A [Pinecone](https://app.pinecone.io) API key

### Step 1 — Push to GitHub

```bash
git init
git add .
git commit -m "initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/eduai.git
git push -u origin main
```

### Step 2 — Deploy on Render

1. Go to https://dashboard.render.com → **New** → **Blueprint**
2. Connect your GitHub repo
3. Render will detect `render.yaml` and automatically create:
   - A **Web Service** (`eduai`)
   - A **PostgreSQL database** (`eduai-db`) — linked automatically, no copy-pasting needed

### Step 3 — Set environment variables in Render Dashboard

Go to your web service → **Environment** tab and set:

| Key | How to get it |
|---|---|
| `SECRET_KEY` | Run `python -c "import secrets; print(secrets.token_hex(32))"` once and paste the output |
| `GEMINI_API_KEY` | https://aistudio.google.com → API Keys |
| `PINECONE_API_KEY` | https://app.pinecone.io → API Keys |
| `DAILY_API_KEY` | https://dashboard.daily.co → Developers (for video classes) |

> `DATABASE_URL` is wired automatically by Render — you do **not** need to set it manually.

### Step 4 — Done 🎉

Your app will be live at `https://eduai.onrender.com` (or your custom domain).

---

## 💻 Local Development

```bash
# 1. Create and activate virtual environment
python -m venv venv

# Windows
venv\Scripts\activate
# Mac/Linux
source venv/bin/activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create .env file
cp .env.example .env
# Then fill in your API keys in .env

# 4. Run the app
python app.py
```

Visit **http://localhost:5000**

### Local `.env` file

```env
SECRET_KEY=any-random-string-for-local-dev
DATABASE_URL=sqlite:///eduai.db
GEMINI_API_KEY=your-gemini-key-from-aistudio
PINECONE_API_KEY=your-pinecone-key
PINECONE_INDEX_NAME=eduai-students
DAILY_API_KEY=your-daily-key
```

---

## 📹 Video Classes Setup (Daily.co)

EduAI uses [Daily.co](https://daily.co) for embedded in-browser video classes — no app download needed for students.

1. Sign up free at https://dashboard.daily.co
2. Go to **Developers** → copy your API key
3. Set `DAILY_API_KEY` in your environment

**Free tier:** 1,000 participant-minutes/month — plenty for small classrooms.

**How it works:**
- Teacher schedules a class → Daily.co room is auto-created
- Teacher shares the student link (via WhatsApp, email, etc.)
- Students click **Join Now** in their portal → video call opens in the browser

---

## 🔑 Environment Variables Reference

| Variable | Required | Description |
|---|---|---|
| `SECRET_KEY` | ✅ Yes | Flask session signing key — must be fixed and secret |
| `DATABASE_URL` | ✅ Yes | PostgreSQL URL (auto-set on Render) |
| `GEMINI_API_KEY` | ✅ Yes | Google Gemini API key from AI Studio |
| `PINECONE_API_KEY` | ✅ Yes | Pinecone vector DB key |
| `PINECONE_INDEX_NAME` | ✅ Yes | Pinecone index name (default: `eduai-students`) |
| `DAILY_API_KEY` | ⚡ Optional | Daily.co key for video classes |

---

## 📁 Project Structure

```
eduai_new/
├── app.py                  # Main Flask app — all routes
├── ai_service.py           # All Gemini AI logic
├── models/
│   └── models.py           # SQLAlchemy database models
├── templates/              # Jinja2 HTML templates
├── static/
│   └── uploads/            # Uploaded report card PDFs
├── render.yaml             # Render deployment config
├── requirements.txt        # Python dependencies
├── RENDER_DEPLOY.md        # Step-by-step Render guide
└── .env.example            # Environment variable template
```

---
# 🏗️ EduAI — Architecture Overview

EduAI is a Flask-based web application that helps teachers manage student performance using AI-powered analysis, recommendations, and study tools. It supports two user roles — **Teacher** and **Student** — each with their own portal.

---

## 📁 Project Structure

```
eduai_new/
├── app.py                  # Main Flask application — routes & business logic
├── ai_service.py           # All AI & vector DB integrations
├── models/
│   ├── __init__.py
│   └── models.py           # SQLAlchemy ORM models
├── templates/              # Jinja2 HTML templates
│   ├── base.html
│   ├── dashboard.html
│   ├── student_portal.html
│   └── ...                 # Feature-specific templates
├── static/
│   └── uploads/            # Uploaded report card PDFs
├── requirements.txt
├── render.yaml             # Render.com deployment config
├── Dockerfile
└── .env.example
```

---

## 🧩 Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                        Browser                          │
│              (Teacher Portal / Student Portal)          │
└───────────────────────┬─────────────────────────────────┘
                        │ HTTP
                        ▼
┌─────────────────────────────────────────────────────────┐
│              Flask Application  (app.py)                │
│                                                         │
│  ┌────────────┐  ┌──────────────┐  ┌────────────────┐  │
│  │  Auth      │  │  Teacher     │  │  Student       │  │
│  │  Routes    │  │  Routes      │  │  Portal Routes │  │
│  │ /login     │  │ /dashboard   │  │ /portal        │  │
│  │ /register  │  │ /add-student │  │ /portal/classes│  │
│  │ /logout    │  │ /report-card │  │ /portal/notes  │  │
│  └────────────┘  └──────┬───────┘  └────────────────┘  │
│                         │                               │
│              ┌──────────▼───────────┐                   │
│              │    ai_service.py     │                   │
│              │  (AI Orchestration)  │                   │
│              └──────────┬───────────┘                   │
└─────────────────────────┼───────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
  ┌───────────────┐ ┌──────────┐ ┌──────────────┐
  │  Gemini 2.5   │ │Pinecone  │ │  Daily.co    │
  │  Flash (LLM)  │ │(Vectors) │ │  (Video)     │
  └───────────────┘ └──────────┘ └──────────────┘
          
          ┌────────────────────────────┐
          │   PostgreSQL (via Render)  │
          │   or SQLite (local dev)    │
          └────────────────────────────┘
```

---

## 🔑 Core Components

### 1. `app.py` — Flask Application
The single-file Flask app that handles all routing, session management, and orchestration. Key areas:

| Area | Routes |
|---|---|
| Auth | `/login`, `/register`, `/logout` |
| Teacher Dashboard | `/dashboard`, `/add-student`, `/bulk-import` |
| Student Management | `/student/<id>`, `/student/<id>/progress` |
| Report Cards | `/report-card`, `/report/<id>` |
| Resources | `/student/<id>/resources`, `/resource-budget` |
| Intervention | `/intervention-groups` |
| Video Classes | `/classes`, `/classes/schedule`, `/classes/<id>/join` |
| Study Notes | `/student/<id>/study-notes` |
| Student Portal | `/portal`, `/portal/classes`, `/portal/study-notes` |
| APIs | `/api/dashboard-summary`, `/api/validate-login` |

### 2. `ai_service.py` — AI Orchestration Layer
All AI calls are isolated here. It integrates with:

- **Google Gemini 2.5 Flash** — primary LLM for all analysis and generation tasks
- **Pinecone** (vector DB) — stores 3D student performance vectors `[marks, attendance, assignments]` for similarity search

| Function | Purpose |
|---|---|
| `analyze_student()` | Classify student as Weak / Average / Strong with study plan |
| `analyze_report_card()` | Parse and analyse uploaded PDF report cards |
| `recommend_resources()` | Suggest study resources per student's weak areas |
| `recommend_resources_with_feedback()` | Re-recommend resources using prior teacher feedback |
| `plan_resource_budget()` | Allocate teacher attention hours across students |
| `plan_group_intervention()` | Cluster weak students and generate group session plans |
| `generate_study_notes()` | Create AI study notes per subject for a student |
| `analyze_progress()` | Analyse a student's checkpoint history for progress trends |
| `get_similar_students()` | Query Pinecone for students with similar performance vectors |
| `store_student_in_pinecone()` | Upsert a student's performance vector into the index |

All AI functions have a `_fallback_*` counterpart that returns rule-based results if the Gemini API is unavailable.

### 3. `models/models.py` — Database Layer (SQLAlchemy ORM)

| Model | Table | Description |
|---|---|---|
| `User` | `users` | Teacher accounts |
| `StudentUser` | `student_users` | Student login accounts (linked to `Student`) |
| `Student` | `students` | Student records with marks, attendance, AI analysis |
| `ReportCardAnalysis` | `report_analyses` | Uploaded PDF analysis results |
| `ResourceRecommendation` | `resource_recommendations` | AI resource suggestions per student |
| `ResourceBudgetPlan` | `resource_budget_plans` | Teacher time allocation plans |
| `InterventionGroup` | `intervention_groups` | AI-clustered student groups with session plans |
| `ProgressCheckpoint` | `progress_checkpoints` | Re-assessment snapshots over time |
| `VideoClass` | `video_classes` | Scheduled Daily.co video sessions |
| `StudyNote` | `study_notes` | AI-generated study notes per subject |
| `ResourceFeedback` | `resource_feedback` | Teacher feedback on resource effectiveness |

---

## 🔐 Authentication

Two separate user types share a single Flask-Login session using a prefixed ID strategy:

- **Teacher** — `User` model, ID stored as integer (e.g. `42`)
- **Student** — `StudentUser` model, ID stored as `student:42`

The `user_loader` dispatches to the correct model based on the prefix, enabling both portals to coexist without session conflicts.

---

## 🌐 External Services

| Service | Purpose | Env Variable |
|---|---|---|
| **Google Gemini 2.5 Flash** | All LLM inference | `GEMINI_API_KEY` |
| **Pinecone** (Serverless, AWS us-east-1) | Student similarity vectors | `PINECONE_API_KEY`, `PINECONE_INDEX_NAME` |
| **Daily.co** | Video class rooms | `DAILY_API_KEY` |
| **PostgreSQL** | Production database (via Render) | `DATABASE_URL` |

---

## 🗄️ Data Flow — Adding a Student

```
Teacher fills form → POST /add-student
        │
        ▼
  Validate inputs
        │
        ▼
  ai_service.get_similar_students()   ← Pinecone similarity query
        │
        ▼
  ai_service.analyze_student()        ← Gemini 2.5 Flash
        │
        ▼
  parse_analysis() → category, weak_areas, suggestions, study_plan
        │
        ▼
  Save Student to PostgreSQL
        │
        ▼
  ai_service.store_student_in_pinecone()  ← upsert vector
```

---

## 🚀 Deployment

The app is configured for **Render.com** via `render.yaml`:

- **Runtime:** Python 3.11
- **Server:** Gunicorn (1 worker, 8 threads, 120s timeout)
- **Database:** Render-managed PostgreSQL (free tier)
- **Static uploads:** stored in `static/uploads/` (ephemeral on free tier — consider object storage for production)

For local development, the app falls back to **SQLite** (`eduai.db`) when `DATABASE_URL` is not set.

```bash
# Quick local start
pip install -r requirements.txt
cp .env.example .env   # fill in API keys
python app.py
```

---

## ⚙️ Key Design Decisions

- **Single `app.py`** — all routes in one file for simplicity; can be split into Flask Blueprints as the project scales.
- **Fallback AI responses** — every AI function has a rule-based fallback so the app remains usable without API keys.
- **JSON in text columns** — some relational data (e.g. `subject_marks`, `allocations`, `student_ids`) is stored as JSON strings in `Text` columns for flexibility; consider migrating to `JSONB` on PostgreSQL.
- **PDF parsing** — `pdfplumber` is used to extract text from uploaded report cards before passing to the LLM.
- **Markdown rendering** — a custom Jinja2 filter (`markdown`) converts AI-generated markdown to HTML for display.

## ⚠️ Important Notes

- **Never change `SECRET_KEY` after going live** — it will log out all users instantly
- **Use PostgreSQL on Render** — SQLite is wiped on every restart (ephemeral disk)
- **Gemini free tier** has generous limits but may slow under heavy load — upgrade to a paid Google AI plan for production

---

## 📄 License

MIT — free to use, modify, and deploy.
