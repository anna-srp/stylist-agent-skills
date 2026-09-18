# Build, Verify, and Publicly Deploy Stylist

Copy everything below this line into a new Codex or Claude Code conversation.

---

Build and publicly deploy Stylist, an English-language fashion Agent for the United States market, using:

<https://github.com/anna-srp/stylist-agent-skills>

The required outcome is one persistent ZooWork setup Agent with all five repository Skills attached, one lightweight text-only Runtime verification, an original fashion UI connected through a secure backend, and a working publicly accessible URL. Do not stop after Runtime setup or ask whether I want a UI. Build and deploy it immediately after verification. Do not search retailers, process personal photos, or generate a try-on during setup.

## Fixed product scope

- Product: Stylist
- Language: English only
- Market: United States only
- Currency: USD only
- Entry points: product search, complete looks, virtual try-on, outfit feedback, and progressive style preferences
- Personalization: never block first use on registration, a quiz, a selfie, or a complete profile
- Privacy: photos are private current-task inputs unless the user explicitly consents otherwise
- UI: required and original; do not copy the reference site's protected design
- Additional requirements I provide after this prompt take precedence

## 1. Open the repository

Use the existing local repository when present. Otherwise clone it once and work from its root. Read `README.md`, `agent/AGENTS.md`, and all five `skills/*/SKILL.md` entrypoints.

## 2. Load official ZooWork guidance

Before making ZooWork calls, use the installed `zoowork-managed-agents` Skill or install it once from <https://github.com/SerendipityOneInc/zoowork-sdk-skills> if missing. Do not reinstall it only to check for updates.

The official development Skill teaches the coding assistant how to deploy. The five fashion Skills in this repository run on the ZooWork Agent.

## 3. Ask me for only the ZooWork API key

Check the environment and ignored `.env` for `ZOOWORK_API_KEY`. Never print it. If missing, ask me to sign in at <https://zoowork.ai/claw-settings?tab=account-api-keys>, create a `zct_...` key, save it myself as `ZOOWORK_API_KEY` in `.env`, and tell you when it is ready without pasting it into chat.

Never ask me for an Agent ID or Skill ID. Setup creates or discovers the Agent and stores all identifiers in ignored `.zoowork/` state. Keep the key out of source, logs, artifacts, browser bundles, and Git history.

## 4. Run the fast incremental setup

```bash
npm ci
npm run setup
```

Use the checked-in automation. Do not browse package registries or replace the provisioning scripts unless this command fails with a concrete compatibility error.

Setup must validate the key, create or reuse one persistent Agent, upload only changed Skill versions, attach only missing Skills, verify that all five are enabled and eligible, start the Agent when needed, wait for `desired_state === 'running'`, and run one English, United States, text-only style-profile turn that proves `fashion-style-profile` was consulted.

The quick verification has a two-minute budget and must remain session-only. Do not shop, call weather services, build an outfit, score a photo, or generate an image. Diagnose a failure without silently escalating into more sessions or an image test.

## 5. Build the public UI immediately

Continue automatically after verification. Build the smallest useful full-stack web experience for this Agent. Reuse the setup Agent ID; do not provision another Agent for the UI. Prefer a simple Vercel-compatible framework and deploy to Vercel when the account is already authenticated. Build an original product experience with:

- fast text entry without a mandatory onboarding gate;
- clear product search, complete-look, try-on, outfit-feedback, and preference entry points;
- product cards with source, USD price, retailer, and direct purchase link fields;
- a streaming multi-turn conversation that survives refresh;
- progressive, editable style preferences kept within that visitor's conversation;
- explicit consent and deletion controls for any verified photo workflow; and
- loading, reconnect, empty, unavailable-inventory, quota, and generation-progress states.

For the lightweight public MVP:

- keep `ZOOWORK_API_KEY`, the Agent ID, and every ZooWork call on the server;
- let the browser call only the app's own server routes;
- create a separate ZooWork Session for each visitor or new conversation, keep its identifier in a signed, HTTP-only cookie or equivalent server-controlled state, and never let one visitor load another visitor's Session;
- stream events until `run.finished`, retain the latest cursor for reconnect, and handle loading, error, timeout, and New conversation states;
- add a basic server-side rate or usage limit so the public page cannot spend credits without bounds;
- do not add login, an admin panel, billing, a dashboard, D1, Durable Objects, or per-user Agent provisioning unless they are genuinely required; and
- do not enable photo uploads in this first release. Keep it text-first until a signed, expiring object-storage path is verified end to end.

Preview locally and run one text-only UI smoke test, then deploy to Vercel. This prompt already authorizes deployment; do not ask whether to publish. If Vercel requires interactive login, ask me only to complete that authorization and continue.

Verify that the public URL loads, a new conversation streams correctly, refresh restores it, visitors are isolated, rate limiting works, and no ZooWork secret or Agent ID appears in client assets or browser network responses.

## 6. Keep expensive capability tests opt-in

Do not exercise retailer search, outfit assembly, photo scoring, or virtual try-on during installation or UI deployment. Run those only for a real user request or an explicitly requested full acceptance test. Use current United States inventory, USD prices, direct retailer links, private photo handling, and at most one bounded image correction. Explain before testing that external search and image generation may add time and spend credits.

## 7. Finish directly in chat

Return the running setup Agent ID; all five attached Skill names and enabled/eligible status; quick-verification result and elapsed time; confirmation that setup performed no shopping, personal-photo processing, or image generation; the public Vercel URL; the UI smoke-test result; and any verified first-release limitation, especially private photo upload status.

Do not create an acceptance report, retry assessment, evidence bundle, or Markdown deliverable instead of the product. The running Agent and public URL are the deliverables.

The intended flow is: API key → fast incremental setup → one text-only verification → lightweight fashion UI → public Vercel URL.
