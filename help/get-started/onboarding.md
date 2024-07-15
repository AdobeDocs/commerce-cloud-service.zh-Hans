---
title: '[!DNL Onboarding]到Commerce'
description: 访问您的云帐户，并在云基础架构项目上设置Adobe Commerce 。
role: Admin
recommendations: noDisplay, catalog
exl-id: c6b768d7-d835-4a8d-aad9-1c0324f7570d
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---

# [!DNL Onboarding]到Commerce

Adobe在云基础架构订阅上激活Commerce后，初始项目和代码访问权限仅可供指定为许可证所有者（帐户所有者）的人员使用。

许可证所有者是贵企业或财务机构中负责管理Adobe Commerce on cloud infrastructure账户的支付和其他业务相关交易的人员。 此人充当与Adobe的联系人。 如果您必须更改您帐户上的许可证所有者，则必须联系您的Adobe帐户团队。

要快速载入项目，以便开始开发站点以供实时部署，您必须完成所需的设置和[!DNL onboarding]任务。 通常，许可证所有者通过保护管理员访问权限并创建可帮助进行设置、自定义和开发工作的技术管理员用户来开始此过程。

## 注册云帐户

如果您在云基础架构上没有Adobe Commerce，请联系[销售人员]。 注册后，Adobe会创建您的帐户，并向您发送一封欢迎电子邮件，为您提供有关如何访问项目界面的说明。 该电子邮件包含一个链接，以便您可以登录到帐户并完成初始项目设置。

### 云[!DNL Onboarding] UI

([!DNL Onboarding] UI)中的“云基础架构上的Adobe Commerce项目”页面提供了一个入门核对清单，用于设置项目和服务、确定访问权限以及开始开发。 在OBUI中，您可以：

- 添加技术管理员，作为可管理您的项目和分支的超级用户
- 访问您的项目环境，包括指向[!DNL Cloud Console]的链接
- 完成快速用户验收测试(UAT)核对清单，并提供指向进一步测试的链接

**打开项目页面**：

1. 登录到您的[Adobe Commerce客户帐户](https://account.magento.com/customer/account/login)。

1. 在&#x200B;_我的帐户_&#x200B;页面上，单击&#x200B;**[!UICONTROL Commerce]**&#x200B;选项卡以查看您帐户中的项目。

1. 单击[项目部分](https://cloud.magento.com/cloud/project/)中的&#x200B;**查看项目页面**。

1. 单击项目名称并打开云项目页面([!DNL Onboarding] UI)。

   ![OBUI项目页面](../assets/onboarding-ui.png)

   浏览该门户，获得有用的信息和选项，以开始规划项目、开发代码和准备UAT和站点启动。

## 访问项目并添加用户

许可证所有者可以添加用户帐户，以提供对代码、管理分支、输入票证和支持环境的访问权限。 这些用户帐户可以包括内部开发、顾问和解决方案专家。

通常，许可证所有者必须创建的唯一用户是&#x200B;_技术管理员_。 技术管理员需要拥有管理员访问权限的用户帐户来为开发人员创建用户帐户、设置环境权限以及管理所有分支和环境。 技术管理员可以是开发人员、顾问、[Adobe解决方案合作伙伴](https://business.adobe.com/products/magento/partners.html)，也可以是您自己。

您可以通过项目门户、[!DNL Cloud Console]或使用`magento-cloud` CLI从命令行创建技术管理员。

### 用户注册

您只能在云基础架构项目和环境中将注册用户添加到Adobe Commerce。 如果您有新用户，请要求他们[注册帐户](https://account.magento.com/customer/account/login/)，并提供与其帐户配置文件关联的电子邮件地址。

### 共享帐户访问权限

许可证所有者可以设置帐户的共享访问权限。 共享访问允许受信任的员工和服务提供商使用帮助中心提交和跟踪与云基础架构项目上的Adobe Commerce相关的支持工单。 有关设置说明，请参阅帮助中心中的[共享访问]文章。

### [!DNL Cloud Console]

您可以使用[[!DNL Cloud Console]](cloud-console.md)管理项目、添加用户帐户并开始开发您的存储。 许可证所有者、技术管理员用户和开发人员可以使用[!DNL Cloud Console]管理所有环境和分支、环境变量、环境设置和路由。

**要访问[!DNL Cloud Console]**：

1. 登录到[我的帐户](https://account.magento.com/customer/account/login)。

1. 在&#x200B;_我的帐户_&#x200B;页面上，单击&#x200B;**[!UICONTROL Commerce]**&#x200B;选项卡以查看您帐户中的项目。

1. 单击&#x200B;**项目**&#x200B;选项卡并选择项目。

1. 单击&#x200B;**基础结构访问**，然后单击&#x200B;**Project Access (Web UI)**。

   ![云项目门户](../assets/obui-project-access.png)

## 注册Adobe状态

从[状态页面]获取有关Adobe Commerce on cloud infrastructure platform环境和相关服务的更新。

此页提供了Adobe Commerce组件与服务的状态，随后提供了有关事件报告、服务升级、计划内中断和计划内维护的通知。 在您的项目上工作的任何人都可以订阅Adobe Commerce状态网站，通过电子邮件或Slack接收事件通知和更新。 您可以自定义Adobe状态订阅，以按地区和事件跟踪特定产品。

>[!TIP]
>
> 打开新的[!DNL Cloud Console]并查看项目和环境活动。
>
>**下一步**：[登录到Cl[！DNL ]oud控制台](cloud-console.md)

<!-- link definitions -->

[销售]: https://business.adobe.com/products/magento/get-demo.html
[共享访问]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#shared-access
[状态页面]: https://status.adobe.com/products/503473
