# Apply AI

An AI-powered job application platform that automates the most tedious parts of the job hunt — from finding relevant openings and tailoring your resume, to reaching out for referrals and filling out applications. This repository serves as the central hub for the entire project; each component lives in its own repo and is linked here as a Git submodule.

> **Note**: This is not a monorepo. Each submodule is an independent repository. This repo simply ties them together so you can explore or set up the full system from one place.

---

## Architecture

```
┌───────────────────────────────────────────────────────────────────────┐
│                          Apply AI Platform                           │
│                                                                      │
│  ┌──────────────┐   ┌──────────────┐   ┌──────────────────────────┐  │
│  │   Frontend    │──▶│   Backend    │──▶│  Lambda PDF (LaTeX→PDF)  │  │
│  │  React + Vite │   │   FastAPI    │   │  AWS Lambda container    │  │
│  └──────────────┘   └──────┬───────┘   └──────────────────────────┘  │
│                            │                                         │
│              ┌─────────────┼─────────────┐                           │
│              ▼                           ▼                           │
│  ┌────────────────────┐     ┌────────────────────────┐               │
│  │ LinkedIn Extension │     │  Auto LinkedIn Jobs    │               │
│  │ Chrome MV3 – send  │     │  Scrape + AI score +   │               │
│  │ connect notes       │     │  email digest          │               │
│  └────────────────────┘     └────────────────────────┘               │
└───────────────────────────────────────────────────────────────────────┘
```

---

## Components

| Submodule | Description | Tech |
|---|---|---|
| [`backend`](./backend/) | Async API powering the platform — handles auth, job tracking, JD parsing, resume optimization, referral generation, and task delegation to browser agents. | FastAPI · PostgreSQL · SQLAlchemy 2.0 · Cloudflare R2 |
| [`frontend`](./frontend/) | Web dashboard where users manage jobs, upload resumes, view optimised resumes, and trigger automations. | React · TypeScript · Vite |
| [`lambda-pdf`](./lambda-pdf/) | Serverless LaTeX-to-PDF compiler running on AWS Lambda (container image). Compiles optimised `.tex` resumes into PDFs on demand. Uses a frozen TeXLive snapshot for reproducible builds. | AWS Lambda · Docker · TeXLive |
| [`linkedin_note_extension`](./linkedin_note_extension/) | Chrome Extension (Manifest V3) that automates sending LinkedIn connection requests with personalised referral notes — triggered directly from the web app, no server-side browser needed. | Chrome Extension · Manifest V3 |
| [`auto-linkedin-jobs`](./auto-linkedin-jobs/) | Standalone pipeline that scrapes jobs from LinkedIn, Indeed, and Google Jobs every 4 hours, scores them against your resume using Gemini AI, and emails you a digest of the best matches. Runs fully automated via GitHub Actions. | Python · Gemini AI · Astra DB · GitHub Actions |

---

## How It Works

1. **Find jobs** — `auto-linkedin-jobs` continuously scrapes fresh postings across multiple platforms and emails you the ones that match your profile.
2. **Track & parse** — Add a job URL to the `frontend` dashboard. The `backend` fetches and parses the job description using an LLM.
3. **Optimise your resume** — The backend generates a tailored LaTeX resume for the role and sends it to `lambda-pdf` for compilation into a polished PDF.
4. **Get referrals** — The backend identifies potential referrals at the company. Trigger the `linkedin_note_extension` to send personalised connect notes directly from your browser.
5. **Apply** — The backend delegates form-filling tasks to local Playwright agents via the Tasks API.

---

## Getting Started

### Clone with all submodules

```bash
git clone --recurse-submodules https://github.com/tomar-ayush/apply-ai.git
```

### Already cloned? Pull in the submodules

```bash
git submodule update --init --remote
```

### Update all submodules to latest `main`

```bash
git submodule update --remote
```

> Submodules are configured to track the `main` branch. A GitHub Actions workflow runs every 12 hours to automatically sync the submodule pointers to the latest commits.

---

## Repository Structure

```
apply-ai/
├── backend/                  # FastAPI backend (submodule)
├── frontend/                 # React frontend (submodule)
├── lambda-pdf/               # LaTeX → PDF Lambda (submodule)
├── linkedin_note_extension/  # Chrome extension (submodule)
├── auto-linkedin-jobs/       # Job scraper pipeline (submodule)
├── .github/workflows/
│   └── sync-submodules.yml   # Auto-sync submodules every 12 hours
├── .gitmodules
└── README.md
```

---

## License

MIT
