# 07 · Spring Cloud（微服务）

> 覆盖：微服务概念、注册中心（Eureka/Nacos）、负载均衡（Ribbon/LoadBalancer/Feign）、熔断限流（Hystrix/Sentinel）、网关（Gateway）、配置中心（Nacos/Apollo）、分布式事务（Seata）、RPC。
>
> 说明：Netflix 组件（Eureka/Ribbon/Hystrix/Zuul）多数已进入维护期，现代技术栈以 **Spring Cloud Alibaba（Nacos/Sentinel/Seata）+ Spring Cloud Gateway + OpenFeign + Spring Cloud LoadBalancer** 为主，下文会标注新旧。

---

## 一、微服务与组件全景

### 1. 什么是微服务，Spring Cloud 的优势 🔥

微服务：把单体应用拆成一组**围绕业务能力、独立部署、独立扩展、技术异构**的小服务，通过轻量通信（HTTP/RPC）协作。

带来的新问题（也正是 Spring Cloud 要解决的）：服务注册发现、负载均衡、远程调用、熔断降级、配置管理、网关路由、链路追踪、分布式事务。

### 2. 微服务核心组件对照 🔥

| 能力 | Netflix（旧） | 现代主流 |
| --- | --- | --- |
| 注册中心 | Eureka | **Nacos** / Consul |
| 配置中心 | Config | **Nacos** / Apollo |
| 负载均衡 | Ribbon | **Spring Cloud LoadBalancer** |
| 声明式调用 | Feign | **OpenFeign** |
| 熔断限流 | Hystrix | **Sentinel** / Resilience4j |
| 网关 | Zuul | **Spring Cloud Gateway** |
| 分布式事务 | — | **Seata** |
| 链路追踪 | Sleuth+Zipkin | Micrometer Tracing |

---

## 二、注册中心

### 3. 注册中心的作用与原理 🔥

服务启动时把自己的地址注册到注册中心；消费者从注册中心拉取服务列表并缓存，结合负载均衡发起调用；通过**心跳**维持健康状态，下线/宕机时摘除实例。

### 4. Eureka 与 Zookeeper 的区别（CAP 取舍）🔥

- **Eureka：AP**。各节点平等、无强一致；网络分区时仍能提供（可能过期的）服务列表，保证**可用性**。
- **Zookeeper：CP**。基于 ZAB，Leader 选举期间不可用，保证**一致性**。
- **注册中心更应偏向 AP**：服务发现宁可拿到稍旧的列表也不要因为选主而整体不可用。Nacos 同时支持 AP（默认，临时实例）和 CP（持久实例，Raft）。

### 5. Eureka 自我保护机制 ⭐

当 Eureka Server 在短时间内丢失大量心跳（疑似网络分区而非服务真挂），会进入自我保护：**不再摘除实例**，避免误删健康节点。代价是可能保留已死实例（靠消费者的容错兜底）。

### 6. Nacos 作为注册中心的原理与模型 ⭐

- **服务领域模型**：`命名空间(Namespace) → 服务(Service) → 集群(Cluster) → 实例(Instance)`。命名空间常用于环境隔离（dev/test/prod）。
- **临时实例（默认，AP，Distro 协议）**：客户端心跳续约，Server 内存存储，宕机即摘除。
- **持久实例（CP，Raft）**：Server 持久化，即使宕机也保留，需主动注销。
- **Distro 协议**：AP 模式下 Nacos 集群每个节点负责一部分实例的写，节点间异步复制，实现最终一致和高可用。

### 7. Nacos 2.x 相比 1.x 的改进 ⭐

1.x 用 HTTP 短连接 + 长轮询，连接数多、性能受限；**2.x 引入 gRPC 长连接**，大幅降低连接数与开销，服务端通过长连接主动推送变更（探活更及时）。

---

## 三、负载均衡与远程调用

### 8. 负载均衡的意义与策略 ⭐

将请求分摊到多个实例，提升吞吐与可用性。常见策略：轮询、随机、加权、最少连接、一致性哈希、区域优先。**客户端负载均衡**（Ribbon/LoadBalancer，调用方持有列表自己选）vs **服务端负载均衡**（Nginx/LVS）。

### 9. Feign / OpenFeign 与 Ribbon 的关系 🔥

- **Feign/OpenFeign**：声明式 HTTP 客户端，写个接口加注解就能像调本地方法一样调远程服务，底层做了动态代理 + 编解码。
- **负载均衡**由 Ribbon（旧）或 Spring Cloud LoadBalancer（新）提供，Feign 集成它在多个实例间选址。
- 一句话：**Feign 负责“怎么发请求”，LoadBalancer 负责“发给哪个实例”**。

### 10. 为什么 Feign 第一次调用慢 ⭐

首次调用需要**懒加载**：创建 Feign 客户端、Ribbon/LoadBalancer 拉取并初始化服务列表、建立连接。可通过 `ribbon.eager-load` 饥饿加载或预热解决。

### 11. Feign 的性能优化 ⭐

- 开启连接池（默认 `URLConnection`，换 **OkHttp/Apache HttpClient**）。
- 开启 GZIP 压缩。
- 合理设置 `connectTimeout`/`readTimeout`。
- 日志级别生产用 `BASIC` 或 `NONE`（`FULL` 很耗性能）。

---

## 四、熔断、降级、限流

### 12. 服务熔断、降级、限流的区别 🔥

- **熔断**：依赖的服务故障率/响应过高时，**断路器打开**，快速失败，不再发起调用，过段时间半开试探恢复（类比保险丝）。
- **降级**：服务不可用或为了保核心，返回**兜底逻辑/默认值**（fallback），主动舍弃非核心功能。
- **限流**：限制单位时间请求量，保护系统不被压垮。

三者关系：限流是入口防护，熔断是对下游故障的自我保护，降级是熔断/限流触发后的用户体验兜底。

### 13. Hystrix 断路器三态 ⭐

`Closed（正常）→ Open（熔断，快速失败）→ Half-Open（半开，放少量请求试探）→ Closed/Open`。Hystrix 用**线程池隔离/信号量隔离**做资源隔离。Hystrix 已停更，新项目用 **Sentinel / Resilience4j**。

### 14. Sentinel 的限流算法与熔断 🔥

- **限流算法**：
  - 计数器（固定窗口）：有临界突刺问题。
  - **滑动窗口**：Sentinel 默认，更平滑。
  - **漏桶**：恒定速率流出，平滑突发（对应“匀速排队”）。
  - **令牌桶**：允许一定突发（对应“预热/Warm Up”）。
- Sentinel 的熔断策略：慢调用比例、异常比例、异常数。相比 Hystrix，Sentinel 提供**实时监控、动态规则、热点参数限流、系统自适应保护**，更适合生产。

---

## 五、网关

### 15. Spring Cloud Gateway 的核心概念 🔥

- **Route（路由）**：网关的基本单元，由 ID、目标 URI、断言、过滤器组成。
- **Predicate（断言）**：匹配条件（Path、Method、Header、时间等）。
- **Filter（过滤器）**：请求/响应的处理逻辑（鉴权、限流、改写、日志）。
- 基于 **WebFlux / Netty 响应式**，性能优于 Zuul 1.x（阻塞）。常用于统一鉴权、限流、灰度、协议转换。

### 16. Zuul 的过滤器类型 ⭐

`pre`（路由前）、`routing`（路由）、`post`（路由后）、`error`（异常）。Zuul 1.x 阻塞，已被 Gateway 取代。

### 17. 网关如何做平滑迁移 / 灰度 ⭐

在网关层按权重/标签（如 header、用户 ID）把流量分配到新旧版本实例（配合注册中心元数据 + 负载均衡的灰度路由），逐步放量，实现平滑迁移。

---

## 六、配置中心

### 18. 配置中心的作用与选型 ⭐

集中管理、动态刷新配置，避免改配置重新打包发布。选型：**Nacos**（注册+配置一体、轻量）、**Apollo**（携程，权限/灰度/审计完善，治理能力强）、Spring Cloud Config（基于 Git，较原始）。

### 19. Nacos 配置中心的动态刷新与长轮询 ⭐

- 1.x：客户端**长轮询**（发请求 hold 住，配置变更或超时 29.5s 返回），兼顾实时性与连接数。
- 2.x：基于 gRPC 长连接，服务端主动推送变更。
- 客户端用 `@RefreshScope` 或 `@ConfigurationProperties` 实现 Bean 配置热更新。

### 20. Apollo 的架构与实时推送 ⭐

Apollo 分 Config Service、Admin Service、Portal、Client。配置发布后通过 **HTTP 长轮询（NotificationController hold 请求）** 通知客户端拉取，客户端本地有缓存文件兜底，Config Service 挂了也能用旧配置，**可靠性高**。

---

## 七、分布式事务（Seata）

### 21. Seata 支持的事务模式 🔥

- **AT 模式（默认，无侵入）**：基于本地 ACID + 全局锁，自动生成 undo log，一阶段提交本地事务并记录回滚日志，二阶段全局提交则删日志、全局回滚则用 undo log 补偿。**业务无感知**，适合大多数场景。
- **TCC 模式**：Try-Confirm-Cancel，业务自己实现三个方法，性能好但侵入强。
- **SAGA 模式**：长事务，每步有正向和补偿操作，适合流程长、参与方多。
- **XA 模式**：基于数据库 XA 协议，强一致但性能差。

### 22. 2PC（两阶段提交）流程与缺点 🔥

- **阶段一（准备）**：协调者问所有参与者能否提交，参与者执行并锁资源，回复 yes/no。
- **阶段二（提交/回滚）**：全 yes 则通知提交，否则回滚。
- 缺点：① **同步阻塞**，资源全程锁定；② **协调者单点**；③ 阶段二通知丢失会导致**数据不一致**。TCC、本地消息表等是其改进。详见 [15-分布式](./15-分布式.md)。

### 23. Seata 的 XID 如何通过 Feign 全局传递 ⭐

Seata 用 `RootContext` 保存全局事务 ID（XID）。跨服务调用时，通过 **Feign 拦截器把 XID 放进请求头**，被调方拦截器取出并 `bind` 到 `RootContext`，从而把分支事务纳入同一全局事务。

---

## 八、其他

### 24. RPC 的实现原理 ⭐

RPC（远程过程调用）让调用远程方法像本地一样：**动态代理**生成客户端 stub → **序列化**参数 → **网络传输**（Netty）→ 服务端反序列化、反射调用 → 返回结果。详见 [12-Dubbo](./12-Dubbo.md)。

### 25. CAP 与 BASE ⭐

- **CAP**：分布式系统在**一致性 C、可用性 A、分区容错 P**中只能同时满足两个；网络分区 P 必须容忍，所以实际是 **CP 或 AP 的取舍**。
- **BASE**：基本可用（Basically Available）、软状态（Soft state）、最终一致（Eventually consistent），是对 CAP 中 AP 的工程实践延伸。详见 [15-分布式](./15-分布式.md)。

---

## 高频追问清单

- 注册中心选 AP 还是 CP？为什么？→ 偏 AP（本文 4）。
- Feign 和 Ribbon 各负责什么？→ 调用 vs 选址（本文 9）。
- 熔断、降级、限流三者区别？→ 本文 12。
- Seata AT 模式怎么做到无侵入回滚？→ undo log + 全局锁（本文 21）。
