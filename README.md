# Closer Copilot

Live AI sales whisper + deal memory for a solo closer on Google Meet.

While you're on a call, Closer Copilot listens to both sides, shows a live
two-speaker transcript, and whispers the next line to say — with tone, pauses,
pitch and stress marked. After the call it extracts a **Client Brain** (what the
prospect said, objections, next steps) into Supabase, and before the next call it
briefs you on where you left off.

## Requirements

- **Node.js 20+**
- **Chrome** (tab-audio capture is Chrome-only)
- API keys for **Deepgram** (transcription) and **OpenAI** (coaching)
- A **Supabase** project (auth + data)

## Setup

```bash
# 1. Install dependencies
npm install

# 2. Build the React UI
npm run build:client

# 3. Add your keys
cp .env.example .env      # Windows PowerShell: copy .env.example .env
#    then open .env and fill in the values
```

**Database:** in your Supabase project, open the SQL editor and run
`admin-schema.sql` and `share-schema.sql`. See [ADMIN-SETUP.md](ADMIN-SETUP.md)
for the admin panel and meeting notetaker.

## Run it

```bash
npm start
```

Then open **http://localhost:7801** in Chrome.

## Use it on a call

1. Join your Google Meet in one Chrome tab.
2. Open http://localhost:7801 in another window and click **Start Call**.
3. Allow the microphone, then in the share picker choose the **Meet tab**
   (the "Chrome Tab" option, not a window/screen) and tick **"Also share tab audio"**.
4. **Left panel** = coached lines: `[TONE]` first, `‖` = pause, `↘/↗` = pitch, **bold** = stress.
   **Right panel** = live two-speaker transcript. Transcripts save to `calls/`.

## Test without a real call

- Click the **Test** button (`/simulate`) to rehearse coaching with no audio at all.
- Or play any YouTube sales-call recording in a tab and share *that* tab as the
  "prospect" — then talk back to it yourself.

## Files that shape the coaching

| File | What it does |
|---|---|
| `playbook.md` | Sales-psychology rules (moment → technique map). Edit in plain English. |
| `knowledge/` | Shared reference material, e.g. objection testimonials. |
| `products/` | Example product playbooks and templates. |
| In-app **Playbooks → + Add playbook** | Per-user products/playbooks, built through the Interview and stored in Supabase. |
| `.env` | Keys and model choices (`LIVE_MODEL`, `PREP_MODEL`, `ANALYSIS_MODEL`). |

## Scripts

| Command | What it does |
|---|---|
| `npm start` | Run the app server |
| `npm run build:client` | Build the React UI |
| `npm test` | Run the test suite |
| `npm run check` | Syntax-check the server files and run tests |
| `npm run bot` | Run the meeting-notetaker bot worker (optional) |

## Deploying

See [DEPLOY.md](DEPLOY.md). It needs a Node host that supports WebSockets and
HTTPS (Railway or Render) — not Vercel.

## Roadmap

- [x] Live two-speaker transcript (mic + tab audio → Deepgram nova-3)
- [x] Whisper loop with playbook (one-line cards, delivery marks)
- [x] Post-call Client Brain extraction (Markdown) → Supabase
- [x] Pre-call "where we left off" brief + battle plan
- [x] Deal memory fed into live prompts
- [x] React UI (dashboard of ranked "next moves", Client Brain, Metrics, Settings)
- [x] Metrics (line acceptance, saved deals, close rate)
- [ ] Usage/margin tracking per user, billing, polish for paid launch

## Troubleshooting

- **Login hangs:** free-tier Supabase projects pause after ~7 days idle.
  Open the Supabase dashboard and click **Restore**.
- **`npm run build:client` fails with `rolldown-binding ... not found` on Windows:**
  run `cd client && npm install @rolldown/binding-win32-x64-msvc --no-save`, then build again.
