---
title: 工作人员
description: 了解如何在中配置Worker资产 [!DNL Commerce] 应用程序配置文件。
feature: Cloud, Configuration
exl-id: d6816925-5912-45ca-8255-6c307e58542d
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Workers属性

您可以定义工作进程独立于Web实例运行，而无需运行Nginx实例；但是，工作进程使用的网络存储与 [!DNL Commerce] 应用程序。 您不需要在工作线程实例上设置Web服务器（使用Node.js或Go），因为路由器无法将公共请求定向到工作线程。 这使得工作人员实例非常适合后台任务或可能阻止部署的持续运行任务。

## 配置工作人员

辅助进程只能与Pro暂存和生产环境一起使用。 专业集成和入门环境可以选择使用 [CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) 变量。

要在Pro Staging或Production中配置工作程序， [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 并包括以下信息：

- 项目编号
- 环境ID
- 工作人员姓名
- 启动命令

您可以为每个工作进程配置一个进程。 中的基本、通用的Worker配置 `.magento.app.yaml` 文件可能如下所示：

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

此示例定义一个名为的工作人员 `queue`，具有小（大小为S）的资源分配级别，并运行 `php ./bin/magento` 命令。 工作人员 `queue` 然后作为工作进程在每个节点上运行。 如果命令退出，则会自动重新启动。

## 命令和覆盖

此 `commands.start` 使用worker应用程序启动命令时需要键。 您可以使用任何有效的shell命令，但最好使用应用程序的语言。 如果命令由 `start` 键终止，它自动重新启动。

>[!IMPORTANT]
>
>此 `deploy` 和 `post_deploy` 挂钩和 `crons` 命令仅在Web容器上运行，而不会在Worker实例中运行。

### 继承

的定义 `size`， `relationships`， `access`， `disk` 和 `mount`、和 `variables` 除非被明确覆盖，否则属性将由工作进程继承。

以下属性最常用于覆盖 [顶级设置](properties.md)：

- `size` — 为单个后台进程分配更少的资源
- `variables` — 指示应用程序以不同的方式运行

### 计时和排队

虽然每个工作人员排在另一个工作人员队列之后，但以下配置会在中生成一致的两秒时间戳 `var/time.txt` 文件，无论PHP代码内是否处于八秒睡眠状态：

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
