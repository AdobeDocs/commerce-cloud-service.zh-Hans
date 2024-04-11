---
title: 备份管理
description: 了解如何在Cloud Infrastructure项目中手动创建和恢复Adobe Commerce的备份。
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: 069cbc233492d22932e8dce5bf0426dce8459727
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# 备份管理

您可以随时使用 **[!UICONTROL Backup]** 中的按钮 [!DNL Cloud Console] 或使用 `magento-cloud snapshot:create` 命令。

备份或 _快照_ 是环境数据的完整备份，包括来自运行服务（MySQL数据库）的所有永久数据以及存储在装入卷(var、pub/media、app/etc)上的任何文件。 快照可以 _非_ 包括代码，因为该代码已存储在基于Git的存储库中。 无法下载快照的副本。

备份/快照功能可以 **非** 适用于Pro暂存和生产环境，默认情况下会接收用于灾难恢复的常规备份。 请参阅 [专业备份和灾难恢复](../architecture/pro-architecture.md#backup-and-disaster-recovery) 以了解更多信息。 与Pro暂存和生产环境中的自动实时备份不同，备份是 **非** 自动。 它是 _您的_ 负责手动创建备份或设置cron作业，以定期创建Starter或Pro集成环境的备份。

## 创建手动备份

您可以从以下位置创建任何活动Starter环境和集成Pro环境的手动备份： [!DNL Cloud Console] 或从Cloud CLI创建快照。 您必须拥有 [管理员角色](../project/user-access.md) 为了环境。

**要使用，创建任何入门级环境的备份，请[!DNL Cloud Console]**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 从项目导航栏中选择一个环境。 环境必须处于活动状态。
1. 在 _备份_ 视图，单击 **[!UICONTROL Backup]**. 此选项不适用于Pro环境。

   ![备份](../../assets/button-backup.png){width="150"}

**要使用创建集成环境的备份，请执行以下操作[!DNL Cloud Console]**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 从项目导航栏中选择集成/开发环境。 环境必须处于活动状态。
1. 选择 **[!UICONTROL Backup]** 选项。 此选项适用于Starter和Pro环境。
1. 单击 **[!UICONTROL Yes]** 按钮。

**要使用创建快照 `magento-cloud` CLI**：

1. 在本地工作站上，转到您的项目目录。
1. 将环境分支签出到快照。
1. 创建快照。

   ```bash
   magento-cloud snapshot:create --live
   ```

   或者，您可以使用 `magento-cloud backup` 简短命令。 此 `--live` 选项使环境保持运行以避免停机。 要获取选项的完整列表，请输入 `magento-cloud snapshot:create --help`.

   示例响应：

   ```terminal
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. 验证最新的快照。

   ```bash
   magento-cloud snapshot:list
   ```

   该列表返回有关快照状态的信息：

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## 恢复手动备份

您必须拥有 [管理员访问权限](../project/user-access.md) 到环境。 您最多可以 **七天** 到 _恢复_ 手动备份。 恢复备份不会更改当前Git分支的代码。 以这种方式恢复备份不适用于Pro暂存和生产环境；请参阅 [专业备份和灾难恢复](../architecture/pro-architecture.md#backup-and-disaster-recovery).

恢复时间因数据库的大小而异：

- 大型数据库（200 GB以上）可能需要5小时
- 中型数据库(150 GB)可能需要2.5小时
- 小型数据库(60 GB)可能需要1小时

>[!TIP]
>
>无需备份即可恢复：
>
>- 要回滚到以前的代码或删除环境中添加的扩展，请参阅 [回滚代码](#roll-back-code).
>- 要恢复不稳定的环境，请执行以下操作 _非_ 有备份，请参见 [恢复环境](../development/restore-environment.md).

**使用[!DNL Cloud Console]**：

1. 登录到 [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. 从项目导航栏中选择一个环境。
1. 在 _备份_ 视图，从中选择备份 _存储_ 列表。 备份功能可以 **非** 适用于Pro环境。
1. 在 ![更多](../../assets/icon-more.png){width="32"} (_更多_)菜单，单击 **恢复**.
1. 查看“从备份中还原”信息，然后单击 **是，恢复**.

**使用Cloud CLI恢复快照**：

1. 在本地工作站上，转到您的项目目录。
1. 签出要恢复的环境分支。
1. 列出所有可用的快照。

   ```bash
   magento-cloud snapshot:list
   ```

   该列表返回有关可用快照的信息：

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. 使用列表中的快照ID恢复快照。

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## 回滚代码

备份和快照可以 _非_ 包括一个代码副本。 您的代码已存储在基于Git的存储库中，因此您可以使用基于Git的命令来回滚（或还原）代码。 例如，使用 `git log --oneline` 滚动浏览以前的提交，然后使用 [`git revert`](https://git-scm.com/docs/git-revert) 从特定提交还原代码。

此外，您可以选择将代码存储在 _不活动_ 分支。 使用Git命令而不是使用 `magento-cloud` 命令。 请参阅关于 [Git命令](../dev-tools/cloud-cli-overview.md#git-commands) （在Cloud CLI主题中）。
