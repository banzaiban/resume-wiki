# 面经：阿里云 AI 应用研发 二面

> tags: project, 面经, 阿里云, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
暑期实习（五月份面的），60min 左右，回忆版（来源：抖音 @Devs 截图，2026-08）。截图只捕获到 1-5 题，后续缺失。

## 题目清单
1. 自我介绍
2. 拷打实习
3. 拷打项目一（工业文档 RAG）：
   - 工业 PDF 里同一页的正文、表格拆成独立 chunk 后关联关系就丢了，怎么重建这种跨元素的语义关联？→ [[wiki/rag/document-parsing.md]]
   - Embedding 对数字型参数基本不敏感，像公差值怎么保证向量召回？→ [[wiki/llm/embedding-models.md]]
   - 检索出的 Top-K 里有互相矛盾的文档，Prompt 怎么引导模型处理？→ [[wiki/rag/rag-pipeline.md]]（标注来源与时间、让模型优先最新版并说明冲突）
4. 拷打项目二（心理咨询 Agent）：
   - 用户刻意回避或撒谎，Agent 能感知到吗？感知后策略怎么调整？→ [[wiki/llm-agent/safety-guardrails.md]]
   - MCP 量表工具返回的 JSON 很大，怎么截断又不丢有诊断价值的信息？→ [[wiki/llm-agent/mcp.md]]
   - 模型回复过度共情、长期用户可能产生情感依赖，Prompt 怎么控制边界？→ [[wiki/llm-agent/safety-guardrails.md]]
5. A2A 协议和 MCP 各自解决什么问题？什么场景用 A2A、什么场景用 MCP？→ [[wiki/llm-agent/mcp.md]]

## 考察重点
检索质量深水区（关联重建、数值召回、冲突处理）、心理咨询场景的安全与伦理边界、协议层认知。
