# HTTP GET 与 POST 的区别

> tags: network, http, get, post, 幂等
> weight: 1
> updated: 2026-08-04

## 核心结论
语义不同：GET 是"获取"（安全、幂等、可缓存），POST 是"提交/处理"（非幂等、默认不缓存）。派生差异：GET 参数在 URL（可见、有长度实践限制、可被浏览器缓存/收藏），POST 参数在 body；浏览器回退/刷新时 GET 无感，POST 会提示重复提交。

## 展开
- **语义与幂等**：GET 安全（safe，不改资源状态）且幂等；POST 非幂等（重复提交可能创建多条）。PUT/DELETE 幂等、PATCH 不一定——常被连带追问。
- **参数位置**：GET 在 query string；POST 在 body（form / json / multipart）。注意 GET 也能带 body 但语义不被保证，别这么用。
- **缓存**：GET 默认可被浏览器/CDN 缓存；POST 默认不缓存（除非显式 Cache-Control）。
- **长度**：HTTP 协议本身不限 URL 长度——限制来自浏览器（约几十 KB~2MB）与服务器配置；body 理论无限，服务器有限制。
- **安全性误区**：GET 参数在 URL 会出现在日志/历史记录里所以"不安全"，但 POST body 同样是明文——安全靠 HTTPS，不靠在不在 URL。
- **副作用与重发**：浏览器后退刷新 GET 直接重发无提示，POST 弹确认；爬虫/预取只敢碰 GET——所以"会改状态的操作绝不能用 GET"（会被预取器误触）。

## 关键细节 / 易错点
- 面试加分：说清楚"安全/幂等"是 RFC 语义约定，不是技术强制——技术上 GET 也能改数据，但违反语义会踩缓存/预取的坑。
- 追问"GET 和 POST 性能"：无本质差别；旧说"GET 一个包 POST 两个包"（100-continue）在现代实现里基本不成立，不是重点。

## 关联
- 相关知识点：[[wiki/network/tcp-vs-udp.md]]
- 常见追问链：语义区别 → 幂等 → 缓存 → 安全误区 → RESTful 里怎么用

## 面经来源
- 快手 AI 应用开发一面（2026-08）：HTTP 协议中 GET 和 POST 请求的区别
