---
title: ECE-Tools发行说明
description: 请参阅ECE-Tools软件包的最新改进列表。
recommendations: noDisplay, catalog
last-substantial-update: 2024-01-16T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: f2aa4aa183298d829d27c4641f21acef1514d312
workflow-type: tm+mt
source-wordcount: '2887'
ht-degree: 0%

---

# ECE-Tools发行说明

此 [ece-tools](https://github.com/magento/ece-tools) 包是一组脚本和工具，用于管理和部署云项目。 以下发行说明介绍了此软件包的最新改进，它是 [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>请参阅 [升级ECE-Tools](../dev-tools/update-package.md) 有关更新到最新版本的信息 `ece-tools` 包。

此 `ece-tools` 包使用以下版本版本控制顺序： `200<major>.<minor>.<patch>`

发行说明包括：

- ![新建图标](../../assets/new.svg) 新增功能
- ![修复图标](../../assets/fix.svg) 修复和改进功能

<!--Add release notes below-->

## v2002.1.17 {#latest}

发行日期： 2024年1月16日

- ![修复图标](../../assets/fix.svg) **Elasticsearch和OpenSearch验证器** — 修复了在启用LiveSearch时安装搜索服务时生成误导性消息的验证器。<!-- MCLOUD-10167 -->
- ![修复图标](../../assets/fix.svg) **部署警告** — 修复了导致出现有关非空文件夹的部署警告的问题。<!-- MCLOUD-8958 -->

## v2002.1.16

发行日期： 2023年10月16日

- ![新建图标](../../assets/new.svg) **ENABLE_WEBHOOKS全局环境变量** — 添加了 [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) 与Commerce Webhook一起使用以连接到外部端点的全局变量，如App Builder运行时操作或第三方库存管理系统。

## v2002.1.15

发行日期： 2023年7月31日

- ![修复图标](../../assets/fix.svg) **错误代码** — 更新了错误代码架构和错误代码文档生成器。
- ![修复图标](../../assets/fix.svg) **自定义Redis模型的验证器** — 更新了自定义Redis后端模型的验证器。 [有关缓存配置，请参阅示例](../environment/variables-deploy.md#cache_configuration).
- ![修复图标](../../assets/fix.svg) **RabbitMQ验证器** — 添加了对RabbitMQ 3.11的支持
- ![修复图标](../../assets/fix.svg) **修复了错误的链接** — 修复了欢迎电子邮件模板中指向载入文档的错误链接。

## v2002.1.14

发行日期： 2023年3月10日

- ![新建图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.2的支持。
- ![新建图标](../../assets/new.svg) **服务的验证器** — 更新了Commerce 2.4.6所需服务的验证器：MariaDB 10.6、Redis 7.0、PHP 8.2、OpenSearch 2.x和RabbitMQ 3.9。
- ![修复图标](../../assets/fix.svg) **ece-tools db-dump** — 修复了导致出现错误的问题。 `db-dump` 操作提前停止。

## v2002.1.13

发行日期： 2022年10月27日

- ![新建图标](../../assets/new.svg) **添加了对Adobe CommerceAdobe I/O事件的支持**. 扩展开发人员现在可以使用 [Adobe I/O事件](https://developer.adobe.com/events/docs/) 框架，用于将云实例中的Commerce事件信息发送到为其编写的应用程序 [Adobe应用程序生成器](https://developer.adobe.com/app-builder/docs/overview/). Adobe Commerce的Adobe I/O事件位于“合作伙伴预览”中。<!-- CEXT-932 -->
- ![新建图标](../../assets/new.svg) **OPcache配置验证器** — 添加了一个验证器，用于检查已排除路径的OPcache配置。<!-- MCLOUD-9485 -->
- ![修复图标](../../assets/fix.svg) **修复了GraphQL缓存配置的问题** — 现在ECE-Tools保留了GraphQL `id_salt` 中的值 `cache` 中的配置 `app/etc/env.php` 文件。<!-- MCLOUD-9486 -->

## v2002.1.12

发行日期： 2022年9月13日

- ![新建图标](../../assets/new.svg) **启用`synchronous_replication`**—ECE-Tools集 `synchronous_replication=>true` 在 `app/etc/env.php` 文件时间 `MYSQL_USE_SLAVE_CONNECTION` 已启用。 此配置仅影响Commerce 2.4.6+。 请参阅 `MYSQL_USE_SLAVE_CONNECTION` 中的变量说明 [部署变量](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![新建图标](../../assets/new.svg) **OpenSearch** — 添加了配置和设置 `opensearch` Adobe Commerce下一版本2.4.6的引擎。请参阅 [设置OpenSearch服务](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

发行日期： 2022年8月4日

- ![修复图标](../../assets/fix.svg) **ElasticSuite Validator和OpenSearch** — 修复了安装OpenSearch时的ElasticSuite完整性检查验证器问题。<!-- MCLOUD-8767 -->
- ![修复图标](../../assets/fix.svg) **部署命令的返回类型** — 修复了部署命令的返回类型。<!-- AC-3208 -->
- ![修复图标](../../assets/fix.svg) **[!DNL RabbitMQ]安装新的Commerce 2.4.5时出现问题** — 固定 [!DNL RabbitMQ] 新Commerce 2.4.5安装时出现崩溃问题。<!-- MCLOUD-9059 -->

## v2002.1.10

发行日期： 2022年3月31日

- ![修复图标](../../assets/fix.svg) **Elasticsearch7.10** — 更新了验证器以支持7.10版本的Elasticsearch。<!-- MCLOUD-8548 -->

## v2002.1.9

发行日期： 2022年3月10日

- ![新建图标](../../assets/new.svg) **OpenSearch** — 添加了对Adobe Commerce版本2.4.4、2.4.3-p2和2.3.7-p3的OpenSearch的支持。<!-- MCLOUD-8296 -->
- ![新建图标](../../assets/new.svg) **PHP** — 添加了对PHP 8.1的支持。
- ![修复图标](../../assets/fix.svg) **交响乐/过程** — 添加了与symfony/进程^5.3的兼容性。<!-- MCLOUD-8283 -->

- ![新建图标](../../assets/new.svg) **使用方多个流程** — 添加了 `multiple_processes` 选项，以便指定每个使用者要衍生的进程数。 请参阅 `CRON_CONSUMERS_RUNNER` 中的变量说明 [部署变量](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![新建图标](../../assets/new.svg) **OpenSearch方案和完整主机路径** — 添加了配置Elasticsearch方案和完整主机路径的功能。
- ![修复图标](../../assets/fix.svg) **AWS S3** — 更改了AWS S3启用方法。
- ![修复图标](../../assets/fix.svg) **修复driver_options读取器** — 添加了从数据库连接读取driver_options配置的功能 `env.php` 文件创建者 `ece-tools` 用于验证器。<!-- MCLOUD-8420 -->

## v2002.1.8

发行日期： 2021年10月25日

- ![新建图标](../../assets/new.svg) **替代转储位置** — 添加了 `--dump-directory` 选项，以便您能够为数据库转储选择目标目录。 现在 `/app/var/dump-main` 是数据库转储的默认目标目录。 请参阅 [备份管理：转储数据库](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![修复图标](../../assets/fix.svg) **更新独白** — 更新了 `monolog` 打包到 `^2.3`.<!-- ACMP-1263 -->
- ![修复图标](../../assets/fix.svg) **更新Symfony** — 更新了Symfony依赖项以与Adobe Commerce 2.4.4兼容。<!-- ACMP-1533 -->
- ![修复图标](../../assets/fix.svg) **功能/解决自动加载** — 修复了部署到集成环境并看到 `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` 错误。<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

发行日期： 2021年7月29日

**配置更新**—

- ![新建图标](../../assets/new.svg) 添加了对Composer 2.0的支持。<!--MCLOUD-8003-->

- ![修复图标](../../assets/fix.svg) **更新了以下项的编辑器要求`symphony/console`** — 更新了ECE-Tools `composer.json` 的版本要求 `symphony/console` 用于修复导致异常的问题 `di:compile` 命令失败，出现以下错误： `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![修复图标](../../assets/fix.svg) 更新了软件生命周期结束检查(`eol.yaml`)以包含Elasticsearch7.9.x。<!--MCLOUD-7938-->

## v2002.1.6

发行日期： 2021年4月20日

- ![新建图标](../../assets/new.svg) **Redis身份验证凭据** — 增加了从读取Redis授权凭据的功能 `relationships` 属性。<!--MCLOUD-7694-->

- ![新建图标](../../assets/new.svg) **授权凭据Elasticsearch** — 增加了从读取Elasticsearch授权凭据的功能 `relationships` 属性。<!--MCLOUD-7695-->

- ![新建图标](../../assets/new.svg) **专用会话存储服务** — 已添加 `redis-session` 作为会话存储的第二个选项。 您可以使用 `redis-session` 服务来存储会话信息并使用 `redis` 服务，以提供更好的性能。<!--MCLOUD-7698-->

- ![新建图标](../../assets/new.svg) **已弃用的SPLIT_DB消息** — 为已弃用的添加了验证器警告和关键消息 `SPLIT_DB` 适用于Adobe Commerce 2.4.2的选项以及在Adobe Commerce 2.5.0中删除该选项。<!--MCLOUD-7806-->

- ![修复图标](../../assets/fix.svg) **关系中的Elasticsearch版本** — 修复了服务验证器，以从检索正确的Elasticsearch版本 `relationships` Cloud Docker和集成环境中的属性。<!--MCLOUD-7572-->

- ![修复图标](../../assets/fix.svg) **灵活的Redis端口验证**—Redis现在可以从的自定义缓存连接中验证端口 `server` URL。 例如，您可以将端口号添加到服务器URL中，如下所示： `server: 'tcp://rfs-store-simple-page-cache:26379'`. 这有助于防止出现以下验证错误： `port` 选项缺失或不正确。<!--MCLOUD-7722-->

- ![修复图标](../../assets/fix.svg) **升级到Adobe Commerce 2.4.2** — 修复了需要用户手动运行的问题 `bin/magento setup:upgrade` 以使其站点在升级到Adobe Commerce 2.4.2后可运行。<!--MCLOUD-7776-->

## v2002.1.5

发行日期： 2021年2月1日

- ![新建图标](../../assets/new.svg) **远程存储** — 添加了 `REMOTE_STORAGE` 环境变量，启用云项目以便使用存储服务(如AWS S3)远程存储媒体文件。 此配置选项是ECE-Tools包的一部分，但在云基础架构上的Adobe Commerce上不受支持。<!--MCLOUD-7153-->

- ![新建图标](../../assets/new.svg) **新建 `cloud:config:validate` 命令** — 已添加命令 `php vendor/bin/ece-tools cloud:config:validate` 验证 `.magento.env.yaml` 配置，然后将更改推送到远程云环境。<!--MCLOUD-7120-->

- ![新建图标](../../assets/new.svg) **刷新opcache** — 增加了对 `opcache.enable_cli` PHP选项，用于在运行部署挂接之前刷新OPcache。 此配置将重置缓存配置，以确保当前配置设置应用于每个部署。<!--MCLOUD-7015-->

- ![新建图标](../../assets/new.svg) **验证Aurora DB** — 更新了数据库服务验证，使其与Aurora数据库兼容。<!--MCLOUD-7269-->

- ![新建图标](../../assets/new.svg) **新建SCD_NO_PARENT环境变量** — 添加了 `SCD_NO_PARENT` 环境变量(适用于Adobe Commerce >=2.4.2)，用于管理为父主题生成静态内容。<!--MCLOUD-7284-->

- ![修复图标](../../assets/fix.svg) **内存限制和命令** — 修复了以下问题： `php vendor/bin/ece-tools` 如果 `cloud.log` 文件超出PHP memory_limit。 而不是阅读整个 `cloud.log` 现在，我们只从日志文件中读取较小的数据子集。<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![修复图标](../../assets/fix.svg) **自定义数据库连接** — 修复了 `.magento.env.yaml` 在其中为定义自定义数据库连接的配置问题 `DATABASE_CONFIGURATION` 未使用。 未将连接设置添加到 `app/etc/env.php`.<!--MCLOUD-7426-->

- ![修复图标](../../assets/fix.svg) **空错误日志** — 修复了在以下情况下导致部署失败的问题： `cloud.error.log` 为空。<!--MCLOUD-7296-->

- ![修复图标](../../assets/fix.svg) **MariaDB 10.3验证** — 修复了对Adobe Commerce 2.3.6-p1的MariaDB 10.3的验证。<!--MCLOUD-7416-->

- ![修复图标](../../assets/fix.svg) **缓存：刷新日志记录** — 改进了日志条目，以指示 `cache:flush` 步骤。<!--MCLOUD-7503-->

## v2002.1.4

发行日期： 2020年11月19日

- ![修复图标](../../assets/fix.svg) 修复了在指定的搜索引擎导致部署失败的问题。 `SEARCH_CONFIGURATION` 环境变量是以下值以外的值： `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

发行日期： 2020年11月9日

**基础架构更新**—

- ![新建图标](../../assets/new.svg) 为只读环境添加了ECE-Tools支持 `pub/static` 目录（当静态内容设置为在构建阶段中部署时）。<!--MC-37699-->

- ![新建图标](../../assets/new.svg) 添加了对Elasticsearch7.9和Redis 6的支持，以便与即将发布的Adobe Commerce版本兼容。<!--MCLOUD-7191-->

- ![修复图标](../../assets/fix.svg) 更新了ECE工具 `composer.json` 为“质量修补程序工具”添加所需的依赖关系。 这修复了ECE-Tools和magento-cloud-patches软件包之间存在的循环依赖关系。<!--MCLOUD-6910-->

**验证和日志改进**—

- ![新建图标](../../assets/new.svg) 添加了搜索引擎验证，以确保 `elasticsearch` 为Adobe Commerce on cloud infrastructure 2.4及更高版本设置。 如果验证失败，部署将停止，并显示一条关键错误消息，建议修复此问题。 请参阅 [严重错误，部署阶段](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![新建图标](../../assets/new.svg) 添加了Elasticsearch验证，以检查Elasticsearch服务版本与Adobe Commerce版本之间的兼容性。<!--MCLOUD-7193-->

- ![新建图标](../../assets/new.svg) 更新了Elasticsearch兼容性错误消息，以显示与Adobe CommerceElasticsearch模块兼容的Elasticsearch版本。 现在，错误消息会提供要在您的Cloud基础架构中安装的特定Elasticsearch版本，以便与您的Adobe Commerce版本使用的Elasticsearch模块兼容。 请参阅 [警告错误，部署阶段](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![新建图标](../../assets/new.svg) 添加了警告错误 `2026` 和 `2027` 无效 `MAGE_MODE` 环境变量设置。 唯一有效值为 `production`. 进行此修复之前， `MAGE_MODE` 可以设置为 `developer` 没有部署错误，只会在以后尝试写入只读文件时导致错误。 请参阅 [警告错误](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![修复图标](../../assets/fix.svg) 修复了对Redis、RabbitMQ和MySQL服务的验证，以确保这些版本与Adobe Commerce版本兼容。 这些服务的有效版本现在会写入 `cloud.log`.<!--MCLOUD-7098-->

- ![修复图标](../../assets/fix.svg) 已更新 `cloud.log` 在缓存预热期间包括发送请求的并发请求限制。 此值配置于 [WARMUP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) 部署后变量。<!--MCLOUD-5563-->

**CLI命令更新**—

- ![新建图标](../../assets/new.svg) 添加了CLI命令(`cloud:config:create` 和 `cloud:config:update`)，以创建和更新 `.magento.env.yaml` 文件，其配置可包含一个或多个生成、部署和部署后变量。 请参阅 [从CLI创建配置文件](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**环境变量更新**—

- ![新建图标](../../assets/new.svg) 添加了 [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) 生成变量。 将变量设置为 `true` 停止应用程序运行 `composer dump-autoload` Cloud Docker for Commerce安装期间的命令。 变量仅与具有可写文件系统（使用为测试和开发而创建）的Cloud Docker for Commerce容器相关 `./vendor/bin/ece-docker build:compose --with-test`)。 对于此类安装，跳过 `composer dump-autoload` 命令可防止在运行其他命令时尝试从已删除的文件中访问文件时出错 `generated` 目录。<!--MCLOUD-6939-->

## v2002.1.2

发行日期： 2020年8月5日

**验证和日志改进**—

- ![新建图标](../../assets/new.svg) 添加了 `schema.error.yaml` 文件，其中包含构建、部署和部署后过程中可能发生的所有错误和警告通知，以及解决错误的建议。 此文件中的信息也可在 _Commerce云指南_. 请参阅 [ece-tools的错误消息引用](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![新建图标](../../assets/new.svg) 更改了云错误日志(`/var/log/cloud.error.log`)条目，以便于以编程方式解析日志。<!--MCLOUD-5879-->

- ![新建图标](../../assets/new.svg) 为生成、部署和部署后处理添加了其他错误检查，并改进了现有检查：

   - 错误代码2026 — 未能将构建阶段生成的一些数据恢复到装入的目录

   - 错误代码3004 — 无法创建备份文件

   - 错误代码102 — 添加了额外检查，以查找在 `env.php` 文件不可写 <!--MCLOUD-6221-->

- ![新建图标](../../assets/new.svg) 添加了 **质量PATCH** 环境变量，用于指定要在部署过程中应用的一个或多个质量修补程序。 请参阅 [生成变量](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

发布日期： 2020年6月25日

- ![新建图标](../../assets/new.svg) **基础架构更新**—

   - ![新建图标](../../assets/new.svg) **日志记录改进** — 改进了日志跟踪功能，为关键部署错误分配退出代码并在错误消息通知和日志事件中公开退出代码。 请参阅 [ece-tools的错误消息引用](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![新建图标](../../assets/new.svg) 改进了数据库转储过程(`vendor/bin/ece-tools db-dump`)并更新了日志消息，以明确说明数据库转储操作会将应用程序切换到维护模式，停止使用者队列进程，并在转储开始之前禁用cron作业。<!--MCLOUD-5324, MCLOUD-2062-->

   - ![修复图标](../../assets/fix.svg) 修复了一个问题，以确保在部署到暂存环境和生产环境时项目URL会正确更新。 现在， `ece-tools` 将URL用于路由，并且 `primary:true` 在项目工艺路线配置中设置的属性。 请参阅 [部署变量](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![修复图标](../../assets/fix.svg) 已更新 `generate.xml` 用于应用修补程序的生成方案工作流。 必须提前应用修补程序以更新Adobe Commerce，从而修复可能导致 `di:compile` 和 `module:refresh` 失败的步骤。<!--MCLOUD-5941-->

   - ![修复图标](../../assets/fix.svg) 修复了安装过程中错误返回 `Crypt key missing` 错误。 此 `crypt/key` 值在安装期间自动生成。<!--MCLOUD-6120-->

- ![新建图标](../../assets/new.svg) **服务更新**—

   - ![新建图标](../../assets/new.svg) 添加了对PHP 7.4和MariaDB 10.4的支持。<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![新建图标](../../assets/new.svg) **环境变量更新**—

   - ![新建图标](../../assets/new.svg) 添加了 **SCD_USE_BALER** 变量启用启用打包模块，以便在Adobe Commerce on Cloud基础架构构建过程中捆绑使用JavaScript。 请参阅中的变量说明 [生成变量](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![新建图标](../../assets/new.svg) 添加了 **REDIS_BACKEND** 用于为Adobe Commerce 2.3.5或更高版本的Redis缓存配置Redis后端模型的环境变量。 请参阅中的变量说明 [部署变量](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![新建图标](../../assets/new.svg) **CLI命令更新**—

   - ![新建图标](../../assets/new.svg) 更新了以下CLI命令，并增加了用于获取更详细日志记录的选项：

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     每个调用的日志记录级别由 [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) 中的变量 `.magento.env.yaml` 文件。<!--MCLOUD-3503-->

- ![新建图标](../../assets/new.svg) **验证改进**—

   - ![新建图标](../../assets/new.svg) **Elasticsearch7.x兼容性检查** — 更新了Elasticsearch7.x软件兼容性检查的Elasticsearch验证。<!--MCLOUD-5542-->

   - ![新建图标](../../assets/new.svg) **更新了服务版本和EOL验证检查** — 更新了验证，以根据Adobe Commerce 2.4的要求检查已安装的服务版本。<!--MCLOUD-6144-->

   - ![修复图标](../../assets/fix.svg) 修复了一个验证问题，以便仅当 `post-deploy` 中缺少挂接配置 `.magento.app.yaml` 文件：

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![新建图标](../../assets/new.svg) **添加了对Zend Framework依赖项的验证** — 为已迁移到Laminas项目的Zend框架添加了编辑器依赖关系验证。 如果缺少所需的依赖关系，则在构建过程中会显示以下错误消息。

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     请参阅 [验证Zend Framework依赖项](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![新建图标](../../assets/new.svg) **已添加验证 `env.php` 文件和数据** — 添加了对的检查 `env.php` 文件和数据。<!--MCLOUD-5991-->

      - 如果 `env.php` 安装中缺少文件，并且 `crypt/key` 值未在 `.magento.app.yaml` 文件，部署失败并显示以下通知：

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - 如果安装不包含 `env.php` 文件，或者配置仅包含一个缓存类型 `cron:enable` 命令在升级过程中运行，以使用所有 `cache_types`. 以下通知将添加到日志中：

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

发行日期： 2020年2月6日

- ![新建图标](../../assets/new.svg) **基础架构更新**—

   - ![新建图标](../../assets/new.svg) **为Cloud Docker for Commerce添加了单独的包** — 将Docker软件包与 `ece-tools` 包以保持代码质量并提供独立的版本。 与相关的更新和修复 `ece-tools` 管理自 [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) github存储库。<!--MAGECLOUD-2927-->

   - ![新建图标](../../assets/new.svg) **更新了修补功能** — 将修补功能从ECE-Tools软件包移到单独的 [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) 包。 在部署期间， `ece-tools` 使用新软件包来应用修补程序。 请参阅 [云修补程序发行说明](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![新建图标](../../assets/new.svg) **更新了编辑器依赖项** — 更新了 `composer.json` 云基础架构上Adobe Commerce的文件，该文件依赖于 `magento/magento-cloud-docker` 包。 现在， `ece-tools` 包含中所有包的依赖关系 [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). 安装或更新时，会自动安装和更新这些软件包 `ece-tools`.

- ![新建图标](../../assets/new.svg) **支持基于场景的部署**—<!--MAGECLOUD-4101-->

   - ![新建图标](../../assets/new.svg) 现在，您可以使用XML配置文件自定义生成、部署和部署后流程，以覆盖或自定义默认配置。

   - ![新建图标](../../assets/new.svg) **已更改 `hooks` 中的配置`.magento.app.yaml`** — 我们更新了 `hooks` 配置格式，用于支持基于方案的部署。 旧版ECE-Tools 2002.0.x仍然受支持。 但是，必须更新为新格式才能使用基于场景的部署功能。 请参阅 [基于方案的部署](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>在更新至ECE-Tools 2002.1.0版之前，请查阅 [向后不兼容的更改](backward-incompatible-changes.md) 了解可能需要您更新Adobe Commerce的云基础架构项目配置或流程更改。

- ![新建图标](../../assets/new.svg) **服务更新**—

   - ![新建图标](../../assets/new.svg) 添加了对PHP 7.3的支持。<!--MAGECLOUD-4022-->

   - ![新建图标](../../assets/new.svg) 添加了对RabbitMQ 3.8的支持。<!--MAGECLOUD-4674-->

   - ![新建图标](../../assets/new.svg) 添加了验证功能，以便根据每项服务的生命周期结束日期来检查已安装的服务版本。 现在，如果服务版本在EOL日期后的三个月内，客户将收到通知，如果EOL日期在过去，客户将收到警告。<!--MAGECLOUD-4076-->

   - ![修复图标](../../assets/fix.svg) 修复了Elasticsearch配置问题，以确保在所有环境中都配置了正确的Elasticsearch设置。<!--MAGECLOUD-4474-->

>[!NOTE]
>
>请参阅 [服务版本](../services/services-yaml.md#service-versions) 有关Adobe Commerce上用于云基础架构的服务及其版本与Cloud模板的兼容性的列表。

- ![新建图标](../../assets/new.svg) **环境变量更新**—

   - ![新建图标](../../assets/new.svg) 扩展了 `WARM_UP_PAGES` 环境变量，用于支持特定产品页面的缓存预加载。 有关扩展定义，请参阅 [部署后变量](../environment/variables-post-deploy.md#warm_up_pages) 主题。<!--MAGECLOUD-4444-->

   - ![新建图标](../../assets/new.svg) 添加了 `ERROR_REPORT_DIR_NESTING_LEVEL` 环境变量来简化中的错误报表数据管理 `<magento_root>/var/report/` 目录。 请参阅中的变量说明 [生成变量](../environment/variables-build.md#error_report_dir_nesting_level) 主题。

   - ![修复图标](../../assets/fix.svg) 已删除 `SCD_EXCLUDE_THEMES`， `STATIC_CONTENT_THREADS`，`DO_DEPLOY_STATIC_CONTENT`、和 `STATIC_CONTENT_SYMLINK` 环境变量。 请参阅 [向后不兼容的更改](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![修复图标](../../assets/fix.svg) 修复了弹性套件配置过程中的问题，以便在配置 `ELASTICSUITE_CONFIGURATION` 部署变量，但不部署 `_merge` 选项。<!--MAGECLOUD-4388-->

- ![新建图标](../../assets/new.svg) **CLI命令更新**—

   - ![新建图标](../../assets/new.svg) **新建cron命令** — 您现在可以使用在Adobe Commerce上的云基础架构环境中手动管理cron处理 `cron:disable` 和 `cron:enable` 命令。 使用disable命令停止所有活动的cron进程并禁用所有cron作业。 准备就绪后，使用enable命令重新启用cron作业。 请参阅 [禁用cron作业](../application/crons-property.md#disable-cron-jobs).

   - ![新建图标](../../assets/new.svg) **改进了错误报告** — 为ECE-Tools处理期间发生的CLI命令失败添加了更好的日志记录。<!--MAGECLOUD-4849-->

   - ![新建图标](../../assets/new.svg) **删除已弃用的生成命令** — 删除了以下生成命令： `m2-ece-build`， `m2-ece-deploy`， `m2-ece-scd-dump`，并重命名 `ece-tools docker` 命令 `ece-docker`. 请参阅 [向后不兼容的更改](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![新建图标](../../assets/new.svg) 删除已弃用的 `build_options.ini` 并添加了验证，以便在文件存在的情况下使构建失败。 使用 [.magento.env.yaml](../environment/configure-env-yaml.md) 文件以配置生成选项。

- ![修复图标](../../assets/fix.svg) 修复了以下情况时导致构建过程失败的问题 `config.php` 文件为空。<!--MAGECLOUD-4127-->

## 2002.0.23

发行日期： 2020年2月27日

- ![修复图标](../../assets/fix.svg) 修复了与的兼容性问题 `ece-tools` 2002.0.x版本，该版本阻止按需静态内容生成在生产模式下成功完成。

## 旧版本

请参阅 [发行说明存档](cloud-release-archive.md) 适用于版本2002.0.22及更早版本。
