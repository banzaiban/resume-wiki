# Java 线程池核心参数

> tags: java, 并发, threadpool, ThreadPoolExecutor
> weight: 1
> updated: 2026-07-30

## 核心结论
七个参数：corePoolSize（核心线程数）、maximumPoolSize（最大线程数）、keepAliveTime + unit（非核心线程空闲存活时间）、workQueue（工作队列）、threadFactory、handler（拒绝策略）。作用是复用线程、控制并发度与资源上限。

## 展开
**任务提交流程**（高频考点，顺序不能错）：
1. 核心线程未满 → 新建核心线程执行；
2. 核心满 → 进 workQueue 排队；
3. 队列满 → 新建非核心线程（直到 maximumPoolSize）；
4. 线程数达 max 且队列满 → 触发拒绝策略。

**队列类型**
- `ArrayBlockingQueue` 有界数组；
- `LinkedBlockingQueue` 默认无界（Executors.newFixedThreadPool 用它，任务堆积会 OOM，所以阿里规约要求手动 new ThreadPoolExecutor）；
- `SynchronousQueue` 不存元素（newCachedThreadPool 用，线程数可能爆）；
- `PriorityBlockingQueue` 带优先级。

**拒绝策略**：AbortPolicy（默认抛异常）、CallerRunsPolicy（调用线程自己执行，起到反压作用）、DiscardPolicy（静默丢弃）、DiscardOldestPolicy（丢最老的再提交）。

**参数怎么定**：CPU 密集 ≈ 核数+1；IO 密集 ≈ 核数 × (1 + 等待时间/计算时间)，实践中先估算再压测调优。

## 关键细节 / 易错点
- 队列满才扩线程，不是线程满才排队——很多人答反。
- 无界队列会让 maximumPoolSize 和拒绝策略永远不生效。
- 追问"线程池怎么监控"：getActiveCount / getQueue().size() / 完成任务数打点，队列积压告警。
- 追问"核心线程会回收吗"：默认不会，`allowCoreThreadTimeOut(true)` 可以让它也超时回收。
- 追问"异常怎么处理"：submit 的异常被 Future 吞掉，需要 get() 才抛；execute 会抛到 UncaughtExceptionHandler。

## 关联
- 相关知识点：[[wiki/os/process-thread-coroutine.md]]
- 常见追问链：七参数 → 提交流程 → 队列/拒绝策略选型 → 参数怎么定 → 怎么监控

## 面经来源
淘宝闪购 AI 应用开发一面（2026-07）
