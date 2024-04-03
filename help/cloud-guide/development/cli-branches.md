---
title: 使用CLI管理分支
description: 了解如何使用Cloud CLI在云基础架构上管理Adobe Commerce的环境分支。
role: Developer
feature: Cloud, Install
exl-id: a871c7e2-4506-4a05-8fc2-fc5ef2afe609
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# 使用CLI管理分支

安装 `magento-cloud` CLI，请参见 [Cloud CLI参考](../dev-tools/cloud-cli-overview.md). 安装之后 `magento-cloud` CLI和设置SSH密钥以远程访问云基础架构，您可以使用 `magento-cloud` CLI命令用于管理项目的环境。 有关环境体系结构的信息，请参见 [入门级架构](../architecture/starter-architecture.md) 或 [专业体系结构](../architecture/pro-architecture.md).

使用管理分支和环境 [!DNL Cloud Console]，请参见 [使用管理分支 [!DNL Cloud Console]](../project/console-branches.md).

## 使用CLI命令

此 `magento-cloud` CLI命令与Git命令类似。 您可以使用它们连接到您的项目并管理环境。 虽然可以从任何目录运行命令，但建议从项目目录运行这些命令。 从项目目录运行时，可以省略 `-p <project-ID>` 参数。 请参阅 [Cloud CLI参考](../dev-tools/cloud-cli-overview.md).

## 克隆项目

以下说明使用的组合 `magento-cloud` CLI命令和Git命令用于将项目克隆到本地工作站。 要查看 `magento-cloud` CLI命令，使用 `magento-cloud list` 命令。

>[!IMPORTANT]
>
>某些Git命令无法在Adobe Commerce中完成云基础架构项目上的操作。 例如，您可以使用Git命令创建分支，但无法创建和激活新环境。 您必须使用创建环境 `magento-cloud environment:branch <branch-name>` 命令使环境变为 _活动_. 或者，您可以使用 [!DNL Cloud Console] 以创建活动环境。 请参阅 [Cloud CLI参考](../dev-tools/cloud-cli-overview.md#git-commands).

**克隆项目 `master` 环境**：

1. 使用登录到您的本地工作站 [文件系统所有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) 帐户。

1. 更改为Web服务器或虚拟主机 _docroot_ 目录。

1. 使用登录 `magento-cloud` CLI

   ```bash
   magento-cloud login
   ```

1. 列出您的项目。

   ```bash
   magento-cloud project:list
   ```

1. 克隆项目。

   ```bash
   magento-cloud project:get <project-ID>
   ```

   出现提示时，提供目录名称。

1. 更改为 `magento2` 目录。

1. 列出项目的可用环境。

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >此 `magento-cloud environment:list` 命令显示环境层次结构，而 `git branch` 命令没有。

1. 获取远程分支。

   ```bash
   git fetch origin
   ```

1. 提取更新的代码。

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>请参阅 [集成](../integrations/overview.md) 有关在云基础架构上将基于Git的托管服务与Adobe Commerce结合使用的信息。

## 创建用于开发的分支

在克隆项目并更新Adobe Commerce管理员帐户配置后，可以分支进行开发。 如前所述，您必须使用 `magento-cloud environment:branch <branch-name>` 命令或 [!DNL Cloud Console] 让环境成为 _活动_.

- 对象 [起始者](../architecture/starter-develop-deploy-workflow.md#clone-and-branch)，考虑为以下对象创建分支： `staging`，然后基于以下内容创建开发分支： `staging` 分支。
- 对象 [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow)，创建开发分支，基于 `Integration` 分支。

**创建开发分支**：

1. 在本地工作站上，转到您的项目目录。

1. 根据为您的项目工作流推荐的分支创建环境。

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 更新依赖关系。

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_可选_] 创建 [备份](../storage/snapshots.md) 环境的URL。

### 合并分支

完成开发后，将此分支合并到父级：

1. 提交和推送代码更改：

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 与父环境合并：

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 删除环境

仅当您确定不再需要某个环境时，才将其删除。 删除环境后无法恢复该环境。

>[!WARNING]
>
>您无法删除 `master` 任何项目的分支。

您必须是项目管理员、环境管理员或帐户所有者才能执行此任务。 请参阅 [管理用户对云项目的访问权限](../project/user-access.md).

删除环境时，环境将设置为 _不活动_. 该代码仍可在Git分支中使用，但不再包含服务或数据库。 要完全删除环境，您还必须删除相应的远程Git分支。

**删除环境**：

1. 在本地工作站上，转到您的项目目录。

1. 从远程服务器获取更新。

   ```bash
   git fetch
   ```

1. 删除环境分支。

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   或者，您也可以通过将多个环境ID添加到delete命令来一次删除多个环境。

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. 响应提示删除本地环境和相应的远程环境。

   ```terminal
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   删除环境会将其置于 _不活动_ 省/州。

   ```terminal
   Delete the remote Git branch too? [Y/n]
   ```

   删除远程Git分支会从项目中删除环境。

1. 等待环境删除。

   ```terminal
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>要激活不活动的环境，请使用 `magento-cloud environment:activate` 命令。

## 与远程环境交互

在您之后 [设置SSH密钥](../development/secure-connections.md)，您可以 [从本地工作区连接到远程环境](../development/secure-connections.md#connect-to-a-remote-environment) 并与project services交互和修改设置。
