# 05 · Spring

> 覆盖：IoC/DI、AOP、Bean 生命周期与作用域、循环依赖（三级缓存）、`@Autowired`/`@Resource`、事务（传播/隔离/失效场景）、Spring MVC、设计模式。版本以 Spring 6 / Spring Boot 3 为基线。

---

## 一、IoC 与 DI

### 1. 什么是 Spring，为什么用它 🔥

Spring 是一个**轻量级、非侵入式**的 Java 应用框架，核心是 **IoC 容器**与 **AOP**。价值：

- **解耦**：对象的创建和依赖由容器管理（IoC），业务代码不再 `new` 依赖。
- **声明式能力**：声明式事务、缓存、安全，少写样板代码。
- **生态整合**：MVC、Data、Security、Cloud 全家桶，集成各种中间件。

### 2. IoC 和 DI 🔥

- **IoC（控制反转）**：把对象的创建和生命周期管理权交给容器，是一种**设计思想**。
- **DI（依赖注入）**：IoC 的**实现方式**，容器在运行时把依赖注入到对象中。

### 3. 依赖注入的方式 🔥

| 方式 | 说明 | 推荐度 |
| --- | --- | --- |
| 构造器注入 | 通过构造方法 | **推荐**（依赖不可变、非空、便于测试、能暴露循环依赖） |
| Setter 注入 | 通过 setter | 可选依赖 |
| 字段注入 `@Autowired` | 直接注字段 | 不推荐（难测试、隐藏依赖、无法 final） |

> Spring 官方与 Spring Boot 都推荐**构造器注入**；单构造器时可省略 `@Autowired`。

### 4. `@Autowired` 与 `@Resource` 的区别 🔥

| | `@Autowired` | `@Resource` |
| --- | --- | --- |
| 来源 | Spring | JSR-250（Jakarta） |
| 默认装配 | **按类型（byType）** | **按名称（byName）** |
| 指定名称 | 配合 `@Qualifier` | `name` 属性 |
| 必需性 | `required` 属性 | 无 |

按类型注入遇到多个候选 Bean 时需 `@Qualifier` 或 `@Primary` 消歧。

---

## 二、AOP

### 5. 什么是 AOP，原理 🔥

AOP（面向切面编程）把**横切关注点**（日志、事务、权限、监控）从业务逻辑中抽离，通过动态代理在方法执行前后织入增强。

核心术语：切面（Aspect）、连接点（JoinPoint）、切点（Pointcut）、通知（Advice：`@Before`/`@After`/`@Around`/`@AfterReturning`/`@AfterThrowing`）、织入（Weaving）。

**动态代理实现**：

- **JDK 动态代理**：目标类**实现了接口** → 基于接口生成代理（`Proxy` + `InvocationHandler`）。
- **CGLIB**：目标类**没有接口** → 生成子类字节码代理（不能代理 `final` 类/方法）。
- Spring Boot 2+ **默认用 CGLIB**（`proxyTargetClass=true`）。

### 6. Spring AOP 与 AspectJ 的区别 ⭐

- Spring AOP 是**运行时**动态代理，只能切 Spring 管理的 Bean 的方法（且是 public、被外部调用的方法）。
- AspectJ 是**编译期/类加载期**字节码织入，功能更强（能切字段、构造器、私有方法），性能更好但更重。

---

## 三、Bean 生命周期与作用域

### 7. Bean 的生命周期 🔥

1. **实例化**（构造器/工厂）；
2. **属性填充**（依赖注入）；
3. **Aware 回调**（`BeanNameAware`、`BeanFactoryAware`、`ApplicationContextAware`）；
4. **`BeanPostProcessor#postProcessBeforeInitialization`**；
5. **初始化**：`@PostConstruct` → `InitializingBean#afterPropertiesSet` → 自定义 `init-method`；
6. **`BeanPostProcessor#postProcessAfterInitialization`**（**AOP 代理在此生成**）；
7. 使用；
8. **销毁**：`@PreDestroy` → `DisposableBean#destroy` → `destroy-method`。

> `BeanPostProcessor` 是扩展核心，AOP、`@Autowired`（`AutowiredAnnotationBeanPostProcessor`）都是通过它实现的。

### 8. Bean 的作用域 ⭐

`singleton`（默认，容器内单例）、`prototype`（每次获取新建）、`request`、`session`、`application`、`websocket`（后四个仅 Web）。

### 9. 单例 Bean 是线程安全的吗 🔥

**不是**。Spring 不保证单例 Bean 的线程安全。若 Bean **无状态**（不存可变成员）则天然安全；有可变状态需自行同步或用 `ThreadLocal`/`prototype`。常见的 `Service`/`DAO` 都设计为无状态，所以无问题。

---

## 四、循环依赖（高频难点）

### 10. Spring 如何解决循环依赖 🔥

通过**三级缓存**解决**单例、setter/字段注入**的循环依赖：

- 一级缓存 `singletonObjects`：完整的成品 Bean。
- 二级缓存 `earlySingletonObjects`：提前暴露的半成品（已实例化未填充属性）。
- 三级缓存 `singletonFactories`：`ObjectFactory` 工厂，用于在需要时**生成（可能是代理的）早期引用**。

流程（A 依赖 B，B 依赖 A）：

1. 创建 A，实例化后把 A 的工厂放入三级缓存；
2. 填充 A 属性时发现需要 B → 创建 B；
3. B 填充属性需要 A → 从三级缓存拿到 A 的早期引用（必要时生成代理，升级到二级缓存），B 完成；
4. 回到 A，注入 B，A 完成。

**为什么需要三级缓存而不是二级**：为了在发生循环依赖时也能正确暴露 **AOP 代理对象**（提前触发代理生成），同时保证无循环依赖时代理在正常时机（初始化后）生成。

**解决不了的情况**：

- **构造器注入**的循环依赖（实例化阶段就互相依赖，无法提前暴露）→ 报错。
- `prototype` 作用域的循环依赖（不缓存）。
- 解法：改 setter 注入、`@Lazy`、`@Lazy` 代理、或重构设计。

---

## 五、事务（高频难点）

### 11. 事务的隔离级别 🔥

| 级别 | 脏读 | 不可重复读 | 幻读 |
| --- | --- | --- | --- |
| 读未提交 READ_UNCOMMITTED | 可能 | 可能 | 可能 |
| 读已提交 READ_COMMITTED | 否 | 可能 | 可能 |
| 可重复读 REPEATABLE_READ | 否 | 否 | 可能（MySQL InnoDB 用 MVCC+间隙锁基本避免） |
| 串行化 SERIALIZABLE | 否 | 否 | 否 |

Spring 默认 `DEFAULT`（跟随数据库，MySQL 为 RR）。详见 [09-MySQL](./09-MySQL.md)。

### 12. 事务的传播行为 🔥

| 传播行为 | 含义 |
| --- | --- |
| `REQUIRED`（默认） | 有事务就加入，没有就新建 |
| `REQUIRES_NEW` | 总是新建事务，挂起当前事务 |
| `NESTED` | 嵌套事务（savepoint），外层回滚连带回滚，内层回滚不影响外层 |
| `SUPPORTS` | 有就用，没有就非事务执行 |
| `NOT_SUPPORTED` | 挂起当前事务，非事务执行 |
| `MANDATORY` | 必须存在事务，否则抛异常 |
| `NEVER` | 必须没有事务，否则抛异常 |

### 13. `@Transactional` 失效的常见场景 🔥（高频加分）

1. **方法非 public**（AOP 代理限制）。
2. **自调用**：同类内 A 调用本类 B（`this.B()`），不走代理，事务不生效 → 注入自身代理或拆类。
3. **异常被捕获**未抛出，或抛的是**受检异常**（默认只回滚 `RuntimeException`/`Error`），需 `rollbackFor = Exception.class`。
4. 数据库引擎不支持事务（MyISAM）。
5. Bean 未被 Spring 管理。
6. 多线程：子线程不在同一事务上下文。

### 14. 事务的实现原理 ⭐

声明式事务基于 **AOP**：`@Transactional` 由 `TransactionInterceptor` 拦截，开启连接、绑定到 `ThreadLocal`，正常提交、异常回滚。事务三要素：原子性由 undo log、隔离性由锁+MVCC、持久性由 redo log（详见 MySQL）。

---

## 六、Spring MVC

### 15. Spring MVC 的执行流程 🔥

```
请求 → DispatcherServlet → HandlerMapping 找到 Handler+拦截器链
     → HandlerAdapter 调用 Controller
     → 返回 ModelAndView → ViewResolver 解析视图 → 渲染 → 响应
```

`@RestController = @Controller + @ResponseBody`，`@RequestMapping`/`@GetMapping`、`@RequestParam`/`@PathVariable`/`@RequestBody`、`@ControllerAdvice + @ExceptionHandler`（全局异常）。

---

## 七、设计模式与对比

### 16. Spring 中用到的设计模式 ⭐

- **工厂**：`BeanFactory`/`ApplicationContext`。
- **单例**：默认作用域。
- **代理**：AOP。
- **模板方法**：`JdbcTemplate`、`RestTemplate`。
- **观察者**：`ApplicationEvent`/`ApplicationListener`。
- **适配器**：`HandlerAdapter`。
- **装饰器**：`BeanWrapper`。
- **责任链**：MVC 拦截器链。

### 17. `BeanFactory` 与 `ApplicationContext` 的区别 ⭐

`BeanFactory` 是基础容器，**懒加载**；`ApplicationContext` 是其子接口，**启动时预实例化单例**，并增加国际化、事件发布、资源加载、AOP 等企业级功能。生产用 `ApplicationContext`。

---

## 高频追问清单

- 循环依赖为什么三级缓存而非两级？→ 为了 AOP 代理的早期暴露（本文 10）。
- `@Transactional` 自调用为什么失效？→ 不走代理（本文 13）。
- AOP 默认用 JDK 还是 CGLIB？→ Spring Boot 默认 CGLIB（本文 5）。
- Bean 初始化顺序：`@PostConstruct`、`afterPropertiesSet`、`init-method`？→ 本文 7。
