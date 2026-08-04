# 面经：快手 AI 应用开发 一面

> tags: project, 面经, 快手, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
日常实习（年初面的），41min，回忆版（来源：抖音 @Devs 截图，2026-08）。有手撕。

## 题目清单
1. 自我介绍
2. 拷打项目一（工业文档 RAG）：
   - MinerU 解析工业文档时如何处理图文混排？为什么选择 MinerU？→ [[wiki/rag/document-parsing.md]]
   - 多模态检索中文本和图片如何映射到同一个向量空间？→ [[wiki/rag/multimodal-rag.md]]
   - 为什么引入 Ragas 评测？用了哪几个指标？Faithfulness 和 Answer Relevance 的具体计算逻辑？→ [[wiki/rag/ragas-evaluation.md]]
3. 拷打项目二（心理咨询 Agent）：
   - LangGraph 相比 LangChain 有什么优势？状态快照机制大概怎么实现的？→ [[wiki/llm-agent/langchain-vs-langgraph.md]]
   - 向量记忆库如何更新用户画像？如何区分短期记忆和长期记忆？→ [[wiki/llm-agent/agent-memory-design.md]]
   - 安全护栏如何实现敏感词拦截？→ [[wiki/llm-agent/safety-guardrails.md]]
4. RAG 中文档切片的粒度如何选择？→ [[wiki/rag/chunking-strategies.md]]
5. 向量数据库索引中 IVF_FLAT 和 HNSW 有什么区别？各自适用场景？→ [[wiki/rag/vector-index.md]]
6. 什么是 CoT？为什么能提高模型处理复杂任务的能力？→ [[wiki/llm/cot.md]]
7. 大模型应用中常见的幻觉有哪些类型？工程上如何缓解？→ [[wiki/llm/hallucination.md]]
8. 介绍一下 Function Call 的流程，模型是如何知道该调用哪个工具的？→ [[wiki/llm-agent/tool-call-accuracy.md]]
9. 介绍一下 vLLM 中 PagedAttention 的原理 → [[wiki/llm/paged-attention.md]]
10. Python 中列表和元组的区别 → [[wiki/python/list-vs-tuple.md]]
11. 介绍一下 Python 中的装饰器及其应用场景 → [[wiki/python/decorator.md]]
12. HTTP 协议中 GET 和 POST 请求的区别？→ [[wiki/network/http-get-vs-post.md]]
13. 手撕：无重复字符的最长子串 → [[wiki/algorithm/longest-substring-without-repeating-chars.md]]

## 考察重点
项目深挖（MinerU/Ragas/LangGraph/记忆/护栏）+ 广谱八股（索引、CoT、PagedAttention、Python、HTTP）+ 手撕。
