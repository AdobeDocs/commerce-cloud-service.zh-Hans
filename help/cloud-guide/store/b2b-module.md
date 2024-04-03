---
title: 启用B2B模块
description: 了解如何在云基础架构上为Adobe Commerce启用B2B模块。
feature: Cloud, B2B
exl-id: 01d02ea0-1e7d-4608-adbf-1dad7f5e2182
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# 启用B2B模块

如果您的客户是公司，则可以安装B2B for Adobe Commerce模块，在Cloud Infrastructure Pro项目中扩展Adobe Commerce以适应企业对企业的模式。 尽管本主题提供了有关在云基础架构上安装和配置Adobe Commerce的B2B模块的特定信息，但您可以在以下指南中找到其他B2B信息：

- [Adobe Commerce B2B开发人员指南](https://developer.adobe.com/commerce/webapi/rest/b2b/)
- [Adobe Commerce B2B用户指南](https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html)

>[!TIP]
>
>由于B2B是云基础架构上Adobe Commerce的一个模块，因此Adobe建议您在开始之前将Adobe Commerce应用程序部署到集成或暂存环境。

## 安装B2B模块

将B2B模块添加到项目时，Adobe建议在开发分支中工作。 如果您没有分支，请参阅 [创建用于开发的分支](../development/cli-branches.md#create-a-branch-for-development). 安装B2B模块时， `Magento_B2b` 模块名称会自动插入到 `app/etc/config.php` 文件。 无需直接编辑文件。

**安装B2B模块**：

1. 在本地工作站上，转到您的项目目录。

1. 创建或签出开发分支。

1. 将B2B模块添加到 `require` 的部分 `composer.json` 文件。

   ```bash
   composer require magento/extension-b2b --no-update
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
   git commit -m "Install the B2B module."
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 构建和部署完成后，使用SSH登录到远程环境并验证是否已安装B2B模块。

   ```bash
   bin/magento module:status Magento_B2b
   ```

   扩展名使用以下格式： `<VendorName>_<ComponentName>`.

   示例响应：

   ```terminal
   Magento_B2b : Module is enabled
   ```

   如果遇到部署错误，请参阅 [从组件故障中恢复](../deploy/recover-failed-deployment.md).

## 启用B2B模块

使用编辑器安装B2B模块时，部署过程会自动启用该模块。 如果已安装B2B模块，则可以使用CLI启用或禁用该模块

启用B2B模块：

```bash
bin/magento module:enable Magento_B2b
```

示例响应：

```terminal
The following modules have been enabled:
- Magento_B2b

Cache cleared successfully.
Generated classes cleared successfully. Please run the 'setup:di:compile' command to generate classes.
Info: Some modules might require static view files to be cleared. To do this, run 'module:enable' with the --clear-static-content option to clear them.
```

请参阅 [管理扩展](extensions.md).

## 配置B2B模块

安装B2B for Adobe Commerce模块后，您必须 [启动消息使用者](https://experienceleague.adobe.com/docs/commerce-admin/b2b/install.html#start-message-consumers) 以便您能够启用 _共享目录_ 模块，并且您必须 [启用B2B功能](https://experienceleague.adobe.com/docs/commerce-admin/b2b/enable-basic-features.html).
