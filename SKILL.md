---
name: shopping-research
description: >-
  Research and compare clothing/accessories across European retailers (Zalando,
  Uniqlo, Mango, H&M, COS, About You, Vinted, etc.) for a structured user
  profile. Use when the user asks for purchase recommendations for specific
  items (white shirt, heels, coat, work bag, etc.), wants cross-store
  comparison under a budget, or asks to "find me X" with image/style
  constraints. Outputs a Cursor Canvas with 3 tiers per item (value /
  balanced / image-first), spec comparison, and risk notes.
---

# Shopping Research

Discover and compare candidate products across multiple European retailers for a structured shopping request, then deliver a Cursor Canvas report.

This skill is the **upstream / discovery** stage. For downstream price tracking on a chosen URL, pair with a price-watch skill.

## When to use

- User asks "help me find a [garment]" with style or budget constraints.
- User wants cross-store comparison for a specific item under a budget.
- User has a clear image/profile (career stage, dress code, size) to inform filters.
- User wants a reusable, repeatable shopping workflow.

## When NOT to use

- The user has already chosen a specific product URL and wants price tracking → use a price-watch skill.
- The user wants to auto-purchase or auto-add-to-cart → out of scope.
- The user only needs general fashion advice without a buy intent.

## Required inputs

A request YAML following [`request-template.yaml`](request-template.yaml). Minimum fields:

- `context` — life situation driving the purchase (e.g. "Zurich grad, job-hunting in finance/consulting").
- `image_keywords` — 3–5 style descriptors (e.g. "minimal", "post-student polish", "quiet luxury").
- `delivery_region` + `currency` — e.g. `Switzerland` / `CHF`.
- `items[]` — for each item: `type`, `must_have`, `nice_to_have`, `avoid`, `budget`, `size`.

If the user hasn't provided a request file, build one with them in chat first by asking the missing fields. Save the filled file under `requests/<YYYY-MM>-<short-tag>.yaml` next to the canvas output.

## Workflow

Copy this checklist and track progress:

```
- [ ] 1. Lock the request YAML (collect missing fields from user)
- [ ] 2. Pick stores from site-registry.md based on delivery_region + budget
- [ ] 3. For each item: 6–10 candidate searches across selected stores
- [ ] 4. Normalize candidates: price → request.currency, size, material, fit
- [ ] 5. Rank into 3 tiers per item (value / balanced / image-first)
- [ ] 6. Render Cursor Canvas using the structure in report-template.md
- [ ] 7. Add risk notes: stock volatility, sizing quirks, return policy
```

### Step 1 — Lock the request

If the user is mid-conversation, summarize their stated needs back into the YAML schema and ask only for **missing critical fields** (size, budget per item, region). Don't re-ask what they already gave.

For "image_keywords", if the user shared a portfolio site or CV, derive 3–5 keywords from it (job target, dress code, existing pieces) before asking. Show them your derived keywords and let them adjust.

### Step 2 — Pick stores

Use [`site-registry.md`](site-registry.md). The default Switzerland-delivery shortlist is:

- Uniqlo.ch, Mango.com, H&M.ch, Zalando.ch, COS, About You, Vinted

Drop stores that don't fit the brief (e.g. drop Shein for any "professional/quiet luxury" brief; drop fast-fashion entirely if the brief says "investment piece").

### Step 3 — Candidate searches

For each item, run 6–10 web searches. Search query template:

```
<store domain> <item type> <gender> <key fit attribute> <color> <budget ceiling>
```

Examples:
- `zalando.ch white shirt women slim fit cotton 60 chf no logo`
- `uniqlo switzerland women supima cotton shirt easy care`
- `mango women block heel pump leather black 80`
- `vinted ch massimo dutti pumps 37`

Capture for each candidate:
- store, brand, product name, URL
- price (in store currency + CHF estimate if different)
- material, fit/silhouette, color, available sizes (if visible)
- a one-line "why this matches"

### Step 4 — Normalize

- Convert all prices to `request.currency` using the spot rate at runtime (note the rate in the canvas footer).
- Translate fit terms: "regular fit" / "slim fit" / "fitted" / "tailored" → unified vocabulary.
- For shoes, capture heel height in cm and heel type (block / stiletto / kitten / wedge).

### Step 5 — Rank into 3 tiers per item

Always give the user **3 candidates per item**, one per tier:

| Tier | Goal | Picking rule |
|------|------|--------------|
| **Value** | Lowest acceptable cost that still passes all `must_have` | Lowest price among candidates passing `must_have` |
| **Balanced** | Best price/quality trade-off | Highest score using weights: price 40% / material 30% / image fit 30% |
| **Image-first** | Best image fit even at top of budget | Highest image fit at or under budget ceiling |

If two tiers would produce the same product, broaden the candidate pool until they differ.

### Step 6 — Render Cursor Canvas

Use the structure described in [`report-template.md`](report-template.md). Save the canvas to:

```
~/.cursor/projects/<workspace>/canvases/shopping-<YYYY-MM>-<short-tag>.canvas.tsx
```

Filename example: `shopping-2026-05-grad-jobsearch.canvas.tsx`.

### Step 7 — Risk notes

Always include in the canvas footer:

- "Prices and stock as of `<date>`. Verify size availability before checkout."
- For Vinted / second-hand: "Single listings; sold out fast. The shown URL may be gone — keyword fallback included."
- For sizing-quirky brands (Mango runs small, Uniqlo runs true to Asian sizing): brand-specific note.

## Operational notes

- **No login walls.** Skip stores that require login to see prices (e.g. BestSecret) unless the user provides a manual link.
- **No scraping that violates ToS.** Use web search and public product pages only. Treat all data as suggestions.
- **No payment, no checkout.** This skill recommends; the user buys.
- **Honest about freshness.** Always surface "as of <date>" in the canvas.
- **Reuse:** The same skill handles future requests by swapping the request YAML — do not modify the SKILL.md per use case.

## Output format

A single `.canvas.tsx` file (see [`report-template.md`](report-template.md) for the section structure) plus the filled request YAML saved alongside. The chat response should be a short summary pointing to the canvas + the top recommendation per item.

## Safety

- Never store user payment details, addresses, or login credentials.
- Don't recommend products from prohibited / suspicious sellers (no marketplace scams).
- For second-hand items, always remind the user to verify the seller's rating and item photos before purchase.

## Related files

- [`request-template.yaml`](request-template.yaml) — input schema with field docs
- [`site-registry.md`](site-registry.md) — curated retailer list with CH/EU notes
- [`report-template.md`](report-template.md) — canvas structure spec
