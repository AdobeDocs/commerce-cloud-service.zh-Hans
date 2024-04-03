---
title: 用于允许请求的自定义VCL
description: 使用Fastly Edge ACL列表和自定义VCL代码片段，筛选传入请求并允许按IP地址访问Adobe Commerce站点。
feature: Cloud, Configuration, Security
exl-id: a6ee958a-c3d3-47be-b2df-510707f551fc
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# 用于允许请求的自定义VCL

您可以使用带有自定义VCL代码片段的Fastly Edge ACL列表来过滤传入请求并允许按IP地址访问。 ACL列表指定要允许的IP地址。

创建一个允许列表以限制对暂存环境的访问，以便只允许内部开发人员和已批准的外部服务来自指定IP地址的请求。 您还可以创建一个允许列表，以确保对暂存环境和生产环境管理员的访问权限。

以下示例说明如何将自定义VCL代码片段与 [Fastly访问控制列表(ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls) 保护管理员对云基础架构项目环境上Adobe Commerce的访问权限。 将自定义VCL代码段添加到云环境时，Fastly仅允许来自ACL中包含的IP地址的请求。

>[!TIP]
>
>对于不应公开访问的暂存和集成环境，请使用中提供的HTTP访问控制选项。 [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) 按IP地址管理对整个站点的访问。

**先决条件：**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要包含在允许列表中的客户端IP地址列表

## 创建边缘ACL以允许客户端IP地址

Edge ACL创建IP地址列表来管理对站点的访问。 在此示例中，您创建了一个Edge ACL，并添加了一个允许访问项目环境管理员的客户端IP地址列表。

{{admin-login-step}}

1. 单击 **商店** >设置> **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **边缘ACL**.

1. 创建ACL容器：

   - 单击 **添加ACL**.

   - 在 *ACL容器* 页面，输入 **ACL名称**—`allowlist`.

   - 选择 **更改后激活** 以部署您对正在编辑的Fastly服务配置版本所做的更改。

   - 单击 **上传** 将ACL附加到您的Fastly服务配置。

1. 添加允许访问管理员的IP地址列表：

   - 单击的“设置”图标 `allowlist` ACL

   - 添加并保存 *IP值* 每个客户端的IP地址。

   - 单击 **取消** 以返回系统配置页面。

1. 单击 **保存配置**.

1. 根据页面顶部的通知刷新缓存。

## 创建自定义VCL代码片段以保护管理员访问权限

以下自定义VCL代码片段（JSON格式）显示了逻辑，用于筛选发送给管理员的请求，并在客户端IP地址与 `allowlist` ACL

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

早于 [创建自定义代码片段](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet) 在本例中，查看值以确定是否需要进行任何更改。 然后在相应的字段中输入每个值，例如 `type` 在“类型”字段中， `content` 到“内容”字段中。

- `name` — VCL代码片段的名称。 在本例中， `allowlist`.

- `priority`  — 确定VCL代码片段的运行时间。 优先级为 `5` 立即运行并检查管理员请求是否来自允许的IP地址。 该代码片段在任何默认MagentoVCL代码片段(`magentomodule_*`)分配了优先级50。 根据您希望代码片段运行的时间，将每个自定义代码片段的优先级设置为高于或低于50。 优先级较低的代码片段首先运行。

- `type`  — 指定在版本化VCL代码中插入代码片段的位置。 此VCL是 `recv` 代码片段类型，用于将代码片段添加到 `vcl_recv` 缺省的Fastly VCL代码下的子例程和任何对象上的子例程。

- `content`  — 要运行的VCL代码片段。 在此示例中，代码将过滤对管理员的请求，并允许在客户端IP地址与 `allowlist` ACL 如果地址不匹配，将使用 `403 Forbidden` 错误。

  如果管理员的URL已更改，请替换示例值 `/admin` 以及环境的URL。 例如， `/company-admin`.

在代码示例中，条件 `!req.http.Fastly-FF` 使用时很重要 [原点屏蔽](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). 请勿删除或编辑此代码。

查看并更新环境的代码后，使用以下任一方法将自定义VCL代码段添加到Fastly服务配置中：

- [添加来自管理员的自定义VCL代码片段](#add-the-custom-vcl-snippet). 如果您可以访问管理员，则建议使用此方法。 (需要 [适用于Magento2版本1.2.58的Fastly CDN模块](fastly-configuration.md#upgrade) 或更高版本。)

- 将JSON代码示例保存到文件(例如， `allowlist.json`)和 [使用Fastly API上传](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). 如果您无法访问管理员，请使用此方法。

## 添加自定义VCL代码片段

{{admin-login-step}}

1. 单击 **商店** >设置> **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **自定义VCL代码片段**.

1. 单击 **创建自定义代码片段**.

1. 添加VCL代码片段值：

   - **名称** — `allowlist`

   - **类型** — `recv`

   - **优先级** — `5`

   - 添加 **VCL** 代码片段内容：

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. 单击 **创建** 生成具有名称模式的VCL代码段文件 `type_priority_name.vcl`例如 `recv_5_allowlist.vcl`

1. 重新加载页面后，单击 **将VCL上传到Fastly** 在 *Fastly配置* 部分将文件添加到Fastly服务配置。

1. 上载完成后，根据页面顶部的通知刷新缓存。

Fastly在上传过程中验证VCL代码的更新版本。 如果验证失败，请编辑自定义VCL代码片段以解决该问题。 然后，再次上传VCL。

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
