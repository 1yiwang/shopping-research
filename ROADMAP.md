# Roadmap

`shopping-research` is a young skill. v0.1 is the first public release. This document captures the design choices behind it and where it is heading.

The README has the short list. This file has the *why* and the open questions.

## 1. Phased architecture

Four implementation shapes were considered for this workflow:

| Phase | Approach | Status |
|---|---|---|
| **A** | Pure agent + Markdown template — zero code, every search is a fresh chat | superseded |
| **B** | **Cursor Skill** — structured workflow, reusable across projects | **current (v0.1)** |
| **C** | Python script + Playwright — real scraping, live prices, stock, login walls | planned (v1.0) |
| **D** | Visual workflow (n8n / Make) — drag-and-drop, triggerable | possible alternative |

### Why B first

- Zero infrastructure, zero hosting cost.
- The agent can already do public-page searching well enough for the first few use cases.
- Keeps the schema and prompt close to the agent that consumes them.

### What B cannot do

- Login walls (BestSecret, members-only sales, etc.).
- Live stock by size — the report cannot tell you "size 36 is sold out".
- Sub-minute price changes (Vinted listings, flash sales).
- Programmatic monitoring or scheduling.

### What v1.0 (C) will add

- A Python package shipped with the skill (`scripts/`).
- Per-site adapters using Playwright for the few sites that block scraping or hide prices behind login.
- Live size-availability check at search time.
- Optional headless run from CI / cron for scheduled requests.
- Same `request-template.yaml` input — only the **Fetcher** layer changes.

The path B → C is intentional. v0.1 stabilizes the schema, the prompt logic, and the report format. v1.0 only swaps the search step, not the design.

Phase D (n8n / Make) is kept on the table for a future "non-Cursor user" entry point. Same schema, different runner.

## 2. Input format evolution

Today the input is a YAML file. Good for repeatability, not great for ergonomics.

| Stage | Form | Friction |
|---|---|---|
| **Now** | Edit `request-template.yaml`, save under `requests/`, hand the path to the agent | Need to read the field docs first |
| **Next** | Free-text paragraph in chat → the skill parses it into the schema and asks only for what is missing | Lowest friction; most natural |
| **Later** | A small hosted web form on the maintainer's site that emits the YAML | Shareable link; no Cursor needed to author a request |

### Stage 2 example (free text → schema)

A future user can simply say:

> *"I'm a recent grad job-hunting for finance and consulting roles in Switzerland. Need a black commuter bag, no logo, must hold A4, budget under 100 CHF."*

The skill parses this into the YAML, asks only for the 1–2 missing fields (preferred material, secondhand OK?), and runs. The schema stays the same — only an "intake" step is added inside `SKILL.md`.

### Stage 3 example (hosted form)

A static page (e.g. on the maintainer's personal site) with the schema rendered as form fields. On submit, it generates the YAML and either downloads it or copies it to the clipboard. Anyone — Cursor user or not — can produce a valid request.

The free-text intake from Stage 2 can also live on this page as a single textarea, parsed client-side via an LLM call.

## 3. Site coverage

### Currently in `site-registry.md`

Switzerland-delivery defaults:

- Uniqlo, Mango, H&M, Zalando.ch, COS, About You, Massimo Dutti, Arket, & Other Stories, Vinted CH, Zara.

Conditional / opt-in:

- Amazon.de / Amazon.it, Asos, Vestiaire Collective.

### Planned to add

| Store | Why | Tier |
|---|---|---|
| Sézane | French wardrobe-staple, ships CH; on-brand for "quiet luxury" briefs | balanced / image |
| The Frankie Shop | Minimal contemporary, ships CH | image-first |
| Everlane | Basics with material transparency; useful for value tier | value / balanced |
| Net-a-Porter / Mytheresa Sale | Investment pieces during sale windows | image-first |
| Zalando Lounge | Flash sales on existing brands; CH delivery | value |
| Galeria.ch / Manor.ch / Globus.ch | Swiss department stores; useful for shoes and outerwear with local return windows | balanced |
| Ssense Sale | High-end second-hand-feel sales for accessories | image-first |

### Excluded by design

- **Shein** — image mismatch with the most common briefs (professional, quiet luxury, sustainable). Material and durability also fail any "investment piece" check. Re-evaluate only if the skill grows a dedicated fast-fashion preset.
- **Temu** — same concerns as Shein, plus marketplace counterfeit risk.
- **BestSecret** — login wall; the agent cannot browse without an authenticated session. Becomes accessible once Phase C (Playwright + cookie injection) lands.

### Region presets to expand

- **UK** — swap CH defaults for `.co.uk` domains; add M&S, John Lewis Own-Brand, & Other Stories UK.
- **Germany / EU** — same brands, `.de` domains; add Asos as a default rather than opt-in.
- **Singapore / Hong Kong** — add Lazada, Zalora, GU Online; remove Vinted (limited inventory).

## 4. Open design questions

These are decisions the skill currently makes implicitly. They will be revisited as more real shopping rounds expose data:

- **Tier weights**. Balanced uses 40% price / 30% material / 30% image fit. Should image-fit weight grow for "investment piece" briefs? Should sustainability priority modify the formula?
- **Second-hand handling**. Vinted listings are individual and volatile. Should the report ever link a single listing, or always link a search URL with the right filters?
- **FX freshness**. Today the FX rate is stamped in the report footer once. For high-volatility periods, fetch live and warn if movement &gt; 2% in 24h.
- **Sizing memory**. After 5–10 runs, the skill could keep a private `sizing-history.md` per user — what fit, what didn't, brand-by-brand — to refine future picks. This file lives **locally only**, never in the public repo (already excluded via `.gitignore`).
- **Brief presets**. Frequent brief shapes (banking interview, consulting interview, fintech smart-casual, evening-out) could be templates that pre-fill `image_keywords` and `must_have`.

## 5. Pair with a downstream skill

Once a candidate is selected from a `shopping-research` report, the user often wants:

- **Price-drop alerts** — a small `price-watch` skill, cleanly separate, that takes the chosen URLs as input. The reference [openclaw shopping-price-drop-coupon-scout](https://github.com/openclaw/skills) is a working example of that downstream stage.
- **Coupon / promo code scout** — same skill, or a sibling.

These stay as separate skills rather than being bolted onto `shopping-research`. Each one focused, each one replaceable.

## 6. Versioning and release notes

The repo uses lightweight semver. Each user-visible change ships in a commit on `master` with a `feat:` / `fix:` / `docs:` prefix. Breaking schema changes will bump the minor (0.1 → 0.2) and document the migration in this file.

Release notes for each version live in commit history; a `CHANGELOG.md` will be added once there are 3+ versions worth tracking.
