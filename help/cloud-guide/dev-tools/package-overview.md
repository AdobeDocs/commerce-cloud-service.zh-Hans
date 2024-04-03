---
title: ’[!DNL ECE-Tools] 包
description: 了解 [!DNL ECE-Tools] 包以及它如何帮助管理和部署Adobe Commerce。
exl-id: 5583a685-29c5-4de5-8d2e-94cff5ff37ab
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# ECE-Tools包

此 [!DNL ECE-Tools] package是一组脚本和工具，用于管理和部署 [!DNL Commerce] 应用程序。 此 `ece-tools` 包简化了许多流程，例如管理cron作业、验证项目配置以及应用Adobe修补程序和修补程序。 您可以查看并投稿 [开源 [!DNL ECE-Tools] GitHub上的代码存储库][ece-repo].

{{ece-tools-package}}

此 `ece-tools` 包与Adobe Commerce兼容（从版本2.1.4开始），并包含脚本和Adobe Commerce on cloud infrastructure命令，这些命令旨在帮助管理代码并自动构建和部署项目。

下面列出了可用的 `ece-tools` 命令：

```bash
php ./vendor/bin/ece-tools list
```

## 生成和部署

此 `ece-tools` 包中包含一些命令，用于对在云基础架构上启动Adobe Commerce的生成、部署和部署后阶段执行操作。 例如， `php ./vendor/bin/ece-tools build` 命令开始应用程序构建过程。

默认情况下，这些 `ece-tools` 命令位于 [挂接属性](../application/hooks-property.md) 的 `.magento.app.yaml` 配置文件。

## Docker配置生成器

此 `ece-tools` 包中包含的依赖项 [magento/magento-cloud-docker] 包，为Docker图像提供功能和配置文件，以启动云基础架构上Adobe Commerce的Docker开发环境。 您还可以作为独立包运行Cloud Docker for Commerce。 请参阅 [Docker开发](../dev-tools/cloud-docker.md).

## 服务、路由和变量

您可以使用 `ece-tools` 用于显示有关Base64编码的详细信息 [云变量](../environment/variables-cloud.md) 在任何Cloud环境中使用。 以下命令显示所有服务、路由和变量。

```bash
php ./vendor/bin/ece-tools env:config:show
```

要显示一组特定的信息，请使用以下格式：

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` — 显示关系数据，来自 `MAGENTO_CLOUD_RELATIONSHIPS` 环境变量，在中定义 `services.yaml` 文件。
- `routes` — 使用显示项目的已配置路由 `MAGENTO_CLOUD_ROUTES` 环境变量。
- `variables` — 使用显示项目的已配置变量 `MAGENTO_CLOUD_VARIABLES` 环境变量。

的输出示例 `services` 选项：

```terminal
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## 验证环境配置

有一组验证命令可用于帮助评估项目的配置。 请参阅 [智能向导](../deploy/smart-wizards.md) 在 _优化部署_ 部分以了解每个向导命令的详细说明。 此 `wizard:ideal-state` 命令在构建阶段自动运行。 验证项目的理想状态：

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>您必须运行 `wizard:ideal-state` 命令。 该命令始终返回 `The configured state is not ideal` 在本地开发环境中运行时出错。

示例输出：

```terminal
Ideal state is configured
```

请参阅 [ece-tools发行说明](../release-notes/cloud-tools-suite.md).

## Adobe修补程序和自定义修补程序

此 `ece-tools` 包中包含的依赖项 [magento/magento-cloud-patches] 软件包提供Adobe修补程序和修补程序，可改进所有Adobe Commerce版本与Cloud环境的集成，并支持快速交付关键修补程序。 “ ”还提供了自定义修补程序，可将其添加到Adobe Commerce on cloud infrastructure项目。 请参阅 [应用修补程序](../development/apply-patches.md).

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
