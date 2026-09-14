---
name: fashion-outfit-builder
description: Build a complete shoppable fashion look for an occasion or around an anchor item using current products in China or the United States. Use for what should I wear, style me, build an outfit, complete this look, or dress-code requests. Do not use for a single-item search or outfit-photo scoring.
---

# Fashion Outfit Builder

Build one decisive main look. Add one meaningfully different alternative only when the same search pass contains enough verified products; do not repeat every search merely to manufacture a second option.

## Inputs and thesis

Determine the occasion or styling goal, market, budget, city or weather context, required categories, dress code, and any anchor item. Load `fashion-style-profile` when available. Treat explicit profile avoids and the current request as hard constraints.

Ask one concise question only when a missing market, city, date, or dress code would materially change the result. Never guess the season from the calendar alone. When live weather matters and a weather tool is available, check it before choosing sleeve length, fabric, outerwear, and footwear.

Before searching, define a compact look thesis: silhouette, two-to-four-color palette, formality, season or weather, one focal point, and must-avoid constraints. Translate aspirational designers or celebrities into visual characteristics unless the budget comfortably supports their current retail prices.

## Complete the core slots

A complete look requires either:

- top + bottom + shoes; or
- one-piece + shoes.

Only after every core slot has a verified item may you add useful outerwear, a bag, or up to two accessories. Respect an existing anchor item and do not replace it unless asked.

Allocate the total budget across required slots before searching. Search each missing category through `fashion-product-search`, using the same market and currency. Prefer parallel category searches when the runtime supports them. Reuse those candidate sets for an alternative look.

Reject combinations with clashing formality, conflicting silhouettes, several unrelated statement pieces, no clear focal point, impractical weather choices, or shoes that break the proportion. The first look is the strongest profile match. An alternative changes exactly one meaningful dimension such as color, silhouette, or formality while preserving occasion, market, budget, and hard constraints.

If a core slot has no verified product, simplify that slot's query once. If it still fails, state that the look is incomplete; never present prose such as “pair with trousers” as a purchasable item and never call a partial result complete.

## Validate and respond

Before answering, verify:

- every item uses the same intended market and currency;
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
