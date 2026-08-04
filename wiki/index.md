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
- [Python 异步与多线程的互斥](python/asyncio-vs-threading-mutex.md) — asyncio.Lock vs threading.Lock、GIL、Agent 开发中异步的用处与必须异步的场景 · w2 · updated 2026-08-04
- [Python 列表与元组的区别](python/list-vs-tuple.md) — 可变性派生一切：可哈希当 dict key、常量优化、同质集合 vs 异质记录语义 · w1 · updated 2026-08-04
- [Python 装饰器及应用场景](python/decorator.md) — 语法糖 f=deco(f)、functools.wraps 保元信息、带参三层嵌套、日志/鉴权/缓存/重试/路由注册 · w1 · updated 2026-08-04

### llm/
- [什么样的 Prompt 效果好](llm/prompt-engineering-principles.md) — 指令直白 + few-shot + 输出格式说死；标准 Prompt 结构与 temperature 调参 · w2 · wrong0.5 · updated 2026-07-30
- [Transformer 与自注意力](llm/transformer-self-attention.md) — QKV 算权重、除 √dk 防 softmax 饱和、多头/KV Cache/因果 mask · w1 · updated 2026-07-30
- [LayerNorm](llm/layernorm.md) — 特征维归一化，NLP 不用 BN 的原因；Pre-LN vs Post-LN、RMSNorm · w1 · updated 2026-07-30
- [位置编码与长文本外推](llm/positional-encoding.md) — 绝对/相对/RoPE 旋转矩阵；PI/NTK/YaRN 插值、滑动窗口扩上下文 · w1 · updated 2026-07-30
- [Embedding 向量与模型选型](llm/embedding-models.md) — 对比学习训练、BGE/M3E、难负例微调；垂域语义偏移与数字不敏感的解法 · w3 · updated 2026-08-04
- [幻觉的原因与缓解](llm/hallucination.md) — 幻觉三类型 + 四层成因；RAG 接地 + prompt 约束 + 置信过滤 + 输出校验 · w3 · updated 2026-08-04
- [Lost in the Middle](llm/lost-in-the-middle.md) — 长上下文 U 型利用率；重排前置、只喂 top3-5、压缩、指令结尾重申 · w2 · updated 2026-08-04
- [Token 与字符的区别、分词](llm/tokenization.md) — BPE/BBPE 子词切分、中英 token 换算、对成本与字符级能力的影响 · w1 · updated 2026-07-30
- [CoT 思维链](llm/cot.md) — 中间推理步骤为什么有效：分解/草稿纸/test-time compute；Self-Consistency/ToT 变体 · w1 · updated 2026-08-04
- [长上下文能否取代向量记忆库](llm/long-context-vs-external-memory.md) — 不能：成本/延迟/利用率/可写性四维度；融合架构才是正解 · w1 · updated 2026-08-04
- [vLLM PagedAttention](llm/paged-attention.md) — KV Cache 分页管理、块表映射非连续显存、块共享 prefix cache，吞吐数倍提升 · w1 · updated 2026-08-04

### llm-agent/
- [Agent 工具调用失败处理](llm-agent/tool-call-failure-handling.md) — 重试(退避+幂等)→降级→监控告警→错误回传自愈；参数幻觉修正、并发部分失败汇总 · w3 · updated 2026-08-04
- [提高工具调用准确率](llm-agent/tool-call-accuracy.md) — 描述写细+few-shot+Schema 约束+校验重试；稳定 JSON 兜底、上百工具 Tool Retrieval · w4 · updated 2026-08-04
- [意图识别优化](llm-agent/intent-classification-optimization.md) — BERT 小模型主分类 + 规则补漏 + 大模型兜底级联；语义路由前置快速分流 · w3 · wrong1 · updated 2026-08-04
- [评测数据集构建](llm-agent/eval-dataset-construction.md) — 线上日志分层抽样 + 人工标注 + 增强改写，覆盖短头长尾，防泄漏定期换血 · w1 · updated 2026-07-30
- [Agent Memory 模块设计](llm-agent/agent-memory-design.md) — 三层记忆架构；记忆压缩、token 预算控制、画像更新与短长期区分 · w6 · updated 2026-08-04
- [Agent 与 Workflow 的取舍](llm-agent/agent-vs-workflow.md) — Workflow 定骨架保确定性，Agent 处理需推理的节点，混合使用 · w2 · updated 2026-08-04
- [常见 Agent 运行模式](llm-agent/agent-patterns.md) — ReAct TAO 状态机/vs 单轮问答、Plan-and-Solve 对比、Self-Reflection 触发重规划、约束动作空间 · w6 · updated 2026-08-04
- [LangChain 与 LangGraph 的差异](llm-agent/langchain-vs-langgraph.md) — 链 vs 有状态图；checkpoint 快照实现、节点 IO 约定、报错状态回滚 · w3 · updated 2026-08-04
- [合格 Agent 需要具备的特性](llm-agent/agent-quality-attributes.md) — 稳定执行、工具准、上下文管住、自纠错、可观测、可中断恢复 · w1 · updated 2026-07-30
- [Bad Case 分析方法论](llm-agent/bad-case-analysis.md) — 坏例库打标→定期复盘→规则/prompt/训练数据分层修复→回归验证 · w1 · updated 2026-07-30
- [Multi-Agent 架构](llm-agent/multi-agent-architecture.md) — 中心化编排 vs P2P、单体拆分边界与不该拆的情形、通信损耗优化、共享全局记忆 · w4 · updated 2026-08-04
- [Agent 轨迹评估与运行监控](llm-agent/agent-observability.md) — trace 记录 + LLM-as-judge 步骤级打分、中间节点有效性量化、死循环检测熔断 · w2 · updated 2026-08-04
- [Agent 状态持久化与话题切换](llm-agent/agent-state-persistence.md) — 每步 checkpoint 落库、状态外置可伸缩、话题切换压栈/恢复现场 · w1 · updated 2026-08-04
- [多工具并行调用与结果汇总](llm-agent/tool-parallel-execution.md) — gather 并发 + tool_call_id 独立封装一次性回传；部分失败带 error 让模型决策 · w2 · updated 2026-08-04
- [MCP 协议](llm-agent/mcp.md) — 解决 N×M 集成；vs A2A（调工具 vs agent 协作）、vs Skill（能力接口 vs 知识流程包）；大 JSON 返回处理 · w3 · updated 2026-08-04
- [Agent Harness、Skills 与自进化](llm-agent/agent-harness-skills.md) — Harness 运行时外壳职责、Skill 设计要点、OpenClaw 本地感知、不微调自进化路径 · w3 · updated 2026-08-04
- [Human-in-the-loop](llm-agent/human-in-the-loop.md) — 高敏感操作风险分级、interrupt 挂起-审批-恢复、审计日志、机制约束而非 prompt 约束 · w1 · updated 2026-08-04
- [安全护栏](llm-agent/safety-guardrails.md) — 词库粗筛+分类模型复核、危机分级上人工、撒谎感知、过度共情边界控制 · w4 · updated 2026-08-04
- [Agent 中间步骤流式返回](llm-agent/agent-streaming.md) — SSE 事件流协议设计（thought/tool_call/step）、断线续传、心跳防超时 · w2 · updated 2026-08-04
- [大模型 API 工程](llm-agent/llm-api-engineering.md) — 限流应对（令牌桶/退避/多 key/降级）、API vs 本地部署选型、上下文缓存降本 · w3 · updated 2026-08-04

### rag/
- [RAG 完整链路](rag/rag-pipeline.md) — 切片→向量化→混合召回→重排→top3-5→生成标注来源；向量 vs 关键词场景、检索为空/超时兜底 · w5 · wrong1 · updated 2026-08-04
- [文本分块策略](rag/chunking-strategies.md) — 固定/语义/递归分割 + overlap；语义分片切点判断、粒度选择、表格图片原子化 · w4 · updated 2026-08-04
- [工业文档解析与多模态入库](rag/document-parsing.md) — MinerU 选型、跨页表格合并防切碎、图纸原子切分、手写批注 VLM、跨元素关联重建 · w5 · updated 2026-08-04
- [Reranker 重排序](rag/reranker.md) — 双塔 vs cross-encoder；直接喂召回结果的问题；延迟与文档量大的优化 · w2 · updated 2026-08-04
- [多模态 RAG](rag/multimodal-rag.md) — 图文同向量空间两路线（CLIP vs VLM 描述化）、两路召回权重分配/RRF · w2 · updated 2026-08-04
- [Graph RAG](rag/graph-rag.md) — 实体关系图谱多跳遍历 vs 向量检索；复杂关联/全局总结优势、建图成本 · w1 · updated 2026-08-04
- [查询改写与 HyDE](rag/query-rewriting.md) — 指代消解/子问题分解/术语扩展/HyDE；伪文档幻觉防护（只检索不生成+双路召回） · w2 · updated 2026-08-04
- [Ragas 评估框架](rag/ragas-evaluation.md) — Faithfulness（原子声明支撑率=测幻觉）、Answer Relevance（反向出题相似度）计算逻辑 · w2 · updated 2026-08-04
- [向量索引 IVF_FLAT vs HNSW](rag/vector-index.md) — 聚类分桶 vs 分层小世界图；内存/召回/构建/更新四维对比与规模选型 · w1 · updated 2026-08-04

### database/
- [业务数据的多数据库选型](database/database-selection-for-business.md) — MySQL 核心业务 / Redis 缓存 / 向量库 embedding / ES 日志搜索 / Neo4j 图谱 · w2 · wrong1 · updated 2026-07-30

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
- [HTTP GET 与 POST 的区别](network/http-get-vs-post.md) — 安全/幂等语义、缓存、参数位置；明文都靠 HTTPS、改状态操作禁用 GET · w1 · updated 2026-08-04

### ai-coding/
- [Vibe Coding 交付质量保障](ai-coding/vibe-coding-quality.md) — 任务拆小、核心逻辑人审、关键单测、静态检查门禁；AI coding 技巧与 NL→可靠执行路径 · w4 · updated 2026-08-04

### algorithm/
- [最长回文子串](algorithm/longest-palindromic-substring.md) — 中心扩展 O(n²) 必写；Manacher 用对称性复用信息达 O(n) · w1 · updated 2026-07-30
- [除自身以外数组的乘积](algorithm/product-of-array-except-self.md) — 前缀积×后缀积两趟扫描，O(n) 时间 O(1) 空间，不用除法规避 0 · w1 · updated 2026-07-30
- [无重复字符的最长子串](algorithm/longest-substring-without-repeating-chars.md) — 滑动窗口+哈希表记最后出现位置，左指针取 max 防回退，O(n) · w1 · updated 2026-08-04

### project/
- [面经：美团 AI Agent 开发一面](project/meituan-ai-agent-mian1.md) — 50min+ 无手撕；RAG 工程细节、LangGraph/MCP/Harness/Skills、流式与异步 · w1 · updated 2026-08-04
- [面经：美团 AI Agent 开发二面](project/meituan-ai-agent-mian2.md) — 65min 无手撕；Graph RAG、查询改写、Multi-Agent 拆分、轨迹评估、限流 · w1 · updated 2026-08-04
- [面经：阿里云 AI 应用研发一面](project/aliyun-ai-app-mian1.md) — 55min 无手撕；MinerU/Milvus 入库、LangGraph 状态回滚、护栏分级、兜底设计 · w1 · updated 2026-08-04
- [面经：阿里云 AI 应用研发二面](project/aliyun-ai-app-mian2.md) — 60min；chunk 关联重建、数值召回、矛盾文档处理、A2A vs MCP、情感依赖边界 · w1 · updated 2026-08-04
- [面经：快手 AI 应用开发一面](project/kuaishou-ai-app-mian1.md) — 41min 有手撕（无重复字符最长子串）；Ragas 指标、IVF/HNSW、PagedAttention、Python 八股 · w1 · updated 2026-08-04
- [面经片段：未知公司 A](project/unknown-agent-mian-a.md) — HyDE 防幻觉、ReAct 状态机、Tool Retrieval、OpenClaw 记忆、1M 上下文 vs 向量库 · w1 · updated 2026-08-04
- [面经片段：未知公司 B](project/unknown-agent-mian-b.md) — Self-Reflection、长期记忆、HITL、Skill 设计、Skill vs MCP、Vibe Coding、上下文缓存 · w1 · updated 2026-08-04
