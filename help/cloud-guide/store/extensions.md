---
title: 管理扩展
description: 了解如何在云基础架构上的Adobe Commerce中安装和管理扩展。
feature: Cloud, Extensions, Upgrade
exl-id: 9c6e98ca-85da-4342-8402-d576eb382ba2
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 0%

---

# 管理扩展

您可以扩展Adobe Commerce应用程序功能，方法是从 [Commerce Marketplace](https://marketplace.magento.com). 例如，您可以添加主题以更改店面的外观，或者添加语言包以将店面和管理员本地化。

## 扩展的编辑器名称

尽管本节讨论如何从Commerce Marketplace获取扩展的编辑器名称和版本，但您可以找到扩展的名称和版本 _任意_ 模块。 打开 `composer.json` 在文本编辑器中编辑文件并记下 `"name"` 和 `"version"` 值。

**从Commerce Marketplace中获取模块的编辑器名称**：

1. 登录 [Commerce Marketplace](https://marketplace.magento.com) 以及您用于购买组件的用户名和密码。

1. 单击右上角的用户名并选择 **我的个人资料**.

   ![访问您的Marketplace帐户](../../assets/marketplace/my-profile.png)

1. 在 _我的帐户_ 页面，单击 **我的购买**.

   ![Marketplace购买历史记录](../../assets/marketplace/my-purchases.png)

1. 在 _我的购买_ 页面上，选择您购买的模块并单击 **技术详细信息**.

1. 单击 **复制** 以复制 [!UICONTROL Component name] 到剪贴板中。

1. 打开文本编辑器并粘贴组件名称并附加一个冒号字符(`:`)。

1. 在 **技术详细信息**，单击 **复制** 以复制 [!UICONTROL Component version] 到剪贴板中。

1. 在文本编辑器中，将版本号附加到组件名称中冒号的后面。 例如：

   ```text
   extension-name/magento2:1.0.1
   ```

## 安装扩展

在向实施中添加扩展时，Adobe建议在开发分支中工作。 安装扩展时，扩展名称为(`<VendorName>_<ComponentName>`)自动插入到 [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html) 文件。 无需直接编辑文件。

**安装扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 创建或签出开发分支。 请参阅 [分支](../development/cli-branches.md).

1. 使用编辑器名称和版本，将扩展添加到 `require` 的部分 `composer.json` 文件。

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. 更新项目依赖关系。

   ```bash
   composer update
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >安装扩展时，必须包含 `composer.lock` 文件。 此 `composer install` 命令读取 `composer.lock` 文件，以在远程环境中启用定义的依赖关系。

1. 构建和部署完成后，使用SSH登录到远程环境并验证已安装的扩展。

   ```bash
   bin/magento module:status <extension-name>
   ```

   扩展名使用以下格式： `<VendorName>_<ComponentName>`.

   示例响应：

   ```terminal
   Module is enabled
   ```

   如果遇到部署错误，请参阅 [扩展部署失败](../deploy/recover-failed-deployment.md).

## 管理扩展

使用编辑器添加扩展时，部署过程会自动启用该扩展。 如果已安装扩展，则可以使用CLI启用或禁用该扩展。 管理扩展时，请使用格式： `<VendorName>_<ComponentName>`

在登录到远程环境时，切勿启用或禁用扩展。

**启用或禁用扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 启用或禁用模块。 此 `module` 命令更新 `config.php` 具有所请求的模块状态的文件。

   >启用模块。

   ```bash
   bin/magento module:enable <module-name>
   ```

   >禁用模块。

   ```bash
   bin/magento module:disable <module-name>
   ```

1. 如果启用了模块，请使用 `ece-tools` 以刷新配置。

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. 验证模块的状态。

   ```bash
   bin/magento module:status <module-name>
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## 升级扩展

在继续之前，您需要具有扩展的编辑器名称和版本。 此外，请确认该扩展与您的项目和Adobe Commerce版本兼容。 特别是， [检查所需的PHP版本](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 开始之前。

**更新扩展**：

1. 在本地工作站上，转到您的项目目录。

1. 创建或签出开发分支。 请参阅 [分支](../development/cli-branches.md).

1. 打开 `composer.json` 文件中的文本编辑器。

1. 找到扩展并更新版本。

1. 保存更改并退出文本编辑器。

1. 更新项目依赖关系。

   ```bash
   composer update
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

如果遇到错误，请参阅 [从组件故障中恢复](../deploy/recover-failed-deployment.md). 要了解有关将扩展与Adobe Commerce结合使用的更多信息，请参阅 [扩展](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html) 在 _管理指南_.
