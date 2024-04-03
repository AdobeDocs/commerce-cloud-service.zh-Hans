---
title: 比特桶集成
description: 了解如何将您的Adobe Commerce on cloud基础架构项目与Bitbucket集成。
feature: Cloud, Integration
exl-id: cd3cffbe-268f-429b-a2ea-0306159f4a6b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# 比特桶集成

您可以将Bitbucket存储库配置为在推送代码更改时自动构建和部署环境。 此集成会将您的Bitbucket存储库与Adobe Commerce在云基础架构帐户中同步。

{{private-repository}}

## 先决条件

- 对云基础架构项目上Adobe Commerce的管理员访问权限
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) 工具
- Bitbucket帐户
- 具有比特桶存储库的管理员访问权限
- 比特桶存储库的SSH访问密钥

## 准备存储库

从现有环境克隆Adobe Commerce on cloud infrastructure项目，并将项目分支迁移到空的新比特桶存储库，保留相同的分支名称。 它是 **关键** 以保留相同的Git树，从而不会丢失Adobe Commerce on cloud基础架构项目中的任何现有环境或分支。

1. 从终端，登录到您的Adobe Commerce on cloud infrastructure项目。

   ```bash
   magento-cloud login
   ```

1. 列出您的项目并复制项目ID。

   ```bash
   magento-cloud project:list
   ```

1. 将项目克隆到本地环境。

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. 将您的Bitbucket存储库添加为远程存储库。

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   远程连接的默认名称可以是 `origin` 或 `magento`. 如果 `origin` 存在，您可以选择其他名称，也可以重命名或删除现有参照。 请参阅 [git-remote文档](https://git-scm.com/docs/git-remote).

1. 验证是否正确添加了Bitbucket远程。

   ```bash
   git remote -v
   ```

   预期响应：

   ```terminal
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. 将项目文件推送至新的Bitbucket存储库。 请记住保持所有分支名称相同。

   ```bash
   git push -u origin master
   ```

   如果您从新的Bitbucket存储库开始，则可能必须使用 `-f` 选项，因为远程存储库与您的本地副本不匹配。

1. 验证Bitbucket存储库是否包含所有项目文件。

## 创建OAuth消费者

Bitbucket集成需要 [OAuth消费者](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). 您需要OAuth `key` 和 `secret` 从此使用者以完成下一部分。

**在Bitbucket中创建OAuth消费者**：

1. 登录 [比特桶](https://id.atlassian.com/login) 帐户。

1. 单击 **设置** > **访问管理** > **OAuth**.

1. 单击 **添加消费者** 并按照以下方式对其进行配置：

   ![Bitbucket OAuth使用者配置](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >有效的 **回调URL** 不是必需的，但必须在此字段中输入值才能成功完成集成。

1. 单击 **保存**.

1. 单击使用者 **名称** 以显示您的OAuth `key` 和 `secret`.

1. 复制您的OAuth `key` 和 `secret` 用于配置集成。

## 配置集成

1. 从终端，导航到您的Adobe Commerce on cloud基础架构项目。

1. 创建名为的临时文件 `bitbucket.json` 并添加以下内容，将尖括号中的变量替换为您的值：

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >请确保使用Bitbucket存储库的名称，而不是URL。 如果您使用URL，则集成会失败。

1. 使用将集成添加到您的项目中 `magento-cloud` cli工具。

   >[!WARNING]
   >
   >以下命令将覆盖 _所有_ 使用Bitbucket存储库中的代码，在Adobe Commerce上实施云基础架构项目。 这包括所有的分支，包括 `production` 分支。 此操作立即发生且无法撤消。 作为最佳实践，在云基础架构项目中从Adobe Commerce克隆所有分支并将它们推送到Bitbucket存储库非常重要 **早于** 添加Bitbucket集成。

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   这会返回带有标头的长HTTP响应。 成功的集成会返回200或201状态代码。 状态400或更高表示出现错误。

1. 删除临时 `bitbucket.json` 文件。

1. 验证项目集成。

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```terminal
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   记下 **挂接URL** 以在BitBucket中配置webhook。

### 在BitBucket中添加webhook

要与Cloud Git服务器通信事件（例如推送），需要为BitBucket存储库具有webhook。 正确执行本页中详细介绍的设置Bitbucket集成的方法时，会自动创建webhook。 请务必验证webhook以避免创建多个集成。

1. 登录 [比特桶](https://id.atlassian.com/login) 帐户。

1. 单击 **存储库** 并选择您的项目。

1. 单击 **存储库设置** > **工作流** > **Webhooks**.

1. 在继续之前验证webhook。

   如果挂接处于活动状态，请跳过其余步骤并 [测试集成](#test-the-integration). 挂接的名称应类似于 **“云基础架构上的Adobe Commerce &lt;project_id>&quot;** 和类似于： `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. 单击 **添加webhook**.

1. 在 _添加新webhook_ 查看，编辑以下字段：

   - **标题**：Adobe Commerce集成
   - **URL**：使用来自您的网站的挂钩URL `magento-cloud` 集成列表
   - **触发器**：默认为基本 _存储库推送_

1. 单击 **保存**.

### 测试集成

配置Bitbucket集成后，您可以使用验证集成是否可以正常工作 `magento-cloud` CLI：

```bash
magento-cloud integration:validate
```

或者，您也可以通过将简单的更改推送到Bitbucket存储库来进行测试。

1. 创建测试文件。

   ```bash
   touch test.md
   ```

1. 提交更改并将其推送到您的Bitbucket存储库。

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. 登录到 [[!DNL Cloud Console]](../project/overview.md) 并验证是否显示提交消息以及您的项目是否正在部署。

   ![测试Bitbucket集成](../../assets/bitbucket-integration.png)

## 创建云分支

Bitbucket集成无法在Adobe Commerce on cloud基础架构项目中激活新环境。 如果您使用Bitbucket创建环境，则必须手动激活该环境。 要避免此额外步骤，最佳做法是使用 `magento-cloud` CLI工具或 [!DNL Cloud Console].

**激活使用比特桶创建的分支**：

1. 使用 `magento-cloud` CLI推送分支。

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```terminal
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. 验证环境是否处于活动状态。

   ```bash
   magento-cloud environment:list
   ```

   ```terminal
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

创建环境后，您可以使用常规Git命令将相应的分支推送到远程Bitbucket存储库。 对您在Bitbucket中的分支所做的后续更改会自动构建和部署环境。

## 删除集成

您可以安全地从项目中移除Bitbucket集成，而不会影响您的代码。

**要删除Bitbucket集成，请执行以下操作**：

1. 从终端，登录到您的Adobe Commerce on cloud infrastructure项目。

1. 列出您的集成。 您需要Bitbucket集成ID才能完成下一步。

   ```bash
   magento-cloud integration:list
   ```

1. 删除集成。

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

此外，您还可以通过登录到您的Bitbucket帐户并撤销帐户上的OAuth授权来删除Bitbucket集成 _设置_ 页面。

## Bitbucket服务器集成

要使用Bitbucket服务器集成，您需要满足以下条件：

- [比特桶访问令牌](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html) — 生成授予项目的令牌 `read` 访问和存储库 `admin` 访问
- [Bitbucket服务器URL](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html) — 添加Bitbucket实例的基本URL

虽然您可以使用Cloud CLI来完成Bitbucket服务器集成步骤，但完整的命令如下所示：

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

有关更多使用要求和选项，请使用help命令： `magento-cloud integration:add --help`
