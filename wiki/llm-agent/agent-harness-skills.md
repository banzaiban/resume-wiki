# Agent Harness、Skills 与 Agent 自进化

> tags: llm-agent, harness, skills, 自进化, openclaw, coding-agent
> weight: 3
> updated: 2026-08-04

## 核心结论
Agent Harness 是模型外的运行时外壳：上下文管理、工具调度、循环控制、状态持久化、权限与安全、可观测。没有它，大模型只是无状态的文本生成器——无法感知和操作环境、多步状态无处安放。Skills 是"知识+流程"包（教模型怎么组合工具完成一类任务），Tool 是单次动作接口。不微调的自进化靠经验沉淀：成功轨迹写入记忆/skill 库、反思生成新 skill、示例库自动扩充。

## 展开
- **Harness 的职责**：维护 ReAct 循环与步数上限、拼装/压缩上下文、解析工具调用并调度执行、checkpoint 与中断恢复、权限审批（危险操作拦截）、trace 打点。模型只负责"想"，Harness 负责"活"。
- **没有 Harness 的瓶颈**：无环境感知（文件/执行结果看不到）、无记忆（每轮从零开始）、无纠错回路（错了没人喂回去）、无安全边界。
- **Skills vs 普通 Tool 调用**：
  - Tool：原子动作接口（一次调用一个结果），schema 驱动；
  - Skill：markdown 指令 + 脚本/资源文件的任务包，按需加载进上下文，本质是"可复用的专家流程知识"——告诉模型完成某类任务的步骤、禁区、输出规范。
  - 关系：Skill 引用/编排 Tool；Skill 解决"怎么做"，Tool 解决"能做什么"。
- **如何设计一个 Skill**：① 触发描述写清"什么时候用"（供路由匹配）；② 流程步骤分解 + 每步可用工具；③ 边界与禁区（什么情况停用/转人工）；④ 输出格式约定；⑤ few-shot 示例；⑥ 版本化、可被评估迭代。
- **Coding Agent（OpenClaw 类）的本地环境感知**：文件系统工具集（read/write/grep/glob）、shell 执行（沙箱 + 权限分级）、工作区状态追踪、记忆文件（跨会话的项目知识落盘成 md，下次启动读回）——记忆实现=持久化文件 + 启动时加载 + 会话中增量追加。
- **不微调的 Agent 自进化**：① 成功任务轨迹沉淀为可检索经验（类似 few-shot 库扩容）；② 失败反思（Reflexion）写入长期记忆；③ 人工修正自动转化为新 skill/规则；④ 工具使用统计反哺工具描述与检索排序。本质：把"学"从参数空间挪到上下文/记忆空间。

## 关键细节 / 易错点
- 追问"Skill 和微调的区别"：Skill 改的是上下文（即时、可回滚、可解释），微调改的是参数（泛化但贵、慢、不可解释）；知识流程类优先 Skill。
- 追问"自进化的风险"：错误经验沉淀会污染——写入前要验证（测试通过/人工确认），支持版本回滚。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-patterns.md]]、[[wiki/llm-agent/mcp.md]]、[[wiki/llm-agent/agent-memory-design.md]]、[[wiki/ai-coding/vibe-coding-quality.md]]
- 常见追问链：Harness 是什么 → Skill vs Tool → 怎么设计 Skill → 自进化怎么做

## 面经来源
- 美团 AI Agent 开发一面（2026-08）：怎么理解 Agent Harness、没有它大模型遇到什么瓶颈；Skills 和普通 Tool 调用有什么区别；不微调怎么实现 Agent 自进化
- 未知公司面经片段A（2026-08）：讲一下 OpenClaw 的记忆实现
- 未知公司面经片段B（2026-08）：如何设计一个 Skill；OpenClaw 如何增强 Agent 对本地文件系统和代码执行环境的感知与操作权限
