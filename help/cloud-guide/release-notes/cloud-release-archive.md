---
title: ece-tools的发行说明存档
description: 了解ece-tools的存档改进。
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 96663d2f-ef00-4490-ad2e-06e8041b228e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# ece-tools的发行说明存档

>[!NOTE]
>
>这些发行说明提供了以下各项的信息和更新： `ece-tools` v2002.0.22及更高版本。 请参阅 [Cloud Tools Suite发行说明](cloud-tools-suite.md) 获取的最新更新 `ece-tools` 和其他云包。

## v2002.0.22

此 `ece-tools` 2002.0.22发行版更改了 `ece-tools` 用于使的版本脱钩的包 `Adobe Commerce on cloud infrastructure` ECE-Tools发行版中的修补程序。 从此版本开始，将使用以下工具提供补丁程序和关键修复： [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 包，这是对的新依赖项 `ece-tools` 包。 我们进行了这些更改，以降低计划发布更新以及处理社区贡献的复杂性。

- ![新建图标](../../assets/new.svg) **对ECE-Tools套件的更改**

   - ![新建图标](../../assets/new.svg) 已将Adobe Commerce修补程序从 `ece-tools` 打包到新 [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) 编辑器包。

   - ![新建图标](../../assets/new.svg) 已更新 `composer.json` 的文件 `ece-tools` 包以添加对的依赖项 `magento/magento-cloud-patches` v1.0.0包。

   - ![修复图标](../../assets/fix.svg) 修复了导致出现错误的问题 `ece-tools` 修补过程在仅安全版本之上应用修补程序集时中断，从版本2.3.2-p2及更高版本开始。 此问题是由为引入版本控制而采用的新版本控制方案所引入的 [仅限安全的修补程序](https://devdocs.magento.com/guides/v2.3/release-notes/bk-release-notes.html#security-only-patches).<!--MAGECLOUD-4661-->

- ![修复图标](../../assets/fix.svg) **补丁程序和关键修复** — 使用更新您的云环境 `ece-tools` 版本2002.0.22 ，以应用以下补丁程序和关键修复。 这些修补程序包含在 `magento/magento-cloud-patches` v1.0.0包。

   - ![修复图标](../../assets/fix.svg) **2.3.1.x和2.3.2.x版本的Page Builder安全修补程序** — 修复了Page Builder预览中的问题，该问题允许未经身份验证的用户访问某些模板化方法，这些方法可用于通过网络触发任意代码执行(RCE)，从而导致全局信息泄漏。 在Adobe Commerce版本2.3.1和2.3.2中使用不受支持的页面生成器版本时，可能会出现此问题。<!--MAGECLOUD-4649-->

   - ![修复图标](../../assets/fix.svg) **MSI修补程序** — 修复了使用默认库存设置管理库存时导致索引错误和性能问题的情况。<!--MAGECLOUD-4428-->

   - ![修复图标](../../assets/fix.svg) **新邮件界面的向后兼容性** — 修复了由以下问题导致的向后不兼容问题： `Magento\Framework\Mail\EmailMessageInterface` Adobe Commerce v2.3.3中引入的PHP接口。在此修补程序范围内，新的 `EmailMessageInterface` 继承自旧的 `MessageInterface`和Adobe Commerce核心模块将恢复为依赖于 `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![修复图标](../../assets/fix.svg) **目录分页在Elasticsearch6.x上不起作用** — 修复了搜索结果分页的一个关键问题，该问题影响使用Elasticsearch6.x作为目录搜索引擎的客户。<!--MAGECLOUD-4448-->

## v2002.0.21

- ![新建图标](../../assets/new.svg) **Docker更新**—

   - ![新建图标](../../assets/new.svg) **新Docker图像** — 受版本2.3.3及更高版本支持<!-- MAGECLOUD-3345 -->

      - PHP版本7.3。<!-- MAGECLOUD-4017 -->

      - 清漆缓存6.2.0<!-- MAGECLOUD-4017 -->

   - ![新建图标](../../assets/new.svg) 添加了对应用中指定的自定义挂接配置的支持 `.magento.app.yaml` 在Docker环境中。 以前，Docker环境仅支持默认挂接配置。<!-- MAGECLOUD-3505-->

   - ![新建图标](../../assets/new.svg) 在Docker构建期间不再生成Docker环境文件，并且 `docker:config:convert` 命令已弃用。 相应的数据现在存储在中 `docker-compose.yml` 文件。<!-- MAGECLOUD-3816-->

   - ![新建图标](../../assets/new.svg) **更新了PHP映像** — 向PHP Docker映像中添加了Node.js，以支持node 、 npm和grunt-cli功能。<!-- MAGECLOUD-3953 -->

- ![新建图标](../../assets/new.svg) **环境变量更新**-

   - ![新建图标](../../assets/new.svg) 添加了 **LOCK_PROVIDER** 部署变量以配置锁定提供程序，从而阻止启动重复的cron作业和cron组。 请参阅中的变量说明 [部署变量](../environment/variables-deploy.md#lock_provider) 主题。<!-- MAGECLOUD-4052 -->

   - ![新建图标](../../assets/new.svg) 添加了 **CONSUMERS_WAIT_FOR_MAX_MESSAGES** 环境变量，用于配置使用者在使用时如何处理来自消息队列的消息。 `CRON_CONSUMERS_RUNNER` 用于管理cron作业的环境变量。 请参阅中的变量说明 [部署变量](../environment/variables-deploy.md#consumers_wait_for_max_messages) 主题。<!-- MAGECLOUD-4071 -->

   - ![修复图标](../../assets/fix.svg) 修复了以下情况时可能导致数据库死锁错误的问题 `consumers_runner` cron作业在不同节点上启动同一使用者的多个实例。 现在，如果您已启用 [**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) 在您的环境中部署变量， `consumers_runner` 作业使用 `single-thread` 用于仅在一个节点上启动每个使用者的一个实例的选项。<!-- MAGECLOUD-3913 -->

   - ![修复图标](../../assets/fix.svg) 修复了会对下列内容造成影响的问题 [**WARMUP_PAGE**](../environment/variables-post-deploy.md#warm_up_pages) 使用默认商店URL的功能。 现在，如果 `config:show:default-url` 命令无法获取基本URL，则使用MAGENTO_CLOUD_ROUTES变量中的URL。<!-- MAGECLOUD-3866 -->

- ![新建图标](../../assets/new.svg) 更新了返回的日志记录信息 `module:refresh` 命令。 现在，您可以在中查看已启用的模块的详细列表 `cloud.log` 文件。<!-- MAGECLOUD-2514 -->

- ![新建图标](../../assets/new.svg) 改进了版本兼容性验证和警告通知，以解决Adobe Commerce版本与已安装的服务(例如Elasticsearch、 [!DNL RabbitMQ]、Redis和DB。<!-- MAGECLOUD-3535 -->

- ![新建图标](../../assets/new.svg) 添加了对RabitMQ版本3.8的支持。<!-- MAGECLOUD-4674-->

- ![新建图标](../../assets/new.svg) 更新了有关服务兼容性的交互式验证，以反映新的Adobe Commerce 2.3.3和2.2.10版本支持的版本。 请参阅 [系统要求](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html) 在 _安装指南_ 推荐的版本。<!-- MAGECLOUD-4018 -->

- ![修复图标](../../assets/fix.svg) 改进了部署阶段的cron作业管理进程尝试停止已完成的cron作业时返回的日志消息，以阐明此问题不是错误。 更改了日志级别 `INFO` 到 `DEBUG`.<!-- MAGECLOUD-3653-->

- ![修复图标](../../assets/fix.svg) 修复了运行的问题 `setup:upgrade` 未中断部署过程的命令。 `app:config:import` 任务。<!-- MAGECLOUD-3806 -->

- ![新建图标](../../assets/new.svg) 已将文件处理程序的默认日志级别更改为 `debug` 以减少日志中显示的详细信息量，该详细信息量 [!DNL Cloud Console]，但仍会提供调试的详细信息。<!-- MAGECLOUD-3871 -->

- ![修复图标](../../assets/fix.svg) 修复了在生成期间导致静态内容部署错误的问题。 安装完成之后，并 `ece-tools` 配置转储，如果在 `config.php` 文件。 现在，管理员用户在 `config.php` 文件。<!-- MAGECLOUD-3957 -->

- ![修复图标](../../assets/fix.svg) 修复了 `Undefined index error` 发生于 `magento-cloud` 在未配置安全URL (https)的环境中，CLI命令失败。 现在，如果安全URL不可用，ECE-Tools包将使用基本URL (http)。<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![新建图标](../../assets/new.svg) **Docker更新**—

   - ![新建图标](../../assets/new.svg) 您现在可以使用以下工具执行功能测试 `ece-tools` Docker环境中的包。 请参阅 [应用程序测试](https://devdocs.magento.com/cloud/docker/docker-test-magecloud-pkg-code.html).<!-- MAGECLOUD-3129/3684 -->

   - ![新建图标](../../assets/new.svg) 添加了对使用配置PHP模块的支持 `.magento.app.yaml` 文件。 任何 [PHP扩展在 `.magento.app.yaml` 文件](https://devdocs.magento.com/cloud/project/magento-app-php-application.html#php-extensions) 在Docker PHP容器中变得可用。<!-- MAGECLOUD-3357 -->

   - ![新建图标](../../assets/new.svg) 有新命令可用于改进Docker命令行体验。 请参阅 [`bin/magento-docker` Docker引用的部分](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![新建图标](../../assets/new.svg) 添加了使用Mutagen.io在本地主机和Docker之间的开发期间同步文件的功能。<!-- MAGECLOUD-3559 -->

   - ![修复图标](../../assets/fix.svg) 更正了使用Docker环境时的默认路径。 现在，当您使用SSH登录到Docker容器时，您位于中的项目根目录 `/app` 目录（预期）。<!-- MAGECLOUD-3582 -->

   - ![修复图标](../../assets/fix.svg) 将Na库从1.0.11版更新到1.0.18版，并更新了Na PHP扩展。<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >云基础架构上的Adobe Commerce客户必须 [提交Adobe Commerce支持票证](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) 在升级到Adobe Commerce 2.3.2之前，在Pro Production and Staging环境中升级libna软件包。目前，您无法将入门环境升级到Adobe Commerce 2.3.2。

   - ![修复图标](../../assets/fix.svg) 添加了 `analysis-icu` 和 `analysis-phonetic` 将插件Elasticsearch到所有Docker图像。<!-- MAGECLOUD-3446 -->

   - ![修复图标](../../assets/fix.svg) 改进了验证：对使用选项时 `docker:build` 命令，则在使用选项时必须提供一个值。 此外，添加了使用时节点版本的验证 `docker:build run` 命令。<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![新建图标](../../assets/new.svg) **环境变量更新**—

   - ![新建图标](../../assets/new.svg) 使用添加了对数据库表前缀的支持 [DATABASE_CONFIGURATION环境变量](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![新建图标](../../assets/new.svg) 添加了 **FORCE_UPDATE_URL** 部署变量可在部署到Pro和Starter生产和暂存环境时更新基本URL。 请参阅中的定义 [部署变量](../environment/variables-deploy.md#force_update_urls) 内容。<!-- MAGECLOUD-3602 -->

   - ![新建图标](../../assets/new.svg) 添加了 **TTFB_TESTED_PAGES** 要配置的部署后变量 _到第一个字节的时间_ 页面测试，用于检查部署到云基础架构的站点上的应用程序性能。 请参阅中的变量说明 [部署后变量](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![修复图标](../../assets/fix.svg) 修复了多线程SCD的问题，该问题会导致静态内容部署中出现随机故障。 解决方法是设置 **SCD_THREADS** 变量到 `1`. 您现在可以根据需要增加数量。 欲知相关定义，请参见 [部署变量](../environment/variables-deploy.md#scd_threads) 和 [生成变量](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![修复图标](../../assets/fix.svg) 您可以配置 **WARMUP_PAGE** 用于缓存单个页面、多个域和多页面的环境变量。 有关扩展定义，请参阅 [部署后变量](../environment/variables-post-deploy.md#warm_up_pages) 内容。<!-- MAGECLOUD-3258 -->

- ![修复图标](../../assets/fix.svg) 添加了 `pub/static/.htaccess` 文件添加到排除列表。 [由PHOENIX MEDIA GmbH的Bjorn Kraus提交](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![修复图标](../../assets/fix.svg) 修复了所有验证消息显示为 `Critical` 至少有一个严重级别验证器返回错误。<!-- MAGECLOUD-3178 -->

- ![修复图标](../../assets/fix.svg) 修复了在数据库中不存在基本URL时导致部署失败的问题。<!-- MAGECLOUD-3075 -->

- ![新建图标](../../assets/new.svg) 添加了一个新的 **`env:config:show`命令** 到 `ece-tools` 显示环境服务、路由或变量的资源包。 请参阅 [服务、路由和变量](https://devdocs.magento.com/cloud/reference/ece-tools-reference.html#services-routes-and-variables). [Vladimir Kerkhoff提交的专题](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![修复图标](../../assets/fix.svg) 修复了在尝试使用安装Adobe Commerce 2.2.6或更低版本时导致严重错误的问题 `ece-tools` 在外壳重构后开发。<!-- MAGECLOUD-3665 -->

- ![修复图标](../../assets/fix.svg) 修复了导致Adobe Commerce 2.1.x和2.2.x安装失败并警告用户使用已弃用的Carbon版本的问题。<!-- MAGECLOUD-3704 -->

- ![修复图标](../../assets/fix.svg) 降低了 `cloud.log` shell输出的日志级别 `info` 到 `debug`.<!-- MAGECLOUD-3277 -->

- ![修复图标](../../assets/fix.svg) 添加了 `--remove-definers (-d)` 选项 `ece-tools db-dump` 用于从转储文件中删除定义器的命令。<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![修复图标](../../assets/fix.svg) 修复了覆盖 `env.php` 在部署期间生成文件，从而导致自定义配置丢失。 此更新确保Adobe Commerce on cloud基础架构更新 `env.php` 文件，同时保留自定义配置。<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![新建图标](../../assets/new.svg) **Docker更新**—

   - ![新建图标](../../assets/new.svg) 现在，Docker环境支持中定义的cron配置 [.magento.app.yaml文件的crons属性](https://devdocs.magento.com/cloud/project/magento-app-properties.html#crons).<!-- MAGECLOUD-3150 -->

   - ![新建图标](../../assets/new.svg) **新建Docker容器** — 添加了 [TLS终止代理容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) 便于通过HTTPS终止Varnish SSL。<!-- MAGECLOUD-2890 -->

   - ![新建图标](../../assets/new.svg) **新建Docker图像** — 添加了Node.js图像以支持Gulp和其他功能，如Jasmine JS单元测试。<!-- MAGECLOUD-3345 -->

   - ![新建图标](../../assets/new.svg) **Docker构建模式** — 现在，您可以选择在中启动Docker环境 [生产模式或开发人员模式](https://devdocs.magento.com/cloud/docker/docker-launch.html#set-the-launch-mode). 开发人员模式支持具有完全可写文件系统权限的活动开发。<!-- MAGECLOUD-3152/3511 -->

   - ![修复图标](../../assets/fix.svg) 修复了导致Docker部署失败的问题， `Name or service not known` 如果为不可用的服务配置了缓存，则会出错。 现在，您可以从 [`.magento/services.yaml` 文件](https://devdocs.magento.com/cloud/project/services.html). Docker配置生成器更新以下位置中的服务： `docker/config.php.dist` 文件自动生成。<!-- MAGECLOUD-3369 -->

   - ![新建图标](../../assets/new.svg) 添加了针对服务兼容性的交互式验证。 现在，如果请求的服务与Adobe Commerce版本或其他服务不兼容，则 _交互模式_ 提示用户一条消息，并可以选择继续。 请参阅 [服务版本](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers) 可用于Docker。 使用 `-n` 选项跳过用于CICD的交互。<!-- MAGECLOUD-3251 -->

   - ![修复图标](../../assets/fix.svg) 修复了Docker撰写的问题 `db-dump` 用于清除现有转储的命令。<!-- MAGECLOUD-3366 -->

   - ![修复图标](../../assets/fix.svg) 修复了分配红色的问题 `session`， `default`、和 `page_cache` 将存储缓存到相同的数据库ID。<!-- MAGECLOUD-3172 -->

- ![新建图标](../../assets/new.svg) **环境变量更新**—

   - ![新建图标](../../assets/new.svg) 新 **ELASTICSUITE\_CONFIGURATION** 环境变量会在部署之间保留自定义的服务设置。 请参阅中的定义 [部署变量](../environment/variables-deploy.md#elasticsuite_configuration) 内容。<!-- MAGECLOUD-3205 -->

   - ![新建图标](../../assets/new.svg) 添加了 **SCD_MAX_EXECUTION_TIMEOUT** 环境变量，以便您可以增加从完成静态内容部署的时间 `.magento.env.yaml` 文件。 请参阅中的定义 [部署变量](../environment/variables-deploy.md#scd_max_execution_time)， [生成变量](../environment/variables-build.md#scd_max_execution_time)，和 [全局变量](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![新建图标](../../assets/new.svg) 添加了 **Magento_云_锁_目录** 环境变量，用于为云基础架构上的锁定提供程序配置装入点的路径。 锁定提供程序阻止启动重复的cron作业和cron组。 Adobe Commerce版本2.2.5及更高版本支持此变量，并且会自动配置此变量。 请参阅中的定义 [云变量](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![修复图标](../../assets/fix.svg) 已更改 **SCD_THREADS** 环境变量默认值，用于根据检测到的CPU线程计数自动确定最佳值。 欲了解更新后的定义，请参见 [部署变量](../environment/variables-deploy.md#scd_threads) 和 [生成变量](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![修复图标](../../assets/fix.svg) 修复了数据库隔离机制补丁的问题，该问题导致在云基础架构版本2002.0.16上升级到Adobe Commerce时出错。<!-- MAGECLOUD-3383 -->

- ![修复图标](../../assets/fix.svg) 添加了一个修补程序，该修补程序将 _Google图像图表_ 替换为 _图像图表_. 请参阅DevBlog文章 [适用于M1的Google图像图表弃用和更新](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![修复图标](../../assets/fix.svg) 添加了对的验证 [SEARCH_CONFIGURATION变量](../environment/variables-deploy.md#search_configuration). 如果未设置“engine”选项并且 `_merge` 非必填。<!-- MAGECLOUD-3470 -->

- ![修复图标](../../assets/fix.svg) 修复了在发生异常后公开敏感数据的问题。 现在，敏感信息被适当地掩盖。<!-- MAGECLOUD-3525 -->

- ![修复图标](../../assets/fix.svg) 改进了Magento Open Source包的容错设置。 在Adobe Commerce无法从Redis读取数据的情况下 `slave` 例如，从Redis中进行读取 `master` 实例。 请参阅 [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>此 `ece-tools` 2002.0.17版包括一个重要的安全修补程序。 请参阅 [技术资源：Magento Open Source修补程序](https://magento.com/tech-resources/download#download2288).

- ![新建图标](../../assets/new.svg) **服务更新** — 受以下Adobe Commerce版本支持：2.2.8及更高版本2.2.x、2.3.1及更高版本2.3.x

   - 添加了对Elasticsearch版本6.x的支持。<!-- MAGECLOUD-3196 -->

   - 添加了对Redis版本5.0的支持。

- ![新建图标](../../assets/new.svg) **新Docker图像** — 向Docker内部版本添加了以下服务：

   - Elasticsearch6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![新建图标](../../assets/new.svg) **新环境变量** — 以前，SCD压缩存在硬编码超时。 现在，您可以使用配置SCD压缩超时 **SCD_COMPRESSION_TIMEOUT** 环境变量。 欲知相关定义，请参见 [生成变量](../environment/variables-build.md#scd_compression_timeout) 和 [部署变量](../environment/variables-deploy.md#scd_compression_timeout) 内容。<!-- MAGECLOUD-2870 -->

- ![修复图标](../../assets/fix.svg) 添加了 `--use-rewrites` 选项，以便它使用Web服务器重写店面中生成的链接以及管理员访问权限来改进安全性和客户体验。<!-- MAGECLOUD-3246 -->

- ![修复图标](../../assets/fix.svg) 已添加时间戳到 `var/log/install_upgrade.log` 文件，以便显示安装和升级事件的日期。<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![新建图标](../../assets/new.svg) **Docker更新**—

   - 现在，在Docker环境中生成的默认服务配置与云模板中的默认配置相同。<!-- MAGECLOUD-3025 -->

   - 您可以使用从Docker环境发送邮件 `sendmail` 服务。<!-- MAGECLOUD-2907 -->

   - 添加了以下功能 [配置Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) 以在Cloud Docker环境中调试。<!-- MAGECLOUD-2891 -->

   - 修复了在生成 `docker-compose.yml` 文件。<!-- MAGECLOUD-2883 -->

- ![新建图标](../../assets/new.svg) **升级改进** — 添加了验证，以确认 `autoload` 中的属性 `composer.json` 文件包含升级到Adobe Commerce v2.3之前所需的配置更改。请参阅 [升级版本](https://devdocs.magento.com/cloud/project/project-upgrade.html).<!-- MAGECLOUD-2392 -->

- ![新建图标](../../assets/new.svg) 现在，部署静态内容时的压缩过程包括本机生成或自定义的所有资源，并且会在构建阶段开始时进行 [`build:transfer` 部分](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks). 以前，压缩过程在应用自定义缩小和静态资源捆绑之前进行。 [由Tryzens Limited的Rafael Garcia Lepper提交的修复](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![修复图标](../../assets/fix.svg) 修复了在配置其他数据库和服务关系后立即在部署期间发生的数据库连接错误。 此外，此修复还解决了在Commerce Reporting for Starter配置过程中发生的问题。 首先，此升级对于使用Commerce Reporting是“必须具有”的。<!-- MAGECLOUD-3035 -->

- ![修复图标](../../assets/fix.svg) 修复了导致部署过程失败的数据库配置的验证问题。<!-- MAGECLOUD-3003 -->

- ![修复图标](../../assets/fix.svg) 已使用相应版本的更新约束 `symfony/yaml` 要与一起使用的包 [php常量](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants). 使用时常量解析不起作用 `symfony/yaml` 早于3.2的包版本。 [Vladimir Kerkhoff提交的修复](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![新建图标](../../assets/new.svg) **环境配置检查** — 添加了验证，以检查PHP版本并在用户未使用最新的推荐版本时警告用户。<!--MAGECLOUD-2903-->

- ![修复图标](../../assets/fix.svg) 修复了处理格式错误的JSON变量时出现的问题。 现在，如果JSON变量导致语法错误，则会在 `cloud.log` 文件和部署将继续使用默认变量。<!-- MAGECLOUD-2851 -->

- ![修复图标](../../assets/fix.svg) 修复了在禁用Redis服务后立即部署期间发生的连接错误。<!-- MAGECLOUD-2747 -->

- ![新建图标](../../assets/new.svg) **记录更改** — 更新了 [日志级别](../environment/log-handlers.md#log-levels) 从 `Info` 到 `Notice` 对于以下生成和部署流程事件：<!--MAGECLOUD-2925-->

   - 在中协调已安装的模块的过程的开始和结束 `composer.json` 中的共享配置设置 `app/etc/config.php` 文件

   - 配置验证过程的开始和结束

   - 开始和结束 `setup:di:compile` 生成类的过程

- ![新建图标](../../assets/new.svg) **新环境变量**—

   - **[RESOURCE_CONFIGURATION部署变量](../environment/variables-deploy.md#resource_configuration)** — 使用此变量将资源名称映射到数据库连接。<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION全局变量](../environment/variables-global.md#x_frame_configuration)** — 使用此变量可更改 `X-Frame-Options` 用于在中呈现Adobe Commerce页面的页眉配置 `<frame>`， `<iframe>`，或 `<object>`.<!-- MAGECLOUD-3048 -->

- ![修复图标](../../assets/fix.svg) **环境变量更新** — 更改了以下环境变量：

   - **[WARMUP_PAGE](../environment/variables-post-deploy.md)** — 添加了在为Adobe Commerce存储定义的所有域上为指定页面预加载缓存的功能。 以前，如果您的站点配置了多个域，则部署后进程无法预先加载非默认域上指定页面的缓存，并在部署后日志中返回以下错误： `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** — 更新了文档和示例 `.magento.env.yaml` 文件具有正确的SCD压缩级别默认值。 欲知相关定义，请参见 [生成变量](../environment/variables-build.md#scd_compression_level) 和 [部署变量](../environment/variables-deploy.md#scd_compression_level) 内容。<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** — 此环境变量已弃用。 使用 [SCD矩阵](../environment/variables-build.md#scd_matrix) 控制主题配置。<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** — 修复了验证过程，以防止在SCD_MATRIX忽略包含不同字符大小写的主题值时发生问题。 欲知相关定义，请参见 [生成变量](../environment/variables-build.md#scd_matrix) 和 [部署变量](../environment/variables-deploy.md#scd_matrix) 内容。<!-- MAGECLOUD-2904 -->

   - **管理员变量**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 提高了使用环境变量管理管理员用户凭据时的安全性。 在升级过程中，不能再使用ADMIN_EMAIL、ADMIN_USERNAME和ADMIN_PASSWORD环境变量覆盖管理员凭据。 如果您无法访问“管理员”面板，请使用 _忘记密码_ 功能或 `admin:user:create` 用于创建新的管理员用户的CLI命令。 请参阅 [访问您的管理面板](https://devdocs.magento.com/cloud/onboarding/onboarding-tasks.html#admin).

      - 升级或应用修补程序时不再需要ADMIN_EMAIL。

## v2002.0.15

- ![新建图标](../../assets/new.svg) **Docker更新**—

   - 现在，Docker生成器使用 `.magento.app.yaml` 和 `.magento/services.yaml` 配置文件时间 [构建Docker环境](https://devdocs.magento.com/cloud/docker/docker-config.html). 您可以使用生成参数选择其他服务版本。<!-- MAGECLOUD-2888 -->

   - 添加了PHP 7.2图像 — 在Cloud Docker中添加了对PHP 7.2的支持；更新了 [Launch Docker配置](https://devdocs.magento.com/cloud/docker/docker-config.html) 以包含 `docker:build --php` 选项来指定与您的Adobe Commerce版本兼容的PHP版本。<!-- MAGECLOUD-2799 -->

   - 添加了 [Cron容器](https://devdocs.magento.com/cloud/docker/docker-containers-cli.html#cron-container) 基于PHP-CLI映像。<!-- MAGECLOUD-2565 -->

   - 已将以下服务添加到Docker版本：

      - [!DNL RabbitMQ] 3.5和3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch1.7、2.4和5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2和4.0<!-- MAGECLOUD-2886 -->

- ![新建图标](../../assets/new.svg) **使用PHP常量配置** — 增加了对 [php常量](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants) 在 `.magento.env.yaml` 配置文件。<!-- MAGECLOUD- 2575 -->

- ![新建图标](../../assets/new.svg) **新环境变量** — 默认情况下，只有生产环境启用了Google Analytics。 您可以使用在暂存环境和集成环境中启用Google Analytics  [ENABLE_GOOGLE_ANALYTICS环境变量](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![修复图标](../../assets/fix.svg) 修复了从删除自定义cron配置的问题 `env.php` 重新部署后的文件。 现在，自定义cron配置安全地保留在 `env.php` 文件。<!-- MAGECLOUD-2923 -->

- ![修复图标](../../assets/fix.svg) 修复了消息和内容不一致的问题 [日志级别](../environment/log-handlers.md#log-levels) 用于生成、部署和部署后阶段。 增加开始和结束日志消息级别 **信息** 到 **通知** 所有阶段和子阶段。 在适当时添加了开始和结束日志消息。<!-- MAGECLOUD-2919 -->

- ![修复图标](../../assets/fix.svg) 修复了在配置后阻止启动部署后阶段的cron进程存在的问题。 现在，如果您启用了部署后挂接，则会在部署后阶段的开始时再次启用cron流程。<!-- MAGECLOUD-2862 -->

- ![修复图标](../../assets/fix.svg) 解决了在指定自定义数据库配置时阻止成功安装Adobe Commerce的问题。 以前，安装过程使用的数据库配置来自 [Magento_CLOUD_RELATIONSHIP变量](../environment/variables-cloud.md) 即使您在 [DATABASE_CONFIGURATION环境变量](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![修复图标](../../assets/fix.svg) 已更正 `config:dump` 命令，以使其包含 `system` 的部分 `config.php` 文件。<!--MAGECLOUD-2740-->

- ![修复图标](../../assets/fix.svg) 修复了导致出现错误的问题 _热身_ 在部署后阶段出现错误，请更正源基本URL引用。<!--MAGECLOUD-2797-->

- ![修复图标](../../assets/fix.svg) 修复了以下任务期间文件生成不正确的问题： `setup:di:compile` 流程，这会影响Amazon支付模块。<!--MAGECLOUD-2850-->

## v2002.0.14

- ![新建图标](../../assets/new.svg) **验证理想状态** — 此 `ideal-state` 向导现在会在每次部署期间验证当前配置，并提供明确的说明来更新配置，以实现更快的零停机部署。<!--MAGECLOUD-2372-->

- ![修复图标](../../assets/fix.svg) **PCI合规性** — 更新了云基础架构上Adobe Commerce的消息协议，使其在与第三方消息服务连接时要求使用传输层安全性(TLS)版本1.2。 如果您使用的消息服务不支持TLS版本1.2，则必须升级您的服务。 否则，当您的Adobe Commerce应用程序尝试连接到消息服务器以发送电子邮件时，会显示以下错误消息： `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![新建图标](../../assets/new.svg) **部署改进** — 添加了验证，以便在暂存或生产环境具有 `dev`， `debug`，或 `debug_logging` 启用选项以防止因过多的日志记录活动而导致性能问题。<!--MAGECLOUD-2517-->

- ![修复图标](../../assets/fix.svg) **部署修复**—

   - 现在，维护模式在部署阶段开始时启用，在部署阶段结束时禁用。 如果部署失败，站点将保持维护模式，直到解决部署问题为止。 以前，即使部署失败，站点也会返回到生产模式。<!--MAGECLOUD-2603-->

      - 已重新执行部署阶段验证检查，将以下部署问题的错误级别降级： `CRITICAL` 到 `WARNING` 以便完成部署。 以前，这些问题会导致部署失败。

      - 环境配置包含不正确的部署或云变量值。

   - 云基础架构上的Elasticsearch版本与云基础架构上的Adobe Commerce支持的elasticsearch/elasticsearch模块的版本不兼容。 请参阅 [Elasticsearch疑难解答文章](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) 在Adobe Commerce支持知识库中。<!--MAGECLOUD-2600-->

   - 修复了中的共享配置设置的问题。 `app/etc/config.php` 导致错误的文件 `recursion detected` 部署过程中出错。<!--MAGECLOUD-2173-->

- ![修复图标](../../assets/fix.svg) **Cron相关修复**—

   - 修复了在指定默认（1分钟）以外的cron频率时，阻止作业运行的cron计划问题。<!--MAGECLOUD-2602-->

   - 修复了部署阶段中存在的一个问题，该问题会导致cron作业在部署期间继续运行，从而可能导致数据库锁定和其他严重问题。 现在，所有cron作业都将在部署阶段开始之前停止，并在部署完成后重新启动。&lt;!—MAGECLOUD—2537—>

   - 修复了2.2.x版中的cron作业工作流以解锁冻结的cron作业，以便在开始部署之前停止它们。 以前，冻结的cron作业导致部署停止。<!--MAGECLOUD-2501-->

- ![修复图标](../../assets/fix.svg) 更改了 `config.php` 生成的文件 `vendor/bin/ece-tools config:dump` 命令以使用短数组语法和4空格缩进来遵循Adobe Commerce编码标准。<!--MAGECLOUD-2527-->

- ![修复图标](../../assets/fix.svg) 修复了以下情况下发生的部署错误： `.magento.env.yaml` 包含 `{{ base_url }}` 和 `{{ unsecure_base_url }}` Web配置的占位符，而不是Adobe Commerce on cloud infrastructure项目的默认URL配置。/<!--MAGECLOUD-2607-->

## v2002.0.13

- ![新建图标](../../assets/new.svg) **实现零停机部署** — 现在，云基础架构上的Adobe Commerce在部署期间将包含所需数据库更改的请求排入队列，并在部署完成后立即应用更改。 请求最长可保留5分钟，以确保不会丢失任何会话。 请参阅 [减少云上部署停机时间的静态内容部署选项](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![新建图标](../../assets/new.svg) **适用于云的Docker合成** — 对进行了以下改进 [Docker设置和配置](https://devdocs.magento.com/cloud/docker/docker-config.html) 进程：

   - 添加了命令 — `docker:config:convert` 将PHP配置文件转换为Docker ENV格式以简化环境配置。 现在，将PHP配置文件复制到Docker目录，并将其转换为Docker ENV文件。 请参阅 [Launch Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).<!--MAGECLOUD-2359-->

   - Adobe Commerce on cloud基础架构安装过程现在支持同时部署到只读和读写文件系统，以便更密切地模拟云文件系统。 请参阅 [配置Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).&lt;!—MAGECLOUD—2357—>

   - **Redis服务支持** — 添加了一个Redis映像，该映像部署到Docker容器并自动配置为与Docker安装配合使用。&lt;!—MAGECLOUD—2442—>

   - 现在，在使用Cloud Docker时，您具有数据库转储功能 [数据库容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#database-container). 此外，您可以 [共享文件](https://devdocs.magento.com/cloud/docker/docker-containers.html#sharing-data-between-host-machine-and-container) 主机和容器之间的连接，使用 `docker/mnt` 目录。<!-- MAGECLOUD-2577 -->

   - **涂漆服务支持** — 添加了Varnish图像，该图像自动部署到Docker容器中。 部署后，您可以按照Adobe Commerce最佳实践手动配置涂漆。 请参阅 [配置和使用清漆](https://devdocs.magento.com/guides/v2.3/config-guide/varnish/config-varnish.html).&lt;!—MAGECLOUD—2358—>

   - 安全站点访问 — 为访问Adobe Commerce存储和管理面板添加了SSL支持。&lt;!—MAGECLOUD—2360—>

- ![修复图标](../../assets/fix.svg) **在云基础架构扩展支持方面改进了Adobe Commerce** — 已降级Adobe Commerce on cloud infrastructure中guzzlehttp/guzzle包的最低版本要求 [composer.json文件](https://devdocs.magento.com/cloud/reference/cloud-composer.html) 到版本6.2，以便 `ece-tools` 包与更多扩展兼容。<!--MAGECLOUD-2205-->

- ![新建图标](../../assets/new.svg) **在构建阶段将自定义更改应用于Adobe Commerce应用程序** — 我们将构建阶段拆分为两个单独的进程，以便您能够使用挂接将自定义更改应用于生成的静态内容，然后再打包应用程序以进行部署。 此 _生成：生成_ 进程生成代码、应用修补程序并生成静态内容。 此 _build：transfer_ 进程将生成的代码和静态内容传输到最终目标。 请参阅 [应用程序挂钩](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks).<!--MAGECLOUD-2363-->

- ![修复图标](../../assets/fix.svg) **环境配置检查** — 改进了环境配置验证，可在云基础架构上构建和部署Adobe Commerce之前，向客户发出版本不兼容和配置错误警告。

   - 添加了特定于版本的验证，以标识不支持或弃用的环境变量和值。<!--MAGECLOUD-2183-->

   - 添加了Elasticsearch兼容性检查，以就Elasticsearch配置问题向用户发出警告。 现在，如果服务器上的Elasticsearch服务版本与Adobe Commerce不兼容，则部署将失败。 以前，即使Elasticsearch版本不兼容，部署也会成功，这会导致在站点部署后出现产品目录问题。<!--MAGECLOUD-2389-->

     您可以通过以下方式解决不兼容问题 [提交支持票证](https://devdocs.magento.com/cloud/trouble/trouble.html) 要将Elasticsearch升级到兼容版本，或更改Adobe Commerce配置以指定ElasticsearchPHP客户端的兼容版本。

      - 对于Adobe Commerce版本2.1.x到2.2.2，请将Elasticsearch升级到版本2.4。

      - 对于Adobe Commerce版本2.2.3及更高版本，请将Elasticsearch升级到版本5.2。

      - 如果您有Elasticsearch1.x或2.x，并且不想升级，请在composer.json中将Adobe CommerceElasticsearchPHP客户端版本要求更新为 `"elasticsearch/elasticsearch": "~2.0"`.

   - 改进了环境变量验证，以识别在生成、部署和部署后阶段可能会导致冲突的配置设置。 例如，如果静态内容部署的全局设置与构建或部署阶段的设置冲突，则在安装和升级过程中会显示警告消息。<!--MAGECLOUD-2156-->

- ![修复图标](../../assets/fix.svg) **环境变量更新** — 更改了以下环境变量：

   - **[SKIP_HTML_缩小全局变量](../environment/variables-global.md#skip_html_minification)** — 将默认值更改为 `true` 启用按需压缩HTML内容，这可以最大限度地减少部署到暂存和生产环境时的停机时间。 零停机部署需要此配置。<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES部署变量](../environment/variables-deploy.md#clean_static_files)** — 添加了基于CLEAN_STATIC_FILES环境变量设置来管理构建阶段生成的静态内容的clean静态文件处理的功能。 以前，始终清理在构建阶段生成的静态内容文件。<!--MAGECLOUD-1506-->

- ![修复图标](../../assets/fix.svg) **记录** — 进行了以下更改以改进日志消息并减少日志大小：

   - 部署失败日志条目现在包括导致失败的操作的命令输出，即使您的环境配置未指定调试级别日志记录也是如此。 请参阅 [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - 添加了部署故障日志记录，当某些扩展所需的生成工厂由于文件系统处于只读状态而无法正确生成时，将发生部署故障。<!--MAGECLOUD-2209-->

   - 减小了部署日志大小，并修复了使用交互式进度条的设置命令导致的格式问题。<!--MAGECLOUD-2402-->

   - 消除了不必要的冗长，并更新了一些日志语句的优先级。<!--MAGECLOUD-2227-->

- ![修复图标](../../assets/fix.svg) **Cron特定修复**—

   - 已将历史记录生命周期的默认cron作业配置设置从3d （4320分钟）更改为1h （60分钟），以防止在cron队列填充过快时可能出现性能问题和部署失败。<!--MAGECLOUD-2427-->

   - 改进了部署阶段的cron作业管理过程，以防止数据库锁定和其他严重问题。 现在，所有cron作业在部署阶段停止，并在部署完成后重新启动。<!--MAGECLOUD-2445-->

   - 修复了Adobe Commerce版本2.2.0及更高版本中由cron作业启动的用于计划使用者的锁定机制的问题，以防止cron作业启动重复使用者。<!--MAGECLOUD-2464-->

- ![修复图标](../../assets/fix.svg) 修复了的问题 [静态内容压缩过程](../environment/variables-intro.md) (`gzip`)而导致 `not overwritten` 和 `no such file or directory` 在部署过程中引用压缩文件时出错。<!-- MAGECLOUD-2182-->

- ![修复图标](../../assets/fix.svg) 修复了导致无法 `php ./vendor/bin/ece-tools config:dump` 命令从删除冗余部分 `config.php` 文件转储过程中（如果未指定存储区域设置）。 现在，您可以轻松地在环境之间移动配置文件。 在您更新到 `ece-tools` v2002.0.13，重新生成旧版本 `config.php` 具有改进功能的文件 `config:dump` 命令。 请参阅 [商店设置的配置管理](https://devdocs.magento.com/cloud/live/sens-data-over.html).<!--MAGECLOUD-2444-->

- ![修复图标](../../assets/fix.svg) 修复了在部署阶段导致错误的问题，如果 `.magento/routes.yaml` 文件从 [Apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) 域到 `www` 域。<!--MAGECLOUD-2556-->

- ![修复图标](../../assets/fix.svg) 修复了的问题 `_merge` 的选项 [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) 导致合并结果不正确的变量 `engine` 参数在更新的 `.magento.env.yaml` 配置文件。 现在，合并操作只能正确覆盖在更新版本中指定的值 `.magento.env.yaml` 无需您设置 `engine` 参数。<!--MAGECLOUD-2520-->

- ![修复图标](../../assets/fix.svg) 修复了Redis配置问题，该问题导致在云基础架构版本2.2.1及更高版本上为Adobe Commerce错误地启用会话锁定，这可能会导致性能和超时缓慢。 现在，默认情况下禁用会话锁定。 此问题是由的缺省行为更改引起的 `disable_locking` Redis会话处理程序包v1.3.4中引入的参数。 请参阅 [colimollenhour/php-redis-session-abstract包](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![新建图标](../../assets/new.svg) **适用于云的Docker合成** — 添加命令 — `docker:build` — 生成 [Docker撰写](https://devdocs.magento.com/cloud/docker/docker-config.html) 云中的配置 `ece-tools` 存储库。<!-- MAGECLOUD-2250 -->

- ![新建图标](../../assets/new.svg) **更改区域设置** — 现在无需导出和导入配置过程即可更改存储区域设置。 当应用程序处于生产状态并启用SCD_ON_DEMAND时，存储和管理区域设置选项可用。<!-- MAGECLOUD-2019 -->

- ![新建图标](../../assets/new.svg) <!-- MAGECLOU-1998 -->**站点地图和机器人** — 已创建 [工作流](../store/robots-sitemap.md) 以添加 `robots.txt` 文件并生成 `sitemap.xml` 文件用于单个域配置，无需更改基础架构。

- ![新建图标](../../assets/new.svg) **向导** — 添加了两个 [向导](../deploy/smart-wizards.md) 要帮助您进行云配置，请执行以下操作：<!-- MAGECLOUD-1910 -->

   - `ideal-state` — 配置理想状态以将部署停机时间降至最低

   - `master-slave` — 配置数据库和Redis的负载平衡

- ![新建图标](../../assets/new.svg) **模块刷新** — 添加了Cloud命令 — `module:refresh` — 启用已禁用或未显式启用的模块，类似于在构建期间自动启用的方法。<!-- MAGECLOUD-1521 -->

- ![新建图标](../../assets/new.svg) 添加了以下功能：使用 `_merge` 中的选项 [缓存](../environment/variables-deploy.md#cache_configuration)， [SESSION](../environment/variables-deploy.md#session_configuration)， [队列](../environment/variables-deploy.md#queue_configuration)、和 [SEARCH](../environment/variables-deploy.md#search_configuration) 配置。<!-- MAGECLOUD-2105 -->

- ![新建图标](../../assets/new.svg) **环境配置示例文件** — 我们添加了 `.magento.env.yaml` ECE-Tools软件包的示例文件，其中包括每个环境变量的详细说明和可能值。<!-- MAGECLOUD-1908 -->

   - 我们还添加了 `.magento.env.yaml` 用于防止部署过程中因意外值而导致失败的配置。 失败时，您现在会收到一条详细的错误消息，其开头为： `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![新建图标](../../assets/new.svg) 添加了以下内容 [**环境变量**](../environment/variables-intro.md)：

   - 现在，您可以使用新的为每个主题定义多个区域设置 [SCD矩阵](../environment/variables-deploy.md#scd_matrix) 环境变量，从而减少要部署的主题文件的数量。<!-- MAGECLOUD-1501 -->

   - 添加了 [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) 用于自定义数据库连接以进行部署的环境变量。<!-- MAGECLOUD-2047 -->

   - 新 [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) 变量覆盖所有输出流的最低日志记录级别，而不对代码进行更改。<!-- MAGECLOUD-2129 -->

- ![修复图标](../../assets/fix.svg) 修复了导致部署和部署后阶段之间出现停机时间的问题。 现在，部署后阶段开始 _立即_ 在部署阶段结束之后。

- ![修复图标](../../assets/fix.svg) 修复了未清理成功的cron作业（即具有的cron作业）的问题。 `status = success`，从计划。<!-- MAGECLOUD-2268 -->

- ![修复图标](../../assets/fix.svg) 修复了的问题 `post_deploy` 挂接，该挂接在部署阶段而不是项目的部署后阶段清除缓存。<!-- MAGECLOUD-2113 -->

- ![修复图标](../../assets/fix.svg) 修复了将SCD用于多个区域设置时的问题，会产生相同的区域设置 `js-translation.json` 每个区域设置的文件。<!-- MAGECLOUD-2034 -->

- ![修复图标](../../assets/fix.svg) 已优化 `db:dump` 中的命令 `ece-tools` 打包以避免锁定表格并提高速度。<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>ECE-Tools版本2002.0.11是2.2.4兼容性的必要条件。

- ![新建图标](../../assets/new.svg) **配置与非主节点的只读连接** — 此版本添加了配置到非主节点的只读连接以接收只读通信量的功能(对于  [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection))。<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) 和 <!--MAGECLOUD-143 -->

- ![新建图标](../../assets/new.svg) **配置向导** — 添加了一个向导，用于帮助验证静态内容部署的配置。 请参阅 [智能向导](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![新建图标](../../assets/new.svg) **Symfony控制台支持** — 增加了对Symfony Console 4和Adobe Commerce 2.3的支持。<!-- MAGECLOUD-1966 -->

- ![修复图标](../../assets/fix.svg) **Cron计划优化** — 改进了队列管理并增强了日志记录功能，以帮助调试cron相关问题。<!-- MAGECLOUD-1607 -->

- ![修复图标](../../assets/fix.svg) 在以下情况下，部署验证失败： `ADMIN_EMAIL` 或 `ADMIN_USERNAME` 值与现有管理员帐户相同。<!-- MAGECLOUD-1221 -->

- ![修复图标](../../assets/fix.svg) 删除了2.2.x版本的SOLR支持。 2.1.x版本保留启用SOLR的功能。<!-- MAGECLOUD-1282 -->

- ![修复图标](../../assets/fix.svg) PRO项目的暂存和生产环境的首次安装现在包括用于Elasticsearch的各种索引前缀，以防止在识别属于每个环境的记录时可能发生冲突。<!-- MAGECLOUD-1489 -->

- ![修复图标](../../assets/fix.svg) 修复了在静态内容部署期间中断旧版体系结构的构建阶段的问题。<!-- MAGECLOUD-2021 -->

- ![修复图标](../../assets/fix.svg) **Cron特定的改进** — 重新实施了cron实施：<!-- MAGECLOUD-1607 -->

   - 修复了导致cron队列快速填充的问题。 如今，它以更可靠的方式清理了过时的cron工作。

   - 重新组织了cron作业序列，以便单独线程中的所有作业在常规组之前启动。

   - 改进了日志记录，以更好地协助调试cron问题。

   - **注意** — 此版本解决了许多与cron相关的问题。 如果您当前在中使用一些与cron相关的修补程序 _m2 — 修补程序_，删除它们。

- ![修复图标](../../assets/fix.svg) **特定于SCD的改进**—

   - 您可以使用 `VERBOSE_COMMANDS` 和 `SCD_COMPRESSION_LEVEL` 环境变量（在两者期间） _生成_ 和部署阶段(_P)。<!-- MAGECLOUD-1819 -->

   - 修复了在遇到“ ”的意外值时导致部署失败并出现随机错误的问题 `SCD_COMPRESSION_LEVEL` 环境变量。 改进了配置验证以提供有意义的通知。 请参阅 [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) 以获取可接受的值。<!-- MAGECLOUD-2043 -->

   - 修复了 `SCD_COMPRESSION_LEVEL` 环境变量配置流程，以便覆盖按预期工作。<!-- MAGECLOUD-2044 -->

   - 修复了阻止配置 `SCD_THREADS` 中的环境变量 `.magento.env.yaml` 文件 _部署_ 暂存。<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![新建图标](../../assets/new.svg) **静态内容部署(SCD)** — 提供了一个新的替代部署流程，可在收到请求时生成静态内容（按需）。 这减少了停机时间，并通过生成最关键的资产改进了缓存处理。<!-- MAGECLOUD-1285 -->

   - **新环境变量** — 添加了 `SCD_ON_DEMAND` 可在请求时生成静态内容的全局环境变量。<!-- MAGECLOUD-1738 -->

   - **部署后挂接** — 添加了 `post_deploy` 钩点 `.magento.app.yaml` 清除缓存并预加载(warms)缓存的文件 _之后_ 容器开始接受连接。 它仅适用于包含暂存环境和生产环境的Pro项目。 [!DNL Cloud Console] 以及入门项目。 尽管不是必需的，但是这与 `SCD_ON_DEMAND` 环境变量。<!-- MAGECLOUD-1788 -->

- ![新建图标](../../assets/new.svg) **优化** — 优化了在部署期间移动或复制文件的过程，以提高部署速度并减少文件系统的负载。<!-- MAGECLOUD-1842 -->

- ![新建图标](../../assets/new.svg) **部署日志记录** — 添加了启用Syslog和Graylog扩展日志格式(GELF)处理程序以便在部署过程中输出日志的功能。 请参阅 [日志记录处理程序](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![新建图标](../../assets/new.svg) 添加了以下内容 [**环境变量**](../environment/variables-intro.md)：

   - `CRYPT_KEY` — 在移动数据库时向另一个环境提供加密密钥。<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_全局_ 跳过在中复制静态视图文件的环境变量 `var/view_preprocessed` 目录，并在请求时生成缩小的HTML。<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_全局_ 环境变量，以便在请求时生成静态内容。<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` — 可以列出用于预加载高速缓存的页面。 新版中提供 [部署后变量](../environment/variables-post-deploy.md).

- ![修复图标](../../assets/fix.svg) 修复了本地应用的修补程序中断实例上的部署的问题。 现在，ECE-Tools可以检测到已应用了修补程序。<!-- MAGECLOUD-982 -->

- ![修复图标](../../assets/fix.svg) 修复了JavaScript捆绑与GZIP功能之间的冲突。 现在，这些功能可正确配合使用。<!-- MAGECLOUD-1735 -->

- ![修复图标](../../assets/fix.svg) 修复了在使用更早的PHP 7.0.x版本时导致ECE-Tools CLI命令失败的问题。<!-- MAGECLOUD-1744 -->

- ![修复图标](../../assets/fix.svg) 修复了阻止在多个线程中使用压缩策略的静态内容部署的问题。<!-- MAGECLOUD-1822 -->

- ![修复图标](../../assets/fix.svg) 修复了导致管理员登录延迟的Redis会话锁定问题。 此外，此修补程序还可用于2.1.x。<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>您必须 [升级Adobe Commerce on cloud基础架构内涵](../dev-tools/install-package.md#update-the-metapackage) 以获取此内容和所有未来更新。

- ![新建图标](../../assets/new.svg) **ece-tools** — 此 `ece-tools` 包现在支持Adobe Commerce 2.1.x。<!-- MAGECLOUD-1086 -->

- ![新建图标](../../assets/new.svg) **Redis配置** — 您现在可以 [配置Redis](../environment/variables-deploy.md#cache_configuration) 页面和默认缓存，以及Redis会话存储（使用环境变量）。<!-- MAGECLOUD-1552 -->

- ![新建图标](../../assets/new.svg) **搜索、AMQP和Redis服务改进** — 我们统一了服务配置流程，以便所有服务的行为现在都相同。 手动编辑 `env.php` 不再支持用于配置服务的文件。 您必须使用环境变量或 `.magento.env.yaml` 文件。<!-- MAGECLOUD-1437 -->

- ![修复图标](../../assets/fix.svg) **环境变量**—

   - 使用 `env:STATIC_CONTENT_THREADS` 已弃用，将在未来版本中删除。 使用 [SCD_THREADS](../environment/variables-deploy.md#scd_threads) 而是。<!-- MAGECLOUD-1507 -->

   - 此 `STATIC_CONTENT_EXCLUDE_THEMES` 已弃用环境变量。 您必须使用 `SCD_EXCLUDE_THEMES` 环境变量。<!-- MAGECLOUD-1640 -->

- ![修复图标](../../assets/fix.svg) **记录** — 我们简化了内置修补操作的日志记录。<!-- MAGECLOUD-1674 -->

- ![修复图标](../../assets/fix.svg) 已删除 `developer` 模式支持和 `APPLICATION_MODE` 环境变量，因为它们引起了意外行为。<!-- MAGECLOUD-1615 -->

- ![修复图标](../../assets/fix.svg) 我们修复了导致与Redis相关的静态内容部署失败的问题。 现在，多线程静态内容部署可按设计要求运行。<!-- MAGECLOUD-1630 -->

- ![修复图标](../../assets/fix.svg) 我们修复了导致用户无法在管理员中保存对配置字段的修改的问题，在运行 `app:config:dump` 命令。<!-- MAGECLOUD-1175 -->

- ![修复图标](../../assets/fix.svg) 我们增加了对早期版本的支持 `symfony/yaml` 用于修复与某些尚未与最新版本兼容的包之间的冲突。<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>我们合并了 `vendor/magento/ece-patches` 替换为 `vendor/magento/ece-tools` 在此版本中。 您无需再更新 `vendor/magento/ece-patches` 单独打包。

**新增功能：**

- **改进了日志记录**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - 我们改进了日志消息，以便在构建或部署过程覆盖环境变量时提供更好的解释。
   - 您现在可以实时查看安装和升级进度。 跟踪 `install_update.log` 查看进度的文件。 例如，

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **新建cron命令** — 您现在可以解锁特定的cron停滞作业，而不是使用 [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) 命令。 在2.1中不可用。<!-- MAGECLOUD-1367 -->

- **统一配置文件** — 您现在可以使用来配置生成和部署阶段 [`.magento.env.yaml`](https://devdocs.magento.com/cloud/project/magento-env-yaml.html) 文件。<!-- MAGECLOUD-1369 -->

- **备份配置文件** — 部署过程现在会自动创建 `app/etc/env.php` 和 `app/etc/config.php` 配置文件部署后。 我们还添加了 [新建CLI命令](https://support.magento.com/hc/en-us/articles/360033182871) 以从备份还原这些配置文件。<!-- MAGECLOUD-1372 -->

- **验证错误疑难解答** — 我们更改了以下情况下必须使用的命令以解决验证错误： `config.php` 未包含足够的数据来进行静态内容部署。 以前，错误消息指示您运行 `bin/magento app:config:dump`. 现在，您必须运行 `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **新环境变量** — 您现在可以使用环境变量来连接自定义 [搜索](../environment/variables-deploy.md#search_configuration) 和 [基于AMQP](../environment/variables-deploy.md#queue_configuration) 对您网站的服务。<!-- MAGECLOUD-1410 -->

- 我们实施了智能修补。 现在，该软件包应用的修补程序不是基于Adobe Commerce on cloud infrastructure版本，而是基于打补丁的软件包版本。<!--MAGECLOUD-1090-->

**已解决的问题：**

- 我们修复了导致生成错误的日志记录问题。<!-- MAGECLOUD-1162 -->

- 我们修复了在交互模式下运行部署时导致超时异常的问题。<!-- MAGECLOUD-1389 -->

- 我们修复了在使用压缩策略生成静态内容时导致错误的问题。 在2.1中不可用。<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- 我们修复了导致部署脚本无法正确识别暂存环境和生产环境的问题。<!-- MAGECLOUD-1493 -->

- 我们修复了在安装和升级过程中导致网络问题中断数据库连接并造成故障的问题。<!-- MAGECLOUD-1520 -->

- 我们修复了导致无法使用导出配置文件的问题 `app:config:dump` 不止一次。 在2.1中不可用。<!--  MAGECLOUD-1567  -->

- 我们修复了Redis会话 _锁定_ 导致出现 _管理员_ 登录延迟。 在2.1中不可用。<!--  MAGECLOUD-1582  -->

- 我们修复了与版本控制相关的实施问题，该问题会导致与其他基于编辑器的修补模块发生冲突。<!-- MAGECLOUD-1450 -->

- 我们修复了导入期间导致PHP内存问题的情况。<!-- MAGECLOUD-1310 -->

- 已移除修补程序；修复中的错误 `colinmollenhour/credis` v1.6，在cloud infrastructure 2.2.1上启用对Adobe Commerce的支持。在2.1中不可用。<!-- MAGECLOUD-1033 -->

## v2002.0.7

**已解决的问题：**

- 已删除 `var/view_preprocessed` 符号链接，以修复导致JavaScript缩小冲突的问题。<!-- MAGECLOUD-1454 -->

## v2002.0.6

**已解决的问题：**

- 我们修复了导致错误的问题 `gzip` 文件或目录名称包含空格时出错。<!-- MAGECLOUD-1413 -->

- 我们修复了导致部署脚本无法正确识别和启用模块依赖项的问题。<!-- MAGECLOUD-1424 -->

## v2002.0.5

**新增功能：**

- **使用环境变量配置cron消费者** — 您现在可以使用新的 `CRON_CONSUMERS_RUNNER` 环境变量。

- **配置扫描** — 现在，我们在构建/部署过程中扫描重要组件，如果扫描失败，则停止该过程，从而防止由于站点处于维护模式而造成的不必要的停机。

- **生成/部署通知** — 我们添加了一个配置文件，您可以 [设置Slack和/或电子邮件通知](../environment/set-up-notifications.md) 用于所有环境中的生成/部署操作。

- **静态内容压缩** — 我们现在使用压缩静态内容 [gzip](https://www.gnu.org/software/gzip/) 在构建和部署阶段执行。 此压缩与Fastly压缩相结合，有助于减小存储的大小并提高部署速度。 如有必要，您可以使用禁用压缩 [构建选项](../environment/variables-build.md) 或 [部署变量](../environment/variables-deploy.md). 有关更多信息，请参阅以下主题：

   - [应用程序环境变量](../application/variables-property.md)

   - [静态内容部署性能](../deploy/static-content.md)

   - [部署过程](https://devdocs.magento.com/cloud/reference/discover-deploy.html)

- **配置管理** — 我们现在自动生成一个 `app/etc/config.php` 文件（如果尚未存在）。 自动生成的文件仅包含模块和扩展名的列表。 如果文件已存在，则构建阶段将继续正常进行。 如果您关注 [配置管理](../store/store-settings.md) 之后，这些命令更新文件而不需要额外的步骤。 请参阅 [部署过程](https://devdocs.magento.com/cloud/reference/discover-deploy.html) 以了解更多信息。

- **数据库转储** — 我们添加了 `magento/ece-tools` 用于在所有环境中创建数据库转储的CLI命令。 对于Pro Plan生产环境，此命令仅从三个高可用性节点中的一个转储，因此转储期间写入其他节点的生产数据可能不会被复制。 我们建议先将应用程序置于维护模式，然后再在生产环境中执行数据库转储。 请参阅 [备份管理](https://devdocs.magento.com/cloud/project/project-webint-snap.html#db-dump) 以了解更多信息。

- **Cron间隔限制已取消** — 在us-3、eu-3和ap-3区域中配置的所有环境的默认cron间隔为1分钟。 对于专业集成环境，所有其他区域的默认cron间隔为5分钟；对于专业暂存和生产环境，默认间隔为1分钟。 要修改现有cron作业，请在中编辑您的设置 `.magento.app.yaml` 或为生产/暂存环境创建支持工单。 请参阅 [设置cron作业](../application/crons-property.md#set-up-cron-jobs) 以了解更多信息。

**已解决的问题：**

- 我们修复了由于部署过程调用 `cache-clean` 静态内容部署之前的操作。<!-- MAGECLOUD-1327 -->

- 我们修复了在生产环境中部署的静态内容生成步骤期间导致错误的问题。<!-- MAGECLOUD-1322 -->

- 我们修复了一个问题，该问题导致 `magento/ece-tools` 命令将输出记录到 `stderr`.<!-- MAGECLOUD-1264 -->

- 我们修复了中阻止基本URL值的问题 `env.php` 在分支中更新的。<!-- MAGECLOUD-1242 -->

- 我们修复了导致 `magento setup:install` 命令添加不安全的前缀(`http://`)以保护基本URL。<!-- MAGECLOUD-1171 -->

- 我们修复了阻止修补程序错误导致部署失败的问题。<!-- MAGECLOUD-1170 -->

- 我们修复了一个问题，该问题导致 `ece-tools` 停止执行并在无法应用修补程序时引发异常。<!-- MAGECLOUD-1152 -->

- 我们修复了在管理员中启用HTML缩小后加载店面时导致错误的问题。<!-- MAGECLOUD-1138 -->

## v2002.0.4

**已解决的问题：**

- 您现在可以 [手动重置卡住的cron作业](https://support.magento.com/hc/en-us/articles/360033099451) 通过SSH访问在所有环境中使用CLI命令。 部署过程会自动重置cron作业。<!-- MAGECLOUD-1355 -->

## v2002.0.3

**已解决的问题：**

- 我们修复了由于Redis读取/写入时间过长而导致页面超时的问题。 您现在可以使用 `disable_locking` redis配置中的参数以防止出现此问题。<!-- MAGECLOUD-1311 -->

## v2002.0.2

**已解决的问题：**

- 此 [!DNL RabbitMQ] 配置过程现在可自动获取所有必需的参数。<!-- MAGECLOUD-1246 -->

## v2002.0.1

**新增功能：**

- 云基础架构上的Adobe Commerce现在支持范围和 [静态内容部署策略](https://devdocs.magento.com/guides/v2.3/config-guide/cli/config-cli-subcommands-static-deploy-strategies.html). 我们已添加 `–s` 默认设置的参数 `quick` 用于静态内容部署策略。 您可以使用环境变量 [SCD_STRATEGY](../environment/variables-deploy.md) 在生成和部署操作中，自定义和使用这些策略。 此变量支持选项 `standard`， `quick`，或 `compact`. 如果您选择 `compact`，我们会覆盖 `STATIC_CONTENT_THREADS` 值 `1`，可能会减慢部署速度，在生产环境中尤其如此。 在2.1中不可用。<!--- MAGECLOUD-1057 -->

- 我们在环境中创建了一个日志文件，用于捕获和编译生成和部署操作。 此 `var/log/cloud.log` 文件位于根应用程序目录中。<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**已解决的问题：**

- 已重构 `ece-tools` 软件包，以使其与Adobe Commerce on cloud infrastructure 2.2.0及更高版本兼容。<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- 我们修复了导致无法恢复的问题 `ece-tools` 停止执行并在无法应用修补程序时引发异常。<!-- MAGECLOUD-1186 -->

- 我们修复了在生成期间跳过依赖项注入(di)编译时导致引发异常的问题。<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- 我们修复了导致部署过程覆盖中的自定义Redis配置的问题 `env.php` 文件。<!-- MAGECLOUD-1019 -->

- 我们修复了导致重定向循环的问题，该问题导致默认安全管理员处于禁用状态。<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>此包不再与云基础架构上的其他Adobe Commerce版本兼容，并且 **不应该** 使用。

### 初始版本

的初始版本 `ece-tools` 适用于Adobe Commerce on cloud infrastructure 2.2.0的。
