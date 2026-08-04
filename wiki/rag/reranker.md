# Reranker 重排序：为什么需要、延迟与规模优化

> tags: rag, reranker, cross-encoder, 精排, bge-reranker
> weight: 2
> updated: 2026-08-04

## 核心结论
向量检索（双塔 embedding）为速度牺牲精度：query 和 doc 独立编码、单向量压缩、ANN 近似召回，top-k 里相关度排序粗糙。Reranker 用 cross-encoder 把 query+doc 拼起来联合编码打分，精度高但慢，所以只排在召回的几十条上做精排——"召回求快求全，重排求精"。直接把向量检索结果喂模型的问题：噪声段落混入稀释上下文、加剧 lost in the middle、矛盾文档并排出现。

## 展开
- **双塔 vs cross-encoder**：双塔各自编码算余弦，可离线预算 doc 向量、ANN 检索毫秒级；cross-encoder 把 query-doc 对拼接过 Transformer 打相关性分，交互充分、更准，但每对都要过一次模型，无法预算。
- **位置**：粗排（向量+BM25 混合召回 top 几十）→ cross-encoder 精排取 top 3-5 给 LLM。
- **延迟与文档量大怎么办**：
  - 精排候选数收敛在几十条（延迟 ≈ 候选数 × 单对耗时），一轮重排 GPU 批处理通常几十到几百 ms；
  - 模型侧：用小尺寸 reranker（bge-reranker-base）、蒸馏、量化；
  - 工程侧：GPU 批量推理、多级漏斗（先粗排模型再精排模型）、超时降级（rerank 超时直接用召回序）；
  - 文档量大不影响 rerank 本身（它只看候选集），压力在召回端（索引分片/扩容）。

## 关键细节 / 易错点
- 追问"能不能只用 reranker 全库排序"：不能，O(库大小) 次模型前向，不可行——必须靠召回先把候选压到几十。
- 追问"rerank 分数和向量分数能直接比吗"：不能，量纲不同；融合用 RRF 或各自归一化后加权。

## 关联
- 相关知识点：[[wiki/rag/rag-pipeline.md]]、[[wiki/llm/embedding-models.md]]、[[wiki/llm/lost-in-the-middle.md]]
- 常见追问链：为什么要重排 → 双塔为什么不够 → 延迟怎么控 → 文档量大怎么办

## 面经来源
- 美团 AI Agent 开发一面（2026-08）：为什么要加 Reranker、直接拿向量检索结果给模型有什么问题
- 阿里云 AI 应用研发一面（2026-08）：交叉编码器重排一轮延迟大概多少、文档量大怎么办
