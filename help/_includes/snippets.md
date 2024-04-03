---
source-git-commit: b08443d937dfc18120daa0d6a1277b9c7bca67aa
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---
# 云片段

## Elasticsearch警告 {#elasticsearch-support}

>[!WARNING]
>
>云基础架构上的Adobe Commerce不支持Elasticsearch7.11及更高版本。 Adobe Commerce版本2.3.7-p3、2.4.3-p2、2.4.4及更高版本支持OpenSearch服务。 内部部署安装继续支持Elasticsearch。

## 增强的集成 {#enhanced-integration-envs}

>[!NOTE]
>
>在2020年6月5日之前配置的项目具有多个较小的集成环境。 如果您需要更大的集成环境来进行测试和开发，请请求升级到增强集成环境。 请参阅 [集成环境请求](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) 中的文章 _Adobe Commerce帮助中心_ 以了解详细信息。

## 合并选项 {#merge-options}

默认情况下，部署过程会覆盖 `env.php` 文件；但是，您可以选择合并服务配置的一个或多个值而不覆盖所有值。

设置 `_merge` 选项更改为以下选项之一：

- `true`—**合并** 配置的服务值以及环境变量值。
- `false`—**覆盖** 配置的服务值以及环境变量值。

## 专用存储库 {#private-repository}

>[!NOTE]
>
>Adobe强烈建议为Adobe Commerce on cloud infrastructure项目使用专用存储库来保护任何专有信息或开发工作，例如扩展和敏感配置。

## 专业自助警告 {#pro-self-service-warning}

>[!WARNING]
>
>部分 **Pro项目** 需要支持票证，以更新 `routes.yaml` 文件和cron配置 `.magento.app.yaml` 文件。 Adobe建议在集成环境中更新和测试YAML配置文件，然后将更改部署到暂存环境。 如果您在重新部署后未将更改应用于暂存站点，并且日志中没有相关的错误消息，则您可以 **必须** [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 描述尝试的配置更改。 在票证中包含任何更新的YAML配置文件。

## 专业服务支持 {#pro-update-service}

>[!TIP]
>对于Pro项目，您必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 安装或更新 [服务](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html) 在 `Staging` 和 `Production` 仅限环境。
>
>指明所需的服务更改，包括您更新的 `.magento.app.yaml` 和 `services.yaml` 文件，并在票证中声明PHP版本。 有关对PHP版本、扩展或环境设置的自助更改，请参见 [PHP设置](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/php-settings.html) 在 _应用程序配置_.
>
>对于的更改 _实时_ 生产环境(**仅限Pro**)，您必须至少提前48小时通知，以便Cloud Infrastructure团队有充足的时间来调配资源并进行安全升级。

## 专业备份 {#pro-backups}

>[!TIP]
>
>在专业暂存和生产环境中，您必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 在票证中检索注明日期、时间和时区的特定备份。
>
>Adobe会 **非** 从自动备份中恢复任何环境。 请参阅 [从暂存或生产环境恢复数据库快照](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) 以帮助选择还原暂存或生产快照的方法。

## 重新部署警告 {#redeploy-warning}

>[!WARNING]
>
>当您执行合并、推送或同步环境时，或者当您触发手动重新部署时，部署过程即会开始，在此过程中 [!DNL Commerce] 应用程序处于维护模式。 对于生产环境，Adobe建议在非高峰时间完成此工作，以避免服务中断。

## 工艺路线占位符 {#route-placeholder}

>[!NOTE]
>
>以下路由配置示例使用带有占位符的路由模板。 此 `{default}` 占位符表示为站点配置的默认域。 如果您的项目有多个域，请使用 `{all}` 用于为默认域和所有别名配置路由的占位符。 请参阅 [配置路由](/help/cloud-guide/routes/routes-yaml.md).

## SCD计时 {#scd-timing-warning}

>[!WARNING]
>
>如果在部署后应用程序中遇到静态内容文件问题（例如缺少自定义主题文件），请将最大预期执行时间增加到900秒或更高。

## 基于方案的部署 {#scenarios}

>[!NOTE]
>
>替换为 [!DNL ECE-Tools] 2002.1.0及更高版本中，您可以使用基于方案的部署功能在云基础架构项目上自定义Adobe Commerce的构建、部署和部署后流程。 请参阅 [基于方案的部署](/help/cloud-guide/deploy/scenario-based.md).

## 服务指令 {#service-instruction}

在专业集成环境和入门环境中使用以下说明进行服务设置，包括 `master` 分支。

>[!NOTE]
>
>[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 更改Pro生产和暂存环境中的服务配置。

## 服务更改 {#service-change-tip}

>[!TIP]
>
>在初始服务设置之后，您可以通过更新 `services.yaml` 和 `.magento.app.yaml` 配置文件。 请参阅 [更改服务版本](/help/cloud-guide/services/services-yaml.md#change-service-version) 以获取有关升级或降级服务的指导。

## 停滞的部署提示 {#stuck-deployment-tip}

>[!TIP]
>
>要获得有关停滞部署的帮助，请使用 [Adobe Commerce部署疑难解答程序](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) 在 _Commerce帮助中心_.

## ECE工具的更新 {#ece-tools-package}

>[!NOTE]
>
>如果您在云基础架构上使用的Adobe Commerce版本不包含 `ece-tools` 包，则必须执行 [一次性升级](/help/cloud-guide/dev-tools/install-package.md) ，以删除已弃用的包。 如果您当前使用 `ece-tools` 包并且您需要更新它，请参阅 [更新ECE工具包](/help/cloud-guide/dev-tools/update-package.md).

## 升级提示 {#upgrade-tip}

>[!TIP]
>
>在开始升级或修补过程之前，请从集成环境创建一个活动分支，并将新分支签出到您的本地工作站。 将分支专用于升级或修补过程有助于避免干扰正在进行的工作。

<!-- Fastly-related snippets begin -->

## 管理员登录 {#admin-login-step}

1. [登录](/help/get-started/onboarding.md#access-your-admin-panel) 发送给管理员。

## 自动部署自定义VCL代码片段 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>您可以将代码片段添加到 `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` 目录。 当您单击时，此目录中的代码片段会自动上传 _将VCL上传到Fastly_ 在Commerce管理员中。 请参阅 [自动自定义VCL代码片段部署](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) 在Magento2的Fastly CDN模块文档中。

<!-- Fastly-related snippets end -->
