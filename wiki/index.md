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

<!-- 格式：- [标题](相对路径) — 一句话摘要 · w<N> · updated YYYY-MM-DD（w<N> = weight 考频权重；答错过的条目在 w<N> 后加 · wrong<N>，归 0 移除）-->

### python/
- [Python 异步与多线程的互斥](python/asyncio-vs-threading-mutex.md) — asyncio.Lock vs threading.Lock、GIL 下的并发模型选择、跨线程协程交互 · w1 · updated 2026-07-30

### llm/
- [什么样的 Prompt 效果好](llm/prompt-engineering-principles.md) — 指令直白 + few-shot + 输出格式说死；标准 Prompt 结构与 temperature 调参 · w2 · wrong1 · updated 2026-07-30
- [Transformer 与自注意力](llm/transformer-self-attention.md) — QKV 算权重、除 √dk 防 softmax 饱和、多头/KV Cache/因果 mask · w1 · updated 2026-07-30
- [LayerNorm](llm/layernorm.md) — 特征维归一化，NLP 不用 BN 的原因；Pre-LN vs Post-LN、RMSNorm · w1 · updated 2026-07-30
- [位置编码与长文本外推](llm/positional-encoding.md) — 绝对/相对/RoPE 旋转矩阵；PI/NTK/YaRN 插值、滑动窗口扩上下文 · w1 · updated 2026-07-30
- [Embedding 向量与模型选型](llm/embedding-models.md) — 对比学习训练、BGE/M3E 维度、指令前缀、难负例微调、单向量局限 · w1 · updated 2026-07-30
- [幻觉的原因与缓解](llm/hallucination.md) — 数据/模型/解码/对齐四层成因；RAG 接地 + prompt 约束 + 置信过滤 + 输出校验 · w1 · updated 2026-07-30
- [Lost in the Middle](llm/lost-in-the-middle.md) — 长上下文 U 型利用率；重排前置、只喂 top3-5、压缩、指令结尾重申 · w1 · updated 2026-07-30
- [Token 与字符的区别、分词](llm/tokenization.md) — BPE/BBPE 子词切分、中英 token 换算、对成本与字符级能力的影响 · w1 · updated 2026-07-30

### llm-agent/
- [Agent 工具调用失败处理](llm-agent/tool-call-failure-handling.md) — 重试(退避+幂等)→降级→监控告警→错误回传让模型自愈 · w1 · updated 2026-07-30
- [提高工具调用准确率](llm-agent/tool-call-accuracy.md) — 工具描述写细 + few-shot + JSON Schema 约束 + 校验重试；工具多则做工具检索 · w1 · updated 2026-07-30
- [意图识别优化](llm-agent/intent-classification-optimization.md) — BERT 小模型主分类 + 规则补漏 + 低置信度大模型兜底的级联架构 · w2 · wrong1 · updated 2026-07-30
- [评测数据集构建](llm-agent/eval-dataset-construction.md) — 线上日志分层抽样 + 人工标注 + 增强改写，覆盖短头长尾，防泄漏定期换血 · w1 · updated 2026-07-30
- [Agent Memory 模块设计](llm-agent/agent-memory-design.md) — 短期 Redis 原文 / 中期摘要向量化+时间衰减 / 长期画像宽表，异步写入 · w1 · updated 2026-07-30
- [Agent 与 Workflow 的取舍](llm-agent/agent-vs-workflow.md) — Workflow 定骨架保确定性，Agent 处理需推理的节点，混合使用 · w1 · updated 2026-07-30
- [常见 Agent 运行模式](llm-agent/agent-patterns.md) — ReAct / Plan-and-Execute / Self-Refine / Multi-Agent 的原理与选型 · w1 · updated 2026-07-30
- [LangChain 与 LangGraph 的差异](llm-agent/langchain-vs-langgraph.md) — 顺序链 vs 有状态图；循环、条件分支、checkpoint、human-in-the-loop · w1 · updated 2026-07-30
- [合格 Agent 需要具备的特性](llm-agent/agent-quality-attributes.md) — 稳定执行、工具准、上下文管住、自纠错、可观测、可中断恢复 · w1 · updated 2026-07-30
- [Bad Case 分析方法论](llm-agent/bad-case-analysis.md) — 坏例库打标→定期复盘→规则/prompt/训练数据分层修复→回归验证 · w1 · updated 2026-07-30

### rag/
- [RAG 完整链路](rag/rag-pipeline.md) — 切片→向量化→混合召回(向量+BM25，RRF 或 0.3/0.7 加权)→重排序→top3-5→生成标注来源 · w3 · wrong1 · updated 2026-07-30
- [文本分块策略](rag/chunking-strategies.md) — 固定窗口/语义/递归分割 + overlap 10-20%；表格转 MD、图片 OCR、父子块 · w1 · updated 2026-07-30

### database/
- [业务数据的多数据库选型](database/database-selection-for-business.md) — MySQL 核心业务 / Redis 缓存 / 向量库 embedding / ES 日志搜索 / Neo4j 图谱 · w2 · updated 2026-07-30

### distributed/
- [消息队列选型 Kafka/RocketMQ/RabbitMQ](distributed/mq-comparison.md) — 高吞吐 vs 事务消息 vs 灵活路由；RocketMQ 半消息保证下单扣库存一致 · w1 · updated 2026-07-30

### java/
- [Java 线程池核心参数](java/thread-pool-parameters.md) — 七参数与提交流程（核心→队列→非核心→拒绝）、队列与拒绝策略选型 · w1 · updated 2026-07-30
- [HashMap 底层原理](java/hashmap-internals.md) — 数组+链表/红黑树，扰动函数、2 的幂容量、树化双条件、扩容与线程安全 · w1 · updated 2026-07-30
- [接口与抽象类的区别及面向对象设计](java/interface-vs-abstract-class.md) — can-do vs is-a；动物/鸭子类体系设计、组合优于继承 · w1 · updated 2026-07-30

### os/
- [进程、线程、协程的区别](os/process-thread-coroutine.md) — 资源分配 vs 调度单位 vs 用户态轻量切换；三种并发模型选型 · w1 · updated 2026-07-30

### network/
- [TCP 与 UDP 的区别及选型](network/tcp-vs-udp.md) — 可靠有序 vs 低延迟；游戏选 UDP，丢包排查与应用层可靠性补偿、QUIC · w1 · updated 2026-07-30

### ai-coding/
- [Vibe Coding 交付质量保障](ai-coding/vibe-coding-quality.md) — 任务拆小、核心逻辑人审、关键单测、静态检查门禁 · w2 · updated 2026-07-30

### algorithm/
- [最长回文子串](algorithm/longest-palindromic-substring.md) — 中心扩展 O(n²) 必写；Manacher 用对称性复用信息达 O(n) · w1 · updated 2026-07-30
- [除自身以外数组的乘积](algorithm/product-of-array-except-self.md) — 前缀积×后缀积两趟扫描，O(n) 时间 O(1) 空间，不用除法规避 0 · w1 · updated 2026-07-30
