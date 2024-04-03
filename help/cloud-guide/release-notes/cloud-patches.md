---
title: Commerce云修补程序
description: 请参阅云修补程序包的最新改进列表。
recommendations: noDisplay, catalog
last-substantial-update: 2024-01-16T00:00:00Z
exl-id: ae6b511b-a37d-4776-9a5e-ad7d9f9f6611
source-git-commit: 1e06545340cb63df483d1db5b2a890499e99e6d3
workflow-type: tm+mt
source-wordcount: '2175'
ht-degree: 0%

---

# Commerce云修补程序

此 [云修补程序](https://github.com/magento/magento-cloud-patches) 该包提供了一组必需的修补程序，用于改进所有Adobe Commerce版本与云环境的集成，并支持快速交付关键修补程序。

Cloud Patches for Commerce软件包依赖于ECE-Tools软件包，并在安装或更新ECE-Tools软件包时安装和更新。 您还可以使用和管理Cloud Patches for Commerce作为独立软件包，将修补程序应用到不在Cloud平台上的Adobe Commerce项目。 以下发行说明介绍了此包的最新改进。

>[!TIP]
>
>为确保您的项目具有所有必需的修补程序，请更新至 [ece-tools的最新版本](../dev-tools/update-package.md).

>[!NOTE]
>
>请参阅 [应用修补程序](../development/apply-patches.md) 以获取有关将修补程序应用到项目的说明。

此 `magento/magento-cloud-patches` 包使用以下版本序列： `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.0.25 {#latest}

发行日期： 2024年1月16日

- **缓存改进** — 对于Adobe Commerce版本2.4.4及更高版本，此补丁提高了布局缓存效率，显着减少了内存使用量。<!-- MCLOUD-11514 -->
- **CRON作业改进** — 此修补程序修复了错过作业会不必要地等待Adobe Commerce版本2.4.4及更高版本的cron作业锁定的问题。<!-- MCLOUD-11329 -->

## v1.0.24

发行日期： 2023年9月15日

- **性能改进** — 该修补程序通过将Adobe Commerce 2.4.6的相同部署配置加载次数减少到2.4.6-p1，修复了一个影响性能的问题<!-- MCLOUD-10604 -->

## v1.0.23

发行日期： 2023年7月31日

- **已删除修补程序MCLOUD-10604** — 此修补程序已移至QPT。<!-- MCLOUD-10736 -->

## v1.0.22

发布日期： 2023年6月19日

- **增强的QPT CLI向导/输出** — 向QPT CLI向导/输出添加了警告，提醒您在存在依赖关系时验证修补程序详细信息和要求。<!-- ACP2E-1963 -->
- **添加了Commerce 2.4.6的修补程序：**
   - 修复了 `regexp cache tag` 验证。<!-- MCLOUD-10226 -->
   - 通过减少相同部署配置加载的次数，提高了性能。<!-- MCLOUD-10604 -->
- **为Commerce 2.3.7至2.4.6添加了修补程序** — 修复了导致增量由随机值递增而不是由1递增的问题 `catalog_product_entity_*` 表格。<!-- MCLOUD-10032 -->
- **为Commerce 2.4.0至2.4.6添加了修补程序** — 修复了以下错误： `The file can't be deleted. Warning!unlink: No such file or directory`，从管理员刷新JS/CSS缓存时发生。<!-- MCLOUD-10279 -->

## v1.0.21

发行日期： 2023年3月10日

- **对PHP 8.2的增强支持** — 修复了某些PHP 8.2.x版本的兼容性问题以支持Commerce 2.4.6。

## v1.0.20

发行日期： 2022年10月27日

- **添加了L2缓存改进修补程序** — 该修补程序修复了刷新Commerce版本2.4.0和2.4.1的本地L2缓存时出现的问题。<!-- MCLOUD-7845 -->

## v1.0.19

发行日期： 2022年9月13日

- **对PHP 8.1的增强支持** — 修复了某些PHP 8.1.x版本存在的兼容性问题。

## v1.0.18

发行日期： 2022年8月11日

Adobe Commerce 2.4.5的关键修补程序：

- **使用Braintree付款的订单问题** — 此修补程序解决了阻止管理员发出新订单或重新订购的关键问题。<!-- MCLOUD-9137 -->

请参阅 [启用Braintree付款后，管理员无法创建订单/重新订单](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## v1.0.17

发行日期： 2022年5月24日

修复了中安全补丁的限制 `patches.json` 文件。

## v1.0.16

发行日期： 2022年3月31日

Adobe Commerce 2.3.3-p1及更高版本的关键修补程序：

更新了修补程序以解决 **关键** 导致未经身份验证的远程代码执行的漏洞。<!-- MCLOUD-8479 -->

请参阅 [Adobe安全公告APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.15

发行日期： 2022年3月10日

- **支持PHP 8.1** — 添加了对PHP 8.1的支持，并删除了对PHP 7.0和7.1的支持。
- **添加了Adobe Commerce 2.3.3的修补程序** — 修复了产品页面上显示的货币。

## v1.0.14

发行日期： 2022年2月13日

Adobe Commerce 2.3.3-p1及更高版本的关键修补程序：

添加了修补程序以解决 **关键** 导致未经身份验证的远程代码执行的漏洞。<!-- MCLOUD-8461 -->

请参阅 [Adobe安全公告APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.13

发行日期： 2021年10月25日

- **更新独白** — 更新了 `monolog` 打包到 `^2.3`.<!-- ACMP-1263 -->
- **PHP方法不兼容** — 修复了Adobe Commerce版本2.4.3和2.3.7的不兼容PHP方法 — p1。<!-- AC-384 -->
- **PHP错误** — 修复了 `PHP error 'Undefined variable: errorMessage' ...` 尝试应用修补程序时出错。<!-- ACP2E-138 -->

## v1.0.12

发行日期： 2021年8月12日

Adobe Commerce 2.4.3和2.3.7-p1的关键修补程序：

- **API速率限制问题** — 该修补程序更正了一个默认速率限制，该限制阻止Web API处理数组中超过20个项的请求。 此修补程序将提高速率限制的默认值。 请参阅Adobe Commerce [2.4.3发行说明](https://devdocs.magento.com/guides/v2.4/release-notes/commerce-2-4-3.html#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting) 和 [2.3.7发行说明](https://devdocs.magento.com/guides/v2.3/release-notes/2-3-7-p1.html#apply-mc-43048__set_rate_limits__237-p1patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

发行日期： 2021年7月29日

- **修复了应用B2B分层导航修补程序导致的问题** — 对于已应用B2B分层导航修补程序的客户，此修复解决了 `Undefined offset` 切换商店视图后在“搜索”页面上显示的错误。<!--MCLOUD-5287-->

- **Paypal签出修补程序** — 修复了PayPal Express的Adobe Commerce 2.3.7问题，该问题会显示之前所下的订单价格。<!--MC-42674-->

- **修补程序类别支持** — 添加了对处理分配给质量修补程序的修补程序类别和源源的支持。 类别允许客户使用过滤器和排序功能在使用时更快地查找修补程序 [Quality Patches工具](https://github.com/magento/quality-patches) 和全站点分析工具(SWAT)。 <!--MC-38577-->

## v1.0.10

发行日期： 2021年5月10日

- **与Adobe Commerce 2.3.7的兼容性** — 解决了在Adobe Commerce 2.3.7上安装的编辑器依赖项冲突。<!--MC-42131-->
- **修复了多次应用捆绑修补程序导致的问题** — 多次应用捆绑的修补程序（包括其他已弃用的修补程序）可以还原所包含的已弃用包。 现在，所有修补程序仅应用一次。 再次尝试应用同一程序包时，将显示一条消息，指出已应用该修补程序。<!--MC-41912-->
- **B2B分层导航修补程序** — 修复了另一个问题，该问题在用户启用B2B共享目录时阻止分层导航显示所有产品选项。<!--MCLOUD-7742-->

## v1.0.9

发行日期： 2021年2月1日

- **B2B分层导航修补程序** — 修复了在启用B2B共享目录时，阻止分层导航显示所有产品选项的问题。<!--MCLOUD-6923-->
- **与PHP 7.4的兼容性** — 修复了PHP 7.4存在的云修补程序兼容性问题。<!--MCLOUD-7367-->
- **弃用的修补程序将变为可见** — 修复了以下云修补程序问题：在应用包含已弃用修补程序的全部内容的替换修补程序后，已弃用的修补程序会在修补程序表中可见。 如果应用的修补程序组合了多个其他修补程序，则可能会发生这种情况。<!--MC-40626-->
- **应用修补程序时的静默式故障** — 修复了以下云修补程序问题： `git apply` 命令在某些环境中静默应用修补程序失败。<!--MC-40529-->

## v1.0.8

发行日期： 2020年10月14日

- **magento/magento-cloud-patches的兼容性更新** — 更新了 `symfony` 和 `semver` 中的版本约束 `composer.json` 文件以与Adobe Commerce 2.4.1及更高版本兼容。<!--MCLOUD-7111-->

## v1.0.7

发行日期： 2020年10月14日

- **适用于Adobe Commerce 2.3.0到2.3.5、2.4.0的Redis修补程序** — 更新了Redis修补程序，以支持在实施第2级缓存时将产品添加到类别中。 <!--MCLOUD-6659-->

- **BraintreeVBE修补程序** — 修复了在管理员尝试查看Braintree结算报告时导致错误的问题。 <!--MCLOUD-6684-->

- 现在， `ece-patches apply` 命令使用Unix `patch` 在主机系统上没有Git时应用修补程序的命令。 <!--MCLOUD-7069-->

## v1.0.6

发行日期：

- **Adobe Commerce 2.3.0 - 2.3.4的Redis修补程序** — 优化通信并提高性能
   - 缩小Redis和Adobe Commerce之间的网络传输量
   - 修复Redis加载和写入操作的争用情况
   - 重写基本缓存适配器以处理保存时的错误
   - 降低Redis CPU消耗<!--MCLOUD-6139-->

- **Adobe Commerce 2.3.0 - 2.3.5的Redis修补程序** — 改进性能和修复错误
   - 修复了缓存锁定实施以防止无限锁定
   - 改进当前锁定机制
   - 实施已签名的锁定以防止对并行请求进行解锁
   - 修复在Redis写入操作中发生的以下错误： `OOM command not allowed when used memory > maxmemory`
   - 修复了清理缓存的处理，修复者： `cat_p` 在产品更新期间运行的标记<!--MCLOUD-6110-->

- 修复了在应用所需内容时导致错误的问题 `amzn/amazon-pay-module` 使用Adobe Commerce v2.2.6或2.3.5的Adobe Commerce在云基础架构项目上修补此模块。 现在，修补过程跳过了 `amzn/amazon-pay-module` 修补（如果未安装模块）。<!--MCLOUD-6588-->

## v1.0.5

发布日期： 2020年6月26日

- **Redis性能改进** — 将Redis优化功能添加到Adobe Commerce版本2.3.3和2.3.4。Adobe Commerce版本2.3.5版本中包含这些修复。 请参阅 [性能提升](https://devdocs.magento.com/guides/v2.3/release-notes/release-notes-2-3-5-commerce.html#performance-boosts) 在 _Adobe Commerce 2.3.5发行说明_.<!--MCLOUD-5771-->

- **New Relic日志丰富程序** — 添加所需的单色处理器界面，以支持Commerce 1.0.4版的New Relic组件中引入的对Cloud日志记录功能的改进。部署Adobe Commerce 2.1.x时需要此修补程序。如果未应用该修补程序，则在 `di:compile` 进程。<!--MCLOUD-6029-->

## v1.0.4

发行日期： 2020年5月12日

- **Amazon Pay结账** — 修复了Amazon支付支付构件阻止客户更改以下页面上的支付方式的问题： _审核与支付_ 结账过程中的步骤。<!--MCLOUD-5930-->

- **类别页面上的产品显示** — 修复了导致产品无法在的类别页面上显示的问题。 _显示所有页面_ 视图。<!--MCLOUD-5684-->

- **Page Builder图像上传** — 修复了在将图像上传到图像库时有时会导致以下错误的Page Builder界面问题： `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **禁止显示不必要的站点地图生成警告** — 在生成Sitemap期间发生错误时添加重试尝试，并在可以自动恢复错误的情况下跳过客户电子邮件通知。<!--MCLOUD-3025-->

- **网站性能改进** — 修复了的性能问题 `Magento\Framework\App\DeploymentConfig\Reader::load` 函数中，该函数会定期经历影响站点性能的长时间加载。 <!--MCLOUD-5650-->

- 更新了支付方式修补程序的修补程序分配，以支付模块为目标，而非Magento基础包(magento/magento2-base)，以便仅当支付模块存在时才应用支付修补程序。<!--MCLOUD-5666-->

- 更新了修补程序以与Magento Open Source兼容。<!--MCLOUD-5701-->

## v1.0.3

发行日期： 2020年4月28日

- 添加了对“部署期间已禁用FPC”修补程序的修复，以支持Adobe Commerce 2.3.5。

## v1.0.2

发行日期： 2020年2月27日

此版本包括以下补丁程序和关键修复：

- **magento/magento-cloud-patches的兼容性更新**

   - 已更新 `symfony` 和 `semver` 中的版本约束 `composer.json` 文件以与Adobe Commerce 2.4及更高版本兼容。<!--MAGECLOUD-5127-->

   - 更新了中的约束 `composer.json` 与兼容 `ece-tools` 2002.0.22及更高版本2002.0.x。

- **PayPal Express签出** — 此修补程序于2020年2月12日发布，它解决了影响通过PayPal Express Checkout下单的问题，其中订单的送货地址指定了手动输入文本字段，而不是从“送货”页面的下拉菜单中选择的国家/地区区域。 请参阅修补程序下载页面上的完整修补程序说明。

- **应用程序部署修复** — 添加了一个修补程序，用于修复在部署过程中禁用了全页缓存的问题。 此修补程序适用于Adobe Commerce 2.3.2及更高版本。

- **异步/批量API的范围参数** — 更新了此修补程序，以修复 `composer.json` 文件。 此修补程序适用于Magento Open Source2.3.1和2.3.2。请参阅修补程序下载页面上的完整修补程序说明。

## v1.0.1

发行日期： 2020年2月6日

我们已在magento/magento-cloud-patches v1.0.1版的软件下载页面中包含所有Magento Open Source2.x修补程序。 如果以前将任何修补程序复制到项目中，请删除它们以避免冲突。

此版本包括以下补丁程序和关键修复：

- **修复cron死锁并改进cron锁定**—

   - 修复了由于中的状态值不正确而导致某些cron作业无法运行的问题。 `cron_schedule` 表格。 现在，我们使用Adobe Commerce锁定框架来检查和更新cron作业状态，而不是使用 `cron_schedule` 表格。 以错误状态结束的Cron作业将在下次cron运行期间重试，而不是等待24小时。

   - 添加 _重试_ 操作以避免在更新数据期间出现死锁 `cron_schedule` 表格。

- **已更新 `magento/magento-cloud-patches` 包括适用于Magento Open Source2.x的所有可用修补程序** — 更新了magento/magento-cloud-patches包，以包含“软件下载”页面上提供的所有Magento Open Source2.x修补程序。 如果您之前已将任何Magento Open Source修补程序复制到Adobe Commerce on cloud infrastructure项目中，请将其删除以避免冲突。<!--MAGECLOUD-4606-->

- **Elasticsearch目录分页修复**  — 用更有效的修复程序替换了magento/magento-cloud-patches v1.0中提供的Elasticsearch目录分页修补程序。<!--MAGECLOUD-4847-->

- **Page Builder修补程序** — 在Cloud Patches for Commerce 1.0.0中，我们捆绑了Page Builder修补程序，以解决已知的Page Builder远程代码执行(RCE)漏洞，初步修复了基于Adobe Commerce 2.3.3的漏洞。我们基于Adobe Commerce 2.3.4更新了这些修补程序，采用了一个更加稳定的实施，其中包括用于修复问题的多个优化。<!--MAGECLOUD-4884-->

  如果您有magento/magento-cloud-patches 1.0.0包，则仍会受Page Builder RCE漏洞问题的保护。 如果更新到1.0.1或更高版本，则实施同一修复的效果会更好。

## v1.0.0

发行日期： 2019年11月14日

这是 [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 包，这是对的新依赖项 `ece-tools` 软件包版本2002.0.22或更高版本。

此版本包括以下补丁程序和关键修复：

- **2.3.1.x和2.3.2.x版本的Page Builder安全修补程序** — 修复了Page Builder预览中的问题，该问题允许未经身份验证的用户访问某些模板方法，这些方法可用于通过网络触发任意代码执行(RCE)，从而导致全局信息泄漏。 在Adobe Commerce版本2.3.1和2.3.2中使用不受支持的页面生成器版本时，可能会出现此问题。<!--MAGECLOUD-4649-->

- **MSI修补程序** — 修复了使用默认库存设置管理库存时导致索引错误和性能问题的情况。<!--MAGECLOUD-4428-->

- **新邮件界面的向后兼容性** — 修复了由以下问题导致的向后不兼容问题： `Magento\Framework\Mail\EmailMessageInterface` Adobe Commerce v2.3.3中引入的PHP接口。在此修补程序范围内，新的 `EmailMessageInterface` 继承自旧的 `MessageInterface`和Adobe Commerce核心模块将恢复为依赖于 `MessageInterface`.<!--MAGECLOUD-4422-->

- **目录分页在Elasticsearch6.x上不起作用** — 修复了搜索结果分页的一个关键问题，该问题影响使用Elasticsearch6.x作为目录搜索引擎的客户。<!--MAGECLOUD-4448-->
