# Karpathy × Chris Park：X API Pay-Per-Use 讨论链

## 事件起点 / 源头帖

- 作者：Chris Park
- 帖子角色：公开路线图帖
- 链接：<https://x.com/chrisparkX/status/1989191276885946622>
- 时间：已抓到公开页面，但当前未补到更精确的原帖发布时间显示

### 中文原文

> 我们在 X 和 Grok 的核心产品上取得的进展非常深刻：
>
> - Grokipedia 上线
> - Grok Imagine 升级，15 秒版本也快来了
> - Grok 5 正在训练中
> - 加密版 XChat 正在推出
> - XMoney 即将到来
> - X API 按量付费测试版
> - 完全由 Grok 驱动的 X Feed 即将上线
> - App 下载量创历史新高
>
> 我们正在以疯狂的速度推进。现在仍然让人觉得一切才刚开始，这恰恰说明了 xAI 指数级增长空间的存在。来加入我们吧！

## 目标帖 / 主回复

- 作者：Andrej Karpathy
- 帖子角色：回复 Chris Park 的公开路线图帖
- 链接：<https://x.com/karpathy/status/2040838208674734473>
- 时间：2026-04-05 17:05

### 中文原文

> @chrisparkX 我觉得这是一个好的方向（针对读取接口，不针对写入接口）。我大约两周前为了一个项目试着用过它，但大概 30 分钟的折腾就花了我 200 美元，我认为这个定价真的太夸张了。那些文档也很难喂给 agent，因为它们是很多彼此分开的短页面。我觉得更合适的做法是提供一份大的 Markdown 入门文档，或者几份也行，并且能通过简单的 curl 地址直接拿到。另外，当前版本的文档里似乎完全没有提到 XMCP？至少 Search / Grok Assistant 给我的结果是，文档里任何地方都 0 次提到这个东西。

## 当前已经明确的链路关系

- `Chris Park` 先公开抛出：`X API Pay-Per-Use Beta`
- `Karpathy` 随后直接回复这条路线图帖
- `Karpathy` 的回复内容集中在四个点：
  - 只认可 **Read endpoints**
  - 不认可直接推广到 **Write endpoints**
  - 试错成本太高（30 分钟约 200 美元）
  - 文档不适合 agent ingest，且 XMCP 在文档中几乎不可见

## 当前缺失链路

### 缺失 1
- Chris Park 这条源头帖里，`X API Pay-Per-Use Beta` 只是被列进路线图
- 当前还没有补到它对应的更详细原始说明帖或官方页面

### 缺失 2
- Karpathy 提到的“官方文档”原文，这次还没有完整并入
- 因此现在只能看到他对文档的批评，还看不到他所指向的那份文档本体

### 缺失 3
- XMCP 在这场讨论里的上游定义链还没补到
- 目前只能确认：Karpathy 认为当前文档中几乎没有提到它

## 当前可直接阅读的最短主链

1. Chris Park 在路线图帖里公开列出：`X API Pay-Per-Use Beta`
2. Karpathy 直接回复这条帖
3. 回复里明确表达：
   - 这个方向本身可以
   - 但只针对读取接口
   - 价格过高
   - 文档太碎，不适合 agent
   - XMCP 在文档里几乎不可见
