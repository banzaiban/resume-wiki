# 面经：美团 AI Agent 开发 二面

> tags: project, 面经, 美团, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
暑期实习，65min 左右，回忆版（来源：抖音 @Devs 截图，2026-08）。无手撕。二面明显比一面深：检索进阶（Graph RAG / 改写 / Lost in the Middle）、Multi-Agent、可观测与限流。

## 题目清单
1. 自我介绍
2. 拷打实习
3. 拷打项目一（工业文档 RAG）：
   - 工业图纸上会有手写批注，这类非结构化图片信息怎么高精提取？→ [[wiki/rag/document-parsing.md]]
   - 工业规范每天都在更新，向量数据库怎么做增量同步？→ [[wiki/rag/rag-pipeline.md]]（增量重建/版本一致性）
   - 检索到的老版文档和新版规范冲突，怎么做过滤？→ [[wiki/rag/rag-pipeline.md]]（元数据版本过滤/时间衰减）
4. 拷打项目二（心理咨询 Agent）：
   - 用户聊天时突然切换话题，状态机怎么做上下文切换？→ [[wiki/llm-agent/agent-state-persistence.md]]
   - 高并发场景下 Agent 状态数据怎么持久化，保证服务不丢失会话？→ [[wiki/llm-agent/agent-state-persistence.md]]
   - 如何利用分类模型在输入端做实时预警拦截？→ [[wiki/llm-agent/safety-guardrails.md]]
5. 什么是语义分片？如何判断在哪里把文章切分？→ [[wiki/rag/chunking-strategies.md]]
6. 了解 Graph RAG 吗？相比向量检索，处理复杂知识关联有什么优势？→ [[wiki/rag/graph-rag.md]]
7. 了解 Lost in the Middle 现象吗？检索流程里怎么处理？→ [[wiki/llm/lost-in-the-middle.md]]
8. 查询重写有什么方法？怎么把用户提问改成更容易被检索的形式？→ [[wiki/rag/query-rewriting.md]]
9. Plan-and-Solve 的流程，处理复杂长任务时和 ReAct 有什么区别？→ [[wiki/llm-agent/agent-patterns.md]]
10. 怎么实现多工具并行调用？代码里怎么控制多个 API 并发执行并将结果一次性返回给模型？→ [[wiki/llm-agent/tool-parallel-execution.md]]
11. 对话轮数变多上下文很容易超限，一般怎么做记忆压缩？→ [[wiki/llm-agent/agent-memory-design.md]]
12. 什么情况下应该把复杂单体 Agent 拆分成多个专门的多 Agent 协同？拆分边界是什么？→ [[wiki/llm-agent/multi-agent-architecture.md]]
13. 什么是语义路由？进入 Agent 复杂决策前怎么用它快速做意图分流？→ [[wiki/llm-agent/intent-classification-optimization.md]]
14. 怎么自动评估 Agent 的执行轨迹？Agent 在工具调用里卡在死循环怎么在系统里监控并解决？→ [[wiki/llm-agent/agent-observability.md]]
15. 大模型 API 经常触发限流，用了什么工程手段处理高并发下的限流？→ [[wiki/llm-agent/llm-api-engineering.md]]
16. 有什么 AI coding 的技巧吗？→ [[wiki/ai-coding/vibe-coding-quality.md]]
17. 反问

## 考察重点
RAG 进阶、Multi-Agent 拆分、轨迹评估与死循环、限流、记忆压缩——偏系统设计与线上稳定性。
