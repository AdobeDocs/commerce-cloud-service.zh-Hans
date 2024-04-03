---
title: Web属性
description: 请参阅中有关如何配置Web属性的示例 [!DNL Commerce] 应用程序配置文件。
feature: Cloud, Configuration
exl-id: 2ca94908-6fe1-42fd-bc3b-ef2dd473f1bb
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Web属性

此 `web` 属性定义应用程序向Web公开的方式（在HTTP中），确定Web应用程序提供内容的方式，并通过在每个位置设置规则来控制应用程序容器如何响应传入请求 _块_. 块表示以正斜杠(`/`)。

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

您可以微调 `locations` 使用以下键值为每个进行配置 `locations` 块：

| 属性 | 描述 |
| :--- | :--- |
| `allow` | 提供不符合“规则”的文件。 默认值= `true` |
| `expires` | 设置要在浏览器中缓存内容的秒数。 此键可启用 `cache-control` 和 `expires` 静态内容的标头。 如果未设置此值，则 `expires` 提供静态内容文件时不包括指令和生成的标头。 A负值1 (`-1`)值将导致无缓存，并且是默认值。 可以使用以下单位表示时间值：  `ms` （毫秒）， `s` （秒）， `m` （分钟）， `h` （小时）、 `d` （天）， `w` （周）， `M` （月，30天），或 `y` （年，365天） |
| `headers` | 设置自定义标头，例如 `X-Frame-Options`，用于从此位置提供的静态内容。 |
| `index` | 列出为应用程序提供服务的静态文件，例如 `index.html` 文件。 此键需要一个收藏集。 仅当对一个或多个文件的访问被“允许”时，此选项才有效。 `allow` 或 `rules` 此位置的密钥。 |
| `rules` | 指定位置的覆盖。 使用正则表达式匹配请求。 如果传入请求与规则匹配，则规则中使用的键会覆盖请求的常规处理。 |
| `passthru` | 设置当找不到静态文件或PHP文件时使用的URL。 通常，此URL是应用程序的前端控制器，例如 `/index.php` 或 `/app.php`. |
| `root` | 设置相对于Web上公开的应用程序根目录的路径。 默认情况下，云项目的公共目录（位置“/”）设置为“pub”。 |
| `scripts` | 允许在此位置加载脚本。 将值设置为 `true` 以允许脚本。 |

默认配置允许执行以下操作：

- 从根(`/`)路径，只能访问Web和媒体
- 从 `~/pub/static` 和 `~/pub/media` 路径，任何文件都可以访问

以下示例显示了 `.magento.app.yaml` 一组可访问Web的位置(与  [`mounts` 属性](properties.md#mounts)：

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>此示例显示配置为支持单个域的云项目的默认Web配置。 对于需要支持多个网站或商店的项目， `web` 必须设置配置才能支持共享域。 请参阅 [为共享域配置位置](../store/multiple-sites.md#configure-locations-for-shared-domains).
