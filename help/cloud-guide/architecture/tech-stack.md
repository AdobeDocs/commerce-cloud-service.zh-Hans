---
title: 技术栈栈
description: 请参阅在云基础架构上形成Commerce的技术栈栈。
feature: Cloud, Iaas, Paas
exl-id: e456db25-c44b-4053-b96d-517d3d1606d0
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# 技术栈栈

将云基础架构上的Adobe Commerce视为五个功能层，如下所示：

![云栈栈](../../assets/CloudStack.svg)

1. [**云基础架构**](pro-architecture.md)：选择Amazon Web Services (AWS)或Microsoft Azure作为您的Adobe Commerce on cloud infrastructure Pro项目上的基础架构即服务(IaaS)基础。

   Adobe会定期分析您的虚拟计算资源(vCPU)使用情况，并自动分配资源以优化长期使用情况，并降低超出最大年度vCPU日允许量的风险。 如果您预计特定时间段的网站流量会有所增加，则必须继续打开支持工单以便 [请求临时扩展](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html).

1. [**平台即服务**](cloud-architecture.md)：每个Adobe Commerce on cloud基础架构项目都提供了一个Platform as a Service (PaaS)集成环境，用于开发、测试和集成服务。
1. [**Adobe Commerce**](../project/overview.md)：云基础架构上的Adobe Commerce提供了一个预配置的基础架构，包括PHP、MySQL (MariaDB)、Redis、 [!DNL RabbitMQ]和支持的搜索引擎技术。
1. [**性能工具**](../monitor/new-relic-service.md)：New Relic性能工具允许您通过收集、分析和显示来自Adobe Commerce的有关云基础架构项目的数据，来调试、监视和管理应用程序和基础架构。
1. [**内容交付网络(CDN)、Web应用程序防火墙([!DNL WAF])和图像优化(IO)**](../cdn/fastly.md)：

   * [Fastly CDN](../cdn/fastly.md#ddos-protection) — 提供安全CDN服务，并内置针对分布式拒绝服务(DDoS)攻击的保护，例如 [!DNL Ping of Death]， [!DNL Smurf] 攻击和其他基于Internet控制消息协议(ICMP)的洪水攻击。
   * [Web应用程序防火墙(WAF)](../cdn/fastly-waf-service.md)—WAF服务确保生产环境中Adobe Commerce店面的PCI合规性和WAF策略，这些策略可保护Adobe Commerce Web应用程序免受注入攻击、恶意输入、跨站点脚本、数据导出、HTTP协议违规和其他攻击 [[!DNL OWASP] 十大安全威胁](https://owasp.org/www-project-top-ten/).
   * [图像优化(IO)](../cdn/fastly-image-optimization.md) — 提供实时图像处理和优化，以加快图像投放并简化响应式Web应用程序的图像源集的维护。 Fastly IO减轻了图像处理和调整负载大小的负担，使服务器能够高效地处理订单和转换。

单一应用程序需要大量资源，难以快速扩展和提供服务。 借助云基础架构，Commerce客户可获得对基于SaaS的丰富、智能和性能卓越的微服务的无与伦比的访问权限。 请参阅 [支持的软件和服务](cloud-architecture.md#supported-software-and-services).

使用 [Commerce入门指南](../../get-started/overview.md) 以设置新的云项目并开始管理 [!DNL Commerce] 云原生环境中的应用程序。
