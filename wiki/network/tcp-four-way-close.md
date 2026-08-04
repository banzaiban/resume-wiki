# TCP 断开为什么需要四次挥手

> tags: network, tcp, 四次挥手, FIN, TIME_WAIT, CLOSE_WAIT
> weight: 1
> updated: 2026-08-04

## 核心结论
TCP 全双工，两个方向的通道要各自独立关闭（各需一对 FIN+ACK，下限 4 个包）。被动方收到 FIN 时内核必须立即回 ACK，但应用可能还有数据没发完，要等应用 close() 才发自己的 FIN——ACK 和 FIN 不能合并，所以比握手多一次。

## 展开
- **FIN 的语义**："我数据发完了"（不再发，但还能收），不等于断开整条连接。
- **四次流程**：① 主动方 FIN → ② 被动方内核立即 ACK（主动方收到后停发 FIN 重传）→ ③ 被动方应用发完剩余数据、close() 后发自己的 FIN → ④ 主动方 ACK，进入 TIME_WAIT。
- **为什么握手能三次**：服务端收到 SYN 时没有待发数据，SYN+ACK 可合并成一个包；挥手时 ACK 与 FIN 之间隔着"应用收尾"的时间窗，只能分包。
- **什么时候能看到三次挥手**：被动方收到 FIN 时恰好也无数据可发，ACK+FIN 可合并（延迟确认机制下常见）。
- **TIME_WAIT = 2MSL**（只有主动关闭方进入）：
  1. 最后一个 ACK 可能丢——丢了对方会重传 FIN，TIME_WAIT 期间还能补 ACK，防止对方卡死 LAST_ACK；
  2. 让本次连接的迷途报文在 2MSL 内消散，防止相同四元组的新连接收到旧连接的包。
- **CLOSE_WAIT 过多** = 被动方收到 FIN 后应用没及时 close()，典型连接泄漏信号。

## 关键细节 / 易错点
- 常见错答："因为要确认双方都收到"——没点到全双工独立关闭 + ACK/FIN 无法合并这两个本质。
- 追问"TIME_WAIT 太多怎么办"：短连接高并发下端口被占——改长连接/连接池，或调 tcp_tw_reuse（仅客户端方向安全），别乱开 tcp_tw_recycle（NAT 下出问题）。
- 同时关闭（双方同时发 FIN）抓包也是 4 个包，属于特例。

## 关联
- 相关知识点：[[wiki/network/tcp-vs-udp.md]]、[[wiki/network/http-get-vs-post.md]]
- 常见追问链：为什么四次 → 能不能三次 → TIME_WAIT 为什么 2MSL → CLOSE_WAIT/TIME_WAIT 过多怎么排查

## 面经来源
- 用户提问（2026-08）：为什么 TCP 断开连接需要四次挥手
