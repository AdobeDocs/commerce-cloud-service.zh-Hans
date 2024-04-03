---
title: 部署过程
description: 了解部署如何适用于Adobe Commerce的云基础架构项目。
feature: Cloud, Build, Deploy, SCD
exl-id: 378fa290-5a71-4ac2-816a-a7c837e45b2f
source-git-commit: 3d9e3294872fedcc43744f54a71c71d8951ed853
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# 部署过程

当您执行合并、推送或同步环境时，或者当您触发 [手动重新部署](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). 部署过程需要时间，但优化部署的方法取决于您是开发和测试还是使用实时站点。 最值得一提的是，您可以控制 [静态内容部署](static-content.md).

部署过程分为三个不同的阶段：构建、部署和部署后。 每个阶段使用有限的资源执行特定操作：

## ![构建阶段](../../assets/status-build.png) 构建阶段

此 _生成_ 阶段为配置文件中定义的服务组装容器，安装依赖项，依赖项基于 `composer.lock` 文件，并运行在中定义的生成挂接 `.magento.app.yaml` 文件。 如果无法连接到任何服务或无法访问数据库，则构建阶段将取决于限制在环境中的资源。

## ![部署阶段](../../assets/status-deploy.png) 部署阶段

此 _部署_ 阶段临时暂挂传入的请求并将站点过渡到 [维护模式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). 部署阶段使用新容器，并在装载文件系统后打开网络连接，激活中定义的服务。 `relationships` 的部分 `.magento.app.yaml` 文件，并运行在中定义的部署挂接 `.magento.app.yaml` 文件。 一切都是 _只读_，中定义的目录除外 `.magento.app.yaml` 文件。 默认情况下， [`mounts` 属性](../application/properties.md#mounts) 包括以下目录：

- `app/etc` — 包含 `env.php` 和 `config.php` 配置文件
- `pub/media` — 包含所有媒体数据，如产品或类别
- `pub/static` — 包含生成的静态文件
- `var` — 包含运行时创建的临时文件

所有其他目录均具有只读权限。 当新站点从维护模式过渡并释放对传入请求的临时保留时，它在部署阶段结束时变为活动状态。

在部署阶段， `app/etc/config.php` 和 `app/etc/env.php` 部署配置文件将使用BAK扩展名进行保存。 请参阅 [商店设置](../store/store-settings.md#restore-configuration-files) 以了解如何恢复这些文件。

## ![部署后阶段](../../assets/status-post-deploy.png) 部署后阶段

此 _部署后_ 阶段运行中定义的部署后挂接 `.magento.app.yaml` 文件。 在此阶段执行任何操作都会影响站点性能；但是，您可以使用 [WARMUP_PAGE](../environment/variables-post-deploy.md#warmuppages) 用于填充缓存的环境变量。

## ![验证状态](../../assets/status-verify.png) 验证配置

您可以通过运行 [智能向导](smart-wizards.md).

>[!NOTE]
>
>替换为 `ece-tools` 2002.1.0及更高版本中，您可以使用基于方案的部署功能在云基础架构项目上自定义Adobe Commerce的构建、部署和部署后流程。 请参阅 [基于方案的部署](scenario-based.md).
