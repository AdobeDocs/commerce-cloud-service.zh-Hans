---
title: Cloud Tools Suite发行说明
description: 了解适用于Adobe Commerce的Cloud Tools套件的最新改进。
feature: Cloud, Release Notes
exl-id: 6a652e93-46a2-4590-97fc-fb5d114ece9a
source-git-commit: 2ce77fdffbc2da51c2dd736ec992637aa421487e
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Commerce Cloud Tools Suite发行说明

此发行信息详细介绍了Cloud Tools Suite for Commerce包的最新改进，这些改进旨在部署和管理Cloud平台上的Adobe Commerce安装和升级。

| 发行说明 | 版本 | 描述 | 来源 |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` 包](ece-tools-package.md) | 2002.1.17 | 一组用于管理和部署云项目的脚本和工具 | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.1) |
| [Commerce云修补程序](cloud-patches.md) | 1.0.25 | 一组修补程序，用于改进所有Adobe Commerce版本与Cloud环境的集成。 此软件包包含Adobe Commerce修补程序以及在您使用时应用的可用修补程序 `ece-tools` 待部署 | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.0.1) |
| [Cloud Docker for Commerce](cloud-docker.md) | 1.3.6 | Docker映像将Adobe Commerce部署到本地云环境的功能和配置文件 | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.0) |
| [Commerce云组件](cloud-components.md) | 1.0.13 | 为部署在云基础架构上的站点扩展了Adobe Commerce核心功能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.0.2) |

当您更新到ECE-Tools 2002.1.0或更高版本时，您会自动更新到其他软件包的最新版本，这些软件包是 `ece-tools` 包。 请参阅 [云中继](../development/overview.md#cloud-metapackage) 以获取依赖关系列表。
