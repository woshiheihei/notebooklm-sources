# OpenClaw 正在变成 MCP server：这条 X 线程真正说清了什么

> 主题：OpenClaw × MCP × 消息基础设施
> 
> 这份档案围绕一条关于 OpenClaw 即将成为 MCP server 的 X 讨论展开，整理主帖、源头帖与高信号评论，帮助读者看清：这不是“多了一个接口”，而是 OpenClaw 的角色正在从单体 agent 外壳，转向 agent 生态里的消息基础设施。

## 主帖

- 作者：Michael（@michael_chomsky）
- 时间：2026-03-29 22:47
- 链接：https://x.com/michael_chomsky/status/2038266680296747079

这条长帖是在解释 Peter Steinberger 前一条简短更新背后的真正含义：OpenClaw 即将成为一个 MCP server。

主帖的核心判断有四层：

1. **过去程序化接 OpenClaw 很痛。** 不是模型本身难，而是外围胶水代码很多：WebSocket、TLS、health checks、chat relay、渠道桥接，全都得自己维护。
2. **现在 OpenClaw 会直接暴露 MCP tools。** 像 `messages_read`、`messages_send`、`events_poll`、`conversations_list` 这样的能力，理论上可以直接被支持 MCP 的 agent 调用。
3. **架构角色被拆开了。** 以前 OpenClaw 同时像“大脑”和“消息层”；现在 Claude Code、Codex、Cursor 等都可以当大脑，而 OpenClaw 专注处理 Slack / Telegram / Discord 等渠道。
4. **真正的价值开始下沉到 messaging infra。** 主帖作者明确说：对云服务商来说，最难卖、也最难维护的，从来不是 AI 本身，而是 50+ 消息平台适配、OAuth token、bot 池、限流与基础设施稳定性。

他给出的最直观对比是：

- **以前**：你告诉 Claude Code → Claude Code 找 OpenClaw agent → OpenClaw agent 再发 Slack
- **现在**：你告诉 Claude Code → Claude Code 直接调用 `messages_send` → Slack 消息发出

少了一跳，整个系统的可组合性和可调试性都上来了。

## 源头帖

- 作者：Peter Steinberger（@steipete）
- 时间：2026-03-28
- 链接：https://x.com/steipete/status/2037835239842861482

源头帖本身非常短，但信息密度很高：

> The next version of OpenClaw is also an MCP, you can use it instead of Anthropic's message channel MCP to connect to a much wider range of message providers. (I know, this is awkward)

中文整理：

下一版 OpenClaw 本身也会成为一个 MCP。你可以把它当成 Anthropic message channel MCP 的替代层，用来接入更广泛的消息提供商。

这句话短，但等于点明了一个结构变化：

- OpenClaw 不只是“某个 agent 的内部组件”
- 它开始变成一个对外暴露标准接口的消息能力层
- 任何支持 MCP 的外部 agent，都有可能把它当作统一通信底座

## 评论精选

### 挂在主帖下的增量评论

**Peter Steinberger（约 1 小时后）**  
原文：looked into some PRs, not all were good, landed some others. Please test main and give us feedback!  
中文：我看了几条相关 PR，不是每一条都够好，但有些已经落地了。欢迎大家直接去测试 `main` 并给反馈。  
意义：这不是纯概念讨论，已经进入“有具体 PR、可在主分支测试”的阶段。

**Panta（约 7 小时后）**  
原文：Given t3-code's architecture, it seems we're moving towards the world where AI labs provide model + runtime, and apps connect to the runtime, from above (UI) and below (providers).  
中文：照这个趋势，AI lab 会越来越像同时提供 model + runtime 的底层，而应用从上层 UI 和下层 provider 两边挂进去。  
意义：把这条消息提升成了平台结构判断——未来应用可能不再自己包全部运行时，而是插到更底层的 agent runtime / protocol 层上。

**Michael 的回应**  
原文：kind of, but OS harnesses will have a place as well. but overall yes  
中文：大体是这个方向，但 OS harness 这类执行层仍然会保有位置；总体判断是对的。  
意义：这是一个重要修正。并不是所有 agent 能力都会被 MCP 替代，尤其 GUI / OS 级执行层仍有独特价值。

**Onur Solmaz**  
原文：Let me look into those PRs  
中文：我去看看这些 PR。  
意义：虽然短，但说明讨论者的注意力已经落到具体实现与代码审阅，而不是空泛转发。

## 这条线程真正完成的三次跃迁

### 1. 从“OpenClaw 是一个 agent”到“OpenClaw 是消息基础设施”

这是最核心的变化。主帖作者最兴奋的点，并不是“OpenClaw 多了一些工具”，而是它开始扮演 agent 生态中的 **messaging layer**。

### 2. 从“必须绑死某个 brain”到“brain 可以替换”

一旦 OpenClaw 用 MCP 暴露消息能力，Claude Code、Codex、Cursor、cron job，甚至其他 agent，都可以成为上层大脑。OpenClaw 不再必须自带完整思考层，反而能专注做好渠道连接与消息路由。

### 3. 从“AI 是 moat”到“集成与渠道才是 moat”

主帖作者作为云服务提供方，把护城河重新定义得很现实：

- 维护 50+ messaging adapters
- 管理 OAuth 与 bot pools
- 处理 rate limits
- 保证稳定投递与多租户运行

这些基础设施工作，才是真正难复制、但用户又离不开的部分。

## 时间线总结

- **2026-03-28**：Peter 发出源头帖，预告下一版 OpenClaw 将成为 MCP，并可连接更广泛的消息 provider。
- **2026-03-29 22:47**：Michael 发长帖，完整解释这对 OpenClaw、云服务商和 agent 协作意味着什么。
- **随后数小时**：讨论开始转向具体 PR、MCP transport、runtime 架构，以及 OpenClaw 在 agent 栈里的新角色。

## 一句话结论

这条线程真正传递的不是“OpenClaw 新增了一个开发者接口”，而是：**OpenClaw 正在从一个带消息能力的 agent 壳，转成 agent 生态里的通用消息底座。** 一旦 HTTP/SSE transport、persistent lifecycle、签名等能力成熟，它的产品位置会比现在大得多。