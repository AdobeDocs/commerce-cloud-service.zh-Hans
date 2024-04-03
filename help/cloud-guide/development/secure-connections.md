---
title: 安全连接
description: 了解如何将SSH密钥应用于云基础架构项目上的Adobe Commerce并登录到远程环境。
role: Developer
feature: Cloud, Security
topic: Security
exl-id: b5780e8e-e3da-4b10-8ca3-2778085acd4a
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# 与远程环境的安全连接

Secure Shell (SSH)是用于安全登录到远程服务器和系统的常用协议。 您可以使用SSH访问远程环境，以便管理Adobe Commerce应用程序和访问远程环境日志。 Adobe仅支持使用SSH公钥的安全FTP (sFTP)连接。 不支持FTP连接。

## 生成SSH密钥对

在需要访问项目源代码和环境的每台计算机和工作区上创建一个SSH密钥对。 SSH密钥允许您连接到GitHub来管理源代码并连接到云服务器，而无需持续提供用户名和密码。 请参阅 [通过SSH连接到GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) 有关创建SSH密钥对的进一步说明。

- 此 _公钥_ 对于访问站点、SSH和sFTP，提供的内容是安全的。
- 此 _私钥_ 在工作站上保持私密状态。

>[!CAUTION]
>
>**切勿共享您的私钥。** 请勿将其添加到票证、复制到聊天中或附加到电子邮件中。

## 向帐户添加SSH公钥

在云基础架构帐户上将SSH公钥添加到Adobe Commerce后，重新部署帐户上的所有活动环境以安装密钥。

您可以使用以下方法之一将SSH密钥添加到帐户： Cloud CLI或 [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### 使用云CLI添加SSH密钥

1. 在本地工作站上，转到您的项目目录。

1. 登录项目：

   ```bash
   magento-cloud login
   ```

1. 添加公钥。

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>您可以使用Cloud CLI命令列出并删除SSH密钥 `ssh-key:list` 和 `ssh-key:delete`.

>[!TAB 控制台]

### 使用以下方式添加SSH密钥 [!DNL Cloud Console]

**将SSH密钥添加到新项目**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 单击 **[!UICONTROL No SSH key]**. 此图标位于命令字段的右侧，当项目不包含SSH密钥时可见。

1. 将公共SSH密钥的内容复制并粘贴到 **公钥** 字段。

1. 按照其余提示操作。

**要将SSH密钥添加到云配置文件，请执行以下操作**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 在右上角的帐户菜单中，单击 **我的个人资料**.

1. 在 _SSH密钥_ 视图，单击 **添加公钥**.

1. 在 _添加SSH密钥_ 表格，给你的钥匙 **标题**，并将公共SSH密钥粘贴到 **键** 字段。

1. 单击 **保存**.

>[!ENDTABS]

## 连接到远程环境

您可以使用连接到远程环境 `magento-cloud` CLI或SSH命令。 此 `magento-cloud` CLI命令只能在Starter和Pro集成环境中使用。

### 使用Cloud CLI

**登录到远程集成环境**：

1. 在本地工作站上，转到您的项目目录。

1. 列出该项目中的环境。

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### 使用SSH命令

此 [!DNL Cloud Console] 包含每个环境的Web和SSH访问命令列表。

**复制SSH命令**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. 从中选择一个项目 _所有项目_ 列表。

1. 选择环境。

1. 单击 **[!UICONTROL SSH]**.

1. 在 _SSH_ 选项卡，单击复制按钮以将完整的SSH命令复制到剪贴板。

1. 打开终端并粘贴SSH命令以创建连接。

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>对于Pro暂存和生产环境，SSH命令可能如下所示：
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

云基础架构上的Adobe Commerce支持使用采用SSH身份验证的sFTP（安全FTP）访问您的环境。 使用支持sFTP的SSH密钥身份验证的客户端并使用公共SSH密钥。 必须将公共SSH密钥添加到目标环境中。 对于入门环境和专业集成环境，您可以 [通过 [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

只读sFTP连接为 _非_ 支持；随附提供sFTP访问 _写入_ 默认权限。

配置sFTP时，请使用SSH访问环境命令中的信息： `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **用户名**：之前 `@` （在SSH访问目标中）。
- **密码**：您不需要sFTP密码。 sFTP访问使用SSH密钥身份验证。
- **主机**：之后的所有内容 `@` 访问SSH。
- **端口**：22，默认SSH端口。
- **SSH** 私钥：如有必要，请向sFTP客户端提供私钥的位置。 默认情况下，私钥存储在 `~/.ssh` 目录。

根据客户端的不同，可能需要其他选项来完成sFTP的SSH身份验证。 查看所选客户端的文档。

对象 **入门级环境和专业级集成环境**，您可能还需要 [添加 `mount`](../application/properties.md#mounts) 以访问特定目录。 您要将装载添加到您的 `.magento.app.yaml` 文件。 有关可写目录的列表，请参见 [项目结构](../project/file-structure.md). 此挂载点仅适用于这些环境。

对象 **专业暂存和生产环境**，如果您没有SSH访问环境的权限，则必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 请求sFTP访问以及访问特定文件夹的装入点，例如， `pub/media`.

>[!NOTE]
>对于Pro暂存和生产，如果sFTP连接用于 _通用_ 具有此功能的用户 **非** 需要 [已添加到云项目](../project/user-access.md)，您必须 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 使用他们的 **公共** 键已附加。 **切勿提供私有SSH密钥。**

## SSH隧道

您可以使用SSH隧道从本地开发环境连接到服务，就像该服务在本地一样。 在隧道之前，配置 [SSH](#add-an-ssh-public-key-to-your-account).

使用终端应用程序登录并发出命令。

```bash
magento-cloud login
```

使用验证是否打开了任何通道。

```bash
magento-cloud tunnel:list
```

要修建隧道，您必须知道 [应用程序名称](../application/properties.md#name). 您可以使用CLI检查应用程序名称：

```bash
magento-cloud apps
```

### 设置SSH通道

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

例如，打开通往 `sprint5` 在包含名为的应用程序的项目中分支 `mymagento`，输入

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

示例响应：

```terminal
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**显示有关通道的信息**：

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### 连接到服务

建立SSH隧道后，您可以像在本地运行一样连接到服务。 例如，要连接到数据库，请使用以下命令：

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
