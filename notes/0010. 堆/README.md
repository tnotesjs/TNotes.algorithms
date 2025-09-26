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
- [7. 🤔 “建堆操作”是什么？](#7--建堆操作是什么)
- [8. 🤔 “建堆操作”如何实现？](#8--建堆操作如何实现)
  - [8.1. 方案 1：借助“入堆操作”实现](#81-方案-1借助入堆操作实现)
  - [8.2. 方案 2：通过遍历“堆化”实现](#82-方案-2通过遍历堆化实现)
- [9. 🤔 “堆”都有那些常见应用场景？](#9--堆都有那些常见应用场景)
- [10. 🤔 Top-K 问题是？](#10--top-k-问题是)
  - [10.1. 方法一：遍历选择](#101-方法一遍历选择)
  - [10.2. 方法二：排序](#102-方法二排序)
  - [10.3. 方法三：堆](#103-方法三堆)
- [11. 🎯 完成 Leetcode 703. 数据流中的第 K 大元素](#11--完成-leetcode-703-数据流中的第-k-大元素)
- [12. 🔗 引用](#12--引用)

<!-- endregion:toc -->

## 1. 🎯 本节内容

- “堆” 结构的特点及常见操作
- “堆” 结构的实现方案及复杂度分析
- Top-K 问题
- 完成对应的 Leetcode 练习 [703. 数据流中的第 K 大元素][1]

## 2. 🫧 评价

- 如果是首次学习“堆”的话，推荐阅读 👉 [hello-algo - 堆][2]，笔记中记录的大部分内容均搬运自 `hello-algo`。
- 查看堆化过程的动画 -> [大根堆、小根堆、节点插入和删除，可视化动画][4]

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

```javascript [JS]
// JavaScript 未提供内置 Heap 类
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

## 7. 🤔 “建堆操作”是什么？

- 在某些情况下，我们希望使用一个列表的所有元素来构建一个堆，这个过程被称为“建堆操作”。

## 8. 🤔 “建堆操作”如何实现？

### 8.1. 方案 1：借助“入堆操作”实现

- 我们首先创建一个空堆，然后遍历列表，依次对每个元素执行“入堆操作”，即先将元素添加至堆的尾部，再对该元素执行“从底至顶”堆化。
- 每当一个元素入堆，堆的长度就加一。由于节点是从顶到底依次被添加进二叉树的，因此堆是“自上而下”构建的。
- 设元素数量为 $n$ ，每个元素的入堆操作使用 $O(\log{n})$ 时间，因此该建堆方法的时间复杂度为 $O(n \log n)$ 。

### 8.2. 方案 2：通过遍历“堆化”实现

- 实际上，我们可以实现一种更为高效的建堆方法，共分为两步。
  1. 将列表所有元素原封不动地添加到堆中，此时堆的性质尚未得到满足。
  2. 倒序遍历堆（层序遍历的倒序），依次对每个非叶节点执行“从顶至底堆化”。
- **每当堆化一个节点后，以该节点为根节点的子树就形成一个合法的子堆**。
- 而由于是倒序遍历，因此堆是“自下而上”构建的。
- 之所以选择倒序遍历，是因为这样能够保证当前节点之下的子树已经是合法的子堆，这样堆化当前节点才是有效的。
- 值得说明的是，**由于叶节点没有子节点，因此它们天然就是合法的子堆，无须堆化**。
- 如以下代码所示，最后一个非叶节点是最后一个节点的父节点，我们从它开始倒序遍历并执行堆化：

::: code-group

```js
/* 构造方法，建立空堆或根据输入列表建堆 */
constructor(nums) {
    // 将列表元素原封不动添加进堆
    this.#maxHeap = nums === undefined ? [] : [...nums];
    // 堆化除叶节点以外的其他所有节点
    for (let i = this.#parent(this.size() - 1); i >= 0; i--) {
        this.#siftDown(i);
    }
}
```

:::

- 下面，我们来尝试推这种方案的算时间复杂度。
- 假设完全二叉树的节点数量为 $n$ ，则叶节点数量为 $(n + 1) / 2$ ，其中 $/$ 为向下整除。
- 因此需要堆化的节点数量为 $n - (n + 1) / 2 = (n - 1) / 2$ 。
- 在从顶至底堆化的过程中，每个节点最多堆化到叶节点，因此最大迭代次数为二叉树高度 $\log n$ 。
- 将上述两者相乘，可得到建堆过程的时间复杂度为 $O(n \log n)$ 。
- **但这个估算结果并不准确，因为我们没有考虑到二叉树底层节点数量远多于顶层节点的性质**。
- 接下来我们来进行更为准确的计算。
- 为了降低计算难度，假设给定一个节点数量为 $n$ 、高度为 $h$ 的“完美二叉树”，该假设不会影响计算结果的正确性。
- ![img](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-00-42-39.png)
- 如上图所示，节点“从顶至底堆化”的最大迭代次数等于该节点到叶节点的距离，而该距离正是“节点高度”。
- 因此，我们可以对各层的“节点数量 $\times$ 节点高度”求和，**得到所有节点的堆化迭代次数的总和**。

$$
T(h) = 2^0h + 2^1(h-1) + 2^2(h-2) + \dots + 2^{(h-1)}\times1
$$

- 化简上式需要借助中学的数列知识，先将 $T(h)$ 乘以 $2$ ，得到：

$$
\begin{aligned}
T(h) & = 2^0h + 2^1(h-1) + 2^2(h-2) + \dots + 2^{h-1}\times1 \newline
2 T(h) & = 2^1h + 2^2(h-1) + 2^3(h-2) + \dots + 2^{h}\times1 \newline
\end{aligned}
$$

- 使用错位相减法，用下式 $2 T(h)$ 减去上式 $T(h)$ ，可得：

$$
2T(h) - T(h) = T(h) = -2^0h + 2^1 + 2^2 + \dots + 2^{h-1} + 2^h
$$

- 观察上式，发现 $T(h)$ 是一个等比数列，可直接使用求和公式，得到时间复杂度为：

$$
\begin{aligned}
T(h) & = 2 \frac{1 - 2^h}{1 - 2} - h \newline
& = 2^{h+1} - h - 2 \newline
& = O(2^h)
\end{aligned}
$$

- 进一步，高度为 $h$ 的完美二叉树的节点数量为 $n = 2^{h+1} - 1$ ，易得复杂度为 $O(2^h) = O(n)$ 。
- 以上推算表明，**输入列表并建堆的时间复杂度为 $O(n)$ ，非常高效**。

## 9. 🤔 “堆”都有那些常见应用场景？

- **优先队列**
  - 堆通常作为实现优先队列的首选数据结构，其入队和出队操作的时间复杂度均为 $O(\log n)$ ，而建堆操作为 $O(n)$ ，这些操作都非常高效。
- **堆排序**
  - 给定一组数据，我们可以用它们建立一个堆，然后不断地执行元素出堆操作，从而得到有序数据。
  - 然而，我们通常会使用一种更优雅的方式实现堆排序，详见“堆排序”章节。 <Badge type="warning" text="⏰ TODO" />
- **获取最大的 $k$ 个元素**
  - 这是一个经典的算法问题（Top-K 问题），同时也是一种典型应用，例如选择热度前 10 的新闻作为微博热搜，选取销量前 10 的商品等。
  - Leetcode 相关例题：[703. 数据流中的第 K 大元素][1]

## 10. 🤔 Top-K 问题是？

::: warning Question

- 给定一个长度为 $n$ 的无序数组 `nums` ，请返回数组中最大的 $k$ 个元素。

:::

- 对于该问题，我们先介绍两种思路比较直接的解法，再介绍效率更高的堆解法。

### 10.1. 方法一：遍历选择

- 我们可以进行下图所示的 $k$ 轮遍历，分别在每轮中提取第 $1$、$2$、$\dots$、$k$ 大的元素，时间复杂度为 $O(nk)$ 。
- 此方法只适用于 $k \ll n$ 的情况，因为当 $k$ 与 $n$ 比较接近时，其时间复杂度趋向于 $O(n^2)$ ，非常耗时。

::: swiper

![遍历寻找最大的 k 个元素](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-31-34.png)

:::

::: tip

- 当 $k = n$ 时，我们可以得到完整的有序序列，此时等价于“选择排序”算法。

:::

### 10.2. 方法二：排序

- 如下图所示，我们可以先对数组 `nums` 进行排序，再返回最右边的 $k$ 个元素，时间复杂度为 $O(n \log n)$ 。
- 显然，该方法“超额”完成任务了，因为我们只需找出最大的 $k$ 个元素即可，而不需要排序其他元素。

::: swiper

![排序寻找最大的 k 个元素](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-31-53.png)

:::

### 10.3. 方法三：堆

- 我们可以基于堆更加高效地解决 Top-k 问题，流程如下图所示。
  1. 初始化一个小顶堆，其堆顶元素最小。
  2. 先将数组的前 $k$ 个元素依次入堆。
  3. 从第 $k + 1$ 个元素开始，若当前元素大于堆顶元素，则将堆顶元素出堆，并将当前元素入堆。
  4. 遍历完成后，堆中保存的就是最大的 $k$ 个元素。

::: swiper

![1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-33-14.png)

![2](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-33-25.png)

![3](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-33-35.png)

![4](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-33-43.png)

![5](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-33-55.png)

![6](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-34-02.png)

![7](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-34-09.png)

![8](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-34-15.png)

![9](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-18-34-23.png)

:::

::: code-group

```javascript [JS]
/* 元素入堆 */
function pushMinHeap(maxHeap, val) {
  // 元素取反
  maxHeap.push(-val)
}

/* 元素出堆 */
function popMinHeap(maxHeap) {
  // 元素取反
  return -maxHeap.pop()
}

/* 访问堆顶元素 */
function peekMinHeap(maxHeap) {
  // 元素取反
  return -maxHeap.peek()
}

/* 取出堆中元素 */
function getMinHeap(maxHeap) {
  // 元素取反
  return maxHeap.getMaxHeap().map((num) => -num)
}

/* 基于堆查找数组中最大的 k 个元素 */
function topKHeap(nums, k) {
  // 初始化小顶堆
  // 请注意：我们将堆中所有元素取反，从而用大顶堆来模拟小顶堆
  const maxHeap = new MaxHeap([])
  // 将数组的前 k 个元素入堆
  for (let i = 0; i < k; i++) {
    pushMinHeap(maxHeap, nums[i])
  }
  // 从第 k+1 个元素开始，保持堆的长度为 k
  for (let i = k; i < nums.length; i++) {
    // 若当前元素大于堆顶元素，则将堆顶元素出堆、当前元素入堆
    if (nums[i] > peekMinHeap(maxHeap)) {
      popMinHeap(maxHeap)
      pushMinHeap(maxHeap, nums[i])
    }
  }
  // 返回堆中元素
  return getMinHeap(maxHeap)
}
```

:::

- 总共执行了 $n$ 轮入堆和出堆，堆的最大长度为 $k$ ，因此时间复杂度为 $O(n \log k)$ 。该方法的效率很高，当 $k$ 较小时，时间复杂度趋向 $O(n)$ ；当 $k$ 较大时，时间复杂度不会超过 $O(n \log n)$ 。
- 另外，该方法适用于动态数据流的使用场景。在不断加入数据时，我们可以持续维护堆内的元素，从而实现最大的 $k$ 个元素的动态更新。

## 11. 🎯 完成 Leetcode 703. 数据流中的第 K 大元素

::: code-group

```js
/**
 * @param {number} k
 * @param {number[]} nums
 */
var KthLargest = function (k, nums) {
  this.k = k
  //#region 使用方案 1 建堆
  // // 实现一个小顶堆来维护前k大的元素
  // this.minHeap = [];

  // // 将初始数组中的元素逐个添加到堆中
  // for (const num of nums) {
  //     this.add(num);
  // }
  //#endregion

  //#region 使用方案 2 建堆
  // 先对数组进行排序，取最大的k个元素
  nums.sort((a, b) => b - a)
  this.minHeap = nums.slice(0, k)
  for (let i = this.parent(this.minHeap.length - 1); i >= 0; i--) {
    this.siftDown(i)
  }
  //#endregion
}

/**
 * 向数据流中添加元素并返回当前第k大的元素
 * @param {number} val
 * @return {number}
 */
KthLargest.prototype.add = function (val) {
  // 如果堆的大小小于k，直接入堆
  if (this.minHeap.length < this.k) {
    this.push(val)
  }
  // 如果新元素大于堆顶元素，则替换堆顶
  else if (val > this.peek()) {
    this.pop() // 移除堆顶（堆顶是在 val 添加之前的最小的元素，也就是第 K 大元素，在 val 添加之后，原来的堆顶就不是第 K 大元素了，因此直接将其移除。）
    this.push(val) // 添加新元素（将 val push 到堆中，重新堆化。）
  }

  // 返回堆顶元素，即第k大的元素
  return this.peek()
}

/**
 * 获取父节点索引
 * @param {number} i
 * @return {number}
 */
KthLargest.prototype.parent = function (i) {
  return Math.floor((i - 1) / 2)
}

/**
 * 获取左子节点索引
 * @param {number} i
 * @return {number}
 */
KthLargest.prototype.left = function (i) {
  return 2 * i + 1
}

/**
 * 获取右子节点索引
 * @param {number} i
 * @return {number}
 */
KthLargest.prototype.right = function (i) {
  return 2 * i + 2
}

/**
 * 交换堆中两个元素
 * @param {number} i
 * @param {number} j
 */
KthLargest.prototype.swap = function (i, j) {
  ;[this.minHeap[i], this.minHeap[j]] = [this.minHeap[j], this.minHeap[i]]
}

/**
 * 访问堆顶元素
 * @return {number}
 */
KthLargest.prototype.peek = function () {
  return this.minHeap[0]
}

/**
 * 元素入堆（小顶堆）
 * @param {number} val
 */
KthLargest.prototype.push = function (val) {
  // 添加节点
  this.minHeap.push(val)
  // 从底至顶堆化
  this.siftUp(this.minHeap.length - 1)
}

/**
 * 从节点 i 开始，从底至顶堆化（小顶堆）
 * @param {number} i
 */
KthLargest.prototype.siftUp = function (i) {
  while (true) {
    // 获取节点 i 的父节点
    const p = this.parent(i)
    // 当"越过根节点"或"节点无须修复"时，结束堆化
    if (p < 0 || this.minHeap[i] >= this.minHeap[p]) break
    // 交换两节点
    this.swap(i, p)
    // 循环向上堆化
    i = p
  }
}

/**
 * 元素出堆（小顶堆）
 * @return {number}
 */
KthLargest.prototype.pop = function () {
  // 判空处理
  if (this.minHeap.length === 0) throw new Error('堆为空')
  // 交换根节点与最右叶节点（交换首元素与尾元素）
  this.swap(0, this.minHeap.length - 1)
  // 删除节点
  const val = this.minHeap.pop()
  // 从顶至底堆化
  this.siftDown(0)
  // 返回堆顶元素
  return val
}

/**
 * 从节点 i 开始，从顶至底堆化（小顶堆）
 * @param {number} i
 */
KthLargest.prototype.siftDown = function (i) {
  while (true) {
    // 判断节点 i, l, r 中值最小的节点，记为 mi
    const l = this.left(i)
    const r = this.right(i)
    let mi = i

    if (l < this.minHeap.length && this.minHeap[l] < this.minHeap[mi]) mi = l
    if (r < this.minHeap.length && this.minHeap[r] < this.minHeap[mi]) mi = r

    // 若节点 i 最小或索引 l, r 越界，则无须继续堆化，跳出
    if (mi === i) break
    // 交换两节点
    this.swap(i, mi)
    // 循环向下堆化
    i = mi
  }
}
```

:::

- 提交结果：

::: swiper

![使用方案 1 建堆](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-01-23-53.png)

![使用方案 2 建堆](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-09-26-01-23-27.png)

:::

- 提交时间：
  - 方案 1：提交于 `2025.09.26 00:04`
  - 方案 2：提交于 `2025.09.26 01:22`

## 12. 🔗 引用

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
