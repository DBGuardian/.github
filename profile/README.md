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

两个模块提供相同的核心能力，仅适配的 Spring Boot 版本不同。请根据项目技术栈选择对应模块接入。

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

### 已验证版本组合

参考 `DBGuardian-doc/doc/测试项目规划.md`，当前已完成的组合如下：

- `Java 8 + Spring Boot 2.7 + MyBatis-Plus`
- `Java 11 + Spring Boot 2.7 + MyBatis-Plus`
- `Java 17 + Spring Boot 3.0 + MyBatis-Plus`
- `Java 17 + Spring Boot 3.1 + MyBatis-Plus`
- `Java 17 + Spring Boot 3.2 + MyBatis-Plus`
- `Java 8 + Spring Boot 2.7 + MyBatis`
- `Java 17 + Spring Boot 3.2 + MyBatis`
- `Java 21 + Spring Boot 3.3 + MyBatis-Plus`

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

### 基础配置

#### Spring Boot 2.x

```yaml
spring:
  application:
    name: your-app-name

  main:
    allow-circular-references: true

  datasource:
    allow-degraded-startup: true

    master:
      url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true
      username: root
      password: password
      driver-class-name: com.mysql.cj.jdbc.Driver

    slave:
      url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true
      username: root
      password: password
      driver-class-name: com.mysql.cj.jdbc.Driver

    replication:
      master-host: localhost
      master-port: 3306
      master-user: repl
      master-password: repl_password
      auto-reconnect: true

  redis:
    host: localhost
    port: 6379
```

#### Spring Boot 3.x

```yaml
spring:
  application:
    name: your-app-name

  main:
    allow-circular-references: true

  datasource:
    allow-degraded-startup: true

    master:
      url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true
      username: root
      password: password
      driver-class-name: com.mysql.cj.jdbc.Driver

    slave:
      url: jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=utf8&useSSL=false&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true
      username: root
      password: password
      driver-class-name: com.mysql.cj.jdbc.Driver

    replication:
      master-host: localhost
      master-port: 3306
      master-user: repl
      master-password: repl_password
      auto-reconnect: true

  data:
    redis:
      host: localhost
      port: 6379
```

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
- MySQL 5.7+ / 8.0+
- Redis 6.x+（可选，用于分布式协调）

## 仓库

[DBGuardian Spring Boot Starter](https://github.com/DBGuardian/DBGuardian-Spring-Boot-Starter) 同时包含 Spring Boot 2 的 `dbguardian-boot2-starter` 和 Spring Boot 3 的 `dbguardian-boot3-starter` 模块。

## 贡献与支持

欢迎通过 Issue、PR 或 Star 参与项目建设，也欢迎分享使用反馈和改进建议。

## 定制化服务

如果你的项目需要以下定制化需求，欢迎联系我们：
- 专用数据库版本支持（如 PostgreSQL、Oracle、SQL Server 等）
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
