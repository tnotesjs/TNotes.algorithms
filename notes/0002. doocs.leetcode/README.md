# [0002. doocs.leetcode](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0002.%20doocs.leetcode)

<!-- region:toc -->

- [1. 🫧 评价](#1--评价)
- [2. 🆚 对比 TNotes.leetcode](#2--对比-tnotesleetcode)
- [3. 🔗 引用](#3--引用)

<!-- endregion:toc -->

## 1. 🫧 评价

[doocs/leetcode][2] 是一个很好的 Leetcode 资源站点，里边儿基本上将 Leetcode 上所有的例题都做了汇总、分类，目前（25.09）star 数 `34.9k`。

![图 0](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-14-11-51-54.png)

## 2. 🆚 对比 TNotes.leetcode

[doocs/leetcode][2] 和自己目前正在写的 [TNotes.leetcode][1] 非常像，前者更规范内容更全面，后者更适合用于做刷题笔记。

| 对比项 | doocs/leetcode | TNotes.leetcode |
| --- | --- | --- |
| Star 数量（25-09） | 34.9k | 0 |
| 首次提交 | 2018-09 | 2025-03 |
| 语言支持 | 中文 + English | 中文 |
| 题目数量 | `3k+` | 根据实际刷题进度走，边刷边整理，截止目前（2025-09-14）已整理 `93`，已刷 `223` |
| 题目描述处理方式 | 从 Leetcode 爬取 HTML 元素节点（猜测） | 使用油猴脚本解析并整理为原始 Markdown 格式 |
| 题目描述优缺点 | 优点：批量处理<br>缺点：非原始 Markdown 格式 | 优点：原始 Markdown 格式，便于二次编辑<br>缺点：需逐题手动处理 |
| 题解结构 | 先提供解法说明，再提供源码 | 先提供源码，再提供解法说明 |
| 技术栈 | 未查 | 基于 Vitepress 实现 |
| 题解维护方式 | 直接将题解源码插入 markdown 中 | 可通过 `<<< ./solutions/1/1.js` 方式引入源码，修改源码文件即可 |
| 项目性质 | 开源社区项目 | 个人学习笔记知识库 |
| 灵活性 | 需要遵循已形成的统一规范 | 高度灵活，可自定义组件和体验 |
| 发布方式 | 需要 PR 和审核 | 可自主决定发布时间及内容 |
| 社区认可度 | 高（已建立统一规范） | 无（尚未形成统一规范） |

## 3. 🔗 引用

- [GitHub 技术社区 Doocs][1]
- [doocs/leetcode][2]
  - github 项目仓库地址
  - 简介：🔥 LeetCode solutions in any programming language | 多种编程语言实现 LeetCode、《剑指 Offer（第 2 版）》、《程序员面试金典（第 6 版）》题解
  - 评价：leetcode 上的相关例题，大多都可以在这个开源项目中找到。包括 leetcode 题库中的 3k 多道题，往期竞赛例题、《剑指 Offer》例题等等。
- [doocs/leetcode - 在线阅读 LeetCode 全解][3]
- [TNotes.leetcode][4]

[1]: https://github.com/doocs
[2]: https://github.com/doocs/leetcode
[3]: https://doocs.github.io/leetcode/lc/1/
[4]: https://tnotesjs.github.io/TNotes.leetcode/
