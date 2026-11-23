<div align="center">
  <img src="https://github.com/DBGuardian/DBGuardian-doc/blob/main/logo.png?raw=true" alt="DBGuardian Logo" width="220" />

  # DBGuardian

  ![v1.0.0](https://img.shields.io/badge/version-v1.0.0-green)
  ![GitHub stars](https://img.shields.io/github/stars/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![GitHub forks](https://img.shields.io/github/forks/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![GitHub issues](https://img.shields.io/github/issues/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)
</div>

DBGuardian 是一套面向 Spring Boot 应用的数据库高可用解决方案，专注于读写分离、自动故障转移、分布式协调与降级启动能力，帮助业务在数据库异常时保持更高的可用性与连续性。

## 项目简介

DBGuardian 在同一个 `DBGuardian-Spring-Boot-Starter` 仓库中提供两个模块，分别面向 Spring Boot 2 与 Spring Boot 3 场景：

- `dbguardian-boot2-starter`：适用于 Spring Boot 2.7.x
- `dbguardian-boot3-starter`：适用于 Spring Boot 3.x

两个模块提供相同的核心能力，均支持 MySQL、PostgreSQL 和 MariaDB。先按 Spring Boot 版本选择 Starter，再按数据库选择方言和 JDBC 驱动。

## 核心能力

- **读写分离**：自动识别读写操作，将查询路由到从库、写操作路由到主库
- **自动故障转移**：主库异常时自动切换，从而降低服务中断风险
- **分布式协调**：基于 Redis 实现多实例状态同步与协同控制
- **健康检查**：持续检测主从数据库可用性，及时感知故障
- **原主库恢复**：支持原主库恢复后自动作为从库继续追赶数据
- **降级启动**：数据库暂时不可用时，允许应用以降级模式启动

## 当前支持矩阵

### ORM 支持

- **MyBatis-Plus**：已支持
- **MyBatis**：已支持
- **JPA / Hibernate**：已支持
- **Spring JDBC**：已支持

### 已验证版本组合

以下列出[测试项目规划](DBGuardian-doc/doc/测试项目规划.md#按-java-版本)中标为「已完成」的组合，按 Java 版本排列：

- `Java 8 + Spring Boot 2.7 + JPA / Hibernate + MariaDB`
- `Java 8 + Spring Boot 2.7 + JPA / Hibernate + MySQL`
- `Java 8 + Spring Boot 2.7 + JPA / Hibernate + PostgreSQL`
- `Java 8 + Spring Boot 2.7 + MyBatis + MariaDB`
- `Java 8 + Spring Boot 2.7 + MyBatis + MySQL`
- `Java 8 + Spring Boot 2.7 + MyBatis + PostgreSQL`
- `Java 8 + Spring Boot 2.7 + MyBatis-Plus + MariaDB`
- `Java 8 + Spring Boot 2.7 + MyBatis-Plus + MySQL`
- `Java 8 + Spring Boot 2.7 + MyBatis-Plus + PostgreSQL`
- `Java 8 + Spring Boot 2.7 + Spring JDBC + MariaDB`
- `Java 8 + Spring Boot 2.7 + Spring JDBC + MySQL`
- `Java 8 + Spring Boot 2.7 + Spring JDBC + PostgreSQL`
- `Java 11 + Spring Boot 2.7 + MyBatis-Plus + MariaDB`
- `Java 11 + Spring Boot 2.7 + MyBatis-Plus + MySQL`
- `Java 11 + Spring Boot 2.7 + Spring JDBC + MariaDB`
- `Java 11 + Spring Boot 2.7 + Spring JDBC + MySQL`
- `Java 11 + Spring Boot 2.7 + Spring JDBC + PostgreSQL`
- `Java 17 + Spring Boot 3.0 + MyBatis-Plus + MariaDB`
- `Java 17 + Spring Boot 3.0 + MyBatis-Plus + MySQL`
- `Java 17 + Spring Boot 3.1 + JPA / Hibernate + MariaDB`
- `Java 17 + Spring Boot 3.1 + JPA / Hibernate + MySQL`
- `Java 17 + Spring Boot 3.1 + MyBatis-Plus + MariaDB`
- `Java 17 + Spring Boot 3.1 + MyBatis-Plus + MySQL`
- `Java 17 + Spring Boot 3.1 + Spring JDBC + MariaDB`
- `Java 17 + Spring Boot 3.1 + Spring JDBC + MySQL`
- `Java 17 + Spring Boot 3.1 + Spring JDBC + PostgreSQL`
- `Java 17 + Spring Boot 3.2 + JPA / Hibernate + MariaDB`
- `Java 17 + Spring Boot 3.2 + JPA / Hibernate + PostgreSQL`
- `Java 17 + Spring Boot 3.2 + MyBatis + MariaDB`
- `Java 17 + Spring Boot 3.2 + MyBatis + MySQL`
- `Java 17 + Spring Boot 3.2 + MyBatis + PostgreSQL`
- `Java 17 + Spring Boot 3.2 + MyBatis-Plus + MariaDB`
- `Java 17 + Spring Boot 3.2 + MyBatis-Plus + MySQL`
- `Java 17 + Spring Boot 3.2 + MyBatis-Plus + PostgreSQL`
- `Java 21 + Spring Boot 3.3 + MyBatis-Plus + MariaDB`
- `Java 21 + Spring Boot 3.3 + MyBatis-Plus + MySQL`
- `Java 21 + Spring Boot 3.3 + Spring JDBC + MariaDB`
- `Java 21 + Spring Boot 3.3 + Spring JDBC + MySQL`
- `Java 21 + Spring Boot 3.3 + Spring JDBC + PostgreSQL`

### 版本参考

- **Spring Boot 2.7.x**：使用仓库内的 `dbguardian-boot2-starter`
- **Spring Boot 3.x**：使用仓库内的 `dbguardian-boot3-starter`
- 具体 Spring Boot 3.x 兼容性以测试项目规划中的矩阵为准

## 适用场景

- 需要提升数据库可用性的 Spring Boot 项目
- 已有主从复制架构，希望自动完成读写切分的系统
- 希望在主库故障时自动切换、减少人工介入的业务
- 需要 Redis 协调多实例状态的分布式应用

## 快速开始

### 选择 Starter

#### Spring Boot 2

```xml
<dependency>
    <groupId>io.dbguardian</groupId>
    <artifactId>dbguardian-boot2-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```

#### Spring Boot 3

```xml
<dependency>
    <groupId>io.dbguardian</groupId>
    <artifactId>dbguardian-boot3-starter</artifactId>
    <version>1.0.0</version>
</dependency>
```

### 数据库方言 `pom.xml` 配置示例

按 Spring Boot 版本选择一个 Starter，再按数据库从下方选择一组「方言 + JDBC 驱动」依赖，一起放入应用 `pom.xml` 的 `<dependencies>` 中。方言与 Starter 使用相同版本。若 Spring Boot 未管理示例所用坐标的驱动版本（例如较早的 Spring Boot 2.7 小版本可能不管理 `com.mysql:mysql-connector-j`），请为该驱动依赖显式指定兼容的 `<version>`。

**MySQL**

```xml
<dependency>
    <groupId>io.dbguardian</groupId>
    <artifactId>dbguardian-dialect-mysql</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <scope>runtime</scope>
</dependency>
```

**PostgreSQL**

```xml
<dependency>
    <groupId>io.dbguardian</groupId>
    <artifactId>dbguardian-dialect-postgresql</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

**MariaDB**

```xml
<dependency>
    <groupId>io.dbguardian</groupId>
    <artifactId>dbguardian-dialect-mariadb</artifactId>
    <version>1.0.0</version>
</dependency>
<dependency>
    <groupId>org.mariadb.jdbc</groupId>
    <artifactId>mariadb-java-client</artifactId>
    <scope>runtime</scope>
</dependency>
```

### 按数据库配置

选择下面的一份配置写入 `application.yml`，适用于 Spring Boot 2.7 和 3.x。先在数据库侧建立复制，再替换示例中的节点地址和库名，确保两个 JDBC URL 分别指向不同节点。启动前设置 `DBGUARDIAN_DB_USERNAME`、`DBGUARDIAN_DB_PASSWORD`、`DBGUARDIAN_REPLICATION_USER` 和 `DBGUARDIAN_REPLICATION_PASSWORD`，分别用于数据源连接和复制，避免将真实凭据写入配置文件。

#### MySQL 配置示例

```yaml
spring:
  datasource:
    master:
      url: jdbc:mysql://mysql-primary:3306/appdb
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    slave:
      url: jdbc:mysql://mysql-replica:3306/appdb
      driver-class-name: com.mysql.cj.jdbc.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    replication:
      master-user: ${DBGUARDIAN_REPLICATION_USER}
      master-password: ${DBGUARDIAN_REPLICATION_PASSWORD}
    allow-degraded-startup: true
```

查询优先走健康从库，写入走当前主库。`allow-degraded-startup` 允许数据库暂不可用时降级启动应用；该选项也适用于下面两种数据库。

#### PostgreSQL 配置示例

以下以 PostgreSQL 16 的 Linux 路径为例，假定应用能在待恢复的原主 `pg-a` 上本地执行 `pg_rewind`，且执行账号有权限访问数据目录。请按安装方式调整路径，并将 `pg-rewind-recover.sh` 部署到待恢复节点。

```yaml
spring:
  datasource:
    master:
      url: jdbc:postgresql://pg-a:5432/appdb
      driver-class-name: org.postgresql.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    slave:
      url: jdbc:postgresql://pg-b:5432/appdb
      driver-class-name: org.postgresql.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    replication:
      master-user: ${DBGUARDIAN_REPLICATION_USER}
      master-password: ${DBGUARDIAN_REPLICATION_PASSWORD}
    recovery:
      enabled: true
      auto-failback: true
      postgresql:
        enabled: true
        keep-promoted-primary: true
        data-directory: /var/lib/postgresql/16/main
        config-file: /etc/postgresql/16/main/postgresql.conf
        script-path: /opt/dbguardian/scripts/pg-rewind-recover.sh
        pg-ctl-command: /usr/lib/postgresql/16/bin/pg_ctl
        psql-command: /usr/lib/postgresql/16/bin/psql
        pg-rewind-command: /usr/lib/postgresql/16/bin/pg_rewind
        local-host: 127.0.0.1
        local-port: 5432
        local-database: postgres
        startup-wait-seconds: 60
        command-timeout-seconds: 900
    allow-degraded-startup: true
```

`keep-promoted-primary: true` 保留故障期间升主的节点承接写入，原主作为从库追赶，确认流复制正常后再承担读请求。应用与数据库分开部署时，在 `spring.datasource.recovery.postgresql` 下设置 `remote-execution-enabled: true` 和 `remote-executor-command`，让恢复脚本在待恢复节点上执行。

#### MariaDB 配置示例

适用于 MariaDB 10.11+ 单主单从环境。请预先配置 GTID（全局事务标识）复制，并为两个节点设置不同的 `server_id`。

```yaml
spring:
  datasource:
    master:
      url: jdbc:mariadb://mariadb-primary:3306/appdb?connectTimeout=3000&socketTimeout=10000
      driver-class-name: org.mariadb.jdbc.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    slave:
      url: jdbc:mariadb://mariadb-replica:3306/appdb?connectTimeout=3000&socketTimeout=10000
      driver-class-name: org.mariadb.jdbc.Driver
      username: ${DBGUARDIAN_DB_USERNAME}
      password: ${DBGUARDIAN_DB_PASSWORD}
    replication:
      master-user: ${DBGUARDIAN_REPLICATION_USER}
      master-password: ${DBGUARDIAN_REPLICATION_PASSWORD}
    recovery:
      enabled: true
      auto-failback: true
      catchup-timeout-seconds: 300
      catchup-check-interval-seconds: 2
    gtid-protection:
      enabled: true
      block-slave-reads-on-risk: true
      max-allowed-lag-seconds: 5
    allow-degraded-startup: true
```

`auto-failback: true` 表示原主追平后尝试切回原主。设为 `false` 时，故障期间升主的节点继续承接写入，原主追平且复制健康后可承担读请求。检测到复制风险时，`gtid-protection` 会阻止从库读取。

#### 通用可选配置

- **Redis**：多实例需要协调节点状态时，添加 `spring-boot-starter-data-redis`，并配置 `host`、`port`、`password`、`database` 和 `timeout`。Spring Boot 2.7 使用 `spring.redis`，Spring Boot 3.x 使用 `spring.data.redis`。单实例可不配置 Redis。
- **连接池**：如需调整 HikariCP，可在 `spring.datasource.master.hikari` 和 `spring.datasource.slave.hikari` 下分别配置连接池大小和超时。

### 使用方式

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;

    public User getUser(Long id) {
        return userMapper.selectById(id);
    }

    public void createUser(User user) {
        userMapper.insert(user);
    }
}
```

## 工作原理

### 读写分离

应用通过方法名与上下文感知请求类型，自动决定使用主库还是从库：

- `select` / `get` / `query` 等读方法 → 从库
- `insert` / `update` / `delete` 等写方法 → 主库

### 故障转移

1. 健康检查发现主库不可用
2. 尝试获取 Redis 分布式锁
3. 将从库提升为主库
4. 广播状态变更
5. 其他实例同步最新状态
6. 原主库恢复后作为从库重新加入

## 版本与依赖要求

- Spring Boot 2.7.x 或 Spring Boot 3.0.x / 3.1.x / 3.2.x / 3.3.x
- Java 8 / 11 / 17 / 21，按测试矩阵选择对应版本
- MySQL 5.7+ / 8.0+、PostgreSQL 15+ 或 MariaDB 10.11+
- Redis 6.x+（可选，用于分布式协调）

## 仓库

[DBGuardian Spring Boot Starter](https://github.com/DBGuardian/DBGuardian-Spring-Boot-Starter) 同时包含 Spring Boot 2 的 `dbguardian-boot2-starter` 和 Spring Boot 3 的 `dbguardian-boot3-starter` 模块。

## 贡献与支持

欢迎通过 Issue、PR 或 Star 参与项目建设，也欢迎分享使用反馈和改进建议。

## 定制化服务

如果你的项目需要以下定制化需求，欢迎联系我们：
- 专用数据库版本支持（如 PostgreSQL、Oracle 等）
- 更多 ORM 框架集成（如 JPA、Hibernate、JdbcTemplate 等）
- 多主库、多从库等复杂架构支持
- 其他定制化功能开发

联系方式：
- QQ：664235822
- 邮箱：664235822@qq.com

## License

MIT

<div align="center">
  <img src="https://github.com/DBGuardian/DBGuardian-doc/blob/main/1780427856871.jpg?raw=true" alt="捐助二维码" width="360" />
</div>
