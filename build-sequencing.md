# Build Sequencing

How a non-technical founder ships a multi-surface product (Chrome extension, web dashboard, Supabase backend) using AI tooling.

The short version: two AI tools, strict lane discipline, backend-first ordering, and verification rituals between every handoff. The point of this doc isn't to argue this is the right way — it's to make the actual mechanism legible, because the operating discipline matters more than the tooling choices.

## The two tools and their lanes

**Claude Code** — backend work (Supabase schema, edge functions, RPCs, migrations) and Chrome extension work (manifest, service worker, content scripts, side panel surfaces).

**Lovable** — web dashboard (React/Vite, pages, components, design system) and the public marketing site.

These never overlap. If a feature touches both surfaces — which most non-trivial features do — Claude Code finishes its work first, the changes are confirmed live, and only then does the Lovable prompt get written and run.

## Why they never overlap

The temptation when shipping fast is to parallelize: kick off backend work and dashboard work simultaneously, save calendar time. This breaks for two reasons.

First, the two tools don't share state. They each hold their own view of the codebase and their own assumptions. If both are editing toward an imagined contract that doesn't exist yet, by the time they meet there are inconsistent column names, mismatched API shapes, and silently broken assumptions on both sides. Reconciling that costs more than what the parallel work saved.

Second, the dashboard depends on data the backend defines. Building dashboard UI against a schema that might still change is building UI that will be rewritten. Sequential ordering means the dashboard is built against a settled contract, not a moving target.

## Build order within a feature

For any feature that touches both backend and frontend:

1. **Schema first.** Tables, columns, RLS policies, GRANTs. Verified against live database state via `information_schema` before any migration is written.
2. **Backend logic second.** Edge functions, RPCs, triggers. Tested in isolation with curl or the SQL editor before any UI is touched.
3. **Extension wiring third.** The extension reads from and writes to the now-stable contract.
4. **Dashboard UI last.** Lovable gets a prompt that names the exact tables, columns, and endpoints already in place.

The first step has the longest leverage — every later step is downstream of schema decisions made on day one. Which is exactly why schema decisions get the most scrutiny and the longest think time.

## Verification rituals between steps

A few things are non-negotiable between handoffs:

- **Schema verification before migration.** Query `information_schema.columns` against the live project before writing any migration that touches existing tables. Assumed column names are how silent failures happen.
- **Permission verification after migration.** Check both RLS policies *and* table-level GRANTs after creating any anon-writable table. GRANTs are checked before RLS, so missing them produce 403s with zero RLS evaluation — debuggable only if you know to look there.
- **Endpoint confirmation before the dashboard prompt.** Before writing a Lovable prompt that says "call this RPC and render the result," that RPC has been called manually with a real auth token and the response shape is known.

None of these are exotic. They're cheap. The reason to make them rituals rather than judgment calls is that skipping them is invisible until something breaks at the worst time.

## What this approach trades

Honest about the costs:

- **Slower than a small engineering team.** Two engineers working in parallel ship faster than this sequencing does.
- **Faster than a non-technical founder doing it serially without tooling.** Dramatically so — what this delivers in a week would otherwise take months.
- **Brittle if either tool changes substantially.** The workflow assumes specific capabilities. Major changes to either tool require reworking the playbook.
- **Requires real schema literacy from a non-technical operator.** The discipline only works if the human in the loop knows enough to evaluate whether a migration is safe before running it.

That last one is the actual skill being demonstrated here. The AI tools execute. Sequencing, schema decisions, and verification are the operator's job — and the parts that don't get easier with better tooling.
