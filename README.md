# AI Grievance Management System

A full-stack grievance handling platform for Higher Education workflows.

It processes `.eml` emails, classifies grievances, tracks follow-ups, provides dashboard metrics, supports rule-based Q&A, and generates policy-grounded suggested replies using a RAG pipeline.

## Features

- Email ingestion from `.eml` files
- Grievance categorization and follow-up detection
- Excel-based grievance data store (`backend/data/grievances.xlsx`)
- Dashboard and category APIs
- Attachment and original email download APIs
- Rule-based chatbot for quick operational questions
- AI suggested reply generation via RAG (`/generate-reply`)
- React + Vite frontend interface

## Tech Stack

### Backend
- Python, FastAPI, Uvicorn
- Pandas / NumPy / OpenPyXL
- Sentence Transformers + FAISS
- OpenAI API (`gpt-4o-mini`)

### Frontend
- React 18 + Vite
- TypeScript
- Radix UI component ecosystem

## Project Structure

```text
webel123/
├─ backend/
│  ├─ main.py                 # FastAPI app + email processing
│  ├─ chatbot.py              # Rule-based Q&A
│  ├─ reply_generator.py      # RAG-based reply generation
│  ├─ requirements.txt
│  ├─ files/                  # Source .eml files
│  ├─ attachments/            # Extracted attachments
│  └─ data/                   # grievances.xlsx, colleges.json, etc.
├─ frontend/
│  ├─ package.json
│  └─ src/
├─ reply-generator-main/
│  └─ reply-generator-main/data/
│     ├─ faiss.index
│     └─ policy_chunks.json
└─ render.yaml
```

## Prerequisites

- Python 3.10+
- Node.js 18+
- npm
- OpenAI API key (for AI reply generation)

## Local Setup

### 1) Backend setup

```bash
cd backend
pip install -r requirements.txt
```

Create environment file:

```bash
# from backend/
cp .env.example .env
```

Then set:

```env
OPENAI_API_KEY=sk-your-api-key
```

### 2) Frontend setup

```bash
cd frontend
npm install
```

### 3) Run the app

Backend:

```bash
cd backend
python main.py
```

Frontend:

```bash
cd frontend
npm run dev
```

- Backend: `http://localhost:8000`
- API docs: `http://localhost:8000/docs`

## Typical Workflow

1. Put `.eml` files in `backend/files/`
2. Process emails (either):
   - `python backend/main.py process`, or
   - call `POST /process`
3. Open the frontend and browse categories/emails
4. Use **Generate AI Suggested Reply (RAG-Based)** in the reply modal

## Backend API (Core)

- `GET /dashboard` – summary counts
- `GET /categories` – category list with counts
- `GET /emails?category=...` – emails by category
- `GET /colleges` – college database
- `POST /process` – process `.eml` files
- `GET /chat?q=...` – rule-based chatbot answer
- `GET /download/attachment/{filename}` – attachment download
- `GET /download/email/{eml_filename}` – raw `.eml` download
- `POST /generate-reply` – RAG-based suggested reply

Example request for AI reply:

```json
{
  "email_content": "...",
  "email_subject": "Suspension case update",
  "sender": "principal@college.edu"
}
```

## Deployment

Render configuration is included via `render.yaml`.

See:
- `RENDER_DEPLOYMENT.md`
- `DEPLOYMENT_CHECKLIST.md`

## Documentation

- `QUICK_START.md`
- `RAG_SETUP_GUIDE.md`
- `RAG_ARCHITECTURE.md`
- `INTEGRATION_SUMMARY.md`
- `backend/RAG_INTEGRATION.md`

## Notes

- RAG reply generation requires both:
  - `OPENAI_API_KEY` in `backend/.env`
  - policy index files in `reply-generator-main/reply-generator-main/data/`
- If RAG retrieval fails, the system falls back to template-style reply output.

## License

No license file is currently defined in this repository.
