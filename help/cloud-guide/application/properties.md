---
title: 属性
description: 配置时，请使用属性列表作为参考 [!DNL Commerce] 用于构建和部署到云基础架构的应用程序。
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 58a86136-a9f9-4519-af27-2f8fa4018038
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# 应用程序配置的属性

此 `.magento.app.yaml` 文件使用属性来管理对的环境支持 [!DNL Commerce] 应用程序。

| 名称 | 描述 | 默认 | 必填 |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | 自定义用户角色 | — | 否 |
| [`crons`](crons-property.md) | 更新规格并安排cron作业 | — | 否 |
| [`dependencies`](#dependencies) | 启用其他依赖项 | `php:composer/composer: '2.2.4'` | 否 |
| [`disk`](#disk) | 定义永久磁盘大小 | `5120` | 是 |
| [`firewall`](firewall-property.md) | （仅限起始者）控制出站流量 | — | 否 |
| [`hooks`](hooks-property.md) | 自定义生成、部署和部署后阶段的shell命令 | — | 否 |
| [`mounts`](#mounts) | 设置路径 | 路径：<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | 否 |
| [`name`](#name) | 定义应用程序名称 | `mymagento` | 是 |
| [`relationships`](#relationships) | 映射服务 | 服务：<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | 否 |
| [`runtime`](#runtime) | 运行时属性包含 [!DNL Commerce] 应用程序。 | 扩展：<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | 是 |
| [`type`](#type-and-build) | 设置基本容器图像 | `php:8.3` | 是 |
| [`variables`](variables-property.md) | 为特定Commerce版本应用环境变量 | — | 否 |
| [`web`](web-property.md) | 处理外部请求 | — | 是 |
| [`workers`](workers-property.md) | 处理外部请求 | — | 是（如果未使用Web属性） |

{style="table-layout:auto"}

## `name`

此 `name` 属性提供中使用的应用程序名称 [`routes.yaml`](../routes/routes-yaml.md) 文件定义HTTP上游(默认情况下， `mymagento:http`)。 例如，如果 `name` 是 `app`，您必须使用 `app:http` 在上游字段中。

>[!WARNING]
>
>部署应用程序后，请勿更改该应用程序的名称。 这样做会导致数据丢失。

## `type` 和 `build`

此 `type`  和 `build` 属性提供有关用于构建和运行项目的基本容器图像的信息。

支持的 `type` 语言是PHP。 按如下方式指定PHP版本：

```yaml
type: php:<version>
```

此 `build` 属性确定构建项目时默认执行的操作。 此 `flavor` 指定要运行的默认生成任务集。 以下示例显示了的默认配置 `type` 和 `build` 从 `magento-cloud/.magento.app.yaml`：

```yaml
# The toolstack used to build the application.
type: php:8.3
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### 安装和使用Composer 2

此 `build: flavor:` 属性不用于Composer 2.x；因此，您必须在构建阶段手动安装Composer。 要在入门和专业版项目中安装和使用Composer 2.x，您必须对进行三项更改 `.magento.app.yaml` 配置：

1. 移除 `composer` 作为 `build: flavor:` 并添加 `none`. 此更改会阻止Cloud使用默认的1.x版本的Composer来运行构建任务。
1. 添加 `composer/composer: '^2.0'` as a `php` 与安装Composer 2.x的依赖关系。
1. 添加 `composer` 将任务生成到 `build` 挂接以使用Composer 2.x运行构建任务。

在您自己的团队中使用以下配置片段 `.magento.app.yaml` 配置：

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

请参阅 [必需的包](../development/overview.md#required-packages) 以了解有关Composer的详细信息。

## `dependencies`

指定应用程序在构建过程中可能需要的依赖关系。

Adobe Commerce支持对以下语言的依赖性：

- PHP
- Ruby
- Node.js

这些依赖项与应用程序的最终依赖项无关，并且可在 `PATH`，在构建过程和应用程序的运行时环境中。

您可以按如下方式指定这些从属关系：

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

用于在运行时修改PHP配置，如启用扩展。 需要以下扩展：

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

请参阅 [PHP设置](php-settings.md) 以了解有关启用扩展的详细信息。

## `disk`

定义应用程序的永久磁盘大小（以MB为单位）。

```yaml
disk: 5120
```

建议的最小磁盘大小为256 MB。 如果您看到此错误 `UserError: Error building the project: Disk size may not be smaller than 128MB`，将大小增加到256 MB。

>[!NOTE]
>
>对于Pro暂存和生产环境，您必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 更新 `mounts` 和 `disk` 应用程序的配置。 在提交票证时，请指明所需的配置更改并包括的更新版本 `.magento.app.yaml` 文件。

## `relationships`

定义应用程序中的服务映射。

关系 `name` 应用程序可在以下位置使用： `MAGENTO_CLOUD_RELATIONSHIPS` 环境变量。 此 `<service-name>:<endpoint-name>` 关系映射到 `.magento/services.yaml` 文件。

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

以下是默认关系的示例：

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

请参阅 [服务](../services/services-yaml.md) 以获取当前支持的服务类型和端点的完整列表。

## `mounts`

其键是相对于应用程序根目录的路径的对象。 装载是磁盘上文件的可写区域。 以下是在中配置的默认装载列表 `magento.app.yaml` 文件使用 `volume_id[/subpath]` 语法：

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

将装载添加到此列表的格式如下：

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared` — 在环境中的应用程序之间共享卷。
- `disk` — 定义共享卷的可用大小。

>[!NOTE]
>
>对于Pro暂存和生产环境，您必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 更新 `mounts` 和 `disk` 应用程序的配置。 在提交票证时，请指明所需的配置更改并包括的更新版本 `.magento.app.yaml` 文件。

通过将装载Web添加到 [`web`](web-property.md) 位置块。

>[!WARNING]
>
>网站拥有数据后，请勿更改 `subpath` 装载名称的一部分。 此值是的唯一标识符 `files` 区域。 如果更改此名称，则将丢失存储在旧位置的所有站点数据。

## `access`

此 `access` 属性指明允许对环境进行SSH访问的最低用户角色级别。 可用的用户角色包括：

- `admin` — 可以在环境中更改设置并执行操作；具有 _参与者_ 和 _查看者_ 权限。
- `contributor` — 可以将代码推送到此环境并从环境分支；具有 _查看者_ 权限。
- `viewer` — 只能查看环境。

默认用户角色为 `contributor`，以限制来自仅具有下列权限的用户的SSH访问： _查看者_ 权限。 您可以将用户角色更改为 `viewer` 仅允许具有的用户进行SSH访问 _查看者_ 权限：

```yaml
access:
    ssh: viewer
```
