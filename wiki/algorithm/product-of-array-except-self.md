# 除自身以外数组的乘积

> tags: algorithm, 数组, 前缀积, leetcode-238
> weight: 1
> updated: 2026-07-30

## 核心结论
用前缀积和后缀积：`ans[i] = 左边所有数之积 × 右边所有数之积`。两次遍历（先从左累乘前缀，再从右累乘后缀），O(n) 时间、O(1) 额外空间（不算输出数组），且**不使用除法**（题目要求，也天然规避 0 的问题）。

## 展开
```python
def productExceptSelf(nums: list[int]) -> list[int]:
    n = len(nums)
    ans = [1] * n
    prefix = 1
    for i in range(n):          # ans[i] 先存左侧前缀积
        ans[i] = prefix
        prefix *= nums[i]
    suffix = 1
    for i in range(n - 1, -1, -1):  # 再乘上右侧后缀积
        ans[i] *= suffix
        suffix *= nums[i]
    return ans
```
- 第一趟：`ans[i] = nums[0..i-1]` 的乘积（i=0 时为 1）。
- 第二趟：用一个变量滚动维护右侧乘积，直接乘进 ans，省掉后缀数组。

## 关键细节 / 易错点
- **为什么不用除法**：总积除以 nums[i] 看似 O(n)，但数组含 0 时失效（要分 0 个数讨论：无 0 正常除；1 个 0 只有该位非 0；≥2 个 0 全为 0），且题目通常明确禁止。前缀后缀法一律不需分类讨论。
- 前缀写入时要**先赋值再累乘**（ans[i] 不含 nums[i] 自身），顺序写反是经典 bug。
- 空间复杂度声明：输出数组不计入额外空间，所以是 O(1)。
- 同类题：接雨水（左右最大值）、字符串前后缀统计——都是"左右两趟扫描"套路。

## 关联
- 相关知识点：[[wiki/algorithm/longest-palindromic-substring.md]]
- 常见追问链：能不能用除法 → 0 怎么处理 → 空间能否 O(1) → 同类前后缀套路题

## 面经来源
淘宝闪购 AI 应用开发一面（2026-07）
