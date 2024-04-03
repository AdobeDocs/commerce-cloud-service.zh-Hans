---
title: Cloud CLI
description: 了解magento-cloud CLI以及它如何帮助您在云基础架构项目上管理Adobe Commerce的本地开发环境。
exl-id: 70dddd62-0269-4af4-bd2a-1a4fbf11a131
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 0%

---


# Cloud CLI

此 `magento-cloud` CLI工具使开发人员和系统管理员能够管理云项目和环境、执行例程和运行自动化任务。 此 `magento-cloud` CLI扩展了 [[!DNL Cloud Console]](../../get-started/cloud-console.md). 安装之后 `magento-cloud` 本地工作站上的CLI，您可以用它来管理云基础架构上的Adobe Commerce Starter和Pro集成环境。

**安装 `magento-cloud` CLI**：

1. 在本地工作站上，切换到要克隆云项目的目录和 [文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) 具有 _写入_ 访问权限。

1. 安装 `magento-cloud` CLI

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. 添加 `magento-cloud` CLI到bash配置文件。

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 重新加载更新的bash配置文件。

   ```bash
   . ~/.bash_profile
   ```

1. 要启动CLI，请调用 `magento-cloud` 并在出现提示时输入您的Cloud帐户凭据。

   ```bash
   magento-cloud
   ```

   ```terminal
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. 验证 `magento-cloud` 命令位于您的路径中。 以下示例列出了可用的命令。

   ```bash
   magento-cloud list
   ```

## 常用命令

Adobe设计了这些命令以管理云集成环境，并建议您运行 `magento-cloud` 项目目录中的CLI，以便可以省略 `-p <project-ID>` 参数。

以下列出了常用的 `magento-cloud` CLI命令仅包括必需选项。 您可以使用 `--help` 选项，以查看更多信息。

| 命令 | 描述 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | 登录到项目。 |
| `magento-cloud list` | 列出CLI工具的可用命令。 |
| `magento-cloud environment:list` | 列出当前项目中的环境。 |
| `magento-cloud environment:checkout` | 查看现有环境。 |
| `magento-cloud environment:merge -e` | 将此环境中的更改与其父级合并。 |
| `magento-cloud variables` | 此环境中的列表变量。 |
| `magento-cloud ssh` | 使用SSH连接到远程环境。 |
| `magento-cloud url` | 在浏览器中打开Adobe Commerce店面。 |
| `magento-cloud web` | 打开 [!DNL Cloud Console]. |

## 环境命令

环境 _name_ 与环境不同 _ID_ 仅当在环境名称中使用空格或大写字母时。 环境ID由所有小写字母、数字和允许的符号组成。 环境名称中的大写字母在ID中转换为小写；环境名称中的空格转换为破折号。

环境名称 _无法_ 包括为Linux shell或正则表达式保留的字符。 禁止使用的字符包括大括号(`{ }`)，圆括号，星号(`*`)，尖括号(`< >`)，&amp;符号(`&`)，% (`%`)，以及其他字符。

此 `magento-cloud environment:list` 命令显示环境层次结构，而 `git branch` 不会。 如果您有任何嵌套环境，请使用以下内容：

```bash
magento-cloud environment:list
```

### 重新部署环境

不使用推送触发重新部署。 验证并确认要重新部署的环境。 如果内部版本处于待处理状态，请勿使用重新部署。

```bash
magento-cloud environment:redeploy
```

示例响应：

```terminal
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git命令

您可能会注意到其中的一些命令与Git命令类似。 此 `magento-cloud` 命令可直接连接到基于Git的云项目，该项目具有其他功能。 如果您不使用创建分支 `magento-cloud` 在CLI中，它不是“已激活”的，而且当您将更改推送到远程环境时，它不会自动构建。 此 `magento-cloud` CLI命令包括激活。

要创建分支，请使用 `magento-cloud` 命令，以激活分支。

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

对于分支状态：

- 使用 `magento-cloud env` 命令查看环境分支及其状态：活动或非活动。
- 使用 `magento-cloud environment:activate` 命令来激活环境分支。

推送空的Git承诺以触发部署。 例如：

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

某些操作（如添加用户）不会导致部署。

### 创建环境分支

以下步骤演示了如何交替使用CLI和Git命令来管理本地环境：

1. 在本地工作站上，转到您的项目目录。

1. 切换到 [文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. 登录到您的项目。

   ```bash
   magento-cloud login
   ```

1. 列出您的项目。

   ```bash
   magento-cloud project:list
   ```

1. 列出项目中的环境。 每个环境都包含一个活动Git分支，其中包含代码、数据库、环境变量、配置和服务。

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >使用 `magento-cloud environment:list` 命令，因为它显示环境层次结构，而 `git branch` 命令没有。

1. 获取来源分支以获取最新代码。

   ```bash
   git fetch origin
   ```

1. 结帐或切换到特定分支和环境。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git命令仅检查Git分支。 此 `magento-cloud checkout` 命令检出分支并切换到活动环境。

   >[!TIP]
   >
   >您可以使用创建环境分支 `magento-cloud environment:branch <environment-name> <parent-environment-ID>` 命令语法。 创建和激活环境分支可能需要一些额外的时间。

1. 使用环境ID将任何更新的代码拉入到您的本地。 如果环境分支是新的，则不必执行此操作。

   ```bash
   git pull origin <environment-ID>
   ```

1. (_可选_)创建一个 [快照](../storage/snapshots.md) 环境作为备份。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## 更新CLI

此 `magento-cloud` CLI会在您登录时检查可用更新，但您可以使用 `self:update` 命令。 如果有可用的更新，请按照说明更新CLI。

如果您的 `magento-cloud` CLI是最新的，您将看到以下响应：

```bash
magento-cloud update
```

```terminal
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
