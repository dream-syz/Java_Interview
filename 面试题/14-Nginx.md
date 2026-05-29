# 14 · Nginx

> 覆盖：是什么与优势、处理 HTTP 请求的流程、进程模型（Master/Worker）、为什么高并发、正向/反向代理、负载均衡策略、与 Apache 对比。

---

## 1. Nginx 是什么，优势 🔥

Nginx 是高性能的 **HTTP 服务器 / 反向代理 / 负载均衡器**。优势：

- **高并发、低内存**：基于**事件驱动 + epoll 异步非阻塞**，单机可支撑数万并发连接。
- **反向代理与负载均衡**。
- **静态资源服务**高效（零拷贝 `sendfile`）。
- **热部署**（不停机重载配置）、稳定（进程隔离）。
- 常用作动静分离、网关入口、限流、缓存。

## 2. Master / Worker 进程模型 🔥

- **Master 进程**：读配置、管理 Worker、平滑重启/升级，不处理请求。
- **Worker 进程**：真正处理请求，数量一般 = CPU 核数（`worker_processes auto`），每个 Worker 单线程 + 事件循环。
- 多个 Worker 通过**共享监听 + 抢锁（accept_mutex）/ SO_REUSEPORT** 接收连接，互不影响（一个崩溃不影响其它），充分利用多核。

## 3. Nginx 如何处理一个 HTTP 请求，为什么高并发 🔥

- 一个 Worker 用 **epoll** 同时监听大量连接的事件，**异步非阻塞**地处理——一个连接的 IO 阻塞不会占住线程，Worker 在事件就绪时才处理。
- 对比传统“一连接一线程/进程”（Apache prefork），Nginx 用极少进程处理海量连接，**没有大量线程上下文切换和内存开销**，所以高并发下表现优异。

## 4. 正向代理 vs 反向代理 🔥

- **正向代理**：代理**客户端**，客户端知道目标服务器，服务器不知道真实客户端（如科学上网、公司出口代理）。
- **反向代理**：代理**服务端**，客户端以为直接访问服务器，实际由 Nginx 转发到后端（隐藏后端、负载均衡、安全）。

## 5. 负载均衡策略 🔥

- **轮询（默认）**：依次分发。
- **weight 加权轮询**：按权重，性能强的多分。
- **ip_hash**：按客户端 IP 哈希，同一 IP 固定到同一后端（解决 session 粘连）。
- **least_conn**：最少连接优先。
- **fair / url_hash**（第三方模块）：按响应时间 / URL 哈希。

```nginx
upstream backend {
    ip_hash;
    server 10.0.0.1:8080 weight=3;
    server 10.0.0.2:8080;
    server 10.0.0.3:8080 backup;  # 备用
}
server {
    location / { proxy_pass http://backend; }
}
```

## 6. Nginx 与 Apache 的区别 ⭐

| | Nginx | Apache |
| --- | --- | --- |
| 模型 | 事件驱动异步非阻塞 | 多进程/多线程（一连接一线程） |
| 并发 | 高（C10K+） | 较低 |
| 静态资源 | 强 | 一般 |
| 动态处理 | 需配合 FastCGI/反代 | 模块丰富（mod_php） |
| 内存 | 省 | 较多 |

## 7. 如何阻止用未定义的服务器名访问 ⭐

配置一个默认 server 捕获未匹配的 `Host`，返回 444（关闭连接）：

```nginx
server {
    listen 80 default_server;
    server_name _;
    return 444;
}
```

## 8. Nginx 的常见用途 ⭐

反向代理与负载均衡、动静分离、静态资源/CDN 源站、API 网关、限流（`limit_req`/`limit_conn`）、灰度发布、防盗链、HTTPS 终止、缓存。

---

## 高频追问清单

- Nginx 为什么能扛高并发？→ epoll + 事件驱动 + 多 Worker（第 2/3 题）。
- 怎么解决 session 粘连？→ `ip_hash` 或后端 session 共享（Redis）（第 5 题）。
- Worker 数怎么配？→ 一般等于 CPU 核数（第 2 题）。
