# Ragas 评估框架与幻觉测量

> tags: rag, ragas, 评估, faithfulness, answer-relevance
> weight: 2
> updated: 2026-08-04

## 核心结论
Ragas 是 RAG 无参考（reference-free）评估框架，用 LLM-as-judge 自动打分，不需要人工标注答案。核心指标：Faithfulness（忠实度，测幻觉）、Answer Relevance（答案相关性）、Context Precision/Recall（检索质量）。测幻觉主要看 Faithfulness：把答案拆成原子声明，逐条判定能否被检索上下文支撑，比例即得分。

## 展开
- **Faithfulness 计算逻辑**：① LLM 把答案拆成若干原子声明（statements）；② 对每个声明问 LLM"给定上下文是否支持该声明"（是/否）；③ 得分 = 被支撑声明数 / 总声明数。分低 = 答案里有上下文之外的内容 = 幻觉。
- **Answer Relevance 计算逻辑**：让 LLM 根据生成的答案**反向生成若干可能的问题**，计算这些问题与原始问题的 embedding 相似度均值——答案答非所问时反推问题会偏离原问题，分低。
- **Context Precision / Recall**：评估检索端——召回的 chunk 里相关的排前面没有（precision）、该召回的召回全没有（recall，需要 ground truth）。
- **为什么引入**：无标注可自动评、可批量回归（改 chunk 策略/prompt 后对比分数）、指标分维定位问题在检索还是生成。
- 局限：LLM judge 本身有偏/成本；重要结论抽样人工复核。

## 关键细节 / 易错点
- 追问"Ragas 怎么测幻觉"：Faithfulness，注意它测的是"相对给定上下文的幻觉"，不测世界知识层面的对错。
- 追问"评估集怎么来"：线上真实 query + 检索上下文 + 生成答案即可跑（无参考）；要 Context Recall 才需标注。

## 关联
- 相关知识点：[[wiki/rag/rag-pipeline.md]]、[[wiki/llm/hallucination.md]]、[[wiki/llm-agent/eval-dataset-construction.md]]
- 常见追问链：为什么引入 Ragas → 用了哪几个指标 → Faithfulness 怎么算 → 局限

## 面经来源
- 美团 AI Agent 开发一面（2026-08）：用 Ragas 评估的时候怎么测幻觉
- 快手 AI 应用开发一面（2026-08）：为什么引入 Ragas、用了哪几个指标、Faithfulness 和 Answer Relevance 的具体计算逻辑
