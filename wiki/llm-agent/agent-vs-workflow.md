# Agent 与 Workflow 的取舍

> tags: llm-agent, workflow, 编排, 架构取舍
> weight: 2
> updated: 2026-08-04

## 核心结论
Workflow 适合步骤固定、可预知的任务（确定性、可控、便宜）；Agent 适合需要动态决策、探索推理的任务（灵活但不可控、贵）。生产上通常混用：确定的骨架写成 Workflow，需要选择和推理的节点交给 Agent。

## 展开
- **Workflow（静态编排）**：DAG/状态机，路径预先写死。优点：行为确定、易调试、易测试、延迟和成本可预算；缺点：遇到未编排的情况就死板。适合：审批流、固定 pipeline（如 RAG 的检索→重排→生成）。
- **Agent（动态决策）**：LLM 在循环中自主选工具、定步骤（ReAct 模式）。优点：处理开放任务、能应对没见过的情况；缺点：路径不可预测、可能跑偏/死循环、token 成本高、难测试。
- **混合模式（主流实践）**：外层 Workflow 定骨架（保证关键步骤必然发生、顺序可控），内层节点用 Agent 处理需要理解和选择的子任务；或 Agent 为主但受约束（限定工具集、步数上限、关键动作走人工确认）。
- 判断标准：任务路径能否穷举？能 → Workflow；不能穷举但目标清晰 → Agent + 约束；对可靠性要求极高的环节 → 一律 Workflow 化。
- Anthropic《Building effective agents》的观点：能用简单 workflow 解决就不要上 agent，复杂度要有收益证明。

## 关键细节 / 易错点
- 追问"Agent 跑偏怎么办"：步数上限、工具白名单、中间结果校验节点、关键操作 human-in-the-loop。
- 追问"怎么从 Workflow 演进到 Agent"：先全 Workflow 上线拿数据，找出规则覆盖不了的分支再局部 Agent 化。
- 成本视角：Workflow 单次 LLM 调用次数固定可控，Agent 的调用次数是开放的，预算要设上限。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-quality-attributes.md]]、[[wiki/llm-agent/tool-call-failure-handling.md]]
- 常见追问链：各自优缺点 → 什么时候必须 Agent → 混合怎么设计 → 跑偏怎么控

## 面经来源
- 快手 Agent 研发一面（2026-07）
- 阿里云 AI 应用研发一面（2026-08）：怎么判断业务场景走 Workflow 还是交给 Agent 自主决策
