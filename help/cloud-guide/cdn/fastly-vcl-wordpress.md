---
title: 将请求重新路由到CMS后端
description: 了解如何使用Fastly Edge模块将来自Adobe Commerce存储区的传入请求重新路由到单独的WordPress网站。
feature: Cloud, Configuration, Routes
exl-id: 5bd9c56f-4412-4643-89b6-590a8ec65ac0
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# 将请求重新路由到CMS后端

使用Fastly Edge Module将来自Adobe Commerce存储区的传入请求重新路由到单独的WordPress网站 _其他CMS/后端集成_ 和边缘词典。 您可以按照类似的过程将请求重新路由到其他CMS后端。

使用Fastly Edge模块从管理员创建和上传自定义VCL代码，而不是手动编写VCL代码并使用Fastly API上传它。

>[!NOTE]
>
>我们建议将自定义VCL配置添加到暂存环境中，您可以在其中测试它们，然后再在生产环境中更新Fastly服务配置。

**先决条件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**将请求从Adobe Commerce重新路由到WordPress**：

1. 在暂存或生产环境中启用Fastly边缘模块。

   - 登录到管理员。

   - 导航到 **商店** >设置> **配置** > **高级** > **系统** > **全页缓存** > **Fastly配置** > **高级配置**.

   - 设置值 **Fastly Edge模块** 到 **是**.

   - 保存配置。

1. 标识要重路由到WordPress后端的URL路径。

1. 完成以下任务以配置Fastly服务并创建自定义VCL代码，以便将请求重新路由到WordPress后端。

   - 创建边缘词典，以指定要从Adobe Commerce存储区重路由到后端的路径。

   - 将WordPress后端添加到Fastly服务配置并附加URL重写的请求条件。

   - 配置 _其他CMS/后端集成_ Edge模块，用于处理从Adobe Commerce到WordPress后端的URL重写。

     有关详细说明，请参阅 [Fastly Edge模块 — 其他CMS/后端集成](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) 在 _用于Magento2的Fastly CDN模块_ 文档。

1. 更新Fastly服务配置后，请测试您的Adobe Commerce存储，以确保为WordPress指定的URL请求正确重路由。
