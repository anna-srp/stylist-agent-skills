---
name: fashion-outfit-builder
description: Build a complete shoppable fashion look for an occasion or around an anchor item using current products from United States retailers with USD prices. Use for what should I wear, style me, build an outfit, complete this look, or dress-code requests. Do not use for a single-item search, outfit-photo scoring, or shopping outside the United States market.
---

# Fashion Outfit Builder

Build one decisive main look. Add one meaningfully different alternative only when the same search pass contains enough verified products; do not repeat every search merely to manufacture a second option.

Keep all user-facing text in English.

## Inputs and thesis

Determine the occasion or styling goal, budget in USD, United States city or weather context, required categories, dress code, and any anchor item. Load `fashion-style-profile` when available. Treat explicit profile avoids and the current request as hard constraints.

The market is fixed to the United States. Do not ask the user to choose a market or currency. Ask one concise question only when a missing city, date, or dress code would materially change the result. Never guess the season from the calendar alone. When live weather matters and a weather tool is available, check it before choosing sleeve length, fabric, outerwear, and footwear. If the request explicitly requires another market, explain that this skill supports the United States only and offer a United States-market alternative.

Before searching, define a compact look thesis: silhouette, two-to-four-color palette, formality, season or weather, one focal point, and must-avoid constraints. Translate aspirational designers or celebrities into visual characteristics unless the budget comfortably supports their current retail prices.

## Complete the core slots

A complete look requires either:

- top + bottom + shoes; or
- one-piece + shoes.

Only after every core slot has a verified item may you add useful outerwear, a bag, or up to two accessories. Respect an existing anchor item and do not replace it unless asked.

Allocate the total budget across required slots before searching. Search each missing category through `fashion-product-search`, using United States inventory and USD throughout. Prefer parallel category searches when the runtime supports them. Reuse those candidate sets for an alternative look.

Reject combinations with clashing formality, conflicting silhouettes, several unrelated statement pieces, no clear focal point, impractical weather choices, or shoes that break the proportion. The first look is the strongest profile match. An alternative changes exactly one meaningful dimension such as color, silhouette, or formality while preserving occasion, the United States market, budget, and hard constraints.

If a core slot has no verified product, simplify that slot's query once. If it still fails, state that the look is incomplete; never present prose such as “pair with trousers” as a purchasable item and never call a partial result complete.

## Validate and respond

Before answering, verify:

- every item is currently offered to United States shoppers with a USD price;
- every purchase link is an exact current result;
- displayed prices add to the stated total;
- total cost stays within budget;
- color, silhouette, formality, weather, and practical use are coherent;
- no item violates an explicit avoid, dress code, size need, or anchor choice.

Lead with `LOOK 1` and one sentence explaining the thesis. Then give one plain line per item:

```text
category | exact product name | price and currency | full direct URL
```

Show the calculated total separately. If a valid second look exists, use the same structure under `LOOK 2`. Offer one refinement path plus `AI try-on`.
