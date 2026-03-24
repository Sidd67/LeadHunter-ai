# LeadHunter AI

> AI agent that automatically finds company leads from the web.

## Project Overview

LeadHunter AI is a full-stack web application that uses an AI-powered agent to automatically discover business leads from the internet. You specify an industry and location, and the agent:

1. Searches the web for matching companies
2. Opens each company website
3. Extracts: company name, founder name, email, and LinkedIn profile
4. Saves all results to a database

You can then browse all collected leads in a clean, filterable dashboard.

---

## Tech Stack

| Layer     | Technology                          |
|-----------|-------------------------------------|
| Frontend  | React + Vite + Tailwind CSS         |
| Backend   | Node.js + Express 5                 |
| Database  | PostgreSQL + Drizzle ORM            |
| API Layer | OpenAPI 3.1 + Orval codegen         |
| State     | TanStack React Query                |

---

## Folder Structure

```
leadhunter-ai/
├── artifacts/
│   ├── api-server/             # Express backend
│   │   └── src/
│   │       ├── agents/
│   │       │   └── leadAgent.ts    # AI agent logic
│   │       ├── routes/
│   │       │   ├── leads.ts        # /leads, /start-agent, /agent-status
│   │       │   └── health.ts       # /healthz
│   │       └── app.ts
│   └── leadhunter-ai/          # React frontend
│       └── src/
│           ├── pages/
│           │   ├── home.tsx        # Landing page
│           │   ├── search.tsx      # Lead Search page
│           │   └── dashboard.tsx   # Leads Dashboard
│           └── components/
├── lib/
│   ├── api-spec/
│   │   └── openapi.yaml        # API contract (source of truth)
│   ├── api-client-react/       # Generated React Query hooks
│   ├── api-zod/                # Generated Zod validators
│   └── db/
│       └── src/schema/
│           └── leads.ts        # Database table schema
└── README.md
```

---

## API Endpoints

| Method | Path             | Description                           |
|--------|------------------|---------------------------------------|
| POST   | `/api/start-agent` | Start the AI lead search agent       |
| GET    | `/api/leads`     | Get all collected leads (filterable)  |
| GET    | `/api/agent-status` | Check agent running status         |
| GET    | `/api/healthz`   | Health check                         |

### POST `/api/start-agent`

**Request body:**
```json
{
  "industry": "AI startups",
  "location": "India"
}
```

**Response:**
```json
{
  "success": true,
  "message": "AI agent started! Searching for AI startups companies in India.",
  "jobId": "job_1710000000000_abc123"
}
```

### GET `/api/leads`

**Query params:** `?industry=AI+startups&location=India` (both optional)

**Response:**
```json
{
  "leads": [
    {
      "id": 1,
      "companyName": "AI Hub",
      "founderName": "Arjun Sharma",
      "email": "arjun@ai-hub.india.io",
      "linkedinUrl": "https://linkedin.com/in/arjun-sharma",
      "website": "https://ai-hub.india.io",
      "industry": "AI startups",
      "location": "India",
      "dateCollected": "2024-03-15T10:00:00.000Z"
    }
  ],
  "total": 1
}
```

---

## How the AI Agent Works

The agent (`artifacts/api-server/src/agents/leadAgent.ts`) follows this flow:

```
User Input (industry + location)
        ↓
Step 1: discoverCompanyUrls()
  → Generates candidate company URLs based on industry/location keywords
  → In production: calls TinyFish Web Agent API to search Google

Step 2 & 3: scrapeCompanyInfo() (for each URL, up to 10)
  → Visits the company website
  → Extracts founder name, email, LinkedIn URL
  → In production: uses headless browser + AI parsing

Step 4: Save to Database
  → Inserts lead record into PostgreSQL via Drizzle ORM
  → Updates live "leadsFound" counter visible in the UI
```

The agent runs **asynchronously** — it starts immediately and continues in the background while the UI polls `/api/agent-status` every 2 seconds to show progress.

---

## Installation & Setup

### Prerequisites

- Node.js 18+
- pnpm 9+
- PostgreSQL database (or Replit built-in DB)

### 1. Clone & Install

```bash
git clone <repo-url>
cd leadhunter-ai
pnpm install
```

### 2. Environment Variables

Create a `.env` file or set these in your environment:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/leadhunter
PORT=8080
```

### 3. Push Database Schema

```bash
pnpm --filter @workspace/db run push
```

### 4. Run Development Servers

Start the backend:
```bash
pnpm --filter @workspace/api-server run dev
```

Start the frontend (in a separate terminal):
```bash
pnpm --filter @workspace/leadhunter-ai run dev
```

---

## Demo Instructions

1. Open the app in your browser
2. On the **Landing Page**, click **"Start Lead Search"**
3. On the **Search Page**:
   - Enter an industry (e.g., `AI startups`)
   - Enter a location (e.g., `India`)
   - Click **"Deploy AI Agent"**
4. Watch the live status indicator — it shows how many leads have been found in real time
5. When complete, click **"View Leads"** to go to the Dashboard
6. On the **Dashboard**, you can:
   - Browse all collected leads in a table
   - Filter by industry or location
   - See company name, founder, email, LinkedIn, and collection date

---

## Extending the Agent

To connect a real web browsing agent (e.g., TinyFish Web Agent API), replace the `discoverCompanyUrls()` and `scrapeCompanyInfo()` functions in `artifacts/api-server/src/agents/leadAgent.ts` with real API calls. The rest of the architecture (async job management, DB persistence, live status polling) is already in place.
