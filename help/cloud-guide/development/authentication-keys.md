---
title: 身份验证密钥
description: 了解如何将身份验证密钥应用于云基础架构上Adobe Commerce中的开发项目。
feature: Cloud, Security
topic: Security
exl-id: b05cd4c2-0804-49c8-980a-4c7b6932082b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 身份验证密钥

您必须拥有身份验证密钥才能访问Adobe Commerce存储库，并在云基础架构项目上为Adobe Commerce启用安装和更新命令。 有两种方法可指定Composer授权凭据。

- **身份验证文件** — 包含您的Adobe Commerce的文件 [授权凭据](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html) 在云基础架构的Adobe Commerce根目录中。
- **环境变量** — 一个环境变量，用于在Adobe Commerce on cloud infrastructure项目中设置身份验证密钥，以防止意外泄露。

>[!BEGINSHADEBOX]

**安全说明**

Adobe建议使用 [环境变量](#composer-auth-environment-variable) 方法，以防止意外泄露您的授权凭据。

将Cloud Docker for Commerce用作本地开发工具时，身份验证文件方法非常理想，但要注意不要上传 `auth.json` 文件到基于Git的公共存储库。 您可以添加 `auth.json` 文件到 [`.gitignore` 文件](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## 身份验证文件

**创建 `auth.json` 文件**：

1. 如果您没有 `auth.json` 创建项目根目录中的文件，请创建一个。

   - 使用文本编辑器，创建 `auth.json` 文件，该文件位于项目的根目录中。
   - 复制 [示例 `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) 到新的 `auth.json` 文件。

1. 替换 `<public-key>` 和 `<private-key>` 使用您的Adobe Commerce身份验证凭据。

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 保存更改并退出文本编辑器。

## Composer身份验证环境变量

以下方法是防止在基于Git的公共存储库中意外泄露敏感凭据的最佳方法。

**使用环境变量添加身份验证密钥**：

1. 在 _[!DNL Cloud Console]_中，单击项目导航右侧的配置图标。

   ![配置项目](../../assets/icon-configure.png){width="36"}

1. 在 _项目设置_ 列表，单击 **[!UICONTROL Variables]**.

1. 单击 **[!UICONTROL Create variable]**.

1. 在 **[!UICONTROL Variable name]** 字段，输入 `env:COMPOSER_AUTH`.

1. 在 _值_ 字段，添加以下内容并替换 `<public-key>` 和 `<private-key>` 使用您的Adobe Commerce身份验证凭据：

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 选择 **[!UICONTROL Available during buildtime]** 并取消选择 **[!UICONTROL Available during runtime]**.

1. 单击 **[!UICONTROL Create variable]**.

1. 删除 `auth.json` 文件。
