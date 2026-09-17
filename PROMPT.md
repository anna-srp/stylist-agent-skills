# Fast Setup Prompt for Codex / Claude Code

Copy everything below this line into a new conversation.

---

Build and deploy Stylist, an English-language fashion Agent for the United States market, on ZooWork Runtime using this repository:

<https://github.com/anna-srp/stylist-agent-skills>

The default mode is **fast setup**, not full shopping or image acceptance testing. The required outcome is one persistent ZooWork Agent with all five repository Skills attached, running, and verified by one lightweight text-only Runtime turn. Do not search retailers, inspect personal photos, or generate a try-on during setup. Do not build a UI unless I ask after the Runtime Agent is ready.

## Fixed product scope

- Agent name: Stylist
- Language: English only
- Market: United States only
- Currency: USD only
- Entry points: product search, complete looks, virtual try-on, outfit feedback, and progressive style preferences
- Personalization: never block first use on registration, a quiz, a selfie, or a complete profile
- Privacy: user photos are private current-task inputs by default
- UI: optional and separate from Runtime deployment; never copy the reference site's protected visual design
- Additional requirements I give after this prompt take precedence

## 1. Open the repository

Use the existing local repository when present. Otherwise clone it once and work from its root. Read `README.md`, `agent/AGENTS.md`, and every `skills/*/SKILL.md` entrypoint. Read a referenced file only when the selected Skill requires it.

## 2. Load the official ZooWork development Skill

Before making ZooWork calls, use the official `zoowork-managed-agents` development Skill from <https://github.com/SerendipityOneInc/zoowork-sdk-skills>. If it is already installed, read and use that copy. Do not clone or reinstall it merely to check for updates. If it is missing, install it once and follow its required deployment guidance.

The official development Skill teaches Codex or Claude how to deploy. The five fashion Skills in this repository are what run on the ZooWork Agent.

## 3. Ask me for only the ZooWork API key

Check whether `ZOOWORK_API_KEY` is available in the process environment or this repository's local `.env` file. Never print its value.

If it is missing, ask me to sign in at <https://zoowork.ai/claw-settings?tab=account-api-keys>, create and copy the one-time `zct_...` secret, save it myself as `ZOOWORK_API_KEY` in the ignored `.env` file, and tell you when it is ready without pasting it into chat.

The API key is the only value I enter manually. Never ask me for an Agent ID. The setup stores the generated ID in ignored `.zoowork/` state. Keep the key out of prompts, source files, logs, artifacts, frontend bundles, and Git history.

## 4. Run the checked-in fast setup

After the key is available, run:

```bash
npm ci
npm run setup
```

Use this checked-in automation instead of writing a new deployment harness. Do not run `npm view`, browse package registries, inspect the entire SDK declaration file, or create replacement setup scripts unless the command fails with a concrete compatibility error.

`npm run setup` must:

1. validate the key with the read-only model catalog;
2. create or reuse one persistent Agent using saved state, stable labels, and an idempotency key;
3. package all five Skills correctly and upload only new or changed versions;
4. attach only missing Skills and verify that all five are enabled and eligible;
5. start the Agent only when needed and use the documented helper to wait for `desired_state === 'running'`;
6. run one English, United States, text-only style-profile turn that confirms `fashion-style-profile` triggers;
7. keep that verification session-only and stop without shopping, weather lookup, outfit building, photo scoring, or image generation.

The quick Runtime verification has a two-minute budget. If it fails, diagnose that failure only. Do not silently escalate into retailer searches, additional sessions, or try-on generation.

## 5. Finish directly in chat

When setup succeeds, report:

- that Stylist is running on ZooWork Runtime;
- the automatically generated Agent ID;
- all five attached Skill names and enabled/eligible status;
- the quick verification result and elapsed time;
- that no retailer search, personal-photo processing, or image generation occurred during setup;
- this exact question: “Would you like to try a real fashion request now, customize the workflow, or build a UI for this Agent?”

Do not create an acceptance report, retry assessment, evidence bundle, or Markdown deliverable. The running Agent is the deliverable.

## 6. Full capability testing is opt-in

Do not test every fashion Skill during installation. Run product searches, outfit assembly, photo scoring, or virtual try-on only when I explicitly request a full acceptance test or make a real fashion request. Use current United States inventory, USD prices, exact retailer links, private user-photo handling, and at most one bounded image correction. Explain before a full test that retailer verification and image generation can add several minutes and may spend image credits.

## 7. UI is opt-in

If I ask for a UI, recommend ZooWork App Kit when I have no frontend preference; otherwise adapt my frontend. Keep the API key server-side, pin the saved Agent ID automatically, set `AGENT_PICKER=off`, and keep all copy in English with United States shopping behavior. Support private uploads, multi-turn sessions, refresh recovery, authentication, user isolation, rate limits, usage limits, and explicit consent before saving or sharing photos. Preview locally and ask for approval before public deployment.

The intended flow is: API key → fast incremental setup → running Runtime Agent → one text-only verification → optional real fashion request, customization, or UI.
