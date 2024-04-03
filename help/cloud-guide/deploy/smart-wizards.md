---
title: 智能向导
description: 了解如何使用智能向导来评估您在云基础架构项目上的Adobe Commerce是否遵循部署最佳实践。
feature: Cloud, Build, Deploy, SCD
exl-id: eb79431c-8835-4ae4-b453-9c4932c5d5ac
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 智能向导

智能向导可帮助您确定云配置是否遵循最佳实践。 可用的向导有助于进行以下配置：

- 最少的部署停机时间的理想状态
- 数据库和Redis的负载平衡配置
- 用于按需、构建阶段或部署阶段的静态内容部署(SCD)

每个Smart Wizard命令都会提供验证响应，并在适用的情况下提供正确配置的建议。

| 命令 | 描述 |
| ------- | ------------|
| `wizard:ideal-state` | 检查SCD是否位于 _生成_ 阶段， `SKIP_HTML_MINIFICATION` 变量为 `true`，以及在云环境中配置的post_deploy挂接。 不用于本地开发环境。 |
| `wizard:master-slave` | 检查 `REDIS_USE_SLAVE_CONNECTION` 变量和 `MYSQL_USE_SLAVE_CONNECTION` 变量为 `true`. |
| `wizard:scd-on-demand` | 检查 `SCD_ON_DEMAND` 全局环境变量为 `true`. |
| `wizard:scd-on-build` | 检查 `SCD_ON_DEMAND` 全局环境变量为 `false` 和 `SKIP_SCD` 环境变量为 `false` 对于 _生成_ 暂存。 验证 `config.php` 文件包含有关商店、商店组和网站的信息。 |
| `wizard:scd-on-deploy` | 检查 `SCD_ON_DEMAND` 全局环境变量为 `false` 和 `SKIP_SCD` 环境变量为 `false` 对于 _部署_ 暂存。 验证 `config.php` 文件是 _NOT_ 包含商店、商店组和网站的列表以及相关信息。 |

例如，您可以验证配置是否正确启用SCD按需功能：

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

成功的配置将返回：

```terminal
SCD on-demand is enabled
```

失败的配置返回：

```terminal
SCD on-demand is disabled
```

## 验证理想配置

此 _理想_ 云项目的配置可通过预热缓存并在用户请求时生成静态内容来最大程度地缩短部署停机时间。 此向导在部署过程中自动运行。 如果您的云未针对此进行配置 _理想状态_，则会收到类似于以下内容的消息：

```terminal
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

根据输出，您需要对配置进行以下更正：

1. 启用跳过HTML缩小变量。

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. 配置部署后挂接。

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. 推送代码更改并重新运行测试。 当您的配置为 _理想_，您将收到以下消息。

   ```terminal
   Ideal state is configured
   ```
