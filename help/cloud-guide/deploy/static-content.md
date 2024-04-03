---
title: 静态内容部署
description: 了解在Adobe Commerce上针对云基础架构项目部署静态内容（如图像、脚本和CSS）的策略。
feature: Cloud, Build, Deploy, SCD
exl-id: e128d0d5-1326-44e5-a822-009e11ba105f
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---

# 静态内容部署策略

静态内容部署(SCD)对存储部署过程有显着影响，这取决于要生成多少内容（如图像、脚本、CSS、视频、主题、区域设置和网页）以及何时生成内容。 例如，默认策略会在以下期间生成静态内容： [部署阶段](process.md#deploy-phase-deploy-phase) 当站点处于维护模式时；但是，此部署策略需要一些时间才能将内容直接写入已装载的站点 `pub/static` 目录。 您可以通过多种选项或策略来帮助您根据自己的需求缩短部署时间。

## 优化JavaScript和HTML内容

您可以使用捆绑和缩小在静态内容部署期间构建优化的JavaScript和HTML内容。

### 缩小内容

如果您跳过复制中的静态视图文件，则可以缩短部署过程中的SCD加载时间 `var/view_preprocessed` 目录并生成 _缩小_ HTML。 您可以通过设置 [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) 全局环境变量到 `true` 在 `.magento.env.yaml` 文件。

>[!NOTE]
>
>从 `ece-tools` 软件包版本2002.0.13，SKIP_VARIABLES_MINIFICATIONHTML的默认值设置为 `true`.

您可以保存 **更多** 通过减少不必要的主题文件数量，减少部署时间和磁盘空间。 例如，您可以部署 `magento/backend` 英语主题以及其他语言的自定义主题。 您可以使用配置这些主题设置 [SCD矩阵](../environment/variables-deploy.md#scdmatrix) 环境变量。

## 选择部署策略

根据您是否选择在部署期间生成静态内容，部署策略有所不同 _生成_ 阶段， _部署_ 阶段，或 _按需_. 如下图所示，在部署阶段生成静态内容是最不理想的选择。 即使使用最小化HTML，也必须将每个内容文件复制到已装载的 `~/pub/static` 目录，这可能需要很长时间。 按需生成静态内容似乎是最佳选择。 但是，如果内容文件不存在于请求时生成的缓存中，则会增加用户体验的加载时间。 因此，在构建阶段生成静态内容是最理想的情况。

![SCD负载比较](../../assets/scd-load-times.png)

### 在生成时设置SCD

在构建阶段使用缩小的HTML生成静态内容是以下项的最佳配置 [**零停机时间** 部署](reduce-downtime.md)，也称为 **理想状态**. 它不会将文件拷贝到已装载的驱动器，而是会从 `./init/pub/static` 目录。

生成静态内容需要访问主题和区域设置。 Adobe Commerce将主题存储在文件系统中（可在构建阶段访问）；但是，Adobe Commerce将区域设置存储在数据库中。 数据库是 _非_ 在构建阶段可用。 要在构建阶段生成静态内容，您必须使用 `config:dump` 中的命令 `ece-tools` 打包以将区域设置移动到文件系统。 它读取区域设置并将它们保存在 `app/etc/config.php` 文件。

**将项目配置为在生成时生成SCD**：

1. 在本地工作站上，转到您的项目目录。
1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 将语言环境移动到文件系统，然后更新 [`config.php` 文件](../development/commerce-version.md#create-a-configphp-file).

1. 此 `.magento.env.yaml` 配置文件应包含以下值：

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) 是 `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) 在构建阶段为 `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) 是 `compact`

1. 验证配置 [部署后挂接](../application/hooks-property.md) 在 `.magento.app.yaml` 文件。

1. 通过运行 [理想状态的智能向导](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### 按需设置SCD

对于集成环境中的开发工作流，按需生成SCD是最佳选择。 它缩短了部署时间，以便您可以快速审查实施并运行集成测试。 启用 [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) 的全局阶段中的环境变量 `.magento.env.yaml` 文件。 SCD_ON_DEMAND变量将覆盖与SCD相关的所有其他配置，并清除现有内容 `~/pub/static` 目录。

使用SCD按需策略时，有助于使用预期请求的页面（例如主页）预加载缓存。 将您的预期页面列表添加到 [WARMUP_PAGE](../environment/variables-post-deploy.md#warmuppages) 环境变量 `.magento.env.yaml` 文件。

>[!WARNING]
>
>请勿在生产环境中使用SCD on-demand策略。

### 跳过SCD

有时，您可以选择完全跳过生成静态内容。 您可以设置 [SKIP_SCD](../environment/variables-build.md#skipscd) 全局阶段中的环境变量忽略与SCD相关的其他配置。 这不会影响 `~/pub/static` 目录。
