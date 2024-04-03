---
title: 存储配置管理
description: 了解如何在云基础架构环境的所有Adobe Commerce中管理和同步存储配置设置。
feature: Cloud, Configuration, SCD
exl-id: f2dd876d-24ee-4d47-b9ac-44fcf77b61b5
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# 存储配置管理

存储区的默认配置存储在中 `config.xml` 以获取相应的模块。 当您在Commerce管理员或CLI中更改设置时 `bin/magento config:set` 命令，这些更改会反映在核心数据库中，特别是 `core_config_data` 表格。 这些设置会覆盖存储在中的默认配置。 `config.xml` 文件。

存储设置，请参阅“管理员”中的配置 **商店** > **设置** > **配置** 部分，根据配置类型存储在部署配置文件中：

- `app/etc/config.php` — 用于商店、网站、模块或扩展、静态文件优化以及与静态内容部署相关的系统值的配置设置。 请参阅 [config.php引用](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) 在 _配置指南_.
- `app/etc/env.php` — 系统特定的覆盖和敏感设置的值，这些设置应 _NOT_ 存储在源代码管理中。 请参阅 [env.php参考](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) 在 _配置指南_.

>[!NOTE]
>
>由于云基础架构上的Adobe Commerce仅支持生产和维护模式，因此 **高级** > **开发人员** 部分不可在管理员中访问。 您必须拥有 [环境管理员权限](../project/user-access.md) 完成配置管理任务。 您可以使用配置其他设置 [环境变量](../environment/configure-env-yaml.md).

配置管理提供了一种使用Pipeline部署以最小的停机时间跨环境部署一致存储设置的方法。 Adobe Commerce on cloud infrastructure项目包括使用设计的生成服务器、生成和部署脚本以及部署环境 [管道部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) 心里想着。

## 配置覆盖方案

所有系统配置都根据以下覆盖方案在构建和部署阶段进行设置：

1. 如果存在环境变量，请使用自定义配置并忽略默认配置。
1. 如果环境变量不存在，则使用来自的配置 `MAGENTO_CLOUD_RELATIONSHIPS` 中的名称 — 值对 [`.magento.app.yaml` 文件](../application/configure-app-yaml.md). 忽略默认配置。
1. 如果环境变量不存在并且 `MAGENTO_CLOUD_RELATIONSHIPS` 不包含名称 — 值对，请删除所有自定义配置并使用默认配置中的值。

总之，环境变量将覆盖所有其他值。

>[!TIP]
>
>请参阅 [配置管理](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) 在 _配置指南_ 了解有关管道部署的覆盖方案的更多信息。

如果在多个位置配置了相同的设置，则应用程序将依靠以下配置层次结构来确定将哪个值应用于环境：

| 优先级 | 配置<br>方法 | 描述 |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>环境变量 | 值添加自 _变量_ 中的环境配置选项卡 [!DNL Cloud Console]. 在此处为敏感配置或特定于环境的配置指定值。 无法从管理员编辑此处指定的设置。 请参阅 [环境配置变量](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | 在中添加的值 `variables` 的部分 `.magento.app.yaml` 文件。 在此处指定值可确保在所有环境中进行一致的配置。 **请勿在 `.magento.app.yaml` 文件。** 请参阅 [应用程序设置](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | 使用添加此处存储的特定于环境的配置值 `app:config:dump` 命令。 使用环境变量或CLI设置特定于系统的敏感值。 请参阅 [敏感数据](#sensitive-data). 此 `env.php` 文件为 **非** 包含在源代码控制中。 |
| 4 | `app/etc/config.php` | 此处存储的值通过使用 `app:config:dump` 命令。 共享配置值将添加到 `config.php`. 从管理员或使用CLI设置共享配置。 此 `config.php` 文件包含在源代码控制中。 |
| 5 | 数据库 | 通过在管理员中设置配置来添加此处存储的值。 使用以上任何方法设置的配置将被锁定（灰显），并且无法从管理员中进行编辑。 |
| 6 | `config.xml` | 许多配置在 `config.xml` 模块的文件。 如果Adobe Commerce找不到通过以上任何方法设置的任何值，则会回退到默认值（如果已设置）。 |

{style="table-layout:auto"}

## 配置转储

您可以使用以下项 `ece-tools` 命令以生成 `config.php` 包含所有当前存储配置的文件：

```bash
./vendor/bin/ece-tools config:dump
```

数据“转储”到 `app/etc/config.php` 文件变为 _已锁定_，这意味着商务管理员中的相应字段将变为 **只读**. 此 `config.php` 文件仅包含您配置的设置。 它不会锁定默认值。 仅锁定您更新的值还可以确保暂存环境和生产环境中使用的所有扩展不会由于只读配置（尤其是Fastly）而中断。

>[!WARNING]
>
>此 `ece-tools config:dump` 命令不会检索模块的详细配置，如B2B。 如果您需要全面的配置转储，请使用 `app:config:dump` 命令，但此命令以只读状态锁定配置值。

### 敏感数据

任何敏感配置都会导出到 `app/etc/env.php` 文件(当您使用 `bin/magento app:config:dump` 命令。 可以使用CLI命令设置敏感值： `bin/magento config:sensitive:set`. 请参阅  [敏感和特定于环境的设置](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) 在 _Commerce PHP扩展_ 指南以了解如何将配置设置指定为敏感设置或特定于系统。

查看列表 [敏感或系统特定的设置](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) 在 _配置指南_.

### SCD性能

根据存储的大小，您可能要部署大量静态内容文件。 通常，当应用程序处于维护模式时，静态内容会在部署阶段进行部署。 最佳配置是在构建阶段生成静态内容。 请参阅 [选择部署策略](../deploy/static-content.md).

如果在转储配置后启用了配置管理，则应将SCD_*变量从部署阶段移动到构建阶段，以便在构建阶段正确启用静态内容生成。 请参阅 [环境变量](../environment/configure-env-yaml.md#environment-variables).

**配置管理之前**：

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

**启用配置管理后**：

将SCD_*变量移至构建阶段：

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```

>[!NOTE]
>
>在部署静态文件之前，构建和部署阶段使用GZIP压缩静态内容。 压缩静态文件可减少服务器负载并提高站点性能。 请参阅 [生成选项](../environment/variables-build.md) 了解有关自定义或禁用文件压缩的信息。

## 管理设置的过程

下面说明了此过程的高级概述：

![入门级配置管理概述](../../assets/starter/configuration-management-flow.png)

**配置存储并生成配置文件**：

1. 在管理员中为您的商店完成以下环境之一的所有配置：

   - 入门：活跃的开发分支
   - Pro：集成环境中的活动分支

   除非您计划将数据库从此环境转储到暂存环境和生产环境，否则这些配置不包括实际产品。 通常，开发数据库不包含您的完整存储数据。

1. 在本地工作站上，转到您的项目目录。

1. 创建远程数据库的本地转储。

   ```bash
   magento-cloud db:dump
   ```

1. 添加、提交和推送代码更改以更新远程环境。

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

部署完成后，登录到已更新环境的管理员以验证设置。 根据需要继续将任何其他配置合并到暂存环境和生产环境。

### 更新配置

当您通过管理员修改环境并再次运行命令时，新配置将附加到中的代码中 `config.php` 文件。

>[!WARNING]
>
>虽然您可以手动编辑 `config.php` 暂存环境和生产环境中的文件，它是 **非** 推荐。 该文件有助于使所有环境中的所有配置保持一致。 从不删除 `config.php` 文件重建它。 删除文件可能会删除生成和部署过程所需的特定配置和设置。

### 还原配置文件

原始文件的副本 `app/etc/env.php` 和 `app/etc/config.php` 文件是在部署过程中创建的，并存储在同一文件夹中。 下面显示了BAK（备份文件）和PHP（原始文件） `app/etc` 文件夹：

```terminal
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

较旧的配置使用了 `app/etc/config.local.php` 文件。 请参阅 [迁移旧配置](#migrate-older-configurations).

**恢复配置文件**：

1. 在本地工作站上，使用SSH登录到远程项目和环境。

   ```bash
   magento-cloud ssh
   ```

1. 验证备份文件的位置和可用性。

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   示例响应：

   ```terminal
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. 还原备份文件。

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### 迁移旧配置

如果您在云基础架构2.2或更高版本上升级到Adobe Commerce，则可能需要从 `config.local.php` 文件到您的新 `config.php` 文件。 如果管理员中的配置设置与文件的内容匹配，请按照说明生成并添加 `config.php` 文件。

如果两者不同，您可以附加来自 `config.local.php` 文件到您的新 `config.php` 文件：

1. 按照说明生成 `config.php` 文件。

1. 打开 `config.php` 并删除最后一行。

1. 打开 `config.local.php` 并复制其内容。

1. 将内容粘贴到 `config.php` 文件，保存并完成将其添加到Git。

1. 跨环境部署。

您只需完成一次此迁移。 迁移后，使用 `config.php` 文件。

### 更改区域设置

您可以更改存储区域设置，而无需遵循复杂的配置导入和导出过程， _如果_ 您有 [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) 已启用。 您可以使用管理员更新区域设置。

通过启用，可以向暂存或生产环境添加其他区域设置 `SCD_ON_DEMAND` 在集成分支中，生成更新的 `config.php` 使用新的区域设置信息创建文件，并将配置文件复制到目标环境。

>[!WARNING]
>
>此流程 **覆盖** 存储配置；仅当环境包含相同的存储时，才执行以下操作。

1. 在集成环境中，启用 `SCD_ON_DEMAND` 变量使用 [`.magento.env.yaml` 文件](../environment/configure-env-yaml.md).

1. 使用您的管理员添加必要的区域设置。

1. 使用SSH登录到远程环境并生成 `/app/etc/config.php` 包含所有区域设置的文件。

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. 将新的配置文件从远程集成环境复制到本地环境目录。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 添加、提交和推送代码更改以更新远程环境。
