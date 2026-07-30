# Transformer 与自注意力

> tags: llm, transformer, self-attention, QKV, scaled-dot-product
> weight: 1
> updated: 2026-07-30

## 核心结论
Transformer 由编码器/解码器堆叠（现代 LLM 多为 decoder-only），核心是自注意力：每个 token 生成 Q、K、V，用 QK^T 算相关性权重，softmax 后加权求和 V。除以 √dk 是防止点积方差随维度增大而过大，导致 softmax 进入饱和区、梯度消失。

## 展开
- 计算：`Attention(Q,K,V) = softmax(QK^T / √dk) V`。Q/K/V 由输入乘三个可学习矩阵 W_Q/W_K/W_V 得到。
- **为什么除以 √dk**：假设 q、k 分量独立同分布（均值 0 方差 1），点积 q·k 的方差为 dk；dk 大时点积值分散到很大区间，softmax 输出趋近 one-hot（饱和），梯度接近 0。除以 √dk 把方差拉回 1，训练稳定。
- **多头注意力**：把 d_model 拆成 h 个头并行做 attention，各头学不同的关系子空间（语法/指代/位置等），拼接后线性变换。
- 自注意力 vs RNN：并行计算（无时序依赖）、任意距离直接建立联系（路径长度 O(1)）；代价是 O(n²) 复杂度。
- 三种架构：encoder-only（BERT，双向，适合理解类）、decoder-only（GPT，因果 mask，适合生成）、encoder-decoder（T5，适合翻译/摘要）。
- 一个 block 的组成：自注意力 → 残差 + LayerNorm → FFN → 残差 + LayerNorm。

## 关键细节 / 易错点
- 追问"为什么用三个矩阵而不直接用输入算相似度"：QKV 解耦"查什么/被查什么/提供什么信息"三种角色，表达能力更强。
- 追问"KV Cache"：推理时缓存历史 token 的 K/V，每步只算新 token 的 Q，把生成复杂度从 O(n²) 降为每步 O(n)。
- 追问"attention 复杂度优化"：稀疏注意力、滑动窗口、FlashAttention（IO 优化，精确不近似）、MQA/GQA（多头共享 KV）。
- decoder 的因果 mask：上三角置 -inf，保证只看历史。

## 关联
- 相关知识点：[[wiki/llm/layernorm.md]]、[[wiki/llm/positional-encoding.md]]、[[wiki/llm/tokenization.md]]
- 常见追问链：QKV 计算 → 为什么除 √dk → 多头作用 → KV Cache → 长文本优化

## 面经来源
小米 AI 大模型应用开发一面（2026-07）
