# Canvas report structure

Render the report as a single `.canvas.tsx` file under `~/.cursor/projects/<workspace>/canvases/shopping-<YYYY-MM>-<tag>.canvas.tsx`. Read the `canvas` skill before writing the file.

## Required sections (in order)

1. **Header** — `<H1>` with one-line title (item count + budget summary). `<Text tone="secondary">` with the request context.
2. **Profile chips** — a `<Row>` of `<Pill>` showing the image keywords, sizes, and total budget.
3. **Per-item section** — repeat for each `items[i]` in the request:
   - `<H2>` with the item name and "Top 3 picks · budget X CHF".
   - A `<Grid columns={3} gap={16}>` with three `<Card>` blocks: **Value · Balanced · Image-first**. Each card:
     - `<CardHeader>` plain text with the brand + product name.
     - `<CardBody>` with: price (large), key specs (`<Text tone="secondary">`), why-this-matches one-liner, and a `<Link>` to the product page.
   - A `<Table>` directly under the grid for spec comparison: columns = Brand · Price · Material · Fit · Heel cm (shoes) · Color · Why.
4. **Cross-item sanity check** — `<Callout tone="info">` summarizing total spend if user buys the **balanced** tier across all items, vs. the budget ceiling.
5. **Risks & next steps** — `<Callout tone="warning">` with:
   - Stock/size verification reminder
   - Brand-specific sizing notes
   - Which links are second-hand single listings (volatile)
6. **Footer** — `<Text tone="tertiary" size="small">` with: data freshness date, FX rate used, store list searched.

## Component / token rules

- Use `useHostTheme()` for any custom color. No hardcoded hex.
- One `<H1>` per canvas. No `<H1>` or `<H2>` inside `<CardHeader>`.
- Tier cards use a single `<Pill>` in `CardHeader.trailing` with tone:
  - Value → `tone="success"`
  - Balanced → `tone="info"` `active`
  - Image-first → `tone="warning"`
- Prices: render the number large via `<Stat value="59 CHF" label="Mango" />` inside the card body, OR a styled `<Text size="body" weight="bold" style={{ fontSize: 20 }}>`. Pick one and stay consistent.
- Don't wrap headings in cards. Don't nest cards.
- Include a **legend** for any pill tones used so the user knows what they mean (one row at the top of per-item sections is enough; or one global legend after the profile chips).

## Self-check before delivering the canvas

- [ ] Every link is a real product URL (or a clearly-labeled search-results URL when single listings are not citable, e.g. Vinted).
- [ ] Every price has a currency.
- [ ] Every spec table column is labeled.
- [ ] No empty cards / placeholder text.
- [ ] Footer states the "as of" date and FX rate (if cross-currency).
- [ ] Total balanced spend is shown vs. budget ceiling.
- [ ] No emojis, no gradients, no box-shadows.

## Chat response after delivering the canvas

Keep it short (≤6 lines):

1. Path to the canvas file.
2. Per item: 1 line "top recommendation = ___" with a reason.
3. Anything to verify before checkout (size, ship-to country selector).
4. If first canvas in workspace: one sentence on what a canvas is.
