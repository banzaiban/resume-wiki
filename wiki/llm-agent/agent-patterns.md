# 常见 Agent 运行模式

> tags: llm-agent, ReAct, plan-and-execute, self-refine, multi-agent
> weight: 1
> updated: 2026-07-30

## 核心结论
四类主流模式：ReAct（思考→行动→观察循环，边想边做）、Plan-and-Execute（先出完整计划再逐步执行，可重规划）、Self-Refine/Reflexion（生成→自我批评→修正迭代）、Multi-Agent 协作（分工/辩论/supervisor 路由）。

## 展开
- **ReAct**（Reason + Act）：每步输出推理 + 工具调用，观察结果进入下一轮，直到能回答。优点：灵活、能根据中间结果调整；缺点：短视（无全局规划）、步数多成本高。适合探索型多步任务。
- **Plan-and-Execute**：先让模型产出任务分解计划，executor 逐步执行，必要时 re-plan。优点：全局视角、子步可用便宜模型、可并行；缺点：计划赶不上变化时要重规划。适合结构清晰的复杂任务。
- **Self-Refine / Reflexion**：生成初稿 → 模型自评（或用外部反馈如测试结果）→ 修正。Reflexion 把失败反思写入记忆供下次尝试。适合代码生成（跑测试作反馈）、写作打磨。
- **Multi-Agent**：
  - supervisor/router：一个协调者分派给专家 Agent；
  - 辩论（debate）：多 Agent 互相质疑提升事实性；
  - 流水线分工：如"产品-开发-测试"角色协作（MetaGPT 式）。
  代价：token 成本倍增、错误可能级联，需要明确收益再用。

## 关键细节 / 易错点
- 追问"ReAct 和 Plan-and-Execute 怎么选"：任务步骤可预知 → 先规划省成本；探索性强、依赖中间结果 → ReAct。
- 追问"Self-Refine 的反馈哪来的"：最好是**外部客观信号**（测试通过、编译错误），纯自我感觉的批评提升有限。
- 追问"多 Agent 一定比单 Agent 好吗"：不一定，通信开销和错误传播可能抵消收益，先把单 Agent 调到上限。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-vs-workflow.md]]、[[wiki/llm-agent/langchain-vs-langgraph.md]]
- 常见追问链：各模式原理 → 怎么选 → 反馈信号 → 多 Agent 的代价

## 面经来源
淘宝闪购 AI 应用开发一面（2026-07）
