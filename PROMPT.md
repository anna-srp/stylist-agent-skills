# 可直接复制给 Codex / Claude Code 的完整 Prompt

从下一行开始完整复制。

---

你要在当前项目中构建一个运行在 ZooWork 上的双语 AI 时尚顾问。目标是复刻 Stylist AI Insider 的核心能力，而不是复刻它的页面视觉。先把 Agent、Persona、Skills、ZooWork Runtime 和测试做好；UI 只做最小接入，或按我之后给出的偏好自由调整。

默认产品设定：

- Agent 名称：Stylist
- 服务市场：中国和美国
- 用户语言：跟随用户的中文或英文
- 核心入口：找单品、搭整套、AI 试穿、穿搭打分
- 个性化：渐进式风格档案，但首轮使用不能被注册、问卷、自拍或完整档案阻塞
- UI 偏好：不复制现有 demo 的视觉；核心能力通过后再决定使用 ZooWork App Kit 或自定义页面
- 我的额外修改要求：无；如果我在这段 Prompt 后补充要求，以补充要求为准

把以下文件当作业务能力的 source of truth：

- `agent/AGENTS.md`
- `skills/fashion-product-search/SKILL.md`
- `skills/fashion-outfit-builder/SKILL.md`
- `skills/fashion-virtual-try-on/SKILL.md` 及其 references
- `skills/fashion-fit-check/SKILL.md` 及其 references
- `skills/fashion-style-profile/SKILL.md`

工作方式与顺序：

## 0. 先安装并读取 ZooWork 官方开发 skill

在写任何 ZooWork SDK 调用前，先运行：

```bash
npx skills add SerendipityOneInc/zoowork-sdk-skills
```

然后完整读取 `zoowork-managed-agents` 的 `SKILL.md`，并按任务需要读取它指向的 `deploy-your-agent`、TypeScript SDK、events/streaming 和 not-supported references。如果官方仓库已经在当前项目中，直接读取本地版本，不要重复安装。

这一步的 skill 是给开发助手用的；本仓库的 fashion skills 是稍后上传给 ZooWork Runtime 中 Agent 使用的。不要混淆两者。

## 1. 安全获取并验证 ZooWork API Key

先只检查 `ZOOWORK_API_KEY` 是否已配置，不要读取后输出它的值。

如果没有配置，暂停创建操作，并明确让我完成下面的动作：

1. 登录 <https://zoowork.ai/claw-settings?tab=account-api-keys>；
2. 打开 `Settings → API Keys → Create API Key`；
3. 用部署环境命名，例如 `stylist-local` 或 `stylist-production`；
4. 立即复制只显示一次的 `zct_...` 密钥；
5. 由我亲自把它保存到本地 `.env` 的 `ZOOWORK_API_KEY`，不要粘贴到聊天中。

确保 `.env` 已被 `.gitignore` 忽略。API Key 不得写入 Prompt、源代码、前端代码、日志、构建产物或 Git 仓库。不要创建、轮换或删除密钥来替我完成这一步。

我确认已保存后，用 `listModels()` 做最小只读验证。验证结果只报告成功/失败和可用模型数量，不显示密钥。若返回未授权，回到同一个 Settings 页面处理，不要猜测其他密钥接口。

## 2. 先设计，得到确认后再创建

阅读本仓库 Persona 与五个 skills，先输出一份紧凑设计方案，至少包含：

1. Persona 和双语语气；
2. 五个 skills 的职责、触发条件和相互路由；
3. 每个 skill 依赖的工具或参考资料；
4. 数据保存范围和隐私边界；
5. 至少 8 个真实测试题及验收标准；
6. 仍然缺失、且会实质影响结果的信息。

这一阶段不要创建或修改 ZooWork Agent。设计足够明确时不要为了形式反复提问，只把真正会改变实现的选择交给我确认。

## 3. 创建并启动 Agent

得到我确认后，再严格按 `zoowork-managed-agents` 执行：

- 使用 `@zoowork-ai/sdk`，不要猜包名或调用形状；
- 从 `listModels()` 的实际返回值选择模型；
- 把 `agent/AGENTS.md` 作为 Persona 文档；
- 创建前先检查本地是否已有保存的 `ZOOWORK_AGENT_ID`，以及是否能通过稳定 label 找到同一 Agent；
- 只有确实不存在时才调用 `createAgent()`，并使用稳定的 idempotency key；
- 创建成功后立即保存 `agent_id` 到被 Git 忽略的服务端配置；
- Agent 是长期资源，只创建一次，绝不能放在每次用户消息的处理函数里；
- 调用 `startAgent(agentId)`，再调用 `waitUntilRunning(agentId)`；不要用 `actual_state` 判断 API 是否可用。

## 4. 打包、上传并绑定本仓库 skills

依次处理：

- `fashion-product-search`
- `fashion-outfit-builder`
- `fashion-virtual-try-on`
- `fashion-fit-check`
- `fashion-style-profile`

对每个 skill：

1. 验证目录名与 `SKILL.md` frontmatter 的 `name` 完全一致；
2. 打包 zip 时保留同名顶层目录；
3. 首次使用 `uploadSkill(..., { scope: 'org' | 'personal' })`；已有同名自有 skill 且内容有变化时按官方说明创建新版本，不要重复制造同名资源；
4. 使用 `putAgentSkill(agentId, skillId)` 绑定；
5. 用 `listAgentSkills(agentId, { verbose: true })` 验证已绑定、启用且 `eligible !== false`；
6. 记录 skill id 和版本，但不要记录任何密钥。

不要尝试重新上传或绑定 ZooWork 全局 catalog 中已经自动附带的 skills。

## 5. 用真实任务验证“触发成功”，不只验证“上传成功”

至少完成以下测试：

- 单品搜索：分别用 CN/CNY 与 US/USD 请求，验证直接商品页、当次价格、预算硬上限和无登录购买路径；
- 整套穿搭：验证 `上装 + 下装 + 鞋` 或 `连体单品 + 鞋` 的完整核心槽位、同市场同货币、总价正确、天气和场合合理；
- AI 试穿：用一张人物图和一张单品图，再用一张人物图和一套完整 look 图，确认只替换目标服饰、人物身份与场景保持、结果带 AI 预览声明；
- 穿搭打分：验证评分有依据、建议可执行、轻度吐槽只针对衣服，不评价人或身体；
- 风格档案：验证缺少档案时其他能力仍能首轮工作；当前请求覆盖旧偏好；照片不被默认持久化；
- 多轮对话：验证换预算、换颜色、围绕搜索结果搭整套、再进入试穿的上下文连续性；
- 安全与失败：验证不会编造商品、价格、库存、图片或链接；私密图片链接不会出现在回答中；工具失败时明确说明限制；
- 连接恢复：按官方 event cursor 方式验证断开后的继续读取，并在 `isRunFinished(ev)` 时结束当前 turn 的 stream。

每个测试都记录：输入、期望触发的 skill、实际观察、通过/失败、失败原因和修改建议。若某个 skill 虽然显示已绑定但实际不触发，优先检查它的 frontmatter `description`。

未经我确认，不要为了修复测试而扩大产品范围或更改已确认的 Persona、安全边界和隐私策略。普通实现缺陷可以直接修复并重测。

## 6. UI 是可选交付层

Agent 和五个 skills 全部通过后，再处理 UI：

- 如果我没有指定技术栈，先建议 ZooWork App Kit；
- 页面固定到保存的 `ZOOWORK_AGENT_ID`，设置 `AGENT_PICKER=off`；
- API Key 只存在服务端；
- 支持多轮对话、流式响应、刷新恢复、用户隔离、额度限制和防刷；
- 用户照片按临时私密输入处理，完成后删除，保存或分享必须单独征得同意；
- 视觉风格、布局、品牌色和组件可以自由替换，不要把现有 demo 的 CSS 或页面结构当作能力依赖。

如果我已有网站，就只提供清晰的后端 session/event 接入层，不要强迫迁移到 App Kit。

## 7. 发布前停一次

本地验收完成后，先向我报告：

- `agent_id`（可以显示）；
- Runtime 状态；
- 已绑定的 skill 名称、id、版本和资格状态；
- 测试通过率、失败项和剩余风险；
- 是否包含 UI，以及 API Key、用户数据、图片和会话的隔离方式；
- 建议的发布方式。

在我明确确认前，不要公开部署页面或改变线上访问权限。确认发布后，部署并返回访问 URL，同时再次检查 Agent 连接、`AGENT_PICKER=off`、用户隔离、限额、防刷和密钥未进入客户端或 Git 历史。

最终目标不是“代码能编译”，而是五个 skills 在 ZooWork Runtime 中被真实触发，四个核心用户任务可以稳定完成，且任何 UI 都只是可替换的交付层。

---
