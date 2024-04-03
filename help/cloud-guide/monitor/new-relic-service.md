---
title: New Relic服务
description: 了解Adobe Commerce在云基础架构项目中提供的New Relic服务。
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 613f0694-5338-4037-8ee4-ac5eca376159
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# New Relic服务概述

云基础架构上的所有Adobe Commerce项目都包括对New Relic服务的访问，以帮助监控云基础架构的性能并调查其事件。 [!DNL Commerce] 应用程序和云基础架构。

以下New Relic功能可用于生产和暂存环境：

- [NEW RELIC APM](#new-relic-apm) （Pro和Starter）
- [New Relic基础架构](#new-relic-infrastructure) （仅限Pro）
- [New Relic日志管理](#new-relic-logs) （仅限Pro）

>[!INFO]
>
>Adobe Commerce项目上不提供其他New Relic功能。

## NEW RELIC APM

[New Relic应用程序性能管理(APM)](https://docs.newrelic.com/introduction-apm/) 是一款软件分析产品，可帮助您分析和改进应用程序交互。 New Relic APM适用于云基础架构项目上的所有Adobe Commerce，并提供以下功能：

- **专注于特定交易** — 主动标记和监控网站中的关键客户操作，如添加到购物车、结帐或处理付款。
- **数据库查询监测** — 查找并监视影响性能的数据库查询。
- **应用程序映射** — 查看站点内的所有应用程序依赖项、扩展和外部服务。
- **[!DNL Apdex]分数** — 评估性能并创建警报，以识别问题并在发生问题时通知您，例如受快速销售或Web事件影响的网站性能。 请参阅 [Apdex得分](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Adobe Commerce的受管警报** — 使用此New Relic警报策略，根据行业最佳实践监控应用程序和基础架构性能。 请参阅 [使用Adobe Commerce的托管警报策略监控性能](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **跟踪部署** — 监视部署事件并分析部署对整体性能的影响。 请参阅 [跟踪部署](track-deployments.md).

您的Adobe Commerce on cloud基础架构项目包括New Relic APM服务的软件以及许可证密钥。 您无需购买或安装任何其他软件。

## New Relic基础架构

专业项目包括 [New Relic基础架构(NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) 服务，自动与应用程序数据和性能分析连接，以提供动态服务器监控。 此服务在专业生产和暂存环境中可用。

## New Relic日志管理

所有云基础架构项目包括 [New Relic日志管理](log-management.md). 该服务已预配置为聚合暂存和生产环境中的所有日志数据，并在集中式日志管理功能板中显示这些数据。
