---
title: 自动缩放
description: 了解云基础架构上的Adobe Commerce如何进行扩展以满足资源需求。
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 2ba49c55-d821-4934-965f-f35bd18ac95f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# 自动缩放

自动缩放功能可自动向云基础架构添加或删除资源，以保持最佳性能和合理的成本。 目前，此功能仅适用于配置了的项目。 [可扩展的体系结构](scaled-architecture.md).

## Web服务器节点

此 [Web层](scaled-architecture.md#web-tier) 扩展以适应流程请求的增加和流量需求的增加。 目前，自动缩放功能只能通过添加或删除Web服务器节点来水平缩放。

当CPU使用率和流量达到预定义的阈值时，会发生自动缩放事件：

- **已添加节点** — 所有活动Web节点的CPU/核心在1分钟内都达到了75%的容量，并且流量连续5分钟增长了20%。
- **节点已删除** — 所有活动Web节点上的CPU/核心以60%的速率加载，时间为20分钟。 节点会按照其添加顺序进行删除。

最小和最大阈值根据每个商户的合同资源限制确定和设置；这降低了无限扩展的风险。

## 使用New Relic监控阈值

您可以使用 [New Relic服务](../monitor/new-relic-service.md) 监视特定阈值，如主机数和CPU使用率。 以下New Relic查询使用变量表示法 `cluster-id` 仅供示例使用。

>[!TIP]
>
>有关构建查询的参考，请参阅 [NRQL语法、子句和函数](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) 在 _New Relic_ 文档。
>使用查询构建 [New Relic功能板](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### 主机计数

以下示例New Relic查询显示环境中的主机计数：

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

在下面的屏幕截图中， **已查看APM主机** 是指在选定期间记录事务的主机数。

![New Relic主机计数](../../assets/new-relic/host-count.png)

### CPU使用率

以下示例New Relic查询显示了Web节点的CPU使用情况：

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic Web节点CPU使用情况](../../assets/new-relic/web-node-cpu-usage.png)

## 启用自动缩放

要在云基础架构项目中为Adobe Commerce启用或禁用自动缩放，请执行以下操作： [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). 在票证中选择以下原因：

- **联系原因**：基础架构更改请求
- **Adobe Commerce基础架构联系原因**：其他基础架构更改请求

>[!IMPORTANT]
>
>自动缩放功能会捕获意外事件。 即使您启用了自动缩放，Adobe仍建议您继续 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 如果您期待即将到来的活动。

### 负载测试

Adobe可在您的云项目上启用自动缩放 _暂存_ 集群优先。 在环境中执行并完成负载测试后，Adobe会在生产群集上启用自动缩放。 有关负载测试的指导，请参阅 [性能测试](../launch/checklist.md#performance-testing).

### IP允许列表

启用自动缩放后，出站Web节点流量源自服务节点的IP地址。 如果您使用的允许列表与云基础架构项目上的Adobe Commerce列入允许列表未捆绑的第三方服务一起使用，请验证第三方服务中的IP地址。

例如：

- 如果允许列表包含服务节点（1、2和3）的IP地址，则无需执行任何操作。
- 如果允许列表包含服务节点（1、2和3）和Web节点（4、5和6）的IP地址（本例中是全部六个节点），则无需执行任何操作。
- 如果允许列表包含IP地址 _仅限_ 列入允许列表对于Web节点（4、5和6），则必须更新以包含服务节点的IP地址。
