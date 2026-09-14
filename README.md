# Stylist Agent Skills

把 [Stylist AI Insider](https://stylist-ai-insider.vercel.app/) 的核心产品能力提炼成一套可移植的 ZooWork Agent skills。这里不复刻页面视觉，只保留可以让 Codex、Claude Code 或其他 coding agent 重新构建、微调和接入任意 UI 的能力层。

## 最快使用方式

1. 克隆本仓库并在 Codex 或 Claude Code 中打开。
2. 复制 [PROMPT.md](PROMPT.md) 的全部内容到新对话。
3. 按提示登录 ZooWork，在 `Settings → API Keys` 创建 API Key，并由你自己保存到本地 `.env`；不要把密钥粘贴到聊天中。
4. 让 coding agent 完成设计确认、Agent 创建、skills 上传绑定、Runtime 启动和真实测试。
5. 核心能力通过后，再决定是否接 ZooWork App Kit 或自己的 UI。

## 精简后的 skills

| 用户意图 | Runtime skill | 作用 |
|---|---|---|
| 找、比较或购买一个单品 | `fashion-product-search` | 返回当前市场中可购买、价格和链接可核验的商品 |
| 为场合或指定单品搭整套 | `fashion-outfit-builder` | 组合完整、同市场、预算内的可购买 look |
| 把单品或整套穿到照片中的人身上 | `fashion-virtual-try-on` | 基于人物图和商品参考图生成 AI 试穿预览 |
| 给一套穿搭打分或轻度吐槽 | `fashion-fit-check` | 只评价服装与搭配，并给出最高影响力的修改建议 |
| 记住市场、尺码、预算和审美偏好 | `fashion-style-profile` | 渐进式个性化，不把问卷或注册变成使用门槛 |

四个用户可见 skills 是并列入口；`fashion-style-profile` 是后台支撑能力。分享页、落地页、前端框架和部署配置不属于本仓库的核心能力。

## 仓库结构

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

每个 skill 的目录名都与 `SKILL.md` frontmatter 中的 `name` 完全一致，可直接按 ZooWork skill zip 规则打包、上传和绑定。

## 两类 skill 不要混淆

- `zoowork-managed-agents` 安装在 Codex/Claude 等开发助手中，负责教它正确使用 ZooWork SDK。
- 本仓库 `skills/` 下的 fashion skills 上传并绑定到 ZooWork Agent，负责教运行中的 Stylist 如何完成时尚任务。

开发助手先安装官方 skill：

```bash
npx skills add SerendipityOneInc/zoowork-sdk-skills
```

然后读取 `zoowork-managed-agents`，再操作 ZooWork。不要凭其他 Agent 平台的经验猜 SDK 调用。

## 重要边界

- `ZOOWORK_API_KEY` 只放服务端环境变量或被忽略的本地 `.env`，不能进入 Prompt、日志、前端 bundle 或 Git。
- Agent 只创建一次。后续对话复用保存的 `agent_id`，不能每次收到消息都重新创建。
- 商品价格、库存、图片和购买链接必须来自当次实时搜索，不允许编造。
- AI 试穿是视觉预览，不是尺码、版型、面料或实物一致性的保证。
- 穿搭点评只针对衣服与搭配，不评价身体、脸、年龄、肤色、性别表达或吸引力。
- 用户照片默认仅用于当前任务；保存或公开分享前必须得到明确同意。

## 参考

- [ZooWork Agent 创作与公开发布入门指南](https://starquest.feishu.cn/docx/AxJAd0dPDoWYIVxWQ9Xc2AjFndh)
- [ZooWork SDK skills](https://github.com/SerendipityOneInc/zoowork-sdk-skills)
- [在线 demo](https://stylist-ai-insider.vercel.app/)
