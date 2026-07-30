# LLM-Wiki 索引

> 本 wiki 面向 **LLM 阅读**：信息密度优先，结构统一，便于检索。人类可读性次之。
> 维护规则：每次新增/修改知识点后，同步更新本文件的条目与摘要。

## 用法
- 定位：先在本文件按主题找到目标知识点，再打开对应 md。
- 每个知识点是一个自包含文件，路径 `wiki/<topic>/<slug>.md`。
- 文件模板见 `skill/resume/SKILL.md`。

## 主题分区

<!-- 约定主题目录（可按需增删）：
java/       Java 语言与 JVM
concurrency/ 并发与多线程
jvm/        JVM 内存/GC/类加载
mysql/      数据库与索引/事务
redis/      缓存
network/    计算机网络
os/         操作系统
distributed/ 分布式与中间件
algorithm/  算法与数据结构
system-design/ 系统设计
project/    项目经历与面经复盘
-->

## 知识点清单

<!-- 格式：- [标题](相对路径) — 一句话摘要 · updated YYYY-MM-DD -->

### python/
- [Python 异步与多线程的互斥](python/asyncio-vs-threading-mutex.md) — asyncio.Lock vs threading.Lock、GIL 下的并发模型选择、跨线程协程交互 · updated 2026-07-30

### llm/
- [什么样的 Prompt 效果好](llm/prompt-engineering-principles.md) — 指令直白 + few-shot 例子 + 输出格式说死；结构顺序与 temperature 调参 · updated 2026-07-30

### llm-agent/
- [Agent 工具调用失败处理](llm-agent/tool-call-failure-handling.md) — 重试(退避+幂等)→降级→监控告警→错误回传让模型自愈 · updated 2026-07-30
- [意图识别优化](llm-agent/intent-classification-optimization.md) — BERT 小模型主分类 + 规则补漏 + 低置信度大模型兜底的级联架构 · updated 2026-07-30
- [评测数据集构建](llm-agent/eval-dataset-construction.md) — 线上日志分层抽样 + 人工标注 + 增强改写，覆盖短头长尾，防泄漏定期换血 · updated 2026-07-30
- [Agent Memory 模块设计](llm-agent/agent-memory-design.md) — 短期 Redis 原文 / 中期摘要向量化+时间衰减 / 长期画像宽表，异步写入 · updated 2026-07-30
- [Agent 与 Workflow 的取舍](llm-agent/agent-vs-workflow.md) — Workflow 定骨架保确定性，Agent 处理需推理的节点，混合使用 · updated 2026-07-30
- [合格 Agent 需要具备的特性](llm-agent/agent-quality-attributes.md) — 稳定执行、工具准、上下文管住、自纠错、可观测、可中断恢复 · updated 2026-07-30
- [Bad Case 分析方法论](llm-agent/bad-case-analysis.md) — 坏例库打标→定期复盘→规则/prompt/训练数据分层修复→回归验证 · updated 2026-07-30

### rag/
- [RAG 完整链路](rag/rag-pipeline.md) — 切片→向量化→混合召回(向量+BM25)→重排序→拼上下文→生成标注来源 · updated 2026-07-30

### database/
- [业务数据的多数据库选型](database/database-selection-for-business.md) — MySQL 核心业务 / Redis 缓存 / 向量库 embedding / ES 日志搜索 / Neo4j 图谱 · updated 2026-07-30

### ai-coding/
- [Vibe Coding 交付质量保障](ai-coding/vibe-coding-quality.md) — 任务拆小、核心逻辑人审、关键单测、静态检查门禁 · updated 2026-07-30

### algorithm/
- [最长回文子串](algorithm/longest-palindromic-substring.md) — 中心扩展 O(n²) 必写；Manacher 用对称性复用信息达 O(n) · updated 2026-07-30
