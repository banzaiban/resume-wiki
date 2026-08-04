# 多工具并行调用与结果汇总

> tags: llm-agent, parallel-tool-calls, asyncio, 并发, 结果汇总
> weight: 2
> updated: 2026-08-04

## 核心结论
模型一轮输出多个 tool_calls（无依赖关系）→ 框架并发执行（Python 用 asyncio.gather / Java 用 CompletableFuture）→ 每个结果按 tool_call_id 独立封装 → 全部就绪后一次性回传给模型。部分成功部分失败：失败项带简要 error 一并回传，由模型决定重试、换工具还是基于部分结果作答。

## 展开
- **触发**：模型在同一回合返回多个 tool_call（OpenAI parallel tool calls），或规划阶段产出无依赖的子任务列表。
- **并发执行**：
  - Python：`asyncio.gather(*tasks, return_exceptions=True)`——异常不互相拖垮；
  - 每个调用独立超时、独立重试预算；下游 API 用信号量限流防打挂；
  - 有依赖的调用不能并行：编排层按依赖图（DAG）分批，同层并发、跨层等待。
- **结果汇总**：每项封装为 {tool_call_id, status, result | error}，按序拼回消息列表一次性给模型——不要逐条来回（多轮往返浪费延迟与 token）。
- **部分失败处理**：模型看到混合结果后可：重试失败项（带上次错误）、换替代工具、或基于已有结果作答并在回复中显式说明缺失部分；关键信息缺失且无法补救时向用户澄清而非编造。

## 关键细节 / 易错点
- 追问"怎么判断能不能并行"：参数不依赖其他工具结果即可并行；模型输出阶段或编排层做依赖分析。
- 追问"并发把下游打挂怎么办"：信号量控并发数 + 下游限流码（429）触发退避。
- 与 Agent 多步慢的关系：并行化 + 流式返回中间状态是两大提速手段。

## 关联
- 相关知识点：[[wiki/llm-agent/tool-call-failure-handling.md]]、[[wiki/llm-agent/tool-call-accuracy.md]]、[[wiki/python/asyncio-vs-threading-mutex.md]]、[[wiki/llm-agent/agent-streaming.md]]
- 常见追问链：怎么实现并行 → 部分失败怎么汇总 → 依赖怎么处理 → 限流怎么做

## 面经来源
- 美团 AI Agent 开发二面（2026-08）：怎么实现多工具并行调用、代码里怎么控制多个 API 并发并一次性返回给模型
- 阿里云 AI 应用研发一面（2026-08）：多个工具并发调用部分成功部分失败，怎么汇总结果给模型
