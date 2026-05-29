# 06 · Spring Boot

> 覆盖：自动装配原理（核心）、Starter、核心注解、配置加载顺序、Actuator、热部署、与 Spring/Spring Cloud 的关系。基线 Spring Boot 3.x（JDK 17+、Jakarta 命名空间）。

---

## 一、核心价值与注解

### 1. 为什么用 Spring Boot 🔥

- **约定优于配置**：内置合理默认，告别 Spring 繁琐的 XML。
- **自动装配**：根据依赖自动配置 Bean。
- **起步依赖 Starter**：一个依赖搞定一类功能的传递依赖与版本管理。
- **内嵌容器**（Tomcat/Jetty/Undertow）：打成可执行 jar，`java -jar` 直接跑，便于云原生部署。
- **生产就绪**：Actuator 提供监控、健康检查、指标。

### 2. 核心注解 `@SpringBootApplication` 🔥

它是一个组合注解，等价于三个：

```java
@SpringBootConfiguration  // 本质是 @Configuration，标记配置类
@EnableAutoConfiguration  // 开启自动装配（核心）
@ComponentScan            // 扫描当前包及子包的组件
```

---

## 二、自动装配原理（必考核心）

### 3. 自动装配（Auto-Configuration）原理 🔥🔥

`@EnableAutoConfiguration` 通过 `@Import(AutoConfigurationImportSelector.class)` 导入选择器：

1. 启动时，`AutoConfigurationImportSelector` 读取所有 jar 包中的自动配置清单：
   - **Spring Boot 2.7 之前**：`META-INF/spring.factories` 里 `EnableAutoConfiguration` 的值。
   - **Spring Boot 2.7+/3.x**：`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`。
2. 得到一堆 `XxxAutoConfiguration` 候选配置类。
3. 每个配置类上用 **`@Conditional` 系列条件注解**按需生效：
   - `@ConditionalOnClass`（类路径存在某类才配置）、`@ConditionalOnMissingBean`（用户没自定义才用默认）、`@ConditionalOnProperty`（配置开关）等。
4. 满足条件的配置类向容器注册 Bean，配合 `@ConfigurationProperties` 把 `application.yml` 的配置绑定进去。

> 一句话总结：**“按依赖、按条件，自动注册一批默认 Bean，且允许用户覆盖（`@ConditionalOnMissingBean`）”**。这就是“约定优于配置”的落地机制。

### 4. Starter 是什么 🔥

Starter 是一组**便捷的依赖描述符**。引入 `spring-boot-starter-web` 就自动带来 Spring MVC、内嵌 Tomcat、Jackson 等及其兼容版本，无需手动管理一堆依赖和版本冲突。

自定义 Starter 套路：`xxx-spring-boot-autoconfigure`（放 `@AutoConfiguration` + 条件注解 + `@ConfigurationProperties`）+ `xxx-spring-boot-starter`（聚合依赖），并在 `AutoConfiguration.imports` 中登记。

常用 Starter：`web`、`data-jpa`、`data-redis`、`amqp`、`security`、`validation`、`actuator`、`test`。

---

## 三、配置

### 5. 核心配置文件与区别 ⭐

- `application.properties` / `application.yml`（YAML 层级清晰，推荐）。
- `bootstrap.yml`（Spring Cloud 中用于配置中心引导，**优先级最高**；Spring Cloud 2020+ 需引入 `bootstrap` 依赖；Boot 3 推荐用 `spring.config.import`）。
- **多环境**：`application-{profile}.yml`，用 `spring.profiles.active` 激活。

### 6. 配置加载优先级 ⭐

从高到低（部分）：命令行参数 > 环境变量 / 系统属性 > 外部 `config/` 下的配置 > jar 包内 `application-{profile}.yml` > jar 包内 `application.yml`。**外部、特定 profile、命令行优先级更高**，便于不改包覆盖配置。

### 7. 启动时执行特定代码 ⭐

实现 `CommandLineRunner` 或 `ApplicationRunner` 接口（`run` 方法在容器启动完成后执行），或监听 `ApplicationReadyEvent`。

---

## 四、监控与部署

### 8. Actuator 监控 ⭐

`spring-boot-starter-actuator` 暴露 `/actuator/health`（健康检查，配合 K8s 探针）、`/metrics`、`/info`、`/env`、`/loggers`、`/threaddump` 等端点。生产需通过 `management.endpoints.web.exposure.include` 控制暴露范围并配合鉴权，避免信息泄露。常配合 **Micrometer + Prometheus + Grafana**。

### 9. 运行方式与是否需要独立容器 ⭐

- 运行：`java -jar app.jar`、IDE 运行 main、`mvn spring-boot:run`。
- **不需要**独立容器，内嵌 Tomcat 即可；也可打 war 部署到外部容器（需继承 `SpringBootServletInitializer`）。

### 10. 热部署 ⭐

引入 `spring-boot-devtools`，类路径变化自动重启（用两个类加载器：base 加载不变的依赖、restart 加载你的代码，仅重载后者，比冷启动快）。生产环境务必关闭。

---

## 五、与 Spring / Spring Cloud 的关系

### 11. Spring Boot 与 Spring Cloud 的区别 ⭐

- **Spring Boot** 是快速构建**单个**应用的脚手架（自动装配、内嵌容器）。
- **Spring Cloud** 是基于 Spring Boot 的**微服务治理**框架集（注册发现、配置中心、网关、熔断、链路追踪），**依赖 Spring Boot**。
- 关系：Boot 是“盖房子”，Cloud 是“管理一片小区”。详见 [07-SpringCloud](./07-SpringCloud.md)。

---

## 高频追问清单

- 自动装配清单文件在哪？2.7 前后有什么变化？→ `spring.factories` → `AutoConfiguration.imports`（本文 3）。
- 怎么让自己的配置覆盖默认配置？→ `@ConditionalOnMissingBean`（本文 3）。
- `@Conditional` 有哪些常用条件？→ `OnClass`/`OnMissingBean`/`OnProperty`（本文 3）。
- Spring Boot 3 有哪些大变化？→ JDK 17 基线、Jakarta 命名空间、原生镜像 GraalVM、Observability。
