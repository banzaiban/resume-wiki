# 面经：美团 AI Agent 开发 一面

> tags: project, 面经, 美团, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
暑期实习，50min+，回忆版（来源：抖音 @Devs 截图，2026-08）。无手撕。结构：自我介绍 → 拷打实习 → 拷打两个项目 → 八股 → 反问。

## 题目清单
1. 自我介绍
2. 拷打实习
3. 拷打项目一（工业文档 RAG）：
   - 工业 PDF 里经常跨页的超大表格，切片时怎么处理的？怎么保证表格不被切碎导致模型看不懂？→ [[wiki/rag/document-parsing.md]]
   - 多模态检索里图片向量和文本向量混合后，怎么给两路检索分配权重？→ [[wiki/rag/multimodal-rag.md]]
   - 用 Ragas 评估的时候怎么测幻觉？→ [[wiki/rag/ragas-evaluation.md]]
4. 拷打项目二（心理咨询 Agent）：
   - 用户情绪波动很大，怎么用 LangGraph 控制对话走向、不让模型说不合适的话？→ [[wiki/llm-agent/langchain-vs-langgraph.md]]、[[wiki/llm-agent/safety-guardrails.md]]
   - 用户长期画像和短期聊天历史分别存在哪里？怎么把两部分记忆喂给大模型？→ [[wiki/llm-agent/agent-memory-design.md]]
   - MCP 在项目里具体解决了什么问题？不用它的话怎么实现工具调用？→ [[wiki/llm-agent/mcp.md]]
5. 向量检索和关键词检索各适合什么场景？为什么现在做混合检索更多？→ [[wiki/rag/rag-pipeline.md]]
6. 为什么要加 Reranker 重排？直接拿向量检索结果给模型会有什么问题？→ [[wiki/rag/reranker.md]]
7. 文档切片为什么要有 Overlap？主要解决什么问题？→ [[wiki/rag/chunking-strategies.md]]
8. 工程和 Prompt 设计上有哪些手段防止大模型产生幻觉？→ [[wiki/llm/hallucination.md]]
9. ReAct 的原理，跟单轮问答最本质的区别在哪？→ [[wiki/llm-agent/agent-patterns.md]]
10. 怎么保证大模型每次都稳定输出 JSON 调工具？JSON 格式不对代码里怎么兜底？→ [[wiki/llm-agent/tool-call-accuracy.md]]
11. Agent 多步任务很慢，怎么把中间思考步骤和工具执行状态流式传输实时返回给前端？→ [[wiki/llm-agent/agent-streaming.md]]
12. 怎么理解 Agent Harness？没有它大模型会遇到什么瓶颈？→ [[wiki/llm-agent/agent-harness-skills.md]]
13. Skills 和普通 Tool 调用有什么区别？→ [[wiki/llm-agent/agent-harness-skills.md]]
14. 不微调模型的话，怎么实现 Agent 自进化？→ [[wiki/llm-agent/agent-harness-skills.md]]
15. Python 异步编程在 Agent 应用开发里有什么用处？哪些场景必须用异步？→ [[wiki/python/asyncio-vs-threading-mutex.md]]
16. 反问

## 考察重点
RAG 工程细节（表格/多模态/评估）、Agent 框架（LangGraph/MCP/Harness/Skills）、流式与异步工程。
