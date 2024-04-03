---
title: 入门级架构
description: 了解Starter架构支持的环境。
feature: Cloud, Paas
exl-id: 03365d32-4eb4-42d4-82a7-771df5e7b3da
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# 入门级架构

您在云基础架构上的Adobe Commerce入门架构最多支持 **四个** 环境，包括 `master` 包含初始项目代码的环境、暂存环境和最多两个集成环境。

所有环境都位于PaaS (Platform as a service)容器中。 这些容器部署在服务器网格上的高度受限的容器中。 这些环境是只读的，可接受从本地工作区推送的分支部署的代码更改。 每个环境都提供一个数据库和Web服务器。

您可以使用喜欢的任何开发和分支方法。 获得对项目的初始访问权限时，创建 `staging` 来自的环境 `master` 环境。 然后，创建 `integration` 环境（通过从分支） `staging`.

## 入门环境体系结构

下图显示了Starter环境的层次关系。

![入门项目的高级视图](../../assets/starter/architecture.png)

## 生产环境

生产环境提供了源代码，用于将Adobe Commerce部署到运行面向公共的单站点和多站点商店的云基础架构。 生产环境使用来自的代码 `master` 分支以配置和启用Web服务器、数据库、配置的服务和应用程序代码。

因为 `production` 环境是只读的，请使用 `integration` 环境以进行代码更改，请从在整个架构中部署 `integration` 到 `staging`，最后转到 `production` 环境。 请参阅 [部署您的商店](../deploy/staging-production.md) 和 [站点启动](../launch/overview.md).

Adobe建议在中进行全面测试 `staging` 分支，然后再推送到 `master` 分支，部署到 `production` 环境。

## 暂存环境

Adobe建议创建一个名为的分支 `staging` 从 `master`. 此 `staging` branch将代码部署到暂存环境，以提供预生产环境以测试代码、模块和扩展、支付网关、运输、产品数据等。 此环境为所有服务提供配置以匹配生产环境，包括Fastly、New Relic APM和搜索。

本指南中的其他部分提供了有关最终代码部署和在安全的暂存环境中测试生产级别交互的说明。 要获得最佳性能和功能测试，请将数据库复制到暂存环境中。

>[!WARNING]
>
>Adobe建议先在暂存环境中测试每个商家和客户的交互，然后再部署到生产环境。 请参阅 [部署您的商店](../deploy/staging-production.md) 和 [测试部署](../test/staging-and-production.md).

## 集成环境

开发人员使用 `integration` 用于开发、部署和测试的环境：

- Adobe Commerce应用程序代码

- 自定义代码

- 扩展

- 服务

**推荐用例：**

集成环境专为有限的测试和开发而设计。 例如，您可以使用集成环境完成以下任务：

- 确保对持续集成(CI)流程所做的更改与云兼容

- 在主页、类别、产品详细信息页面(PDP)、结账和管理员等关键页面上测试关键工作流

要在集成环境中获得最佳性能，请遵循以下最佳实践：

- 限制目录大小

- 仅限使用一位或两位并发用户

- 禁用cron作业并根据需要手动运行

您最多可以 **二** 活动集成环境。 您可以通过以下方式创建集成环境： `staging` 分支。 创建集成环境时，环境名称与分支名称匹配。 集成环境包括Web服务器和数据库。 它并不包含所有服务，例如Fastly CDN和New Relic不可用。

您可以拥有无限数量的非活动分支用于代码存储。 要访问、查看和测试非活动分支，必须激活它

{{enhanced-integration-envs}}

## 生产和暂存技术栈栈

生产和暂存环境包括以下技术。 您可以通过以下方式修改和配置这些技术 [`.magento.app.yaml`](../application/configure-app-yaml.md) 文件。

- Fastly用于HTTP缓存和CDN
- Nginx Web服务器与PHP-FPM通信，一个实例具有多个工作程序
- Redis服务器
- Adobe Commerce 2.2到2.4.3-p2的目录搜索Elasticsearch
- OpenSearch for Adobe Commerce 2.3.7-p3、2.4.3-p2、2.4.4及更高版本的目录搜索
- 出口过滤（出站防火墙）

### 服务

云基础架构上的Adobe Commerce当前支持以下服务：PHP、MySQL (MariaDB)、Elasticsearch(Adobe Commerce 2.2到2.4.3-p2)、OpenSearch（2.3.7-p3、2.4.3-p2、2.4.4及更高版本）、Redis和 [!DNL RabbitMQ].

每个服务都在一个单独的安全容器中运行。 容器在项目中一起管理。 某些服务是标准服务，例如：

- HTTP路由器（处理传入请求，以及缓存和重定向）

- PHP应用程序服务器

- Git

- 安全外壳(SSH)

### 软件版本

云基础架构上的Adobe Commerce使用Debian GNU/Linux操作系统和NGINX Web服务器。 您无法升级此软件，但可以配置以下版本：

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

在暂存和生产环境中，您可以使用Fastly进行CDN和缓存。 最新版本的Fastly CDN扩展将在项目初始配置期间安装。 您可以升级扩展以获取最新的错误修复和改进。 请参阅 [用于Magento2的Fastly CDN模块](https://github.com/fastly/fastly-magento2). 此外，您还有权访问 [New Relic](../monitor/account-management.md) 用于性能监控。

使用以下文件配置要在实施中使用的软件版本。

- [&#39;.magento.app.yaml&#39;](../application/configure-app-yaml.md)

- [&#39;路由.yaml&#39;](../routes/routes-yaml.md)

- [&#39;services.yaml&#39;](../services/services-yaml.md)

### 备份和灾难恢复

您可以使用创建数据库和文件系统的备份 [!DNL Cloud Console] 或CLI。 请参阅 [备份管理](../storage/snapshots.md).

## 准备开发

以下工作流汇总了生成代码分支、开发和部署存储的过程：

1. 设置您的本地环境

1. 克隆 `master` 分支到您的本地环境

1. 创建 `staging` 分支自 `master`

1. 创建分支以进行开发，从 `staging`

1. 将代码推送到Git，以便生成和部署到环境以进行测试

有关开发、测试和部署存储区的详细说明和演练，请参阅以下部分：

- [入门开发和部署工作流程](starter-develop-deploy-workflow.md)

- [Docker开发](../dev-tools/cloud-docker.md) （由Cloud Docker for Commerce启用的本地开发环境）

- [管理分支](../project/console-branches.md)

- [部署您的商店](../deploy/staging-production.md)

- [站点启动](../launch/overview.md)
