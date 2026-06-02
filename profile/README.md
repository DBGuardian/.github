<div align="center">
  <img src="https://github.com/DBGuardian/DBGuardian-doc/blob/main/logo.png?raw=true" alt="DBGuardian Logo" width="220" />

  # DBGuardian

  ![开发中](https://img.shields.io/badge/status-%E5%BC%80%E5%8F%91%E4%B8%AD-orange)
  ![GitHub stars](https://img.shields.io/github/stars/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![GitHub forks](https://img.shields.io/github/forks/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![GitHub issues](https://img.shields.io/github/issues/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
  ![GitHub license](https://img.shields.io/github/license/DBGuardian/DBGuardian-Spring-Boot-Starter?style=flat-square)
</div>

DBGuardian 是一套面向 Spring Boot 应用的数据库高可用解决方案，专注于读写分离、自动故障转移、分布式协调与降级启动能力，帮助业务在数据库异常时保持更高的可用性与连续性。

## 项目简介

DBGuardian 目前提供两个 Starter 版本，分别面向 Spring Boot 2 与 Spring Boot 3 场景：

- `DBGuardian-Spring-Boot-Starter`：适用于 Spring Boot 2.7.x
- `DBGuardian-Spring-Boot3-Starter`：适用于 Spring Boot 3.0.x

两者都提供相同的核心能力，只是适配的 Spring Boot 版本不同。你可以根据项目技术栈选择对应版本接入。

## 核心能力

- **读写分离**：自动识别读写操作，将查询路由到从库、写操作路由到主库
- **自动故障转移**：主库异常时自动切换，从而降低服务中断风险
- **分布式协调**：基于 Redis 实现多实例状态同步与协同控制
- **健康检查**：持续检测主从数据库可用性，及时感知故障
- **原主库恢复**：支持原主库恢复后自动作为从库继续追赶数据
- **降级启动**：数据库暂时不可用时，允许应用以降级模式启动

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

- Spring Boot 2.7.x 或 Spring Boot 3.0.x
- MySQL 5.7+ / 8.0+
- Redis 6.x+（可选，用于分布式协调）

## 相关仓库

- [DBGuardian Spring Boot Starter](https://github.com/DBGuardian/DBGuardian-Spring-Boot-Starter)
- [DBGuardian Spring Boot 3 Starter](https://github.com/DBGuardian/DBGuardian-Spring-Boot3-Starter)

## 贡献与支持

欢迎通过 Issue、PR 或 Star 参与项目建设，也欢迎分享使用反馈和改进建议。

## License

MIT

<div align="center">
  <img src="https://github.com/DBGuardian/DBGuardian-doc/blob/main/1780427856871.jpg?raw=true" alt="捐助二维码" width="360" />
</div>