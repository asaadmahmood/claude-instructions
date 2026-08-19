# Morket — Full Feature Overview - morket.io

## What this app is

**Morket** is a multi-tenant **AI marketing platform** that acts as a **marketing co-founder for technical/solo founders**. The pitch is literal: *"You build the product. Morket does the marketing."* A founder pastes a product URL, Morket reads the site "like a marketer," builds a full brand + strategy foundation, and then runs the actual marketing work across every channel — launch, SEO, content, social, directories, AI-search visibility, and analytics — from one workspace.

Despite the README calling it an "AI marketing co-founder MVP," the codebase (35 database tables, ~46 frontend pages/views, ~180 backend API endpoints in a single ~12,700-line Elysia server, plus its own MCP server) is a genuinely broad **go-to-market operating system**: brand strategy, a knowledge/RAG base, a launch engine, a full SEO suite (including programmatic SEO), multi-network social publishing, an AI blog with 4 CMS publishers, generative-engine (AI-search) optimization, launch-video rendering, and an agentic "Mission" that decides what the founder should do each week.

It ships as **several connected surfaces**:

1. **Web app** — Vue 3 SPA (the core product + dashboard)
2. **API** — Bun/Elysia backend (the engine — one large service)
3. **Edge worker** — Cloudflare Worker scaffold (runtime-portable service wiring)
4. **Browser extension** — Chrome MV3 "Comment Assistant" for X/LinkedIn, grounded in the workspace's brand brain + knowledge base
5. **MCP server** — Morket exposes its own tools to external LLMs (with OAuth + API keys)
6. **Sanity Studio + Remotion** — a blog CMS schema and a programmatic launch-video renderer

The product is sold on a **Stripe subscription** (Morket Pro, $100/mo or $960/yr) with a permanent free tier.

---

## Tech stack

| Layer | Technology |
|---|---|
| **Frontend** | Vue 3, TypeScript, Vite, Tailwind, shadcn-vue, Phosphor icons, Inter; dark-mode-only, Raycast-inspired design system; Vue Router SPA |
| **Backend** | Bun runtime, Elysia (TypeScript), single service; JWT auth; **SSE streaming** for every generator |
| **Data** | PostgreSQL via **Drizzle ORM**; **Redis** for cache + job queue (`enqueueJob`/`getJob`/`updateJobStatus`) |
| **AI** | Provider abstraction in `packages/core`: **OpenAI** (GPT-4o, gpt-4o-mini, **gpt-image-1**, `text-embedding-3-small`) + **Anthropic** (Claude Sonnet 4.6, Opus 4.8, Haiku 4.5). `MockAiProvider` is the default; real provider selected when a key is present. Agentic **tool-loop** (`chatToolLoop`) for the chat assistant |
| **RAG** | OpenAI embeddings + text chunking; document ingest from uploads and GitHub source |
| **Video** | **Remotion** (Chromium render) for brand launch videos |
| **Payments** | **Stripe** — self-bootstrapping products/prices by lookup key, subscriptions, portal, card-on-file, webhooks |
| **Publishing / integrations** | LinkedIn, Twitter/X, Reddit (OAuth publish); GitHub (source sync); Google **GA4 + Search Console**; **Webflow, WordPress, Sanity, Framer** (blog CMS); **DataForSEO** (keyword/SERP data) |
| **Interop** | Model Context Protocol server (`/mcp`) with OAuth (`register`/`authorize`/`token`) + issuable API keys |
| **Infra** | Bun workspaces monorepo; Docker Compose (dev/prod); Railway + Vercel configs; static SPA rsync deploy; GitHub CI; Postfix email |

---

## Onboarding — "From a URL to a full launch"

The signature flow. A founder pastes a product URL and Morket runs a guided, streaming intake:

**URL → scrape brand → enrich-from-URL → questions → goal → pick your first move → plan.**

The API scrapes the site, enriches product/positioning context, runs a **marketing scan**, captures brand-identity screens, and produces a recommended first "growth move" plus a plan tuned to the founder's **weekly bandwidth (hours)** and **launch goal** — so recommendations fit how the business actually gets customers.

---

## Core feature areas

### 1. Brand Brain (the foundation)

A generated, editable **brand foundation** — positioning, voice/tone, key messages, honest competitor comparisons, use cases by segment, founder story and strong opinions, and visual identity. Generated with live streaming, editable field-by-field, and **injected as `brandContext` into every downstream generator's prompt** so all output stays on-brand. Backed by `brand_brains`, `voice_profiles`, `visual_references`, and a brand-identity screen-capture step.

### 2. Knowledge Base & RAG

A per-workspace **knowledge store** that fuels every generator. Supports document **upload/ingest/reindex**, **GitHub source sync**, and text chunking + **OpenAI embeddings** for retrieval. Content is organized into "content **fuel**" categories (customer wins, testimonials, proof/credibility, customer insight, voice/story, competitive edge, raw material) with a **fuel-coverage** view that shows which categories are thin and a **fuel-generate** helper.

### 3. Competitors

Auto-**discover** competitors, **analyze** them, and **select** a working set. Feeds honest comparisons into the Brand Brain and shapes positioning across modules.

### 4. Mission (the agentic to-do)

The founder's **weekly action system** — `mission_runs` + `mission_tasks`. Morket decides the highest-leverage moves ("Mission Today"), tasks can be regenerated, and the **agent chat can append items to the Mission** from a conversation. This is the "what should I actually do this week" brain.

### 5. Traction OS

The compounding-growth layer — `weekly_traction_plans` + `performance_signals`. Generates a **weekly traction plan** (streaming) with **"this week's #1 move" pinned first**, and tracks performance signals so plans build on what's working.

---

## Channels & execution

### 6. Launch engine

Generates a full **launch package** (`launch_packages` + `launch_tasks`) from the brand foundation — the assets and task list to actually ship a launch.

### 7. Directories & backlinks

A curated **directories catalog** matched to the business type (digital product, physical, marketplace, local). Generates **submission targets** and **backlink/PR opportunities** (streaming), each with editable status and links, and never wipes accumulated progress on regeneration.

### 8. SEO suite

A near-complete SEO product in its own right:

- **Site Audit** — technical/on-page audit with a saved, prioritized **action plan** (robots.txt, sitemap.xml, and DataForSEO organic data as independent inputs).
- **Keywords** — keyword-set research (DataForSEO), aware of **existing pages** so it won't recommend pages you already have.
- **Page Audit & Page Briefs** — per-page audits and generated content briefs.
- **Questions** — question-based content discovery that can draft a page from a question.
- **Programmatic SEO** — strategies → templates → generated pages at scale (`seo_programmatic_strategies/templates/pages`).
- **Content Audit** — audits existing blog content and identifies draft gaps.

### 9. Social Studio

Multi-network **generation, ideas, scheduling, and publishing** to **LinkedIn, Twitter/X, and Reddit** (OAuth per network, tokens stored server-side). Topics, per-module instructions, manual posts, duplication, schedule/unschedule, and a dedicated **Reddit strategy** module (strategy + drafts) since Reddit needs its own tone.

### 10. AI Blog

End-to-end blog engine: **ideas → monthly/weekly plans → posts → images** (gpt-image-1), an **autopilot** mode, and a stable "house style" (voice + SEO + AEO rules). **Publishes to four CMSes** — **Webflow, WordPress, Sanity, and Framer** — with per-CMS connection tests and field mapping, plus media upload.

### 11. AI Visibility (GEO / AEO)

Generative-engine optimization: content **engineered to be cited by AI assistants** (ChatGPT, Perplexity, etc.), plus **`llms.txt` generation and live verification** (confirms the founder actually hosted it at the domain root).

### 12. UGC / clipping campaigns

Generates a **UGC/clipping campaign** — but only when it genuinely fits the product (the generator returns `fits: false` and pushes nothing for products where UGC doesn't make sense). One of three **card-on-file-gated** premium modules.

### 13. Launch Video (Remotion)

Renders composed **brand launch videos** (no AI-generated people) via Remotion + Chromium. Degrades gracefully — if the renderer/Chromium isn't installed, specs still save as **drafts**.

---

## Assistant, automation & interop

### 14. Agent chat

An in-app conversational marketing assistant with **conversation history**, entity context, and an **agentic tool loop** (`chatToolLoop`) that can read workspace context and take actions (e.g. add Mission tasks, draft content).

### 15. MCP server

Morket **exposes itself over the Model Context Protocol** (`/mcp`) so external LLMs can call its tools — `get_context`, `list_mission`, `create_mission_tasks`, `add_mission_tasks`, `draft_content` — secured by an **OAuth flow** (`register`/`authorize`/`token`) and issuable/revocable **API keys**.

### 16. Browser extension

Chrome **Manifest V3** "Comment Assistant" that drafts helpful, on-brand comments for **X and LinkedIn**, grounded in the workspace's brand brain + knowledge base, with a page **bridge** so the extension authenticates against the logged-in Morket session. You review and post.

### 17. Analytics

**Google OAuth** connection to **GA4 + Google Search Console** — pulls properties/sites, traffic series, and source breakdowns into an in-app analytics dashboard so traction is measured, not guessed.

---

## Platform, billing & infrastructure

### 18. Multi-tenancy & access

Multiple **workspaces** per user with **members, roles, and email invites** (`workspaces`, `workspace_members`, `workspace_invites`). All data is workspace-scoped, with product-scoping and workspace-wide orphan fallbacks so a single-product workspace "just works." Auth is **email/password + Google OAuth**, JWT-based, with invite acceptance.

### 19. Persistence (a hard rule)

**Everything the user generates or sees persists server-side and rehydrates on load** — audit findings, generated posts/plans, connection state, settings, statuses. The established pattern: a `run`/`save` endpoint upserts into the `assets` table (by workspace + type + title), a `…/latest` endpoint rehydrates on mount, and connections live in `integrations`. If it disappears on refresh, it's a bug.

### 20. Subscription billing

**Stripe** integration that self-bootstraps its product/prices by lookup key (no dashboard setup). **Morket Pro** at **$100/mo** or **$960/yr** ($80/mo billed yearly); a permanent **free tier** with lifetime allowances (2 blog posts, 5 social posts, 15 agent messages); **internal-domain comps** (e.g. `thesmallsquare.com`); checkout, billing portal, cancel, card collection, and **Stripe webhooks**. A **card-on-file gate** protects the three expensive modules — **UGC, launch video, and blog image generation**.

### 21. Approvals & assets

An **approvals workflow** (approve/reject generated work) and a unified **Assets** library — the single persistence surface every module writes through.

### 22. Real-time, jobs & streaming

**Server-Sent Events** stream nearly every generator (brand brain, launch, SEO, social, blog plans, directories, traction plan) token-by-token. A **Redis-backed job queue** (`jobs` table + `enqueueJob`/`getJob`/`updateJobStatus`) handles longer async work.

### 23. Deployment

Bun-workspaces monorepo built and shipped via **Docker Compose** (dev/prod), with **Railway** and **Vercel** configs and a static-SPA **rsync** deploy target. Postgres + Redis + Postfix email round out the stack; the Cloudflare Worker demonstrates the same core running at the edge.

---

## In one sentence

Morket started as an "AI marketing co-founder" that turns a URL into a launch and became a **full go-to-market operating system** — brand strategy, a RAG knowledge base, an agentic weekly Mission, launch and directory engines, a complete SEO suite (including programmatic SEO), multi-network social publishing, an AI blog with four CMS publishers, AI-search (GEO) visibility, launch-video rendering, GA4/Search Console analytics, and its own MCP server — delivered as a multi-tenant, Stripe-billed SaaS with a web app, edge worker, and browser extension.