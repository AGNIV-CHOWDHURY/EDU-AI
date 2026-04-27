# Deploying EduAI on Render — Step-by-Step

## Why users were getting logged out / "invalid email"

Two bugs caused this:

1. **SQLite on ephemeral disk** — Render's free tier wipes the filesystem on every
   restart or deploy. All registered users were deleted, so previously valid emails
   showed as "invalid". Fixed by using Render's PostgreSQL (persistent).

2. **No fixed SECRET_KEY** — Flask uses this to sign session cookies. If it isn't
   set as a permanent env variable, sessions break on every restart. Fixed by
   requiring you to set a fixed value once (instructions below).

Both are now fixed in the code. You just need to do the one-time setup below.

---

## One-Time Setup (do this before your first deploy)

### Step 1 — Generate a SECRET_KEY

Run this once on your computer (requires Python):

```bash
python -c "import secrets; print(secrets.token_hex(32))"
```

Copy the output. You'll need it in Step 3.

### Step 2 — Push your code to GitHub

Push this project to a GitHub repository (public or private).

### Step 3 — Create a new Render project

1. Go to https://dashboard.render.com → **New** → **Blueprint**
2. Connect your GitHub repo
3. Render will detect `render.yaml` automatically
4. It will create:
   - A **Web Service** named `eduai`
   - A **PostgreSQL database** named `eduai-db`
   - `DATABASE_URL` is wired automatically — you don't need to touch it

### Step 4 — Set SECRET_KEY in the Render Dashboard

1. Go to your `eduai` web service → **Environment** tab
2. Find `SECRET_KEY` → click **Edit**
3. Paste the value you generated in Step 1
4. Click **Save Changes** — Render will redeploy automatically

### Step 5 — Set your other API keys

In the same **Environment** tab, set:

| Key | Value |
|-----|-------|
| `GEMINI_API_KEY` | Your Google Gemini API key |
| `PINECONE_API_KEY` | Your Pinecone API key |
| `DAILY_API_KEY` | Your Daily.co API key (if using video) |

### Step 6 — Done!

Your app is live. Users can register and their accounts will persist forever
across restarts and deploys.

---

## Important: Never change SECRET_KEY after going live

If you ever regenerate or change `SECRET_KEY`, all logged-in users will be
immediately logged out (their session cookies become invalid). Only change it
if you believe the key has been compromised.
