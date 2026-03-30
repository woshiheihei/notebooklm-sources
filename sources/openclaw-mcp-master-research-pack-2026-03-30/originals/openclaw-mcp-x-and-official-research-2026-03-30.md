# OpenClaw × MCP 深度调研素材（X / 官方文档 / GitHub）

- 日期：2026-03-30
- 主题：围绕 OpenClaw 创始人关于“下一版 OpenClaw 也是一个 MCP”的发言，尽可能收集 **官方 X / 官方 GitHub / 官方文档 / 官方 release notes** 中与这次更新相关的线索，并初步解释：
  - 为什么他们要这么做
  - 背后的设计哲学是什么
  - 未来方向可能是什么
  - 这件事在产品与商业层面的意义是什么

---

## 0. 先说结论

从目前拿到的官方材料看，OpenClaw 围绕 MCP 的更新，不只是“支持一个新协议”，而是在做两件互相关联的事：

1. **让 OpenClaw 能更好地消费外部 MCP 生态**
   - 也就是 OpenClaw 作为 MCP client / bridge，把外部 MCP servers 接进自己的 agent/tool 系统。

2. **让 OpenClaw 自己变成一个可被外部 agent 消费的 MCP endpoint**
   - 也就是 OpenClaw 作为 MCP server，把自己已经擅长的 conversation / messaging / route / approval 能力暴露给 Claude Code、Codex 等外部 agent。

这背后的更深层变化是：

> **OpenClaw 正在把“brain”和“messaging infrastructure”拆开。**

它不再要求“所有智能都必须在 OpenClaw 自己内部完成”，而是更像在抢占一个更稳定的位置：

> **agent 生态里的消息底座 / 会话桥 / 协议节点。**

---

## 1. 官方 X 源头：创始人 Peter Steinberger 的帖子

### 源头帖
- 链接：<https://x.com/steipete/status/2037715163562815817>
- 时间：2026-03-28 10:15 AM
- 原文：

> The next version of OpenClaw is also an MCP, you can use it instead of Anthropic's message channel MCP to connect to a much wider range of message providers. (I know, this is awkward)

### 中文整理
下一版 OpenClaw 本身也会成为一个 MCP。你可以把它当作 Anthropic message channel MCP 的替代层，用来接入更广泛的消息提供商。（“我知道这说法有点 awkward。”）

### 这条话最重要的三个信号
1. **OpenClaw 自己会变成 MCP endpoint**
   - 不是“OpenClaw 只是支持一些 MCP 工具”，而是它自己在对外暴露一个 MCP 界面。

2. **它对标的是 message channel MCP，而不是通用 agent runtime**
   - 这意味着重点不在“所有工具都开放”，而在“消息渠道 / 会话路由 / communication layer”。

3. **它强调“wider range of message providers”**
   - 也就是 OpenClaw 的独特价值不在抽象协议本身，而在它已经接好了很多 message providers / channel backends。

---

## 2. 创始人在 X 搜索结果里给出的补充说法

我额外搜索了 `from:steipete (OpenClaw MCP OR "message channel MCP" OR Codex OR Claude Code)`，有一条很关键的补充回复：

### 回复 1：MCP + notification extension
- 链接：<https://x.com/steipete/status/2038470522414239918>
- 原文：

> it is a normal mcp but also supports the notification extension.

### 中文整理
它是一个正常的 MCP，但同时也支持 notification extension。

### 这条回复的价值
这句话说明两件事：

1. **他们不是搞一个奇怪私有协议**
   - 先是 “normal MCP”——说明默认仍然在 MCP 兼容语义里。

2. **但 OpenClaw 需要补充 notification / push 相关扩展**
   - 这非常合理，因为 OpenClaw 的核心价值之一是“活的会话”和“新消息到达”，而不只是 request-response 式工具调用。
   - 这也解释了为什么官方文档里会出现 `events_poll` / `events_wait`，以及为什么创始人提到 notification extension。

这说明 OpenClaw 的 MCP 设计不是把自己做成“静态工具箱”，而是想保留 **消息系统的实时性**。

---

## 3. 官方 X 账号 @openclaw 的相关信号

虽然 @openclaw 账号没直接发“下一版 OpenClaw is an MCP”这句话，但和 MCP 相关的两个方向都在增强：

### 官方账号：2026.3.23 release post
- 链接：<https://x.com/openclaw/status/2036293335007264807>
- 文案里有：
  - `Chrome MCP waits for tabs`

### 官方账号：Chrome DevTools MCP 相关说明
- 链接：<https://x.com/openclaw/status/2032694261993427260>
- 文案强调：

> your agent can now see your tabs, cookies, logins — everything uses Chrome DevTools MCP under the hood, no extensions needed

### 这两条的价值
虽然它们不是“OpenClaw 自己变成 MCP server”的直接公告，但能看出：

- 官方账号已经在把 **MCP 当作一条明确产品路线** 来宣传，而不是边缘实验；
- 他们在公众沟通里，把 MCP 讲成一种让 agent 和已有环境/会话接起来的方式；
- 这和创始人关于 OpenClaw 作为 message-channel MCP 替代层的说法，属于同一个大方向：
  - **让 OpenClaw 成为 agent 与外部世界之间的标准连接层。**

---

## 4. 官方文档：`openclaw mcp serve`

### 文档关键句
来自 `docs/cli/mcp.md`：

> `openclaw mcp` has two jobs:
> - run OpenClaw as an MCP server with `openclaw mcp serve`
> - manage OpenClaw-owned outbound MCP server definitions ...

> `serve` is OpenClaw acting as an MCP server

> Use `openclaw mcp serve` when:
> - Codex, Claude Code, or another MCP client should talk directly to OpenClaw-backed channel conversations
> - you already have a local or remote OpenClaw Gateway with routed sessions
> - you want one MCP server that works across OpenClaw's channel backends instead of running separate per-channel bridges

### 中文解释
这份文档已经把方向说得很清楚：

- `openclaw mcp serve` 不是随手加的兼容层，而是 **OpenClaw 作为 MCP server 的正式入口**；
- 典型使用者就是 **Codex、Claude Code 或其他 MCP client**；
- 它要暴露的不是某个单一渠道，而是 **OpenClaw 背后的 channel-backed conversations**；
- 它的价值主张是：
  - 不用为 Slack / Telegram / Discord 各写一套桥
  - 用一个 MCP server 就能跨 OpenClaw 的多渠道后端工作

### 这意味着什么
这基本已经明示了产品定位：

> **OpenClaw 想把自己从“一个 agent 产品”下沉成“agent 生态里的多渠道消息基础设施”。**

---

## 5. 官方源码：当前 MCP server 实际暴露的能力

来自 `src/mcp/channel-tools.ts`，OpenClaw MCP bridge 暴露的核心工具包括：

- `conversations_list`
- `conversation_get`
- `messages_read`
- `attachments_fetch`
- `events_poll`
- `events_wait`
- `messages_send`
- `permissions_list_open`
- `permissions_respond`

### 这组工具说明了什么
如果 OpenClaw 的目标只是“做一个普通 MCP 工具服务器”，它完全可以暴露很多散装工具。

但这里暴露出来的其实是一套 **conversation-centric** 能力：

- 先找到会话 `conversations_list`
- 再看会话 `conversation_get`
- 读历史 `messages_read`
- 拉增量 `events_poll` / `events_wait`
- 沿着同一路由回消息 `messages_send`
- 处理审批 `permissions_*`

所以 OpenClaw 暴露的不是“工具集合”，而是：

> **一个已经存在的消息系统 / 会话系统 / 路由系统 / 审批系统，在 MCP 中的投影。**

这点非常关键，因为它直接解释了创始人为什么拿 Anthropic 的 message channel MCP 来做类比。

---

## 6. 官方 release notes：2026.3.28 已经把这方向写进版本说明

来自 `v2026.3.28` release notes：

> MCP/channels: add a Gateway-backed channel MCP bridge with Codex/Claude-facing conversation tools, Claude channel notifications, and safer stdio bridge lifecycle handling for reconnects and routed session discovery.

### 中文整理
在 MCP / channels 方向，这个版本加入了：
- 一个 **Gateway-backed channel MCP bridge**
- 面向 **Codex / Claude** 的 conversation tools
- Claude channel notifications
- 更安全的 stdio bridge lifecycle handling
- 更稳的 reconnect / routed session discovery

### 这条 release note 的价值
这是目前最硬的一条官方证据之一，因为它把方向一口气说全了：

1. **Gateway-backed**
   - 说明不是一个临时本地工具，而是建立在 OpenClaw Gateway 现有路由能力之上的桥。

2. **Codex/Claude-facing**
   - 非常明确：这不是只给 OpenClaw 内部用，而是要面向外部 coding agents。

3. **conversation tools**
   - 再次强调它暴露的是 conversation semantics，不是零散工具函数。

4. **notifications + lifecycle + reconnect**
   - 这说明官方非常清楚：MCP 真正落地到消息场景时，难点不只是 schema，而是：
     - 实时通知
     - 生命周期
     - 重连
     - 会话发现

这已经非常接近“产品方向宣言”了。

---

## 7. 官方愿景文件：为什么他们不想把 MCP 粗暴塞进 core

来自 `VISION.md`：

> OpenClaw supports MCP through `mcporter`

> This keeps MCP integration flexible and decoupled from core runtime:
> - add or change MCP servers without restarting the gateway
> - keep core tool/context surface lean
> - reduce MCP churn impact on core stability and security

> For now, we prefer this bridge model over building first-class MCP runtime into core.

### 中文解释
这说明 OpenClaw 团队对 MCP 一直有一个很鲜明的态度：

- 他们**欢迎 MCP**，但不想让核心 runtime 直接被 MCP 拖着走；
- 他们偏好的是 **bridge model / decoupled model**；
- 他们担心的是：
  - protocol churn
  - core stability
  - security surface

### 这和本次更新怎么连起来
所以这次“OpenClaw 自己也变成 MCP”并不意味着团队放弃了这个理念；反而更像是：

> 他们终于找到了一个更合理的边界：
> - 核心 runtime 继续保持自己的完整性
> - 但对外，OpenClaw 可以以 MCP 形式稳定暴露自己最强的一层：消息与会话基础设施

也就是：
- **内核不被协议绑死**
- **外部接口尽量利用生态标准**

这是一种很典型的“protocol leverage without core capture”的设计。

---

## 8. 为什么他们要这么做：设计、产品、商业三层解释

### A. 设计层：把 brain 和 messaging infra 拆开
这是最核心的设计动机。

过去更像：
- OpenClaw 既是 brain，又是消息层

现在更像：
- Claude Code / Codex / 别的 agent 可以当 brain
- OpenClaw 当 messaging / routing / conversation / approval layer

这意味着什么？

- OpenClaw 不需要在“谁的大脑最好”上永远竞争
- 它可以占据一个更稳、更底层的位置
- 它可以让更多上层 agent 接进来，而不是要求都住在它内部

### B. 产品层：从单体 agent 产品，变成生态基础设施
如果你把 release notes、文档、源码合起来看，OpenClaw 在做的其实是：

- 让外部 agent 能直接读写 OpenClaw 路由出来的多渠道对话
- 让 conversation / event / approval 这些东西进入标准协议层
- 让 OpenClaw 的多渠道后端，被更多上层 agent 直接消费

这就会把产品定位从：
- 一个“会发消息的 agent”

往：
- 一个“给任何 agent 提供消息能力的 infra”

迁移。

### C. 商业层：真正难复制的是消息基础设施，不是模型
这一点虽然官方没在文档里直接说死，但从产品路径上很明显。

更稳的商业价值来自：
- 多平台接入
- 渠道路由
- OAuth / token 管理
- bot pools
- rate limits
- 会话和身份映射
- operator approvals
- 网关与连接稳定性

而不是“我内置的那个 agent 壳是不是最聪明”。

换句话说：

> 模型层可能快速商品化，但消息与会话基础设施不会那么快商品化。

这也解释了为什么他们会愿意把上层 brain 让出来，转而占更底层、更稳定的位置。

---

## 9. 未来方向：从官方线索推出来的可能路线

### 方向 1：OpenClaw 成为面向外部 agents 的统一消息层
从 `Codex/Claude-facing conversation tools` 这个官方措辞看，这几乎已经是明牌：

- 外部 coding agents 是目标客户/目标使用者
- OpenClaw 负责把这些 agents 接到真实世界渠道上

### 方向 2：会越来越重视 notification / event / live session
创始人提到：
- normal MCP + notification extension

源码和文档里又有：
- `events_poll`
- `events_wait`
- Claude channel notifications

这说明未来 OpenClaw 的 MCP 不会停留在“同步工具调用”，而会更像：
- evented runtime bridge
- conversation stream bridge
- live route bridge

### 方向 3：stateful lifecycle 会持续增强
相关 PR 已经显示：
- persistent MCP server lifecycle
- session-level runtime cache
- reconnect / routed session discovery

也就是说，未来这条线很可能继续补：
- session continuity
- long-lived MCP processes
- multi-turn state preservation
- crash recovery

### 方向 4：控制面 / 安全面会同步加强
这点从 tool approval PR 和 release notes 已经很明显：
- approvals
- policy gating
- operator control
- safer lifecycle

所以未来 OpenClaw 的 MCP 路线，大概率不是“越开放越好”，而是：

> **越开放，越要把控制面做厚。**

---

## 10. 评论区里最有价值的几条回复

### 10.1 ImL1s：Interoperability > lock-in
原文：

> The "I know, this is awkward" is doing a lot of heavy lifting here. But this is actually the right move — MCP as a standard means the ecosystem wins when more providers connect to it, even if it means competing tools reference each other. Interoperability > lock-in.

#### 价值
这条评论很好地抓到了创始人那句 “awkward” 的含义：
- 从竞争视角，拿 Anthropic 的 message channel MCP 来类比确实 awkward；
- 但从生态视角，这是在强调标准优先、互操作优先。

这补充了 OpenClaw 这次更新背后的一个核心哲学：

> **协议标准比封闭锁定更重要。**

### 10.2 创始人补充：normal MCP + notification extension
原文：

> it is a normal mcp but also supports the notification extension.

#### 价值
这说明他们不是想偏离 MCP，而是想在 MCP 的基础上保留“消息系统需要实时通知”这一现实。

### 10.3 社区对官方账号的反应：Chrome MCP 相关传播非常强
从 @openclaw 关于 Chrome DevTools MCP 的传播量看，官方已经验证了一个事实：

- 用户非常理解并欢迎“用 MCP 把已有环境接给 agent”这种叙事
- 这和 OpenClaw 自己变成 channel MCP bridge 是完全同一路线

---

## 11. 这次设计的哲学，我的当前判断

如果把所有官方线索压缩一下，这次更新最像在追求这几个东西：

### 1. Composability-first
不是所有东西都必须在 OpenClaw 内部完成。
别的 agent 可以做 brain，OpenClaw 做消息底座。

### 2. Protocol-first, but not protocol-captured
积极拥抱 MCP，但不让 core runtime 被协议细节完全绑架。

### 3. Lean core + strong edges
核心继续保持克制；对外接口则尽量利用生态标准，把价值层暴露出来。

### 4. Interoperability over lock-in
宁可 awkward，也要站在标准互联这一边。

### 5. Infra as moat
更可能的护城河，不在单个模型/agent，而在：
- message providers
- routing
- events
- approvals
- gateway reliability
- operator control

---

## 12. 给 Deep Research 的重点问题

如果要继续用 ChatGPT Deep Research 往下钻，我建议重点追：

1. OpenClaw 暴露的到底是“工具”，还是“会话系统”？
2. 为什么 `events_poll` / `events_wait` / notifications 是这条路线的关键，而不是附属功能？
3. OpenClaw 和 Anthropic message channel MCP 的真正定位差异是什么？
4. 这是不是 OpenClaw 在把自己从 agent shell 迁移成 communications substrate？
5. 为什么官方既强调 bridge model、lean core，又同时不断给 MCP 补 lifecycle / cache / approvals？
6. 这条路线的商业价值是不是来自“任何 agent 都可能需要 message infra”，而不是“只有 OpenClaw agent 会用到它”？

---

## 13. 参考来源清单

### X / 官方账号
- Peter Steinberger 源头帖：<https://x.com/steipete/status/2037715163562815817>
- Peter 关于 notification extension 的补充：<https://x.com/steipete/status/2038470522414239918>
- OpenClaw 2026.3.23 release 帖：<https://x.com/openclaw/status/2036293335007264807>
- OpenClaw Chrome DevTools MCP 相关帖：<https://x.com/openclaw/status/2032694261993427260>

### 官方文档 / 源码 / release
- `docs/cli/mcp.md`
- `src/mcp/channel-tools.ts`
- `VISION.md`
- Release `v2026.3.28`

### 相关官方/半官方开发线索
- PR #29526 — `feat(extensions): add mcp-bridge plugin for native MCP support`
- PR #54957 — `feat(mcp): persistent MCP server lifecycle support`
- PR #54958 — `feat(mcp): session-level MCP runtime cache + rollover cleanup`
- PR #46970 — `feat(security): add MCP tool call approval system`
- Issue #29053 — native MCP client support
- Issue #53215 — OpenClaw MCP Server for Claude/Cursor

---

## 14. 一句话总括

**这次 OpenClaw 的 MCP 更新，表面是在“支持 MCP”，更深层是在把 OpenClaw 从一个自己包办思考和发消息的 agent 外壳，重构成 agent 生态里的多渠道消息桥、会话桥和协议节点。**
