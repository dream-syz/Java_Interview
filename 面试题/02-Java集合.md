# 02 · Java 集合

> 覆盖：集合体系、`ArrayList`/`LinkedList`、`HashMap`（含 JDK 8 红黑树化、扩容）、`HashMap` 与 `HashTable`/`ConcurrentHashMap`、`fail-fast`、红黑树等。这是面试**最高频**的模块。

---

## 一、集合体系总览

### 1. List、Set、Map 三者的区别 🔥

- **List**：有序（插入顺序）、可重复、有索引。
- **Set**：不重复（靠 `equals`+`hashCode` 或比较器去重），`HashSet` 无序、`LinkedHashSet` 保插入序、`TreeSet` 有序。
- **Map**：键值对，key 不重复。

继承关系：`Collection` 是 `List`/`Set`/`Queue` 的父接口；`Map` 独立于 `Collection`。

> `Collection`（接口，单个元素集合）与 `Collections`（工具类，提供排序、同步包装、不可变包装等静态方法）不要混淆。

### 2. 常见实现的选型

| 需求 | 推荐 |
| --- | --- |
| 随机访问多、尾部增删 | `ArrayList` |
| 频繁头部/中间增删、做队列/栈 | `ArrayDeque`（优于 `LinkedList`） |
| 键值映射、无序 | `HashMap` |
| 保持插入/访问顺序 | `LinkedHashMap`（可做 LRU） |
| 按 key 排序 | `TreeMap` |
| 并发场景 | `ConcurrentHashMap`、`CopyOnWriteArrayList` |

---

## 二、List

### 3. `ArrayList` 与 `LinkedList` 的区别 🔥

| 维度 | `ArrayList` | `LinkedList` |
| --- | --- | --- |
| 底层 | 动态数组 `Object[]` | 双向链表 |
| 随机访问 `get(i)` | O(1) | O(n) |
| 头部/中间增删 | O(n)（要搬移） | O(1)（找到位置后），但定位是 O(n) |
| 内存 | 连续，缓存友好 | 每节点额外两个指针 |
| 实际使用 | **绝大多数场景首选** | 很少用，队列/栈用 `ArrayDeque` 更好 |

> 实战结论：`LinkedList` 的“增删快”在实践中常被定位成本和缓存不友好抵消，**默认用 `ArrayList`**。需要双端队列用 `ArrayDeque`。

### 4. `ArrayList` 的扩容机制 🔥

- 默认初始容量 **10**（首次 `add` 时才真正分配，JDK 8 起懒初始化）。
- 扩容为原来的 **1.5 倍**（`oldCap + (oldCap >> 1)`），通过 `Arrays.copyOf` 拷贝到新数组。
- 已知大小时**预设容量**（`new ArrayList<>(expectedSize)`）可避免多次扩容拷贝。

### 5. `fail-fast` 与 `fail-safe` ⭐

- **fail-fast（快速失败）**：`ArrayList`/`HashMap` 等在迭代时若被结构性修改（非迭代器自身的 `remove`），`modCount != expectedModCount`，抛 `ConcurrentModificationException`。这是一种**错误检测**，不保证一定触发。
- 正确删除：用迭代器的 `it.remove()`，或 JDK 8 的 `removeIf`。
- **fail-safe**：`CopyOnWriteArrayList`、`ConcurrentHashMap` 迭代基于副本/快照，不抛异常但可能读到旧数据。

---

## 三、HashMap（重中之重）

### 6. `HashMap` 底层数据结构 🔥

- **JDK 7**：数组 + 链表（头插法，扩容时可能成环导致死循环）。
- **JDK 8+**：数组 + 链表 + **红黑树**，采用**尾插法**。
  - 当**链表长度 ≥ 8** 且**数组容量 ≥ 64** 时，链表转红黑树；否则优先扩容。
  - 当树节点 ≤ 6 时退化回链表（留出滞回区间避免频繁转换）。

### 7. put 流程 🔥

1. 计算 key 的 hash：`(h = key.hashCode()) ^ (h >>> 16)`（**扰动函数**，让高位参与运算，减少冲突）。
2. 定位桶：`index = (n - 1) & hash`（n 为容量，是 2 的幂，等价取模）。
3. 桶为空 → 直接放；否则遍历链表/树：key 相等则覆盖，否则追加（链表尾插，达到阈值树化）。
4. `++size > threshold` 则扩容。

### 8. 为什么容量是 2 的 N 次方 🔥

- `(n-1) & hash` 仅在 n 为 2 的幂时**等价于 `hash % n`**，且位运算更快。
- 扩容时元素要么留在原位置 `index`，要么移到 `index + oldCap`，通过 `(hash & oldCap) == 0` 判断，**无需重新计算 hash**，迁移高效。
- 你传入非 2 的幂的初始容量，会被 `tableSizeFor` 向上取整到最近的 2 的幂。

### 9. 为什么扩容阈值是 0.75（负载因子）⭐

空间与时间的折中：太小浪费空间、太大冲突增多。0.75 在泊松分布下使单桶冲突概率较低（链表长度达到 8 的概率约千万分之六）。`threshold = capacity * loadFactor`。

### 10. `HashMap` 为什么线程不安全 🔥

- JDK 7 并发扩容**头插法**可能形成环形链表，`get` 时死循环（CPU 100%）。
- JDK 8 改尾插法解决了成环，但仍有**数据覆盖丢失**（两个线程同时 put 到同一空桶）、**size 不准**等问题。
- 并发场景用 `ConcurrentHashMap`。

### 11. `HashMap` 的 key 能用任意类吗 ⭐

可以，但作为 key 的类**必须正确重写 `hashCode` 和 `equals`**，且最好**不可变**（如 `String`、`Integer`）。若 key 可变且 hash 随之变化，put 后再修改会导致 get 不到。

### 12. `HashMap` 与 `HashTable` 的区别 🔥

| 维度 | `HashMap` | `HashTable` |
| --- | --- | --- |
| 线程安全 | 否 | 是（方法 `synchronized`，全表锁，性能差） |
| null 键值 | 允许一个 null key、多个 null value | 都不允许 |
| 初始容量/扩容 | 16 / ×2 | 11 / ×2+1 |
| 迭代 | fail-fast | 同样 fail-fast（`Enumeration` 不） |
| 地位 | 常用 | **已过时**，并发用 `ConcurrentHashMap` |

---

## 四、ConcurrentHashMap

### 13. `ConcurrentHashMap` 的实现 🔥

- **JDK 7**：**分段锁（Segment）**，每段一把 `ReentrantLock`，并发度 = 段数（默认 16）。
- **JDK 8+**：放弃分段锁，采用 **数组 + 链表/红黑树 + `synchronized`（锁单个桶头节点）+ CAS**：
  - 桶为空时用 **CAS** 写入，无锁；
  - 桶非空时对**头节点 `synchronized`**，锁粒度更细（锁单个桶），并发度更高。
  - `size` 用 `baseCount` + `CounterCell[]`（类似 `LongAdder`）分段统计，减少竞争。
  - 扩容支持**多线程协助迁移**（`transfer`，`ForwardingNode` 标记已迁移桶）。
- **不允许 null 键值**：并发下 `null` 无法区分“不存在”与“值为 null”（无法用 `containsKey` 二次确认）。

### 14. `SynchronizedMap` 与 `ConcurrentHashMap` 区别 ⭐

`Collections.synchronizedMap` 是对整个 map 加同一把锁（包装），并发度低；`ConcurrentHashMap` 锁粒度到桶，吞吐高得多。

---

## 五、其他

### 15. 红黑树的特征 ⭐

红黑树是**自平衡二叉查找树**，保证最长路径不超过最短路径的 2 倍，增删查均摊 O(log n)。五条性质：

1. 节点非红即黑；
2. 根节点是黑色；
3. 叶子（NIL）节点是黑色；
4. 红节点的子节点必为黑（不能有连续红节点）；
5. 任一节点到其所有叶子的路径包含**相同数目的黑节点**（黑高相同）。

> 对比 AVL：AVL 严格平衡查询更快，但增删旋转更频繁；红黑树是“近似平衡”，增删性能更均衡，故 `HashMap`/`TreeMap`/Linux 调度器都用红黑树。

### 16. Java 的四种引用：强、软、弱、虚 ⭐

| 引用 | 回收时机 | 典型用途 |
| --- | --- | --- |
| 强引用 `Object o = new ..` | 永不回收（可达即不回收） | 普通对象 |
| 软引用 `SoftReference` | **内存不足**时回收 | 内存敏感缓存 |
| 弱引用 `WeakReference` | **下次 GC** 即回收 | `ThreadLocalMap` 的 key、`WeakHashMap` |
| 虚引用 `PhantomReference` | 任何时候，仅用于回收通知 | 管理堆外内存（`Cleaner`） |

软/弱/虚引用都可配合 `ReferenceQueue` 在对象被回收时收到通知。

---

## 高频追问清单

- `HashMap` JDK 7 → 8 改了什么？为什么？→ 本文第 6/10 题
- `ConcurrentHashMap` JDK 8 为什么放弃分段锁？→ 本文第 13 题
- 为什么 `ConcurrentHashMap` 不允许 null？→ 本文第 13 题
- 怎么用 `LinkedHashMap` 实现 LRU？→ 重写 `removeEldestEntry`，`accessOrder=true`
