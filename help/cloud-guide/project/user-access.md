---
title: 管理用户访问权限
description: 了解如何在云基础架构项目和环境中管理用户对Adobe Commerce的访问权限。
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 3357a3ea-bf86-4a65-95d1-6b24f1152248
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1441'
ht-degree: 0%

---

# 管理用户访问权限

云基础架构上的Adobe Commerce项目使用基于角色的访问。 在项目级别有两个可用的角色：

- **项目管理员** — 写入对所有项目环境的访问权限，并可管理用户、推送代码和更新项目设置。
- **项目查看器** — 对所有项目环境的仅查看访问权限。

项目查看者不能在任何环境中执行任务；但是，您可以授予项目查看者对特定环境类型的写入权限。

环境级访问基于环境类型：生产、暂存和开发。 授予用户 _查看者_ 权限 _开发_ 环境意味着他们可以查看 **所有** 开发环境。 下表说明了授予各个权限级别的功能：

| 权限级别 | 访问 | SSH访问 |
| ------------------ | ----------- | :----------: |
| **管理员** | 执行管理员任务，如更改设置、推送代码、执行任务和分支管理（包括与父环境合并） | 是 |
| **投稿人** | 推送代码并分支环境；无法更改设置或执行操作 | 是 |
| **查看器** | 对环境类型的仅查看访问权限 | 否 |
| **无权访问** | 无法访问环境类型 | 否 |

{style="table-layout:auto"}

您可以使用添加用户和分配角色 `magento-cloud` CLI或 [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**先决条件：**

- Adobe ID的注册用户。 用户必须 [注册Adobe帐户](https://account.adobe.com) 然后 [初始化他们的云帐户](https://console.adobecommerce.com) 将它们添加到云项目之前。
- 用户已分配 **管理员** 角色无法管理具有以下功能的用户： `magento-cloud` CLI 仅授予用户 **帐户所有者** 角色可以管理用户。

>[!ENDSHADEBOX]

## 使用CLI管理用户

使用 `magento-cloud` 用于管理用户并与自动化系统集成的CLI：

- `magento-cloud user:add` — 向项目添加用户
- `magento-cloud user:delete` — 删除用户
- `magento-cloud user:list [users]`-list项目用户
- `magento-cloud user:role` — 查看或更改用户角色
- `magento-cloud user:update` — 更新项目中的用户角色

以下示例使用 `magento-cloud` CLI用于添加用户、配置角色、修改项目分配以及分配用户角色。

**添加用户和分配角色**：

1. 使用 `magento-cloud` 用于添加用户的CLI。

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >用户必须具有Adobe ID；请参阅 [先决条件](#add-users-and-manage-access).

1. 按照提示操作：指定用户电子邮件地址，设置项目和环境类型角色，并添加用户。

   > 示例提示

   ```terminal
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   添加用户后，Adobe会向指定地址发送一封电子邮件，说明如何访问云基础架构上的Adobe Commerce。

### 查看用户的项目角色

```bash
magento-cloud user:get alice@example.com
```

>示例响应：

```terminal
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### 将用户添加到多个环境

将用户添加为 `viewer` 在 `Production` 环境和as a `contributor` 在 `Integration` 环境：

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### 更新用户环境权限

要将用户环境权限更新到 `admin` 在 `Production` 环境：

```bash
magento-cloud user:update alice@example.com -r production:a
```

## 从管理用户 [!DNL Cloud Console]

您可以使用 [[!DNL Cloud Console]](../../get-started/cloud-console.md) 要添加权限并使用 _编辑_ 功能，用于修改现有用户的权限。

>[!IMPORTANT]
>
>用户必须具有Adobe ID；请参阅 [先决条件](#add-users-and-manage-access).

### 在项目中添加用户

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. 从中选择一个项目 _所有项目_ 列表。

1. 在项目仪表板上，单击右上角的配置图标。

1. 下 _项目设置_，单击 **[!UICONTROL Access]**.

1. 在 _访问_ 视图，单击 **[!UICONTROL Add]**.

1. 完成 _[!UICONTROL Add User]_表单：

   - 输入用户电子邮件地址。

   - **[!UICONTROL Project admin]** — 授予所有设置和环境类型的管理员权限。

   - **[!UICONTROL Environment types and permissions]** — 向特定环境类型授予访问权和特定权限级别。 _无权访问_， _管理员_ （更改设置、执行操作、合并代码）、 _投稿人_ （推送代码），或 _查看器_ （仅查看）。

   >[!TIP]
   >
   >仅a **项目管理员** 可以在任何环境中管理用户。 授予用户访问 **访问** 选项卡，另一个 **项目管理员** 或 **帐户所有者** 必须为该用户分配 **项目管理员** 角色。

1. 单击 **[!UICONTROL Add User]**.

1. 添加用户后，重新部署所有环境以应用更改。 添加用户不会自动触发部署。 重新部署是确保用户可以使用SSH访问环境的重要步骤。

添加用户后，Adobe会向指定地址发送一封电子邮件，说明如何访问云基础架构上的Adobe Commerce。

## 用户身份验证要求

为了提高安全性，Adobe提供了项目级别的多因素身份验证(MFA)实施，以要求在云基础架构项目源代码和环境上对Adobe Commerce的SSH访问需要双重身份验证(TFA)。 请参阅 [为SSH启用MFA](multi-factor-authentication.md).

在云基础架构项目上的Adobe Commerce上启用MFA强制执行后，所有对该项目中的环境具有SSH访问权限的用户都必须在其云基础架构帐户上的Adobe Commerce上启用TFA。 对于自动化进程，您可以创建计算机用户和API令牌，以通过命令行进行身份验证。

将用户添加到云项目后，请要求用户查看其帐户安全设置并根据需要添加以下安全配置：

- **启用TFA** — 通过配置二元身份验证来满足安全和法规遵从性标准。 项目配置方式 [MFA实施](multi-factor-authentication.md) 要求使用SSH访问项目的帐户具有TFA。

- **启用SSH密钥** — 需要访问Cloud Infrastructure源代码存储库上的Adobe Commerce的用户必须在其帐户中启用SSH密钥。 请参阅 [安全连接](../development/secure-connections.md).

- **创建API令牌** — 用户必须生成一个API令牌，用于通过SSH访问环境。 您需要令牌以启用自动化流程的身份验证工作流。

  在启用了MFA强制的项目中，您必须使用API令牌来验证来自自动帐户的SSH访问请求。 令牌允许自动化进程绕过需要TFA的身份验证工作流。

### 为云帐户启用TFA

云基础架构上的Adobe Commerce支持使用以下任意应用程序进行TFA：

- [Google Authenticator (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [授权(Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [GAuth身份验证器（Firefox OS、台式机、其他）](https://github.com/gbraad-apps/gauth)

有关安装验证器应用程序和启用TFA的说明，请参阅 _帐户设置_ 中的页面 [!DNL Cloud Console].

**在您的用户帐户上启用TFA**：

1. 登录 [您的帐户](https://console.adobecommerce.com).

1. 在右上角的帐户菜单中，单击 **[!UICONTROL My Profile]**.

1. 在 _安全性_ 选项卡，单击 **[!UICONTROL Set up application]**.

1. 如果您的移动设备上没有经过批准的验证器应用程序，请使用链接说明安装该应用程序。

1. 将您的Adobe Commerce on cloud infrastructure帐户添加到验证器应用程序。

   - 在移动设备上，打开验证器应用程序。 然后，将设置代码添加到应用程序中。

   - 在 [!UICONTROL **[!UICONTROL TFA set up - Application]**] 页面，在中，键入移动设备中的TFA代码 **[!UICONTROL Application verification code]** 字段。

   - 单击 **[!UICONTROL Verify and save]**.

     如果代码有效，Adobe会向帐户电子邮件地址发送通知，确认帐户现在具有TFA。

1. 可选。 启用 _受信任的浏览器_ 设置缓存浏览器中的身份验证代码30天。

   此配置减少了项目登录期间的身份验证挑战。

1. 单击 **保存** 或 **跳过**.

1. 保存恢复代码。

   - 在 _TFA设置 — 恢复_ 代码页面，复制并保存恢复代码，以便在无法访问移动设备或身份验证应用程序时登录到Adobe Commerce on cloud infrastructure项目。

   - 将恢复代码复制到其他位置，或者在您无法访问设备或身份验证应用程序时将其写下。

   - 单击 **保存** 将代码保存到您的帐户，以便您能够通过帐户安全设置查看和管理这些代码。

     >[!WARNING]
     >
     >如果您无法访问具有TFA的帐户并且没有恢复代码列表，则必须联系项目管理员，或者 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 以重置TFA应用程序。

1. 完成TFA设置后，单击 **保存** 以更新您的帐户。

1. 通过TFA验证当前会话。

   - 注销您的帐户。
   - 使用您的用户名和密码登录。
   - 出现提示时，输入的TFA代码 `accounts.magento.cloud` 从移动设备上的验证器应用程序进入。

### 管理TFA配置和恢复代码

您可以在以下位置管理Adobe Commerce on cloud infrastructure帐户的TFA配置： _安全性_ 部分，位于 _我的个人资料_ 页面。

1. 登录 [您的帐户](https://console.adobecommerce.com).

1. 在右上角的帐户菜单中，单击 **[!UICONTROL My Profile]**.

1. 在 _我的个人资料_ 页面上，单击 **[!UICONTROL Security]** 选项卡。

1. 使用可用链接更新云基础架构帐户上Adobe Commerce的TFA设置：

   - 禁用TFA
   - 重置验证程序应用程序
   - 添加或删除受信任的浏览器
   - 查看或刷新您帐户上的TFA恢复代码

### 创建API令牌

可以将API令牌交换为OAuth 2访问令牌，然后该令牌可用于验证请求。

在已启用MFA强制的项目中，您必须具有API令牌才能为计算机用户和自动化流程启用SSH访问。

>[!IMPORTANT]
>
>您的帐户的Protect API令牌值。 不要在代码示例、屏幕捕获或不安全的客户端 — 服务器通信中公开值。 此外，不要公开存储在公共存储库中的源代码中的值。

**创建API令牌**：

1. 登录 [您的帐户](https://console.adobecommerce.com).

1. 在右上角的帐户菜单中，单击 **[!UICONTROL My Profile]**.

1. 在 _我的个人资料_ 页面上，单击 **[!UICONTROL API tokens]** 选项卡。

1. 单击 **[!UICONTROL Create API token]** 并输入名称，例如，指定与使用API令牌的计算机用户或自动进程匹配的名称。

   ![api令牌](../../assets/api-token-name.png)

1. 单击 **[!UICONTROL Create API token]**.
