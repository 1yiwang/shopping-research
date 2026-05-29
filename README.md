# shopping-research — Cursor Agent Skill

A reusable [Cursor Skill](https://cursor.com) for clothing & accessories shopping research across European retailers. From a structured request — item type, image keywords, budget, sizes — it produces a **Cursor Canvas** report with three tiered recommendations per item (*Value / Balanced / Image-first*), a cross-store comparison table, and risk notes.

> **What is a Cursor Skill?** A small Markdown file (`SKILL.md`) that teaches the Cursor agent how to perform a specialized workflow. Once installed under `~/.cursor/skills/`, the agent can reuse it in any project on your machine.

## What it does

- Takes a structured shopping request ([`request-template.yaml`](request-template.yaml))
- Searches across curated European retailers (Zalando, Uniqlo, Mango, H&M, COS, About You, Vinted, Massimo Dutti, Tamaris, …)
- Outputs a Cursor Canvas with **3 tiered candidates per item** + comparison table + risk notes
- Skips stores incompatible with the brief (e.g. fast-fashion for a "quiet luxury" request)
- Honest about freshness — every report carries an "as of" date and FX-rate footer

## Install

```bash
git clone https://github.com/<your-username>/shopping-research.git ~/.cursor/skills/shopping-research
```

That's it — open Cursor in any project and the skill is available globally.

To verify, ask Cursor: *"列出我已安装的 skills"*.

## Use

In any Cursor chat, simply say:

```
跑 shopping-research：我需要黑色通勤包，无 logo，适合 finance/consulting 面试，预算 100 CHF。
```

Or, for a more involved request, fill in [`request-template.yaml`](request-template.yaml), save it as `requests/<YYYY-MM>-<tag>.yaml` in your workspace, and tell the agent:

```
用 shopping-research，按 requests/2026-06-winter-coat.yaml 跑一份报告。
```

See [`examples/winter-coat.example.yaml`](examples/winter-coat.example.yaml) for a sanitized full example.

## File structure

| File | Purpose |
|---|---|
| `SKILL.md` | Main skill instructions, read by Cursor |
| `request-template.yaml` | Input schema with field docs |
| `site-registry.md` | Curated retailer list — delivery / sizing / pricing notes |
| `report-template.md` | Canvas output structure spec |
| `examples/` | Sanitized request examples |

## What the output looks like

The Canvas report has six sections:

1. Header with profile chips (image keywords, sizes)
2. Tier legend (Value / Balanced / Image-first)
3. **Per-item section** — 3-card grid with prices, specs, links + comparison table
4. Cross-item sanity check (does Balanced tier total fit your budget?)
5. Risks & next-steps (sizing quirks, single-listing volatility, FX)
6. Footer (data freshness date, stores searched)

## What it does *not* do

- Auto-purchase or auto-checkout
- Bypass login walls (e.g. BestSecret) — you can paste links manually
- Live stock/size inventory — the agent reads public product pages; verify availability before checkout

## Status

**v0.1** — first publicly released version. Iterating in public; expect the schema and store registry to evolve.

## Roadmap

- More region presets (UK, Germany, Singapore, Hong Kong)
- Deeper sustainability filter (OEKO-TEX, GOTS, B-Corp)
- Optional Python adapter layer (Playwright) for live price/stock on key stores
- Pair with a downstream `price-watch` skill once a candidate is selected

## Contributing

Issues and PRs welcome — especially:

- New retailer adapters in `site-registry.md`
- Region-specific sizing notes
- Sanitized example requests for new use cases (work bag, evening wear, athleisure, etc.)

## License

MIT — see [LICENSE](LICENSE).
