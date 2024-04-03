---
title: 服务器端包括
description: 了解如何在云基础架构上将服务器端包含与Adobe Commerce结合使用。
feature: Cloud, Routes
exl-id: 34a38cb5-5f0e-49ac-9dba-bb581a06aeed
source-git-commit: 649c11b111aa9c9105e54908bf9c6f48741f10e4
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# 服务器端包括

[服务器端包括](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI)是HTML页中的指令，在页面呈现时在服务器上进行计算。 通过SSI，您可以将动态生成的内容添加到现有HTML页面，而无需为整个页面提供服务。

您可以在中按工艺路线激活或停用SSI。 `.magento/routes.yaml`；例如：

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

通过SSI，您可以在HTML响应指令中包含这些指令，这些指令会导致服务器填充HTML的各个部分，同时保留任何现有的部分 [缓存配置](caching.md).

以下示例显示如何在页面顶部插入动态日期控件，在底部插入每600秒更新一次的日期控件：

将以下内容添加到任何页面，例如 `/index.php`：

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

将以下内容添加到 `time.php`：

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

浏览到添加控件的页面。 刷新页面几次，并注意页面顶部的时间会更改，而底部的时间每600秒更改一次。
