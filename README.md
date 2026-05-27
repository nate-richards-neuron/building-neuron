# Building Neuron

A case study in shipping a real consumer product as a non-technical founder, using AI tooling to execute on product decisions.

---

**Who I am:** Nate Richards — sole founder of Neuron. I don't write code. I write specs, make architecture and product decisions, run customer and legal work, and orchestrate AI tools (Claude Code for backend and extension, Lovable for dashboard and web UI) to produce shipping software.

**What Neuron is:** An AI-first shopping assistant that lives as a Chrome extension and web dashboard. The differentiators are bulk shipping threshold optimization — helping users hit free-shipping minimums by bundling intelligently across stores — and cross-retailer cart consolidation. Currently live at [neuroncart.com](https://neuroncart.com).

**What this repo is:** Not the source code — that stays private. This is the operating layer behind the product. The specs that produced shipping features, the architectural decisions and the lessons that taught them, the strategy work that determines what gets built and what gets cut, and the process by which a non-technical founder ships technical product.

## Why this repo exists

Most engineering portfolios show code. This one shows judgment.

If you're hiring for an operating role — Chief of Staff, GM, Head of Operations, Operating Partner, Founder-in-Residence — what you actually need to know is whether someone can:

- Make decisions under uncertainty with incomplete information
- Sequence work so the right things ship in the right order
- Cut features that aren't earning their keep, even after building them
- Hold a coherent product strategy across product, engineering, marketing, and legal
- Communicate clearly enough across functions that nothing gets lost in translation
- Be self-aware about tradeoffs and willing to talk about what hasn't worked

Each section of this repo is structured to show one of those, with real artifacts from real product decisions on a live product.

## How to read this

You probably don't want to read it top to bottom. Pick the section that maps to what you're trying to evaluate:

- **Want to see how I think about a feature before it's built?** → `specs/`
- **Want to see how I reason about architecture tradeoffs?** → `decisions/`
- **Want to see how I think about the business?** → `strategy/`
- **Want to see how I actually work day-to-day?** → `process/`

## Repo structure

**`specs/`** — Sanitized feature specs from features that shipped. The format mirrors what I actually use day-to-day. Each one walks from problem statement → user story → success criteria → components → data model → edge cases → what got cut and why.

**`decisions/`** — Architecture decision records. The hard-won lessons that now define how new work gets scoped. Most of these started as bugs that took a day to track down; they exist as documents so the next person doesn't lose that day.

**`strategy/`** — Product and business thinking. Wedge analysis, customer discovery plan, monetization model, competitive positioning. Includes things I changed my mind on.

**`process/`** — How I work. Build sequencing across two AI tools, sanitized examples of the prompts that produce shipping features, and an honest learnings doc covering what I'd do differently.

## What's been shipped (operating snapshot)

A non-exhaustive list, to give a sense of scope:

- **Chrome extension** — Side Panel API surface, Shadow DOM contextual banners on supported stores, AI extraction fallback for unsupported stores, snapshot-based cart diffing, checkout interception
- **Web dashboard** — Multi-page React/Vite app with a structured design system, AI chat surface with 30+ backend tools
- **Backend** — Supabase with row-level security, multiple cron pipelines, edge functions handling auth, email, and notifications, instrumentation tables for usage analytics
- **Live features** — Price alert pipeline (cron + email digests + push notifications), smart credit-card matching at checkout, order tracking across multiple carriers with Gmail integration, savings dashboard, product search powered by web search APIs, referral system, customer support ticketing
- **Admin tooling** — Internal dashboard for user management, audit logging, instrumentation review
- **Off the product** — Trademark filings (USPTO Classes 9, 35, 42), LLC formation, domain and Workspace infrastructure, Chrome Web Store listing

A meaningful amount of the work above is decisions about what *not* to build. Those are documented in `decisions/deferred-features-rationale.md`.

## Contact

richards.natet@gmail.com  
[LinkedIn](https://www.linkedin.com/in/nate-richards-26978329)  
[Medium](https://medium.com/@richards.natet)
