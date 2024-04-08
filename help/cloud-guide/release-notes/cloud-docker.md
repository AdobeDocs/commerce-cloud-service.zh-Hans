---
title: Cloud Docker包
description: 请参阅Cloud Docker包的最新改进列表。
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: 907d977f-2e9c-4553-a46b-000bc6a57b28
source-git-commit: bc76cba0219f16fd055c20289811b51c35c9b026
workflow-type: tm+mt
source-wordcount: '3662'
ht-degree: 0%

---

# Cloud Docker包

此 [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) 包提供了用于将Adobe Commerce部署到本地云环境的功能和Docker图像。 以下发行说明介绍了此软件包的最新改进，它是 [Cloud Tools Suite for Commerce](cloud-tools-suite.md).

此 `magento/magento-cloud-docker` 包使用以下版本序列： `<major>.<minor>.<patch>`

发行说明包括：

- ![新建图标](../../assets/new.svg) 新增功能
- ![修复图标](../../assets/fix.svg) 修复和改进功能

<!--Add release notes below-->

## v1.3.7 {#latest}

发行日期： 2024年4月8日

- ![新建图标](../../assets/new.svg) **PHP**  — 添加了对PHP 8.3和PHP 8.3图像的支持。
- ![新建图标](../../assets/new.svg) **恩金克斯**  — 添加了图像nginx版本1.24。
- ![新建图标](../../assets/new.svg) **Opensearch**  — 添加了OpenSearch v. 2.12、1.3图像。
- ![新建图标](../../assets/new.svg) **Composer**  — 已将Composer版本更新为2.2.23。

## v1.3.6

发行日期： 2023年7月31日

- ![新建图标](../../assets/new.svg) **已添加新的服务版本**—OpenSearch 2.5。
- ![新建图标](../../assets/new.svg) **启用编辑器缓存** — 现在，您可以扩展Docker配置以在启动Docker容器时启用Composer清除缓存。 请参阅 [扩展Docker配置](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) 在 _Cloud Docker for Commerce_ 指南。

## v1.3.5

发行日期： 2023年3月10日

- ![新建图标](../../assets/new.svg) **ionCube** — 为PHP 8.1映像添加了ionCube扩展。
- ![新建图标](../../assets/new.svg) **已添加新的服务版本**—OpenSearch 2.3和2.4、PHP 8.2、Varnish 7.1.1。
- ![新建图标](../../assets/new.svg) **对PHP 8.2的增强支持** — 修复了某些PHP 8.2.x版本的兼容性问题以支持Commerce 2.4.6。
- ![修复图标](../../assets/fix.svg) **编辑器问题** — 修复了在Docker容器中更新Composer版本后发生的问题。

## v1.3.4

发行日期： 2022年10月27日

- ![新建图标](../../assets/new.svg) **添加了新的清漆图像** — 添加了有关Varnish 6.5、7.0和7.1的图像。<!-- MCLOUD-7879 -->

## v1.3.3

发行日期： 2022年9月13日

- ![新建图标](../../assets/new.svg) **Apple M1 (ARM64)支持** — 添加了对Docker映像的更改，以支持Apple M1 (ARM64)体系结构。<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![修复图标](../../assets/fix.svg) **Mailhog** — 修复了在开发人员模式下Mailhog服务未捕获电子邮件的问题。<!-- MCLOUD-8643 -->
- ![修复图标](../../assets/fix.svg) **init-docker.sh** — 修复了中的服务版本验证器 `init-docker.sh` 脚本。<!-- MCLOUD-8765 -->

## v1.3.2

发行日期： 2022年3月31日

- ![新建图标](../../assets/new.svg) **添加了Elasticsearch7.10图像**<!-- MCLOUD-8548 -->

## v1.3.1

发行日期： 2022年3月10日

- ![新建图标](../../assets/new.svg) **支持PHP 8.1** — 添加了对PHP 8.1的支持。
- ![新建图标](../../assets/new.svg) **OpenSearch** — 添加了OpenSearch版本1.1和1.2的图像。
- ![新建图标](../../assets/new.svg) **Composer 2.1** — 在PHP 8.x映像中默认设置编辑器2.1.x。
- ![新建图标](../../assets/new.svg) **PHP映像改进**—

   - 添加了PHP 8.1图像
   - 已升级xDebug版本3.1.2
   - 已升级xmlrpc 1.0.0RC3

- ![修复图标](../../assets/fix.svg) **Elasticsearch和OpenSearch改进** — 改进了Elasticsearch和OpenSearch Dockerfiles；删除了Elasticsearch5.2映像。
- ![修复图标](../../assets/fix.svg) **钠离子延伸** — 已启用 `sodium` 在所有PHP映像中默认扩展。
- ![修复图标](../../assets/fix.svg) **Composer缓存卷** — 修复了包含缓存的编辑器包的编辑器缓存卷的路径。
- ![修复图标](../../assets/fix.svg) **nginx中的内存限制** — 修复了NGINX映像中的内存限制。

## v1.3.0

发行日期： 2021年10月25日

- ![修复图标](../../assets/fix.svg) **改进开发人员模式工作流** — 以前，您需要在生成和部署步骤中指定模式。 现在， `--mode` 中的选项 `build` 步骤确定后面的模式 `deploy` 步骤。 不再需要设置部署后的模式。 请参阅 [开发人员模式](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html).<!-- ACMP-1086 -->
- ![修复图标](../../assets/fix.svg) **只读文件系统的改进**—<!-- ACMP-1106 -->
   - 修复了启动邮件配置的PHP容器时出现的问题。
   - 可以在INI文件中使用环境变量。
   - 确保PHP入口点不需要写入权限。
- ![修复图标](../../assets/fix.svg) **更新节点** — 更新捆绑的Node版本；在PHP-CLI映像中安装Node时，它现在使用当前的LTS版本。<!-- ACMP-1539 -->
- ![修复图标](../../assets/fix.svg) **更新Symfony** — 更新了Symfony配置依赖项，使其与Adobe Commerce 2.4.4兼容。<!-- ACMP-1533 -->

## v1.2.4

发行日期： 2021年7月29日

- ![新建图标](../../assets/new.svg) **新建 `Zookeeper` 容器** — 添加了 [Zookeeper容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#zookeeper-container) 管理未在Cloud基础架构上部署到Adobe Commerce的项目的锁定提供程序配置。<!--MCLOUD-8000-->

- ![新建图标](../../assets/new.svg) **添加了对Composer 2.0的支持。** — 将Composer版本2.0添加到Composer配置文件中，以支持从Composer 1.0进行升级（该版本已接近生命周期结束）。<!--MCLOUD-8003-->

## v1.2.3

发布日期： 2021年6月14日

- ![新建图标](../../assets/new.svg) **添加了PHP 8.0** — 已将PHP更新到版本8.0，允许您利用PHP 8.0包含的所有新增功能和优化。<!--MCLOUD-7941-->
- ![新建图标](../../assets/new.svg) **更新至清漆6.6和Elasticsearch7.11.2** — 以下链接提供了以下版本信息： [清漆缓存6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) 和Elasticsearch7.11.2。<!--MCLOUD-7921-->
- ![新建图标](../../assets/new.svg) **已添加 `ioncube` PHP 7.4图像的扩展** — 此 `ioncube` 扩展最初从PHP 7.3升级到PHP 7.4时被排除之后，已重新添加到PHP 7.4映像中。 *[由mattskr提交](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![新建图标](../../assets/new.svg) **添加了文件同步选项：`manual-native`** — 此 `manual-native` 文件同步选项提供对同步的手动控制，从而为macOS和Windows环境提供最佳性能。 阅读有关使用 `manual-native` 中的选项 [开发人员模式](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) 和 [在Docker开发人员环境中同步数据](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html#file-synchronization-options).<!--MCLOUD-7977-->
- ![新建图标](../../assets/new.svg) **已从中删除卷删除 `up` 和 `down` 命令** — 此 `--volume` 选项已从 `bin/magento-docker up` 和 `bin/magento-docker down` 命令，替换为新的 `bin/magento-docker init` 命令，出现数据丢失警告。 此更改有助于防止意外数据丢失。 *[由joeshelton-wagento提交](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![修复图标](../../assets/fix.svg) **已更新 `CN` 生成的证书的值** — 删除了硬编码 `CN` 值。 此值创建了一个证书错误(`NET::ERR_CERT_INVALID`)而导致 `--host` 的选项 `ece-docker build:compose` 命令将被忽略。<!--MCLOUD-7934-->

## v1.2.2

发行日期： 2021年4月20日

- ![新建图标](../../assets/new.svg) **已更新 `host.docker.internal` 独立于平台** — 您现在可以为Ubuntu、Windows和macOS创建相同的Docker撰写脚本。 在Ubuntu上使用Xdebug不再需要单独的环境变量。 [由Igor Vitol提交的修复](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![新建图标](../../assets/new.svg) **更新了init-docker.sh** — 添加了 `mounts` 对象 `MAGENTO_CLOUD_APPLICATION` 环境变量。 [由Chiranjevi提交的修复](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![新建图标](../../assets/new.svg) **更新了init-docker.sh** — 更新了 `init-docker.sh` PHP 7.4和Cloud Docker 1.2.1版本的脚本。 [由Adarsh Manickam提交的修复](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![新建图标](../../assets/new.svg) **缺省情况下启用钠** — 已启用 `sodium` 默认情况下，PHP扩展位于PHP Docker映像中。<!--MCLOUD-7548-->
- ![新建图标](../../assets/new.svg) **`custom-registry`option** — 添加了 `--custom-registry` 选项至 `php ./vendor/bin/ece-docker build:compose` 命令使用您自己的图像注册表。<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![新建图标](../../assets/new.svg) **已删除旧Elasticsearch版本** — 从Elasticsearch图像中删除了Elasticsearch版本1.7和2.4。<!--MCLOUD-7504-->
- ![新建图标](../../assets/new.svg) **自动生成NGINX证书** — 从NGINX映像中删除了现有证书。 现在，每个新部署都会自动生成NGINX证书，以提高安全性。<!--MCLOUD-7396-->
- ![修复图标](../../assets/fix.svg) **已启用`opcache.validate_timestamps`** — 已启用 `opcache.validate_timestamps` 默认情况下，在开发人员模式下设置PHP。 启用此设置修复了在Docker中无法识别文件系统更改的问题。<!--MCLOUD-7466-->
- ![修复图标](../../assets/fix.svg) **固定`build:custom:compose`** — 修复了 `build:custom:compose` 命令，用于在构建过程中无法覆盖文件时引发错误。 抛出错误可防止出现以下情况 `docker-compose up` 可能使用了错误的文件。<!--MCLOUD-7457-->
- ![修复图标](../../assets/fix.svg) **固定 `--sync_engine="native"` option** — 修复了生产模式下的问题(`--mode="production"`)，则 `--sync_engine="native"` 选项不会在中为本地文件夹创建任何条目 `docker.composer.yml` 文件。<!--MCLOUD-7254-->
- ![修复图标](../../assets/fix.svg) **修复了服务版本验证错误** — 已添加的服务版本 [!DNL RabbitMQ]、Elasticsearch和其他服务 `type` 中的属性 `MAGENTO_CLOUD_RELATIONSHIP` 变量。 将这些版本添加到 `relationships` 变量修复了在部署阶段发生的验证错误。<!--MCLOUD-7572-->

## v1.2.1

发行日期： 2020年12月21日

- ![新建图标](../../assets/new.svg) **NGINX命令选项** — 添加了用于更改NGINX编号的构建命令选项 `worker_processes` 和NGINX `worker_connections` 用于TLS和Web服务。 此 `worker_process` 参数保留将值设置为 `auto`. 示例： <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![新建图标](../../assets/new.svg) **TLS命令选项** — 添加了生成命令选项，以创建不带TLS服务的配置。 示例： <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![新建图标](../../assets/new.svg) **NGINX内存消耗** — 减少了NGINX进程用于TLS和Web服务的内存。<!--MCLOUD-7259-->

- ![新建图标](../../assets/new.svg) **Blackfire** — 在Cloud Docker映像中默认禁用BlackfirePHP扩展。

- ![修复图标](../../assets/fix.svg) **PHP-FPM容器** — 通过更改PHP-FPM容器运行状况检查 `WEB_PORT` 从 `80` 到 `8080`.<!--MCLOUD-7232-->

- ![修复图标](../../assets/fix.svg) **无效的卷命名** — 修复了在开发人员模式下卷命名无效的错误。<!--MCLOUD-7442-->

- ![修复图标](../../assets/fix.svg) **NGINX上游端口** — 更新了Docker NGINX 1.19映像以使用端口8080以避免无限循环。 [由Adarsh Manickam提交的修复](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

发行日期： 2020年11月9日

- ![新建图标](../../assets/new.svg) **容器更新 —**

   - ![新建图标](../../assets/new.svg) **PHP-FPM容器** — 添加了对gnupg PHP扩展的支持。 [Zilker Technology的G Arvind提交的修复](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![修复图标](../../assets/fix.svg) **数据库容器** — 通过将所需的数据库密码添加到运行状况检查命令，修复了数据库容器运行状况检查问题。<!--MCLOUD-7122-->

   - ![新建图标](../../assets/new.svg) **Elasticsearch容器**

      - 添加了对Elasticsearch7.9的支持，以便与即将发布的Adobe Commerce版本兼容。<!--MCLOUD-7190-->

      - **插件配置Elasticsearch** — 增加了对使用中的Elasticsearch插件配置信息的支持 `services.yaml` 文件以生成 `docker-compose.yaml` Cloud Docker for Commerce环境的文件。 请参阅 [Elasticsearch插件](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Elasticsearch插件支持** — 添加了对以下Elasticsearch插件的支持： `analysis-icu`， `analysis-phonetic`， `analysis-stempel`、和 `analysis-nori`. 此 `analysis-icu` 和 `analysis-phonetic` 插件默认处于安装状态。 您可以添加或删除 `analysis-stempel` 和 `analysis-nori` 插件。<!--MCLOUD-2789-->

   - ![新建图标](../../assets/new.svg) **CLI容器**

      - **在Docker PHP容器中运行命令** — 现在，您可以使用Cloud Docker CLI在Docker环境中的PHP容器中运行命令，而无需在主机上安装PHP。 例如，以下命令构建配置：  `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. 请参阅 [Cloud Docker CLI](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli). [Zilker Technology的G Arvind提交的修复](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - 将OpenSSH-client添加到PHP CLI容器。 现在，您可以在以下情况下为编辑器使用ssh-agent转发 `composer.json` 文件包含私有Git存储库，这些存储库要求ssh客户端使用编辑器命令。<!--MCLOUD-6008-->

   - ![修复图标](../../assets/fix.svg) **TLS容器** — 现在， [TLS容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) 基于 `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker映像而非CentOS映像。 此更改修复了在Cloud Docker环境中的容器之间发送HTTPS请求时导致错误的问题。<!--MCLOUD-6469-->

   - ![新建图标](../../assets/new.svg) **测试容器** — 添加了一个用于应用程序测试的测试容器，并添加了 `--with-test` Docker选项 `build:compose` 命令创建容器，仅在Docker环境中测试时。 请参阅 [应用程序测试](https://devdocs.magento.com/cloud/docker/docker-test-app-mftf.html).<!--MCLOUD-6394-->

   - ![新建图标](../../assets/new.svg) **FPM-XDEBUG容器**

      - ![新建图标](../../assets/new.svg) **在Linux上配置Xdebug** — 添加了 `--set-docker-host` 选项 `ece-docker build:compose` 命令配置 `host.docker.internal` 值。 在Linux系统上使用Xdebug时需要此选项。 请参阅 [为Docker配置Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-6430-->

      - ![修复图标](../../assets/fix.svg) 修复了要解析的Docker ENTRYPOINT的Xdebug变量配置 `uninitialized "with_xdebug" variable` 日志中有错误。 [由Florent Olivaud提交的修复](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![新建图标](../../assets/new.svg) **Docker配置更改**

   - **MailHog配置** — 现在您可以使用以下功能 `ece-docker build:compose` 用于禁用MailHog并指定端口的命令选项： `--no-mailhog`， `--mailhog-http-port`、和 `--mailhog-smtp-port`. 请参阅 [设置电子邮件](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - 对于Cloud Docker for Commerce 1.2.0及更高版本，Adobe现在为每个修补程序版本提供Docker图像，并且Docker配置生成器使用指定的修补程序版本创建Docker配置，而不是使用最新的修补程序版本。 以前，Docker配置生成器使用最新修补程序版本构建配置，这可能会破坏使用早期版本构建的Cloud Docker for Commerce环境。<!--MCLOUD-7093-->

   - **在自定义Cloud Docker配置中指定自定义图像和版本** — 更新了 `build:custom:compose` 命令，带有在生成自定义Docker编写配置文件时指定自定义图像和版本的选项(`docker-compose.yaml`)。 请参阅 [构建自定义Docker撰写配置](https://devdocs.magento.com/cloud/docker/docker-config-sources.html#build-a-custom-docker-compose-configuration). <!--MCLOUD-7089-->

   - 更新了Docker主机配置以公开端口443以启用对Adobe Commerce的访问(`https://magento2.docker`)。 您可以通过添加 `--tls-port` 选项。<!--MCLOUD-6806-->

- ![修复图标](../../assets/fix.svg) 修复了以下情况下导致Cloud Docker for Commerce构建失败的问题： `app/etc/env.php` 文件存在。<!--MCLOUD-6732-->

- ![修复图标](../../assets/fix.svg) 更新了生成配置以将命名卷替换为常规卷，以防止在Linux或Windows子系统for Linux (WSL2)上部署Cloud Docker for Commerce时出现问题。<!--MCLOUD-6732-->

- ![修复图标](../../assets/fix.svg) 更新了Cloud Docker for Commerce功能测试以支持Composer 2.0。<!--MCLOUD-7183-->

## v1.1.2

发行日期： 2020年9月9日

- ![新建图标](../../assets/new.svg) 添加了对Elasticsearch7.7的支持<!--MCLOUD-6219-->

## v1.1.1

发行日期： 2020年8月5日

- ![修复图标](../../assets/fix.svg) **已更新电子邮件配置** — 更新了默认的Cloud Docker for Commerce配置以支持MailHog服务，而不是使用SendMail。 请参阅 [设置电子邮件](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-5624-->

- ![修复图标](../../assets/fix.svg) 已将PS库还原到Cloud Docker环境配置以进行修复 `ps:  command not found` 错误。<!--MCLOUD-6621-->

- ![修复图标](../../assets/fix.svg) 更新了默认的Cloud Docker for Commerce配置以删除要修复的数据库入口点和MariaDB卷的自动装载 `Cannot create container for service db` 启动Cloud Docker环境时可能发生的错误。

  现在，您可以将Cloud Docker环境配置为通过将以下选项添加到 `ece-docker build:compose` 命令： `--with-entry-point` 和 `with-mariadb-conf`. 请参阅 [服务配置选项](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-6424-->

- ![新建图标](../../assets/new.svg) **CLI命令更新**

| 操作 | 命令 |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 向数据库容器添加入口点，以从备份还原数据库 | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| 添加MariaDB配置卷 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

发布日期： 2020年6月25日

- ![新建图标](../../assets/new.svg) **增加了对拆分数据库性能解决方案的支持** — 现在，您可以使用Cloud Docker环境中的拆分数据库性能解决方案配置和部署存储。<!--MCLOUD-3740-->

- ![新建图标](../../assets/new.svg) **支持Adobe Commerce和Magento Open Source部署** — 现在，您可以使用Cloud Docker for Commerce为未在云基础架构上的Adobe Commerce上托管的项目部署本地开发环境。<!--MCLOUD-5667-->

- ![新建图标](../../assets/new.svg) **Blackfire.io支持** — 添加了对使用 [Blackfire.io扩展](https://devdocs.magento.com/cloud/docker/docker-config-blackfire-io.html) 以进行自动性能测试。 [由Adarsh Manickam从Zilker Technology提交的修复](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![新建图标](../../assets/new.svg) **容器更新**

   - **清漆** — 现在，在Cloud Docker环境中使用受支持的云应用程序模板版本部署Adobe Commerce时，Varnish是默认缓存。 请参阅 [清漆容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container).<!--MCLOUD-2634-->

   - 添加了 `--no-varnish` 用于在生成Cloud Docker配置文件时跳过Varnish服务安装的选项。<!--MCLOUD-2634-->

   - ![新建图标](../../assets/new.svg) **数据库**

      - 添加了对MySQL数据库的支持。 现在，您可以使用MariaDB或MySQL配置Cloud Docker环境。 请参阅 [服务配置选项](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-5691-->

      - 添加了生成Docker组合文件时为数据库复制设置增量设置和偏移设置的功能。 请参阅 [服务容器](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers).<!--MCLOUD-5735-->

   - ![新建图标](../../assets/new.svg) **PHP-FPM**

      - 添加了对PHP 7.4的支持。 [Mohanela Murugan从Zilker Technology提交的修复](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - 添加了复制 `php.ini` 将根项目目录中的文件应用到Cloud Docker环境，并将自定义PHP设置应用到PHP-FPM和CLI容器。 请参阅 [自定义PHP设置](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#customize-php-settings). [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - 添加了容器运行状况检查。 [Visanth Sampath提交的来自Zilker Technology的修复](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![修复图标](../../assets/fix.svg) **Node.js** — 将默认Node.js版本从版本8更新到版本10，以提高安全性。 Node.js版本8已弃用，不会再更新为错误修复或安全修补程序。 [Zilker Technology的Mohan Elamurugan提交的修复](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![新建图标](../../assets/new.svg) **Elasticsearch**

      - 添加了对Elasticsearch6.8、7.2、7.5和7.6的支持。<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - 添加了自定义 [Elasticsearch容器配置](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) 生成Docker编写配置文件时。<!--MCLOUD-3059-->

      - 添加了 `--no-es` 用于生成Docker编写配置文件的服务配置选项的选项。 使用此选项可跳过Elasticsearch容器安装，改用MySQL搜索。 仅Adobe Commerce版本2.3.5及更早版本支持此选项。<!--MCLOUD-3766-->

   - ![新建图标](../../assets/new.svg) **FPM-XDEBUG容器** — 添加了服务配置选项，用于在Cloud Docker环境中安装和配置Xdebug以调试PHP。 请参阅 [配置Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-4098-->

- ![新建图标](../../assets/new.svg) **Docker配置更改**

   - 为PHP-FPM、Redis、Elasticsearch和MySQL Docker服务容器添加了运行状况检查。<!--MCLOUD-3335 and MCLOUD-5856-->

   - 已将默认文件同步模式更改为 `native` 处于开发人员模式时。<!--MCLOUD-3890 -->

   - 在生成时，向通用Docker服务容器图像添加了版本信息 `docker-compose.yml` 文件。<!--MCLOUD-3878-->

   - 通过增加 `fastcgi_buffers` Nginx服务器的值。<!--MCLOUD-5980-->

   - 通过添加第二个同步会话来同步中的文件，改进了突变文件同步性能。 `vendor` 目录。 此更改可防止突变在文件同步过程中卡住。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![新建图标](../../assets/new.svg) **CLI命令更新**

| 操作 | 命令 |
| -------- | --------------- |
| 清除Redis缓存 | `bin/magento-docker flush-redis` |
| 清除清漆缓存 | `bin/magento-docker flush-varnish` |
| 跳过默认清漆安装 | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [自定义Elasticsearch选项](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [删除Elasticsearch配置](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| 使用MySQL版本5.6或5.7配置数据库容器 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| 指定自定义基本URL | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [添加Xdebug配置的容器](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![修复图标](../../assets/fix.svg) 修复了诱变文件同步的配置，以防止诱变创建陈旧会话。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![修复图标](../../assets/fix.svg) 修复了在启动PHP-FPM容器时导致Docker撰写日志中语法错误的一个配置问题。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![修复图标](../../assets/fix.svg) 修复了在使用多个Docker环境时有时发生的卷冲突错误。 [Zilker Technology的G Arvind提交的修复](https://github.com/magento/magento-cloud-docker/pull/168).

- ![修复图标](../../assets/fix.svg) 修复了导致出现错误的问题 `ece-docker build:compose` 命令在配置包含Blackfire.io时失败。 [Zilker Technology的G Arvind提交的修复](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![修复图标](../../assets/fix.svg) 更新了PHP CLI映像配置，以防止在使用Cloud Docker for Commerce安装多个包时发生内存不足错误。 [Zilker Technology的Mohan Elamurugan提交的修复](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![修复图标](../../assets/fix.svg) 在Cloud Docker环境中添加了对多个MySQL用户的支持。 在早期版本中， `build:compose` 操作失败，如果 `magento.app.yaml` 文件指定了多个数据库用户。 [Zilker Technology的G Arvind提交的修复](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![修复图标](../../assets/fix.svg) 已删除 `rsyslog` 从Cloud Docker for Commerce PHP容器中，解决在部署期间导致警告通知的兼容性问题。 Cloud Docker不使用rsyslog实用程序。<!--MCLOUD-6173-->

## v1.0.0

发行日期：2020年2月5日

- ![新建图标](../../assets/new.svg) **已创建单独的包以投放`Cloud Docker for Commerce`** — 从移动了源代码以交付Cloud Docker for Commerce `ece-tools` 存储库到 [新建 `magento-cloud-docker` 存储库](https://github.com/magento/magento-cloud-docker) 保持代码质量并提供独立的版本。 新软件包依赖于ECE-Tools v2002.1.0及更高版本。

  当您更新ece-tools时，也会更新 `magento/magento-cloud-docker` 包到版本1.0.0。如果您之前使用了Cloud Docker for Commerce `ece-tools` 版本(2002.0.x)，查看 [向后不兼容性](backward-incompatible-changes.md) 并根据需要以脚本、命令和进程形式更新项目。

- ![新建图标](../../assets/new.svg) **向Docker图像添加了版本控制** — 您必须立即更新 `magento/magento-cloud-docker` 包以获取更新的图像。<!--MAGECLOUD-4737-->

- ![新建图标](../../assets/new.svg) **容器更新**—

   - ![新建图标](../../assets/new.svg) **PHP-FPM容器**—

      - ![新建图标](../../assets/new.svg) **添加了Node.js支持** — 更新了PHP-FPM映像以支持PHP容器内的节点、 npm和grunt-cli功能。<!--MAGECLOUD-3953-->

      - ![新建图标](../../assets/new.svg) **添加了对的支持 [ionCube](https://www.ioncube.com/)** — 更新了默认Docker配置以支持本地Docker开发环境中的ionCube。<!--MAGECLOUD-4354-->

   - ![新建图标](../../assets/new.svg) **Web容器**—

      - ![新建图标](../../assets/new.svg) **自定义NGINX配置** — 添加了装载定制产品的功能 `nginx.conf` 文件到Cloud Docker for Commerce环境。 请参阅 [Web容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#web-container).<!--MAGECLOUD-4204-->

      - ![新建图标](../../assets/new.svg) **自动生成的NGINX证书**— Docker配置文件现在包括用于为Web容器自动生成NGINX证书的配置。<!--MAGECLOUD-4258-->

   - ![新建图标](../../assets/new.svg) **新建Selenium容器** — 添加了 [硒容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#selenium-container) 以使用Magento功能测试框架(MFTF)支持Adobe Commerce应用程序测试。<!--MAGECLOUD-4040-->

   - ![新建图标](../../assets/new.svg) **[!DNL RabbitMQ]版本支持** — 更新了 [!DNL RabbitMQ] 要支持的容器配置 [!DNL RabbitMQ] 版本3.8。<!--MAGECLOUD-4674-->

   - ![修复图标](../../assets/fix.svg) **持久数据库容器** — 此 `magento-db: /var/lib/mysql` 在您停止并删除Docker配置并在重新启动Docker配置时恢复后，数据库卷现在仍然存在。 现在，您必须手动删除数据库卷。 请参阅 [数据库容器].<!--MAGECLOUD-3978-->

   - ![新建图标](../../assets/new.svg) **TLS容器**—

      - ![新建图标](../../assets/new.svg) **更新了容器基本图像以使用官方图像** — 此 [Cloud TLS容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) 图像是基于官方网站上的 `debian:jessie` Docker图像……<!--MAGECLOUD-4163-->

      - ![新建图标](../../assets/new.svg) **添加了对 [井号TLS终止代理]** — 此 [井号配置文件](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) 添加以下ENV变量以自定义TLS容器的Docker配置：

         - **`TimeOut`** — 设置首字节时间(TTFB)超时值。 默认值为300秒。

         - **`RewriteLocation`** — 确定磅代理是否默认将位置重写到请求URL。 默认为 `0` 以防止重写中断对外部网站（如外部SSO网站）的重定向。 [索林·舒格提交的修正](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![新建图标](../../assets/new.svg) 将TLS容器配置中的超时值从15秒增加到300秒。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![新建图标](../../assets/new.svg) **清漆容器**—

      - ![新建图标](../../assets/new.svg) **更新了容器基本图像以使用官方图像** — 此 [云上光容器](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) 现在是基于官方的 `centos` Docker图像。<!--MAGECLOUD-4163-->

      - ![新建图标](../../assets/new.svg) **改进了默认超时配置** — 已添加 `.first_byte_timeout` 和 `.between_bytes_timeout` 配置到清漆容器。 这两个超时值均默认为 `300s` （5分钟）。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![修复图标](../../assets/fix.svg) **在Xdebug会话期间跳过清漆** — 更新了清漆容器配置以返回 `pass` 在启用Xdebug时收到的请求时。 在以前的版本中，如果Docker环境包含Varnish，则无法使用Xdebug。 [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![新建图标](../../assets/new.svg) **Docker配置更改**—

   - ![新建图标](../../assets/new.svg) **管理项目的装载和卷** — 增加了在启动Docker环境以进行本地开发时管理挂载和卷的功能。 请参阅 [共享项目数据].<!--MAGECLOUD-3248-->

   - ![新建图标](../../assets/new.svg) **支持网桥模式** — 增加了对网络桥接模式的支持，以通过本地网络启用Docker容器之间的连接。<!--MAGECLOUD-4165-->

   - ![新建图标](../../assets/new.svg) **默认禁用Cron容器** — 为了提高性能，在构建Docker环境时，默认情况下不再配置Cron容器。 您可以使用 `--with-cron` 选项，用于向环境添加Cron容器。 请参阅 [管理cron作业](https://devdocs.magento.com/cloud/docker/docker-manage-cron-jobs.html).<!--MAGECLOUD-5181-->

   - ![新建图标](../../assets/new.svg) **停止同步大型备份文件** — 将数据库转储和存档文件（ZIP、SQL、GZ和BZ2）添加到的 `dist/docker-sync.yml` 和 `dist/mutagen.sh` 文件。 同步大型文件(>1 GB)可能会导致一段时间不活动，并且备份文件通常不需要同步，因为您可以重新生成它们。<!--MAGECLOUD-3979-->

- ![新建图标](../../assets/new.svg) **命令更改**—

   - ![修复图标](../../assets/fix.svg) 已重命名 `./bin/docker` 文件到 `./bin/magento-docker` 修复由于以下原因导致某些Docker环境中断的问题： `./bin/docker` 文件覆盖现有Docker二进制文件。 这是 [向后不兼容的更改](backward-incompatible-changes.md) 需要更新脚本和命令。<!-- MAGECLOUD-4038 -->

   - ![新建图标](../../assets/new.svg) **添加了一个服务配置选项，用于将数据库端口公开给主机** — 使用 `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` 选项，用于在生成数据库端口时将数据库端口公开给主机。 `docker-compose.yml` 文件： `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![新建图标](../../assets/new.svg) **新的部署后命令** — 以前，在 `.magento.app.yaml` 文件在使用将Adobe Commerce部署到Cloud Docker容器后自动运行 `cloud-deploy` 命令。 现在，您必须发出一个单独的 `cloud-post-deploy` 命令以在部署后运行部署后挂接。 请参阅更新的启动说明，了解 [开发人员](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) 和 [生产](https://devdocs.magento.com/cloud/docker/docker-mode-production.html) 模式。<!--MAGECLOUD-3996-->

   - ![新建图标](../../assets/new.svg) 添加了 `--rm` 选项至 `./bin/magento-docker` 生成和部署容器的命令。 这会在任务完成后删除容器。<!--MAGECLOUD-4205-->

   - ![新建图标](../../assets/new.svg) **更新至 `build:compose` 命令**—

      - ![新建图标](../../assets/new.svg) 添加了 `--sync-engine="native"` 选项 `docker-build` 命令以在开发人员模式下生成Docker撰写配置文件时禁用文件同步。 在Linux系统上开发时，使用此选项，这些系统不需要文件同步以进行本地Docker开发。 请参阅 [在Docker环境中同步数据](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![新建图标](../../assets/new.svg) 已将默认文件同步设置从 `docker-sync` 到 `native`. [由Zilker Technology的Mathew Beane提交的修复](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![新建图标](../../assets/new.svg) **验证改进**—

   - ![新建图标](../../assets/new.svg) 向本地Docker开发环境的部署过程添加了验证，以验证云环境配置是否包含解密数据库所需的加密密钥。 现在，如果环境配置未指定加密密钥的值，则日志中会显示错误消息。<!--MAGECLOUD-4423-->

   - ![新建图标](../../assets/new.svg) 已将容器运行状况检查添加到Elasticsearch服务，以确保该服务在继续生成和部署处理之前已准备就绪。 如果运行状况检查返回错误，容器将自动重新启动。<!--MAGECLOUD-4456-->
