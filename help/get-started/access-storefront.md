---
title: 访问Commerce管理面板
description: 了解如何访问Commerce管理面板。
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '228'
ht-degree: 0%

---

# 访问Commerce管理面板

对Commerce“管理员”面板具有管理访问权限的用户可以添加用户、配置商店服务、完成商店设置和自定义工作等。

对于新项目，收到欢迎电子邮件后的第一步是通过更改许可证所有者帐户上的密码来保护管理员对项目的访问。 该帐户的默认用户名是“许可证所有者”电子邮件地址。

您可以使用以下任一方法提交密码更改请求：

- 找到发送到许可证所有者电子邮件地址的欢迎电子邮件，然后按照链接更改密码。

- 从复制商店URL [[!DNL Cloud Console]](../cloud-guide/project/overview.md) 到浏览器中。 然后，附加 `/admin` 以打开登录页面。 单击 **忘记密码？** 用于将密码更改请求发送到许可证所有者电子邮件地址的链接。

提交密码更改请求后，请查看电子邮件中的密码重置通知。 如果未收到电子邮件，请检查垃圾邮件文件夹。

>[!TIP]
>
>如果密码重置失败或您无法登录到“管理员”面板，则具有管理员访问权限的用户可以使用SSH连接到项目并使用 `admin:user:create` cli命令。 请参阅 [创建、编辑或解锁管理员帐户](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) 在 _安装指南_.
