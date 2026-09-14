---
name: fashion-style-profile
description: Progressively remember, load, update, export, or delete a user's fashion preferences such as language, China or United States market, sizes, budget, favorite styles, and exclusions. Use in the background when stable preferences are provided or when the user asks to manage a style profile. Never require profile completion before another fashion skill can run.
---

# Fashion Style Profile

Remember only information that improves future fashion results.

## Principles

- There is no mandatory onboarding, quiz, registration, or selfie gate.
- The current request overrides saved preferences.
- Save explicit stable preferences. Do not turn a one-off choice into permanent taste without evidence.
- Missing memory is normal. Continue the requested fashion task with available information.
- Ask before saving a photo or other persistent sensitive data. A try-on or outfit photo is session-only by default.
- Let the user view, correct, export, or delete their profile.
- Delete only after an explicit request and confirmation at the action boundary.

## Profile fields

Store only fields that have values:

- preferred language and `CN` or `US` market;
- city when the user wants weather-aware styling;
- clothing and shoe sizes, preserving the market-specific sizing system;
- budget ranges and currency;
- preferred and avoided colors, brands, silhouettes, materials, and occasions;
- fit or proportion preferences stated by the user;
- feedback-tone preference;
- short user-authored notes.

Do not infer or store sensitive traits from photos. Do not store body, face, age, ethnicity, health, income, or identity judgments.

## Storage behavior

Use the product's authenticated per-user store when available. Isolate records by stable user ID and keep profile data server-side. Never expose hidden profile fields to another user, a public artifact, logs, or the browser bundle.

When durable storage is unavailable, keep preferences only in the current session and say so if the user asks whether they were saved. Do not pretend persistence succeeded.

Load relevant preferences before `fashion-product-search` and `fashion-outfit-builder`. For `fashion-virtual-try-on`, use style preferences to choose between products only; never redesign an exact reference garment from the profile.
