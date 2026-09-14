# Complete Prompt for Codex / Claude Code

Copy everything below this line into a new conversation.

---

Build a bilingual AI fashion stylist on ZooWork in the current project. Reproduce the core capabilities of Stylist AI Insider, not its visual design.

The required default outcome is:

1. install and read the official ZooWork development skill;
2. ask me to configure only my ZooWork API key;
3. create or reuse one ZooWork Agent;
4. upload and attach the five fashion skills in this repository;
5. start the Agent and confirm that it is running on ZooWork Runtime;
6. run a small bounded smoke test;
7. report the usable Agent directly in the chat and ask whether I want a UI.

Do not turn this task into a documentation or QA project. Do not create an acceptance report, retry assessment, evidence bundle, Markdown deliverable, or other report file unless I explicitly request one. The Agent running successfully on ZooWork Runtime is the primary deliverable. A UI is an optional next step.

Default product definition:

- Agent name: Stylist
- Markets: China and the United States
- User language: follow the user's Chinese or English
- Core entry points: find a product, build a complete look, AI virtual try-on, and outfit scoring
- Personalization: use a progressive style profile, but never block first use on registration, a quiz, a selfie, or a complete profile
- UI preference: do not copy the existing demo's visual design; offer ZooWork App Kit or a custom UI only after the Runtime Agent is ready
- My additional requirements: none; requirements I add after this prompt take precedence

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

Then read the complete `SKILL.md` for `zoowork-managed-agents`. Read its `deploy-your-agent` reference and any SDK or event-streaming reference needed for the implementation. If the official repository is already available locally, read it instead of installing it again.

The official skill is for the development assistant. The five fashion skills in this repository are uploaded and attached to the Agent running in ZooWork Runtime. Do not confuse these two layers.

## 1. Ask for only the ZooWork API key

Check only whether `ZOOWORK_API_KEY` is configured. Do not print its value.

The API key is the only value I enter manually. Never ask me to find, copy, paste, or configure an Agent ID. ZooWork returns the Agent ID after creation; save and reuse it automatically.

If the API key is missing, pause the setup and ask me to:

1. sign in at <https://zoowork.ai/claw-settings?tab=account-api-keys>;
2. open `Settings → API Keys → Create API Key`;
3. create and immediately copy the one-time `zct_...` secret;
4. save it myself as `ZOOWORK_API_KEY` in a local `.env` file;
5. tell you when it is saved without pasting the key into the chat.

Ensure `.env` is ignored by Git. The API key must never enter a prompt, source file, frontend bundle, log, artifact, or Git history. Do not create, rotate, or delete the key on my behalf.

After I confirm it is saved, call `listModels()` as the smallest read-only validation. Report only whether validation succeeded and how many models are available.

## 2. Read the product definition and proceed

Read `agent/AGENTS.md` and all five skill entrypoints. Read a skill's references when that skill requires them.

Do not require a separate design-approval round when the repository already answers the implementation questions. Briefly state what you are about to provision, then proceed. Ask me only when a genuinely missing choice would materially change the Agent; do not ask ceremonial questions and do not create a design document.

## 3. Create or reuse one Agent

Follow `zoowork-managed-agents` exactly:

- Use `@zoowork-ai/sdk`; do not guess package names or API shapes.
- Select a model from the actual `listModels()` response.
- Use `agent/AGENTS.md` as a Persona document.
- Check ignored local state for a previously generated `agent_id` and try to resolve the same Agent through stable labels. Do not ask me for the ID.
- Call `createAgent()` only if the Agent genuinely does not exist, using a stable idempotency key.
- Automatically save the returned `agent_id` in ignored server-side state, such as `.zoowork/stylist-agent.json`.
- If a backend or App Kit later expects `ZOOWORK_AGENT_ID`, populate it automatically from the saved state rather than asking me to enter it.
- Never create an Agent inside the per-message request path.

## 4. Package, upload, and attach the five skills

Process:

- `fashion-product-search`
- `fashion-outfit-builder`
- `fashion-virtual-try-on`
- `fashion-fit-check`
- `fashion-style-profile`

For each skill:

1. verify that the directory name matches the `name` in `SKILL.md` frontmatter;
2. preserve that directory as the zip's top-level directory;
3. use `uploadSkill(..., { scope: 'org' | 'personal' })` for a new owned skill, or add a version when the same owned skill already exists and changed;
4. attach it with `putAgentSkill(agentId, skillId)`;
5. verify with `listAgentSkills(agentId, { verbose: true })` that it is attached, enabled, and `eligible !== false`;
6. persist the skill IDs and versions without storing secrets.

Do not re-upload or attempt to attach global ZooWork catalog skills that a new Agent already receives automatically.

## 5. Publish the Agent to ZooWork Runtime

After the Persona and skills are attached, call `startAgent(agentId)` and then `waitUntilRunning(agentId)`. Do not use `actual_state` as the API-readiness signal.

For this task, “published to ZooWork Runtime” means the persistent Agent exists, all five skills are attached, and `waitUntilRunning()` confirms `desired_state === 'running'`. This does not automatically create a public website, and the absence of a UI is not a Runtime failure.

## 6. Run a bounded smoke test

Run one representative test for each of the four user-facing skills, plus one style-profile precedence check. Confirm that the expected skill actually triggers; a successful upload alone is insufficient.

Keep this phase bounded:

- make at most one corrective retry for a failed skill trigger or implementation defect;
- do not repeatedly spend image-generation credits to chase a perfect score;
- treat `insufficient_credits`, retailer anti-bot responses, temporary product-page failures, and other third-party availability issues as clearly labeled external limitations rather than reasons to undo a healthy Runtime deployment;
- do not require production UI concerns such as user authentication, rate limits, or abuse controls before declaring the Runtime Agent ready when no UI was requested;
- do not generate report or evidence files; keep the concise results in the final chat response.

A fatal Runtime blocker is an invalid key, an Agent that cannot reach `running`, a skill that cannot be attached or is ineligible, or a broken session/event path that prevents any conversation. A single blocked retailer, an exhausted image quota, or an optional production-hardening item is not the same as a failed Agent deployment.

## 7. Finish with the Agent, not a Markdown file

When the Agent is running, respond directly in the conversation with:

- a clear statement that Stylist is running on ZooWork Runtime;
- the automatically generated `agent_id`;
- the five attached skill names and their status;
- a short smoke-test summary;
- any external limitation that remains, without presenting it as the main deliverable;
- the exact next-step question: “Would you like me to build a UI for this Agent now? I can use ZooWork App Kit or adapt your existing frontend.”

Do not create or return `acceptance-summary.md`, `retry-assessment.md`, or a similar document unless I explicitly ask for a written test report.

If I do not want a UI, stop after delivering the running Agent status. Do not treat this as incomplete.

## 8. Build and deploy a UI only if I want one

If I ask for a UI:

- recommend ZooWork App Kit when I have no existing frontend or stack preference;
- otherwise adapt my existing frontend and keep ZooWork session and event handling on the backend;
- pin the UI to the automatically saved Agent ID and set `AGENT_PICKER=off`;
- keep `ZOOWORK_API_KEY` server-side only;
- support multi-turn conversations, streaming, and refresh recovery;
- treat user photos as temporary private inputs and require explicit consent before saving or sharing them;
- add authentication, user isolation, usage limits, rate limiting, and abuse controls in proportion to the intended audience;
- let the layout, brand, colors, and components be freely customized instead of copying the demo.

Preview and verify the UI locally. Before making a website public or changing production access, ask for my explicit approval. After approval, deploy it and return the actual URL rather than a report file.

The final product flow is: skills attached → Agent running on ZooWork Runtime → direct usable-status response → optional UI choice → optional UI deployment.

---
