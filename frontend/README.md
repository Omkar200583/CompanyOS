# CompanyOS Frontend

The CompanyOS frontend is the user-facing Finance Operations Command Center, built with Next.js, React, TypeScript and Tailwind CSS.

## Responsibilities

- Authentication screens and role-aware navigation.
- Finance dashboard and invoice inbox.
- Invoice upload, PDF preview and extraction review.
- AI agent status and workflow activity.
- Exception handling and human approval workspace.
- Financial analytics, vendor views, reports, policies and audit history.

## Recommended Product Experience

### Core navigation

- Overview
- AI Command Center
- Invoices
- Exceptions
- Approvals
- Vendors
- Reports
- Policies
- Audit Logs
- Company Settings

### Differentiating features

1. **AI Command Center:** seven agent cards showing status, current task and recent executions.
2. **Explainable Invoice Review:** document preview beside extracted values and evidence.
3. **Exception Intelligence:** missing fields, duplicate candidates, arithmetic mismatches and policy flags.
4. **Workflow Replay:** chronological steps, tool results, errors and human intervention.
5. **Human Approval Workspace:** approval reason, evidence, comments and decision history.
6. **Policy Simulator:** preview configured policy effects against sample invoices.

## Technology

- Next.js App Router
- React and TypeScript
- Tailwind CSS
- Lucide React icons
- Framer Motion for restrained motion
- Recharts for analytics
- React Hook Form and Zod for forms and validation
- Axios or a typed `fetch` wrapper for backend requests
- Zustand only where shared client state is genuinely needed

Use accessible components, keyboard navigation, visible focus indicators, descriptive labels and sufficient color contrast.

## Setup

```powershell
cd frontend
npm install
Copy-Item .env.example .env.local
npm run dev
```

Open `http://localhost:3000`.

## Environment

Create `.env.example` with safe public configuration, for example:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1
```

Do not add Groq API keys, JWT signing secrets, database credentials or internal service secrets to frontend environment variables. Any `NEXT_PUBLIC_` value is exposed to the browser.

## Suggested Structure

```text
frontend/
├── public/
├── src/
│   ├── app/
│   │   ├── (public)/
│   │   ├── (auth)/
│   │   └── (dashboard)/
│   ├── components/
│   │   ├── layout/
│   │   ├── dashboard/
│   │   ├── invoices/
│   │   ├── approvals/
│   │   ├── agents/
│   │   └── common/
│   ├── features/
│   │   ├── auth/
│   │   ├── invoices/
│   │   ├── approvals/
│   │   ├── agents/
│   │   ├── analytics/
│   │   ├── vendors/
│   │   └── policies/
│   ├── hooks/
│   ├── lib/
│   ├── store/
│   ├── types/
│   └── styles/
└── README.md
```

Adapt this structure to the actual Next.js scaffold; avoid creating duplicate route or component systems.

## API Integration

The frontend should call the backend only. The backend calls the AI Engine through authenticated internal requests.

Suggested API base:

`NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1`

Handle loading, empty, success and error states. Do not assume a workflow has completed until the backend reports its persisted status.

## Quality Checklist

- Responsive layouts for desktop, tablet and mobile.
- Accessible forms, dialogs, tables and navigation.
- Role-aware navigation backed by server-side authorization.
- Clear status labels for pending, processing, needs-review, approved, rejected and failed.
- No fake financial totals presented as real company data.
- Tests for important forms and workflow states.

## Scripts

Use the scripts defined in `package.json`, commonly:

```powershell
npm run dev
npm run build
npm run start
npm run lint
```

A script is available only if it is defined in the current `package.json`.
