---
title: 存储选项和配置管理概述
description: 在云基础架构上自定义您的Adobe Commerce商店。
feature: Cloud, Configuration, Services
exl-id: 06d477e4-02de-4742-8495-541458400e93
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 0%

---

# 存储选项和配置管理概述

自定义存储的方法有很多，例如添加自定义主题、安装扩展或跨云基础架构环境强制实施特定配置。 您可以直接在暂存环境和生产环境中为特定服务配置设置。 您可以设置多个网站和商店。 应用商店配置可帮助您在本地工作站中配置这些选项，并跨环境部署特定设置。

要访问店面，请使用`magento-cloud url`命令并回答提示。 或者，您可以在[!DNL Cloud Console]中的&#x200B;**访问站点**&#x200B;下找到URL。

## 配置商店选项

存储选项包括：

* [Business-to-business模块(B2B)](b2b-module.md)
* [自定义主题](custom-theme.md)
* [扩展](extensions.md)
* [多个站点](multiple-sites.md)
* [支付服务](paypal.md)

## 配置服务和集成

有特定的[配置文件](../environment/overview.md)管理对远程环境的特定部署行为。 您可以单独查看这些主题：

* [应用程序部署](../application/configure-app-yaml.md)
* [环境构建和部署操作](../environment/configure-env-yaml.md)
* [传入请求路由](../routes/routes-yaml.md)
* [支持的服务](../services/services-yaml.md)

## 配置管理

在配置存储选项、服务和集成后，使用配置管理以一致的方式在所有环境中部署这些配置，并将停机时间减至最少。 请参阅[配置管理](store-settings.md)。
