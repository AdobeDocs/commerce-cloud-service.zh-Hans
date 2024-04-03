---
title: 环境变量
description: 请参阅云基础架构上特定于Adobe Commerce的环境变量列表。
feature: Cloud, Build, Configuration, Deploy
exl-id: bfee2f69-93a6-4d26-bb9e-be8acc5673c3
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 环境变量

云基础架构上的Adobe Commerce允许您分配环境变量以覆盖配置选项。 此 `ece-tools` 包将值设置为 `env.php` 基于以下项的值的文件： [云变量](variables-cloud.md)，在中设置的变量 [!DNL Cloud Console]，和 `.magento.env.yaml` 配置文件。

中的环境变量 `.magento.env.yaml` 文件通过覆盖现有Commerce配置自定义云环境。 如果默认值为 `Not Set`，然后 `ece-tools` 包获取次数 **否** 操作并使用 [!DNL Commerce] 默认值或值 `MAGENTO_CLOUD_RELATIONSHIPS` 配置。 如果设置了默认值，则 `ece-tools` 包用于设置默认值。

环境变量的类型包括：

- [管理员](variables-admin.md) — 变量覆盖项目管理员变量
- [Magento云](variables-cloud.md) — 特定于云基础架构的变量
- 中使用的变量 `.magento.env.yaml` 文件：
   - [全局](variables-global.md) — 变量会影响生成、部署和部署后阶段
   - [生成](variables-build.md) — 变量控制构建操作
   - [部署](variables-deploy.md) — 变量控制部署操作
   - [部署后](variables-post-deploy.md) — 变量控制部署后的操作

变量为 _分层_，这意味着如果变量未被覆盖，则会从父环境继承该变量。

您可以设置 [管理员变量](variables-admin.md) 从 [!DNL Cloud Console] 或使用Adobe Commerce CLI。 您可以从管理其他环境变量 [`.magento.env.yaml`](configure-env-yaml.md) 文件，用于管理所有环境（包括Pro Staging和Production）中的构建和部署操作，而无需支持工单。

>[!TIP]
>
>YAML文件区分大小写，不允许使用制表符。 请注意在整个过程中使用一致的缩进 `.magento.env.yaml` 文件或您的配置可能无法按预期工作。 本文档中的示例和示例文件中使用的示例 _两空间_ 缩进。 使用 [ece-tools validate命令](configure-env-yaml.md#validate-configuration-file) 以检查您的配置。
