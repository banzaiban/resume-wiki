# 向量索引：IVF_FLAT vs HNSW

> tags: rag, 向量索引, IVF_FLAT, HNSW, milvus, ANN
> weight: 1
> updated: 2026-08-04

## 核心结论
IVF_FLAT：先聚类分桶，查询只搜最近的 nprobe 个桶——内存省、构建快，召回率受 nprobe 影响（搜的桶越多越准越慢）。HNSW：分层小世界图，贪心跳层逼近最近邻——召回高、查询快，但内存大（要存图的边）、构建慢、删除麻烦。内存紧/数据量大可牺牲少量召回 → IVF；追求低延迟高召回 → HNSW。

## 展开
- **IVF_FLAT**（Inverted File）：训练时用 k-means 把向量分成 nlist 个簇（各簇一个质心）；查询先算 query 与所有质心距离，选最近 nprobe 个簇，在簇内向量**暴力精确**比对（FLAT = 不压缩）。召回率 ≈ 命中最优簇的概率，nprobe↑ 召回↑ 延迟↑。
- **HNSW**（Hierarchical Navigable Small World）：多层图——上层稀疏长边快跳转、下层稠密短边精逼近；查询从顶层贪心走到局部最优再逐层下探，对数级接近最近邻。参数 M（每点边数，影响内存与召回）、efSearch（搜索宽度，调召回/延迟）。
- **对比维度**：
  - 内存：IVF_FLAT 只存向量+质心；HNSW 额外存图结构（可多出 50%+）。
  - 召回/延迟：同预算下 HNSW 通常更优（高 90s 召回时延迟低一个量级）。
  - 构建/更新：IVF 快且支持增量；HNSW 建图慢、删除是标记删除，频繁更新场景不利。
  - 规模：亿级 + 内存受限 → IVF（或 IVF_PQ/IVF_SQ8 压缩版）；千万级内追求体验 → HNSW。
- Milvus 里两者都支持，且都能叠加标量过滤（先过滤元数据再向量搜）。

## 关键细节 / 易错点
- 追问"FLAT 后缀什么意思"：簇内不压缩原样比对；IVF_PQ 是簇内再做乘积量化压缩，更省内存但掉点召回。
- 追问"nprobe 怎么定"：在评测集上扫 nprobe-召回率曲线，取召回达标的最小值。
- 精确检索（FLAT 无索引暴力）在百万级内也可行，别过度设计。

## 关联
- 相关知识点：[[wiki/rag/rag-pipeline.md]]、[[wiki/database/database-selection-for-business.md]]、[[wiki/llm/embedding-models.md]]
- 常见追问链：两种原理 → 各自参数怎么调 → 什么规模选什么 → 标量过滤怎么配合

## 面经来源
- 快手 AI 应用开发一面（2026-08）：IVF_FLAT 和 HNSW 有什么区别、各自适用场景
