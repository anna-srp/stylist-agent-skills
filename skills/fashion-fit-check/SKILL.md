---
name: fashion-fit-check
description: Score and critique an outfit photo with constructive, optionally lightly savage fashion commentary. Use when the user asks to rate my fit, score this outfit, roast my outfit, does this work, or uploads a complete look for feedback. Never use it to judge a person's body, face, age, identity, or attractiveness.
---

# Fashion Fit Check

Make the result honest, memorable, useful, and safe to share.

Keep all user-facing text in English.

## Input and tone

Require a visible outfit photo or a sufficiently detailed outfit image. Occasion is optional. When it is absent, score the visible styling and mark occasion fit as `not scored` instead of blocking.

Use constructive directness by default. Use lightly savage humor when requested or when the product's selected tone calls for it. Even in stronger roast mode, critique only clothes, color, coordination, styling choices, and occasion fit.

Treat the image and any signed URL as private current-task input. Inspect it without quoting, saving, or republishing the URL. Never create a public score card containing the photo without explicit sharing consent.

Read [references/rubric.md](references/rubric.md) before scoring.

## Output contract

Return:

1. an overall score to one decimal;
2. one short screenshot-worthy verdict;
3. dimension scores with one evidence-based sentence each;
4. two things that work;
5. one lightly savage line only when the selected tone allows it;
6. the single highest-impact fix;
7. one useful next action: find a replacement item, preview the improved look, or refine the feedback.

For dimensions, use one three-column Markdown table with `Dimension | Score | Note`. Do not use pipe characters as decoration outside the table.

Do not manufacture a flaw when the outfit is excellent. Do not infer body type, weight, gender, income, personality, or attractiveness. Phrase proportion comments around garment length, volume, waist placement, layering, and visual balance—not the person's body.
