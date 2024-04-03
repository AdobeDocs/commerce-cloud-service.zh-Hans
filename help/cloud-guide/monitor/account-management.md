---
title: New Relic帐户管理
description: 了解如何访问您的New Relic帐户并管理您的Adobe Commerce在云基础架构项目上的访问权限、集成和工具使用。
feature: Cloud, Observability
role: Admin
exl-id: ee639e2e-4074-4384-8f68-152bc3bac93b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# New Relic帐户管理

当Adobe配置您的云基础架构项目时，许可证所有者会收到来自New Relic的电子邮件，其中包含用于访问New Relic帐户的凭据和说明。 如果您没有收到电子邮件，请使用许可证所有者电子邮件地址来重置New Relic密码。

## 管理用户访问权限

一个New Relic帐户只能有一个人员被分配给“所有者”角色。 如果必须更改帐户所有者，请将“管理员”角色分配给当前所有者，然后将“所有者”角色分配给其他用户。 请参阅 [更新帐户所有者](https://docs.newrelic.com/docs/accounts/original-accounts-billing/original-users-roles/users-roles-original-user-model/) 在 _New Relic文档_ 以获取说明。

管理New Relic访问权限的准则：

- 项目所有者和管理员用户可以从New Relic帐户中添加和删除用户。
- 请勿创建超过五个的完全访问权限 **用户**.
- 仅向严格要求访问完整功能集的用户授予完全访问权限。
- 目前尚无关于免费使用的具体指南 **受限** 用户。

>[!TIP]
>
>在将“所有者”角色分配给用户之前，请验证该用户是否存在于云基础架构上Adobe Commerce的New Relic帐户中。 如果您必须将该用户添加到该帐户，并且现有帐户所有者或管理员无法提供帮助，则任何有权访问 [Adobe伙伴关系所有者帐户](https://account.newrelic.com/accounts/1311131/users) for New Relic可以代表客户添加用户。

至少添加一个 **管理员** 用户访问New Relic帐户，该帐户可以管理所有访问权限、集成和工具使用。

**在New Relic中访问用户管理**：

1. 登录 [New Relic帐户](https://login.newrelic.com/login).

1. 从左下方导航中选择您的用户名。

1. 单击 **[!UICONTROL Administration]** 并从列表中选择以下选项之一：

   - **[!UICONTROL User management]** 以添加用户并管理活动用户和待处理邀请。

   - **[!UICONTROL Access management]** 管理用户组、角色和帐户。

请参阅 [用户管理](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) 在 _New Relic_ 文档。

## 为入门环境配置New Relic

>[!NOTE]
>
>**专业环境** 已预配置为使用New Relic服务，可跳过启用和连接说明。 如果暂存和生产环境中未安装New Relic APM，或者New Relic基础架构在生产环境中不可用， [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 请求安装。

对于入门环境，您必须检查 `.magento.app.yaml` 文件验证 `runtime` 部分包含New Relic扩展。 如果尚未配置该扩展，请添加以下内容：

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### 应用许可证密钥

要将Cloud环境连接到New Relic，请将New Relic许可证密钥添加到环境。

- 对象 **Pro项目**，Adobe会在配置过程中将许可证密钥添加到您的生产和暂存环境。 您可以登录到 [New Relic帐户](https://login.newrelic.com/login) 验证Adobe Commerce on cloud infrastructure站点与New Relic之间的连接。

- 对象 **入门项目** New Relic ，您拥有最多支持 _三_ 环境。 您必须手动将密钥添加到环境配置。 未预配置入门环境以使用New Relic服务。

对于入门环境，请通过将New Relic许可证密钥添加到环境配置来启用New Relic集成。 将密钥添加到暂存环境和生产环境以及您选择的其他一个环境。 配置只需要使用New Relic许可证密钥。 有关其他配置选项的信息，请参见 [New Relic报表](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) 中的主题 _Adobe Commerce用户指南_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce帐户页面或与项目关联的New Relic许可证的登录凭据
>- [管理员级访问权限](../project/user-access.md) 到入门环境进行配置
>- 用于访问 [管理员](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) 环境

**为入门环境配置New Relic**：

1. 从查找您的New Relic许可证密钥 [!DNL Cloud Console] 或Cloud CLI。

   **[!DNL Cloud Console]方法**：

   - 打开您的云项目 [帐户页面](https://accounts.magento.cloud/user).

   - 在 _项目_ 选项卡，查找您的项目。

   - 单击 **查看详细信息** 用于项目基础结构信息。

   - 展开 **New Relic服务** 部分以查看许可证密钥。

   - 复制许可证密钥。

   **Cloud CLI方法**：

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. 使用将New Relic许可证密钥添加到环境中 `magento-cloud` CLI

   - 更改到需要许可证密钥的环境。
   - 使用以下方式更新变量值 `magento-cloud` CLI命令：

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   或者，您也可以从 [商务管理员](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store).

1. 登录 [New Relic帐户](https://login.newrelic.com/login) 以验证是否可以从Adobe Commerce环境中查看数据。 请参阅 [调查性能](investigate-performance.md).

### 删除许可证密钥

您只能在三个活动环境中使用New Relic许可证密钥。 如果密钥在三个环境中使用，则必须先从其中一个环境中移除密钥，然后才能将其添加到其他环境。

**从环境中移除许可证密钥**：

1. 列出环境变量。

   ```bash
   magento-cloud variable:list
   ```

   示例响应：

   ```terminal
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >如果您将许可证密钥添加为 _项目_ 变量中，则必须移除该项目级别的变量。 项目变量将许可证添加到 _每_ 创建的环境分支，可能会占用或超过许可证限制。 要列出项目变量，请执行以下操作： `magento-cloud variable:list --level project`

1. 删除许可证变量。

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
