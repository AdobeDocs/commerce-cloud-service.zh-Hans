---
title: 自定义主题
description: 了解如何在云基础架构上安装Adobe Commerce的自定义主题。
feature: Cloud, Themes
exl-id: f08134ab-daea-471d-a927-02531d36a809
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# 自定义主题

您可以安装一个或多个主题以用于项目中的一个或多个商店和网站。 主题包括多个静态文件（包括图像、字体、CSS、JavaScript、PHP等），以便完全设计存储。 您可以通过提取主题代码到文件系统或使用编辑器来添加主题。

## 手动安装主题

要手动安装主题，主题代码必须位于压缩存档中或类似于以下内容的目录结构中：

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**手动安装主题**：

1. 将主题的代码复制到下 `<Project root dir>/app/design/frontend` 店面主题或 `<Project root dir>/app/design/adminhtml` 用于管理主题。 验证顶层目录是否为 `<VendorName>`；否则，该主题将无法正确安装。

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 确认将主题复制到正确位置。

   * 店面主题： `ls <project-root>/app/design/frontend`
   * 管理员主题： `ls <project-root>/app/design/adminhtml`

   下面是一个示例：

   示例主题Adobe Commerce

1. 添加和提交文件。

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. 将文件推送到您的分支。

   ```bash
   git push origin <branch name>
   ```

1. 等待部署完成。
1. 登录到管理员。
1. 单击 **内容** >设计> **主题**.

   主题显示在右侧窗格中。

## 使用编辑器安装主题

使用Composer安装主题与使用Composer安装任何其他扩展相同。 请参阅 [安装、管理和升级模块](extensions.md) 以了解详细信息。

要使用编辑器安装主题，请执行以下操作：

1. 从Commerce Marketplace购买主题。
1. 获取主题的编辑器名称。
1. 转到Adobe Commerce根目录并输入命令：

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   例如，

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. 等待依赖项更新。
1. 输入以下命令：

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. 登录到管理员。
1. 单击 **内容** >设计> **主题**.

   主题显示在右侧窗格中。

## 多个主题

使用多个主题（如每种区域设置使用不同的主题）时，请查看 `SCD_MATRIX` 用于自定义主题部署的环境变量。 请参阅 [生成](../environment/variables-build.md#scd_matrix) 或 [部署](../environment/variables-deploy.md#scd_matrix) 中的阶段 [环境配置](../environment/configure-env-yaml.md).
