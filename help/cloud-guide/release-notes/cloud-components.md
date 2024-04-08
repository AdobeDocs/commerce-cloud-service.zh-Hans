---
title: 适用于Commerce的云组件
description: 请参阅云组件包的最新改进列表。
recommendations: noDisplay, catalog
exl-id: b4e2508a-3558-4fa8-bae0-3eb76c7b2775
source-git-commit: c02dfd2709cdc63ac1630edaa8c89cad5f737ea1
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 适用于Commerce的云组件

此 [云组件](https://github.com/magento/magento-cloud-components) 该包为部署在Adobe Commerce基础架构上的站点提供扩展云核心功能。 此软件包是对ECE-Tools软件包的依赖项。 以下发行说明介绍了此软件包的最新改进，它是 [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

此 `magento/magento-cloud-components` 包使用以下版本序列： `<major>.<minor>.<patch>`

发行说明包括：

- ![新建图标](../../assets/new.svg) 新增功能
- ![修复图标](../../assets/fix.svg) 修复和改进功能

<!--Add release notes below-->

## v1.0.14 {#latest}

发行日期： 2024年4月8日

- ![新建图标](../../assets/new.svg) **PHP**  — 添加了对PHP 8.3的支持。

## v1.0.13

发行日期： 2023年3月10日

- ![新建图标](../../assets/new.svg) **对PHP 8.2的增强支持** — 修复了某些PHP 8.2.x版本的兼容性问题以支持Commerce 2.4.6。

## v1.0.12

发行日期： 2022年9月13日

- ![修复图标](../../assets/fix.svg) **预热时出错** — 修复了尝试以下操作的问题： [热身](../environment/variables-post-deploy.md#warm_up_pages) 页面可见性设置为 [**无法单独显示**](https://docs.magento.com/user-guide/system/data-attributes-product.html#simple-product-csv-file-structure) 在Admin中，导致 `ERROR: Warming up failed: <link to page>` 部署日志中有错误。<!-- MCLOUD-9134 -->

## v1.0.11

发行日期： 2022年8月4日

- ![修复图标](../../assets/fix.svg) **添加了对Symfony 5.4兼容性的支持** — 修复了与Symfony 5.4的兼容性。<!-- AC-3550 -->

## v1.0.10

发行日期： 2022年3月10日

- ![新建图标](../../assets/new.svg) **支持PHP 8.1** — 添加了对PHP 8.1的支持，并删除了对PHP 7.1的支持。

## v1.0.9

发行日期： 2021年10月25日

- ![修复图标](../../assets/fix.svg) **更新独白** — 更新了 `monolog` 打包到 `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

发行日期： 2021年7月29日

- ![修复图标](../../assets/fix.svg) **从自动生成的URL中删除了尾随斜杠** — 从缓存预热期间生成的类别页面URL中删除尾随斜杠。<!--MCLOUD-7192-->

## v1.0.7

发行日期： 2020年9月9日

- ![新建图标](../../assets/new.svg) **日志记录改进** — 减小的大小 `cache.log` 文件以改进性能。<!--MCLOUD-6859-->

- ![修复图标](../../assets/fix.svg) 修复了缓存配置值中导致出现以下问题的类型错误： `php bin/magento cache:evict` CLI命令失败。

## v1.0.6

发行日期： 2020年8月5日

- ![新建图标](../../assets/new.svg) **提高Redis性能** — 添加了 `./bin/magento cache:evict` 用于删除过期的Redis密钥的命令，该命令可减少Redis内存使用率以提高性能。<!--MCLOUD-6023-->

- ![修复图标](../../assets/fix.svg) 删除了对 *上下文中的New Relic日志* 以修复性能问题。<!--MCLOUD-6422-->

## v1.0.5

发布日期： 2020年6月25日

- ![修复图标](../../assets/fix.svg) 修复了magento/magento-cloud-components版本1.0.4中引入的问题，该问题会导致刷新缓存操作在部署阶段失败，中断部署过程。

## v1.0.4

发布日期： 2020年6月25日

- ![新建图标](../../assets/new.svg) **在上下文中实施了New Relic日志**—Adobe Commerce生成的应用程序日志现在显示在New Relic的跟踪中，以提高故障排除功能。<!--MCLOUD-6029-->

- ![新建图标](../../assets/new.svg) **改进了日志记录** — 添加了日志记录以跟踪缓存失效和完全重新索引事件。<!--MCLOUD-6157-->

## v1.0.3

发行日期： 2020年2月27日

- ![修复图标](../../assets/fix.svg) 修复了支持的兼容性问题 `ece-tools` 2002.0.x发行版，这些发行版使用较旧的PHP版本。

## v1.0.2

发行日期： 2020年2月6日

- ![新建图标](../../assets/new.svg) 扩展了 `WARM_UP_PAGES` 环境变量，用于支持特定产品页面的缓存预加载。 请参阅 [部署后变量](../environment/variables-post-deploy.md#warm_up_pages) 主题以了解详细的功能说明。<!--MAGECLOUD-4444-->

- ![修复图标](../../assets/fix.svg) 修复了在使用时，无效的存储URL导致部署后挂接失败的问题 `WARM_UP_PAGES` 填充缓存的功能。 仅当URL重写被禁用时才会发生此问题。<!-- MAGECLOUD-4094 -->

## v1.0.1

发行日期： 2019年7月23日

- ![修复图标](../../assets/fix.svg) 修复了会对下列内容造成影响的问题 [**WARMUP_PAGE**](../environment/variables-post-deploy.md#warm_up_pages) 使用默认商店URL的功能。 现在，如果 `config:show:default-url` 命令无法获取基本URL，则使用MAGENTO_CLOUD_ROUTES变量中的URL。<!-- MAGECLOUD-3866 -->

## v1.0.0

发布日期： 2019年6月12日

这是 [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) 包，这是对的新依赖项 `ece-tools` 包版本2002.0.20及更高版本。

- ![新建图标](../../assets/new.svg) 添加了使用正则表达式模式配置 **WARMUP_PAGE** 用于缓存单个页面、多个域和多页面的环境变量。 请参阅 [部署后变量](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
