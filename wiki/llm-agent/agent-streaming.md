# Agent 中间步骤流式返回（SSE 事件流）

> tags: llm-agent, streaming, sse, websocket, 流式, 前端
> weight: 2
> updated: 2026-08-04

## 核心结论
Agent 多步任务动辄几十秒，不能等全部跑完才出结果：后端 Agent 循环每步产生事件（思考 token、工具调用开始/结束、步骤状态），通过 SSE（或 WebSocket）实时推给前端，前端渲染步骤时间线 + 打字机效果。关键在事件协议设计与断线恢复。

## 展开
- **事件协议**：定义统一事件类型——`thought_delta`（思考 token 流）、`tool_call_start`（工具名+参数）、`tool_call_end`（结果摘要+耗时）、`step_update`（当前第几步/总进度）、`answer_delta`（最终回答流）、`error`、`done`。每事件带 event_id + timestamp。
- **传输选型**：SSE（HTTP 长连接、服务端单向推、实现简单、自动重连）够大多数场景；需要双向实时交互（用户中途打断/补充）→ WebSocket。
- **后端实现**：Agent 循环里每个节点 yield 事件（Python async generator 天然契合），经网关转发；LLM token 流与工具事件交织推送。
- **工程细节**：
  - 长工具执行期间发心跳/进度事件防连接超时；
  - 断线重连用 last_event_id 续传，或重连后拉取当前全量状态快照；
  - 事件量太大可做采样/聚合（token 级事件合并成块）；
  - 敏感中间结果（内部推理）按配置脱敏后再推。
- 价值：感知延迟大幅降低（用户看到"正在查订单→正在算价格"）、可中途打断纠偏、调试时可回放。

## 关键细节 / 易错点
- 追问"SSE 和 WebSocket 怎么选"：单向推送 SSE（简单、过代理友好）；双向/高频交互 WebSocket。
- 追问"多实例部署下推送怎么路由"：连接所在实例即执行实例，或事件走消息队列广播到持连接的实例。

## 关联
- 相关知识点：[[wiki/python/asyncio-vs-threading-mutex.md]]、[[wiki/llm-agent/tool-parallel-execution.md]]、[[wiki/llm-agent/agent-observability.md]]
- 常见追问链：为什么要流式 → 事件怎么设计 → SSE 还是 WS → 断线怎么办

## 面经来源
- 美团 AI Agent 开发一面（2026-08）：Agent 多步任务很慢，怎么把思考步骤和工具执行状态流式传输实时返回前端
- 阿里云 AI 应用研发一面（2026-08）：多步推理中间步骤怎么流式返回前端，而不是等全部跑完才出结果
