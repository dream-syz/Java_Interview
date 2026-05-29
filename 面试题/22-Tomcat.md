# 22 · Tomcat

> 覆盖：是什么、整体架构、请求处理流程、类加载机制（打破双亲委派）、连接器线程模型、调优。

---

## 1. Tomcat 是什么 ⭐

开源的 **Servlet 容器 + Web 服务器**，实现了 Servlet/JSP 规范，负责把 HTTP 请求转成 Servlet 调用并返回响应。

## 2. 整体架构 🔥

```
Server
└── Service
    ├── Connector（连接器，处理网络协议：HTTP/AJP）
    └── Container（容器）
        └── Engine → Host（虚拟主机）→ Context（一个 Web 应用）→ Wrapper（一个 Servlet）
```

- **Connector**：负责网络通信与协议解析（把字节流变成 `Request`/`Response`）。
- **Container（Catalina）**：负责 Servlet 的加载与请求处理，四层容器 Engine/Host/Context/Wrapper 用**责任链 + 管道（Pipeline-Valve）**处理请求。

## 3. 请求处理流程 ⭐

1. Connector 接收连接，解析 HTTP 请求为 `Request`/`Response`。
2. 交给 Engine → 按域名选 Host → 按路径选 Context（应用）→ 选 Wrapper（Servlet）。
3. 经过各级 Pipeline 的 Valve，最终执行 **Filter 链 → Servlet.service()**。
4. 返回响应。

## 4. Tomcat 的类加载机制 🔥🔥（高频）

Tomcat **打破双亲委派**以实现**应用隔离**。类加载器层次：

```
Bootstrap → System → Common → ┬─ Catalina（Tomcat 自身）
                              └─ Shared → WebappClassLoader（每个应用一个）
```

- 每个 Web 应用有独立的 **WebappClassLoader**，加载 `WEB-INF/classes` 和 `WEB-INF/lib`。
- 加载顺序：先让 Bootstrap 加载 JRE 核心类（**这部分仍遵守双亲委派，防止核心类被覆盖**），然后**优先自己加载**（不再委派给父加载器），父加载器找不到再委派。
- 目的：① 同一容器多个应用的类**相互隔离**；② 不同应用可用**同一类库的不同版本**；③ 与 Tomcat 自身类隔离。

## 5. 连接器线程模型（IO 模型）🔥

Tomcat 的 Connector 支持多种 IO：

- **BIO**：一连接一线程（老版本，已移除）。
- **NIO（默认）**：基于 Java NIO，少量线程 + Selector 处理大量连接。
- **NIO2（AIO）**：异步 IO。
- **APR**：基于本地库，性能高（需安装）。

线程池参数：`maxThreads`（最大工作线程）、`acceptCount`（等待队列）、`maxConnections`（最大连接数）、`minSpareThreads`。

## 6. Tomcat 调优 ⭐

- 调线程池：`maxThreads`、`acceptCount`、`maxConnections` 按并发量与压测调整。
- 用 NIO/APR 连接器。
- 开启压缩、调 JVM 参数（堆、GC，见 [03-JVM](./03-JVM.md)）。
- 关闭无用应用、调 `connectionTimeout`、开启 keep-alive。
- 高并发入口常用 **Nginx 反向代理 + 多 Tomcat** 集群。

---

## 高频追问清单

- Tomcat 为什么打破双亲委派？→ 应用隔离 + 多版本共存（第 4 题）。
- Tomcat 默认用什么 IO 模型？→ NIO（第 5 题）。
- 一个请求在 Tomcat 内部怎么流转？→ Connector → Engine/Host/Context/Wrapper → Filter → Servlet（第 3 题）。
