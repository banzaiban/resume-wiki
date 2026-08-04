# 无重复字符的最长子串

> tags: algorithm, 滑动窗口, 哈希表, leetcode-3
> weight: 1
> updated: 2026-08-04

## 核心结论
滑动窗口 + 哈希表记录每个字符最后出现的位置：右指针扩张，遇到重复字符时左指针跳到"该字符上次出现位置 + 1"（注意取 max 防止左指针回退），全程 O(n)。

## 展开
```python
def lengthOfLongestSubstring(s: str) -> int:
    last = {}          # 字符 -> 最后出现的下标
    left = best = 0
    for right, ch in enumerate(s):
        if ch in last and last[ch] >= left:
            left = last[ch] + 1   # 收缩窗口到重复字符之后
        last[ch] = right
        best = max(best, right - left + 1)
    return best
```
- **为什么 O(n)**：左右指针都只右移不回退，每字符最多被访问两次。
- 关键不变量：窗口 `[left, right]` 内始终无重复字符。
- 变体：字符集有限（如 ASCII）可用数组代替哈希表；求最长长度 vs 求子串本身（记录 best 时的 left/right）。

## 关键细节 / 易错点
- 易错点：`left = last[ch] + 1` 前要判断 `last[ch] >= left`（或写 `left = max(left, last[ch] + 1)`）——否则遇到像 "abba" 会回退 left，把已移出窗口的旧重复算进来。
- 面试先讲暴力 O(n²) 再优化到滑动窗口，讲清"为什么左指针不用回扫"。

## 关联
- 相关知识点：[[wiki/algorithm/longest-palindromic-substring.md]]
- 常见追问链：暴力怎么写 → 滑动窗口为什么对 → 不变量 → 复杂度证明

## 面经来源
- 快手 AI 应用开发一面（2026-08）：手撕——无重复字符的最长子串
