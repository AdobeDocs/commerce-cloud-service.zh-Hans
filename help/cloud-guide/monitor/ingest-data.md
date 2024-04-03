---
title: 数据摄取
description: 了解如何在New Relic中查看和管理Commerce数据引入。
feature: Cloud, Observability
exl-id: f88bf20c-604b-4986-b71c-bb726b2f00b8
source-git-commit: bf3debc5986d51a721537b52ffced58b2ee521ea
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# 数据摄取

New Relic依靠丰富的数据提供有效的监测和分析，但大型数据集可能会影响及时的结果、性能和合规性。 本主题提供有关管理数据摄取的一些指南，以及优化数据以使其最有效的策略。

New Relic提供 _数据管理_ 按数据源汇总计划使用情况的视图。

**要查看您的摄取数据和源，请执行以下操作**：

1. 从New Relic用户菜单中，单击 **[!UICONTROL Manage your data]**.
1. 单击 **[!UICONTROL Data management]** 在 _管理_ 列表。

   ![数据管理](../../assets/new-relic/data-ingestion.png)

   此 **[!UICONTROL Data ingestion]** 选项卡显示针对日期摄取的数据以及数据源。
“数据保留”选项卡显示并控制数据的存储时间。

1. 选择 **[!UICONTROL Limits]** 选项卡并查看您帐户的限制。

Adobe Commerce的数据源包括：

- **APM事件** — 在图表和功能板中使用的事件数据
- **基础架构** — 进程和主机指标，如CPU 、存储、网络
- **记录**—CDN、APM和应用程序服务器的日志

日志数据在摄取中占很大比例。 了解如何 [查看和分析日志数据](log-management.md#view-and-analyze-log-data) 并与您的Adobe代表合作，针对数据摄取和保留需求制定策略。 详细了解 [管理数据引入](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) 在 _New Relic文档_.
