# Token 与字符的区别、分词

> tags: llm, tokenizer, BPE, token, 计费
> weight: 1
> updated: 2026-07-30

## 核心结论
Token 是模型处理的最小单元，由 tokenizer（多为 BPE/BBPE）按语料统计切分：一个常见英文词可能是 1 个 token，生僻词拆成多个子词；中文一个字常是 1-2 个 token（UTF-8 字节级 BPE 下更可能拆多个）。Token ≠ 字符 ≠ 词，上下文长度和计费都按 token 算。

## 展开
- **为什么用子词而非字符或整词**：整词表爆炸且有 OOV；字符级序列太长；子词（BPE）平衡词表大小与序列长度，天然处理未登录词。
- **BPE 流程**：从字符开始，统计相邻符号对频率，反复合并最高频对，直到达到目标词表大小。BBPE（byte-level BPE，GPT 系用）在字节上做，任何 unicode 都可编码，无 OOV。
- **经验换算**：英文约 1 token ≈ 4 字符 ≈ 0.75 词；中文约 1 汉字 ≈ 1~2 token（取决于 tokenizer 的中文覆盖）。不同模型 tokenizer 不同，换算不通用。
- **工程影响**：
  - 成本与限流按 token 计，中文文本 token 数常被低估；
  - chunk 大小要按 token 而非字符算；
  - 数字、代码、罕见符号会被切得很碎，影响算术能力和上下文占用；
  - 拼写/字符级任务（数字母、反转字符串）表现差，根因就是模型看不到字符。

## 关键细节 / 易错点
- 追问"为什么模型数不清 strawberry 里有几个 r"：token 化后模型看到的是子词块而非单个字符。
- 追问"怎么准确算 token 数"：用对应模型的 tokenizer（tiktoken / HF tokenizer）实测，别用字符数估。
- 追问"词表大小的权衡"：词表大 → 序列短、embedding 层参数多；词表小 → 反之。多语言模型需要更大词表。

## 关联
- 相关知识点：[[wiki/rag/chunking-strategies.md]]、[[wiki/llm/transformer-self-attention.md]]
- 常见追问链：token 是什么 → BPE 原理 → 中英文换算 → 对成本和能力的影响

## 面经来源
淘宝闪购 AI 应用开发一面（2026-07）
