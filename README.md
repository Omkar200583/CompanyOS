# CompanyOS — The Full-Stack AI Firm

> **An AI workforce that runs your business operations.**

CompanyOS is an AI-first Finance Operations Command Center. It coordinates specialized AI agents to process invoices, verify extracted information, calculate financial summaries, check company-configured policies, route sensitive decisions to people, and produce reports with an audit trail.

This repository is organized as three services:

| Service | Stack | Local port | Purpose |
|---|---|---:|---|
| `frontend/` | Next.js, React, TypeScript, Tailwind CSS | 3000 | User experience and operations dashboard |
| `backend/` | Node.js, Express, TypeScript, MongoDB/Mongoose | 5000 | Authentication, authorization, business APIs and persistence |
| `ai-engine/` | Node.js, TypeScript, LangChain.js, LangGraph.js, Groq | 8000 | Agent orchestration, tools and workflows |
| MongoDB | MongoDB | 27017 | Application data |

## MVP: Invoice Operations

1. A user uploads an invoice PDF.
2. The backend authenticates the user, validates the file and creates an invoice record.
3. The AI Engine extracts invoice fields and supporting evidence.
4. Verification checks required fields, arithmetic and possible duplicates.
5. Finance logic calculates totals using deterministic decimal-safe code.
6. Company-configured policies are evaluated.
7. Sensitive decisions are routed to an authorized human approver.
8. The workflow resumes and reporting uses verified persisted records.
9. Important actions are recorded in the audit trail.

## Seven AI Agents

| Agent | Responsibility |
|---|---|
| Manager | Plans and coordinates the workflow |
| Document | Extracts invoice fields from documents |
| Verification | Checks required fields, totals and duplicate candidates |
| Finance | Prepares expense and financial summaries |
| Compliance | Evaluates configured company policies |
| Approval | Creates approval requests and resumes after a human decision |
| Reporting | Produces reports from verified records |

Agents assist with work; they must not self-approve invoices or execute payments.

## Repository Layout

```text
company-os/
├── frontend/              # Next.js application
├── backend/               # Express API and business services
├── ai-engine/             # LangGraph agent service
├── docs/                  # Shared architecture and project documentation
├── .gitignore
└── README.md
```

Each service has its own README with setup, environment variables, scripts and responsibilities.

## Prerequisites

- Node.js LTS (use a version supported by the installed dependencies)
- npm
- MongoDB local instance or MongoDB connection string
- Groq API key for AI Engine development

## Local Development

Open three PowerShell terminals from the repository root.

### 1. Frontend

```powershell
cd frontend
npm install
Copy-Item .env.example .env.local
npm run dev
```

Open `http://localhost:3000`.

### 2. Backend

```powershell
cd backend
npm install
Copy-Item .env.example .env
npm run dev
```

Backend runs at `http://localhost:5000` when configured.

### 3. AI Engine

```powershell
cd ai-engine
npm install
Copy-Item .env.example .env
npm run dev
```

AI Engine runs at `http://localhost:8000` when configured.

> These commands assume each service's package scripts and entry files have been implemented. A folder scaffold or README alone does not mean the service is operational.

## Environment Variables

Never commit real secrets. Use `.env.example` as a template and keep actual `.env` files local or in the hosting provider's secret manager.

### Frontend

Typical variables:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api/v1
```

Only values safe for browser exposure may use the `NEXT_PUBLIC_` prefix. Never place private API keys here.

### Backend

```env
PORT=5000
NODE_ENV=development
MONGO_URI=mongodb://127.0.0.1:27017/companyos
JWT_SECRET=replace_with_a_long_random_secret
JWT_EXPIRES_IN=7d
AI_ENGINE_URL=http://localhost:8000
AI_ENGINE_SECRET=replace_with_a_shared_service_secret
MAX_FILE_SIZE=10485760
```

### AI Engine

```env
PORT=8000
NODE_ENV=development
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=your_current_supported_groq_model
MONGO_URI=mongodb://127.0.0.1:27017/companyos
AI_ENGINE_SECRET=use_the_same_secret_as_backend
BACKEND_URL=http://localhost:5000
```

Check the current Groq model catalog before choosing a model ID.

## Service Communication

```text
Browser
  |
  v
Next.js Frontend :3000
  |
  v
Express Backend :5000 ---- MongoDB :27017
  |
  | authenticated internal service request
  v
AI Engine :8000
  |
  v
LangGraph workflow -> agents -> validated tools
```

The backend remains the authority for user identity, roles, company membership, tenant isolation and business records. The AI Engine must not be directly exposed as a privileged browser-facing service.

## Security Principles

- Enforce authentication and authorization on the backend.
- Scope every company-owned database query to the authenticated tenant.
- Validate file size, MIME type and file signature.
- Treat invoice text as untrusted input and defend against prompt injection.
- Keep JWT, Groq and internal service secrets out of source control and browser code.
- Use decimal-safe deterministic calculations for money.
- Require an authorized human decision for sensitive approvals.
- Record important workflow and approval actions in an audit log.

## Documentation

- [Frontend README](frontend/README.md)
- [Backend README](backend/README.md)
- [AI Engine README](ai-engine/README.md)
- [Documentation Index](docs/README.md)

## Suggested Build Order

1. Confirm all three services start and expose health checks.
2. Implement authentication, company membership and tenant isolation.
3. Implement invoice upload and secure file storage.
4. Implement extraction and verification.
5. Add deterministic finance calculations and policy evaluation.
6. Add approval pause/resume with human authorization.
7. Add reports, audit trail and agent monitoring.
8. Complete tests, accessibility and deployment configuration.

## Current Project Status

This README describes the intended architecture and MVP. Update this section as features become implemented and tested. Do not describe planned capabilities as completed.

## Contributing

1. Create a feature branch.
2. Keep changes scoped to the relevant service.
3. Add or update tests for behavior changes.
4. Update service documentation when APIs or configuration change.
5. Never commit secrets, private customer documents or production data.

## License

Add the license selected by the project owners before publishing or accepting external contributions.
