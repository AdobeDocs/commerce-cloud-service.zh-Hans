---
title: 升级Commerce版本
description: 了解如何在云基础架构项目中升级Adobe Commerce版本。
feature: Cloud, Upgrade
exl-id: 87821007-4979-4a20-940b-aa3c82c192d8
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# 升级Commerce版本

您可以将Adobe Commerce代码库升级到较新版本。 在升级项目之前，请查看 [系统要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 在 _安装_ 指南以了解最新的软件版本要求。

根据您的项目配置，升级任务可能包括以下内容：

- 更新服务(例如MariaDB (MySQL)、OpenSearch、RabbitMQ和Redis)以便与新的Adobe Commerce版本兼容。
- 转换旧版配置管理文件。
- 更新 `.magento.app.yaml` 包含挂接和环境变量的新设置的文件。
- 将第三方扩展升级到支持的最新版本。
- 更新 `.gitignore` 文件。

{{upgrade-tip}}

{{pro-update-service}}

## 从旧版本升级

如果您正在从低于2.1的Commerce版本开始升级，Adobe Commerce代码库中的某些限制可能会影响您执行 _更新_ 到特定的ECE-Tools版本或 _升级_ 到下一个支持的Commerce版本。 使用下表确定最佳路径：

| 当前版本 | 升级路径 |
| ----------------- | ------------ |
| 2.1.3及更早版本 | 在继续操作之前，请将Adobe Commerce升级到版本2.1.4或更高版本。 然后执行 [一次性升级以安装ECE-Tools](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [更新ECE-Tools](../dev-tools/update-package.md) 包。<br>请参阅发行说明，了解 [2002.0.9](../release-notes/cloud-release-archive.md#v200209) 和2002.0.x更高版本。 |
| 2.1.15 - 2.1.16 | [更新ECE-Tools](../dev-tools/update-package.md) 包。<br>请参阅发行说明，了解[2002.0.9](../release-notes/cloud-release-archive.md#v200209) 等会儿再说。 |
| 2.2.x及更高版本 | [更新ECE-Tools](../dev-tools/update-package.md) 包。<br>请参阅发行说明，了解[2002.0.8](../release-notes/cloud-release-archive.md#v200208) 等会儿再说。 |

{style="table-layout:auto"}

{{ece-tools-package}}

## 配置管理

旧版Adobe Commerce（如2.1.4或更高版本到2.2.x或更高版本）使用 `config.local.php` 用于配置管理的文件。 Adobe Commerce版本2.2.0及更高版本使用 `config.php` 文件，其工作方式与 `config.local.php` 文件，但它具有不同的配置设置，其中包括已启用的模块的列表和其他配置选项。

从旧版本升级时，您必须迁移 `config.local.php` 要使用较新版本的文件 `config.php` 文件。 使用以下步骤备份配置文件并创建一个。

**创建临时 `config.php` 文件**：

1. 创建副本 `config.local.php` 文件并为其命名 `config.php`.

1. 将此文件添加到 `app/etc` 文件夹中的任意位置。

1. 将文件添加并提交到分支。

1. 将文件推送到集成分支。

1. 继续升级过程。

>[!WARNING]
>
>升级后，您可以删除 `config.php` 并生成新的完整文件。 您只能删除此文件以一次替换它。 生成新的后，完成 `config.php` 文件，则无法删除该文件以生成新文件。 请参阅 [配置管理和管道部署](../store/store-settings.md).

### 验证Zend框架编辑器依赖关系

当升级到 **2.2.x中的2.3.x或更高版本**，验证是否已将Zend Framework依赖项添加到 `autoload` 的属性 `composer.json` 文件支持Laminas。 此插件支持Zend框架的新要求，该框架已迁移到Laminas项目。 请参阅 [将Zend框架迁移到Laminas项目](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) 在 _MagentoDevBlog_.

**要检查 `auto-load:psr-4` 配置**：

1. 在本地工作站上，转到您的项目目录。

1. 请查看您的集成分支。

1. 打开 `composer.json` 文件中的文本编辑器。

1. 查看 `autoload:psr-4` 部分，了解控制器依赖关系的Zend插件管理器实施。

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. 如果缺少Zend依赖关系，请更新 `composer.json` 文件：

   - 将以下行添加到 `autoload:psr-4` 部分。

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - 更新项目依赖关系。

     ```bash
     composer update
     ```

   - 添加、提交和推送代码更改。

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - 先将更改合并到暂存环境，然后再合并到生产环境。

## 配置文件

在升级应用程序之前，必须更新项目配置文件，以便考虑对云基础架构或应用程序上Adobe Commerce的默认配置设置所做的更改。 最新的默认设置可在 [magento-cloud GitHub存储库](https://github.com/magento/magento-cloud).

### .magento.app.yaml

始终检查 [.magento.app.yaml](../application/configure-app-yaml.md) 安装的版本的文件，因为它控制应用程序构建和部署到云基础架构的方式。 以下示例适用于版本2.4.7，并且使用编辑器2.7.2。此 `build: flavor:` 属性不用于Composer 2.x；请参阅 [安装和使用Composer 2](../application/properties.md#installing-and-using-composer-2).

**要更新 `.magento.app.yaml` 文件**：

1. 在本地工作站上，转到您的项目目录。

1. 打开并编辑 `magento.app.yaml` 文件。

1. 更新PHP选项。

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. 修改 `hooks` 属性 `build` 和 `deploy` 命令。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 在文件末尾添加以下环境变量。

   对于Adobe Commerce 2.2.x到2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   对于Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. 保存文件。 不要将更改提交或推送到远程环境。

1. 继续升级过程。

### composer.json

升级之前，请始终检查 `composer.json` 文件与Adobe Commerce版本兼容。

**要更新 `composer.json` 适用于Adobe Commerce版本2.4.4及更高版本的文件**：

1. 添加以下内容 `allow-plugins` 到 `config` 部分：

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 将以下插件添加到 `require` 部分：

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 将以下组件添加到 `extra:component_paths` 部分：

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. 保存文件。 尚未提交更改或将更改推送到分支。

1. 继续升级过程。

## 项目备份

我们建议在升级之前创建项目的备份。 使用以下步骤可备份集成、暂存和生产环境。

**备份集成环境数据库和代码**：

1. 创建远程数据库的本地备份。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >此 `magento-cloud db:dump` 命令运行 [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) 命令和 `--single-transaction` 标志，用于备份数据库而不锁定表。

1. 备份代码和介质。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   或者，您也可以忽略 `[--media]` 如果您有大量静态文件已在源代码管理中。

**在部署之前备份暂存或生产环境数据库**：

1. 使用SSH登录到远程环境。

1. 创建 [数据库转储](../storage/database-dump.md). 要为DB转储选择目标目录，请使用 `--dump-directory` 选项。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   转储操作将创建 `dump-<timestamp>.sql.gz` 存档文件。 请参阅 [备份数据库](../storage/database-dump.md).

## 应用程序升级

查看 [服务版本](../services/services-yaml.md#service-versions) 有关升级应用程序之前的最新软件版本要求的信息。

**升级应用程序版本**：

1. 在本地工作站上，转到您的项目目录。

1. 使用设置升级版本 [版本约束语法](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >您必须使用版本约束语法才能成功更新 `ece-tools` 包。 可以在以下位置找到版本限制： `composer.json` 版本的文件 [应用程序模板](https://github.com/magento/magento-cloud/blob/master/composer.json) 您正在使用进行升级。

1. 更新项目。

   ```bash
   composer update
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` 由于Composer封送基础包的方式，需要将所有更改的文件添加到源代码管理。 两者 `composer install` 和 `composer update` 从基础包封送文件(`magento/magento2-base` 和 `magento/magento2-ee-base`)导入包根目录。

   Composer封送的文件属于新版本的Adobe Commerce，用于覆盖这些相同文件的过时版本。 目前，Adobe Commerce中已禁用封送处理，因此您必须将封送处理文件添加到源代码管理。

1. 等待部署完成。

1. 通过使用SSH登录并检查版本，在集成、暂存或生产环境中验证升级。

   ```bash
   php bin/magento --version
   ```

### 创建config.php文件

如中所述 [配置管理](#configuration-management)，升级后，您必须创建一个已更新的 `config.php` 文件。 通过集成环境中的管理员完成任何其他配置更改。

**创建系统特定的配置文件**：

1. 从终端，使用SSH命令生成 `/app/etc/config.php` 环境文件。

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   例如，对于Pro，要运行 `scd-dump` 在 `integration` 分支：

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. 转移 `config.php` 文件到您的本地工作站，使用 `rsync` 或 `scp`. 您只能将此文件添加到本地分支中。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   这将生成一个更新的 `/app/etc/config.php` 包含模块列表和配置设置的文件。

>[!WARNING]
>
>对于升级，您删除 `config.php` 文件。 将此文件添加到代码后，您应 **非** 删除它。 如果必须删除或编辑设置，请手动编辑该文件。

### 升级扩展

在Marketplace或其他公司站点中查看您的第三方扩展和模块页面，并验证在云基础架构上对Adobe Commerce和Adobe Commerce的支持。 如果必须升级任何第三方扩展和模块，Adobe建议在禁用扩展的情况下使用新的集成分支。

**验证和升级扩展**：

1. 在本地工作站上创建分支。

1. 根据需要禁用扩展。

1. 可用时，下载扩展升级。

1. 按照第三方文档的说明安装升级。

1. 启用并测试扩展。

1. 添加、提交代码更改并将其推送到远程。

1. 推送到并在您的集成环境中测试。

1. 推送到暂存环境以在预生产环境中测试。

Adobe强烈建议升级您的生产环境 _早于_ 将升级的扩展包含在站点启动流程中。

>[!NOTE]
>
>当您升级应用程序版本时，升级过程将更新到最新版本的 [Fastly CDN模块](../cdn/fastly.md#fastly-cdn-module-for-magento-2) 自动。

## 升级疑难解答

如果升级失败，您会在浏览器中收到一条错误消息，指示您无法访问店面或管理面板：

```terminal
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**解决错误**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 打开 `./app/var/report/<error number>` 文件。

1. [检查日志](../test/log-locations.md) 并确定问题的来源。

1. 添加、提交和推送代码更改。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
