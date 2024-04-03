---
title: 添加站点地图和搜索引擎机器人
description: 了解如何在云基础架构上将站点地图和搜索引擎机器人添加到Adobe Commerce。
feature: Cloud, Configuration, Search, Site Navigation
exl-id: b98f43fa-1878-466d-8ea0-1e7207af8b60
source-git-commit: ee1db75c73c086e0ea54e1a7591ca7f2b4d2b36d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# 添加站点地图和搜索引擎机器人

尝试生成并写入 `sitemap.xml` 文件到根目录会导致以下错误：

```terminal
Please make sure that "/" is writable by the web-server.
```

在云基础架构上使用Adobe Commerce，您只能写入特定目录，例如 `var`， `pub/media`， `pub/static`，或 `app/etc`. 当您生成 `sitemap.xml` 文件，您必须指定 `/media/` 路径。

您无需生成 `robots.txt` 文件，因为它会生成 `robots.txt` 按需提供内容并将其存储在数据库中。 您可以使用在浏览器中查看内容 `<domain.your.project>/robots.txt` 或 `<domain.your.project>/robots` 链接。

这需要ECE-Tools版本2002.0.12及更高版本，并带有更新版本 `.magento.app.yaml` 文件。 有关这些规则的示例，请参阅 [magento-cloud存储库](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**要生成 `sitemap.xml` 版本2.2及更高版本中的文件**：

1. 访问管理员。
1. 在 _营销_ 菜单，单击 **网站地图** 在 _SEO和搜索_ 部分。
1. 在 _网站地图_ 视图，单击 **添加站点地图**.
1. 在 _新建站点地图_ 视图，输入以下值：

   - **文件名**：`sitemap.xml`
   - **路径**：`/media/`

1. 单击 **保存并生成**. 新的站点地图将在 _网站地图_ 网格。
1. 单击 _Google的链接_ 列。

**将内容添加到 `robots.txt` 文件**：

1. 访问管理员。
1. 在 _内容_ 菜单，单击 **配置** 在 _设计_ 部分。
1. 在 _设计配置_ 视图，单击 **编辑** 中的网站 _操作_ 列。
1. 在 _主网站_ 视图，单击 **搜索引擎机器人**.
1. 更新 **编辑robots.txt的自定义指令** 字段。
1. 单击 **保存配置**.
1. 验证 `<domain.your.project>/robots.txt` 文件或 `<domain.your.project>/robots` 浏览器中的URL。

>[!NOTE]
>
>如果 `<domain.your.project>/robots.txt` 文件生成 `404 error`， [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 删除重定向 `/robots.txt` 到 `/media/robots.txt`.

## 使用Fastly VCL代码片段重写

如果您有不同的域并且需要单独的站点地图，则可以创建一个VCL以路由到正确的站点地图。 生成 `sitemap.xml` 文件，然后创建自定义Fastly VCL代码片段来管理重定向。 请参阅 [自定义Fastly VCL片段](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> 您可以从管理员UI或使用Fastly API上传自定义VCL片段。 请参阅 [自定义VCL代码片段示例和教程](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### 使用Fastly VCL代码片段进行重定向

创建自定义VCL代码片段以重写路径 `sitemap.xml` 到 `/media/sitemap.xml` 使用 `type` 和 `content` 键值对。

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

以下示例演示了如何重写 `robots.txt` 和 `sitemap.xml` 到 `/media/robots.txt` 和 `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**为特定域重定向使用Fastly VCL代码片段**：

创建 `pub/media/domain_robots.txt` 文件，其中域为 `domain.com`，并使用下一个VCL代码片段：

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL代码片段路由 `http://domain.com/robots.txt` 并呈现 `pub/media/domain_robots.txt` 文件。

要为配置重定向 `robots.txt` 和 `sitemap.xml` 在单个代码片段中，创建 `pub/media/domain_robots.txt` 和 `pub/media/domain_sitemap.xml` 文件，其中域为 `domain.com` 并使用下一个VCL代码片段：

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

在 `sitemap` 管理配置，您必须使用指定文件的位置 `pub/media/` 而不是 `/`.

### 按搜索引擎配置索引

激活 `robots.txt` 自定义项，您必须启用 **已为打开按搜索引擎编制索引`<environment-name>`** 选项。

![使用 [!DNL Cloud Console] 管理环境](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>如果您正在使用PWA Studio并且无法访问您配置的 `robots.txt` 文件，添加 `robots.txt` 到 [列入允许列表前名](https://github.com/magento/magento2-upward-connector#front-name-allowlist) 在 **商店** >配置> **常规** > **Web** >向上PWA配置。
