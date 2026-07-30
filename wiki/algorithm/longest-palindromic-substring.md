# 最长回文子串

> tags: algorithm, 字符串, 动态规划, 中心扩展, manacher, leetcode-5
> weight: 1
> updated: 2026-07-30

## 核心结论
面试主写**中心扩展法**：枚举每个中心（奇偶两种）向两边扩，O(n²) 时间 O(1) 空间。追问优化答 **Manacher**：插分隔符统一奇偶 + 利用已知回文的对称性复用信息，O(n)。

## 展开
**中心扩展（必须会写）**
```python
def longestPalindrome(s: str) -> str:
    def expand(l, r):
        while l >= 0 and r < len(s) and s[l] == s[r]:
            l -= 1; r += 1
        return l + 1, r - 1  # 回退到最后合法位置
    start, end = 0, 0
    for i in range(len(s)):
        for l, r in (expand(i, i), expand(i, i + 1)):  # 奇中心、偶中心
            if r - l > end - start:
                start, end = l, r
    return s[start:end + 1]
```
- 2n-1 个中心（n 个奇 + n-1 个偶），每次扩展 O(n)，总 O(n²)。

**DP 解法（可提及）**：`dp[i][j] = (s[i]==s[j]) and dp[i+1][j-1]`，O(n²) 时间 O(n²) 空间，不如中心扩展省空间，但思路要能讲。

**Manacher 思路（会讲即可，一般不要求手写）**
1. 预处理：插 `#`（如 `aba` → `#a#b#a#`），所有回文变奇数长度，统一处理。
2. 维护当前已知最右回文边界 R 及其中心 C；数组 p[i] 记录以 i 为中心的回文半径。
3. 求 p[i] 时：若 i < R，利用对称点 mirror = 2C - i，初始化 p[i] = min(p[mirror], R - i)，再尝试继续扩展——避免从 1 开始重复比较。
4. 每个字符最多被扩展比较一次 → 均摊 O(n)。

## 关键细节 / 易错点
- 中心扩展别忘偶数长度中心（`expand(i, i+1)`），最经典的 bug。
- expand 返回时要回退一步（循环退出时 l,r 已越界/不匹配）。
- Manacher 的关键句：**用已经算出的回文信息（对称性）避免重复扩展**——面试答到这句就及格。
- 追问变体：回文子序列（LC516，是 DP 不是本题解法）、回文串个数（LC647，同样中心扩展）。

## 关联
- 常见追问链：中心扩展写码 → 复杂度 → 能否 O(n) → Manacher 核心思想 → 回文子序列区别

## 面经来源
快手 Agent 研发一面（2026-07）：写中心扩展，口述 Manacher 思路
