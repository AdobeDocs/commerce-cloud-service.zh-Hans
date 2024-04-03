---
title: 挂接属性
description: 请参阅中有关如何配置hooks属性的示例 [!DNL Commerce] 应用程序配置文件。
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# 挂接属性

使用 `hooks` 部分以在生成、部署和部署后阶段运行shell命令：

- **`build`** — 执行命令 _早于_ 打包应用程序。 诸如数据库或Redis之类的服务不可用，因为应用程序尚未部署。 添加自定义命令 _早于_ 默认 `php ./vendor/bin/ece-tools` 命令，以便自定义生成的内容可继续进入部署阶段。

- **`deploy`** — 执行命令 _之后_ 打包和部署应用程序。 此时您可以访问其他服务。 由于默认 `php ./vendor/bin/ece-tools` 命令复制 `app/etc` 目录到正确的位置，您必须添加自定义命令 _之后_ 用于防止自定义命令失败的部署命令。

- **`post_deploy`** — 执行命令 _之后_ 部署应用程序和 _之后_ 容器开始接受连接。 此 `post_deploy` 挂接清除缓存并预加载（加温）缓存。 您可以使用自定义页面列表 `WARM_UP_PAGES` 中的变量 [部署后阶段](../environment/variables-post-deploy.md). 尽管不是必需的，但是这与 `SCD_ON_DEMAND` 环境变量。

以下示例显示了 `.magento.app.yaml` 文件。 添加CLI命令 `build`， `deploy`，或 `post_deploy` 部分 _早于_ 该 `ece-tools` 命令：

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

此外，您还可以使用进一步自定义构建阶段 `generate` 和 `transfer` 专门构建代码或移动文件时执行其他操作的命令。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` — 导致挂接在第一个失败的命令上失败，而不是在最后一个失败的命令上失败。
- `build:generate` — 如果为构建阶段启用了SCD，则应用修补程序、验证配置、生成DI并生成静态内容。
- `build:transfer` — 将生成的代码和静态内容传输到最终目标。

命令从应用程序运行(`/app`)目录。 您可以使用 `cd` 命令更改目录。 如果挂接中的最终命令失败，则挂接将失败。 要使其在第一个失败的命令上失败，请添加 `set -e` 到钩子的开头。

**使用grunt编译Sass文件**：

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

编译Sass文件，使用 `grunt` 静态内容部署之前，该操作将在生成期间进行。 放置 `grunt` 命令位于 `build` 命令。

{{scenarios}}
