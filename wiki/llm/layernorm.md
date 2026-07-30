# LayerNorm

> tags: llm, layernorm, batchnorm, 归一化, pre-ln
> weight: 1
> updated: 2026-07-30

## 核心结论
LayerNorm 在**特征维度**上归一化：对单个 token 的所有特征算均值和方差，缩放平移（可学习 γ、β），稳定训练。区别于 BatchNorm 在 batch 维度归一化——NLP 序列变长、batch 内样本不对齐，且推理时 batch 统计不稳定，所以 Transformer 用 LN 不用 BN。

## 展开
- 公式：`y = (x - μ) / √(σ² + ε) · γ + β`，μ/σ² 在该 token 的 hidden 维度上计算，每个 token 独立，与 batch 和序列长度无关。
- **为什么 NLP 不用 BatchNorm**：① 变长序列 padding 使 batch 统计失真；② 推理时依赖训练期的滑动平均统计，分布漂移敏感；③ 小 batch 时统计噪声大。LN 完全不依赖 batch。
- **Pre-LN vs Post-LN**：原始 Transformer 是 Post-LN（残差后归一化），深层训练不稳、需要 warmup；现代 LLM 基本用 Pre-LN（先归一化再进子层），梯度更平稳、可训更深，代价是表达略弱。
- **RMSNorm**（LLaMA 等采用）：去掉均值中心化，只用均方根缩放，省计算且效果相当。

## 关键细节 / 易错点
- 归一化的作用维度是最常见考点：LN=特征维（每个样本每个 token 自己），BN=batch 维（跨样本同一特征）。
- 追问"γ、β 为什么需要"：归一化会破坏表达能力，可学习的缩放平移让网络能恢复需要的分布。
- 追问"LLaMA 用的什么 Norm"：RMSNorm + Pre-LN 结构。

## 关联
- 相关知识点：[[wiki/llm/transformer-self-attention.md]]
- 常见追问链：LN 在哪个维度 → 为什么不用 BN → Pre/Post-LN → RMSNorm

## 面经来源
小米 AI 大模型应用开发一面（2026-07）
