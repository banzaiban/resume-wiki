# LangChain 与 LangGraph 的差异

> tags: llm-agent, langchain, langgraph, 框架, 编排
> weight: 1
> updated: 2026-07-30

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
- 追问"为什么 Agent 需要循环"：工具结果决定下一步，步数不可预知，必须 while 循环而非固定管道。
- 追问"LangGraph 的 State 怎么管"：reducer 定义字段合并方式（如 messages 追加），checkpoint 存每步快照。
- 追问"为什么后来自己封装"：控制力（重试/超时/降级细节）、调试透明度、减少依赖升级风险——答出权衡即可。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-vs-workflow.md]]、[[wiki/llm-agent/agent-patterns.md]]
- 常见追问链：差异 → 为什么需要图/循环 → State/checkpoint 机制 → 什么时候自研

## 面经来源
淘宝闪购 AI 应用开发一面（2026-07）
