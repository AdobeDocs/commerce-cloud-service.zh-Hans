---
title: 更新ECE工具包
description: 了解如何升级ECE-Tools包，以利用在云基础架构上应用于Adobe Commerce的最新修复和功能。
feature: Cloud, Upgrade
exl-id: 7cce45eb-ae53-4468-b16d-4f4d3422ac52
source-git-commit: 513bc5b52f046ffd98005d80f34725b7f60b38bd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 更新ECE工具包

的更新 `ece-tools` 包也会更新其他 [适用于Commerce包的云工具套件](../release-notes/cloud-tools-suite.md)，即的依赖项 `ece-tools`. Adobe Commerce因此，您必须在云基础架构上使用支持 `ece-tools` 包。

{{ece-tools-package}}

**先决条件**：

- 更新之前 `ece-tools`，查看 [Cloud Tools Suite for Commerce发行说明](../release-notes/cloud-tools-suite.md).
- 如果您要从 `ece-tools` 2002.0.22或更早版本至2002.1.0，审查 [向后不兼容的更改](../release-notes/backward-incompatible-changes.md) 并对您的Adobe Commerce on cloud infrastructure项目进行任何所需的更改。
- 审核 [升级和修补程序](../development/commerce-version.md#upgrade-from-older-versions) 确定与云基础架构项目上的Adobe Commerce兼容的ECE-Tools版本。

{{upgrade-tip}}

**要更新 `ece-tools` 包**：

1. 在本地工作站上，使用Composer执行更新。

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >如果更新时间不能超过 `ece-tools` 版本2002.0.8，请参见 [升级项目以使用ECE-Tools包](install-package.md).

1. 添加、提交和推送代码更改。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 测试验证后，将此分支合并到集成分支。
