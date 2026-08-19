# Cover Letters Data — Internal Reference

> Internal portfolio reference for writing Upwork cover letters, LinkedIn outreach, and proposals.
> Not client-facing. Internal notes, attributions, and guardrails are intentional.

---

## AI Integration & LLM

Hands-on experience integrating LLMs into production platforms: exposing platform tools to AI agents for CRUD operations, performance optimisation, and multi-provider support so clients can swap between OpenAI, Claude, and Gemini without re-architecting.

**Live demo of our AI integration work:**
https://www.loom.com/share/43c7eae8aa0d4e9ba5a9691f0d56f88d
A walkthrough of an MCP server we built that exposes a customer's live accounting books as tool calls to GPT, Claude, Gemini, Perplexity, and Dify, with multi-provider switching.

**We also ship AI products, not just AI features:**
Morket (https://morket.io) — multi-tenant AI marketing platform with a provider abstraction over OpenAI (GPT-4o, gpt-image-1, embeddings) and Anthropic (Claude Sonnet/Opus/Haiku), an agentic tool-loop chat assistant, RAG over ingested docs, SSE streaming on every generator, and **its own MCP server** exposing Morket's tools to external LLMs with OAuth + issuable API keys.

**Platform migrations off AI/no-code builders (credibility signal for "rescue my Lovable/Bolt/v0/Cursor/Manus/Figma Make MVP" jobs):**
- A Manus-built platform taken fully to production
- A Figma Make build taken to production *(DLVRD — https://www.dlvrd.io/ — started from a Figma Make design export, now a production media-delivery SaaS; 300+ users within weeks of launch. Safe to cite by name.)*
- A Base44-built app migrated to production stack with zero data loss *(HeyFern — internal reference only; do not cite by name in proposals or outreach)*

---

## Development

Primary stack: Node.js, React, Next.js. Production apps shipped on this stack.

**Team / agency walkthrough:**
https://www.loom.com/share/e8adb22449f245e2aed4ae129ffcd893
Overview of our development team and current work.

### HalalAccounts — https://halalaccounts.com/
**One-liner:** Multi-tenant SaaS accounting and business management platform for Muslim-owned SMBs, with Zakat-aware double-entry bookkeeping, multi-provider open banking, payroll, HR, project management with time tracking, document management, and an MCP server that exposes the customer's live books to GPT/Claude/Gemini/Perplexity/Dify.

**Stack:** React 19 + Vite SPA, Express 5 + Prisma 7 + PostgreSQL API (~591 routes, 94 models), Next.js 16 super-admin, Chrome MV3 extension. Tailwind v4, Zustand, TanStack Query. JWT + Google OAuth. Docker Compose behind HAProxy, MinIO, Redis, Postfix, GitHub Actions. ~240k lines TypeScript across a 4-app monorepo.

**Use for:** SaaS accounting/fintech, multi-tenant B2B, Stripe billing, accounting/ledger logic, AI/MCP integration, large TypeScript monorepos, halal/Islamic finance.

**Figma file (corrected link — note: the project's Figma file lives under the Tabadulat workspace, this is intentional):**
https://www.figma.com/design/8RlTFFQBTppwMbt7EEsi8q/HalalAccounts-Design?t=fstb2nQNFWs3BMUu-11

### DoxOptima — https://www.doxoptima.com/
**One-liner:** Multi-tenant dental clinic and dental lab management platform: clinical records, appointments, lab order workflow engine, double-entry ledger accounting, commission engine, payroll with biometric attendance, plus an Electron desktop app with offline SQLite and ZKTeco fingerprint device integration.

**Stack:** Express + Prisma + PostgreSQL API, Next.js 16 (clinic/lab web app), Next.js 14 (super-admin), Electron desktop. Stripe-gated plans. Three languages (English, Italian, Swedish). Voice assistant API for phone-based bookings.

**Use for:** Healthtech, multi-tenant SaaS, workflow engines, ledger/financial logic, hardware integration (biometric/IoT), Electron desktop with offline sync, regulated/compliance-aware industries.

**Figma file:**
https://www.figma.com/design/7fZkiNQOA6YDwXnoOuxsnK/DOX---Clinic-App?node-id=0-1&p=f&t=YKA0pVVbIijasRxY-11

### DLVRD — https://www.dlvrd.io/
**One-liner:** Premium video delivery platform and client gallery engine for filmmakers and production agencies: creators upload large masters, DLVRD generates streaming proxies and thumbnails, and the creator ships a branded cinematic gallery on one secure link that clients open with no account. Finished work archives to a cheap cold-storage Vault tier. Started from a Figma Make design export and went to production. **300+ users within weeks of launch.**

**Stack:** React 19 + Vite 8 + TypeScript SPA, Express 5 + Prisma 7 + PostgreSQL API (~160 REST endpoints, 23 models, 37 migrations), BullMQ workers on Redis. FFmpeg proxy compression, Sharp thumbnails, dedicated RAW develop queue. MinIO hot storage + R2/S3 cold Vault, presigned multipart uploads. Stripe subscriptions plus one-time storage blocks. ClickHouse bandwidth analytics. Docker Compose behind HAProxy, GitHub Actions across main/staging/uat/production, split API + worker deploys.

**Use for:** Media/video SaaS, large-file upload and streaming, background job queues and worker fleets, FFmpeg/transcoding, S3-compatible and tiered storage, creator tools, Stripe subscriptions plus usage add-ons, no-code rescue jobs (Figma Make specifically), fast-traction launches.

### Morket — https://morket.io
**One-liner:** Multi-tenant AI marketing platform positioned as a "marketing co-founder" for technical founders. A founder pastes a product URL; Morket scrapes and reads the site like a marketer, builds brand and strategy foundations, then runs the actual work across launch, SEO (including programmatic), content, social publishing, directories, AI-search visibility, and analytics from one workspace, with an agentic weekly "Mission" that decides what to do next.

**Stack:** Vue 3 + Vite + TypeScript SPA (dark-mode design system), Bun + Elysia API (~180 endpoints, ~12,700-line service), PostgreSQL via Drizzle, Redis cache and job queue. OpenAI + Anthropic provider abstraction with an agentic tool-loop, embeddings-backed RAG, SSE streaming on every generator. Remotion launch-video rendering, Chrome MV3 comment assistant, MCP server with OAuth and API keys. Stripe subscriptions. Integrations: LinkedIn, X, Reddit, GitHub, GA4, Search Console, Webflow, WordPress, Sanity, Framer, DataForSEO. Bun-workspaces monorepo, Docker Compose, Railway/Vercel.

**Use for:** AI/LLM products (not just AI features), MCP and agent tooling, multi-provider LLM architecture, RAG, streaming UIs, marketing/SEO tech, multi-tenant B2B SaaS, Vue work, browser extensions, heavy third-party OAuth integration work.

### HeyFern — https://heyfern.app/
**One-liner:** Production AI somatic therapy web app for Anna Ferguson (author of *The Applied Nervous System Method*) — Claude Sonnet guides users through nervous system regulation based on a real-time 1–10 activation check-in, with voice input and TTS. Migrated entire production app off Base44 with zero data loss via custom ETL.

**Stack:** React + Vite + TypeScript, Hono (Node), PostgreSQL + Prisma (18 models), Better Auth (email + Google OAuth), Stripe, AWS S3, Nodemailer. 63+ REST endpoints, 28+ pages.

**Standout engineering:** AI safety layer (FernOutputGuards) with 7 issue-type detectors, 1–6 retry policy with corrective instructions, telemetry that auto-prunes at 5K events, admin dashboard for real-time policy adjustment. Atomic daily message limits via Serializable transaction isolation.

**Use for:** AI/LLM products, healthtech/wellness, AI safety and output validation, platform migrations off no-code (Base44, Lovable, Bolt, v0, Manus, Figma Make), complex admin panels, Stripe subscriptions, full-stack TypeScript.

**Internal note: no longer an active project — keep for internal reference only; do not include in proposals, cover letters, or outreach.**

### Shugaland — https://shugaland.com/
**One-liner:** Nigerian real estate SaaS — agent CRM, property listings, transaction workflows. Built on a React SPA with full on-page and technical SEO (meta tags, OG, schema, sitemap, location-targeted landing pages) plus bundle-size cuts for LCP. Backend on .NET.

**Use for:** React/Next.js, SEO, SPA-to-SSR migrations, B2B marketplaces, listing/directory platforms, real estate tech. A full 18-page SEO/PageRank/PageSpeed audit drove the roadmap and is available on request.

---

## SaaS

### Reference notes
- Decade-plus of SaaS design experience.
- Standard collaboration: client/team invited to Slack for daily updates, queries, revisions.
- Dribbble agency portfolio: https://dribbble.com/thesmallsquare
- SaaS-focused YouTube playlist (UX psychology, conversion, retention):
  https://www.youtube.com/playlist?list=PL3D6gNbm5p0nvzHebvYaVrCwVzlT0rFSq

### Figma files for SaaS work

**Arbling — AI/AR Jewelry Platform**
https://www.figma.com/design/AR24pIixqO8EQ3cTP6MWiM/Arbling?node-id=1-561&p=f&t=cCqB04Vikxkk8pLx-11
End-to-end product design across 9 deliverables: brand guide, SaaS dashboard, AR landing pages, mobile app (try-on, PDP, ecommerce, checkout), pitch deck, one-pager, 3D configurator page.

**Rhome — Property Co-Ownership Platform (PropTech)**
Case study: https://www.thesmallsquare.com/work/rhome
**One-liner:** End-to-end product design for a co-ownership platform where multiple people jointly buy and own one home (more affordable than buying alone, more permanent than renting). We designed both halves on one coherent design system: the marketing website that sells an unfamiliar model, and the web app that runs the full co-ownership journey.

Dribbble case studies:
https://dribbble.com/shots/25324586-Rhome-Website-Redesign-Case-Study


**Scope/features:** Marketing site led with "global homeownership simplified / more affordable than buying, more flexible than renting," fully responsive, typographic system (Urbanist + Inter Display) built to signal trust. Web app covered property listings (rich cards, filters, map browsing), **PropertyMatch** (pairs compatible co-buyers — the hardest trust problem in the product), in-app messaging/negotiation threads tied to each property/deal, a co-owner dashboard (properties, co-owners, documents, deal status at a glance), and document management. Deeper platform work included identity verification via Proof, an editable info repository, in-platform e-signatures (signer fields, initials, dates, custom text), a "sell your property" / swap-marketplace flow with equity calculation, and **RhomeAI** features — document summarization plus AI autofill of info fields from MLS data and uploaded documents, and an AI assistant for drafting contract language. Phased/milestone engagement; designs went live (platform.rhome.com, with US/Dubai/Portugal data).

**Use for:** PropTech/real estate tech, AI-in-product SaaS (document summarization, autofill, AI drafting), multi-party/multi-sided SaaS, marketplace + dashboard products, trust-heavy onboarding and identity verification, e-signature/document workflows, two-part scope (marketing site + product) on one design system, branding + web design + SaaS.

Figma: https://www.figma.com/design/cyPvycVBmxZx3zgVcR38BP/Rhome?node-id=1-504&p=f&t=ZozSYfSCCDqM6Cis-11
**Internal note:** for portfolio also have the Dribbble case study — https://dribbble.com/shots/25324586-Rhome-Website-Redesign-Case-Study (older guidance was to prefer Dribbble over rhome.com as the live link; the official case-study page above is now the primary reference).

**FreshQuote**
https://www.figma.com/design/CSNfhxekbl28ad0pWKsc2w/FreshQuote_v2-TSS?node-id=4383-22575&p=f&t=WWNvy8XbTdFfn017-11
SaaS quoting platform product UX.

**DoxOptima — Clinic App**
https://www.figma.com/design/7fZkiNQOA6YDwXnoOuxsnK/DOX---Clinic-App?node-id=0-1&p=f&t=YKA0pVVbIijasRxY-11

**Tabadulat — branding and bilingual mobile app (English + Arabic)**
https://www.figma.com/design/FSwYOM0Cyy8VKEDACsBCJu/Tabadulat-App?node-id=6171-25571&p=f&t=20GryKP9gsoUUBld-11
Helped them secure millions in funding. **Now live on the App Store and Google Play** — use as a "shipped, publicly available" credibility signal for mobile proposals.

### Visual SaaS case studies

- **Arbling AI/AR Jewelry Platform** — https://dribbble.com/shots/27222294-Arbling-AI-AR-Jewelry-Platform
- **Mattermost** — https://mattermost.com/ (Asaad was the first designer; PACE Award, 2022 TrustRadius Best of Awards)
- **VIPR AI** — https://www.behance.net/gallery/155583639/VIPR-AI-Web-and-Mobile-App (AI-powered web and mobile app)

### Mattermost concept demo videos (for workflow / incident-response / playbook job posts)

- AI-Generated Status Updates for Mattermost Playbooks: https://www.youtube.com/watch?v=A1Y1dH8lwXM
- Conditional Workflows: https://www.youtube.com/watch?v=bxbg2Di6TPU
- Automated Incident Mitigation in Mattermost: https://www.youtube.com/watch?v=oyMORI2sQGk
- Exportable Incident Audit Reports with Mattermost Playbooks: https://www.youtube.com/watch?v=lvMLkyeg9SY

### Focalboard
https://www.focalboard.com/
Led UX/UI design. Product Hunt recognition, FOSS award for project management.

---

## Motion Design

In-house motion designer's portfolio:
https://youtube.com/playlist?list=PLbh5wbuWUFp2Ulj2xBv7PwJKIhDWMDVr4&si=OteCDImzZ7vMoFAz

---

## Website & Landing Page Design

### YouTube credibility
Teaching conversion-focused web design, UX psychology, and Framer/Webflow to 39,000+ subscribers:
- Channel: https://www.youtube.com/c/AMDesignAndDev
- Conversion-focused playlist: https://www.youtube.com/watch?v=Mqi-VY_71p8&list=PL3D6gNbm5p0nvzHebvYaVrCwVzlT0rFSq

### Live sites (priority order)

**HalalAccounts** — https://halalaccounts.com/
Marketing site for the multi-tenant Halal accounting SaaS. Built on Framer.

**FigLearning** — https://www.figlearning.com/
Marketing site for Fig Learning — official Docebo partner helping SMBs get results from the Docebo LMS (strategy, implementation, adoption, and ongoing support). Positioned around measurable business outcomes (onboarding, compliance, knowledge capture, manager development, customer education) with solutions consulting, onboarding, customer education, help desk, and customer success. Designed and built in Framer. See fuller note in the Framer section.

**Letly** — https://letly.ai/
UK-based (London) AI-powered lettings operator that runs rental homes end-to-end — finding tenants, managing tenancies, and guaranteeing rent — across four service tiers (Guaranteed, Management, Placement, Self-Managed). Designed in Figma, built in Framer with dedicated SEO service pages. Pricing from 6% + VAT. Client: Cezar Rugasira (Letly Ltd). See fuller note in the Framer section.

**OzuraPay** — https://ozurapay.com/
Framer marketing site for a payments platform (onboard merchants, monetize payment volume, manage partner revenue) serving partners (ISOs/ISVs/PSPs), enterprises, and developers. See fuller note in the Framer section.

**Rhome** — https://www.thesmallsquare.com/work/rhome (case study)
Marketing website for the Rhome co-ownership platform, designed *and* developed by us in Webflow. Premium, fully responsive site built to explain an unfamiliar model (co-buying a home) with a "global homeownership simplified" hero, multi-page layout (incl. investors/press pages with forms), waitlist capture, and a Zapier integration piping form submissions to Google Sheets. Client: Bronson Hixon / Nolan (joinrhome.com). **Use for:** PropTech/real estate marketing sites, Webflow design+dev, startup landing pages selling a new/unfamiliar concept, form + automation (Zapier) work.

**FirstClassFlyer** — https://firstclassflyer.com/
One of our most complex Webflow builds. Memberstack (members, no count), n8n automations, Airtable-to-Webflow sync via Whalesync, GSAP, custom scalable search, 3,000+ CMS items. **Internal note: never say "2,000+ active members" — just "members."**

**Cerebral** — https://cerebral.framer.ai/
Our own Framer template aimed at AI SaaS and startups. Custom-built, currently sold on Contra/Framer marketplace.

**Quadra** — https://quadra.framer.website/
Our own Framer template for design studios and creative agencies. Custom-built, not from a marketplace template.

**Revidia** — http://revidia.com/
WordPress/Elementor marketing site.

**SecurityDocs** — https://securitydocs.de/
Marketing site for a German compliance/security documentation product. Framer.

**Productier** — https://productier.io/ (90+ PageSpeed)
Marketing site for an AI-powered bug/feedback intelligence platform for product teams. Framer build.

**Cairn (live demo)** — https://cairn-site-pied.vercel.app/
Live Vercel showcase: B2B cybersecurity / security operations marketing site (calm editorial positioning, platform and compliance narrative, trust and metrics blocks, customer proof). Use for premium SaaS landing pages, security/compliance positioning, and high-polish marketing-site credibility.

### Landing page scroll animations (only include if people ask for scroll-based landing pages)
**Maison Horlogère** — https://maison-horlogere.vercel.app/
Luxury watch website with scroll-driven animations. **Internal notes: list lower in the order**

### Dribbble landing page shots (priority order)

- Finexa Banking Landing Page: https://dribbble.com/shots/26354969-Finexa-Banking-Landing-Page
- Stackflow Landing Page: https://dribbble.com/shots/26217637-Stackflow-Landing-Page
- Revidia: https://dribbble.com/shots/25985263-Revidia
- Rhome: https://dribbble.com/shots/25324586-Rhome-Website-Redesign-Case-Study
- Nextronium Crypto Investment Platform: https://dribbble.com/shots/25698102-Nextronium-Crypto-Investment-Platform
- Arbling AI/AR Jewelry Platform: https://dribbble.com/shots/27222294-Arbling-AI-AR-Jewelry-Platform
- 13Cure Case Study: https://www.behance.net/gallery/215833537/13Cure-Case-Study

### Personal / course site
**Asaad Mahmood — Figma course** — https://www.asaadmahmood.com/courses/figma-noob-to-pro
Custom Framer build with code overrides, connected to Teachable's API to fetch course modules.

---

## Webflow

### Credibility
- Certified Webflow Expert and Webflow Partner.
- Featured on dark.design.
- Teach Webflow on YouTube (channel: https://www.youtube.com/c/AMDesignAndDev).
- Both designer and developer end-to-end: no design-to-Webflow handoff drops.

### Live Webflow sites

**FirstClassFlyer** — https://firstclassflyer.com/
Most complex Webflow build to date. Memberstack (members, no count), n8n automations, Airtable-to-Webflow sync via Whalesync, GSAP animations, 3,000+ CMS items, custom scalable search.

**Chemishield** — https://www.chemishield.com/
Marketing site for a hazardous waste segregation and compliance SaaS targeting EHS, lab ops, sustainability, facilities, and compliance teams. CLP-compliant QR labelling, full audit trail, multi-persona landing pages (EHS Teams, Lab Operations, Sustainability Leaders, Facilities, Compliance, Waste Vendors, Academia), plus ROI calculators.

**UpgradeFormula** — https://upgradeformula.webflow.io/
Marketing site for a personal-development / coaching brand ("Master Life's GPS"). Interactive "What do you want?" slider, media and reviews CMS, gated content with login and pricing flow.

**MrUpgrade** — https://mr-upgrade.webflow.io/
Personal brand site for the founder behind The Upgrade Formula, targeting high-achievers (Academy Award winners, professional athletes). Media and reviews CMS, custom typography and brand system.

**Mosque Template** — https://mosque-template.webflow.io/
Custom Webflow template for mosques and Islamic centres.

**TheSmallSquare (our agency site)** — http://thesmallsquare.com/
Built in Webflow.

### Webflow do-not-include list
Never include for Webflow-specific applications: HalalAccounts (Framer), Revidia (WordPress).

---

## Framer

### Credibility
- Certified Framer Expert: https://www.framer.com/@asaad-mahmood/
- 2 templates on Framer's official marketplace.
- Teach Framer on YouTube: https://www.youtube.com/watch?v=Q03vYJ5RNag&list=PL3D6gNbm5p0kGLaI2JObLHD0-CEulHDcA

### Our own Framer templates

**Cerebral** — https://cerebral.framer.ai/ (published)
AI SaaS / startup landing template. Sold on Contra.

**Quadra** — https://quadra.framer.website/
Creative studio / design agency template. Custom design + components.

**Gridhaus** — https://gridhaus.framer.website/
Framer template in progress. Sold on Contra.

**Pathfinderr** — https://pathfinderr.framer.website/ (published)
Published Framer template.

### Live Framer client sites

**Cicruit Garden** - https://spiky-line-352679.framer.app/
It’s a playful circuit-learning puzzle game for kids. Players connect nodes, place components like switches and resistors, and complete each circuit to turn on a grow light. When the circuit works, energy flows through the board and a seed blooms into life.

Custom Framer components in JavaScript, is where I do my deepest work. I built a full circuit-learning puzzle game as a single custom Framer component: drag-and-drop parts, live wire-drawing, a real circuit solver (union-find connectivity, logic gates, voltage math), Web Audio sound, and saved progress.

**Letly** — https://letly.ai/
UK-based (London) AI-powered lettings operator — a technology company that runs rental homes end-to-end: finding tenants, managing tenancies, and guaranteeing rent. Four service tiers landlords can move between: Guaranteed (rent paid every month, even through voids), Management (own it, don't run it), Placement (tenant-find only), and Self-Managed (platform-as-agent, coming soon). Pricing from 6% + VAT. We designed the marketing site in Figma and built it in Framer, including dedicated SEO service pages. Client: Cezar Rugasira (Letly Ltd). **Internal note:** also positioned as backed by investors from Revolut, AWS, Apollo, and Factorial Capital — verify before citing, not stated on the current live site.

**OzuraPay** — https://ozurapay.com/
Marketing site for a payments platform that lets operators onboard merchants, monetize payment volume, and manage partner revenue from one centralized platform. Three audiences: Partner (ISOs, ISVs, PSPs — embed, whitelabel, resell payment infrastructure), Enterprise (replace legacy payment stacks), and Developer (one API across every processor/gateway). Built on Framer.

**HalalAccounts** — https://halalaccounts.com/
Marketing site for the multi-tenant Halal accounting SaaS (Framer).

**FigLearning** — https://www.figlearning.com/
Marketing site for an official Docebo partner focused on SMBs — helping organizations move from buying learning technology to getting measurable results (onboarding, compliance, knowledge retention, leadership development, workforce readiness, customer education). Site sells the full Fig team offering: solutions consulting, onboarding, customer education, help desk/Harmony support, and customer success — backed by deep Docebo platform expertise (50+ implementations; 75% of team ex-Docebo). Designed and built in Framer with pricing tiers, customer stories, testimonials, and demo CTAs. **Use for:** B2B services marketing, LMS/edtech partner sites, multi-section SaaS-style landing pages, conversion-focused Framer design+dev.

**Productier** — https://productier.io/
Marketing site for an AI-powered bug/feedback intelligence platform (Framer, 90+ PageSpeed).

**SecurityDocs** — https://securitydocs.de/
Marketing site for a German compliance/security documentation product (Framer).

**Debook** — https://debook.app/
Complex Framer landing page. **Internal note: ALWAYS attribute as "Mateo Silva worked on it prior to me" if mentioning Debook.**

**Asaad Mahmood — personal + course site** — https://www.asaadmahmood.com/
Custom Framer build with code overrides, connected to Teachable's API to fetch course modules. Not a template.

### Do not use for Framer proposals
- Orion Funded (https://www.orionfunded.com/) — not custom work, do not include.
- "In the works" projects — only include when published.

---

## Mobile

### Credibility
Mobile designs used to secure millions in seed/Series A funding for clients.

### Live shipped mobile apps

**Tabadulat** — live on App Store and Google Play (use as shipped credibility signal).
Bilingual (English + Arabic) Islamic finance mobile app. Helped client secure millions in funding.
Figma: https://www.figma.com/design/FSwYOM0Cyy8VKEDACsBCJu/Tabadulat-App?node-id=6171-25571&p=f&t=20GryKP9gsoUUBld-11

### Mobile case studies

**Zentra Banking App** — https://www.behance.net/gallery/243021609/Zentra-Banking-App-Case-Study
Fintech / banking app case study. Strong narrative reference for mobile banking, fintech, or finance UI work.

**Arbling — AI/AR Jewelry mobile experience** — https://dribbble.com/shots/27222294-Arbling-AI-AR-Jewelry-Platform
End-to-end mobile experience: AR try-on, PDP, category browsing, purchase flow. Part of a 9-deliverable engagement also covering brand guide, SaaS dashboard, landing pages, pitch deck, one-pager, ecommerce, 3D configurator page.
Figma: https://www.figma.com/design/AR24pIixqO8EQ3cTP6MWiM/Arbling?node-id=1-561&p=f&t=cCqB04Vikxkk8pLx-11

**Rhome — mobile** — https://dribbble.com/shots/25324586-Rhome-Website-Redesign-Case-Study
Use Dribbble case study, not rhome.com.

### Mobile portfolio hub
https://www.behance.net/thesmallsquare/services/31101/Mobile-Application-Design

---

## WordPress

### Reference notes
- Design + dev services across Elementor, WordPress, Webflow, Framer, and custom code.
- Webflow Partner (separate, for Webflow-specific proposals).

### Live WordPress / Elementor / Divi sites

**TandemTrader** — https://www.tandemtrader.com/home-2/
Built on Divi. **Internal note:** main page is currently behind a waitlist. Always include for WordPress proposals as primary WP reference.

**Revidia** — https://revidia.com/
Elementor/WordPress.

**Garage Door Pros AU** — https://garagedoorpros.com.au/
Elementor/WordPress.

**Akison Locksmith** — https://akisonlocksmith.com/
Elementor/WordPress.

**Lichy** — https://lichy.io/
Elementor/WordPress.

### Portfolio hubs
- Landing page designs (Behance): https://www.behance.net/thesmallsquare/services/31097/Landing-page-design
- Full agency portfolio (Behance): https://behance.com/thesmallsquare
- Agency website (built in Webflow — Webflow Partner): http://thesmallsquare.com/

### WordPress do-not-include list
Never include for WordPress-specific applications: HalalAccounts (Framer), Rhome (Webflow).

---

## Next.js / Development-specific

### Live Next.js production work

**HalalAccounts** — https://halalaccounts.com/
Next.js 16 App Router super-admin panel + Express API + React 19 SPA. See full breakdown in Development section.

**DoxOptima** — https://www.doxoptima.com/
Two separate Next.js codebases (14 + 16) plus shared Express API and Electron desktop.

**Shugaland** — https://shugaland.com/
React SPA with full SEO implementation, agent CRM, listings. **Use for SPA-to-SSR migrations, SEO on React, directory/listing platforms, real estate tech.**

**HeyFern** — https://heyfern.app/
Full-stack TypeScript: React + Vite frontend, Hono backend, Prisma + Postgres, Stripe, AWS S3. **Internal note: no longer an active project — do not include in proposals or outreach.**

### Dev team walkthrough Loom
https://www.loom.com/share/e8adb22449f245e2aed4ae129ffcd893

---

## About / Agency credentials

- 10+ years collectively of shipped product experience.
- Full agency overview: https://thesmallsquare.com/
- Webflow Partner.
- Certified Framer Expert.
- Upwork Expert-Vetted (Asaad's profile).
- O'Reilly Figma course (published): https://www.oreilly.com/videos/figma-fundamentals-create/0790145626066/
- 39,000+ YouTube subscribers: https://www.youtube.com/c/AMDesignAndDev
- Mattermost: first designer; PACE Award, 2022 TrustRadius Best of Awards.
- Focalboard: led UX/UI; Product Hunt + FOSS award.

---

## Portfolio link shortcuts (most-used)

- Asaad personal Dribbble: https://dribbble.com/asaadmahmood
- The Small Square Dribbble: https://dribbble.com/thesmallsquare
- The Small Square Behance: https://www.behance.net/thesmallsquare
- YouTube: https://www.youtube.com/c/AMDesignAndDev
- Agency site: https://thesmallsquare.com/
- AI integration Loom: https://www.loom.com/share/43c7eae8aa0d4e9ba5a9691f0d56f88d
- Dev walkthrough Loom: https://www.loom.com/share/e8adb22449f245e2aed4ae129ffcd893
