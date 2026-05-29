# Site registry

Curated retailers with delivery, scrape-ability, and brief-fit notes. Update as new stores are tested.

## Switzerland-delivery defaults

These ship to CH natively (no rerouting parcels), have public product pages indexable by web search, and cover most "office / smart-casual / quiet-luxury" briefs.

| Store | URL | CH delivery | Public pricing | Strengths | Avoid for |
|---|---|---|---|---|---|
| Uniqlo | `uniqlo.com/ch` | Yes | Yes | Basics king (white shirt, t-shirts, blazers, easy-care). True to size. | Statement / runway pieces. |
| Mango | `mango.com/ch` | Yes | Yes | Office basics, suiting, block-heel pumps. Runs small — size up if borderline. | Heavy outerwear. |
| H&M | `hm.com/ch` | Yes | Yes | Cheapest entry; "Premium Quality" line decent for basics. Inconsistent fit. | Investment pieces. |
| Zalando.ch | `zalando.ch` | Yes (free returns) | Yes | Aggregator: house brands (Anna Field, Even & Odd, Pier One) + 3rd-party. | When you want curation; can be overwhelming. |
| COS | `cos.com/en_chf` | Yes | Yes | Minimal architectural style. Cotton poplin shirts, leather pumps. Mid-price. | Tight budgets (often >75 CHF). |
| About You | `aboutyou.ch` | Yes | Yes | Aggregator with editor curation; frequent sales. | Same as Zalando — depth over curation. |
| Massimo Dutti | `massimodutti.com/ch` | Yes | Yes | Polished workwear, leather goods. Mid-high. | Tight budgets new; great on Vinted. |
| Vinted CH | `vinted.ch` | Yes | Yes | Second-hand: Massimo Dutti / COS / Other Stories pumps at 1/3 price. | When you need it tomorrow. |
| Arket | `arket.com/en_chf` | Yes | Yes | Quality basics, sustainable focus. Mid-price. | Tight budgets. |
| & Other Stories | `stories.com/en_chf` | Yes | Yes | Feminine workwear, heels. | Statement fast-fashion. |
| Zara | `zara.com/ch` | Yes | Yes | Trend-forward basics. Sizing inconsistent. | Long-term wardrobe staples. |

## Conditional / opt-in

| Store | When to use | Why off-default |
|---|---|---|
| Amazon.de / .it | Price-floor items where brand is known (e.g. specific Tamaris pump) | Bot-protected listings; counterfeit risk on fashion. |
| Asos | UK-fit users; lots of returns | UK→CH duty + slower returns. |
| Shein / Temu | Almost never for office briefs | Image mismatch with "professional / quiet luxury"; quality risk. |
| BestSecret | Manual link only | Login wall — agent can't browse. |
| Vestiaire Collective | High-end second-hand (Manolo, Ferragamo) | Pricier than Vinted, slower shipping. |

## Region presets

When `delivery_region` is set, default `stores.include` to:

- **Switzerland** → Uniqlo, Mango, H&M, Zalando.ch, COS, About You, Vinted CH (+ Massimo Dutti for accessories/leather)
- **Germany / EU** → above (with `.de` domains) + Asos
- **UK** → Asos, M&S, Uniqlo UK, Mango, COS, Vinted UK, & Other Stories

## Brand sizing quick-reference

| Brand | Tops fit | Notes |
|---|---|---|
| Uniqlo | True to size, generous bust | Easy Care line is interview-friendly. |
| Mango | **Runs small** (size up 1) | Slim-fit shirts can pull at chest. |
| H&M | Inconsistent across collections | Try Premium Quality / "Conscious" lines first. |
| COS | Roomy / oversized cut by default | Look for "fitted" tagged items if you want slim. |
| Massimo Dutti | True to size, slim cut | Italian sizing equivalent. |
| Zara | Inconsistent; check reviews | Often runs small in tops. |

## Brand pricing quick-reference (new, full price, women's basics)

Useful for sanity-checking budgets before searching.

| Brand | White poplin shirt | Block-heel pump (leather) |
|---|---|---|
| Uniqlo | 19.90–39.90 CHF | n/a (no heels) |
| H&M | 19.95–39.95 CHF | 39.95–69.95 CHF |
| Mango | 35.99–49.99 CHF | 49.99–89.99 CHF |
| Zalando house brands | 25–45 CHF | 35–69 CHF |
| Arket | 69 CHF | n/a |
| COS | 75–115 CHF | 135+ CHF |
| & Other Stories | 65–95 CHF | 99–165 CHF |
| Massimo Dutti | 49.95–79.95 CHF | 99–149 CHF |
| Vinted (used) | 8–25 CHF | 20–60 CHF |
