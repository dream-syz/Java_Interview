# 20 · Maven

> 覆盖：是什么、坐标与仓库、生命周期、依赖范围、依赖传递与冲突仲裁、常用命令。

---

## 1. Maven 是什么 ⭐

项目构建与依赖管理工具。核心能力：**依赖管理**（自动下载、传递依赖）、**标准化构建生命周期**、**多模块管理**。基于 `pom.xml` 描述项目。

## 2. 坐标与仓库 ⭐

- **坐标（GAV）**：`groupId`（组织）+ `artifactId`（项目）+ `version`（版本），唯一定位一个构件。
- **仓库**：本地仓库（`~/.m2/repository`）→ 私服（Nexus）→ 中央仓库。先找本地，没有再去远程下载并缓存。

## 3. 构建生命周期 🔥

三套独立生命周期，每套由阶段（phase）组成，执行某阶段会执行它之前的所有阶段：

- **clean**：`pre-clean → clean → post-clean`。
- **default（核心）**：`validate → compile → test → package → verify → install → deploy`。
- **site**：生成站点文档。

常用：`mvn clean package`（清理后打包）、`mvn clean install`（装到本地仓库供其它模块依赖）、`mvn deploy`（发到私服）。

## 4. 依赖范围 scope 🔥

| scope | 编译 | 测试 | 运行 | 打包 | 例子 |
| --- | --- | --- | --- | --- | --- |
| compile（默认） | ✓ | ✓ | ✓ | ✓ | 大多数依赖 |
| provided | ✓ | ✓ | ✗ | ✗ | servlet-api、lombok |
| runtime | ✗ | ✓ | ✓ | ✓ | JDBC 驱动 |
| test | ✗ | ✓ | ✗ | ✗ | junit |
| system | ✓ | ✓ | ✗ | ✗ | 本地 jar（不推荐） |

## 5. 依赖传递与冲突仲裁 🔥（高频）

A 依赖 B，B 依赖 C，则 A 间接依赖 C（传递依赖）。当同一构件出现多个版本时，Maven 的仲裁规则：

1. **最短路径优先**：依赖树中路径更短的版本胜出。
2. **路径相同则先声明优先**（pom 中靠前的）。

排查与解决冲突：

```bash
mvn dependency:tree          # 查看依赖树，定位冲突
mvn dependency:tree -Dverbose -Dincludes=xxx
```

- 用 `<exclusions>` 排除传递来的不想要的依赖。
- 用 `<dependencyManagement>` **统一管理版本**（不实际引入，只锁版本，子模块继承）。Spring Boot 的 `spring-boot-dependencies` 就是 BOM。

## 6. `dependencyManagement` vs `dependencies` ⭐

- `dependencies`：直接引入依赖。
- `dependencyManagement`：只**声明版本和配置**，不引入；子模块/本模块再写 `dependencies` 时可省略版本，统一受管。父 POM / BOM 常用。

## 7. 常用命令 ⭐

`mvn clean`、`mvn compile`、`mvn test`、`mvn package`、`mvn install`、`mvn deploy`、`mvn dependency:tree`、`-DskipTests`（跳过测试执行但编译测试代码）、`-Dmaven.test.skip=true`（跳过编译和执行）。

---

## 高频追问清单

- 依赖冲突怎么排查解决？→ `dependency:tree` + 最短路径/先声明 + exclusions（第 5 题）。
- `<dependencyManagement>` 有什么用？→ 统一版本管理、BOM（第 6 题）。
- `provided` 和 `compile` 区别？→ provided 不参与打包/运行（容器已提供）（第 4 题）。
