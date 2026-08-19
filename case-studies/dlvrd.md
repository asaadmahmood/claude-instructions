# DLVRD — Full Feature Overview

## What this app is

**DLVRD** is a premium **video delivery platform and client gallery engine** built for professional filmmakers, videographers, and production agencies. The pitch is practical: *stop sending a Dropbox link and a file called `FINAL_v7_2.mp4` — send a branded gallery your client opens on their phone and immediately feels like they hired a professional.*

A creator uploads large video and photo masters, DLVRD handles proxy generation and streaming optimization, and the creator designs a cinematic gallery (hero, branding, security) before sharing a single secure link. Clients view, stream, and download without creating an account. Finished projects can be archived into cheap cold storage so working drives stop filling up.

Despite starting from a Figma Make design export, the codebase (23 database models, 37 migrations, ~160 REST API endpoints, a split API/worker deployment, and a full admin ops console) is a production-grade **media delivery SaaS** — not a gallery MVP. It launched at [dlvrd.io](https://www.dlvrd.io/) and reached **300+ users within weeks** of going live.

It ships as **two connected surfaces**:

1. **Web app** — React SPA (creator dashboard, gallery editor, public client galleries, admin console)
2. **API + workers** — Express backend with BullMQ job workers for video compression, thumbnails, vault transfers, bulk ZIP downloads, and RAW image development

The product is sold on **Stripe subscriptions** (Starter free → Basic $15/mo → Pro $39/mo → Studio $79/mo) plus **one-time Vault storage block purchases** (1 TB cold archive).

---

## Tech stack

| Layer | Technology |
|---|---|
| **Frontend** | React 19, TypeScript, Vite 8, Tailwind CSS 4, Radix UI / shadcn, Motion, Video.js, React Router 7; dark cinematic UI originated from Figma Make |
| **Backend** | Node.js, Express 5 (TypeScript), JWT + refresh-token auth, Zod validation |
| **Data** | PostgreSQL via **Prisma 7**; **Redis** for rate limiting + BullMQ job queues |
| **Media processing** | **FFmpeg** (`fluent-ffmpeg`) for 1080p/720p proxy compression; **Sharp** for thumbnails; dedicated RAW develop queue; per-user job fairness and orphan reclaim |
| **Storage** | **MinIO** (S3-compatible) for active/hot storage; separate **Vault** object storage (R2/S3-compatible cold tier); presigned multipart uploads |
| **Payments** | **Stripe** — lazy-created products/prices, subscriptions, checkout sessions, webhooks, vault add-on purchases, partner promo codes |
| **Integrations** | **Google OAuth** signup/login; **Dropbox** import (OAuth); **Cloudflare Turnstile** (registration + abuse reports) |
| **Analytics & ops** | **ClickHouse** for bandwidth event analytics; **OpenObserve** scaffold; admin worker fleet dashboard |
| **Email** | Nodemailer + Postfix (verification, password reset, download notifications) |
| **Infra** | npm monorepo (`client/` + `server/`); Docker Compose (dev/prod); **HAProxy** load balancing; GitHub Actions CI/CD (main/staging/uat/production); production SPA on **Cloudflare Workers Static Assets** (or S3 + CloudFront / rsync); split **API stack + Worker stack** deploy for production scale; private Docker registry |

---

## Signature flow — "From upload to branded client delivery"

The core creator journey:

**Upload masters → organize in library → design gallery → share secure link → client streams/downloads → archive to Vault.**

Creators drag large video and photo files into a folder tree or gallery. DLVRD enqueues background jobs to generate streaming proxies (720p/1080p) and thumbnails while uploads continue. The gallery editor lets them set a cinematic hero, client metadata, theme/accent, layout, crew credits, and security (password, expiry, watermark, download controls). One share link (`/g/{token}`) gives the client a phone-ready viewing experience — no login, no ads, optional password gate — with full-resolution downloads when allowed.

---

## Core feature areas

### 1. Media library & upload pipeline

A full **media manager** with nested **folder trees**, grid/list views, multi-select, keyboard selection, and preview modals. Uploads are **multipart and resumable** with pause/resume, a global upload queue, and in-flight quota enforcement so creators can't overshoot active storage mid-upload. Supports **Dropbox import** via OAuth. Handles **RAW image formats** (including BRAW) with dedicated develop workers and format-aware placeholders. Every file tracks processing status, compressed versions, vault state, and storage keys.

### 2. Video compression & streaming

A distributed **FFmpeg worker fleet** processes uploaded video into **720p and 1080p proxy streams** for instant playback on any device, while preserving full-resolution originals for client download. Thumbnail generation runs on a separate queue with **thumbnail-first priority** so galleries feel ready quickly. Includes compression decision logic (skip when not needed), retry/backfill cron jobs, stuck-job recovery, per-user fairness limits, and admin visibility into pending 1080p/720p/thumbnail work per file.

### 3. Client gallery engine (the product)

The heart of DLVRD. Creators build **branded client galleries** with:

- **Hero** — cover image, title, subtitle, date, location, total size, crew credits
- **Content** — ordered media from library with sort control
- **Style** — four themes (`cinematic`, `elegant`, `glass`, `light`), accent color, font style, dark mode
- **Layout** — grid vs list, configurable columns
- **Security** — password protection, expiring links, watermarks, download toggle, download email notifications, branded logo overlay

Public galleries live at `/g/{shareToken}` with view/download counts, individual media permalinks, password gate with branded accent, expired/not-found states, bulk and single-file client downloads, and abuse reporting (Turnstile-protected). **Hybrid galleries** — video masters and high-res photos in one immersive link.

### 4. The Vault (cold storage archive)

A distinct **cold storage tier** for finished deliverables that shouldn't live on working drives forever. Creators purchase **1 TB blocks** (one-time Stripe checkout) and move media from **active** (hot MinIO) to **vault** (separate object storage). Background workers handle `MOVING → VAULTED → RESTORING` state transitions. Vault has its own quota tracking, thumbnail regen on restore, and plan-gated access (Basic+). This is a major differentiator vs. generic gallery tools.

### 5. Media requests (client upload links)

Shareable **`/request/{id}`** pages where **clients upload files back to the creator** — footage drops, selects, revisions. Optional password and expiration. Same multipart/resumable upload UX as the creator side, with upload count tracking and destination folder binding. Useful for receiving client-provided assets without email chains or WeTransfer links.

### 6. Subscription billing & plan gating

Full **Stripe** lifecycle — checkout sessions, webhooks, upgrades, downgrades, scheduled cancellation, and vault add-on purchases. Five plan tiers:

| Plan | Price | Active storage | Galleries | Highlights |
|---|---|---|---|---|
| **Starter** | Free | 10 GB | 2 | Basic delivery |
| **Basic** | $15/mo ($12/yr) | 100 GB | Unlimited | Vault, watermark, password, download notifications |
| **Pro** | $39/mo ($33/yr) | 500 GB | Unlimited | + Remove DLVRD branding |
| **Studio** | $79/mo ($67/yr) | 2 TB | Unlimited | + Team seats (schema ready) |
| **Partner** | Admin-assigned | 2 TB | Unlimited | + Referral program |

Feature flags (`PlanFeature`) gate vault access, watermarking, password protection, download notifications, white-label branding, and partner program access. Active storage quotas are enforced on upload, restore, and media-request paths.

### 7. Partner / referral program

Built for creator-led growth (likely driving the 300+ user launch spike):

- Unique **partner codes** (e.g. `patrick-0001`) with lazy-created Stripe coupons/promo codes
- **$15 commission per successful referral**, tracked through pending → completed → paid-out states
- Creator-facing **Referrals tab** with stats, code copy, and payout request
- Admin tools to assign partner profiles, set payout dates, mark commissions paid, and export referral data

---

## Platform, admin & infrastructure

### 8. Auth & onboarding

**Email/password + Google OAuth** with email verification, password reset, refresh tokens, and Cloudflare Turnstile bot protection. New users flow through plan selection (`/select-plan`) with optional marketing checkout resume from URL params. Company profile stores logo, name, and social links (Instagram, Facebook, LinkedIn, X). Newsletter opt-in and Google signup consent modal. Gallery defaults saved per user for faster repeat delivery.

### 9. Admin console

A full internal ops dashboard at `/admin`:

- **Overview** — platform-level metrics
- **Users** — search, filter by plan, CSV export, marketing opt-in target export
- **Workers** — fleet monitoring (compression, thumbnail, vault-transfer, vault-thumb-regen), pending media by need (1080p/720p/thumbnail/vault/failed), stuck jobs, per-user job view, file-level compression debug panel
- **Storage** — usage across the platform
- **Subscriptions** — Stripe analytics
- **Bandwidth** — ClickHouse-backed transfer analytics
- **Moderation & support** tabs
- Partner profile management and referral payout tracking

Admin access is granted via env bootstrap or delegated grants (`AdminAccess` model).

### 10. Bulk download & delivery

Clients and creators can **bulk-download gallery media as streamed ZIP archives**. Large jobs are prepared asynchronously via worker queue with cancellation support. MinIO object streaming with retry/resume, concurrency gates, and prefetch for zip assembly — designed for multi-GB deliverables without loading everything into memory.

### 11. Real-time upload UX & quota enforcement

A **global upload context** spans the app — upload indicator, navigation warnings ("upload in progress"), logout confirmation, and virtualized upload queue. Active storage usage includes in-flight upload bytes. Quota checks run before batch uploads with human-readable error messages ("Insufficient Active Storage — clear space or archive to Vault").

### 12. Deployment & environments

Production-grade CI/CD via **GitHub Actions**:

- Version stamping on every deploy (`1.0.0-{run_number}`)
- **Web**: Vite build → Cloudflare Workers (production) or S3/CloudFront or rsync
- **Server**: Docker image build → private registry → pull on target host
- **Combined stack** (main/uat): HAProxy + API replicas + Postgres + Redis + MinIO + Postfix + workers
- **Split stack** (staging/production): separate API host and Worker host sharing Postgres/Redis/ClickHouse over private bind IP
- Slack deploy notifications with job-level success/failure and recent commit log
- Internal hosting cost comparison model for infra planning

---

## In one sentence

DLVRD started as a filmmaker's answer to the gray Dropbox folder and became a **full premium media delivery platform** — resumable multipart uploads, FFmpeg proxy streaming, branded cinematic client galleries with password/expiry/watermark controls, hybrid photo+video delivery, client upload request links, Stripe subscription billing with a cold-storage Vault archive, a creator partner/referral program, and a production admin ops console — delivered as a React SPA + Express API with a distributed BullMQ worker fleet, deployed across Cloudflare, Docker, and split API/worker infrastructure, reaching 300+ users within weeks of launch at dlvrd.io.