---
title: 配置应用程序部署
description: 了解如何在应用程序配置文件中配置属性，这些属性控制 [!DNL Commerce] 应用程序构建和部署到云环境的方式。
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# 配置应用程序部署

`.magento.app.yaml`文件控制应用程序构建和部署的方式。 虽然云基础架构上的Adobe Commerce支持每个项目使用多个应用程序，但通常一个项目具有一个应用程序，其中存储库根目录具有`.magento.app.yaml`文件。

`.magento.app.yaml`有许多默认值，请参阅[示例`.magento.app.yaml`文件](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml)。 始终查看已安装版本的`.magento.app.yaml`。 在云基础架构版本上，此文件可能会在Adobe Commerce中有所不同。

使用`.magento.app.yaml`文件定义以下配置值：

- [属性](properties.md) — 定义应用程序实例的属性值。
- [变量属性](variables-property.md) — 查看[!DNL Commerce]应用程序版本所需的环境变量。
- [PHP设置](php-settings.md) — 配置运行时PHP选项。
- [为静态文件设置缓存](set-cache.md) — 为媒体和静态文件设置缓存TTL。
