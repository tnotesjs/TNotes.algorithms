# [0008. leetcode 查看不同语言环境的描述](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0008.%20leetcode%20%E6%9F%A5%E7%9C%8B%E4%B8%8D%E5%90%8C%E8%AF%AD%E8%A8%80%E7%8E%AF%E5%A2%83%E7%9A%84%E6%8F%8F%E8%BF%B0)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 如何在 Leetcode 中查看不同语言环境的描述呢？](#3--如何在-leetcode-中查看不同语言环境的描述呢)
- [4. 💻 使用内置的数据结构简化题解 - Leetcode - 703. 数据流中的第 K 大元素](#4--使用内置的数据结构简化题解---leetcode---703-数据流中的第-k-大元素)
- [5. ⚠️ 注意版本问题](#5-️-注意版本问题)
- [6. 🔗 引用](#6--引用)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- 在 leetcode 中查看不同语言环境的描述

## 2. 🫧 评价

- 如果你也在 Leetcode 上刷题，这篇笔记中记录的内容或许能够帮你在某些题中以更简洁的写法去解决问题。

## 3. 🤔 如何在 Leetcode 中查看不同语言环境的描述呢？

在左上角的代码选择下拉列表中，语言选项的右侧有一个叹号 icon，点击这个 icon 即可查看官方对该语言执行环境的相关描述。

::: swiper

![语言环境查看的入口](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-05-55-11.png)

![JavaScript 执行环境说明](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-05-56-22.png)

:::

## 4. 💻 使用内置的数据结构简化题解 - Leetcode - 703. 数据流中的第 K 大元素

以 JavaScript 为例，我们可以直接使用 [datastructures-js][2] 内置的 [MinPriorityQueue][4] 最小优先队列的写法完成这题 [Leetcode - 703. 数据流中的第 K 大元素][3]。

因为在 JavaScript 的执行环境描述中，官方说了可以使用 [datastructures-js][2] 中所提供的数据类型。

::: code-group

```js [✅ 手写最小堆的写法]
/**
 * @param {number} k
 * @param {number[]} nums
 */
var KthLargest = function (k, nums) {
  this.k = k
  this.heap = []

  // 初始化堆
  for (const num of nums) {
    this.add(num)
  }
}

/**
 * @param {number} val
 * @return {number}
 */
KthLargest.prototype.add = function (val) {
  // 向堆中添加元素
  this.heap.push(val)
  this.heapifyUp(this.heap.length - 1)

  // 如果堆的大小超过k，移除最小元素
  if (this.heap.length > this.k) {
    this.heap[0] = this.heap[this.heap.length - 1]
    this.heap.pop()
    this.heapifyDown(0)
  }

  // 返回第k大的元素（堆顶）
  return this.heap[0]
}

// 最小堆向上调整
KthLargest.prototype.heapifyUp = function (index) {
  while (index > 0) {
    const parentIndex = Math.floor((index - 1) / 2)
    if (this.heap[index] >= this.heap[parentIndex]) {
      break
    }
    ;[this.heap[index], this.heap[parentIndex]] = [
      this.heap[parentIndex],
      this.heap[index],
    ]
    index = parentIndex
  }
}

// 最小堆向下调整
KthLargest.prototype.heapifyDown = function (index) {
  while (index < this.heap.length) {
    let minIndex = index
    const leftChildIndex = index * 2 + 1
    const rightChildIndex = index * 2 + 2

    if (
      leftChildIndex < this.heap.length &&
      this.heap[leftChildIndex] < this.heap[minIndex]
    ) {
      minIndex = leftChildIndex
    }

    if (
      rightChildIndex < this.heap.length &&
      this.heap[rightChildIndex] < this.heap[minIndex]
    ) {
      minIndex = rightChildIndex
    }

    if (minIndex === index) {
      break
    }

    ;[this.heap[index], this.heap[minIndex]] = [
      this.heap[minIndex],
      this.heap[index],
    ]
    index = minIndex
  }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * var obj = new KthLargest(k, nums)
 * var param_1 = obj.add(val)
 */
```

```js [✅ MinPriorityQueue 写法]
/**
 * @param {number} k
 * @param {number[]} nums
 */
var KthLargest = function (k, nums) {
  this.k = k
  // 使用最小堆
  this.minHeap = new MinPriorityQueue()

  // 初始化堆
  for (const num of nums) {
    this.add(num)
  }
}

/**
 * @param {number} val
 * @return {number}
 */
KthLargest.prototype.add = function (val) {
  // 向堆中添加元素
  this.minHeap.enqueue(val)

  // 如果堆的大小超过k，移除最小元素
  if (this.minHeap.size() > this.k) {
    this.minHeap.dequeue()
  }

  // 返回第k大的元素（堆顶）
  return this.minHeap.front()
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * var obj = new KthLargest(k, nums)
 * var param_1 = obj.add(val)
 */
```

:::

## 5. ⚠️ 注意版本问题

虽然官方说引用了 `datastructures-js` 库，但是并没有说明用的是哪个版本，不同版本之间在调用语法上就存在差异，并且有些还是不兼容的。

之前使用 `MinPriorityQueue` 的写法提交的时候没有通过，随后给官方提了反馈，写这篇笔记时（2025.09.23）再次提交又通过了，-\_-||。

- 当时提交没有通过，在网上查了些相关资料，也有人遇到 [类似的问题][5]，原因出在了 Leetcode 中使用的 `datastructures-js` 的版本过旧导致的 BUG。
- ![图 3](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-06-56-08.png)
- 本来是写这篇笔记来吐槽 Leetcode 没给相关库加上版本描述的说明的，写到题解的时候，发现证据木得了～，然后又屁颠儿屁颠儿得去改笔记…… -\_-||。
- 可能是官方 fix 了，但是目前也依旧没有在执行环境说明面板中看到库版本信息，姑且认为是最新的包吧，后续若再遇到类似的问题，留个心眼。

::: swiper

![JavaScript 执行环境说明](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-05-56-22.png)

:::

## 6. 🔗 引用

- [datastructures-js github][1]
- [datastructures-js 官网][2]
  - [Priority Queue][4]
- [Leetcode - 703. 数据流中的第 K 大元素][3]
- [datastructures-js Issues - 我在 leetcode 中对 MinPriorityQueue 实例化，然后使用 enquene 方法报错][5]

[1]: https://github.com/datastructures-js
[2]: https://datastructures-js.info/
[3]: https://leetcode.cn/problems/kth-largest-element-in-a-stream/description/
[4]: https://datastructures-js.info/docs/priority-queue
[5]: https://github.com/datastructures-js/priority-queue/issues/21
