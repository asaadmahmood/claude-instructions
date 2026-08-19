# TSS Accounts ("Halal Accounts") — Full Feature Overview

## What this app is

**TSS Accounts** is a full-stack, multi-tenant **business management SaaS platform** built around accounting, but grown into a complete back-office suite. Despite the README calling it an "Accounting & Invoicing Application," the codebase (94 database models, ~58 frontend pages, ~45 backend API domains) actually spans **accounting, invoicing, payroll/HR, time tracking, project management, recruitment, AI assistants, and bank integrations** — sold to businesses on tiered subscription plans.

It ships as **four connected products**:

1. **Web app** — React frontend + Node/Express backend (the core)
2. **Desktop app** — Electron wrapper with system-tray timer & AI
3. **Browser extension** — Chrome MV3 time tracker with ClickUp integration
4. **User-management service** — a separate Next.js admin app for plans/users/businesses

The "Halal" branding reflects its Islamic-finance features: a **Zakat calculator** (with live gold prices) and Sharia-aware reporting.

---

## Tech stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, TypeScript, Vite, Tailwind 4, HeroUI, React Router 7, TanStack Query + Table, Zustand, React Hook Form + Zod, Chart.js, Framer Motion, Socket.io client |
| **Backend** | Node/Bun, Express 5, TypeScript, Prisma ORM, PostgreSQL, Redis (ioredis), Socket.io (Redis adapter), JWT auth |
| **Storage/Docs** | MinIO/S3 + local FS, PDFKit/jsPDF, ExcelJS, Sharp, pdf-parse/mammoth (doc text extraction) |
| **AI** | Anthropic, OpenAI, Google Gemini, DeepSeek, Perplexity, Dify; MCP (Model Context Protocol) |
| **Payments** | Stripe (subscriptions + invoice payments), Payoneer |
| **Banking** | Plaid, SaltEdge, Yodlee |
| **Desktop** | Electron; **Extension**: Chrome Manifest V3 |
| **Admin** | Next.js 16, NextAuth, Upstash Redis |
| **Infra** | Docker Compose (dev/stage/prod), HAProxy, Postfix, Playwright E2E |

---

## Core feature areas

### 1. Accounting (the foundation)

- **Chart of Accounts** — coded accounts (Asset/Liability/Equity/Revenue/Expense), per-account currency overrides, links to clients/staff/bank accounts; CSV import/export; AI-suggested account codes. Balances are computed on the fly from entries (no stored balances).
- **Journal Entries & Transactions** — strict double-entry bookkeeping, multi-line debit/credit, document attachments, reusable templates (with percentage distribution), reversing/closing entries.
- **Financial Periods** — open/close accounting periods with locking, a one-time closing transaction, configurable reopen grace days, and optional FX-revaluation snapshots.
- **Multi-currency** — home currency + foreign amounts and exchange rates per entry; live rates from Frankfurter (fallbacks: Free Currency API, Open Exchange Rates), Redis-cached; mid/buy/sell variants; period-close forex revaluation for unrealized gains/losses.

### 2. Invoicing & Receivables

- Full invoice lifecycle (Draft → Sent → Paid/Overdue → Cancelled) with a valid state machine, line items, per-invoice/per-item tax policies, multi-currency, custom numbering/prefixes per client.
- **PDF generation & preview**, email delivery, **Stripe online payments**, batch **monthly invoice generation**, and **retainer billing** (auto-invoice from time entries based on per-project member rates).

### 3. Clients & CRM

- Client records with billing addresses, per-client currency/tax IDs/invoice prefixes, document storage, revenue tracking, and **profile-lock protection** for clients synced live from PM tools.

### 4. Banking & Reconciliation

- **Bank accounts** typed as BANK / PAYONEER / STRIPE.
- **Live bank connections** via **Plaid, SaltEdge, Yodlee** (auto-sync ~hourly) plus manual **CSV/OFX/PDF statement upload** with hash-dedup.
- **Reconciliation** with an ML-style **account-mapping engine** that learns description→account mappings with confidence scoring, plus webhooks for provider transaction updates.

### 5. Reporting & Analytics

Income Statement, Balance Sheet, Cash Flow Statement, **Cash Flow Projection** (forecasting), Revenue Analysis, Expense Categories, **Forex Gain/Loss**, Billing Reports, Timesheet Reports, and the **Zakat Calculator** (Islamic wealth tax with live gold prices). Exports to PDF/Excel/CSV.

### 6. Dashboards

Role-aware multi-dashboard system — Accounting, Client/Invoice, Staff/Payroll, Time-Tracking, Attendance, and Welcome variants, with real-time Socket.io updates.

---

## HR & workforce suite

### 7. Staff Management

Employee records with salary/hourly/billing/overtime rates (per-staff currency), bank & Payoneer payout details, work schedules, joining/termination tracking, and document storage.

### 8. Payroll

Payroll periods, per-staff salary calc (base + bonus + overtime − deductions − tax), **tax-policy withholding**, reimbursements & loan repayments, unpaid-leave deductions, payslip emails, and auto-creation of financial transactions on approval. Pays via bank transfer or Payoneer.

### 9. Attendance & Leave

Punch in/out sessions with auto-timeout detection, manual entry/correction, calendar & table views, leave types (paid/unpaid), leave request approval workflows, and attendance-driven payroll hours.

### 10. Appraisals

Multi-axis (weighted) star-rating performance reviews with Draft→Submitted→Approved/Rejected workflow and **automatic salary-increment calculation** from scores.

### 11. Tax Policies

Progressive tax **slabs**, country/region **templates** (e.g., Pakistan income tax), separate payroll vs. sales tax types, and date-ranged validity.

---

## Project & time management

### 12. Projects, Tasks, Sprints, Epics, Releases

Full PM module: projects with billing rates, work items (Task/Bug/Story) with hierarchy/assignees/priority, **Scrum** support (sprints, epics, releases with progress rollups), **custom fields/properties**, and project member rates.

### 13. Time Tracking

Real-time timer (play/pause/stop) across web, desktop tray, and browser extension; manual entries; billable flags; tags; voice notes (Web Speech API); overlap detection; and invoice linking.

### 14. PM/Time integrations

Two-way/live sync with **ClickUp, Jira, Asana, Monday, Trello, Linear, Azure DevOps, Clockify, Everhour** — incremental cursor-based sync running on a background job every few minutes.

---

## AI & automation

### 15. AI Chat Assistant

In-app conversational assistant with **streaming responses**, extended-thinking visualization, **voice input + TTS output**, conversation history, entity linking, and file/PDF export. Provider-agnostic across **Anthropic, OpenAI, Gemini, DeepSeek, Perplexity, and Dify**, with per-user provider/model/API-key overrides (AES-GCM encrypted) and **per-business cost tracking**.

### 16. MCP (Model Context Protocol)

Exposes **40+ business tools** (read accounts, create invoices, run reports, view payroll/time, manage bank connections) for LLM function-calling, secured by issuable/revocable API-key credentials.

### 17. Dify knowledge base

Auto-syncs business data (clients, projects, invoices, staff, documents — with text extracted from PDF/DOCX/Excel/CSV) to a Dify knowledge base on a cron schedule for RAG, with a CLI sync tool and per-business isolation.

### 18. Recruitment / Onboarding

A complete **hiring suite**: job ads (AI-generated), candidate pipeline, interview templates & question banks, interview recording/transcription/translation (Fathom), weighted ratings, AI interview summaries, public **questionnaire forms** that auto-create candidates, LinkedIn job-ad sync, and candidate→staff conversion.

---

## Platform, security & infrastructure

### 19. Multi-tenancy, auth & access control

- Email/password + **Google OAuth**, email verification, password reset.
- **Multi-business** per user with INVITED→ACTIVE→INACTIVE membership.
- **RBAC** across 50+ modules with READ/CREATE/UPDATE/DELETE/LINK/MANAGE permission types, role-level + user-level overrides, all Redis-cached.
- **Domain/email allowlist** restrictions.

### 20. Subscription billing

Tiered **Plans** with per-plan permission sets and **data limits** (max users/staff/clients/projects/tasks/etc.) enforced at the API layer; Stripe subscription lifecycle with grace periods, plan-expired pages, and daily subscription validation.

### 21. Documents

Versioned document system across 5 scopes (staff, client, transaction, task, project) with categories, tags, hash-dedup, visibility controls, and text extraction for AI RAG. Stored on MinIO/S3 or local FS.

### 22. Security & compliance

Comprehensive **audit logging** (user/IP/agent/diff via Prisma middleware), tiered **rate limiting**, Helmet headers, CORS, token blacklisting, AES-GCM encryption of secrets, webhook HMAC verification, a **privacy mode** (auto-blur on window-blur), and documented **NCA (KSA) + ISO 27001** compliance assessments.

### 23. Real-time & background jobs

Socket.io with Redis adapter for multi-instance broadcasting (live timers, chat streaming, dashboards) plus a **cron leader-election** system so only one instance runs scheduled jobs (bank sync, subscription validation, AI sync, PM live sync, Clockify refresh).

### 24. Desktop, extension & admin

- **Electron desktop app** — embedded web app, system-tray timer & AI panels, native notifications, mic permissions, deep linking, S3-backed offline storage.
- **Browser extension** — MV3 time tracker, badge timer, ClickUp in-page widgets, real-time socket sync, per-business form-draft storage.
- **User-management (Next.js)** — separate admin panel for managing plans, users, and businesses platform-wide (shares the Prisma schema).

### 25. Deployment

Dockerized dev/stage/prod stacks behind **HAProxy** (load-balanced, sticky sessions for sockets), with PostgreSQL, Redis, Postfix email, and optional MinIO. CI scripts handle build versioning, image tagging, and registry deploys. **Playwright** covers E2E flows (auth, business creation, accounts/invoices/clients/journals/periods CRUD, time tracking, attendance, leave).

---

## In one sentence

It started as halal-friendly accounting & invoicing software and became a **full SMB operating system** — general ledger, invoicing, banking, payroll, HR, attendance, appraisals, projects, time tracking, recruitment, multi-LLM AI assistance, and Zakat compliance — delivered as a multi-tenant SaaS across web, desktop, and browser-extension clients.
