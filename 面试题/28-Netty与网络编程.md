# 28 · Netty 与网络编程

> 覆盖：BIO/NIO 回顾、Reactor 线程模型、Netty 核心组件与架构、零拷贝、粘包拆包、心跳、为什么用 Netty。RPC、网关、IM、消息中间件的底层都离不开它。

---

## 一、IO 模型与 Reactor

### 1. Reactor 线程模型 🔥（核心）

Reactor 模式基于 **事件驱动 + IO 多路复用**，三种演进：

- **单 Reactor 单线程**：一个线程负责 accept + read + 业务 + write。简单，但业务阻塞会拖垮整个服务（Redis 早期类似）。
- **单 Reactor 多线程**：Reactor 单线程负责 IO 事件分发，业务交给**线程池**。瓶颈在单 Reactor 处理所有连接。
- **主从 Reactor 多线程（Netty 默认）**：
  - **MainReactor（bossGroup）**：只负责 `accept` 新连接。
  - **SubReactor（workerGroup）**：多个线程，负责已建立连接的读写 IO。
  - 业务逻辑再交给业务线程池。
  - 充分利用多核，是高性能网络框架的标准架构。

### 2. Netty 与 BIO/原生 NIO 的关系 ⭐

Netty 是对 Java NIO 的封装与增强。原生 NIO 难用（API 复杂、有 epoll 空轮询 bug、需自己处理粘包/断连/线程模型），Netty 解决了这些，提供了**易用、高性能、稳定**的网络编程框架。

---

## 二、Netty 核心组件

### 3. 核心组件 🔥

| 组件 | 作用 |
| --- | --- |
| **EventLoopGroup / EventLoop** | 线程组 / 单线程，每个 EventLoop 绑定一个 Selector，处理多个 Channel 的事件（一个 Channel 只属于一个 EventLoop，**无线程安全问题**） |
| **Channel** | 网络连接的抽象（读写操作） |
| **ChannelPipeline** | **责任链**，由多个 ChannelHandler 组成 |
| **ChannelHandler** | 处理器（编解码、业务），分 Inbound（入站）和 Outbound（出站） |
| **ByteBuf** | Netty 的字节容器，比 NIO `ByteBuffer` 好用（读写指针分离、池化、自动扩容） |
| **ChannelFuture** | 异步操作结果（Netty 全异步） |

### 4. ChannelPipeline 工作机制 ⭐

入站事件（read）从 head → tail 经过 InboundHandler；出站事件（write）从 tail → head 经过 OutboundHandler。典型链：`解码器 → 业务 Handler → 编码器`。这是责任链 + 装饰器模式的经典应用。

---

## 三、关键特性

### 5. Netty 的零拷贝 🔥

Netty 的零拷贝是**用户态层面**的优化（区别于 OS 的 `sendfile` 零拷贝）：

- **`CompositeByteBuf`**：把多个 ByteBuf 逻辑合并，不发生内存拷贝。
- **`wrap`**：把字节数组包装成 ByteBuf，共享底层数组。
- **`slice`**：把一个 ByteBuf 切片，共享内存。
- **`FileRegion`**：用 `transferTo` 实现文件传输的 OS 级零拷贝。
- **堆外内存（DirectBuffer）**：避免 socket 读写时 JVM 堆与堆外的额外拷贝。

> OS 级零拷贝（`sendfile`/`mmap`）减少内核态↔用户态拷贝；Netty 级零拷贝减少 JVM 内部的内存复制。

### 6. 粘包与拆包 🔥

TCP 是字节流，没有消息边界，多个包可能粘连或被拆分。解决（Netty 内置解码器）：

- **固定长度**：`FixedLengthFrameDecoder`。
- **分隔符**：`DelimiterBasedFrameDecoder`（如换行）。
- **长度字段**（最常用）：`LengthFieldBasedFrameDecoder`，消息头声明体长度。
- 自定义协议一般是 `魔数 + 版本 + 长度 + 消息类型 + body`。

### 7. 心跳与断线检测 ⭐

用 `IdleStateHandler` 检测读/写空闲，超时则发心跳包或关闭连接，及时清理死连接、保活。

### 8. Netty 高性能的原因 🔥

- 主从 Reactor 多线程模型，充分利用多核。
- IO 多路复用（epoll），少量线程管海量连接。
- 零拷贝 + 池化 ByteBuf，减少内存分配与拷贝。
- 无锁化设计（一个 Channel 绑定一个 EventLoop 线程，串行无锁）。
- 高效的对象池、FastThreadLocal 等细节优化。

---

## 四、进阶与实战（深水区）

### 9. ByteBuf 内存管理与泄漏 🔥（高频实战坑）

- **引用计数**：池化 ByteBuf 用 `refCnt` 管理，`retain()`+1、`release()`-1，归零才回收到池。**忘记 release 会内存泄漏**（堆外内存，`top` 看 RES 涨但堆正常）。
- **谁最后用谁 release**：Pipeline 中 `SimpleChannelInboundHandler` 会自动 release，普通 `ChannelInboundHandlerAdapter` 需手动。
- 排查工具：`-Dio.netty.leakDetection.level=paranoid` 开启泄漏检测定位到分配栈。
- **PooledByteBufAllocator**（默认，基于 jemalloc 思想的 Arena 分配）减少频繁申请/释放堆外内存的开销。

### 10. 为什么不要在 EventLoop 里做耗时操作 🔥

EventLoop 线程**串行处理它负责的所有 Channel**。一旦在 Handler 里阻塞（查库、远程调用、大计算），会**拖垮同一 EventLoop 上的所有连接**。正确做法：耗时业务提交到**独立业务线程池**（`addLast(businessGroup, handler)` 或 handler 内 `executor.submit`），IO 与业务分离。

### 11. 写优化与背压 ⭐

- **高低水位（WriteBufferWaterMark）**：写缓冲超高水位时 `channel.isWritable()` 返回 false，应停止写入，防止**写太快积压导致 OOM**（慢消费者场景的背压）。
- **批量 flush**：`write` 只入队，`flush` 才真正发；高频小包用 `flush` 合并或 `FlushConsolidationHandler`。
- 关闭 **Nagle 算法**（`TCP_NODELAY=true`）降低小包延迟（见 [16-计算机网络](./16-计算机网络.md)）。

### 12. EventLoop 空轮询 bug 与 Netty 的应对 ⭐

JDK NIO epoll 在某些 Linux 内核下会**空轮询**（select 立即返回 0 导致 CPU 100%）。Netty 的对策：记录空轮询次数，超阈值（默认 512）**重建 Selector** 把 Channel 迁移过去，绕过 bug。

---

## 高频追问清单

- Netty 的线程模型？→ 主从 Reactor 多线程，boss accept + worker IO（一）。
- Netty 零拷贝体现在哪？→ CompositeByteBuf/slice/wrap/FileRegion/堆外内存（5）。
- 粘包拆包怎么解决？→ 长度字段解码器为主（6）。
- 为什么一个 Channel 不用加锁？→ 绑定单一 EventLoop 线程串行处理（8）。
- ByteBuf 为什么会内存泄漏，怎么查？→ 引用计数忘 release，paranoid 检测（9）。
- 能在 Handler 里查数据库吗？→ 不能阻塞 EventLoop，提交业务线程池（10）。
- 写太快会怎样，怎么背压？→ 高低水位 + isWritable（11）。
