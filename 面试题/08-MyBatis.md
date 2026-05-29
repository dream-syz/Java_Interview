# 08 · MyBatis

> 覆盖：`#{}`/`${}`、Mapper 接口动态代理、一级/二级缓存、延迟加载、结果映射、分页插件、设计模式、批量插入。

---

## 一、基础

### 1. 什么是 MyBatis，优缺点 🔥

MyBatis 是**半自动 ORM** 框架：SQL 由开发者编写（写在 XML 或注解），框架负责参数映射、结果集映射、连接管理。

- 优点：SQL 灵活可控、与 JDBC 相比消除大量样板代码、易于优化、学习成本低。
- 缺点：SQL 与数据库耦合（移植性差）、要手写 SQL（相比 JPA 自动化低）。
- 对比 Hibernate/JPA：Hibernate 全自动、可移植性强但 SQL 不可控、复杂查询性能调优难；MyBatis 在国内互联网更流行（SQL 可控、便于优化）。

### 2. `#{}` 和 `${}` 的区别 🔥🔥

| | `#{}` | `${}` |
| --- | --- | --- |
| 处理方式 | 预编译占位符 `?`，参数通过 `PreparedStatement` 设值 | 字符串直接拼接 |
| 安全性 | **防 SQL 注入** | **有注入风险** |
| 用途 | 传值（绝大多数场景） | 动态表名/列名/`order by` 字段等无法用占位符的地方 |

> 结论：**默认一律用 `#{}`**；只有在拼接表名、排序字段等不得已时用 `${}`，且必须做白名单校验。

---

## 二、缓存

### 3. MyBatis 的一级缓存与二级缓存 🔥

- **一级缓存（SqlSession 级，默认开启）**：同一个 `SqlSession` 内，相同 SQL+参数的查询第二次命中缓存。执行 `insert/update/delete`、`commit/close`、`clearCache` 会清空。在 Spring 中通常每个事务/请求一个 SqlSession，跨请求不共享。
- **二级缓存（Mapper/namespace 级，默认关闭）**：同一个 namespace 的多个 SqlSession 共享，需 `<cache/>` 开启、实体可序列化。
- **二级缓存的坑**：多表关联、多 namespace 操作同一张表时容易**脏读**；分布式部署下本地二级缓存不一致。**生产很少用 MyBatis 二级缓存，更多用 Redis 等外部缓存。**

---

## 三、原理（高频）

### 4. Mapper 接口没有实现类，为什么能调用 🔥🔥

MyBatis 用 **JDK 动态代理**：`MapperProxyFactory` 为接口生成代理对象 `MapperProxy`，调用接口方法时被 `invoke` 拦截，根据**接口全限定名 + 方法名**找到对应的 `MappedStatement`（即 XML 里的 SQL），交给 `SqlSession` 执行。

```
mapper.selectById(1)
  → MapperProxy.invoke()
  → MapperMethod.execute()
  → sqlSession.selectOne("com.xx.UserMapper.selectById", 1)
  → Executor → StatementHandler → JDBC
```

### 5. MyBatis 的核心组件 ⭐

`SqlSessionFactory` → `SqlSession` → `Executor`（执行器：SimpleExecutor/ReuseExecutor/BatchExecutor，CachingExecutor 装饰二级缓存）→ `StatementHandler`（处理 JDBC Statement）→ `ParameterHandler`（参数）→ `ResultSetHandler`（结果映射）→ `TypeHandler`（类型转换）。

### 6. 结果集如何映射为对象 ⭐

通过 `ResultSetHandler` + 反射，根据 `resultType`/`resultMap` 把列映射到对象属性。映射形式：
- 列名与属性名一致 → 自动映射。
- 不一致 → 用别名 `as`、或开启 `mapUnderscoreToCamelCase`（下划线转驼峰）、或显式 `resultMap`。

### 7. 延迟加载（懒加载）原理 ⭐

针对关联查询（`association`/`collection`），开启 `lazyLoadingEnabled` 后，MyBatis 用 **CGLIB/Javassist 生成代理对象**，访问关联属性的 getter 时才触发额外 SQL 查询。适合关联数据不一定用到的场景，减少无谓查询。

---

## 四、实战

### 8. 如何分页，分页插件原理 ⭐

- 逻辑分页：`RowBounds` 内存分页（查全部再截取，**大数据量会 OOM，不推荐**）。
- 物理分页：手写 `limit`，或用 **PageHelper** 插件。
- 插件原理：基于 MyBatis 的**拦截器（Interceptor / 插件机制）**，拦截 `Executor`/`StatementHandler`，在 SQL 执行前**改写 SQL 拼接 `limit`** 并自动执行 `count` 查询。插件本质是**责任链 + 动态代理**。

### 9. 如何批量插入 ⭐

- `<foreach>` 拼接 `INSERT INTO t VALUES (),(),()`（单条大 SQL，注意 `max_allowed_packet`）。
- 用 `ExecutorType.BATCH` 的 `SqlSession`，循环 `insert` 后统一 `flushStatements`（JDBC batch，性能好、内存可控）。

### 10. XML 常用标签 ⭐

`<select>`/`<insert>`/`<update>`/`<delete>`、`<resultMap>`、`<sql>`/`<include>`（SQL 片段复用）、动态 SQL：`<if>`/`<choose>`/`<where>`/`<set>`/`<trim>`/`<foreach>`/`<bind>`。

### 11. MyBatis 用到的设计模式 ⭐

- **代理模式**：Mapper 接口动态代理。
- **工厂模式**：`SqlSessionFactory`、`ObjectFactory`。
- **建造者模式**：`SqlSessionFactoryBuilder`、`XMLConfigBuilder`。
- **装饰器模式**：`CachingExecutor` 装饰 Executor。
- **模板方法**：`BaseExecutor`。
- **责任链**：插件拦截器链。

---

## 高频追问清单

- `#{}` 和 `${}` 区别、为什么能防注入？→ 本文 2。
- Mapper 没实现类怎么工作的？→ JDK 动态代理（本文 4）。
- 一级缓存什么时候失效、为什么慎用二级缓存？→ 本文 3。
- MyBatis-Plus 与 MyBatis 关系？→ 增强工具，提供通用 CRUD、条件构造器、代码生成，不破坏 MyBatis。
