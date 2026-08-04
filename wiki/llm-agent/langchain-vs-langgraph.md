# LangChain 与 LangGraph 的差异

> tags: llm-agent, langchain, langgraph, 框架, 编排
> weight: 3
> updated: 2026-08-04

## 核心结论
LangChain 是组件库 + 顺序链（LCEL 管道式组合 prompt/模型/解析器），适合线性流程；LangGraph 是**带状态的图**：节点 + 边 + 共享 State，支持条件分支、循环、人工介入和 checkpoint 持久化，适合多分支、需要循环迭代的 Agent。

## 展开
- **LangChain**：提供模型接入、prompt 模板、输出解析、检索器等标准组件；LCEL 把它们串成 DAG 式的链，数据单向流动，无内建循环概念。
- **LangGraph**：
  - StateGraph：定义共享 State（typed dict），节点是函数（读 State 返回更新），边可带条件路由；
  - 支持循环（Agent 的 think→act→observe 回到 think）——这是与 chain 的本质区别；
  - checkpoint：State 可持久化，支持中断恢复、time-travel、human-in-the-loop（暂停等人批准再继续）；
  - 多 Agent 编排：子图、supervisor 模式。
- 选择：一次性的 RAG 问答链 → LangChain 够用；需要工具循环、条件分支、状态回滚、人工审批的 Agent → LangGraph。
- 生产实践常见路径：早期 LangChain 快速搭链路 → 复杂后要么上 LangGraph 要么**自研轻量封装**（LangChain 抽象层深、调试黑盒、版本变动快是常见吐槽点，面试可以提）。

## 关键细节 / 易错点
- 一句话本质：chain 是无环的数据流，graph 是有状态可循环的状态机。
- 追问"状态快照（checkpoint）机制大概怎么实现"：每个 super-step（一轮节点执行）结束把完整 State 序列化持久化，checkpointer 可插拔（内存/SQLite/Postgres/Redis），用 thread_id 标识会话；有了逐快照历史就能崩溃恢复、time-travel 回任意节点、human-in-the-loop 挂起续跑。
- 追问"节点之间输入输出怎么约定"：共享 typed State（如 TypedDict），节点函数读 State 返回**部分更新**（只返回自己负责的字段），reducer 定义字段合并方式（messages 追加、其余覆盖）；跨节点的数据契约就是这个 State schema。
- 追问"中间节点报错了，图的状态怎么回滚"：状态本来就在 checkpoint 里，失败时从最近快照恢复重试；或用条件边把异常路由到错误处理节点（降级/人工），不会丢整图进度。
- 追问"为什么 Agent 需要循环"：工具结果决定下一步，步数不可预知，必须 while 循环而非固定管道。
- 追问"LangGraph 的 State 怎么管"：reducer 定义字段合并方式（如 messages 追加），checkpoint 存每步快照。
- 追问"为什么后来自己封装"：控制力（重试/超时/降级细节）、调试透明度、减少依赖升级风险——答出权衡即可。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-vs-workflow.md]]、[[wiki/llm-agent/agent-patterns.md]]
- 常见追问链：差异 → 为什么需要图/循环 → State/checkpoint 机制 → 什么时候自研

## 面经来源
- 淘宝闪购 AI 应用开发一面（2026-07）
- 快手 AI 应用开发一面（2026-08）：LangGraph 相比 LangChain 的优势、状态快照机制怎么实现
- 阿里云 AI 应用研发一面（2026-08）：状态图节点输入输出怎么约定、中间节点报错状态怎么回滚
