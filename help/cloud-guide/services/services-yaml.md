---
title: 配置服务
description: 了解如何在云基础架构上配置Adobe Commerce使用的服务。
feature: Cloud, Configuration, Services
exl-id: 48091c10-c53f-4aad-afbe-b4516653bda1
source-git-commit: 4d790bff2ba5d02ef10de5c36a2f0d140e31a407
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 0%

---

# 配置服务

此 `services.yaml` 文件定义Adobe Commerce在云基础架构上支持和使用的服务，例如MySQL、Redis以及Elasticsearch或OpenSearch。 您无需订阅外部服务提供商。 此文件位于 `.magento` 项目目录。

部署脚本使用 `.magento` 目录，以使用配置的服务配置环境。 如果服务包含在 [`relationships`](../application/properties.md#relationships) 的属性 `.magento.app.yaml` 文件。 此 `services.yaml` 文件包含 _type_ 和 _磁盘_ 值。 服务类型定义服务 _name_ 和 _版本_.

更改服务配置会导致部署为环境提供更新的服务，这会对以下环境造成影响：

- 所有入门环境（包括生产） `master`
- 专业集成环境

{{pro-update-service}}

## 默认服务和支持的服务

云基础架构支持和部署以下服务：

- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

您可以查看当前版本中的默认版本和磁盘值， [默认 `services.yaml` 文件](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml). 以下示例显示了 `mysql`， `redis`， `opensearch` 或 `elasticsearch`、和 `rabbitmq` 中定义的服务 `services.yaml` 配置文件：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## 服务值

您必须提供服务ID和服务类型配置 `type: <name>:<version>`. 如果服务使用永久存储，则必须提供磁盘值。

使用以下格式：

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

此 `service-id` 值标识项目中的服务。 您只能使用小写字母数字字符： `a` 到 `z` 和 `0` 到 `9`，例如 `redis`.

此 _service-id_ 值用于 [`relationships`](../application/properties.md#relationships) 的属性 `.magento.app.yaml` 配置文件：

```yaml
relationships:
    redis: "<name>:redis"
```

您可以命名每种服务类型的多个实例。 例如，您可以使用多个Redis实例 — 一个用于会话，一个用于缓存。

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

重命名中的服务 `services.yaml` 文件 **永久删除** 以下各项：

- 使用您指定的新名称创建服务之前的现有服务。
- 服务的所有现有数据都会被删除。 Adobe强烈建议您 [备份您的入门环境](../storage/snapshots.md) 更改现有服务的名称之前。

### `type`

此 `type` 值指定服务名称和版本。 例如：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

此 `disk` 值指定要分配给服务的永久磁盘存储的大小（以MB为单位）。 使用永久存储的服务（如MySQL）必须提供磁盘值。 使用内存而不是永久存储的服务（如Redis ）不需要磁盘值。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

每个项目的当前默认存储量为5 GB，即512 0 MB。 您可以在应用程序及其各项服务之间分配此金额。

## 服务关系

在Adobe Commerce中关于云基础架构项目、服务 [关系](../application/properties.md#relationships) 在中配置 `.magento.app.yaml` 文件确定应用程序可用的服务。

您可以从以下位置检索所有服务关系的配置数据 [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md) 环境变量。 配置数据包括服务名称、类型和版本，以及任何所需的连接详细信息，如端口号和登录凭据。

**验证本地环境中的关系**：

1. 在本地环境中，显示活动环境的关系。

   ```bash
   magento-cloud relationships
   ```

1. 确认 `service` 和 `type` 从响应中。 响应提供连接信息，如IP地址和端口号。

   >缩写示例响应

   ```terminal
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**验证远程环境中的关系**：

1. 使用SSH登录到远程环境。

1. 列出环境中配置的所有服务的关系配置数据。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或者，使用以下方法 `ece-tools` 用于查看关系的命令：

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 确认 `service` 和 `type` 从响应中。 响应提供连接信息，如IP地址和端口号以及任何所需的用户名和密码凭据。

## 服务版本

云基础架构上Adobe Commerce的服务版本和兼容性支持取决于在云基础架构上部署和测试的版本，有时与Adobe Commerce内部部署支持的版本不同。 请参阅 [系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 在 _安装_ Adobe已在特定Adobe Commerce和Magento Open Source版本中测试的第三方软件依赖项列表。

### 软件EOL检查

在部署过程中， `ece-tools` 软件包会根据每项服务的生命周期结束(EOL)日期检查已安装的服务版本。

- 如果服务版本在EOL日期后的三个月内，则部署日志中会显示通知。
- 如果EOL日期是过去的时间，则会显示警告通知。

为维护存储安全性，请在已安装的软件版本达到EOL之前更新这些版本。 您可以在 [ece工具 `eol.yaml` 文件](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### 迁移到OpenSearch

{{elasticsearch-support}}

有关Adobe Commerce版本2.4.4及更高版本，请参阅 [设置OpenSearch服务](opensearch.md).

## 更改服务版本

您可以升级已安装的服务版本，以便与云环境中部署的Adobe Commerce版本兼容。

不能直接降级已安装服务的服务版本。 但是，您可以使用所需的版本创建服务。 请参阅 [降级服务版本](#downgrade-version).

### 升级已安装的服务版本

您可以通过更新中的服务配置来升级已安装的服务版本 `services.yaml` 文件。

1. 更改 [`type`](#type) 中服务的值 `.magento/services.yaml` 文件：

   > 原始服务定义

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > 更新了服务定义

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### 降级版本

不能直接降级已安装的服务。 您有两个选项：

1. 使用新版本重命名现有服务，这将删除现有服务和数据，并添加新服务和数据。

1. 创建服务并从现有服务中保存数据。

更改服务版本时，必须在以下位置更新服务配置： `services.yaml` 文件，并更新 `.magento.app.yaml` 文件。

**通过重命名现有服务来降级服务版本**：

1. 重命名中的现有服务 `.magento/services.yaml` 文件并更改版本。

   >[!WARNING]
   >
   >重命名现有服务会替换现有服务并删除所有数据。 如果需要保留数据，请创建一个服务，而不是重命名现有服务。

   例如，要降级的MariaDB版本 _mysql_ 服务从10.4版更改为10.3版，更改现有 _service-id_ 和 _type_ 配置。

   > 原有 `services.yaml` 定义

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 新建 `services.yaml` 定义

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. 更新中的关系 `.magento.app.yaml` 文件。

   > 原有 `.magento.app.yaml` 配置

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 已更新 `.magento.app.yaml` 配置

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 添加、提交和推送代码更改。

**通过创建服务来降级服务**：

1. 将服务定义添加到 `services.yaml` 用于具有降级版本规范的项目文件。 请参阅 _mysql2_ 在以下示例中：

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. 更改中的关系配置 `.magento.app.yaml` 文件以使用新服务。

   > 原有 `.magento.app.yaml` 配置

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 新建 `.magento.app.yaml` 配置

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 添加、提交和推送代码更改。
