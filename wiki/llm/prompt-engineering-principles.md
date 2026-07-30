# 什么样的 Prompt 效果好

> tags: llm, prompt-engineering, few-shot, 结构化输出
> weight: 2
> updated: 2026-07-30

## 核心结论
三要素：指令直白无歧义、给 2-3 个高质量例子（few-shot）、输出格式写死（JSON Schema/固定模板）。结构上角色和约束放前面、例子紧跟、任务输入放最后；需要稳定输出时把 temperature 调低。

## 标准 Prompt 结构
`系统指令/角色` → `背景知识（含 RAG 上下文）` → `规则与约束` → `few-shot 示例` → `用户输入` → `输出格式要求`。重要指令放开头，关键约束在结尾再重申一次。

## 展开
- **直白**：用祈使句写清做什么、不做什么；避免"尽量、可能"这类模糊词；一条指令一件事，复杂任务拆步骤（先分析再输出）。
- **Few-shot**：例子比描述更有效，2-3 个覆盖典型 + 边界 case；例子的格式就是模型模仿的格式，所以例子本身要和期望输出严格一致。
- **格式说死**：给出 JSON Schema 或逐字段说明，配合"只输出 JSON，不要解释"；生产上再加结构化输出约束（function calling / JSON mode）双保险。
- **结构顺序**：system role（角色+红线约束）→ 规则 → few-shot 例子 → 用户输入。重要约束放开头和结尾（模型对首尾更敏感，中间易丢失 — lost in the middle）。
- **参数**：分类/抽取类任务 temperature 0~0.3；创意生成类可高。top_p 与 temperature 一般只调一个。
- **迭代方法**：建小评测集 → 改 prompt → 跑分对比，而不是凭感觉；prompt 版本化管理。

## 关键细节 / 易错点
- 负面指令（"不要 X"）效果弱于正面指令（"只做 Y"），能改写就改写。
- 例子数量不是越多越好：过多占上下文、还可能让模型过拟合例子的表面模式。
- 追问"CoT 什么时候用"：推理/数学/多步任务加 "think step by step" 或给带推理过程的例子有效；简单分类任务反而增加延迟和成本。
- 追问"prompt 太长怎么办"：删冗余规则、把稳定知识挪到 RAG、例子换成更短的典型 case。

## 关联
- 相关知识点：[[wiki/llm/lost-in-the-middle.md]]、[[wiki/llm-agent/tool-call-accuracy.md]]、[[wiki/llm-agent/intent-classification-optimization.md]]、[[wiki/rag/rag-pipeline.md]]
- 常见追问链：few-shot 怎么选例子 → 格式约束怎么保证 → temperature 原理 → CoT 适用场景

## 面经来源
- 快手 Agent 研发一面（2026-07）：什么样的 Prompt 效果好
- 淘宝闪购 AI 应用开发一面（2026-07）：标准 Prompt 结构包含哪些、Prompt 工程常用优化手段
