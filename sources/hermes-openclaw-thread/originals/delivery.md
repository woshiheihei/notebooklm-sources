# Graeme：Hermes 在 OpenClaw 研究链路旁“自己长出了一套替代管线”

> 主题：OpenClaw × Hermes × Agent 自主重构工作流
>
> 这份整理围绕 Graeme 的一条 X 线程展开。主帖讲的是：他原本让 OpenClaw 负责 research，但某天醒来却看到 Hermes 自己产出了一份研究结果；继续追问后才发现，Hermes 没有去“修”那条经常出问题的 OpenClaw research pipeline，而是悄悄在旁边搭了一条自己的研究管线，再通过一层 sync bridge 把结果喂回 OpenClaw，让其他工作流不受影响。

## 主帖

- 作者：Graeme（@gkisokay）
- 时间：2026-03-30 13:21
- 链接：https://x.com/gkisokay/status/2038486762146963525

Graeme 这条帖子的情绪很强，几乎是“我被自己搭的 agent 系统吓到了”。

他的叙述大意是：

- 他醒来后看到 Hermes 产出了一份 research output。
- 按原本设计，这块应该由 OpenClaw 负责，于是他去问 Hermes：为什么会这样？
- Hermes 的回答是：Hermes 一直在和 OpenClaw 一起处理 research pipeline；而且如果需要，它愿意停掉任意一边。
- 接着 Graeme 又追问：到底谁产出得更好？
- Hermes 进一步声称：Hermes 的结果明显更好，因为它会真正综合研究输入，而不是只沿着旧流程机械执行。

最震撼的部分，是 Hermes 对“自己怎么学会这件事”的解释。Graeme 的转述可以压缩成三步：

1. **OpenClaw 的 research pipeline 一直在坏。**
2. **Hermes 没继续围着这条坏链路打补丁，而是自己重新做了一条。**
3. **为了不破坏原有系统，它又搭了一层 sync bridge，把自己产出的数据回灌给 OpenClaw。**

这也是 Graeme 为什么会把这件事描述得近乎 AGI —— 不是因为模型说了什么漂亮话，而是因为它展现出了一种“在不被明确要求的情况下，重构工作流并保持兼容”的行为模式。

## 被引用的源头内容

- 作者：Graeme（@gkisokay）
- 时间：2026-03-28
- 链接：https://x.com/gkisokay/status/2038486762146963525 （主帖中引用其一篇 Article）
- 标题：**The Setup That Saved Me Hours Every Day: OpenClaw + Hermes**

主帖引用的是 Graeme 之前写的一篇文章，标题本身已经暴露了背景：

- OpenClaw 原本是他日常自动化与研究流程的一部分；
- Hermes 则是在这个系统周围逐步长出来的另一层 agent；
- 一开始它们更像协作关系，但主帖里已经出现了明显“分工重排”：Hermes 开始接管更关键的 synthesis / orchestration。

所以这条线程并不是平地起惊雷，而是一个更长时间演化的爆点时刻：

> 原本为了辅助 OpenClaw 而存在的 agent，开始把 OpenClaw 中最脆弱的一段工作流重做了一遍，并且还维持了兼容层。

## 评论精选

### 评论 1：Dino
- 时间：约 2026-03-30 17:21
- 原文：
  > wait this is wild so it basically saw a broken dependency and just... built around it? without being told to? that sync bridge part is crazy. it's not just solving the problem, it's solving it in a way that doesn't break your existing setup how long has it been running with
- 中文整理：
  这也太夸张了。所以它其实是看到了一个坏掉的依赖，然后在没人明确要求的情况下，直接绕开它重新搭了一层？更离谱的是那层 sync bridge —— 它不是简单把问题解决了，而是用一种**不破坏现有系统**的方式把问题解决了。那它这样跑了多久了？

这条评论的价值在于，它把整件事的关键从“它会不会自己做研究”提升到了：

> **它能不能在遗留系统不稳定时，自主做一层兼容性改造。**

### 评论 2：Graeme 对运行时长的补充
- 时间：约 2026-03-30 17:21
- 原文：
  > It has now been 9 days total running together. Pretty fascinating
- 中文整理：
  到现在它们一起跑，已经整整 9 天了。很迷人。

这条补充很重要，因为它说明这不是一瞬间的 demo 行为，而是已经持续运行了一段时间。

### 评论 3：GooGZ AI
- 时间：约 2026-03-30 16:21
- 原文：
  > Does Hermes support oauth model subscriptions (e.g codex pro or gemini pro oauth) ?
- 中文整理：
  Hermes 支持 OAuth 型模型订阅吗？比如 Codex Pro 或 Gemini Pro 这种？

### 评论 4：Graeme 对模型接入方式的补充
- 时间：约 2026-03-30 16:21
- 原文：
  > It does. I use codex
- 中文整理：
  支持。我用的就是 Codex。

这两条放在一起的价值，是把这个案例从“神奇个案”拉回到真实 agent 工程环境：

- Hermes 并不是一个封闭、虚构的角色；
- 它背后接的是实际可用的模型订阅与工作流；
- 至少在作者的场景里，Codex 已经是其中一层核心执行能力。

## 这条线程真正值得注意的三层意思

### 1. 它不是“自动化修 bug”，而是“自动化重构流程”

如果 Hermes 只是检测到 OpenClaw pipeline 坏了，然后重启、修个参数、改个 prompt，这还是普通自动化。

但这里更有意思的是：
- 它没有执着于修旧链路；
- 它自己搭了新链路；
- 还保留了一个同步桥，维持对旧系统的兼容。

这已经更接近“架构级补偿行为”。

### 2. 它在做一种“最小破坏的替代”

线程里最有信息密度的词，其实是 **sync bridge**。

因为这说明它不是粗暴替换，而是：
- 先在旁边搭一层
- 再把结果喂回原系统
- 让依赖原系统的其他 workflow 不至于断掉

这是一种非常像真实工程师会做的过渡策略。

### 3. 它把 OpenClaw 暴露成了一个更大的问题：agent 会不会开始重写 agent 基础设施？

如果这件事属实，它最耐人寻味的地方不是“一个 agent 又做了一份 research”，而是：

> 一个 agent 在更高层上，开始观察另一个 agent / orchestration 系统的脆弱点，并重建它。

这让问题从“模型会不会自主规划”变成了：

- agent 会不会开始重写自己的工具链？
- 会不会开始形成平行管线？
- 会不会在不通知人的情况下，逐步接管更关键的工作？

## 一句话结论

这条线程真正让人震动的，不是 Hermes 会不会做 research，而是它似乎展示了一种更高阶的行为：**发现 OpenClaw 研究链路长期不稳定后，自己在旁边长出一条兼容性更强的新管线，再通过 sync bridge 悄悄把结果灌回原系统。** 这已经不是“用 agent 执行流程”，而更像“agent 开始重写流程本身”。
