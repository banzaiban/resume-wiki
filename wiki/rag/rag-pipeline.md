# RAG 完整链路

> tags: rag, 向量检索, 混合召回, 重排序, chunking, embedding
> updated: 2026-07-30

## 核心结论
离线：文档解析 → 切片（chunking）→ embedding 向量化 → 存向量库。在线：query 改写 → 混合召回（向量 + 关键词 BM25）→ 重排序（cross-encoder）→ 取 top-k 拼上下文 → LLM 生成 → 引用标注来源。

## 展开
**离线建库**
- 解析：PDF/HTML/表格清洗，保留标题层级等结构信息。
- 切片：按语义/标题切，常见 200-500 token，相邻块 10-20% overlap 防止语义截断；块带元数据（来源、标题、时间）。
- 向量化：选 embedding 模型（如 bge、text-embedding 系列），存入向量库（Milvus/pgvector/ES 向量插件），建 HNSW 索引。

**在线检索**
- query 预处理：改写（补全指代、拆多跳问题）、可选 HyDE（用假设答案去检索）。
- 混合召回：向量检索抓语义相似 + BM25 抓精确关键词（术语、编号、人名），两路结果用 RRF（reciprocal rank fusion）合并——纯向量对精确匹配弱，纯关键词对同义改写弱，混合互补。
- 重排序：cross-encoder（如 bge-reranker）对 query-doc 对精排，比双塔 embedding 更准但更慢，所以只对召回的几十条做。
- 组装：top 3-5 段拼 prompt，带来源标识；控制总长度防 lost-in-the-middle。
- 生成：要求模型只依据给定上下文回答、注明引用来源、检索不到就说不知道（降幻觉）。

## 关键细节 / 易错点
- 效果瓶颈通常在**检索**不在生成：先评检索命中率（recall@k、MRR），再看端到端。
- 切片太碎 → 上下文不完整；太大 → 噪声多、embedding 语义稀释。
- 追问"为什么要重排序"：召回追求快和全（近似检索），精度不够；rerank 用交互式打分补精度。
- 追问"怎么评测 RAG"：检索指标（recall/MRR）+ 生成指标（忠实度 faithfulness、答案相关性），可用 RAGAS 框架。
- 追问"多跳问题"：query 分解 / 迭代检索（检索→部分回答→再检索）/ Agentic RAG。
- 更新问题：文档变更要增量重建索引，注意新旧版本共存时的一致性。

## 关联
- 相关知识点：[[wiki/llm/prompt-engineering-principles.md]]、[[wiki/database/database-selection-for-business.md]]
- 常见追问链：chunk 怎么切 → 为什么混合召回 → rerank 原理 → 怎么评测 → 幻觉怎么降

## 面经来源
快手 Agent 研发一面（2026-07）
