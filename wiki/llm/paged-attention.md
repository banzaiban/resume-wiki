# vLLM PagedAttention 原理

> tags: llm, vllm, pagedattention, kv-cache, 推理优化
> weight: 1
> updated: 2026-08-04

## 核心结论
PagedAttention 借鉴 OS 虚拟内存分页思想管理 KV Cache：把每条序列的 KV Cache 切成固定大小的块（block/page），逻辑块通过块表映射到**非连续**的物理块，按需分配。解决了传统做法"按最大长度预分配连续显存"的巨大浪费（内部碎片 + 为不确定长度预留），显存利用率从 ~30% 提到 90%+，吞吐提升数倍。

## 展开
- **问题背景**：自回归生成每步要读全部历史 KV Cache；传统实现为每个请求预分配 max_len 连续显存——实际序列长度不定，短序列浪费、预留空间闲置，且无法共享。
- **分页机制**：
  - KV Cache 按 block（如 16 个 token）切分；
  - 块表（block table）记录逻辑块 → 物理块映射，物理块可不连续；
  - 生成新 token 时当前块满了才申请新块——按需增长，零预留浪费；
  - 注意力 kernel 按块表_gather_ 非连续块计算。
- **块共享**：并行采样（n>1）/ beam search / 共享 system prompt 前缀时，相同前缀的物理块被多序列引用计数共享，写时复制（copy-on-write）——prefix caching 的基础。
- **类比记忆**：逻辑地址=序列的 token 位置，物理地址=显存块，块表=页表，换出=序列抢占时把 KV 块换到 CPU/磁盘（vLLM 的 swapping/recompute 调度）。

## 关键细节 / 易错点
- 追问"为什么能提吞吐"：显存浪费少了 → 同卡可塞更多并发序列（更大 batch）→ GPU 利用率拉满；瓶颈从显存容量回到算力。
- 追问"和 FlashAttention 关系"：FlashAttention 优化单次注意力的 IO（tiling 不落地中间矩阵），PagedAttention 优化 KV Cache 的显存管理，正交互补，vLLM 两者都用。

## 关联
- 相关知识点：[[wiki/llm/transformer-self-attention.md]]、[[wiki/llm/positional-encoding.md]]
- 常见追问链：KV Cache 显存问题 → 分页怎么做 → 块共享 → 和 FlashAttention 的区别

## 面经来源
- 快手 AI 应用开发一面（2026-08）：介绍一下 vLLM 中 PagedAttention 的原理
