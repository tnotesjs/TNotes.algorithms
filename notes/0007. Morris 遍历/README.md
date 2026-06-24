# [0007. Morris 遍历](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0007.%20Morris%20%E9%81%8D%E5%8E%86)

<!-- region:toc -->

- [1. 本节内容](#1-本节内容)
- [2. 评价](#2-评价)
- [3. bilibili：【Manim】Morris 中序遍历可视化（UP：RocksLi）](#3-bilibilimanimmorris-中序遍历可视化uprocksli)
- [4. Morris 遍历算法及其核心思想](#4-morris-遍历算法及其核心思想)
- [5. 为什么要使用 Morris 遍历？](#5-为什么要使用-morris-遍历)
- [6. Morris 遍历 vs. 递归/迭代遍历](#6-morris-遍历-vs-递归迭代遍历)
- [7. Morris 遍历的优点：极致的空间效率](#7-morris-遍历的优点极致的空间效率)
- [8. Morris 遍历的缺点：会临时修改树结构](#8-morris-遍历的缺点会临时修改树结构)
- [9. Morris 中序遍历](#9-morris-中序遍历)
  - [9.1. 中序 Morris 遍历步骤](#91-中序-morris-遍历步骤)
    - [情况一：`cur.left == None`](#情况一curleft--none)
    - [情况二：`cur.left != None`](#情况二curleft--none)
      - [1 `pred.right == None`](#1-predright--none)
      - [2 `pred.right == cur`](#2-predright--cur)
  - [9.2. 代码实现](#92-代码实现)
  - [9.3. 辅助理解](#93-辅助理解)
- [10. Morris 前序遍历](#10-morris-前序遍历)
- [11. Morris 后序遍历](#11-morris-后序遍历)
- [12. Morris 遍历适合什么时候用？](#12-morris-遍历适合什么时候用)
- [13. 相关 Leetcode 例题](#13-相关-leetcode-例题)
- [14. 小结](#14-小结)
- [15. 你知道 Morris 遍历的名称由来吗？【AI】](#15-你知道-morris-遍历的名称由来吗ai)
- [16. 引用](#16-引用)

<!-- endregion:toc -->

## 1. 本节内容

- Morris 遍历

## 2. 评价

Morris 遍历的的核心思想是：临时修改二叉树的空右指针，让遍历完左子树后能够回到当前节点。这种临时指针也叫“线索”，在学习“Morris 遍历”的时候，重点就是理解“线索”。

::: tip

可以将笔记中记录的 B 站的 Morris 中序遍历可视化的视频看一下，重点关注“线索”的建立和销毁。

:::

## 3. bilibili：【Manim】Morris 中序遍历可视化（UP：RocksLi）

<B id="BV17H4y1p7DD" />

![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs-2026@main/2026-06-24-07-32-08.png)

- Current Node：当前节点
- Predecessor：前驱节点
- Finished：完成遍历的节点

![svg](./assets/1.svg)

线索：临时指针，也就是任意节点的左子树中的最右节点。

- 6 的前驱节点是 5（因为 6 为根的左子树中，最右节点是 5，在中序遍历时，访问完 5 之后，下一个就是访问 6）
- 2 的前驱节点是 1
- 4 的前驱节点是 3
- 9 的前驱节点是 8

## 4. Morris 遍历算法及其核心思想

Morris 遍历算法：

- Morris 遍历算法是一种用于二叉树的遍历算法（可用于中序、前序），它能在 O(n) 时间 和 O(1) 空间内完成遍历。
- 这意味着它不需要使用递归（避免系统调用栈）或自定义栈（避免额外空间），是真正的常数空间复杂度算法。

核心思想：“线索化”

- 利用树中大量空闲的叶子节点的右指针，指向其后继节点（在中序遍历序列中的下一个节点），从而在遍历完成后可以轻松回溯到上层节点。
- 这个过程也称为“线索化”。

## 5. 为什么要使用 Morris 遍历？

传统的二叉树遍历方法有两种：

1. 递归：实现简单，但空间复杂度为 O(h)（h 为树高），最坏情况下（链状树）为 O(n)。这是由于函数调用栈造成的。
2. 迭代（使用栈）：同样，空间复杂度为 O(h)，因为需要显式地维护一个栈。

当树的节点数量 `n` 非常大，或者对空间效率要求极高时（例如在嵌入式系统中），O(h) 的空间开销可能是不可接受的。

Morris 遍历完美地解决了这个“O(h) 的空间开销”问题。

## 6. Morris 遍历 vs. 递归/迭代遍历

| 特性         | Morris 遍历                        | 递归/迭代遍历    |
| :----------- | :--------------------------------- | :--------------- |
| 时间复杂度   | O(n)                               | O(n)             |
| 空间复杂度   | O(1)                               | O(h)             |
| 是否修改原树 | 是（临时修改，会恢复）             | 否               |
| 实现难度     | 较复杂                             | 简单             |
| 适用场景     | 对空间有极致要求，且允许临时修改树 | 绝大多数通用场景 |

## 7. Morris 遍历的优点：极致的空间效率

- 空间复杂度 O(1)
- 不需要递归
- 不需要显式栈
- 适合要求极致空间优化的遍历问题

## 8. Morris 遍历的缺点：会临时修改树结构

- 算法过程稍复杂，不易理解（代码比递归复杂）
- 会临时修改树的结构（虽然最后会恢复），这在并发环境下是危险的（如果其他线程同时在读这棵树，会看到错误的结构）
- 不适合不可变树结构
- 如果中途异常退出，可能导致树结构没有恢复

## 9. Morris 中序遍历

中序遍历是最经典和常见的 Morris 遍历应用，前序和后序的步骤需要做一些调整，可以先把中序遍历的核心步骤看懂，再去看前序和后序的步骤就很简单了。

### 9.1. 中序 Morris 遍历步骤

设当前节点为 `cur`。

#### 情况一：`cur.left == None`

说明没有左子树，可以直接访问当前节点。

```text
访问 cur
cur = cur.right
```

---

#### 情况二：`cur.left != None`

找到 `cur` 的中序前驱 `pred`：

```python
pred = cur.left
while pred.right is not None and pred.right is not cur:
    pred = pred.right
```

然后分两种情况。

##### 1 `pred.right == None`

说明这是第一次来到 `cur`。

建立临时线索：

```python
pred.right = cur
cur = cur.left
```

意思是：先去遍历左子树，遍历完后可以通过 `pred.right` 回到 `cur`。

##### 2 `pred.right == cur`

说明左子树已经遍历完，并且现在是通过临时线索回到 `cur` 的。

这时需要：

```python
pred.right = None   # 恢复原树结构
访问 cur
cur = cur.right
```

### 9.2. 代码实现

```py
def morris_inorder(root):
    result = []
    cur = root # 从根节点开始，初始化 cur 指针指向根节点

    while cur: # 当 cur 不为空时，循环执行以下操作
        if cur.left is None: # 如果 cur 没有左子节点
            result.append(cur.val) # 访问当前节点 cur.val
            cur = cur.right # 将 cur 移动到其右子节点
        else: # 如果 cur 有左子节点

            # 开始找 cur 节点在中序遍历序列中的直接前驱节点
            # 具体方法：
            # 从 cur 的左子节点开始，一直向右遍历
            # 直到某个节点的 right 指针为空或指向当前 cur 节点

            pred = cur.left # 从 cur 的左子节点开始

            # 找到 cur 的中序前驱
            # 如果节点的 right 指针非空且未指向当前 cur 节点
            # 就一直向右遍历
            while pred.right is not None and pred.right is not cur:
                pred = pred.right

            if pred.right is None:
                # 第一次到 cur，建立线索
                pred.right = cur
                cur = cur.left
            else:
                # 第二次到 cur，说明左子树已经遍历完成
                pred.right = None
                result.append(cur.val)
                cur = cur.right

    return result
```

### 9.3. 辅助理解

- 想象一下，这个算法像是在“攀岩”。
- 当你想往左子树下降时，你会先扔下一根绳子（建立线索）绑在下一层的某个点（前驱节点）上。
- 当你沿着左子树探索到底后，拉着这根绳子就能轻松回到原来的上层节点（当前节点）。
- 回去之后，你把绳子收起来（断开线索，恢复树结构），然后继续向右子树前进。

## 10. Morris 前序遍历

前序遍历顺序是：当前节点 -> 左子树 -> 右子树

和中序 Morris 的区别是：当第一次来到一个有左子树的节点时，就要立刻访问它。

代码如下：

```py
def morris_preorder(root):
    result = []
    cur = root#

    while cur:
        if cur.left is None:
            result.append(cur.val)
            cur = cur.right
        else:
            pred = cur.left

            while pred.right is not None and pred.right is not cur:
                pred = pred.right

            if pred.right is None:
                # 第一次到 cur，前序遍历此时访问
                result.append(cur.val)
                pred.right = cur
                cur = cur.left
            else:
                # 第二次到 cur，恢复线索
                pred.right = None
                cur = cur.right

    return result
```

## 11. Morris 后序遍历

后序遍历顺序是：左子树 -> 右子树 -> 当前节点

Morris 后序遍历比中序和前序复杂很多。

常见做法是：

1. 创建一个虚拟节点 `dummy`。
2. 令 `dummy.left = root`。
3. 对每个节点，在第二次回到它时，逆序访问它左子树的右边界。
4. 访问完后恢复右边界。

核心原因是：后序遍历需要在左右子树都处理完之后才访问当前节点，而 Morris 的线索回退天然更适合中序和前序。

面试或实际使用中，最常考的是：

```text
Morris 中序遍历
Morris 前序遍历
```

后序 Morris 理解即可，写起来相对容易出错。

## 12. Morris 遍历适合什么时候用？

适合这些场景：

- 题目明确要求 O(1) 空间遍历二叉树
- 需要避免递归栈溢出
- 面试中考察二叉树高级遍历技巧

不太适合这些场景：

- 普通业务代码
- 树结构不允许修改
- 并发环境
- 代码可读性比空间优化更重要

## 13. 相关 Leetcode 例题

::: tip

和二叉树遍历相关的例题都可以下是否可以使用 Morris 遍历来解题。

:::

- [94. 二叉树的中序遍历【简单】][4]
- [99. 恢复二叉搜索树【中等】][5]
- [501. 二叉搜索树中的众数【简单】][3]

## 14. 小结

Morris 遍历就是：利用二叉树中原本为空的右指针，临时建立“回到当前节点”的线索，从而在不使用栈和递归的情况下完成遍历，并在遍历过程中恢复原树结构。

其中最经典的是中序 Morris 遍历：

```
对于任意根节点：
  没有左子树：访问当前节点，去右边
  有左子树：找到左子树最右节点
    第一次遇到：建立线索，去左边
    第二次遇到：删除线索，访问当前节点，去右边
```

## 15. 你知道 Morris 遍历的名称由来吗？【AI】

知道。**Morris 遍历的 “Morris” 不是缩写，而是作者姓氏**。

它通常追溯到 **J. M. Morris / Joseph M. Morris** 在 1979 年发表的论文 **“Traversing Binary Trees Simply and Cheaply”**。DBLP 记录中列出的信息是：Joseph M. Morris，发表于 _Information Processing Letters_ 9(5)，197–200 页，1979 年。([dblp.org](https://dblp.org/rec/journals/ipl/Morris79a))

这篇论文的摘要说，它描述了一种简单高效的二叉树遍历算法，时间与节点数成正比，并且**不需要运行时栈，也不需要在节点里加 flag 位**。([xueshu.baidu.com](https://xueshu.baidu.com/usercenter/paper/show?paperid=2060d1710dcbf0934053a492fadc86bd)) 后来大家就把这种利用临时线索指针、O(1) 额外空间的遍历方法称为 **Morris traversal / Morris 遍历**。

有个小细节：中文资料里有时会写成 **J. H. Morris**，甚至说他就是 KMP 算法里的 Morris。但 KMP 论文的作者是 **James H. Morris Jr.**，而 Morris 遍历的书目信息通常列为 **Joseph M. Morris / J. M. Morris**，所以这点很可能是资料混淆。([dblp.org](https://dblp.org/rec/journals/siamcomp/KnuthMP77?utm_source=chatgpt.com))

## 16. 引用

- [bilibili - 【Manim】Morris 中序遍历可视化][1]
- [知乎 - 神级遍历——morris][2]
- [leetcode - 501. 二叉搜索树中的众数][3]
- [leetcode - 94. 二叉树的中序遍历][4]
- [leetcode - 99. 恢复二叉搜索树][5]

[1]: https://www.bilibili.com/video/BV17H4y1p7DD/
[2]: https://zhuanlan.zhihu.com/p/101321696
[3]: https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/
[4]: https://leetcode.cn/problems/binary-tree-inorder-traversal/
[5]: https://leetcode.cn/problems/recover-binary-search-tree/
