# Python 异步与多线程的互斥

> tags: python, asyncio, threading, GIL, lock, 并发
> weight: 1
> updated: 2026-07-30

## 核心结论
互斥都靠锁：协程用 `asyncio.Lock`，多线程用 `threading.Lock`，两者不能混用。因为 CPython 有 GIL，多线程跑 CPU 密集任务无加速，所以 IO 密集场景更多用协程，或用消息队列解耦。

## 展开
- `threading.Lock`：内核级/阻塞式，`acquire()` 会阻塞整个线程。适合多线程共享可变状态。
- `asyncio.Lock`：协作式，`async with lock:` 时挂起当前协程、让出事件循环，不阻塞线程。只在单事件循环内有效。
- 为什么不能混用：在协程里用 `threading.Lock.acquire()` 会阻塞事件循环所在的线程，所有协程全部卡死；`asyncio.Lock` 不是线程安全的，跨线程用会有竞态。
- GIL：CPython 同一时刻只有一个线程执行字节码。CPU 密集 → 多进程（`multiprocessing`）或 C 扩展；IO 密集 → 协程（单线程高并发、无线程切换开销）或多线程。
- 跨线程与协程交互的正确姿势：`loop.call_soon_threadsafe()`、`asyncio.run_coroutine_threadsafe()`、`loop.run_in_executor()`（把阻塞调用丢线程池，避免卡事件循环）。

## 关键细节 / 易错点
- GIL 保证的是解释器内部状态安全，**不保证业务临界区原子性**（如 `x += 1` 仍需锁，因为它是多条字节码）。
- `asyncio.Lock` 在 3.10+ 不再接受 `loop` 参数；它靠 `await` 让出控制权实现互斥。
- 追问"协程里要调用阻塞库怎么办"：`run_in_executor` / `asyncio.to_thread()`。
- 追问"多进程怎么互斥"：`multiprocessing.Lock`（基于信号量），或外部化到 Redis 分布式锁。

## 关联
- 常见追问链：GIL 是什么 → CPU/IO 密集怎么选并发模型 → 协程里调阻塞代码怎么办 → 分布式锁

## 面经来源
快手 Agent 研发一面（2026-07）
