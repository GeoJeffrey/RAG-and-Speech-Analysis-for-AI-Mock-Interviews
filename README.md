# AI Mock Interview Engine

A full-stack mock-interview practice app. Upload your resume, get interview questions tailored to it, answer by typing or speaking, and receive scored feedback with speech-fluency analytics.

> **Status: working prototype.** The sections below separate what is implemented from what is planned, so nothing here overstates the code.

---

## What works today

- **Resume-aware question generation.** Resumes (PDF and other documents) are parsed to text with `markitdown`, and the text is passed to an LLM (Groq, Llama 3.1) together with the role and job description to generate tailored questions.
- **Interview sessions.** Create a user, start a session, answer questions one at a time, end the session and get overall feedback.
- **Answer scoring.** Each answer gets a score, one-line feedback and a model-answer hint from the LLM.
- **Filler-word and fluency analysis.** Detects filler words in answers (rule-based, compiled regex) and reports counts and rate.
- **Voice in and out.** Speech-to-text and text-to-speech run in the browser (Web Speech API, Chrome or Edge), so no extra API key is needed for voice.
- **Analytics dashboard.** Per-user overview and per-session report with charts.

## Planned, not implemented yet

- **True Retrieval-Augmented Generation.** Right now the resume text is placed directly in the prompt. There is no vector database or embedding search yet. Planned: chunk documents, embed them, store in ChromaDB and retrieve relevant parts per question.
- **Eye tracking / eye-contact scoring.** The database has an `eye_contact_score` column, but there is no eye-tracking code.
- **Long-term learning analytics** beyond the current per-session and per-user summaries.

---

## Tech stack

| Layer | Tools |
|---|---|
| Backend | Python, FastAPI, SQLAlchemy (async), SQLite, Groq API, markitdown |
| Frontend | Next.js 14, React 18, TypeScript, Tailwind CSS, Recharts |
| Voice | Browser Web Speech API (SpeechRecognition and SpeechSynthesis) |

## Project structure

```text
interview-engine/
├── backend/
│   ├── main.py            # FastAPI app, CORS, routers, /health
│   ├── routers/           # interview, upload, analytics, session endpoints
│   ├── services/          # llm.py (questions, scoring, feedback, fillers), parser.py, database.py
│   └── requirements.txt
└── frontend/
    ├── app/               # pages: home, setup, interview, dashboard
    └── lib/               # api client, useVoice (STT), useTTS (TTS)
```

## Run it locally

**Backend**

```bash
cd interview-engine/backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Create `interview-engine/backend/.env`:

```env
GROQ_API_KEY=your-groq-key
FRONTEND_URL=http://localhost:3000
# optional
# DATABASE_URL=sqlite+aiosqlite:///./interview_engine.db
# UPLOAD_DIR=./uploads
```

```bash
uvicorn main:app --reload --port 8000
```

**Frontend**

```bash
cd interview-engine/frontend
npm install
npm run dev
```

Open http://localhost:3000. The frontend talks to http://localhost:8000 unless `NEXT_PUBLIC_API_URL` is set.

## Roadmap

- [ ] Real RAG with embeddings and ChromaDB
- [ ] Automated tests for scoring and filler detection
- [ ] Dockerfile and one-command run
- [ ] Eye-contact analysis
- [ ] Demo video

## License

No license has been chosen yet.
