---
title: 配置传出电子邮件
description: 了解如何在云基础架构上为Adobe Commerce启用传出电子邮件。
exl-id: 814fe2a9-15bf-4bcb-a8de-ae288fd7f284
source-git-commit: 59f82d891bb7b1953c1e19b4c1d0a272defb89c1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# 配置传出电子邮件

您可以从为每个环境启用和禁用传出电子邮件 [!DNL Cloud Console] 或命令行中的。 为集成和暂存环境启用传出电子邮件，以发送双重身份验证或重置云项目用户的密码电子邮件。

默认情况下，出站电子邮件会在生产和暂存环境中启用。 但是， [!UICONTROL Enable outgoing emails] 在设置之前，环境设置中可能会显示为禁用 `enable_smtp` 属性通过 [命令行](#enable-emails-in-the-cli) 或 [云控制台](outgoing-emails.md#enable-emails-in-the-cloud-console).

正在更新 [!UICONTROL enable_smtp] 属性值依据 [命令行](#enable-emails-in-the-cli) 也会更改 [!UICONTROL Enable outgoing emails] 在Cloud Console中设置此环境的值。

{{redeploy-warning}}

## 在Cloud Console启用电子邮件

使用 **[!UICONTROL Outgoing emails]** 切换到 _配置环境_ 查看以启用或禁用电子邮件支持。

如果必须在专业生产或暂存环境中禁用或重新启用传出电子邮件，则可以提交 [Adobe Commerce支持票证](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Cloud Console上的Pro环境可能不会反映出站电子邮件状态。 请改用 [命令行](#enable-emails-in-the-cli) 用于启用和测试传出电子邮件。

**要从管理电子邮件支持，请[!DNL Cloud Console]**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 从中选择一个项目 _所有项目_ 列表。
1. 在项目仪表板上，单击右上角的配置图标。
1. 单击 **[!UICONTROL Environments]** 并从列表中选择特定环境。
1. 要启用或禁用传出电子邮件，请切换 _启用传出电子邮件_ **开启** 或 **关闭**.

   ![启用传出电子邮件配置](../../assets/outgoing-emails.png)

更改设置后，环境将使用新配置进行构建和部署。

## 在CLI中启用电子邮件

您可以使用更改活动环境的电子邮件配置 `magento-cloud` CLI `environment:info` 命令来设置 `enable_smtp` 属性。 启用SMTP将更新 `MAGENTO_CLOUD_SMTP_HOST` 用于发送邮件的、具有SMTP主机IP地址的环境变量。

**从命令行管理电子邮件支持**：

1. 在本地工作站上，转到您的项目目录。

1. 检查环境的传出电子邮件设置。

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. 通过设置 `enable_smtp` 环境变量到 `true` 或 `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   等待环境构建和部署。

1. 使用SSH登录到远程环境。

1. 验证电子邮件是否有效；向可检查的地址发送测试电子邮件。

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. 验证SendGrid是否拾取电子邮件。

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
