# CoT（思维链）：为什么能提高复杂任务能力

> tags: llm, cot, chain-of-thought, 推理, prompt
> weight: 1
> updated: 2026-08-04

## 核心结论
CoT（Chain-of-Thought）让模型输出中间推理步骤再给最终答案，而不是直接给答案。有效的原因：把复杂问题分解为子步骤逐个解决、显式中间状态降低"一步到位"的跳步错误、本质是把更多计算量（test-time compute）花在难问题上。

## 展开
- **形式**：
  - Few-shot CoT：示例里给出推理过程，模型模仿；
  - Zero-shot CoT：加"Let's think step by step"即触发；
  - 推理模型（o1/R1 类）：把长 CoT 能力通过 RL 内化，自动生成思维链。
- **为什么有效**：
  1. 分解：多步数学题/逻辑题一步算错概率高，拆步后每步简单；
  2. 自回归特性：生成的中间步骤成为后续 token 的条件上下文，相当于把中间结果"写下来"当草稿纸；
  3. 计算量分配：答案 token 数变多 = 串行计算深度增加，Transformer 单步前向的表达力有限，CoT 绕过该限制。
- **变体**：Self-Consistency（采样多条 CoT 投票取多数答案）、ToT（树状搜索多条推理路径）、GoT（图状）；ReAct = CoT + 工具交互（推理与行动交织）。
- 代价：输出变长 → 延迟和 token 成本上升；简单问题用 CoT 反而可能"想多出错"。

## 关键细节 / 易错点
- 追问"CoT 什么时候没用"：简单事实问答、模型本就会的题；对超长链推理错误会累积（一步错步步错）。
- 追问"怎么验证中间步骤对不对"：过程奖励模型（PRM）/ 步骤级校验，或 self-consistency 多路径交叉验证。

## 关联
- 相关知识点：[[wiki/llm-agent/agent-patterns.md]]、[[wiki/llm/prompt-engineering-principles.md]]
- 常见追问链：CoT 是什么 → 为什么有效 → 变体 → 什么时候失效

## 面经来源
- 快手 AI 应用开发一面（2026-08）：什么是 CoT、为什么能提高模型处理复杂任务的能力
