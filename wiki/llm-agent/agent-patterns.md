# 常见 Agent 运行模式

> tags: llm-agent, ReAct, plan-and-execute, self-refine, multi-agent
> weight: 6
> updated: 2026-08-04

## 核心结论
四类主流模式：ReAct（思考→行动→观察循环，边想边做）、Plan-and-Execute（先出完整计划再逐步执行，可重规划）、Self-Refine/Reflexion（生成→自我批评→修正迭代）、Multi-Agent 协作（分工/辩论/supervisor 路由）。

## 展开
- **ReAct**（Reason + Act）：循环状态机——`Thought`（分析当前状态、推理下一步）→ `Action`（选工具 + 给参数）→ `Observation`（工具结果回填上下文）→ 回到 Thought，直到模型输出最终答案或触达步数上限。优点：灵活、能根据中间结果调整；缺点：短视（无全局规划）、步数多成本高。适合探索型多步任务。
  - **与单轮问答的本质区别**：单轮是一次性用参数知识生成；ReAct 把推理与外部环境交互交织，中间结果接地（grounding）、可纠偏——模型每步都能基于真实观察修正方向，所以复杂任务表现显著更好。
  - **约束动作空间防止乱跳**（ReAct 多步时动作空间大）：阶段化工具集（当前阶段只暴露相关工具）、有限状态机约束合法转移、步数/同工具调用次数上限、用 prompt 给出流程骨架（半 workflow 化）。
- **Plan-and-Solve / Plan-and-Execute**：先让模型产出任务分解计划，executor 逐步执行，必要时 re-plan。优点：全局视角、子步可用便宜模型、可并行；缺点：计划赶不上变化时要重规划。适合结构清晰的复杂任务。
  - **vs ReAct（长任务场景）**：Plan 有全局视角、不易偏航，适合步骤可预知的复杂长任务；ReAct 灵活但短视，适合路径依赖中间结果的探索型任务。实践可混合：Plan 定骨架 + 每步 ReAct 执行 + 失败时 replan。
- **Self-Refine / Reflexion / Self-Reflection**：生成初稿 → 模型自评（或用外部反馈如测试结果）→ 修正。Reflexion 把失败反思写入记忆供下次尝试。适合代码生成（跑测试作反馈）、写作打磨。
  - **Self-Reflection 触发重新规划的设计**：触发条件——工具结果不符预期（空结果/错误码/schema 校验失败）、critic 自检发现矛盾、置信度低于阈值；动作——把"哪一步错了、为什么、下一步怎么改"的诊断文本写入上下文，回到规划节点重试。
  - **如何识别输出中的逻辑错误**：外部客观信号优先（测试、校验器、规则），纯模型自评（critic prompt 对照目标逐步检查）为辅——自我感觉的批评提升有限。
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
- 淘宝闪购 AI 应用开发一面（2026-07）
- 美团 AI Agent 开发一面（2026-08）：ReAct 原理、与单轮问答最本质的区别
- 美团 AI Agent 开发二面（2026-08）：Plan-and-Solve 流程、长任务下与 ReAct 的区别
- 阿里云 AI 应用研发一面（2026-08）：ReAct 动作空间太大乱跳，怎么约束工具调用顺序
- 未知公司面经片段A（2026-08）：ReAct 中 Thought/Action/Observation 循环状态机；工具结果不符预期时 Self-Reflection 触发重新规划
- 未知公司面经片段B（2026-08）：ReAct 为什么提升复杂任务理解能力；Self-Reflection 如何识别输出中的逻辑错误
