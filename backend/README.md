# CompanyOS Backend

The backend is the trusted business API for CompanyOS. It is built with Node.js, Express, TypeScript and MongoDB/Mongoose.

## Responsibilities

- User authentication and session/token handling.
- Company membership, roles and tenant isolation.
- Invoice and vendor records.
- Secure document upload and storage integration.
- AI Engine service communication.
- Approval authorization and workflow coordination.
- Policies, reports and audit events.
- Request validation, error handling, logging and rate limiting.

## Setup

```powershell
cd backend
npm install
Copy-Item .env.example .env
npm run dev
```

Expected local port: `5000`.

## Environment

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

Generate a strong random JWT secret for local development and use a managed secret store in production. The backend and AI Engine must use the same internal service secret if the chosen authentication design requires it.

## Suggested Modules

| Module | Responsibility |
|---|---|
| auth / users | Identity, login, profile, memberships and roles |
| companies | Tenant records and company settings |
| invoices | Upload metadata, lifecycle, extraction and verification |
| vendors | Supplier directory and invoice matching |
| agents | Start permitted tasks and retrieve execution summaries |
| approvals | Approval requests, decisions and separation of duties |
| policies | Company-specific thresholds and policy versions |
| reports | Reports based on verified persisted data |
| audit | Actor, action, resource, timestamp and correlation ID |

## Suggested API Contract

Prefix: `/api/v1`

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/auth/register` | Register user |
| POST | `/auth/login` | Authenticate |
| GET | `/auth/me` | Current user |
| POST | `/invoices` | Upload invoice |
| GET | `/invoices` | List tenant invoices |
| GET | `/invoices/:invoiceId` | Invoice details |
| POST | `/invoices/:invoiceId/process` | Start processing |
| GET | `/agents/status` | Agent status |
| POST | `/agents/execute` | Request permitted agent task |
| GET | `/approvals` | List approval requests |
| POST | `/approvals/:id/approve` | Authorized human approval |
| POST | `/approvals/:id/reject` | Authorized human rejection |
| GET | `/reports` | List reports |
| POST | `/reports/generate` | Generate report |
| GET / POST | `/policies` | Read or create policies |
| GET | `/audit-logs` | Read authorized audit history |

Treat these as proposed API contracts; align route names and response shapes with the implementation.

## Data Model Concepts

- **User:** identity, password hash, status and company memberships.
- **Company:** tenant identity, settings, currency and timezone.
- **Invoice:** tenant ID, vendor, invoice number, dates, currency, line items, totals, status and file reference.
- **Vendor:** tenant ID, normalized name and contact metadata.
- **Approval:** tenant ID, invoice, requester, approver, status, decision and timestamps.
- **Policy:** tenant ID, version, conditions, thresholds and effective dates.
- **AgentExecution:** tenant ID, workflow ID, agent, state, summary and errors.
- **AuditLog:** tenant ID, actor, action, resource, timestamp and metadata.

## Security Requirements

1. Derive tenant identity from the authenticated user and authorized membership—not an untrusted request body.
2. Apply tenant filters to every company-owned read, update and delete.
3. Validate role and resource authorization on every sensitive route.
4. Validate uploaded file size, MIME type and file signature.
5. Store production files in managed object storage; do not rely on ephemeral deployment disks.
6. Use decimal-safe money calculations and explicit rounding rules.
7. Validate approval authority on the server and enforce separation of duties.
8. Never expose internal AI Engine credentials to the browser.
9. Use safe error responses, rate limiting and structured logs.

## Scripts

Check `package.json` for the actual available scripts. Common scripts:

```powershell
npm run dev
npm run build
npm test
npm start
```
