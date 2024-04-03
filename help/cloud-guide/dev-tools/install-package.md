---
title: 升级项目以使用ECE工具
description: 了解如何升级云基础架构项目上的Adobe Commerce以使用ECE-Tools包并利用最新的修复和功能。
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# 升级项目以使用ECE-Tools包

Adobe已弃用 `magento/magento-cloud-configuration` 和 `magento/ece-patches` 包支持 `ece-tools` 包，从而简化许多云流程。 如果您在云基础架构项目上使用旧版Adobe Commerce，而该项目可以 _非_ 包含 `ece-tools` 包，则必须执行一次性、手动 _升级_ 流程到您的项目。

>[!WARNING]
>
>如果您的项目包含 `ece-tools` 包中，您可以跳过以下升级。 要进行验证，请检索 [!DNL Commerce] 版本使用 `php vendor/bin/ece-tools -V` 命令。

此项目升级过程要求您更新 `magento/magento-cloud-metapackage` 中的版本限制 `composer.json` 文件。 此限制允许对Adobe Commerce进行云基础架构中继（包括删除已弃用的包）的更新，而无需升级您当前的Adobe Commerce版本。

{{upgrade-tip}}

## 移除已弃用的包

在执行升级以使用 `ece-tools` 包，检查 `composer.lock` 以下已弃用包的文件：

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## 更新中继包

每个Adobe Commerce版本都需要一个基于以下条件的限制：

```terminal
>=current_version <next_version
```

- 对象 `current_version`，指定要安装的Adobe Commerce版本。
- 对象 `next_version`，请在中指定的值之后指定下一个修补程序版本。 `current_version`.

如果要安装Adobe Commerce `2.3.5-p2`，设置 `current_version` 到 `2.3.5` 和 `next_version` 到 `2.3.6`. 约束 `">=2.3.5 <2.3.6"` 安装适用于2.3.5的最新可用包。

您始终可以在 [`magento-cloud` 模板](https://github.com/magento/magento-cloud/blob/master/composer.json).

以下示例将Adobe Commerce对云基础架构的元包限制为大于或等于当前版本2.4.5且小于下一个版本2.4.6的任何版本：

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.5 <2.4.6"
},
```

## 升级项目

要将项目升级为使用 `ece-tools` 包中，必须更新中继包和 `.magento.app.yaml` 挂接属性，然后执行编辑器更新。

**要将项目升级为使用ece-tools**：

1. 更新 `magento/magento-cloud-metapackage` 中的版本限制 `composer.json` 文件。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.5 <2.4.6" --no-update
   ```

1. 更新中继。

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. 修改中的挂接命令 `magento.app.yaml` 文件。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 检查并删除 [已弃用的包](#remove-deprecated-packages). 已弃用的包可能会阻止成功升级。

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. 可能需要更新 `ece-tools` 包。

   ```bash
   composer update magento/ece-tools
   ```

1. 添加并提交代码更改。 在此示例中，更新了以下文件：

   ```terminal
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. 将代码更改推送到远程服务器，然后将此分支与 `integration` 分支。

   ```bash
   git push origin <branch-name>
   ```
