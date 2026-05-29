# 19 · Linux

> 覆盖：常用命令、目录与路径、进程查看、日志排查（重点）、文本处理、IO 模型。后端工程师线上排查必备。

---

## 1. 路径与目录符号 ⭐

- 绝对路径：以 `/` 开头。
- 当前目录：`.`；上层目录：`..`；用户主目录：`~`；上一次目录：`cd -`。
- 切换目录：`cd`。

## 2. 常用命令速查 🔥

| 类别 | 命令 |
| --- | --- |
| 目录文件 | `ls -al`、`cd`、`pwd`、`mkdir`、`rm -rf`、`cp`、`mv`、`find / -name xx` |
| 查看文件 | `cat`、`less`/`more`、`head -n`、`tail -f`、`wc -l` |
| 文本处理 | `grep`、`awk`、`sed`、`sort`、`uniq`、`cut`、`wc` |
| 进程 | `ps -ef`、`top`、`kill -9 pid`、`jps` |
| 网络 | `netstat -anp`、`ss -lntp`、`curl`、`ping`、`telnet`、`lsof -i:8080` |
| 磁盘内存 | `df -h`、`du -sh *`、`free -m` |
| 权限 | `chmod`、`chown`、`chgrp` |

## 3. 怎么查看/排查日志 🔥（高频实战）

```bash
tail -f app.log                     # 实时跟踪日志
tail -n 200 app.log                 # 看最后 200 行
grep -n "Exception" app.log         # 找异常并显示行号
grep -C 10 "OrderId=123" app.log    # 匹配行的上下各 10 行
grep "ERROR" app.log | wc -l        # 统计错误数
less app.log                        # 大文件分页查看（/搜索，G 到末尾）
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head  # 统计访问 IP TopN
zcat app.log.gz | grep ERROR        # 查压缩日志
```

排查链路：先 `tail`/`grep` 定位异常时间和堆栈 → 结合链路 ID（traceId）串联 → 必要时 `jstack`/`jmap`（见 [03-JVM](./03-JVM.md)）。

## 4. 排查 CPU 飙高 / 内存 🔥

```bash
top                       # 看哪个进程 CPU/内存高
top -Hp <pid>             # 看进程内哪个线程高
printf '%x\n' <tid>       # 线程号转 16 进制
jstack <pid> | grep <hex> -A 30   # 定位 Java 线程栈
```

内存：`free -m` 看系统，`jmap -heap <pid>` / `jstat -gcutil` 看 JVM 堆与 GC。

## 5. 查看进程与端口占用 ⭐

```bash
ps -ef | grep java        # 查进程
jps -l                    # 查 Java 进程
netstat -anp | grep 8080  # 查端口
lsof -i:8080              # 查端口占用进程
kill -9 <pid>             # 强制结束
```

## 6. Linux 五种 IO 模型 ⭐（与 NIO 关联）

阻塞 IO、非阻塞 IO、**IO 多路复用（select/poll/epoll）**、信号驱动 IO、异步 IO。

- **epoll** 相比 select/poll：无 fd 数量限制、事件回调（不轮询全部）、O(1) 就绪通知，是 Nginx、Redis、Netty 高并发的基础（详见 [32-操作系统](./32-操作系统.md)）。

## 7. 负载与性能排查工具链 🔥（线上实战进阶）

```bash
uptime                    # load average 1/5/15 分钟负载，>核数说明繁忙
vmstat 1                  # 看 r(运行队列) b(阻塞) si/so(换页) us/sy/wa(CPU 分布)
iostat -x 1               # 看磁盘 %util(接近100%=IO瓶颈) await(IO延迟)
mpstat -P ALL 1           # 每核 CPU，定位单核打满
pidstat -u 1 -p <pid>     # 指定进程的 CPU/IO 明细
sar -n DEV 1              # 网络流量
```

> **排障心法（USE 法）**：对每类资源(CPU/内存/磁盘/网络)看 **使用率(Utilization)、饱和度(Saturation)、错误(Errors)**。
> - load 高但 CPU 不高 → 看 `vmstat` 的 `b` 列和 `iostat` 的 `await`，多半是 **IO 等待/磁盘瓶颈**。
> - `wa`(iowait) 高 → 磁盘慢或换页；`sy`(内核态) 高 → 系统调用/上下文切换多。

## 8. 关键参数与常见坑 ⭐

- **文件句柄数**：`ulimit -n` 默认 1024，高并发服务必调大（`too many open files` 的根因），改 `/etc/security/limits.conf`。
- **TIME_WAIT 堆积**：短连接高并发场景，`ss -s` 看连接状态分布；调 `net.ipv4.tcp_tw_reuse`、用长连接/连接池缓解（见 [16-计算机网络](./16-计算机网络.md)）。
- **OOM Killer**：内存不足时内核杀进程，`dmesg | grep -i kill` 查是否被杀（容器里尤其常见，见 [26-云原生](./26-云原生.md) JVM 容器感知）。
- **僵尸进程**：`ps` 看 `Z` 状态，父进程未 wait 子进程退出码所致。

---

## 高频追问清单

- 怎么查一个端口被谁占用？→ `lsof -i:port` / `netstat -anp`（第 5 题）。
- 实时看日志并过滤错误？→ `tail -f | grep`（第 3 题）。
- CPU 100% 怎么定位到 Java 代码？→ top -Hp + jstack（第 4 题）。
- epoll 比 select 强在哪？→ 无遍历、无上限、回调（第 6 题）。
- load 高但 CPU 不高是什么原因？→ IO 等待/磁盘瓶颈，看 vmstat/iostat（第 7 题）。
- `too many open files` 怎么解决？→ 调 `ulimit -n` 与 limits.conf（第 8 题）。
- 进程莫名被杀怎么查？→ `dmesg` 看 OOM Killer（第 8 题）。
