# [0010. 堆](https://github.com/tnotesjs/TNotes.algorithms/tree/main/notes/0010.%20%E5%A0%86)

<!-- region:toc -->

- [1. 🎯 本节内容](#1--本节内容)
- [2. 🫧 评价](#2--评价)
- [3. 🤔 “堆”是什么？](#3--堆是什么)
- [4. 🤔 “堆”和“优先队列”有什么关系？](#4--堆和优先队列有什么关系)
- [5. 🤔 “堆”都有哪些常用操作？](#5--堆都有哪些常用操作)
- [6. 🤔 如何实现一个“堆”？](#6--如何实现一个堆)
  - [6.1. 堆的存储与表示](#61-堆的存储与表示)
  - [6.2. 访问堆顶元素](#62-访问堆顶元素)
  - [6.3. 元素入堆](#63-元素入堆)
  - [6.4. 堆顶元素出堆](#64-堆顶元素出堆)
- [7. 🤔 “堆”都有那些常见应用场景？](#7--堆都有那些常见应用场景)
- [8. 🔗 引用](#8--引用)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- “堆”

## 2. 🫧 评价

- 如果是首次学习“堆”的话，推荐阅读 👉 [hello-algo - 堆][2]，笔记中记录的大部分内容均搬运自 `hello-algo`。

## 3. 🤔 “堆”是什么？

- 堆（heap）是一种满足特定条件的完全二叉树，主要可分为两种类型：
  - 小顶堆（min heap）：$任意节点的值 <= 其子节点的值$。
  - 大顶堆（max heap）：$任意节点的值 >= 其子节点的值$。
- ![heap](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-08-11-33.png)
- 堆作为完全二叉树的一个特例，具有以下特性：
  - 最底层节点靠左填充，其他层的节点都被填满。
  - 我们将二叉树的根节点称为“堆顶”，将底层最靠右的节点称为“堆底”。
  - 对于大顶堆（小顶堆），堆顶元素（根节点）的值是最大（最小）的。

## 4. 🤔 “堆”和“优先队列”有什么关系？

---

$$
堆(heap) = 优先队列(priority queue)
$$

---

- 需要指出的是，许多编程语言提供的是 **优先队列（priority queue）**，这是一种抽象的数据结构，定义为具有优先级排序的队列。
- 实际上，**堆通常用于实现优先队列，大顶堆相当于元素按从大到小的顺序出队的优先队列**。
- 从使用角度来看，我们 **可以将“优先队列”和“堆”看作等价的数据结构**。
- 可以对两者不做特别区分，统一称作“堆”。

## 5. 🤔 “堆”都有哪些常用操作？

- 堆的常用操作见下表（具体的方法名需要根据编程语言来确定）：

<!-- <p align="center"> 表 <id> &nbsp; 堆的操作效率 </p> -->

| 方法名      | 描述                                             | 时间复杂度  |
| ----------- | ------------------------------------------------ | ----------- |
| `push()`    | 元素入堆                                         | $O(\log n)$ |
| `pop()`     | 堆顶元素出堆                                     | $O(\log n)$ |
| `peek()`    | 访问堆顶元素（对于大 / 小顶堆分别为最大 / 小值） | $O(1)$      |
| `size()`    | 获取堆的元素数量                                 | $O(1)$      |
| `isEmpty()` | 判断堆是否为空                                   | $O(1)$      |

- 在实际应用中，我们可以直接使用编程语言提供的堆类（或优先队列类）。
- 类似于排序算法中的“从小到大排列”和“从大到小排列”，我们可以通过设置一个 `flag` 或修改 `Comparator` 实现“小顶堆”与“大顶堆”之间的转换。

::: code-group

```python [Python]
# 初始化小顶堆
min_heap, flag = [], 1
# 初始化大顶堆
max_heap, flag = [], -1

# Python 的 heapq 模块默认实现小顶堆
# 考虑将“元素取负”后再入堆，这样就可以将大小关系颠倒，从而实现大顶堆
# 在本示例中，flag = 1 时对应小顶堆，flag = -1 时对应大顶堆

# 元素入堆
heapq.heappush(max_heap, flag * 1)
heapq.heappush(max_heap, flag * 3)
heapq.heappush(max_heap, flag * 2)
heapq.heappush(max_heap, flag * 5)
heapq.heappush(max_heap, flag * 4)

# 获取堆顶元素
peek: int = flag * max_heap[0] # 5

# 堆顶元素出堆
# 出堆元素会形成一个从大到小的序列
val = flag * heapq.heappop(max_heap) # 5
val = flag * heapq.heappop(max_heap) # 4
val = flag * heapq.heappop(max_heap) # 3
val = flag * heapq.heappop(max_heap) # 2
val = flag * heapq.heappop(max_heap) # 1

# 获取堆大小
size: int = len(max_heap)

# 判断堆是否为空
is_empty: bool = not max_heap

# 输入列表并建堆
min_heap: list[int] = [1, 3, 2, 5, 4]
heapq.heapify(min_heap)
```

```cpp [C++]
/* 初始化堆 */
// 初始化小顶堆
priority_queue<int, vector<int>, greater<int>> minHeap;
// 初始化大顶堆
priority_queue<int, vector<int>, less<int>> maxHeap;

/* 元素入堆 */
maxHeap.push(1);
maxHeap.push(3);
maxHeap.push(2);
maxHeap.push(5);
maxHeap.push(4);

/* 获取堆顶元素 */
int peek = maxHeap.top(); // 5

/* 堆顶元素出堆 */
// 出堆元素会形成一个从大到小的序列
maxHeap.pop(); // 5
maxHeap.pop(); // 4
maxHeap.pop(); // 3
maxHeap.pop(); // 2
maxHeap.pop(); // 1

/* 获取堆大小 */
int size = maxHeap.size();

/* 判断堆是否为空 */
bool isEmpty = maxHeap.empty();

/* 输入列表并建堆 */
vector<int> input{1, 3, 2, 5, 4};
priority_queue<int, vector<int>, greater<int>> minHeap(input.begin(), input.end());
```

:::

::: warning 注意

- 并非所有语言都提供了内置的 Heap 类，比如 JS，如果需要使用 Heap 类的话，可以考虑自己手写实现，也可以考虑直接使用第三方库，比如 [datastructures-js][6] 中封装好的 Heap。

:::

## 6. 🤔 如何实现一个“堆”？

- 下文实现的是大顶堆。若要将其转换为小顶堆，只需将所有大小逻辑判断进行逆转（例如，将 $\geq$ 替换为 $\leq$ ）。感兴趣的读者可以自行实现。

### 6.1. 堆的存储与表示

- 完全二叉树非常适合用数组来表示，由于堆正是一种完全二叉树，**因此我们将采用数组来存储堆**。
- 当使用数组表示二叉树时，元素代表节点值，索引代表节点在二叉树中的位置。**节点指针通过索引映射公式来实现**。
- ![图 1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-23-08-30-37.png)
- 给定索引 $i$，则：
  - 左子节点的索引为 $2i + 1$
  - 右子节点的索引为 $2i + 2$
  - 父节点的索引为 $(i - 1) / 2$（向下整除）
  - 当索引越界时，表示空节点或节点不存在
- 我们可以将索引映射公式封装成函数，方便后续使用：

::: code-group

```js
/* 获取左子节点的索引 */
#left(i) {
    return 2 * i + 1;
}

/* 获取右子节点的索引 */
#right(i) {
    return 2 * i + 2;
}

/* 获取父节点的索引 */
#parent(i) {
    return Math.floor((i - 1) / 2); // 向下整除
}
```

:::

### 6.2. 访问堆顶元素

- 堆顶元素即为二叉树的根节点，也就是列表的首个元素：

::: code-group

```js
/* 访问堆顶元素 */
peek() {
    return this.#maxHeap[0];
}
```

:::

### 6.3. 元素入堆

- ↑ 从底至顶堆化（heapify）
  - 给定元素 `val` ，我们首先将其添加到堆底。
  - 添加之后，由于 `val` 可能大于堆中其他元素，堆的成立条件可能已被破坏，**因此需要修复从插入节点到根节点的路径上的各个节点**，这个操作被称为 **堆化（heapify）**。
  - 考虑从入堆节点开始，**从底至顶执行堆化**。
- 堆化过程的图示说明
  - 如下图所示，我们比较插入节点与其父节点的值，如果插入节点更大，则将它们交换。
  - 然后继续执行此操作，从底至顶修复堆中的各个节点，直至越过根节点或遇到无须交换的节点时结束。

::: swiper

![1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-20-22.png)

![2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-20-38.png)

![3](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-21-02.png)

![4](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-21-11.png)

![5](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-21-38.png)

![6](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-21-50.png)

![7](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-22-05.png)

![8](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-22-12.png)

![9](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-22-28.png)

:::

- 设节点总数为 $n$ ，则树的高度为 $O(\log n)$ 。由此可知，堆化操作的循环轮数最多为 $O(\log n)$ ，**元素入堆操作的时间复杂度为 $O(\log n)$** 。代码如下所示：

::: code-group

```js
/* 元素入堆 */
push(val) {
    // 添加节点
    this.#maxHeap.push(val);
    // 从底至顶堆化
    this.#siftUp(this.size() - 1);
}

/* 从节点 i 开始，从底至顶堆化 */
#siftUp(i) {
    while (true) {
        // 获取节点 i 的父节点
        const p = this.#parent(i);
        // 当“越过根节点”或“节点无须修复”时，结束堆化
        if (p < 0 || this.#maxHeap[i] <= this.#maxHeap[p]) break;
        // 交换两节点
        this.#swap(i, p);
        // 循环向上堆化
        i = p;
    }
}
```

:::

### 6.4. 堆顶元素出堆

- ↓ 从顶至底执行堆化（heapify）
  - 堆顶元素是二叉树的根节点，即列表首元素。
  - 如果我们直接从列表中删除首元素，那么二叉树中所有节点的索引都会发生变化，这将使得后续使用堆化进行修复变得困难。
  - 为了尽量减少元素索引的变动，我们采用以下操作步骤。
    1. 交换堆顶元素与堆底元素（交换根节点与最右叶节点）。
    2. 交换完成后，将堆底从列表中删除（注意，由于已经交换，因此实际上删除的是原来的堆顶元素）。
    3. 从根节点开始，**从顶至底执行堆化**。
- 堆化过程的图示说明
  - 如下图所示，**“从顶至底堆化”的操作方向与“从底至顶堆化”相反**，我们将根节点的值与其两个子节点的值进行比较，将最大的子节点与根节点交换。
  - 然后循环执行此操作，直到越过叶节点或遇到无须交换的节点时结束。

::: swiper

![1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-29-40.png)

![2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-29-49.png)

![3](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-29-57.png)

![4](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-04.png)

![5](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-12.png)

![6](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-17.png)

![7](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-24.png)

![8](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-31.png)

![9](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-37.png)

![10](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-24-12-30-43.png)

:::

- 与元素入堆操作相似，堆顶元素出堆操作的时间复杂度也为 $O(\log n)$ 。代码如下所示：

::: code-group

```js
/* 元素出堆 */
pop() {
    // 判空处理
    if (this.isEmpty()) throw new Error('堆为空');
    // 交换根节点与最右叶节点（交换首元素与尾元素）
    this.#swap(0, this.size() - 1);
    // 删除节点
    const val = this.#maxHeap.pop();
    // 从顶至底堆化
    this.#siftDown(0);
    // 返回堆顶元素
    return val;
}

/* 从节点 i 开始，从顶至底堆化 */
#siftDown(i) {
    while (true) {
        // 判断节点 i, l, r 中值最大的节点，记为 ma
        const l = this.#left(i),
            r = this.#right(i);
        let ma = i;
        if (l < this.size() && this.#maxHeap[l] > this.#maxHeap[ma]) ma = l;
        if (r < this.size() && this.#maxHeap[r] > this.#maxHeap[ma]) ma = r;
        // 若节点 i 最大或索引 l, r 越界，则无须继续堆化，跳出
        if (ma === i) break;
        // 交换两节点
        this.#swap(i, ma);
        // 循环向下堆化
        i = ma;
    }
}
```

:::

## 7. 🤔 “堆”都有那些常见应用场景？

- **优先队列**
  - 堆通常作为实现优先队列的首选数据结构，其入队和出队操作的时间复杂度均为 $O(\log n)$ ，而建堆操作为 $O(n)$ ，这些操作都非常高效。
- **堆排序**
  - 给定一组数据，我们可以用它们建立一个堆，然后不断地执行元素出堆操作，从而得到有序数据。
  - 然而，我们通常会使用一种更优雅的方式实现堆排序，详见“堆排序”章节。 <Badge type="warning" text="⏰ TODO" />
- **获取最大的 $k$ 个元素**
  - 这是一个经典的算法问题，同时也是一种典型应用，例如选择热度前 10 的新闻作为微博热搜，选取销量前 10 的商品等。
  - Leetcode 相关例题：[703. 数据流中的第 K 大元素][1]

## 8. 🔗 引用

- [hello-algo - 堆][2]
- [知乎：图解:最小堆构建、存储、插入、删除过程][3]
- [大根堆、小根堆、节点插入和删除，可视化动画][4]
  - [github][5]
- Leetcode 相关例题
  - [703. 数据流中的第 K 大元素][1]
- [datastructures-js][6]

[1]: https://leetcode.cn/problems/kth-largest-element-in-a-stream/
[2]: https://www.hello-algo.com/chapter_heap/
[3]: https://zhuanlan.zhihu.com/p/341418979
[4]: https://gallery.selfboot.cn/zh/algorithms/heap
[5]: https://github.com/selfboot/ai_gallery
[6]: https://datastructures-js.info/
