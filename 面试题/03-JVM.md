# 03 · JVM

> 覆盖：运行时数据区、类加载机制与双亲委派、对象创建与内存布局、对象可达性与 GC 算法、垃圾收集器（含 G1/ZGC）、调优参数与工具、逃逸分析、元空间。

---

## 一、运行时数据区

### 1. JVM 内存结构（运行时数据区）🔥

线程私有：

- **程序计数器（PC）**：当前线程执行字节码的行号指示器，唯一不会 OOM 的区域。
- **虚拟机栈（VM Stack）**：每个方法对应一个**栈帧**（局部变量表、操作数栈、动态链接、返回地址）。栈深超限 → `StackOverflowError`；无法扩展 → OOM。
- **本地方法栈**：为 native 方法服务。

线程共享：

- **堆（Heap）**：对象实例和数组的分配区，GC 主战场，分新生代（Eden + 2 Survivor）和老年代。
- **方法区（Method Area）**：类元信息、运行时常量池、静态变量。JDK 8 起由**元空间（Metaspace，本地内存）** 实现，取代永久代。

> JDK 8 把字符串常量池、静态变量挪到了**堆**，类元数据挪到了**元空间**。

### 2. 堆和栈的区别 🔥

| 维度 | 堆 | 栈 |
| --- | --- | --- |
| 存储 | 对象实例 | 局部变量、方法调用栈帧 |
| 共享 | 线程共享 | 线程私有 |
| 生命周期 | GC 管理 | 随方法调用/返回自动分配释放 |
| 错误 | OOM | `StackOverflowError` / OOM |
| 速度 | 较慢 | 很快（指针移动） |

---

## 二、类加载机制

### 3. 类的生命周期与加载过程 🔥

`加载 → 验证 → 准备 → 解析 → 初始化 → 使用 → 卸载`，其中**连接 = 验证 + 准备 + 解析**。

- **加载**：读取 `.class` 字节流，生成 `Class` 对象。
- **验证**：校验字节码合法性、安全性。
- **准备**：为**静态变量**分配内存并设**零值**（`static int a = 1` 此阶段为 0，赋 1 在初始化阶段；但 `static final` 常量在此阶段直接赋值）。
- **解析**：符号引用 → 直接引用。
- **初始化**：执行 `<clinit>`（静态变量赋值 + 静态代码块），**线程安全**（JVM 保证）。

**类初始化触发时机**：`new`/读写静态字段/调用静态方法、反射、初始化子类先初始化父类、main 类、`MethodHandle` 等。访问 `static final` 常量、`ClassName.class`、数组定义不会触发初始化。

### 4. 双亲委派模型 🔥

类加载器层次：`Bootstrap（C++，加载 JDK 核心）` → `Extension/Platform` → `Application（classpath）` → 自定义。

**双亲委派**：收到加载请求时，先**委派给父加载器**，父加载不了才自己加载。

- 好处：避免核心类被篡改（如你写个 `java.lang.String` 也加载不了），保证类型唯一性与安全。
- **打破双亲委派**的场景：
  - **JDBC/SPI**：`DriverManager`（核心类）要加载第三方驱动 → 用**线程上下文类加载器（TCCL）**反向委派。
  - **Tomcat**：每个 Web 应用独立 `WebappClassLoader`，**优先加载自己的类**（隔离 + 同一容器多应用同库不同版本）。
  - **OSGi / 热部署 / 模块化**。
- 自定义类加载器：重写 `findClass`（推荐）；若要打破委派则重写 `loadClass`。

### 5. Tomcat 的类加载机制 ⭐

Tomcat 违背双亲委派以实现**应用隔离**：`Common → Catalina/Shared → WebappClassLoader`。每个 webapp 的 `WebappClassLoader` 加载 `WEB-INF/classes` 和 `WEB-INF/lib` 时**先自己找**（除 JVM 核心类外），实现不同应用类隔离、同名类多版本共存。

---

## 三、对象的创建与布局

### 6. 对象的创建过程 🔥

1. **类加载检查**：是否已加载，未加载则先加载。
2. **分配内存**：
   - **指针碰撞**（内存规整，配合 Serial/G1+压缩）：移动指针划出一块。
   - **空闲列表**（内存零散，配合 CMS）：从空闲表里找。
   - 并发安全：**CAS+重试** 或 **TLAB（线程本地分配缓冲）**，每个线程在 Eden 预分配私有区域，无锁分配。
3. **零值初始化**（字段默认值）。
4. **设置对象头**（类型指针、GC 信息等）。
5. **执行 `<init>`**（构造方法）。

### 7. 对象的内存布局 / 对象头 ⭐

对象 = **对象头（Header）+ 实例数据 + 对齐填充（8 字节对齐）**。

对象头包含：

- **Mark Word**：哈希码、GC 分代年龄、**锁状态标志**（偏向锁/轻量级锁/重量级锁，与 `synchronized` 升级相关，见 [并发编程](./04-并发编程.md)）。
- **类型指针（Klass Pointer）**：指向方法区的类元数据（开启指针压缩 `-XX:+UseCompressedOops` 时为 4 字节）。
- 数组还多一个**长度字段**。

### 8. 对象分配规则 ⭐

- 优先在 **Eden** 分配。
- 大对象（超过 `-XX:PretenureSizeThreshold`）直接进**老年代**。
- 长期存活对象进老年代：每熬过一次 Minor GC 年龄 +1，达到 `-XX:MaxTenuringThreshold`（默认 15）晋升。
- **动态年龄判定**：Survivor 中同年龄对象总和超过 Survivor 一半，年龄 ≥ 该年龄者直接晋升。
- **空间分配担保**：Minor GC 前检查老年代是否容得下，不足可能触发 Full GC。

---

## 四、垃圾回收

### 9. 如何判断对象可被回收 🔥

- **引用计数法**：无法解决**循环引用**，JVM 不采用。
- **可达性分析**：从 **GC Roots** 出发，不可达的对象可回收。
- **GC Roots 包括**：虚拟机栈中引用的对象、静态变量引用的对象、常量引用、native 方法引用的对象、被同步锁持有的对象等。
- 对象被回收前还会经历 `finalize()`（只调一次，已不推荐依赖）。

### 10. 垃圾收集算法 🔥

- **标记-清除（Mark-Sweep）**：标记后清除，**产生碎片**。
- **标记-复制（Copying）**：分两块，存活对象复制到另一块，无碎片但浪费空间，适合**新生代**（朝生夕灭）。
- **标记-整理（Mark-Compact）**：标记后让存活对象向一端移动，适合**老年代**，无碎片但移动成本高。
- **分代收集**：新生代用复制，老年代用标记-整理/清除。

### 11. Minor GC、Major GC、Full GC、什么时候触发 Full GC 🔥

- **Minor GC（Young GC）**：回收新生代，频繁、快。
- **Major GC / Old GC**：回收老年代。
- **Full GC**：回收整个堆 + 方法区，最慢，应尽量避免。

触发 Full GC 的常见原因：

- 老年代空间不足（对象晋升、大对象）。
- 空间分配担保失败。
- 元空间（Metaspace）不足。
- 显式调用 `System.gc()`（建议 `-XX:+DisableExplicitGC` 禁用）。
- CMS 的 promotion failed / concurrent mode failure。

### 12. 常见垃圾收集器 🔥

| 收集器 | 区域 | 算法 | 特点 |
| --- | --- | --- | --- |
| Serial / Serial Old | 新/老 | 复制 / 标整 | 单线程，STW，client 模式 |
| ParNew | 新 | 复制 | Serial 的多线程版，配 CMS |
| Parallel Scavenge / Old | 新/老 | 复制 / 标整 | **吞吐量优先**，JDK 8 默认 |
| **CMS** | 老 | 标记-清除 | **低延迟**，并发收集，有碎片，已在 JDK 14 移除 |
| **G1** | 整堆 | 复制+标整 | **JDK 9+ 默认**，分 Region，可预测停顿 |
| **ZGC** 🆕 | 整堆 | 染色指针+读屏障 | 停顿 < 1ms，超大堆，JDK 15 生产可用，21 分代 |
| Shenandoah 🆕 | 整堆 | 并发压缩 | 低延迟，OpenJDK |

### 13. CMS 工作过程与缺点 ⭐

四步：**初始标记（STW）→ 并发标记 → 重新标记（STW）→ 并发清除**。
缺点：① 标记-清除产生**碎片**；② 并发阶段占用 CPU，吞吐下降；③ 浮动垃圾、concurrent mode failure 退化为 Serial Old 全程 STW。**已被 G1/ZGC 取代**。

### 14. G1 的核心思想 🔥

- 把堆分成大量大小相等的 **Region**（Eden/Survivor/Old/Humongous 角色动态变化）。
- 维护每个 Region 的回收价值，**优先回收垃圾最多的 Region**（Garbage First），用 `-XX:MaxGCPauseMillis` 设定目标停顿。
- 回收过程：初始标记 → 并发标记 → 最终标记 → 筛选回收（复制存活对象，整理无碎片）。
- 跨 Region 引用用 **RSet（Remembered Set）** + 写屏障维护，避免全堆扫描。

### 15. 如何选择垃圾收集器 ⭐

- 内存小、单核、客户端 → Serial。
- 关注**吞吐量**（批处理、后台计算）→ Parallel。
- 关注**低延迟**、堆中等（4–32G）→ G1（默认）。
- 超大堆（几十 G~TB）、极致低延迟 → **ZGC / Shenandoah**。

### 16. STW、安全点（Safepoint）、OopMap 是什么 ⭐

- **Stop The World**：GC 某些阶段必须暂停所有用户线程，保证引用关系不变。
- **安全点（Safepoint）**：线程只能在特定位置（方法调用、循环回边、异常跳转等）停下来 GC，保证此处能准确枚举 GC Roots。
- **OopMap**：记录栈上哪些位置是对象引用，使 GC 能**快速、准确**地枚举根，而不必全栈扫描。
- **安全区域（Safe Region）**：处理 sleep/blocked 线程跑不到安全点的问题。

---

## 五、调优与新特性

### 17. 常用 JVM 参数 🔥

```bash
-Xms2g -Xmx2g            # 堆初始/最大（生产建议相等，避免动态扩容抖动）
-Xmn1g                   # 新生代大小
-Xss512k                 # 单线程栈大小
-XX:MetaspaceSize=256m -XX:MaxMetaspaceSize=256m
-XX:SurvivorRatio=8      # Eden:Survivor = 8:1:1
-XX:MaxTenuringThreshold=15
-XX:+UseG1GC             # 选择 G1
-XX:MaxGCPauseMillis=200 # G1 目标停顿
-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/path/dump.hprof
-XX:+PrintGCDetails -Xlog:gc*  # GC 日志（JDK 9+ 统一日志）
```

### 18. 调优命令与工具 🔥

- 命令行：`jps`（进程）、`jstat -gcutil <pid> 1000`（GC 统计）、`jstack`（线程栈，排查死锁/CPU 飙高）、`jmap -dump`（堆转储）、`jinfo`（参数）。
- 图形/进阶：**JConsole、VisualVM、Arthas（强烈推荐线上诊断）、MAT（分析 dump）、JProfiler、async-profiler（火焰图）**。

> 排查 CPU 100%：`top` 找进程 → `top -Hp <pid>` 找线程 → 线程号转 16 进制 → `jstack <pid> | grep <hex>` 定位代码。

### 19. 逃逸分析 ⭐

逃逸分析判断对象的作用域是否“逃出”方法/线程，从而做优化：

- **栈上分配**：未逃逸的对象可分配在栈上，随方法结束自动回收，减轻 GC（HotSpot 实为标量替换实现）。
- **标量替换**：把对象拆成基本类型变量，连对象都不创建。
- **锁消除**：未逃逸对象上的同步可被消除（如局部 `StringBuffer`）。

由 `-XX:+DoEscapeAnalysis`（默认开启）控制。注意：**并非所有对象都分配在堆上**——这是常见的考点反直觉。

### 20. 为什么用元空间取代永久代 ⭐

- 永久代大小受 `-XX:MaxPermSize` 限制，**易 OOM**且难调；元空间使用**本地内存**，上限是机器内存（仍可用 `-XX:MaxMetaspaceSize` 限制）。
- 字符串常量池在永久代易 OOM，移到堆更合理。
- 便于 HotSpot 与 JRockit 融合，类元数据管理更灵活。

---

## 高频追问清单

- 一次 Full GC 频繁，怎么排查？→ GC 日志 + jstat 看晋升速率，dump 看老年代大对象/泄漏。
- G1 和 CMS 的本质区别？→ Region 化、可预测停顿、整理无碎片（本文 13/14）。
- 对象一定在堆上吗？→ 不一定，逃逸分析 + 标量替换（本文 19）。
- 类加载器能不能加载两个同名类？→ 能，不同类加载器加载的同名类是不同类型。
