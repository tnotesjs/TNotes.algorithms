# [0014. KMP 算法](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0014.%20KMP%20%E7%AE%97%E6%B3%95)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 考察 KMP 算法的 LeetCode 例题都有哪些？](#3--考察-kmp-算法的-leetcode-例题都有哪些)
  - [3.1. 题目描述](#31-题目描述)
  - [3.2. 解法 1：暴力解法](#32-解法-1暴力解法)
  - [3.3. 解法 2：KMP](#33-解法-2kmp)
    - [核心步骤](#核心步骤)
    - [“最大相同前缀”是什么](#最大相同前缀是什么)
    - [初始化 next 数组](#初始化-next-数组)
    - [查找匹配项](#查找匹配项)
- [4. 🤔 KMP 是什么？](#4--kmp-是什么)
- [5. 🤔 haystack、needle 是什么？](#5--haystackneedle-是什么)
- [6. 🤔 KMP 算法的基本思想是什么？](#6--kmp-算法的基本思想是什么)
- [7. 🤔 next 数组是什么？](#7--next-数组是什么)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- KMP 算法简介
- LeetCode 例题 - `28. 实现 strStr()`

## 2. 🫧 评价

KMP 算法的核心：构建并维护 next 数组。

这篇笔记将以 LeetCode 例题 `28. 实现 strStr()` 为引子来引出 KMP 算法。

## 3. 🤔 考察 KMP 算法的 LeetCode 例题都有哪些？

以下是 LeetCode 上和 KMP 算法相关的一些例题：

- `28. 实现 strStr()`
- `214. 最短回文串`
- `459. 重复的子字符串`
- `686. 重复叠加字符串匹配`
- `1392. 最长快乐前缀`
- `2851. 字符串转换`
- `3008. 找出数组中的美丽下标 II`
- `3036. 匹配模式数组的子数组数目 II`
- `3009. 折线图上的最大交点数量`

::: tip

为了方便介绍 KMP 算法，这里以 `28. 实现 strStr()` 为示例加以说明，其他例题可以在刷完这篇笔记之后，再去找来练手巩固！

下面将通过对比两种解法（暴力解法 vs. KMP 算法）来认识 KMP 算法的核心优化点（失配的时候无需暴力重头来过，会依据 next 数组记录的值来决定下次的开始匹配位置）。

:::

### 3.1. 题目描述

- [leetcode](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1`。

---

示例 1：

```txt
输入：haystack = "sadbutsad", needle = "sad"
输出：0
```

解释：

"sad" 在下标 0 和 6 处匹配。

第一个匹配项的下标是 0，所以返回 0 。

---

示例 2：

```txt
输入：haystack = "leetcode", needle = "leeto"
输出：-1
```

解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1。

---

提示：

- `1 <= haystack.length, needle.length <= 10^4`
- `haystack` 和 `needle` 仅由小写英文字符组成

### 3.2. 解法 1：暴力解法

```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n, m = len(haystack), len(needle)

        if m == 0:
            return 0

        for i in range(n - m + 1):
            j = 0
            while j < m and haystack[i + j] == needle[j]:
                j += 1
            if j == m:
                return i

        return -1
```

- 时间复杂度：$O(n \times m)$，其中 $n$ 和 $m$ 分别是 haystack 和 needle 的长度，最坏情况下每个起点都要完整比较一次 needle
- 空间复杂度：$O(1)$，只使用了常数级别的额外空间

算法思路：

- 枚举 haystack 中所有可能的匹配起点，起点范围是 $[0, n - m]$
- 对每个起点继续逐字符比较 `haystack[i + j]` 和 `needle[j]`
- 一旦出现失配，立即结束当前起点的检查并尝试下一个起点
- 如果某个起点能连续匹配完全部 $m$ 个字符，那么它就是第一个匹配项的下标

### 3.3. 解法 2：KMP

```py
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n, m = len(haystack), len(needle)

        if m == 0:
            return 0

        next = [0] * m
        j = 0
        for i in range(1, m):
            while j > 0 and needle[i] != needle[j]:
                j = next[j - 1]
            if needle[i] == needle[j]:
                j += 1
            next[i] = j

        j = 0
        for i, ch in enumerate(haystack):
            while j > 0 and ch != needle[j]:
                j = next[j - 1]
            if ch == needle[j]:
                j += 1
            if j == m:
                return i - m + 1

        return -1
```

- 时间复杂度：$O(n + m)$，其中 $n$ 和 $m$ 分别是 haystack 和 needle 的长度，构建 next 数组需要 $O(m)$，匹配过程只需线性扫描 haystack 一次
- 空间复杂度：$O(m)$，需要额外的 next 数组存储模式串的前缀信息

算法思路：

- 先为 needle 构建 next 数组，`next[i]` 表示子串 `needle[0...i]` 的最长相等真前后缀长度
- 匹配时同时扫描 haystack 和 needle，字符相等就让两个指针一起前进
- 如果发生失配，不回退 haystack 指针，而是利用 next 数组把 needle 指针跳到上一个可复用的位置
- 当 needle 指针走到长度 $m$ 时，说明已经找到完整匹配，起始下标就是当前下标减去 $m - 1$

KMP 算法通过预处理模式串构建 next 数组记录最大相同前后缀长度，匹配失败时根据 next 数组智能移动模式串指针，避免重复比较，将时间复杂度优化到线性。

这种写法对 s.2 的暴力匹配做了优化，如果发现不匹配的情况，不会暴力地直接回溯到子串的开头位置，而是根据 next 中记录的索引来决定当本次匹配失败时，下次匹配开始的位置应该是哪。理解 next 是理解 KMP 算法的关键。

#### 核心步骤

- 步骤 1. 初始化 next 数组：这部分代码构建了 PMT（或称 next 数组）。通过遍历模式串，计算每个位置的 最大相同前后缀长度，从而指导后续匹配时如何移动模式串。
- 步骤 2. 匹配过程：使用两个指针 i 和 j 分别遍历主串和模式串。当字符匹配时，两个指针都向前移动；如果不匹配，模式串指针 j 会根据 next 数组进行调整，以尝试新的匹配位置。如果模式串完全匹配，则返回匹配的起始位置。

步骤 1、2 的实现流程是 KMP 算法的核心，它们的实现逻辑是非常类似的。

#### “最大相同前缀”是什么

- `next[i] = xxx` 表示位置 i 的最大相同前后缀长度是 `xxx`。
- 示例：`needle = "sad"` 对应的 next 数组为 `[0, 0, 0]`。
- 示例：`leeto` 对应的 next 数组为 `[0, 0, 0, 0, 0]`。
- 示例：`needle = "ababca"` 对应的 next 数组为 `[0, 0, 1, 2, 0, 1]`。
  - ![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2024-11-17-12-17-38.png)
- 官方提供的示例：
  - ![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2024-11-17-12-27-49.png)

#### 初始化 next 数组

```js
const next = new Array(m).fill(0)
for (let i = 1, j = 0; i < m; i++) {
  while (j > 0 && needle[j] !== needle[i]) j = next[j - 1]
  if (needle[i] === needle[j]) j++
  next[i] = j
}
```

- m 表示子串 needle 的长度。
- i 表示子串 needle 的第几个位置。
- j 是一个辅助变量，用于记录子串的当前位置失配时，需要回退到哪里。
- `const next = new Array(m).fill(0);` next 数组中存放的成员，表示的含义是如果在匹配过程中，如果子串的某个位置失配了，那么需要根据 next 来决定下次匹配的开始位置，所以在初始化的时候，需要根据子串的长度来初始化。
- `for (let i = 1, j = 0; i < m; i++) { ... }`
  - 循环初始语句：
    - `let i = 1` 如果第一个位置就失配了，不用纠结，直接从头开始，所以不需要去管 `next[0]` 的值，它肯定得是 `0`。
    - `j = 0` 辅助变量 j 默认从 0 开始走。
  - 循环条件：
    - `i < m` 根据子串的长度来决定外层循环的次数，每次循环决定一个当前的 `next[i]` 的值。
  - 循环体：
    - 失配 - 收缩：`while (j > 0 && needle[j] !== needle[i]) j = next[j - 1]` 失配，`j` 回退到 `next[j - 1]` 的位置。
    - 匹配 - 扩散：`if (needle[i] === needle[j]) j++` 匹配，j 向后移动一位。
    - 更新 next：`next[i] = j` 将 j 的值赋给 `next[i]`。
      - next 记录后续【查找匹配项】的流程中，当子串的 `j` 位置失配 `needle[j] !== haystack[i]` 时，指针 `j` 应该回溯到的位置是 `next[j - 1]`。

#### 查找匹配项

```js
for (let i = 0, j = 0; i < n; i++) {
  while (j > 0 && needle[j] !== haystack[i]) j = next[j - 1]
  if (haystack[i] === needle[j]) j++
  if (j === m) return i - m + 1
}
```

- i 表示匹配到了主串 haystack 的第几个位置。
- m 表示子串 needle 的长度。
- j 表示当前匹配到了子串 needle 的第几个位置。
- `while (j > 0 && needle[j] !== haystack[i]) j = next[j - 1]` 失配，`j` 回退到 `next[j - 1]` 的位置。
- `if (haystack[i] === needle[j]) j++` 匹配，j 向后移动一位。
- `if (j === m) return i - m + 1` 一旦条件成立，表示在主串中找到了满足条件的连续子串，将匹配的起始位置返回。

## 4. 🤔 KMP 是什么？

KMP（Knuth-Morris-Pratt）算法是一种高效的字符串匹配算法，由 Donald Knuth、Vaughan Pratt 和 James H. Morris 独立发明，并于 1977 年发表。这种算法的主要优点在于它能够在线性时间内完成模式串在文本串中的查找，即其时间复杂度为 $O(n+m)$，其中 $n$ 是文本串（haystack）的长度，$m$ 是模式串（needle）的长度。

KMP 算法因其高效性和实用性，在文本处理、搜索引擎等领域有着广泛的应用。通过预处理模式串，KMP 能够在最坏情况下也能保证线性时间复杂度，这使得它成为解决字符串匹配问题的一个非常优秀的算法。

## 5. 🤔 haystack、needle 是什么？

- 文本串 - 待匹配的字符串，也称为主串（haystack）。
- 模式串 - 要在文本串中查找的字符串，也称为子串（needle）。

::: tip

- 学习一个短语 => Find needle in haystack 大海捞针
- 顾名思义 => 从 haystack 中找 needle

:::

## 6. 🤔 KMP 算法的基本思想是什么？

KMP 算法的核心在于利用已经匹配过的信息，避免从头开始重新匹配。当模式串的一个位置与主串不匹配时，KMP 算法可以知道之前已经匹配过的字符信息，并据此决定模式串应该移动的位置，而不是简单地将模式串向后移动一位。

## 7. 🤔 next 数组是什么？

部分匹配表（Partial Match Table, PMT），也称为“next 数组”，用于存储模式串中每个前缀的最大相同前后缀的长度。

例如，对于模式串 "ABCDABD"，其 PMT（或 next 数组）可能是 [0, 0, 0, 0, 1, 2, 0]。这意味着，如果在模式串的第 `j` 个位置失配，那么模式串应该回退到 `next[j - 1]` 的位置继续匹配。

具体来说，假设匹配到 ABCDAB D 加粗的 D 位置时候出现了失配的情况，此时 j 为 6，这时候就需要让 j 回退，如果暴力处理的话，一旦出现了失配的情况，那么直接将 j 回退到开头 0 的位置，一切都从头开始。但是 next 可以对回退的逻辑进行优化，此时只需要回退到 `next[j - 1]` 的位置（也就是 `next[6 - 1] => 2`）即可，表示下次匹配的位置是 AB C DABD 中索引为 2 的位置，也就是加粗的 C 的位置开始，这是因为 AB CDABD 和 ABCD AB D 子串 AB 相同，虽然 D 位置出现了失配，但是没必要回退到开头，D 前边的 AB 子串和开始位置开始的 AB 是相同的，下次直接从 C 开始即可。

PMT 的构建是 KMP 算法中较为复杂的部分，但一旦构建好，就能大大提高匹配效率。

匹配过程：

- 在匹配过程中，KMP 算法使用两个指针，一个指向主串（haystack），另一个指向模式串（needle）。
- 当发生失配时，模式串指针不会回溯到起始位置，而是根据 PMT 移动到下一个可能匹配的位置。
- 如果模式串完全匹配，则返回匹配的起始位置；否则，继续匹配直到主串结束或找到匹配。
