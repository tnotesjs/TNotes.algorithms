# [0014. KMP 算法](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0014.%20KMP%20%E7%AE%97%E6%B3%95)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 KMP 是什么？](#3--kmp-是什么)
- [4. 🤔 KMP 算法的基本思想是什么？](#4--kmp-算法的基本思想是什么)
- [5. 🤖 kmp 算法是中国计算机考研常考的知识点吗？](#5--kmp-算法是中国计算机考研常考的知识点吗)
  - [5.1. 常考形式包括：](#51-常考形式包括)
- [6. 🔍 相关 LeetCode 例题 - `28. 实现 strStr()`](#6--相关-leetcode-例题---28-实现-strstr)
  - [6.1. 📝 题目描述](#61--题目描述)
  - [6.2. 解法 1：暴力解法](#62-解法-1暴力解法)
  - [6.3. 解法 2：KMP](#63-解法-2kmp)
    - [核心步骤](#核心步骤)
    - [🤔 “最大相同前缀”是什么意思？](#-最大相同前缀是什么意思)
    - [初始化 next 数组](#初始化-next-数组)
    - [查找匹配项](#查找匹配项)
- [7. 🔗 引用](#7--引用)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- KMP 算法简介
- LeetCode 例题 - `28. 实现 strStr()`

## 2. 🫧 评价

KMP 算法的核心：构建 next 数组。

通过对比 LeetCode 例题 - `28. 实现 strStr()` 的两种解法（暴力解法 vs. KMP 算法）来认识 KMP 算法的核心优化点（失配的时候无需暴力重头来过，会依据 next 数组记录的值来决定下次的开始匹配位置）。

学习一个短语 -> Find needle in haystack 大海捞针。

## 3. 🤔 KMP 是什么？

KMP（Knuth-Morris-Pratt）算法是一种高效的字符串匹配算法，由 Donald Knuth、Vaughan Pratt 和 James H. Morris 独立发明，并于 1977 年发表。这种算法的主要优点在于它能够在线性时间内完成模式串在文本串中的查找，即其时间复杂度为 O(n+m)，其中 n 是文本串的长度，m 是模式串的长度。

- 文本串 - 待匹配的字符串，也称为主串（haystack）。
- 模式串 - 要在文本串中查找的字符串，也称为子串（needle）。

KMP 算法因其高效性和实用性，在文本处理、搜索引擎等领域有着广泛的应用。通过预处理模式串，KMP 能够在最坏情况下也能保证线性时间复杂度，这使得它成为解决字符串匹配问题的一个非常优秀的算法。

## 4. 🤔 KMP 算法的基本思想是什么？

KMP 算法的核心在于利用已经匹配过的信息，避免从头开始重新匹配。当模式串的一个位置与主串不匹配时，KMP 算法可以知道之前已经匹配过的字符信息，并据此决定模式串应该移动的位置，而不是简单地将模式串向后移动一位。

KMP 算法的关键组件【关键在于理解匹配过程】

1. 部分匹配表（Partial Match Table, PMT），也称为“next 数组”：
   - 这是一个数组，用于存储模式串中每个前缀的最大相同前后缀的长度。
   - 例如，对于模式串 "ABCDABD"，其 PMT（或 next 数组）可能是 [0, 0, 0, 0, 1, 2, 0]。这意味着，如果在模式串的第 `j` 个位置失配，那么模式串应该回退到 `next[j - 1]` 的位置继续匹配。
     - 具体来说，假设匹配到 ABCDAB D 加粗的 D 位置时候出现了失配的情况，此时 j 为 6，这时候就需要让 j 回退，如果暴力处理的话，一旦出现了失配的情况，那么直接将 j 回退到开头 0 的位置，一切都从头开始。但是 next 可以对回退的逻辑进行优化，此时只需要回退到 `next[j - 1]` 的位置（也就是 `next[6 - 1] => 2`）即可，表示下次匹配的位置是 AB C DABD 中索引为 2 的位置，也就是加粗的 C 的位置开始，这是因为 AB CDABD 和 ABCD AB D 子串 AB 相同，虽然 D 位置出现了失配，但是没必要回退到开头，D 前边的 AB 子串和开始位置开始的 AB 是相同的，下次直接从 C 开始即可。
   - PMT 的构建是 KMP 算法中较为复杂的部分，但一旦构建好，就能大大提高匹配效率。
2. 匹配过程：
   - 在匹配过程中，KMP 算法使用两个指针，一个指向主串（haystack），另一个指向模式串（needle）。
   - 当发生失配时，模式串指针不会回溯到起始位置，而是根据 PMT 移动到下一个可能匹配的位置。
   - 如果模式串完全匹配，则返回匹配的起始位置；否则，继续匹配直到主串结束或找到匹配。

## 5. 🤖 kmp 算法是中国计算机考研常考的知识点吗？

是的，KMP 算法（Knuth-Morris-Pratt 算法）是中国计算机专业硕士研究生入学考试（即“计算机考研”）中 较为常见且重要的考点，尤其在以下几类考试中：

- 408 计算机学科专业基础综合（全国统考）：KMP 算法属于“数据结构”部分，常出现在选择题或应用题中。虽然近年直接大题考查频率略降，但其核心思想（如 部分匹配表 / next 数组构造、字符串匹配过程中的“跳转避免回溯”思想）仍常以选择题或填空题形式出现。
- 各高校自命题科目（如清华、浙大、北航、华科等）：很多学校偏爱考查 KMP，因其能综合考察对算法思想、数组操作、递推逻辑和编程能力的理解。部分院校曾出过手写 `next` 数组 / `nextval` 数组，甚至要求写出完整匹配过程或代码片段。

### 5.1. 常考形式包括：

- ✅ 给定模式串，求其 `next` 或 `nextval` 数组；
- ✅ 描述 KMP 相比朴素匹配的优化点（核心：利用已匹配信息避免主串指针回溯）；
- ✅ 给出匹配过程，问某一步主串/模式串指针位置；
- ✅ （较少但存在）手写 `next` 数组构造函数或完整 KMP 匹配伪代码。

> 📌 提示：KMP 算法本身不难，但 `next` 数组的定义在不同教材中略有差异（如是否从 0 开始、是否加 1 偏移），建议以目标院校指定参考书（如严蔚敏《数据结构》C 语言版）为准。近年趋势更侧重理解而非死记公式。

需要我帮你梳理 KMP 的核心推导逻辑或典型真题示例吗？

## 6. 🔍 相关 LeetCode 例题 - `28. 实现 strStr()`

### 6.1. 📝 题目描述

- [leetcode](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)

给你两个字符串 `haystack` 和 `needle` ，请你在 `haystack` 字符串中找出 `needle` 字符串的第一个匹配项的下标（下标从 0 开始）。如果 `needle` 不是 `haystack` 的一部分，则返回 `-1`。

示例 1：

```txt
输入：haystack = "sadbutsad", needle = "sad"
输出：0
解释："sad" 在下标 0 和 6 处匹配。
第一个匹配项的下标是 0 ，所以返回 0 。
```

示例 2：

```txt
输入：haystack = "leetcode", needle = "leeto"
输出：-1
解释："leeto" 没有在 "leetcode" 中出现，所以返回 -1 。
```

提示：

- `1 <= haystack.length, needle.length <= 10^4`
- `haystack` 和 `needle` 仅由小写英文字符组成

### 6.2. 解法 1：暴力解法

```js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function (haystack, needle) {
  const n = haystack.length
  const m = needle.length

  if (m === 0) return 0 // 特殊情况处理：空字符串

  for (let i = 0; i <= n - m; i++) {
    // 外层循环遍历主串
    let match = true
    for (let j = 0; j < m; j++) {
      // 内层循环检查子串是否匹配
      if (haystack[i + j] !== needle[j]) {
        match = false
        break
      }
    }
    if (match) return i // 如果匹配成功，返回起始索引
  }

  return -1 // 如果没有找到匹配的子串，返回 -1
}
```

- 时间复杂度：$O(n \times m)$，其中 n 是 haystack 的长度，m 是 needle 的长度
- 空间复杂度：$O(1)$，只使用了常数级别的额外空间

双层循环暴力匹配。外层遍历主串，内层逐字符比较子串，一旦不匹配就从主串的下一位重新开始匹配。

### 6.3. 解法 2：KMP

```js
/**
 * @param {string} haystack
 * @param {string} needle
 * @return {number}
 */
var strStr = function (haystack, needle) {
  const n = haystack.length,
    m = needle.length

  if (m === 0) return 0

  // 初始化 next
  const next = new Array(m).fill(0)
  for (let i = 1, j = 0; i < m; i++) {
    while (j > 0 && needle[j] !== needle[i]) j = next[j - 1] // j 收缩
    if (needle[i] === needle[j]) j++ // j 扩散
    next[i] = j // 更新 next[i]
  }

  // 查找匹配项
  for (let i = 0, j = 0; i < n; i++) {
    while (j > 0 && needle[j] !== haystack[i]) j = next[j - 1]
    if (haystack[i] === needle[j]) j++
    if (j === m) return i - m + 1
  }

  return -1
}
```

- 时间复杂度：$O(n + m)$，其中 n 是 haystack 的长度，m 是 needle 的长度，构建 next 数组需要 $O(m)$，匹配过程需要 $O(n)$
- 空间复杂度：$O(m)$，需要额外的 next 数组存储模式串的前缀信息

KMP 算法通过预处理模式串构建 next 数组记录最大相同前后缀长度，匹配失败时根据 next 数组智能移动模式串指针，避免重复比较，将时间复杂度优化到线性。

这种写法对 s.2 的暴力匹配做了优化，如果发现不匹配的情况，不会暴力地直接回溯到子串的开头位置，而是根据 next 中记录的索引来决定当本次匹配失败时，下次匹配开始的位置应该是哪。理解 next 是理解 KMP 算法的关键。

#### 核心步骤

- 步骤 1. 初始化 next 数组：这部分代码构建了 PMT（或称 next 数组）。通过遍历模式串，计算每个位置的 最大相同前后缀长度，从而指导后续匹配时如何移动模式串。
- 步骤 2. 匹配过程：使用两个指针 i 和 j 分别遍历主串和模式串。当字符匹配时，两个指针都向前移动；如果不匹配，模式串指针 j 会根据 next 数组进行调整，以尝试新的匹配位置。如果模式串完全匹配，则返回匹配的起始位置。

步骤 1、2 的实现流程是 KMP 算法的核心，它们的实现逻辑是非常类似的。

#### 🤔 “最大相同前缀”是什么意思？

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

## 7. 🔗 引用

- [参考 solutions - 【宫水三叶】简单题学 KMP 算法][1]
- [哔哩哔哩 - 最浅显易懂的 KMP 算法讲解][2]
  - 理解 next 数组（next 数组有什么用？如何构建 next 数组？）是 KMP 算法的核心，可以多看看视屏中的动画演示。

[1]: https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/575568/shua-chuan-lc-shuang-bai-po-su-jie-fa-km-tb86/
[2]: https://www.bilibili.com/video/BV1AY4y157yL/?spm_id_from=333.337.search-card.all.click&vd_source=f8873530fc00410ea3fbec0d4b875972
