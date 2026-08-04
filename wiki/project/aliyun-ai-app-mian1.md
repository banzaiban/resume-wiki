# 面经：阿里云 AI 应用研发 一面

> tags: project, 面经, 阿里云, ai-agent
> weight: 1
> updated: 2026-08-04

## 概况
暑期实习（五月份面的），55min 左右，回忆版（来源：抖音 @Devs 截图，2026-08）。无手撕。

## 题目清单
1. 自我介绍
2. 拷打实习
3. 拷打项目一（工业文档 RAG）：
   - MinerU 解析出表格、公式、图片后怎么分别存进 Milvus？索引策略一样吗？→ [[wiki/rag/document-parsing.md]]、[[wiki/rag/multimodal-rag.md]]
   - 跨页的图纸切分时怎么保证图不丢？→ [[wiki/rag/document-parsing.md]]
   - 交叉编码器重排一轮延迟大概多少？文档量大怎么办？→ [[wiki/rag/reranker.md]]
4. 拷打项目二（心理咨询 Agent）：
   - LangGraph 状态图节点之间输入输出怎么约定的？→ [[wiki/llm-agent/langchain-vs-langgraph.md]]
   - 中间节点报错了，图的状态怎么回滚？→ [[wiki/llm-agent/langchain-vs-langgraph.md]]
   - 安全护栏是规则词库还是分类模型做的？敏感词命中后处理链路怎么走？危机场景分了几级、什么情况上人工？→ [[wiki/llm-agent/safety-guardrails.md]]
5. 大模型应用里什么时候该用 API 调用、什么时候该本地部署？选型因素有哪些？→ [[wiki/llm-agent/llm-api-engineering.md]]
6. 垂域 Embedding 语义偏移，除了混合检索和 Reranker 还有什么办法？→ [[wiki/llm/embedding-models.md]]
7. 检索为空或超时的时候，Prompt 上怎么兜底？→ [[wiki/rag/rag-pipeline.md]]
8. Token 消耗越来越大，怎么控制单次会话的预算？→ [[wiki/llm-agent/agent-memory-design.md]]
9. 怎么判断一个业务场景该走 Workflow 还是交给 Agent 自主决策？→ [[wiki/llm-agent/agent-vs-workflow.md]]
10. ReAct 执行多步任务时动作空间太大容易乱跳，怎么约束工具的调用顺序？→ [[wiki/llm-agent/agent-patterns.md]]
11. 怎么保证模型每次都稳定输出 JSON 来调工具？格式不对代码里怎么兜底？→ [[wiki/llm-agent/tool-call-accuracy.md]]
12. Agent 多步推理中间步骤怎么流式返回前端，而不是等全部跑完才出结果？→ [[wiki/llm-agent/agent-streaming.md]]
13. 多 Agent 拆分的边界是什么？什么情况下不该拆？→ [[wiki/llm-agent/multi-agent-architecture.md]]
14. 多个工具并发调用，部分成功部分失败，怎么汇总结果给模型？→ [[wiki/llm-agent/tool-parallel-execution.md]]
15. 上下文越拉越长，有哪些压缩方法能在尽量不丢信息的前提下控制长度？→ [[wiki/llm-agent/agent-memory-design.md]]
16. 反问

## 考察重点
RAG 全链路工程细节、LangGraph 状态管理、护栏/降级/兜底等线上稳定性设计。
