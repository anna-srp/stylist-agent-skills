# Complete Prompt for Codex / Claude Code

Copy everything below this line into a new conversation.

---

Build a bilingual AI fashion stylist on ZooWork in the current project. The goal is to reproduce the core capabilities of Stylist AI Insider, not its visual design. Build and validate the Agent, Persona, Skills, ZooWork Runtime, and tests first. Keep the UI minimal, or customize it later according to my preferences.

Default product definition:

- Agent name: Stylist
- Markets: China and the United States
- User language: follow the user's Chinese or English
- Core entry points: find a product, build a complete look, AI virtual try-on, and outfit scoring
- Personalization: use a progressive style profile, but never block first use on registration, a quiz, a selfie, or a complete profile
- UI preference: do not copy the existing demo's visual design; decide between ZooWork App Kit and a custom UI only after the core capabilities pass
- My additional requirements: none; if I add requirements after this prompt, those requirements take precedence

Treat these files as the source of truth for product behavior:

- `agent/AGENTS.md`
- `skills/fashion-product-search/SKILL.md`
- `skills/fashion-outfit-builder/SKILL.md`
- `skills/fashion-virtual-try-on/SKILL.md` and its references
- `skills/fashion-fit-check/SKILL.md` and its references
- `skills/fashion-style-profile/SKILL.md`

Follow this workflow in order.

## 0. Install and read the official ZooWork development skill

Before writing any ZooWork SDK call, run:

```bash
npx skills add SerendipityOneInc/zoowork-sdk-skills
```

Then read the complete `SKILL.md` for `zoowork-managed-agents`. Read its referenced `deploy-your-agent`, TypeScript SDK, events and streaming, and not-supported documents when relevant. If the official repository is already available in the current project, read that local copy instead of installing it again.

The official skill is for the development assistant. The fashion skills in this repository will later be uploaded to the Agent running in ZooWork Runtime. Do not confuse the two.

## 1. Obtain and validate a ZooWork API key safely

Check only whether `ZOOWORK_API_KEY` is configured. Do not read and print its value.

The API key is the only value I must enter manually. Never ask me to find, copy, paste, or configure an Agent ID. ZooWork returns the Agent ID after creation, and you must save and reuse it automatically.

If it is missing, pause all Agent-creation operations and ask me to complete these steps:

1. Sign in at <https://zoowork.ai/claw-settings?tab=account-api-keys>.
2. Open `Settings → API Keys → Create API Key`.
3. Name the key after its deployment environment, such as `stylist-local` or `stylist-production`.
4. Immediately copy the `zct_...` secret, which is displayed only once.
5. Save it myself as `ZOOWORK_API_KEY` in a local `.env` file. I must not paste it into the chat.

Verify that `.env` is ignored by `.gitignore`. The API key must never be written into a prompt, source code, frontend code, logs, build artifacts, or Git history. Do not create, rotate, or delete a key on my behalf.

After I confirm that the key is saved, call `listModels()` as the smallest read-only validation. Report only success or failure and the number of available models. Never display the key. If authentication fails, return me to the same Settings page rather than guessing another key-management endpoint.

## 2. Design first and create only after approval

Read the Persona and five skills in this repository. First produce a concise design proposal that includes:

1. the Persona and bilingual voice;
2. the responsibility, trigger conditions, and routing relationships of all five skills;
3. the tools and references required by each skill;
4. data-retention scope and privacy boundaries;
5. at least eight realistic test prompts with acceptance criteria;
6. any missing information that would materially change the implementation.

Do not create or modify a ZooWork Agent during this phase. When the design is already clear, do not ask ceremonial questions. Ask only for choices that would materially change the implementation, then wait for my approval.

## 3. Create and start the Agent

After I approve the design, follow `zoowork-managed-agents` exactly:

- Use `@zoowork-ai/sdk`. Do not guess the package name or API shapes.
- Select a model from the actual response returned by `listModels()`.
- Use `agent/AGENTS.md` as a Persona document.
- Before creating anything, check your ignored local state for a previously generated `agent_id` and try to resolve the same Agent through stable labels. Do not ask me for this ID.
- Call `createAgent()` only when that Agent genuinely does not exist, and use a stable idempotency key.
- Save the returned `agent_id` automatically in ignored server-side configuration, for example `.zoowork/stylist-agent.json`. If the selected backend or App Kit expects `ZOOWORK_AGENT_ID`, populate that server-side value yourself from the saved result rather than asking me to enter it.
- Treat an Agent as a persistent resource. Never create one inside the per-message request path.
- Call `startAgent(agentId)`, followed by `waitUntilRunning(agentId)`. Do not use `actual_state` as the API-readiness signal.

## 4. Package, upload, and attach the repository skills

Process these skills:

- `fashion-product-search`
- `fashion-outfit-builder`
- `fashion-virtual-try-on`
- `fashion-fit-check`
- `fashion-style-profile`

For each skill:

1. Verify that the directory name exactly matches the `name` in the `SKILL.md` frontmatter.
2. Preserve that same directory as the zip's top-level directory.
3. On first upload, use `uploadSkill(..., { scope: 'org' | 'personal' })`. When an owned skill with the same name already exists and the content changed, add a version according to the official documentation instead of creating duplicate resources.
4. Attach it with `putAgentSkill(agentId, skillId)`.
5. Verify with `listAgentSkills(agentId, { verbose: true })` that it is attached, enabled, and `eligible !== false`.
6. Record its skill ID and version, but never record a secret.

Do not re-upload or attempt to attach global ZooWork catalog skills that a new Agent already receives automatically.

## 5. Validate real skill triggers, not only successful uploads

Run at least these tests:

- Product search: run separate CN/CNY and US/USD requests. Verify direct product pages, current prices, hard budget limits, and a purchase path that works without login.
- Outfit building: verify a complete core of `top + bottom + shoes` or `one-piece + shoes`, one market and currency, a correct total, and sensible weather and occasion choices.
- Virtual try-on: test one person image with one product image, then one person image with a complete-look reference. Verify that only the intended fashion items change, the person's identity and scene remain stable, and the result includes an AI-preview disclosure.
- Fit check: verify evidence-based scoring, actionable advice, and lightly savage feedback that targets clothing rather than the person or body.
- Style profile: verify that missing profile data does not block another skill's first turn, a current request overrides old preferences, and photos are not persisted by default.
- Multi-turn behavior: change a budget and color, build an outfit around a search result, and continue into virtual try-on while preserving context.
- Safety and failure behavior: verify that the Agent never invents products, prices, stock, images, or links; private image URLs never appear in responses; and tool failures are reported honestly.
- Connection recovery: follow the official event-cursor pattern after a disconnect and stop the current turn's stream when `isRunFinished(ev)` is true.

For every test, record the input, expected skill trigger, actual observation, pass or failure status, failure cause, and recommended change. If a skill is attached but does not trigger, inspect its frontmatter `description` first.

Do not expand the product scope or change an approved Persona, safety boundary, or privacy rule merely to make a test pass without my approval. Fix ordinary implementation defects directly and rerun the affected tests.

## 6. Treat UI as an optional delivery layer

Handle UI only after the Agent and all five skills pass:

- If I have not selected a stack, recommend ZooWork App Kit first.
- Pin the page to the automatically saved Agent ID and set `AGENT_PICKER=off`. Do not ask me to copy or enter the ID.
- Keep the API key server-side only.
- Support multi-turn conversations, streaming replies, refresh recovery, user isolation, usage limits, rate limiting, and abuse prevention.
- Treat user photos as temporary private inputs and delete them after processing. Saving or sharing requires separate explicit consent.
- Allow the visual style, layout, brand colors, and components to be replaced freely. Do not make the existing demo's CSS or page structure a capability dependency.

If I already have a website, build only a clear backend session and event integration. Do not force a migration to App Kit.

## 7. Stop once before public release

After local acceptance testing, report:

- the `agent_id`, which may be displayed;
- Runtime status;
- attached skill names, IDs, versions, and eligibility status;
- test pass rate, failures, and remaining risks;
- whether a UI is included and how the API key, user data, images, and sessions are isolated;
- the recommended release method.

Do not deploy a public page or change production access until I explicitly approve it. After approval, deploy and return the public URL. Recheck Agent connectivity, `AGENT_PICKER=off`, user isolation, usage limits, rate limiting, and that the API key never entered the client or Git history.

The final goal is not code that merely compiles. All five skills must trigger in ZooWork Runtime, the four core user tasks must complete reliably, and any UI must remain a replaceable delivery layer.

---
