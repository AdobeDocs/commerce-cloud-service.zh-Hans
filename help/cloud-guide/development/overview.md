---
title: 开发概述
description: 使用Adobe Commerce on cloud基础架构项目为本地开发做准备。
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: d4452d7d-d3dc-4f8d-8bd7-76f05d89f545
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# 开发概述

云基础架构上的Adobe Commerce远程环境包括 **只读**，包括所有入门级环境和所有专业级集成、暂存和生产环境。 在本地开发环境中，您可以先编写和测试代码，然后再将其推送到集成环境，以便进一步测试并部署到暂存和生产环境。

在准备本地工作区之前，请确保您拥有 [凭据](../../get-started/prepare-workspace.md). 除非您选择使用，否则本地开发要求安装PHP和Composer [Cloud Docker for Commerce](#docker-environment).

## 必需的包

云基础架构上的Adobe Commerce使用编辑器管理项目的依赖项和升级。 对于本地开发，必须安装与云项目兼容的PHP和Composer版本。 例如，如果您使用 [!DNL Commerce] 2.4.6云模板，您可以看到 [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.6/.magento.app.yaml) 配置文件使用 **PHP 8.2** 和 **编辑器2.2.21**.

Composer会将您项目所需的库和依赖项安装在中 `vendor` 目录。 以下必需的编辑器文件位于项目根目录中：

- `composer.json` — 使用 `composer.json` 用于管理产品安装和升级的文件。
- `composer.lock` — 此 `composer.lock` file存储一组精确的版本依赖关系，这些依赖关系满足项目依赖关系树中每个软件包的每个要求的版本约束。

**常用命令：**

| 命令 | 描述 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | 更新了最新版本的依赖项，反映在 `composer.json` 文件。 这将更新 `composer.lock` 文件。 |
| `composer install` | 阅读 `composer.lock` 要下载依赖项的文件。 最佳做法是保留以下内容的最新副本： `composer.lock` 在您的项目存储库中。 |

{style="table-layout:auto"}

添加、提交和推送更新的代码后，部署过程将自动运行 `composer install` 命令执行期间 [构建阶段](../deploy/process.md#build-phase-build-phase).

### 云中继

云基础架构上的Adobe Commerce使用一个需要 `magento/product-enterprise-edition`. 要获取最新版本的Commerce的最新更新，请使用以下约束语法：

```text
>=current_version <next_version
```

例如，要使用最新的Adobe Commerce版本2.4.5，请设置 `2.4.5` 作为“当前”版本和 `2.4.6` 作为 `composer.json` 文件：

```text
"magento/magento-cloud-metapackage": ">=2.4.5 <2.4.6"
```

此中继包的主要包如下：

- **vendor/magento/ece-tools** — 此 `ece-tools` 包与Adobe Commerce版本2.1.4及更高版本兼容，以提供一组丰富的功能，可用于管理Adobe Commerce on cloud基础架构项目。 它包含脚本和云基础架构上的Adobe Commerce命令，旨在帮助管理代码并自动构建和部署项目。 请参阅 [`ece-tools` 包概述](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition** — 此元包需要应用程序组件，包括模块、框架、主题等。
- **vendor/fastly2/magento2** — 此模块为Pro暂存和生产以及入门生产环境管理Fastly CDN和服务。 请参阅 [Fastly服务](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **vendor/magento/module-paypal-on-boarding** — 此模块通过连接到您的PayPal商家帐户来提供PayPal支付网关结账。 请参阅 [PayPal入门培训工具](../store/paypal.md).

>[!TIP]
>
>请参阅 [Adobe Commerce云包](/help/cloud-guide/release-notes/cloud-packages.md) 在 _Commerce发行说明_ 以获取依赖项和第三方许可证的列表。

## Docker环境

您可以使用Cloud Docker for Commerce工具在云基础架构生产和开发环境中模拟Adobe Commerce以进行本地开发。 Cloud Docker for Commerce不需要在本地安装PHP和Composer。

- [使用Cloud Docker进行本地开发](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) 在Adobe Developer网站中
- [Docker体系结构和常用命令](../dev-tools/cloud-docker.md)
- [Cloud Docker发行说明](../release-notes/cloud-docker.md)

>[!TIP]
>
>有关在云基础架构上将基于Git的托管服务与Adobe Commerce结合使用的信息，请参阅 [集成](../integrations/overview.md).
