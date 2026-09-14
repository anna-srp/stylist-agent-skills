---
name: fashion-product-search
description: Find and compare individual fashion products with current prices and direct purchase links. Use when the user asks to find, shop, compare, buy, locate a cheaper alternative, find another color, or asks where to buy one fashion item in China or the United States. Do not use for a complete multi-item outfit.
---

# Fashion Product Search

Return a short, credible list the user can act on now.

## Resolve the request

Extract the product category, market, budget and currency, size, color, occasion, brand preferences, and exclusions. Load `fashion-style-profile` when available, but never block on a missing profile.

Market controls retailers, currency, stock, sizing, shipping, and links. Infer `CN` or `US` only from reliable context; otherwise ask once before shopping. Interpret an unspecified currency as CNY only after CN is established and USD only after US is established.

Treat the budget as a hard ceiling. A candidate with no current numeric price has unproven budget fit and must be excluded from a budget-constrained result.

## Search and verify

Use the runtime's current web or shopping-search capability. Prefer official brands and established retailers in the selected market.

For every accepted product, verify in the current turn:

- exact product name and retailer;
- current numeric price and `CNY` or `USD` currency;
- a direct product-detail URL, not a homepage, category, search, editorial, social, or short-link page;
- the page identifies the same product and is usable by a logged-out visitor;
- product image, size, stock, shipping, and returns when the source exposes them.

Never construct, localize, shorten, or recall a product URL from memory. Copy the exact current URL. Reject a `404`, login wall, app-only route, category/search page, or URL that cannot identify a specific product. A `403` or `429` on an exact official product URL may be an automation block rather than proof the item is gone; keep it only when the search result itself identifies the exact product and the URL contains a clear product identifier.

Do not silently cross markets or recommend over-budget products. Fewer verified products are better than filler. Never invent a product, price, stock state, image, retailer, or link.

## Response

Respect the requested count; otherwise return up to five results. Use one plain line per item:

```text
category | exact product name | price and currency | retailer | full direct URL
```

Add one short `Stylist pick` grounded in the request or saved preferences. End with only the most useful next action: refine the search, build a full look, or try on a selected result.
