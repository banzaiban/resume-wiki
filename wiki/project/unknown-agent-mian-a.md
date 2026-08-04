# 面经片段：未知公司 A（AI Agent 方向）

> tags: project, 面经, 未知公司, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
来源：抖音截图（2026-08），只捕获到第 6-17 题，公司与轮次未知。内容围绕 OpenClaw 项目、Multi-Agent、记忆与上下文。无手撕。

## 题目清单
6. 引入 HyDE 进行查询改写时，如何防止生成的伪文档产生幻觉干扰检索？→ [[wiki/rag/query-rewriting.md]]
7. 讲一下 ReAct 中 Thought、Action 和 Observation 的循环状态机逻辑 → [[wiki/llm-agent/agent-patterns.md]]
9. 如果模型发现上一步工具调用的结果不符合预期，如何设计 Self-Reflection 机制触发重新规划？→ [[wiki/llm-agent/agent-patterns.md]]
10. 如果工具规模达到上百个，如何设计 Tool Retrieval 来降低上下文压力？→ [[wiki/llm-agent/tool-call-accuracy.md]]
11. 讲一下 OpenClaw 的记忆实现 → [[wiki/llm-agent/agent-harness-skills.md]]
12. 随着模型上下文扩展到 1M+，外挂的向量记忆库未来会被长上下文完全取代吗？→ [[wiki/llm/long-context-vs-external-memory.md]]
13. 讲一下 Multi-Agent 分层架构与 P2P 架构的区别 → [[wiki/llm-agent/multi-agent-architecture.md]]
14. 讲一下如何优化智能体之间的通信损耗，防止冗余信息导致决策效率下降？→ [[wiki/llm-agent/multi-agent-architecture.md]]
15. 如何设计共享全局空间记忆来解决各智能体间上下文理解不一致的问题？→ [[wiki/llm-agent/multi-agent-architecture.md]]
16. 针对 Agent 推理链路，如何通过中间节点的轨迹有效性量化模型的表现？→ [[wiki/llm-agent/agent-observability.md]]
17. 反问

注：题号 8 在截图中缺失（截图序号直接从 7 跳到 9，原文如此）。
