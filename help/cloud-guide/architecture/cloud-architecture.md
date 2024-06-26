---
title: Commerce云架构
description: 了解Starter和Pro项目架构如何与Cloud基础架构上的Commerce形成对比。
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 37cd6733-c10a-4d06-b784-171da576f9fc
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Commerce云架构

云基础架构上的Adobe Commerce有一个入门和专业计划。 每个计划都有一个独特的架构来推动您的Adobe Commerce开发和部署过程。 Starter计划和Pro计划体系结构都跨多个环境部署数据库、Web服务器和缓存服务器以进行端到端测试，同时支持持续集成。

为了进行比较，每个计划都包括以下基础架构功能和支持的产品。

|          | 起始者 | Pro |
| -------- | --------------------| ------------------ |
| 核心功能 | <ul><li>[所有Adobe Commerce功能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal载入工具</li><li>[Commerce报表](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[所有Adobe Commerce功能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal载入工具</li><li>[Commerce报表](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B模块](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| 基础架构和部署 | <ul><li>具有无限用户的持续云集成工具</li><li>Fastly Content Delivery Network (CDN)、图像优化(IO)，以及宽带宽许可带来的安全性。 Web应用程序防火墙(WAF)服务仅在生产环境中可用。</li><li>[New Relic](../monitor/new-relic-service.md) 3个分支上的APM（性能监控）： `master` 和您选择的2个<br>针对Adobe Commerce优化的Platform as a service (PaaS)生产、暂存和开发环境（总共4个活动环境）</li><li>出口过滤（出站防火墙）</li></ul> | <ul><li>具有无限用户的持续云集成工具</li><li>Fastly Content Delivery Network (CDN)、图像优化(IO)，以及宽带宽许可带来的安全性。 Web应用程序防火墙(WAF)服务仅在生产环境中可用。</li><li>[New Relic](../monitor/new-relic-service.md) 生产上的基础架构+暂存和生产上的APM（性能监控）。 此 [受管警报策略](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) for Adobe Commerce策略实施了监控最佳实践，以主动通知您有关影响站点性能的应用程序和基础架构问题。</li><li>基于Platform as a service (PaaS) [集成开发](pro-architecture.md#integration-environment) 针对Adobe Commerce优化的环境（共2个活动环境）</li><li>基础架构即服务(IaaS) — 用于暂存和生产环境的专用虚拟基础架构</li></ul> |
| 高可用性基础架构 | | [高可用性体系结构](pro-architecture.md#redundant-hardware) 在基础基础架构即服务(IaaS)中设置三台服务器，以提供企业级可靠性和可用性 |
| 专用硬件 | | 底层基础架构即服务(IaaS)中的独立专用硬件，可提供更高级别的可靠性和可用性 |
| 全天候电子邮件支持 | 对核心应用程序和云基础架构的全天候监控和电子邮件支持 | 对核心应用程序和云基础架构的全天候监控和电子邮件支持 |
| 专职客户技术顾问(CTA) | | 初始启动期间的专门技术帐户管理，从您的订阅开始，直到您的初始站点启动为止 |
| 加载项\* | <ul><li>[B2B模块](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _可额外收费_

## 入门项目

此 [入门计划体系结构](starter-architecture.md) 有四个环境：

- **集成** — 集成环境提供了两个可测试的环境。 每个环境都包含一个活动的Git分支、数据库、Web服务器、缓存、某些服务、环境变量和配置。

- **暂存** — 当代码和扩展通过测试时，您可以合并 `integration` 分支到暂存环境，该环境将成为您的预生产测试环境。 它包括 `staging` 活动分支、数据库、Web服务器、缓存、第三方服务、环境变量、配置和服务，例如Fastly和New Relic。

- **生产** — 当代码准备就绪并进行测试时，所有代码将合并到 `master` 用于部署到生产实时站点。 此环境包含您的活动 `master` 分支、数据库、Web服务器、缓存、第三方服务、环境变量和配置。

- **不活动** — 您有无限数量的不活动分支。

## Pro项目

此 [专业计划体系结构](pro-architecture.md) 具有全局 `master` 具有三个环境：

- **集成** — 集成环境提供了一个可测试的环境，其中包括数据库、Web服务器、缓存、某些服务、环境变量和配置。 在合并到暂存环境之前，您可以开发、部署和测试代码。

   - _不活动_ — 您可以根据 `integration` 环境，但只有一个活动分支(不包括 `integration` )。

- **暂存** — 暂存环境用于预生产测试，包括数据库、Web服务器、缓存、第三方服务、环境变量、配置和服务，如Fastly。

- **生产** — 生产环境包括一个用于您的数据、服务、缓存和存储的三节点高可用性体系结构。 生产环境是您的实时公共存储环境，具有环境变量、配置和第三方服务。

## 支持的软件和服务

云基础架构上的Adobe Commerce使用：

- 操作系统：Debian GNU/Linux
- Web服务器：Nginx
- 数据库：MySQL (MariaDB)
- 内容交付网络(CDN)： Fastly CDN

您可以配置以下服务：

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>请参阅 [系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 在 _安装指南_ 推荐的版本。

Fastly CDN模块用于暂存和生产环境中的CDN和缓存服务。 请参阅 [配置Fastly服务](../cdn/fastly.md).

有关配置要在实施中使用的软件版本的信息，请参阅以下Adobe Commerce on cloud infrastructure配置文件：

- [应用程序配置(.magento.app.yaml)](../application/configure-app-yaml.md)
- [环境配置(.magento.env.yaml)](../environment/configure-env-yaml.md)
- [路由配置(routes.yaml)](../routes/routes-yaml.md)
- [服务配置(services.yaml)](../services/services-yaml.md)
