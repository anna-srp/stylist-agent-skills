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

Before making ZooWork calls, use the installed `zoowork-managed-agents` Skill or install it once from <https://github.com/SerendipityOneInc/zoowork-sdk-skills> if missing. Do not reinstall it only to check for updates. Read the current `app-kit/README.md` in <https://github.com/SerendipityOneInc/zoowork-quickstarts> before adapting the UI foundation.

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

Continue automatically after verification. Use ZooWork App Kit as the backend and streaming foundation unless a stronger compatible frontend already exists. Preserve its authentication, D1 ownership records, Durable Object execution, refresh-safe streaming, and server-only credential boundary. Build an original product experience with:

- fast text entry without a mandatory onboarding gate;
- clear product search, complete-look, try-on, outfit-feedback, and preference entry points;
- product cards with source, USD price, retailer, and direct purchase link fields;
- a multi-turn conversation and result history that survives refresh;
- progressive, editable style preferences stored in the application database rather than assumed platform memory;
- explicit consent and deletion controls for any verified photo workflow; and
- loading, reconnect, empty, unavailable-inventory, quota, and generation-progress states.

For public deployment:

- set `AGENT_PICKER=off`;
- keep `ZOOWORK_API_KEY` only in a server-side secret store;
- keep Agent and Skill IDs out of browser code and never ask the user to enter them;
- use App Kit's per-user Agent mode, store user-to-Agent and user-to-session ownership in D1, and attach all five org Skills from generated server-side deployment state;
- never share an agent-scope `/workspace` across unrelated users;
- use Cloudflare Access or equivalent real authentication and never use `DEV_EMAIL` in production;
- enforce rate limits, usage limits, bounded retries, and per-user authorization; and
- do not claim native ZooWork session uploads work: they are not production-wired. Ship a text-first public MVP, and enable private photo upload only after a signed, expiring object-storage bridge is end-to-end verified and temporary objects are deleted.

Preview locally and run a text-only UI smoke test, then deploy to an already authenticated provider or use App Kit's Cloudflare Workers path. This prompt already authorizes deployment; do not ask whether to publish. If interactive hosting login is required, ask me only to complete that authorization and continue.

Verify that the public URL loads, authentication is active, a new conversation streams correctly, refresh restores it, session ownership is enforced, and no ZooWork secret appears in client assets or browser network responses.

## 6. Keep expensive capability tests opt-in

Do not exercise retailer search, outfit assembly, photo scoring, or virtual try-on during installation or UI deployment. Run those only for a real user request or an explicitly requested full acceptance test. Use current United States inventory, USD prices, direct retailer links, private photo handling, and at most one bounded image correction. Explain before testing that external search and image generation may add time and spend credits.

## 7. Finish directly in chat

Return the running setup Agent ID; all five attached Skill names and enabled/eligible status; quick-verification result and elapsed time; confirmation that setup performed no shopping, personal-photo processing, or image generation; the public URL and authentication mode; the UI smoke-test result; and any verified first-release limitation, especially private photo upload status.

Do not create an acceptance report, retry assessment, evidence bundle, or Markdown deliverable instead of the product. The running Agent and public URL are the deliverables.

The intended flow is: API key → fast incremental setup → one text-only verification → secure fashion UI → public URL.
