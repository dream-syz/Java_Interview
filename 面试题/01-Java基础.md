# 01 · Java 基础

> 覆盖：语言特性、面向对象、基本类型与装箱、`==`/`equals`/`hashCode`、`String`、`final`/`static`、异常体系、泛型、反射、IO/NIO、序列化、创建对象的方式，以及 JDK 8 → 17/21 的现代语法。

---

## 一、语言与面向对象

### 1. Java 语言有哪些特点 🔥

一句话：**面向对象、平台无关（一次编写到处运行）、自动内存管理、强类型安全、内置多线程、生态丰富**。

- **平台无关**：源码编译成与平台无关的字节码（`.class`），由各平台的 JVM 解释/即时编译执行。「平台无关」的根本是 **JVM**，而不是语言本身。
- **自动内存管理**：GC 负责回收堆内存，开发者不直接管理内存，降低了内存泄漏/越界风险。
- **强类型 + 编译期检查**：相比 C 更安全，配合泛型在编译期消除大量类型错误。
- **一切皆对象（除 8 种基本类型外）**，支持封装、继承、多态。

> 架构师视角的补充：Java 的真正壁垒是**成熟稳定的 JVM + 海量中间件生态（Spring、Netty、Kafka 客户端等）+ 可观测/可调优**。面试时不要只背“跨平台”，要点出「JVM 作为运行时平台」这一层价值。

### 2. 面向对象（OOP）的三大 / 四大特性 🔥

| 特性 | 含义 | 价值 |
| --- | --- | --- |
| 封装 | 隐藏内部实现，暴露有限接口 | 降低耦合、保护不变性 |
| 继承 | 子类复用父类结构与行为 | 复用，但会增强耦合，慎用 |
| 多态 | 同一引用调用，运行时绑定到实际类型的方法 | 面向接口编程、可扩展 |
| （抽象） | 抽取共性、忽略细节 | 有时被单列为第四大特性 |

**多态的实现**：编译期看引用类型（决定能调用哪些方法），运行期看实际对象类型（决定调用哪个实现），底层通过**虚方法表（vtable）/ 动态分派**完成。

**面向过程 vs 面向对象**：面向过程以「步骤/函数」为中心，性能开销小（嵌入式/单片机常用）；面向对象以「对象」为中心，易维护、易扩展、易复用，适合复杂业务系统。

> 设计上更应强调 **组合优于继承**：继承是强耦合的“白盒复用”，破坏封装；组合是“黑盒复用”，更灵活。Spring、Guava 等大量用组合。

---

## 二、基本类型与装箱

### 3. 八种基本数据类型及封装类 🔥

| 基本类型 | 字节 | 位数 | 默认值 | 封装类 |
| --- | --- | --- | --- | --- |
| `byte` | 1 | 8 | 0 | `Byte` |
| `short` | 2 | 16 | 0 | `Short` |
| `int` | 4 | 32 | 0 | `Integer` |
| `long` | 8 | 64 | 0L | `Long` |
| `float` | 4 | 32 | 0.0f | `Float` |
| `double` | 8 | 64 | 0.0d | `Double` |
| `char` | 2 | 16 | `\u0000` | `Character` |
| `boolean` | 未明确规定 | — | `false` | `Boolean` |

要点：

- `boolean` 在 JVM 规范中**没有规定具体大小**。HotSpot 中单个 `boolean` 用 `int`（4 字节）表示，`boolean[]` 用 `byte`（1 字节/元素）表示。
- 基本类型存值（栈上/对象内联），封装类是对象（堆上，含对象头）。
- 封装类默认值是 `null`，因此能区分“0/false”与“未赋值”，这在 ORM 字段映射时很关键（**实体类字段建议用封装类**，避免把 `null` 误存成 0）。

### 4. 自动装箱与拆箱 / Integer 缓存 🔥

- **装箱**：`int` → `Integer`，编译期替换为 `Integer.valueOf(int)`。
- **拆箱**：`Integer` → `int`，编译期替换为 `intValue()`。

经典题：

```java
Integer a = 100, b = 100;
Integer c = 200, d = 200;
System.out.println(a == b); // true
System.out.println(c == d); // false
```

原因：`Integer.valueOf` 对 **[-128, 127]** 做了缓存（`IntegerCache`），范围内返回同一对象，故 `==` 为 `true`；超出范围 `new` 新对象，`==` 比较引用为 `false`。

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

延伸坑：

- 缓存上界可通过 `-XX:AutoBoxCacheMax=<size>` 调整（仅上界）。
- 拆箱可能抛 `NullPointerException`：`Integer x = null; int y = x;` 在 `x.intValue()` 处 NPE。
- 三元表达式会触发统一类型导致意外拆箱：`Object o = true ? Integer.valueOf(1) : Double.valueOf(2.0);` 结果是 `1.0`。
- **比较封装类的值一律用 `equals` 或先拆箱**，不要用 `==`。

### 5. `3 * 0.1 == 0.3` 返回什么 ⭐

返回 `false`。`float`/`double` 是 **IEEE 754** 浮点，0.1、0.3 无法精确表示，`3 * 0.1 = 0.30000000000000004`。

**金额等精确计算必须用 `BigDecimal`，并且用字符串构造**：

```java
new BigDecimal("0.1").multiply(new BigDecimal("3")); // 0.3
new BigDecimal(0.1); // 错误：0.1000000000000000055511151231257827021181583404541015625
```

### 6. `a = a + b` 与 `a += b` 的区别 ⭐

`+=` 是复合赋值运算符，**隐含强制类型转换**，而 `a = a + b` 不会。

```java
byte a = 10, b = 20;
// a = a + b; // 编译报错：a+b 提升为 int，不能隐式赋回 byte
a += b;       // 等价 a = (byte)(a + b); 编译通过，但可能溢出
```

---

## 三、`==`、`equals`、`hashCode`

### 7. `==` 与 `equals` 的区别 🔥

- `==`：基本类型比较**值**；引用类型比较**地址（是否同一对象）**。
- `equals`：`Object` 默认实现就是 `==`；多数类（`String`、封装类等）重写为**内容比较**。

### 8. `hashCode` 的作用，与 `equals` 的契约 🔥

`hashCode` 返回对象散列值，用于哈希表（`HashMap`/`HashSet`）快速定位桶。契约：

1. `equals` 为 `true` 的两个对象，`hashCode` **必须相等**；
2. `hashCode` 相等的对象，`equals` **不一定**相等（哈希冲突允许）；
3. **重写 `equals` 必须同时重写 `hashCode`**，否则会破坏 `HashMap` 等容器的语义（put 进去 get 不到）。

> 两个不相等的对象**可能**有相同 `hashCode`（冲突），这是哈希的本质，靠链表/红黑树 + `equals` 解决。

---

## 四、String 家族

### 9. `String`、`StringBuilder`、`StringBuffer` 的区别 🔥

| 类 | 可变性 | 线程安全 | 场景 |
| --- | --- | --- | --- |
| `String` | 不可变（`final char[]`/JDK9 起 `byte[]`） | 安全（不可变天然安全） | 常量、少量拼接 |
| `StringBuilder` | 可变 | **不安全** | 单线程大量拼接（首选） |
| `StringBuffer` | 可变 | 安全（方法 `synchronized`） | 多线程共享拼接（少见） |

- `String` 不可变的好处：可安全共享、可做常量池缓存、可作为 `HashMap` 的 key（`hashCode` 缓存且稳定）、线程安全。
- 循环里用 `+` 拼接字符串，编译器在每次循环都 `new StringBuilder`（JDK 9 后为 `invokedynamic` + `StringConcatFactory`），**循环内务必显式用 `StringBuilder`**。
- JDK 9 起 `String` 内部由 `char[]` 改为 `byte[] + coder`（Latin1/UTF16），节省内存（Compact Strings）。

### 10. 字符串常量池 ⭐

```java
String s1 = "abc";            // 常量池
String s2 = "abc";            // 复用常量池，s1 == s2 为 true
String s3 = new String("abc"); // 堆上新对象，s1 == s3 为 false
String s4 = s3.intern();      // 返回常量池引用，s1 == s4 为 true
```

JDK 7 起字符串常量池移到**堆**中（此前在永久代）。

---

## 五、final、static、关键字

### 11. `final` 的用法 🔥

- 修饰**变量**：基本类型值不可变；引用类型引用不可变（指向的对象内部仍可变）。
- 修饰**方法**：不可被重写。
- 修饰**类**：不可被继承（如 `String`、`Integer`）。
- 在并发中，`final` 字段有**特殊的内存语义**：构造函数结束后，其它线程能看到正确初始化的 `final` 字段（安全发布的一种手段）。

### 12. `static` 的用法 🔥

- 静态变量：类级别共享，类加载时初始化，存于方法区（元空间）/堆中的类对象。
- 静态方法：不依赖实例，不能访问实例成员、不能用 `this`。
- 静态代码块：类初始化时执行一次。
- 静态内部类：不持有外部类引用（避免内存泄漏，常用于单例/Builder）。
- 静态导入 `import static`。

### 13. `instanceof` 关键字 ⭐

判断对象是否为某类型实例。`null instanceof X` 恒为 `false`。基本类型不能用 `instanceof`。

🆕 JDK 16 起支持**模式匹配**，省去强转：

```java
if (obj instanceof String s && !s.isEmpty()) {
    System.out.println(s.length());
}
```

---

## 六、异常体系

### 14. `Exception` 与 `Error`，异常体系 🔥

```
Throwable
├── Error            // 严重错误，程序无法处理（OOM、StackOverflowError）
└── Exception
    ├── RuntimeException   // 非受检异常（NPE、IndexOutOfBounds、ClassCast...）
    └── 其它（受检异常）    // IOException、SQLException... 必须显式处理或声明
```

- **受检异常（checked）**：编译期强制 `try-catch` 或 `throws`。
- **非受检异常（unchecked）**：`RuntimeException` 及子类，编译期不强制。
- `Error` 通常不应捕获处理。

### 15. `try-catch-finally` 中 `return` 的执行顺序 🔥

- `try`/`catch` 中有 `return`，`finally` **仍会执行**，且在 `return` 真正返回前执行。
- `finally` 中如果有 `return`，会**覆盖** `try` 的返回值（强烈不建议在 finally 里 return）。
- `finally` 中修改基本类型返回值不影响已暂存的返回值（值已复制）；修改返回的对象内部状态会生效。
- 唯一不执行 `finally` 的情况：`System.exit()`、JVM 崩溃、线程被杀死。

> 工程实践：用 **try-with-resources**（JDK 7+）替代手写 finally 关流，自动调用 `close()`，且能正确处理异常抑制（suppressed）。

### 16. OOM 与 StackOverflowError 你遇到过哪些 ⭐

- **OOM（OutOfMemoryError）**：
  - `Java heap space`：堆放不下（内存泄漏 / 大对象 / 集合无限增长）。
  - `GC overhead limit exceeded`：GC 占用过多但回收很少。
  - `Metaspace`：类太多（动态生成类、热部署泄漏）。
  - `Direct buffer memory`：NIO 堆外内存超限。
  - `unable to create new native thread`：线程数超系统限制。
- **StackOverflowError（SOF）**：递归过深 / 方法调用链过长，栈帧超过 `-Xss`。

排查思路：`-XX:+HeapDumpOnOutOfMemoryError` 导出 dump，用 MAT 分析支配树找泄漏根。

---

## 七、泛型、反射、Object

### 17. 泛型与类型擦除 ⭐

- 泛型提供**编译期类型检查**和**自动类型转换**，提升安全性与可读性。
- Java 泛型是**类型擦除**实现：编译后泛型信息被擦除为原始类型（或上界），运行期 `List<String>` 与 `List<Integer>` 是同一个 `List`。
- 由此带来的限制：不能 `new T()`、不能 `new T[]`、不能 `instanceof T`、静态字段不能用类型参数。
- 通配符：`? extends T`（上界，只读，PECS 中的 Producer）、`? super T`（下界，可写，Consumer）。

### 18. 反射的作用与原理 🔥

反射允许在**运行时**获取类的结构信息（字段、方法、构造器）并操作对象，是框架（Spring、MyBatis、序列化库）的基石。

获取 `Class` 对象的三种方式：

```java
Class<?> c1 = String.class;          // 类字面量，编译期确定
Class<?> c2 = "x".getClass();        // 对象的 getClass()
Class<?> c3 = Class.forName("java.lang.String"); // 全限定名，会触发类初始化
```

代价：反射调用比直接调用慢（有安全检查、无法内联），可通过 `setAccessible(true)`、缓存 `Method`、或使用 `MethodHandle`/`LambdaMetafactory` 优化。

> 🆕 JDK 9 模块化后，反射访问 JDK 内部类受 `--add-opens` 限制；高性能场景更推荐 `VarHandle`/`MethodHandle`。

### 19. `Object` 的常用方法 ⭐

`equals`、`hashCode`、`toString`、`getClass`、`clone`、`wait`/`notify`/`notifyAll`、`finalize`（已废弃，JDK 9 起 deprecated）。

---

## 八、拷贝、IO、序列化

### 20. 深拷贝与浅拷贝 🔥

- **浅拷贝**：复制对象本身，引用类型字段仍指向同一对象（`Object.clone()` 默认行为）。
- **深拷贝**：递归复制所有引用对象，副本与原对象完全独立。
- 实现深拷贝：逐层 `clone`、序列化反序列化、或用拷贝构造/工具（注意性能）。

### 21. Java 中的 IO 流；BIO / NIO / AIO 🔥

- 分类：按方向分输入/输出流；按单位分字节流（`InputStream`/`OutputStream`）和字符流（`Reader`/`Writer`）；常用装饰器 `Buffered*`。
- **BIO（同步阻塞）**：一连接一线程，`read` 阻塞，并发高时线程爆炸。
- **NIO（同步非阻塞，JDK 4+）**：基于 **Channel + Buffer + Selector**，单线程可管理多连接（IO 多路复用），Netty 即基于此。
- **AIO（异步非阻塞，JDK 7+）**：基于回调/Future，由 OS 完成 IO 后通知，Linux 下底层仍是 epoll 模拟，应用不广。

| 维度 | BIO | NIO | AIO |
| --- | --- | --- | --- |
| 模型 | 同步阻塞 | 同步非阻塞（多路复用） | 异步非阻塞 |
| 连接:线程 | 1:1 | N:1 | N:0（回调） |
| 适用 | 连接数少 | 高并发、连接多 | 长连接、密集读写 |

### 22. 序列化；`transient` 与 `serialVersionUID` ⭐

- 实现 `Serializable` 即可被 JDK 序列化；`transient` 修饰的字段**不参与**序列化。
- `serialVersionUID` 是版本号，反序列化时校验，不一致抛 `InvalidClassException`，**建议显式声明**。
- `static` 字段不参与序列化（属于类不属于对象）。
- 工程上很少用 JDK 原生序列化（性能差、有安全漏洞），更多用 **JSON（Jackson）/ Protobuf / Hessian / Kryo**。

---

## 九、创建对象的方式 ⭐

1. `new`（最常用，调用构造器）；
2. 反射：`Class.newInstance()`（已废弃）/ `Constructor.newInstance()`；
3. `Object.clone()`（不调用构造器）；
4. 反序列化（不调用构造器）；
5. 工厂方法 / `Unsafe.allocateInstance()`（绕过构造器）。

> 对象创建过程（JVM 层）：类加载检查 → 分配内存（指针碰撞/空闲列表）→ 零值初始化 → 设置对象头 → 执行 `<init>`。详见 [03-JVM](./03-JVM.md)。

---

## 十、现代 Java 语法（JDK 9 → 21）🆕

面试加分项，体现你跟进了版本演进：

| 特性 | 版本 | 说明 |
| --- | --- | --- |
| `var` 局部变量类型推断 | 10 | 仅局部变量，类型仍是静态确定的 |
| 文本块 Text Blocks | 15 | `"""..."""` 多行字符串 |
| `record` 记录类 | 16 | 不可变数据载体，自动生成构造/`equals`/`hashCode`/`toString` |
| 密封类 `sealed` | 17 | 限制可继承的子类，配合模式匹配做穷尽检查 |
| `switch` 表达式与模式匹配 | 14 / 21 | `switch` 返回值、`case` 类型模式 |
| 虚拟线程 Virtual Threads | 21 | 轻量级线程，海量并发 IO 的革命，详见 [并发编程](./04-并发编程.md) |
| ZGC / G1 改进 | 15 / 17+ | 低延迟 GC 进入生产可用，详见 [JVM](./03-JVM.md) |

```java
// record：一行定义不可变数据类
public record Point(int x, int y) {}

// 密封类 + switch 模式匹配（穷尽）
sealed interface Shape permits Circle, Square {}
double area(Shape s) {
    return switch (s) {
        case Circle c -> Math.PI * c.r() * c.r();
        case Square q -> q.side() * q.side();
    };
}
```

---

## 高频追问清单

- `HashMap` 为什么线程不安全、`ConcurrentHashMap` 怎么做的？→ [02-Java集合](./02-Java集合.md)
- 对象在内存中怎么布局、怎么被回收？→ [03-JVM](./03-JVM.md)
- `String` 拼接为什么推荐 `StringBuilder`，常量池在哪？→ 本文第 9/10 题
- 为什么重写 `equals` 一定要重写 `hashCode`？→ 本文第 8 题
