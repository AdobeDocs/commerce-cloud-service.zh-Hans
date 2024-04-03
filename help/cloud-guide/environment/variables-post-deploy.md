---
title: 部署后变量
description: 请参阅环境变量列表，这些变量用于控制Adobe Commerce中针对云基础架构部署后阶段的操作。
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: 8b02757591c4e8f607e936de4eda74d76953d9b7
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# 部署后变量

以下各项 _部署后_ 变量控制部署后阶段的操作，并可继承和覆盖以下位置的值： [全局变量](variables-global.md). 将这些变量插入到 `post-deploy` 阶段 `.magento.env.yaml` 文件：

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

有关自定义生成和部署过程的详细信息：

- [部署配置](configure-env-yaml.md)
- [部署过程](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **默认**— `[]` （空数组）
- **版本**—Adobe Commerce 2.1.4及更高版本

配置 _到第一个字节的时间_ (TTFB)测试指定页面，以测试网站性能。 为需要测试的每个页面指定绝对路径引用，或指定带有协议和主机的URL。

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

指定要测试和提交更改的页面后， _到第一个字节的时间_ 测试在部署后阶段运行，并将每个路径的结果发布到云日志：

```terminal
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

对于重定向路径，日志会报告重定向目标的路径，而不是环境变量中配置的路径。 如果指定的路径无效，日志将显示警告消息。

## `WARM_UP_CONCURRENCY`

- **默认**—_未设置_
- **版本**—Adobe Commerce 2.1.4及更高版本

指定在高速缓存预热操作期间发送的并发请求数限制，以减少服务器负载。 此值限制并行连接的数量，对于 `WARM_UP_PAGES` 部署后变量指定多个用于缓存预加载的页面。

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **默认**— `index.php`
- **版本**—Adobe Commerce 2.1.4及更高版本

自定义用于在中预载缓存的页面列表 `post_deploy` 暂存。 您必须配置部署后挂接。 请参阅 [挂接区域](../application/hooks-property.md) 的 `.magento.app.yaml` 文件。

- **单页面** — 指定要添加到高速缓存中的单个页。 您不必指定默认的基本URL。 以下示例缓存 `BASE_URL/index.php` 页面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **多个域** — 列出多个URL。 以下示例缓存来自两个域的页面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **多页** — 使用以下格式根据特定的正则表达式模式缓存多个页面：

  ```terminal
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`：可能的变量 `category`， `cms-page`， `product`， `store-page`
   - `pattern|url|product_sku`：使用 `regexp` 模式或完全匹配 `url` 要筛选URL，或对所有页面使用星号(\*)。 将产品SKU用于 `product` 实体类型
   - `store_id|store_code`：使用商店的ID或代码或星号(\*)表示所有商店，您可以传递多个以分隔的商店ID或代码 `|`

  以下示例缓存了 `category` 和 `cms-page` 基于以下条件的实体类型：
   - 具有ID的商店的所有类别页面 `1`
   - 具有代码的存储的所有类别页面 `store1` 和 `store2`
   - 类别页面 `cars` 用于带有代码的存储 `store_en`
   - cms页面 `contact` 适用于所有商店
   - cms页面 `contact` 适用于具有ID的商店 `1` 和 `2`
   - 任何包含的类别页面 `car_` 结束于 `html` （对于ID为2的存储）
   - 任何包含的类别页面 `tires_` 用于带有代码的存储 `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  以下示例缓存了 `product` 实体类型，基于以下条件：
   - 所有商店的所有产品（通过编程方式限制为每个商店100个产品，以避免性能问题）
   - 商店的所有产品 `store1`
   - 产品与 `sku1` 适用于所有商店
   - 产品与 `sku1` 适用于包含代码的商店 `store1` 和 `store2`
   - 产品与 `sku1`， `sku2` 和 `sku3` 适用于包含代码的商店 `store1` 和 `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  以下示例缓存了 `store-page` 实体类型，基于以下条件：
   - 页面 `/contact-us` 适用于所有商店
   - 页面 `/contact-us` 用于具有ID的存储 `1`
   - 页面 `/contact-us` 适用于包含代码的商店 `code1` 和 `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
