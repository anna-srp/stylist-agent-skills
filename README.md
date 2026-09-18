# Stylist Agent Skills

A portable, English-language Skill pack distilled from the core capabilities of [Stylist AI Insider](https://stylist-ai-insider.vercel.app/) for the United States market. It provides the capability layer without copying the site's visual design.

## Fast start

1. Clone this repository and open it in Codex or Claude Code.
2. Copy [SHORT_PROMPT.md](SHORT_PROMPT.md) for email or a web page, or use the complete [PROMPT.md](PROMPT.md) for explicit implementation details.
3. Create a ZooWork API key, save it in a local ignored `.env` file, and tell the assistant when it is ready. Never paste it into chat.
4. The assistant runs the checked-in setup and performs one text-only Runtime verification.
5. It then builds an original Stylist UI and deploys it to a publicly accessible URL.

The API key is the only value entered manually. The Agent ID is created, stored, and reused automatically.

## Fast setup versus real use

| Mode | What it does | When to use it |
|---|---|---|
| Fast setup | Incremental deployment plus one text-only style-profile verification | Default installation |
| Real request | Shopping, outfit building, fit feedback, or virtual try-on | First actual use |
| Full acceptance test | Exercises multiple retailer and image paths | Only when explicitly requested |

Fast setup intentionally avoids retailer search, weather lookup, personal-photo processing, and image generation. The quick Runtime turn has a two-minute hard budget. Expensive or failure-prone third-party work is deferred until it produces something the user actually wants.

After that verification, the default prompt continues into a secure public UI built on the ZooWork App Kit foundation. The API key remains server-side, the Agent picker is disabled, per-user Agents receive all five org Skills automatically, and real authentication and session ownership are required. The first public build is text-first; private photo upload is enabled only after a signed temporary-media bridge is verified end to end.

## Included automation

```bash
npm ci
npm run setup
```

The commands are also available separately:

```bash
npm run deploy  # create/reuse the Agent and reconcile only changed Skills
npm run verify  # one text-only style-profile turn
```

Ignored `.zoowork/` state stores Agent and Skill IDs, content hashes, versions, and the last verification result. It stores no API key. Repeated deployment skips unchanged Skill uploads.

## Included Skills

| User intent | Runtime Skill | Purpose |
|---|---|---|
| Find or compare one product | `fashion-product-search` | Verifies current United States products, USD prices, and direct links |
| Build a complete look | `fashion-outfit-builder` | Creates a coherent, budget-compliant shoppable outfit |
| Preview an item or look | `fashion-virtual-try-on` | Produces a private AI preview from person and product references |
| Score or lightly roast an outfit | `fashion-fit-check` | Critiques clothing and styling, never the person |
| Manage stable preferences | `fashion-style-profile` | Adds progressive personalization without an onboarding gate |

## Boundaries

- All user-facing conversation and UI copy are English only.
- Shopping is limited to the United States market and USD.
- Products, prices, inventory, images, and links must come from current sources; never invent them.
- Virtual try-on is an AI preview, not a fit or sizing guarantee.
- User photos are private current-task inputs unless the user explicitly consents to persistence or sharing.
- `zoowork-managed-agents` belongs in the development assistant; the five repository Skills belong on the Runtime Agent.
- ZooWork Runtime hosts the Agent and Skills; the default prompt separately builds and publicly deploys the product UI.

## Repository structure

```text
.
├── PROMPT.md
├── SHORT_PROMPT.md
├── package.json
├── scripts/
│   ├── provision.mjs
│   └── verify.mjs
├── agent/AGENTS.md
└── skills/
    ├── fashion-product-search/
    ├── fashion-outfit-builder/
    ├── fashion-virtual-try-on/
    ├── fashion-fit-check/
    └── fashion-style-profile/
```

## References

- [ZooWork Agent creation and public-release guide](https://starquest.feishu.cn/docx/AxJAd0dPDoWYIVxWQ9Xc2AjFndh)
- [ZooWork SDK Skills](https://github.com/SerendipityOneInc/zoowork-sdk-skills)
- [Live demo](https://stylist-ai-insider.vercel.app/)
