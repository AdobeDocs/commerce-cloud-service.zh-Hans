---
title: 部署最佳实践
description: 探索在云基础架构上部署Adobe Commerce的最佳实践。
feature: Cloud, Deploy, Best Practices
exl-id: bac3ca83-0eee-4fda-9a5c-a84ab25a837a
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# 部署最佳实践

将代码合并到远程环境时，生成和部署脚本会激活。 这些脚本使用环境 [配置文件](../environment/overview.md) 以及应用程序代码，以便为云基础架构提供适当的数据和服务。 此外，这些脚本还用于在云环境中安装或更新Adobe Commerce应用程序、第三方服务和自定义扩展。

每个计划的构建和部署过程略有不同：

- **入门计划** — 对于集成环境，每个活动分支都会构建并部署到完整的环境以进行访问和测试。 在合并到 `staging` 分支。 要启动您的网站，请推送 `staging` 到 `master` 以部署到生产环境。 通过，您可以完全访问所有分支 [!DNL Cloud Console] 和CLI命令。

- **专业计划** — 对于集成环境，每个活动分支都会构建并部署到完整的环境以进行访问和测试。 将您的代码合并到 `integration` 在合并到暂存环境和生产环境之前分支。 您可以使用合并到暂存环境和生产环境 [!DNL Cloud Console] 或使用SSH和 `magento-cloud` cli命令。

## 跟踪进程

您可以使用终端或 [!DNL Cloud Console] 状态消息 — `in-progress`， `pending`， `success`，或 `failed` — 在部署过程中显示。 您可以在日志文件中查看详细信息。 请参阅 [查看日志](../test/log-locations.md).

如果您使用外部GitHub存储库，则操作日志不会显示在GitHub会话中。 但是，您仍然可以关注界面中针对外部存储库的活动以及 [!DNL Cloud Console]. 请参阅 [集成](../integrations/overview.md).

>[!NOTE]
>
>在集成环境中，您无法从 [!DNL Cloud Console]. 此功能仅适用于生产和暂存环境。 但是，您可以使用查看任何环境中每个部署阶段的日志 [生成和部署](../test/log-locations.md#build-and-deploy-logs) 日志。 有关疑难解答信息，请参阅 [部署错误引用](../dev-tools/error-reference.md).

您可以启用 [使用New Relic跟踪部署](../monitor/track-deployments.md) 监视部署事件并分析部署之间的性能。

## 构建和部署的最佳实践

请查看部署过程中的以下最佳实践和注意事项：

- **确保您运行的是最新版本的 `ece-tools` 包**

  请参阅 [ECE工具的发行说明](../release-notes/ece-tools-package.md).

- **按照构建和部署过程操作**

  确保您在每个环境中都拥有正确的代码，以避免在环境之间合并代码时覆盖配置。 例如，要将配置更改应用于所有环境，请在部署到远程集成环境之前修改并测试本地环境中的更改。 然后，在部署到生产环境之前，在暂存环境中部署和测试更改。 从一个环境合并到另一个环境时，部署将覆盖环境中所有代码，特定于环境的配置和设置除外。

- **跨环境使用相同的变量**

  这些变量的值可能因环境而异；但是，您通常需要在每个环境中使用相同的变量。 请参阅 [商店设置的配置管理](../store/store-settings.md).

- **将敏感的配置值和数据保留在特定于环境的变量中**

  这些值包括使用Cloud CLI指定的变量，以及 [!DNL Cloud Console]，或添加到 `env.php` 文件。 请参阅 [变量级别](../environment/variable-levels.md).

- **确保环境分支中所有代码都可用**

  引用其他分支的代码（如专用分支）可能会在构建和部署过程中导致问题。 例如，如果您从专用分支引用主题，则无法访问该主题，也无法使用应用程序代码构建该主题。

- **在迭代的分支中添加扩展、集成和代码**

  在本地进行和测试更改，推送到 `integration`，然后转到 `staging` 和 `production`. 在将更新合并到下一个环境之前，测试和解决每个环境中的问题。 由于依赖关系，某些扩展和集成必须按特定顺序启用和配置。 在组中添加和测试可以更轻松地执行生成和部署过程，并有助于确定出现问题的位置。

- **验证服务版本和关系以及连接能力**

  验证应用程序可用的服务，并确保您使用的是最新的兼容版本。 请参阅 [服务关系](../services/services-yaml.md#service-relationships) 和 [系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 在 _安装指南_ 推荐的版本。

- **在部署到暂存和生产环境之前，请在本地和集成环境中测试**

  识别并修复本地环境和集成环境中的问题，以防止在部署到暂存环境和生产环境时延长停机时间。

  >[!TIP]
  >
  >有 [智能向导](../deploy/smart-wizards.md) 可用于验证云项目配置是否遵循生成和部署配置(包括静态内容部署(SCD)策略)的最佳实践的命令。

- **在本地和集成环境中完成测试后，在暂存环境中部署和测试**

  请参阅 [暂存和生产测试](../test/staging-and-production.md).

- **检查生产环境配置**

  在部署到生产环境之前，请完成以下任务：

   - 确保您可以使用连接到生产环境中的所有三个节点 [SSH](../development/secure-connections.md).

   - 验证索引器是否设置为 _按计划更新_. 请参阅 [索引模式](https://developer.adobe.com/commerce/php/development/components/indexing/) 在 _扩展开发人员指南_.

   - 通过更新生产代码中的任何特定于环境的变量、验证服务可用性和兼容性并进行任何其他所需的配置更改来准备环境。

- **监控部署过程**

  查看部署状态消息并根据需要缓解问题。 查看云 [日志](../test/log-locations.md#) 详细日志消息。

## 集成构建和部署的五个阶段

在本地开发环境和集成环境中将执行以下阶段。 对于Pro计划，在这些初始阶段中，代码不会部署到暂存或生产环境。

### 阶段1：代码和配置验证

最初设置项目时， [云基础架构模板](https://github.com/magento/magento-cloud) 为代码文件提供了基础。 此代码存储库将克隆到您的项目，作为 `master` 分支。

- **起始日期**—`master` 分支是您的生产环境。
- **适用于Pro**—`master` 作为集成环境的源分支开始。

创建分支，从 `master` 用于您的自定义代码、扩展和模块以及第三方集成。 有一个用于在云中测试代码的远程集成环境。

将代码从本地工作区推送到远程存储库时，在开始构建和部署脚本之前会完成一系列检查和代码验证。 内置Git服务器会验证您正在推送的内容并进行更改。 例如，如果添加OpenSearch服务，内置的Git服务器将验证是否相应地修改了集群的拓扑。

如果配置文件中出现语法错误，Git服务器会拒绝推送。 请参阅 [保护块](../development/protective-block.md).

此阶段也将运行 `composer install` 以检索依赖关系。

### 第2阶段：构建

>[!NOTE]
>
>在构建阶段，站点未处于维护模式，如果发生错误或问题，则不会停机。 它仅构建自上次构建以来发生更改的代码。

此阶段构建代码库并在中运行挂接 `build` 部分 `.magento.app.yaml`. 默认的生成挂接为 `php ./vendor/bin/ece-tools` 命令并执行以下操作：

- 在中应用修补程序 `vendor/magento/ece-patches`中的特定项目修补程序和可选修补程序 `m2-hotfixes`
- 重新生成代码和 [依赖注入](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) 配置(即 `generated/` 目录，包括 `generated/code` 和 `generated/metapackage`)使用 `bin/magento setup:di:compile`.
- 检查 [`app/etc/config.php`](../store/store-settings.md) 文件存在于代码库中。 如果Adobe Commerce在构建阶段未检测到此文件并包含模块和扩展名的列表，则会自动生成此文件。 如果存在，则构建阶段将照常继续，使用GZIP压缩静态文件并进行部署，从而减少部署阶段的停机时间。 请参阅 [生成选项](../environment/variables-build.md) 了解有关自定义或禁用文件压缩的信息。

>[!WARNING]
>
>此时，尚未创建群集，因此请勿尝试连接到数据库或假定存在活动的守护程序进程。

应用程序构建后，将其装载在 **只读文件系统**. 您可以配置将要读/写的特定装载点。 您无法通过FTP连接到服务器并添加模块。 相反，您必须将代码添加到本地存储库并运行 `git push`，用于构建和部署环境。 有关项目结构，请参阅 [本地项目目录结构](../project/file-structure.md).

### 阶段3：准备概要

构建阶段的结果是只读文件系统，称为 _概要_. 此阶段将创建一个归档文件，并将该概要文件置于永久存储中。 下次推送代码时，如果服务未发生更改，则它会使用存档中的概要。

- 通过重用未更改的代码，加快持续集成的构建速度
- 如果代码发生更改，会更新概要文件以供下次构建重用
- 如果需要，允许即时恢复部署
- 包括静态文件，如果 `app/etc/config.php` 文件存在于代码库中

概要文件包括所有文件和文件夹 **排除以下** 装载配置于 `magento.app.yaml`：

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### 阶段4：部署Slug和群集

您的应用程序和所有 [后端](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) 服务供应如下：

- 将每个服务挂载到一个容器中，如Web服务器、OpenSearch [!DNL RabbitMQ]
- 装载读写文件系统（装载在高可用分布式存储网格上）
- 配置网络，使服务可以“看到”彼此（并且只能看到彼此）

>[!NOTE]
>
>在所有构建和部署完成后在Git分支中进行更改并再次推送。 所有环境文件系统都是 _只读_. 只读系统可保证确定性部署，并显着提升网站安全性，因为没有任何进程可以写入文件系统。 它还可以确保您的代码在集成、暂存和生产环境中均相同。

### 阶段5：部署挂接

>[!NOTE]
>
>此阶段将 [!DNL Commerce] 应用程序处于维护模式，直到部署完成。

最后一步将运行部署脚本，使用该脚本可对开发环境中的数据进行匿名处理、清除缓存以及查询外部持续集成工具。 当此脚本运行时，您可以访问环境中的所有服务，例如Redis。

如果 `app/etc/config.php` 文件在代码库中不存在，静态文件将使用进行压缩 `gzip` 并在此阶段部署，这会增加部署阶段和站点维护的长度。

>[!NOTE]
>
>请参阅 [部署变量](../environment/variables-deploy.md) 了解有关自定义或禁用文件压缩的信息。

有两个部署挂钩。 此 `pre-deploy.php` 挂接完成对生成挂接中生成的资源和代码进行必要的清理和检索。 此 `php ./vendor/bin/ece-tools deploy` hook运行一系列命令和脚本：

- 如果Adobe Commerce为 **未安装**，它安装有 `bin/magento setup:install`，会更新部署配置， `app/etc/env.php`，以及指定环境（如Redis和网站URL）的数据库。 **重要提示：** 当您完成 [首次部署](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/launch/overview.html) 在安装期间，在所有环境中安装和部署Adobe Commerce。

- 如果Adobe Commerce **已安装**，执行任何必要的升级。 部署脚本运行 `bin/magento setup:upgrade` 更新数据库架构和数据（在更新扩展或核心代码后必需），并更新部署配置， `app/etc/env.php`，以及环境的数据库。 最后，部署脚本清除Adobe Commerce缓存。

- 脚本可以选择使用命令生成静态Web内容 `magento setup:static-content:deploy`.

- 使用范围(`-s` 标记符)，默认设置为 `quick` 用于静态内容部署策略。 您可以使用环境变量自定义策略 [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). 有关这些选项和功能的详细信息，请参阅 [静态文件部署策略](../deploy/static-content.md) 和 `-s` 标记对象 [部署静态视图文件](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>部署脚本使用配置文件在 `.magento` 目录，则脚本将删除该目录及其内容。 您的本地开发环境不受影响。

### 部署后：配置路由

在部署运行时，该进程将在入口点暂停传入流量60秒，并重新配置路由，以便您的Web流量到达新创建的群集。

成功部署将删除维护模式以允许正常访问，并为创建备份(BAK)文件 `app/etc/env.php` 和 `app/etc/config.php` 配置文件。

启用静态内容生成，使用 `SCD_ON_DEMAND` 变量并配置 [`post_deploy` 挂钩](../application/hooks-property.md) 以便清除缓存并预加载（加温）缓存 _之后_ 容器开始接受连接并 _期间_ 正常的，传入的流量。

要查看生成和部署日志，请参阅 [查看日志](../test/log-locations.md#view-and-manage-logs).
