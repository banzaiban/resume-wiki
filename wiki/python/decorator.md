# Python 装饰器及应用场景

> tags: python, decorator, 装饰器, functools
> weight: 1
> updated: 2026-08-04

## 核心结论
装饰器本质是高阶函数：接收函数、返回包装后的函数，`@deco` 只是 `f = deco(f)` 的语法糖。包装时用 `functools.wraps` 保留原函数元信息。典型场景：日志、计时、鉴权、缓存、重试、事务、路由注册。

## 展开
- **基本结构**：
```python
import functools

def log_time(f):
    @functools.wraps(f)  # 保留 __name__/__doc__，否则元信息被覆盖
    def wrapper(*args, **kwargs):
        start = time.time()
        try:
            return f(*args, **kwargs)
        finally:
            print(f.__name__, time.time() - start)
    return wrapper
```
- **带参装饰器**：三层嵌套——`@retry(3)` 先拿到参数返回真正的装饰器：`retry(3) → deco → wrapper`。
- **类装饰器**：实现 `__call__` 的类可当装饰器（需要维护状态时用，如计数）。
- **应用场景**：AOP 式横切逻辑——接口日志/耗时统计、权限校验（先查 token 再进函数）、缓存（lru_cache 就是装饰器）、失败重试、事务包装、Flask/FastAPI 的路由注册（`@app.get("/x")` 把函数注册进路由表）。
- 多个装饰器叠加时**自底向上**应用（离函数近的先包）。

## 关键细节 / 易错点
- 追问"为什么要 functools.wraps"：不包的话 f.__name__ 变成 "wrapper"，日志、文档、框架反射全乱。
- 追问"装饰器执行时机"：装饰器本身在**定义时**执行（import 时），wrapper 在调用时执行——注册类逻辑放装饰器体，每调用一次的逻辑放 wrapper。
- 追问"怎么保留签名"：wraps + 必要时 `inspect.signature`；FastAPI 等框架依赖签名做参数注入。

## 关联
- 相关知识点：[[wiki/python/asyncio-vs-threading-mutex.md]]
- 常见追问链：本质是什么 → 带参怎么写 → 执行时机 → 实际用在哪

## 面经来源
- 快手 AI 应用开发一面（2026-08）：介绍一下 Python 中的装饰器及其应用场景
