---
name: fashion-product-search
description: Find and compare individual fashion products from United States retailers with current USD prices and direct purchase links. Use when the user asks to find, shop, compare, buy, locate a cheaper alternative, find another color, or asks where to buy one fashion item. Do not use for a complete multi-item outfit or shopping outside the United States market.
---

# Fashion Product Search

Return a short, credible list the user can act on now.

Keep all user-facing text in English.

## Resolve the request

Extract the product category, budget in USD, United States size, color, occasion, brand preferences, and exclusions. Load `fashion-style-profile` when available, but never block on a missing profile.

The market is fixed to the United States. Search United States inventory, interpret an unspecified budget as USD, and evaluate retailer availability, sizing, shipping, returns, and links for United States shoppers. Do not ask the user to choose a market or currency. If the request explicitly requires another market, explain that this skill supports the United States only and offer to search for a United States-market equivalent.

Treat the budget as a hard ceiling. A candidate with no current numeric price has unproven budget fit and must be excluded from a budget-constrained result.

## Search and verify

Use the runtime's current web or shopping-search capability. Prefer official brands and established United States retailers.

For every accepted product, verify in the current turn:

- exact product name and retailer;
- current numeric price in `USD`;
- a direct product-detail URL, not a homepage, category, search, editorial, social, or short-link page;
- the page identifies the same product and is usable by a logged-out visitor;
- product image, size, stock, shipping, and returns when the source exposes them.

Never construct, localize, shorten, or recall a product URL from memory. Copy the exact current URL. Reject a `404`, login wall, app-only route, category/search page, or URL that cannot identify a specific product. A `403` or `429` on an exact official product URL may be an automation block rather than proof the item is gone; keep it only when the search result itself identifies the exact product and the URL contains a clear product identifier.

Do not silently use another market or recommend over-budget products. Fewer verified products are better than filler. Never invent a product, price, stock state, image, retailer, or link.

## Response

Respect the requested count; otherwise return up to five results. Use one plain line per item:

```text
category | exact product name | price and currency | retailer | full direct URL
```

Add one short `Stylist pick` grounded in the request or saved preferences. End with only the most useful next action: refine the search, build a full look, or try on a selected result.
