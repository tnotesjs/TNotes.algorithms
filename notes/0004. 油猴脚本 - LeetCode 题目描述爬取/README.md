# [0004. 油猴脚本 - LeetCode 题目描述爬取](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0004.%20%E6%B2%B9%E7%8C%B4%E8%84%9A%E6%9C%AC%20-%20LeetCode%20%E9%A2%98%E7%9B%AE%E6%8F%8F%E8%BF%B0%E7%88%AC%E5%8F%96)

<!-- region:toc -->

- [1. 🫧 评论](#1--评论)
- [2. 💻 scripts - 油猴一键复制 Leetcode 题目描述的实现脚本](#2--scripts---油猴一键复制-leetcode-题目描述的实现脚本)

<!-- endregion:toc -->

## 1. 🫧 评论

- 该油猴脚本用于从 LeetCode 网站上抓取题目描述并转换成 Markdown 格式，方便用户复制使用。
- 脚本通过添加在右键菜单中注入一个能一键复制 LeetCode 题目描述为 markdown 文本的菜单项。
- 复制的 markdown 内容适配 TNotes.leetcode 中的题目描述格式。在 TNotes.leetcode 中的相关 LeetCode 例题的 `📝 题目描述` 部分，就是通过这个脚本来获取的。

## 2. 💻 scripts - 油猴一键复制 Leetcode 题目描述的实现脚本

::: code-group

<<< ./scripts/youhou.js [scripts/youhou.js]

<<< ./scripts/youhou-2.js [scripts/youhou-2.js]

:::

- 🤔 `scripts/youhou.js` 和 `scripts/youhou-2.js` 有什么区别？
  - `scripts/youhou.js` 是将示例部分解析到代码块中；
  - `scripts/youhou-2.js` 是将示例部分解析正常解析到 `.md` 中；
  - 如有其它需求，可自行扩展脚本。
- 🤔 如何使用脚本？
  - 【1】将上述脚本直接丢到油猴中，然后保存即可在油猴插件中安装此脚本。
  - 【2】使用方式也非常简单，只需要在对应的题解描述区域右键，然后点击【力扣题目转 Markdown】即可复制题目描述。

::: swiper

![1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2024-10-24-22-15-35.png)

![2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2024-10-24-22-17-24.png)

:::

- 🤔 复制的效果如何？
  - 下面是以 `scripts/youhou.js` 作用于第一题为例复制下来的结果。
  - 内容复制下来后，丢到 VSCode 的 markdown 文件中，保存文件内容，自动格式化后，基本没有太多地方需要调整。

::: swiper

![第一题](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-08-05-07-49-29.png)

:::

- 🤔 在 VSCode 中配置 `.md` 文件的内容在保存后自动完成格式化？

::: code-group

```json [.vscode/settings.json]
{
  // 保存时设置文件格式。
  "editor.formatOnSave": true,
  "[markdown]": {
    // 定义一个默认格式化程序, 该格式化程序优先于所有其他格式化程序设置。
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

:::
