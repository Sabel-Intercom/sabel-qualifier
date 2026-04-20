# Sabel Engagement Qualifier

Internal AE-facing qualifier for full Sabel engagements across the four service pillars. Companion tool to the Migr8Now qualifier (migration deals are routed out via a red callout).

## Pillars covered

- **Transformation Blueprint** — AI operating model, KPIs, governance
- **Intercom Foundations** — workspace, inboxes, routing, workflows
- **Automation Engine** — Fin procedures, data connectors, integrations
- **Fin Enablement** — Fin setup, knowledge, training, launch

**Migration Accelerator** is handled separately via the Migr8Now qualifier.

## Architecture

Same stack as the Migr8Now qualifier:

```
GitHub Pages (static form)
  ↓ POST
Vercel serverless (api/submit.js)
  ↓
Upstash Redis (persistence) + Resend (email)
```

## Deployment

### 1. Repo setup

Push to `sabel-intercom/sabel-qualifier`.

### 2. GitHub Pages

Enable Pages on `main` root. Form served at `https://sabel-intercom.github.io/sabel-qualifier/`.

### 3. Vercel

Import the repo into Vercel. **Reuse the same Upstash and Resend credentials from the Migr8Now qualifier** — they're namespaced by key prefix (`sabel-qualifier-lead-*` here vs `migr8now-lead-*` there), so no collisions.

Environment variables:

| Variable | Value |
|----------|-------|
| `UPSTASH_REDIS_REST_URL` | same as Migr8Now qualifier |
| `UPSTASH_REDIS_REST_TOKEN` | same as Migr8Now qualifier |
| `RESEND_API_KEY` | same as Migr8Now qualifier |
| `FROM_EMAIL` | `Sabel Qualifier <noreply@sabelcustomersuccess.com>` |

### 4. Wire the form

Update `API_ENDPOINT` in `index.html` to match your Vercel URL.

## Pricing model (calibrated against real signed SOWs)

Internal rate: **USD $200/hr** (never disclosed to client).

| Engagement | Actual | Qualifier estimate | Delta |
|---|---|---|---|
| WorkInitiatives (Feb 2026) | 31 hrs | 29 hrs | -6% |
| Cirqul Phase 1 (Apr 2025) | 48 hrs | 53 hrs | +10% |
| Education Perfect (non-migration) | 50 hrs | 60 hrs | +20% |
| Huuuge Global (non-migration) | 95 hrs | 78 hrs | -18% |

All within plus or minus 20% — appropriate spread for a pre-discovery estimate carrying the asterisk caveat.

### Transformation Blueprint
- Base: 8 hrs — only activates when AI operating model is explicitly "No" (rare standalone)
- KPI framework: +4 hrs
- Multi-brand scoping: +3 hrs

### Intercom Foundations
- Base: 5-7 hrs depending on workspace state
- 6+ inboxes: +3 hrs
- Workflow build (1-3 workflows): +5 hrs
- Workflow suite (4+ workflows): +9 hrs
- Multi-brand routing: +10 hrs

### Automation Engine
- Base: 3 hrs (register review + plan)
- 1-2 connectors: +8 hrs
- 3+ connectors: +12 hrs
- 1-3 Fin procedures: +6 hrs
- 4+ Fin procedures: +16 hrs

### Fin Enablement
- Base: 10 hrs (no Fin) or 6 hrs (Fin tuning)
- 50-200 articles: +14 hrs
- 200+ articles: +22 hrs
- 2-3 languages: +6 hrs
- 4+ languages: +15 hrs
- 50+ agents: +2 hrs
- Multi-brand Fin: +6 hrs

### Engagement minimum
- **30 hours** — below this triggers an amber warning on the form

## Data persistence

Keys: `sabel-qualifier-lead-{company-slug}-{SHORTID}`

## Recipients

- `richard@sabelcustomersuccess.com` (always)
- `akbur.ghafoor@intercom.io` (always)
- AE email if provided

## Updating the pricing model

If a new engagement's actual hours differ from the original estimate by more than ~15%, review which line items were off and update the relevant band in `recommendPillars()` inside `index.html`. Keep aligned with the `sabel-pricing-model` skill (source of truth).
