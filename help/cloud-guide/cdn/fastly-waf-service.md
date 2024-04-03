---
title: Web应用程序防火墙(WAF)
description: 了解Fastly WAF服务如何检测、记录和阻止恶意请求流量，以防其损坏Adobe Commerce网络或网站。
feature: Cloud, Configuration, Security
exl-id: 40bfe983-7f32-4155-ae77-7cd18866f6e2
source-git-commit: 6b01bf91c3bf63ba268d0f8e10e19db4cb355997
workflow-type: tm+mt
source-wordcount: '824'
ht-degree: 0%

---

# Web应用程序防火墙(WAF)

由Fastly提供支持的Adobe Commerce云基础架构上的Web应用程序防火墙(WAF)服务可检测、记录并阻止恶意请求流量，以防其损害您的网站或网络。 WAF服务仅在生产环境中可用。

WAF服务提供以下优点：

- **PCI合规性** — 支持WAF可确保生产环境中的Adobe Commerce存储符合PCI DSS 6.6安全要求。
- **默认WAF策略** — 由Fastly配置和维护的默认WAF策略提供专门定制的安全规则集合，用于保护Adobe Commerce Web应用程序免受范围广泛的攻击，包括注入攻击、恶意输入、跨站点脚本、数据导出、HTTP协议违规及其他 [OWASP前十名](https://owasp.org/www-project-top-ten/) 安全威胁。
- **WAF入门和支持**—Adobe在最终资源调配后的2到3周内在您的生产环境中部署并启用默认WAF策略。
- **运行和维护支持**—
   - Adobe和Fastly设置并管理WAF服务的日志和警报。
   - Adobe会分类与WAF服务问题相关的客户支持工单，这些工单将阻止合法流量作为优先级1问题。
   - 自动升级至WAF服务版本，确保即时覆盖新的或不断演变的漏洞利用。 请参阅 [WAF维护和升级](#waf-maintenance-and-updates).

>[!TIP]
>
>有关在云基础架构存储上维护Adobe Commerce的PCI合规性的其他信息，请参阅 [PCI合规性](https://business.adobe.com/products/magento/pci-compliance.html).

## 启用WAF

Adobe可在预配完成后两到三周内在新帐户上启用WAF服务。 WAF通过Fastly CDN服务实现。 您无需安装或维护任何硬件或软件。

>[!NOTE]
>
>在使用WAF服务之前，您必须在云基础架构项目上连接到Adobe Commerce的所有外部流量都必须通过Fastly服务路由。 请参阅 [设置Fastly](fastly-configuration.md).

## 工作原理

WAF服务与Fastly集成，并使用Fastly CDN服务中的缓存逻辑过滤Fastly全局节点上的流量。 我们在您的生产环境中启用WAF服务，默认WAF策略基于 [Trustwave SpiderLabs的ModSecurity规则](https://github.com/owasp-modsecurity/ModSecurity) 以及OWASP十大安全威胁。

WAF服务会针对WAF规则集筛选HTTP和HTTPS流量(GET和POST请求)，并阻止恶意流量或不遵守特定规则的流量。 该服务仅过滤尝试刷新缓存的原点绑定流量。 因此，我们会在Fastly缓存中停止大多数攻击流量，从而保护原始流量免受恶意攻击。 通过仅处理源流量，WAF服务可以保留缓存性能，只为每个非缓存请求引入估计为1.5毫秒到20毫秒的延迟。

## 疑难解答被阻止的请求

启用WAF服务后，它会根据WAF规则筛选所有Web和管理员流量，并阻止触发规则的任何Web请求。 当请求被阻止时，请求者会看到一个默认值 `403 Forbidden` 包含阻止事件引用ID的错误页面。

![WAF错误页](../../assets/cdn/fastly-waf-403-error.png)

您可以从管理员自定义此错误响应页面。 请参阅 [自定义WAF响应页](fastly-custom-response.md#customize-the-waf-error-page).

如果您的Adobe Commerce管理页面或店面返回 `403 Forbidden` 错误页面响应合法的URL请求，提交 [Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). 复制错误响应页面中的引用ID，并将其粘贴到票证描述中。

## WAF维护和更新

Fastly根据商业第三方、Fastly研究和公开来源的规则更新来维护和更新WAF规则集。 Fastly会根据需要或在可从各自来源对规则进行更改时将发布的规则更新为策略。 此外，在启用WAF服务后，Fastly可以将与发布的规则类匹配的规则添加到任何服务的WAF实例中。 这些更新确保即时涵盖新的或不断演变的利用漏洞攻击。

Adobe和Fastly管理更新过程，以确保新的或修改的WAF规则在您的生产环境中有效工作，然后才以阻止模式部署更新。

## 限制

由Fastly提供的标准WAF服务不支持以下功能：

- 防范恶意软件或减少机器人 — 考虑使用 [访问控制列表](./fastly-vcl-allowlist.md) 或第三方服务。
- 速率限制 — 请参阅 [速率限制](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) 在Fastly文档中，或参阅 [限速](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) 在 _Commerce Web API_ 安全部分。
- 配置客户的日志记录端点 — 请参阅 [PrivateLink服务](../development/privatelink-service.md) 作为替代方案。

虽然WAF服务不允许根据IP地址阻止或允许流量，但您可以向Fastly服务添加访问控制列表(ACL)和自定义VCL片段，以指定IP地址和VCL逻辑来阻止或允许流量。 请参阅 [自定义Fastly VCL片段](fastly-vcl-custom-snippets.md).

WAF服务不支持过滤TCP、UDP或ICMP请求。 但是，此功能由Fastly CDN服务中包含的内置DDoS保护提供。 请参阅 [DDoS保护](fastly.md#ddos-protection).
