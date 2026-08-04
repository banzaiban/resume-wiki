# MCP 协议：解决什么、与 A2A / Skill 的区别

> tags: llm-agent, mcp, a2a, skill, 协议, 工具调用
> weight: 3
> updated: 2026-08-04

## 核心结论
MCP（Model Context Protocol）标准化"模型应用 ↔ 外部能力"的接口：server 统一暴露工具/资源/prompt，client（host 应用）按协议发现与调用，解决 N 个应用 × M 个服务的两两集成（N×M → N+M）。不用 MCP 就自己写 function calling + 手写 API 封装层，能做但每个工具都要手工对接。A2A 解决 agent 之间的协作委派（agent↔agent），MCP 解决 agent 调工具（agent↔tool），互补不冲突。Skill 是"知识/流程包"（教模型怎么做），MCP 是"能力接口"（让模型能调用）。

## 展开
- **MCP 架构**：host（Agent 应用）内嵌 client ↔ 各 MCP server（本地 stdio 或远程 HTTP）；server 声明 tools（名称/描述/JSON Schema 参数）、resources（数据）、prompts（模板）；client 把工具列表注入模型上下文，模型输出调用请求，client 转发执行。
- **不用 MCP 怎么实现工具调用**：function calling（模型 API 原生能力）+ 自建工具注册表（描述、schema、handler）+ 统一网关（鉴权/超时/重试/日志）——功能等价，但生态和复用性差。
- **A2A vs MCP 场景区分**：调一个确定性能力（查库、发消息、读文件）→ MCP；把子任务委派给另一个有自主性的 agent（跨组织/跨系统协作、长任务状态同步）→ A2A。
- **MCP 工具返回大 JSON 的处理**：截断会丢诊断价值——① server 端分页/字段过滤参数化（让模型按需取）；② 只返回摘要 + 明细可再查（指针式返回）；③ 关键字段优先排序，截断保头不保尾；④ 入库存原文，上下文里放摘要。
- **Skill vs MCP**：Skill = markdown 指令 + 脚本资源的包，加载进上下文指导模型"如何完成一类任务"（知识与流程）；MCP 是运行时接口协议，提供"能调什么"。Skill 可以引用 MCP 工具，两者正交。

## 关键细节 / 易错点
- 追问"MCP 在安全上要注意什么"：server 即代码执行面——鉴权、参数校验、权限最小化、危险操作走人工审批。
- 追问"A2A 和 MCP 能一起用吗"：典型组合——orchestrator 用 A2A 委派子 agent，子 agent 用 MCP 调自己的工具。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-harness-skills.md]]、[[wiki/llm-agent/multi-agent-architecture.md]]、[[wiki/llm-agent/tool-call-accuracy.md]]
- 常见追问链：MCP 解决什么 → 不用怎么实现 → A2A 什么关系 → Skill 什么区别

## 面经来源
- 美团 AI Agent 开发一面（2026-08）：MCP 在项目里具体解决了什么问题、不用它怎么实现工具调用
- 阿里云 AI 应用研发二面（2026-08）：MCP 量表工具返回 JSON 很大怎么截断不丢诊断信息；A2A 和 MCP 各自解决什么问题、什么场景用哪个
- 未知公司面经片段B（2026-08）：对比一下 Skill 和 MCP 的区别
