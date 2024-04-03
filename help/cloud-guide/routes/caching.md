---
title: 缓存
description: 了解如何在云基础架构环境中为Adobe Commerce启用缓存。
feature: Cloud, Cache, Routes
exl-id: 4856aa94-2947-4dc8-b0d1-0960869dc39c
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 缓存

您可以在云基础架构项目环境中启用缓存。 如果禁用缓存，则Adobe Commerce直接提供文件。

{{route-placeholder}}

## 设置缓存

通过在以下位置配置缓存规则，启用应用程序的缓存： `.magento/routes.yaml` 文件如下所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## 基于路由的缓存

通过分别为多个路由设置缓存规则来启用细粒度缓存，如以下示例所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

上述示例缓存以下路由：

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

以下路由为 **非** 已缓存：

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>路由中的正则表达式为 **非** 受支持。

## 缓存持续时间

缓存持续时间由 `Cache-Control` 响应标头值。 如果否 `Cache-Control` 标头在响应中， `default_ttl` 键已使用。

## 缓存键

为了确定如何缓存响应，Adobe Commerce构建了一个依赖于多个因素的缓存键并存储与此键关联的响应。 当请求带有相同的缓存键时，将重用响应。 其目的与HTTP类似 [`Vary` 标题](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

参数 `headers` 和 `cookies` 键允许您更改此缓存键。

这些键的默认值如下：

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## 缓存属性

### `enabled`

当设置为 `true`，为此路由启用缓存。 当设置为 `false`，禁用此路由的缓存。

### `headers`

定义缓存键必须依赖的值。

例如，如果 `headers` 关键如下：

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

然后Adobe Commerce会为的每个值缓存不同的响应 `Accept` HTTP标头。

### `cookies`

此 `cookies` key定义缓存键必须依赖的值。

例如：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

缓存键取决于 `value` 请求中的Cookie。

如果出现以下情况，则会出现特殊情况 `cookies` 键具有 `["*"]` 值。 此值表示任何带有Cookie的请求都会绕过缓存。 这是默认值。

>[!NOTE]
>
>不能在Cookie名称中使用通配符。 使用精确的Cookie名称或用星号(`*`)。 例如， `SESS*` 或 `~SESS` 当前为 **非** 有效值。

Cookie具有以下限制：

- 您最多可以设置 **50个Cookie** 在系统中。 否则，应用程序将丢弃 `Unable to send the cookie. Maximum number of cookies would be exceeded` 例外。
- Cookie的最大大小为 **4096字节**. 否则，应用程序将丢弃 `Unable to send the cookie. Size of '%name' is %size bytes` 例外。

### `default_ttl`

如果响应没有 `Cache-Control` 标题， `default_ttl` 键用于定义缓存持续时间（以秒为单位）。 默认值为 `0`，表示不缓存任何内容。
