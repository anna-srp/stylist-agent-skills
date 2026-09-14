# Stylist Agent Skills

A portable skill pack distilled from the core product capabilities of [Stylist AI Insider](https://stylist-ai-insider.vercel.app/). It does not reproduce the site's visual design. Instead, it provides the capability layer that Codex, Claude Code, or another coding agent can use to rebuild the product, customize it, and connect it to any UI.

## Quick start

1. Clone this repository and open it in Codex or Claude Code.
2. Copy the complete contents of [PROMPT.md](PROMPT.md) into a new conversation.
3. Follow the instructions to sign in to ZooWork, create an API key under `Settings → API Keys`, and save it yourself in a local `.env` file. The API key is the only value you enter manually; never paste it into the chat.
4. Let the coding agent create the Agent, automatically save its returned `agent_id`, upload and attach the skills, start the Runtime, and run real trigger tests.
5. After the core capabilities pass, decide whether to use ZooWork App Kit or connect your own UI.

## Included skills

| User intent | Runtime skill | Purpose |
|---|---|---|
| Find, compare, or buy one product | `fashion-product-search` | Returns currently purchasable products with verifiable prices and links in the selected market |
| Build a complete look for an occasion or around an anchor item | `fashion-outfit-builder` | Assembles a complete, single-market, budget-compliant shoppable look |
| Preview one product or a complete look on a person | `fashion-virtual-try-on` | Creates an AI try-on preview from a person image and product references |
| Score or lightly roast an outfit | `fashion-fit-check` | Critiques only clothing and styling, then recommends the highest-impact improvement |
| Remember market, sizing, budget, and taste | `fashion-style-profile` | Adds progressive personalization without making registration or a quiz a prerequisite |

The four user-facing skills are equal entry points. `fashion-style-profile` is a supporting capability. Share pages, landing pages, frontend frameworks, and deployment configuration are deliberately outside this repository's core scope.

## Repository structure

```text
.
├── PROMPT.md
├── agent/
│   └── AGENTS.md
└── skills/
    ├── fashion-product-search/
    ├── fashion-outfit-builder/
    ├── fashion-virtual-try-on/
    ├── fashion-fit-check/
    └── fashion-style-profile/
```

Every skill directory name exactly matches the `name` in its `SKILL.md` frontmatter, so it can be packaged, uploaded, and attached using ZooWork's skill zip rules.

## Two different kinds of skill

- `zoowork-managed-agents` is installed into a development assistant such as Codex or Claude. It teaches the assistant how to use the ZooWork SDK correctly.
- The fashion skills under this repository's `skills/` directory are uploaded and attached to a ZooWork Agent. They teach the running Stylist Agent how to complete fashion tasks.

Install the official development skill first:

```bash
npx skills add SerendipityOneInc/zoowork-sdk-skills
```

Then load `zoowork-managed-agents` before working with ZooWork. Do not guess SDK calls from experience with another Agent platform.

## Important boundaries

- Keep `ZOOWORK_API_KEY` only in a server-side environment variable or an ignored local `.env` file. It must never enter a prompt, log, frontend bundle, or Git history.
- Users enter only `ZOOWORK_API_KEY`. They must never be asked to find, copy, or manually configure an Agent ID.
- Create the Agent once, automatically persist the returned `agent_id` in ignored server-side configuration, and reuse it for later conversations instead of creating an Agent for each message.
- Product prices, inventory, images, and purchase links must come from current search results. Never invent them.
- AI try-on is a visual preview, not a guarantee of size, fit, fabric behavior, or exact product fidelity.
- Outfit feedback evaluates clothing and styling only. It must not judge a person's body, face, age, skin tone, gender expression, or attractiveness.
- User photos are current-task inputs by default. Saving or publicly sharing them requires explicit consent.

## References

- [ZooWork Agent creation and public-release guide](https://starquest.feishu.cn/docx/AxJAd0dPDoWYIVxWQ9Xc2AjFndh)
- [ZooWork SDK skills](https://github.com/SerendipityOneInc/zoowork-sdk-skills)
- [Live demo](https://stylist-ai-insider.vercel.app/)
