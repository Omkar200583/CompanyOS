# CompanyOS AI Engine

The AI Engine coordinates CompanyOS's specialized agents and tools. It uses Node.js, TypeScript, LangChain.js, LangGraph.js and Groq.

## Responsibilities

- Coordinate multi-step invoice workflows.
- Extract document fields and evidence.
- Run verification and duplicate checks.
- Invoke deterministic finance and policy tools.
- Create approval requests through authorized backend capabilities.
- Pause and resume workflows after a human decision.
- Prepare reports from verified persisted data.

The AI Engine is not the system of record for user permissions and must not make final sensitive financial approvals.

## Seven Agents

| Agent | Responsibility | Safety boundary |
|---|---|---|
| Manager | Plans and delegates workflow steps | Cannot bypass policy |
| Document | Extracts invoice fields and evidence | Must not invent missing values |
| Verification | Checks required fields, totals and duplicates | Use deterministic validation |
| Finance | Produces expense and financial summaries | Arithmetic belongs in decimal-safe code |
| Compliance | Evaluates configured company policies | Does not certify legal or tax compliance |
| Approval | Creates requests and resumes after a human decision | Cannot self-approve |
| Reporting | Summarizes verified records | Must not present unverified output as fact |

## Setup

```powershell
cd ai-engine
npm install
Copy-Item .env.example .env
npm run dev
```

Expected local port: `8000`.

## Environment

```env
PORT=8000
NODE_ENV=development
GROQ_API_KEY=your_groq_api_key
GROQ_MODEL=your_current_supported_groq_model
MONGO_URI=mongodb://127.0.0.1:27017/companyos
AI_ENGINE_SECRET=use_the_same_secret_as_backend
BACKEND_URL=http://localhost:5000
```

Choose a model ID currently supported by Groq. Keep the API key and internal service secret server-side.

## Suggested Agent Tools

- Invoice search and invoice details.
- Duplicate check.
- PDF extraction and OCR.
- Expense calculation, tax calculation and financial summary.
- Company policy check.
- Create approval request and retrieve approval status.
- Report generation and vendor search.
- Audit logging, workflow planning and agent delegation.

Validate every tool input with a schema. Use an allowlist of tools available to each agent.

## LangGraph Invoice Workflow

```text
START
  -> MANAGER
  -> DOCUMENT
  -> VERIFICATION
  -> FINANCE
  -> COMPLIANCE
  -> HUMAN APPROVAL (when required)
  -> REPORTING
  -> END
```

Route missing fields, duplicate candidates and failed checks to an exception/manual-review path. Use persistent checkpointing and a controlled interrupt/resume mechanism for human decisions.

## Suggested API Contract

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/health` | Health status |
| GET | `/api/v1/agents/status` | Agent status |
| POST | `/api/v1/agents/execute` | Execute permitted task |
| POST | `/api/v1/agents/test` | Development test |
| POST | `/api/v1/workflows/invoice-processing` | Start invoice workflow |
| POST | `/api/v1/workflows/monthly-report` | Start monthly report |
| GET | `/api/v1/workflows/:executionId` | Get workflow state |
| POST | `/api/v1/workflows/:executionId/resume` | Resume after authorized decision |

The backend should authenticate internal calls and verify user authorization before requesting privileged operations.

## AI Safety

- Treat invoice text and attachments as untrusted data.
- Defend against prompt injection in documents.
- Do not let model output directly execute arbitrary code or database operations.
- Validate tool arguments and outputs.
- Do not let agents approve invoices or initiate payments.
- Use deterministic code for financial calculations.
- Keep an audit trail of tool calls and workflow transitions.
- Avoid sending unnecessary personal or confidential data to the model.

## Testing

Test successful extraction, missing fields, malformed documents, duplicate candidates, arithmetic mismatches, policy flags, tool failures, retries, timeouts and human approval pause/resume.

## Scripts

Check `package.json` for actual scripts. Common scripts:

```powershell
npm run dev
npm run build
npm test
npm start
```
