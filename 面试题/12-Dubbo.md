# 12 · Dubbo

> 覆盖：调用流程、整体架构、SPI 机制、协议、负载均衡、容错策略、动态代理、与 Spring Cloud 对比、与 Zookeeper 的关系。

---

## 1. Dubbo 是什么，整体架构 🔥

Dubbo 是高性能的 **Java RPC 框架**，解决服务治理（注册发现、负载均衡、容错、监控）。五个角色：

- **Provider**：服务提供者。
- **Consumer**：服务消费者。
- **Registry**：注册中心（Zookeeper/Nacos）。
- **Monitor**：监控中心（统计调用次数、耗时）。
- **Container**：服务运行容器。

### 调用流程

1. Provider 启动，向 Registry **注册**服务。
2. Consumer 启动，向 Registry **订阅**所需服务。
3. Registry 返回 Provider 地址列表，变更时**推送**给 Consumer。
4. Consumer 基于负载均衡从地址列表选一个 Provider，**直连调用**（不经注册中心）。
5. 异步上报调用数据给 Monitor。

## 2. Dubbo 的分层架构 ⭐

从上到下：Service（业务）→ Config（配置）→ Proxy（服务代理）→ Registry（注册）→ Cluster（集群容错）→ Monitor → Protocol（远程调用）→ Exchange（信息交换）→ Transport（网络传输，Netty/Mina）→ Serialize（序列化）。

## 3. Dubbo SPI 机制 🔥🔥（高频）

- **Java SPI** 缺点：一次性加载所有实现、无法按需、不支持依赖注入。
- **Dubbo SPI** 增强：
  - 配置在 `META-INF/dubbo/` 下，以 **key=实现类** 形式，可**按 key 精确加载**。
  - 支持 **`@Adaptive`（自适应扩展）**：运行时根据 URL 参数动态选择实现。
  - 支持 **`@Activate`（自动激活）**：按条件激活一组扩展（如 Filter 链）。
  - 支持**扩展点的 IOC 和 AOP**（依赖注入、包装类增强）。
- Dubbo 的几乎所有组件（协议、负载均衡、容错、序列化）都通过 SPI 可扩展。

## 4. Dubbo 支持哪些协议 ⭐

`dubbo`（默认，单一长连接 + NIO，适合小数据高并发）、`rmi`、`hessian`、`http`、`grpc`，Dubbo 3 推荐 **Triple（基于 HTTP/2 + gRPC 兼容，跨语言、云原生）**。

## 5. 负载均衡策略 🔥

- **Random（默认，加权随机）**。
- **RoundRobin**（加权轮询）。
- **LeastActive**（最少活跃调用，让慢的提供者收到更少请求）。
- **ConsistentHash**（一致性哈希，相同参数路由到同一节点）。
- Dubbo 3 新增 **最短响应（ShortestResponse）**。

## 6. 容错策略 🔥

- **Failover（默认）**：失败自动切换重试其它节点（读操作、幂等接口）。
- **Failfast**：快速失败，只调一次（非幂等写操作，如新增）。
- **Failsafe**：失败忽略（记录日志，如审计）。
- **Failback**：失败后台定时重发（通知类）。
- **Forking**：并行调多个，有一个成功即返回（实时性高、浪费资源）。
- **Broadcast**：广播逐个调用，任一失败则失败（更新所有节点状态/缓存）。

## 7. 注册中心挂了，consumer 还能调用 provider 吗 ⭐

**能**。Consumer 启动时会把 Provider 地址列表**缓存在本地**，注册中心宕机后仍可用缓存的列表直连调用（只是无法感知新的服务上下线）。这体现了 Dubbo 的高可用设计。

## 8. 如何动态感知服务下线 ⭐

基于注册中心的**事件通知/Watch 机制**：Provider 下线时其在注册中心的临时节点消失，注册中心推送变更给 Consumer，Consumer 更新本地地址列表。

## 9. Dubbo 与 Spring Cloud 的区别 🔥

| 维度 | Dubbo | Spring Cloud |
| --- | --- | --- |
| 通信 | RPC（默认 Netty 长连接，二进制） | HTTP REST（Feign） |
| 性能 | 更高（二进制、长连接） | 略低（HTTP） |
| 定位 | 专注 RPC 服务治理 | 全家桶（配置/网关/熔断/链路） |
| 耦合 | 接口强约定 | 松耦合、跨语言友好 |

> 现在二者融合：Dubbo 3 拥抱云原生（应用级注册、Triple、可对接 Nacos/K8s），Spring Cloud Alibaba 也整合了 Dubbo。

## 10. Zookeeper 与 Dubbo 的关系 ⭐

Zookeeper 是 Dubbo 最常用的注册中心：用 ZNode 临时节点存 Provider 地址，节点宕机会话失效自动删除（实现下线感知），用 Watch 机制推送变更。Dubbo 也支持 Nacos、Redis 等注册中心。

---

## 高频追问清单

- Dubbo SPI 比 Java SPI 强在哪？→ 按需加载 + 自适应 + IOC/AOP（第 3 题）。
- Failover 和 Failfast 分别用在什么场景？→ 幂等读 vs 非幂等写（第 6 题）。
- 注册中心挂了影响调用吗？→ 不影响已缓存的（第 7 题）。
