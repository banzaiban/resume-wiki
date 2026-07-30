# 消息队列选型：Kafka / RocketMQ / RabbitMQ

> tags: distributed, mq, kafka, rocketmq, rabbitmq, 事务消息
> weight: 1
> updated: 2026-07-30

## 核心结论
Kafka 高吞吐、顺序写盘 + 零拷贝，适合日志、流处理、大数据管道；RocketMQ 支持**事务消息**、定时/延时消息、消息回溯，适合电商交易链路；RabbitMQ 基于 AMQP，路由灵活、延迟低，适合业务解耦和复杂路由。电商下单扣库存选 RocketMQ——半消息（half message）机制保证本地事务与消息投递的最终一致，防超卖。

## 展开
**对比**
| 维度 | Kafka | RocketMQ | RabbitMQ |
|---|---|---|---|
| 吞吐 | 最高（十万~百万级/s） | 高（十万级） | 中（万级） |
| 延迟 | ms 级 | ms 级 | 微秒~ms（最低） |
| 事务消息 | 有（流处理 exactly-once 语义） | 有（半消息+回查，业务侧最好用） | 无（有 publisher confirm） |
| 延时消息 | 无原生 | 有（固定等级/任意时刻） | 靠死信队列+TTL 变通 |
| 路由 | topic + partition | topic + tag 过滤 | exchange 四种类型，最灵活 |
| 典型场景 | 日志、埋点、流计算 | 电商交易、订单 | 业务解耦、任务分发 |

**RocketMQ 事务消息流程**（下单扣库存的答案要点）：
1. 生产者发送**半消息**到 broker（对消费者不可见）；
2. broker 确认收到 → 生产者执行本地事务（写订单）；
3. 本地事务成功 → 提交（commit）半消息，消息变可见；失败 → 回滚（rollback）丢弃；
4. 若生产者宕机导致状态未知，broker 定时**回查**本地事务状态，据结果 commit/rollback。
- 消费侧要**幂等**（用订单号去重），因为 MQ 保证 at-least-once 而非 exactly-once。

**Kafka 高吞吐的原因**：顺序追加写（磁盘顺序 IO 接近内存随机 IO）、page cache + 零拷贝（sendfile）、批量发送与压缩、partition 水平并行。

## 关键细节 / 易错点
- 顺序消息：Kafka 只保证 partition 内有序（同 key 打到同 partition）；RocketMQ 有顺序消息（MessageQueueSelector）。
- 消息丢失三个环节都要防：生产端（同步发送 + ack=all + 重试）、broker（多副本刷盘）、消费端（处理完再提交 offset）。
- 重复消费不可避免 → 消费幂等是必答项（唯一键、去重表、状态机）。
- 追问"为什么不用本地消息表/最大努力通知"：可以，事务消息本质就是把本地消息表的逻辑下沉到 MQ。

## 关联
- 相关知识点：[[wiki/database/database-selection-for-business.md]]
- 常见追问链：三者对比 → 电商选哪个为什么 → 事务消息流程 → 消息丢失/重复怎么办 → Kafka 为什么快

## 面经来源
美团 AI 应用开发一面（2026-07）
