---
title: 项目结构
description: 了解云基础架构上Adobe Commerce的文件结构和项目模板。
exl-id: 6dc559bd-116b-4745-a85b-731508e113ff
source-git-commit: 47ef728ea46b137eeaabbdbada940143d8773ef0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 项目结构

云基础架构项目上的Adobe Commerce包括凭据和应用程序配置的基本文件。 根据Adobe Commerce版本，这些文件在中作为模板提供。 请参阅中基于Adobe Commerce版本的云模板 [`magento/magento-cloud` GitHub存储库](https://github.com/magento/magento-cloud).

下表介绍了云项目中包含的文件：

| 文件 | 描述 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | 重定向的配置文件 `www` 到Apex域和 `php` 应用程序来提供HTTP。 请参阅 [配置路由](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | 定义MySQL实例(MariaDB)、Redis和OpenSearch或Elasticsearch的配置文件。 请参阅 [配置服务](../services/services-yaml.md). |
| `/app` | 此 `code` 文件夹用于自定义模块。 此 `design` 文件夹用于 [自定义主题](../store/custom-theme.md). 此 `etc` 文件夹包含应用程序的配置文件。 |
| `/m2-hotfixes` | 用于自定义修补程序。 |
| `/update` | 支持模块使用的服务文件夹。 |
| `.gitignore` | 指定要忽略的文件和目录。 请参阅 [`.gitignore` 引用](#ignoring-files). |
| `.magento.app.yaml` | 定义用于构建应用程序的属性的配置文件。 请参阅 [配置应用程序](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | 生成、部署和部署后阶段的配置文件。 此 `ece-tools` 包中包含此文件的示例。 请参阅 [配置环境](../environment/configure-env-yaml.md). |
| `composer.json` | 获取Adobe Commerce和配置脚本以准备您的应用程序。 请参阅 [必需的包](../development/overview.md#required-packages). |
| `composer.lock` | 存储每个包的版本依赖关系。 请参阅 [必需的包](../development/overview.md#required-packages). |
| `magento-vars.php` | 用于定义 [多个商店](../store/multiple-sites.md) 和使用变量的网站。 |

{style="table-layout:auto"}

>[!NOTE]
>
>当您将本地更改推送到远程服务器时，部署脚本使用由配置文件定义的值 `.magento` 目录，则脚本将删除该目录及其内容。 您的本地开发环境不受影响。

## 应用程序根目录

应用程序根目录的位置取决于环境。

- **Starter和Pro集成**： `/app`
- **入门级生产**： `/<project-ID>`
- **Pro Staging**： `/<project-ID>_stg`
- **Pro Production**： `/<project-ID>`

### 可写目录

远程集成、暂存和生产环境是只读的。 以下目录为 *仅限* 出于安全原因，可写目录：

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>在生产环境和暂存环境中，三节点群集中的每个节点都有一个 `/tmp` 未与其他节点共享的目录。

## 忽略文件

有一个基地 `.gitignore` 使用Adobe Commerce on cloud infrastructure项目存储库创建文件。 查看最新信息 [magento-cloud存储库中的.gitignore文件](https://github.com/magento/magento-cloud/blob/master/.gitignore). 添加位于 `.gitignore` 列表，您可以使用 `-f` 暂存提交时的（强制）选项：

```bash
git add <path/filename> -f
```

## 更改基本模板

您可以使用以下步骤更改现有项目的结构，以反映云基础架构上Adobe Commerce的最新基础模板。

1. 将项目克隆到本地工作站。

1. 更新 `composer.json` 文件，其值为 `extra` 部分。

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 添加 `.gitignore` 为基本模板设计的文件。 例如，如果您需要 `.gitignore` 文件对于2.2.6版本的模板，请使用 [.gitignore适用于2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) 文件作为引用。

1. 清除Git缓存。

   ```bash
   git rm -r --cached .
   ```

1. 添加和提交更改。

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
