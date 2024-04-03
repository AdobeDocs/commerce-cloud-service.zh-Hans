---
title: 设置MySQL服务
description: 了解如何在云基础架构上使用Adobe Commerce管理用于永久数据存储的MySQL服务。
feature: Cloud, Services, Storage
exl-id: 70820d00-8b82-4b60-87e4-ea98fd7ffcb2
source-git-commit: 9be8d1e062ab3dba86bc4d9c9ee8b8ece33d5b75
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# 设置MySQL服务

此 `mysql` 服务提供持久性数据存储，基于 [MariaDB](https://mariadb.com/) 版本10.2到10.4，支持 [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) 并重新实现了MySQL 5.6和5.7中的功能。

与其他MariaDB或MySQL版本相比，在MariaDB 10.4上重新索引需要更多时间。 请参阅 [索引器](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) 在 _性能最佳实践_ 指南。

>[!WARNING]
>
>将MariaDB从版本10.1升级到10.2时要小心。MariaDB 10.1是支持的最新版本 _XtraDB_ 作为存储引擎。 MariaDB 10.2使用 _InnoDB_ 用于存储引擎。 从10.1升级到10.2后，无法回滚更改。 Adobe Commerce支持这两个存储引擎；但是，您必须检查项目使用的扩展和其他系统，以确保它们与MariaDB 10.2兼容。请参阅 [10.1和10.2之间的不兼容更改](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**启用MySQL**：

1. 将所需的名称、类型和磁盘值（以MB为单位）添加到 `.magento/services.yaml` 文件。

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >MySQL错误，例如 `PDO Exception: MySQL server has gone away`中，可能会因为磁盘空间不足而发生。 验证是否已为中的服务分配足够的磁盘空间 [`.magento/services.yaml`](services-yaml.md#disk) 文件。

1. 在中配置关系 `.magento.app.yaml` 文件。

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships).

{{service-change-tip}}

## 配置MySQL数据库

配置MySQL数据库时，有以下选项：

- **`schemas`** — 模式定义数据库。 默认架构为 `main` 数据库。
- **`endpoints`** — 每个端点表示具有特定权限的凭据。 默认终结点为 `mysql`，具有 `admin` 访问 `main` 数据库。
- **`properties`** — 属性用于定义其他数据库配置。

以下是中的基本示例配置 `.magento/services.yaml` 文件：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

此 `properties` 在上例中，修改了 `optimizer` 设置为 [性能最佳实践指南中推荐](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**MariaDB配置选项**：

| 选项 | 描述 | 默认值 |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | 默认字符集。 | utf8mb4 |
| `default_collation` | 默认归类。 | utf8mb4_unicode_ci |
| `max_allowed_packet` | 数据包的最大大小（以MB为单位）。 Range `1` 到 `100`. | 16 |
| `optimizer_switch` | 设置查询优化器的值。 请参阅 [MariaDB文档](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | 选择优化程序使用的统计信息。 Range `1` 到 `5`. 请参阅 [MariaDB文档](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 for 10.4及更高版本 |

### 设置多个数据库用户

或者，您可以设置多个具有不同权限的用户来访问 `main` 数据库。

默认情况下，有一个名为的端点 `mysql` 具有数据库管理员访问权限的用户。 要设置多个数据库用户，必须在 `services.yaml` 文件并在中声明关系 `.magento.app.yaml` 文件。 对于专业暂存和生产环境， [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 以请求附加用户。

使用嵌套数组定义特定用户访问的端点。 每个端点可以指定对一个或多个架构（数据库）的访问以及对每个架构的不同权限级别。

有效的权限级别为：

- `ro`：仅允许SELECT查询。
- `rw`：允许SELECT查询和INSERT、UPDATE和DELETE查询。
- `admin`：允许所有查询，包括DDL查询（CREATE TABLE、DROP TABLE等）。

例如：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

在上例中， `admin` 端点提供管理员级别的 `main` 数据库， `reporter` 端点提供只读访问权限，以及 `importer` 端点提供读写访问权限，这意味着：

- 此 `admin` 用户可以完全控制数据库。
- 此 `reporter` 用户仅具有SELECT权限。
- 此 `importer` 用户具有SELECT、INSERT、UPDATE和DELETE权限。

将上例中定义的端点添加到 `relationships` 的属性 `.magento.app.yaml` 文件。 例如：

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>如果配置一个MySQL用户，则不能使用 [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) 存储过程和视图的访问控制机制。

## 连接到数据库

直接访问MariaDB数据库需要使用SSH登录到远程云环境，然后连接到数据库。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 从检索MySQL登录凭据 `database` 和 `type` 中的属性 [$MAGENTO_云关系](../application/properties.md#relationships) 变量。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在响应中，查找MySQL信息。 例如：

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. 连接到数据库。

   - 首先，使用以下命令：

     ```bash
     mysql -h database.internal -u <username>
     ```

   - 对于Pro，请使用以下命令，其中主机名称、端口号、用户名和密码检索自 `$MAGENTO_CLOUD_RELATIONSHIPS` 变量。

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>您可以使用 `magento-cloud db:sql` 用于连接到远程数据库并运行SQL命令的命令。

## 连接到辅助数据库

>[!IMPORTANT]
>
>此功能仅在Pro Production和Staging群集上可用。

有时，必须连接到辅助数据库以提高数据库性能或解决数据库锁定问题。 如果需要此配置，请使用 `"port" : 3304` 以建立连接。 请参阅 [配置MySQL从属连接的最佳实践](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) 中的主题 _实施最佳实践_ 指南。

## 故障排除

请参阅以下Adobe Commerce支持文章，以获取有关MySQL问题疑难解答的帮助：

- [正在检查慢查询和进程MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [在云上创建数据库转储](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [数据迁移工具故障排除](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Adobe Commerce升级：压缩到动态表2.2.x、从2.3.x到2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
