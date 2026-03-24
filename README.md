# LeadHunter AI 🤖
**Autonomous Sourcing Agent built with TinyFish API**

LeadHunter AI is an autonomous lead generation and job sourcing platform that automates the tedious process of finding and validating professional opportunities. It scours multiple platforms (LinkedIn, Naukri, Wellfound, etc.) and prioritizes official "Direct Career Page" links to ensure you get the highest quality application sources.

---

## 🚀 Key Features
- **Autonomous Sourcing**: Powered by TinyFish API for human-like web navigation.
- **Direct Link Priority**: Automatically identifies and extracts official company career pages.
- **Self-Cleaning Pipeline**: Automated 4:00 AM refresh and 30-day cleanup cycle.
- **Professional Dashboard**: Sleek React-based interface with glassmorphism aesthetics.
- **Team Context Integrated**: Capture team descriptions to attract high-quality talent.

## 🛠️ Technology Stack
- **AI Agent**: TinyFish API
- **Frontend**: Vite, React, Tailwind CSS, Framer Motion
- **Backend**: Express.js, TypeScript
- **Database**: Drizzle ORM + SQLite (Local) / LibSQL (Cloud)
- **Deployment**: Optimized for Vercel Monorepo

## 📦 Getting Started

### Prerequisites
- Node.js (v18+)
- pnpm

### Installation
1. Clone the repository:
   ```bash
   git clone https://github.com/Sidd67/LeadHunter-ai.git
   cd LeadHunter-ai
   ```
2. Install dependencies:
   ```bash
   pnpm install
   ```
3. Set up environment variables:
   Create a `.env` file in the root with:
   ```env
   TINYFISH_API_KEY=your_api_key_here
   PORT=8080
   BASE_PATH=/
   ```

### Running Locally
1. Start the Backend API:
   ```bash
   cd artifacts/api-server
   pnpm run dev
   ```
2. Start the Frontend Dashboard:
   ```bash
   cd artifacts/leadhunter-ai
   pnpm run dev
   ```

## 🌐 Deployment (Vercel)
This project is pre-configured for Vercel deployment. 
1. Connect your repository to Vercel.
2. Set the **Root Directory** to `.` 
3. Configure **Build Command** to `pnpm build` and **Output Directory** to `artifacts/leadhunter-ai/dist/public`.
4. Add your `DATABASE_URL` (Turso) and `TINYFISH_API_KEY` to Vercel Environment Variables.

---
*Developed as part of the Autonomous Agent Hackathon Challenge.*
