---
title: “使用管理分支机构 [!DNL Cloud Console]"
description: 了解如何使用管理Adobe Commerce在云基础架构上的环境分支 [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c4ef149-fdb9-473f-91fd-5e6421ac5a43
source-git-commit: a5af3cc6e174a731a8f2ff198acd794384ef4585
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# 使用管理分支 [!DNL Cloud Console]

您可以使用以下任一方式管理环境 [!DNL Cloud Console] 或 `magento-cloud` CLI 您的项目文件存储在Git存储库中。 您可以使用Git命令管理您的代码，但是 `magento-cloud` CLI旨在与平台功能进行交互，而Git命令则不然。 请参阅 [Git命令](../dev-tools/cloud-cli-overview.md#git-commands) （在云CLI主题中）。

本主题讨论如何使用 [!DNL Cloud Console] 至：

- 添加或删除环境
- 同步(`git pull`)
- 合并(`git push`)到父环境

>[!TIP]
>
>您无法从Pro暂存环境和生产环境创建分支。 您可以从 `master` 分支。

## 创建环境

分支策略使用通用的Git工作流，您可以在其中开发代码并在开发分支中添加扩展。 请参阅 [起始者](../architecture/starter-architecture.md) 和 [Pro](../architecture/starter-develop-deploy-workflow.md) 架构概述。

- 首先，创建一个 `staging` 分支自 `master` 分支，然后从分支 `staging` 用于发展。
- 对于Pro，从 `Integration` 环境。

您的帐户支持的数量有限 ![活动分支](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} （非活动）开发分支。 通过仅使用添加或删除分支来管理活动和不活动分支 [!DNL Cloud Console] 或Cloud CLI。 在删除分支之前，需要先停用该分支，它保留在 _环境_ 列表为 _不活动_. 您可以稍后重新激活分支，也可以 [删除分支](../dev-tools/cloud-cli-overview.md#) 在环境设置中或使用Cloud CLI。

如果您需要其他活动环境进行开发，请提交 [支持服务单](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**添加分支**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 选择环境。

   >[!TIP]
   >
   >您的新分支是从此环境中克隆的。 选择与要创建的环境类似的父环境。

1. 单击 **[!UICONTROL Branch]**.

   ![创建分支](../../assets/button-branch.png){width="150"}

1. 在 _正在从……分支_ 表单中，输入分支名称。

   环境 _name_ 与环境不同 _ID_ 仅当在环境名称中使用空格或大写字母时。 环境ID由所有小写字母、数字和允许的符号组成。 环境名称中的大写字母在ID中转换为小写；环境名称中的空格转换为破折号。

   环境名称 **无法** 包括为Linux shell或正则表达式保留的字符。 禁止使用的字符包括大括号(`{ }`)，圆括号，星号(`*`)，尖括号(`>`)，&amp;符号(`&`)，% (<code>%</code>)，以及其他字符。

1. 选择 **[!UICONTROL Environment type]**.

1. 单击 **[!UICONTROL Create Branch]**.

1. 正在部署环境，请稍候。

   在部署期间，环境状态为  **处理中**. 成功部署后，的状态将更改为绿色复选标记 **success**.

## 创建非活动分支

您无法从Adobe Commerce Cloud控制台或CLI创建非活动分支。 如果要创建非活动分支，请在Git存储库中创建它，然后使用 `environment.Parent` 选项。

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## 删除环境

在删除环境之前，必须取消激活该环境。 环境处于非活动状态后，您可以将其删除。

**取消激活环境**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 从导航栏中选择环境 _环境_ 列表。

1. 单击顶部导航栏右侧的配置图标，打开环境设置。

1. 在 _[!UICONTROL General]_选项卡，向下滚动到_[!UICONTROL Deactivate environment]_ 部分并单击 **[!UICONTROL Deactivate environment and delete data]** 然后按照说明操作。

## 同步环境

同步环境（或分支）的方式与 `git pull origin <parent>`. 您可以从父环境中同步更新的代码。 您可以通过 [!DNL Cloud Console] 适用于所有入门和专业环境。

对于Pro计划，您可以从暂存和生产同步到 `master` 分支。 此同步仅提取和推送代码，而不提取数据。 要同步数据，请转储数据库数据并将其推送到另一个环境的数据库。 请参阅 [迁移和部署静态文件和数据](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**同步环境**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 在环境列表中，单击要同步的分支的名称。

1. 单击（同步）。

   ![同步环境](../../assets/button-sync.png){width="150"}

1. 选择要同步的项目。

   - 替换数据 — （数据和文件）同步父分支中数据库和内容文件的更改。
   - 合并 — （代码）同步来自父分支的已更新代码。

   这还会生成一个CLI命令供您复制和使用。

1. 单击 **同步**.

## 与父环境合并

合并环境（或分支）的过程与合并相同 `git push origin`. 您可以合并以将更新后的代码从环境推送到其父环境。 您可以将此代码合并到 `master`. 您可以使用部署到暂存和生产环境 `merge` 命令。

**要与父环境合并**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 在环境列表中，单击要合并的分支的名称。

1. 单击（合并）。

   ![合并环境](../../assets/button-merge.png){width="150"}

1. 单击 **合并** 并确认操作。

## 查看日志

通过 [!DNL Cloud Console]，您可以查看环境的各种日志，包括生成、部署和部署历史记录。

对象 **起始者**，您可以查看生成和部署日志以及部署历史记录。 这些环境包括 `master` （生产）分支以及从中创建的所有分支。

对象 **Pro**&#x200B;中，您可以查看每个环境中的以下日志：

- 集成 — 构建、部署和部署历史记录
- 暂存 — 构建日志和部署历史记录。 使用SSH登录到服务器以查看部署日志。
- 生产 — 构建日志和部署历史记录。 使用SSH登录到服务器以查看部署日志。

**在中查看日志[!DNL Cloud Console]**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 选择环境。

   环境视图提供 [活动列表](activity-stream.md) 显示 _最近_ 事件，每个尝试的操作一个条目，包括同步、合并、分支、备份等。 单击 **全部** 获取完整的部署历史记录。

1. 要查看生成日志，请选择帐户上每个部署记录的成功或失败链接。

>[!TIP]
>
>单击 **过滤方式** 图标，并选择要查看的消息类型。

## 从专用Git存储库中提取代码

您在云基础架构上的Adobe Commerce项目可以包含来自私有Git存储库的代码。 例如，您可能拥有专用存储库中自定义模块或主题的代码。 为此，您必须将项目的公共SSH密钥添加到私有Git存储库并更新项目 `composer.json` 文件。

要向专用GitHub存储库添加部署密钥，您必须是该存储库的管理员。 GitHub允许您仅对一个存储库使用部署密钥。

如果您希望项目访问多个存储库，则可以将SSH密钥附加到自动用户帐户。 由于此帐户不是由人使用，因此称为 [机器用户](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). 将计算机帐户添加为协作者，或者将计算机用户添加到具有存储库访问权限的团队。

>[!INFO]
>
>Adobe建议将此代码添加并合并到项目的Git存储库中。 如果不配置连接，则可能会遇到生成问题。

**查找SSH公钥**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 单击顶部导航栏右侧的配置图标。

1. 在 _项目设置_，单击 **[!UICONTROL Deploy Key]**.

1. 将部署密钥复制到剪贴板，以供在以下基于Git的方法之一中使用：

>[!BEGINTABS]

>[!TAB GitHub]

### 输入您的GitHub部署密钥

在GitHub上，部署密钥默认为只读。

**输入项目公钥作为GitHub部署密钥**：

1. 以管理员身份登录到您的GitHub存储库。
1. 单击存储库 **[!UICONTROL Settings]** 选项卡。

   >[!NOTE]
   >
   >如果没有看到此选项，则表示您不是以存储库管理员的身份登录，并且无法完成此任务。 请咨询GitHub存储库管理员以执行此操作。

1. 在 _设置_ 选项卡，单击 **[!UICONTROL Deploy Keys]**.
1. 单击 **[!UICONTROL Add deploy key]**.
1. 按照提示操作。

在 `composer.json`，使用 `<user>@<host>:<.git</code>` 格式，或 `ssh://<user>@<host>:<port>/<path>.git` 如果使用非标准端口。

>[!TAB 比特桶]

### 输入您的Bitbucket部署密钥

**输入项目公钥作为Bitbucket部署密钥**：

1. 以管理员身份登录到您的Bitbucket存储库。
1. 在左侧导航中，单击 **[!UICONTROL Settings]**.
1. 单击常规> **[!UICONTROL Deployment Keys]**.
1. 单击 **[!UICONTROL Add Key]**.
1. 按照提示操作。

>[!TAB GitLab]

### 输入您的GitLab部署密钥

**添加项目的公共SSH密钥作为GitLab部署密钥**：

1. 以所有者身份登录到您的GitLab存储库。
1. 验证 _管道_ 已为您的项目启用选项：

   1. 在项目设置中，展开 **[!UICONTROL Visibility, project, features, permissions]** 部分。
   1. 如有必要，请单击 **[!UICONTROL Pipelines]** 以启用该选项。

1. 将公共SSH密钥添加到CI/CD设置。

   1. 在左侧导航中，单击设置> **[!UICONTROL CI / CD]**.
   1. 单击“部署密钥” **展开** 以配置密钥。
   1. 在 _部署密钥_ 表单，将部署密钥名称添加到 **[!UICONTROL Title]** 将您的公共SSH密钥粘贴到 **[!UICONTROL Key]** 字段。
   1. 单击 **[!UICONTROL Add Key]** 以保存配置。

>[!ENDTABS]

## 保护环境和分支机构的安全

您可以使用从任何位置通过Web浏览器访问您的项目和环境。 [!DNL Cloud Console]. 您可能已为生产环境、商店和站点设置了安全性。 本节将帮助您确保集成和暂存环境的安全，严格限制为开发人员、DBA等的安全。

>[!WARNING]
>
>**不要** 请使用以下方法保护Pro暂存环境和生产环境。 这破坏了快速缓存。 使用 [阻止](../cdn/fastly-vcl-blocking.md) 可在Adobe Commerce的Fastly CDN中使用的功能。

**保护环境**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 选择环境并单击导航栏上的配置图标。

1. 在环境设置上 _常规_ 选项卡，单击 **开启** 对象 **[!UICONTROL HTTP access control enabled]** 以启用安全访问。 您可以在凭据或IP地址之间进行选择以筛选访问权限。

1. 要按凭据筛选，请单击 **[!UICONTROL Add Login]**，输入用户名和密码，然后单击 **[!UICONTROL Add Login]** 以添加。

1. 要按IP地址过滤，请在列表中输入IP地址，并使用 `deny` 或 `allow`. 例如：

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. 单击 **[!UICONTROL Save]**. 这将重新部署环境以更新安全和设置。 Adobe建议在完成安全设置后测试环境。
