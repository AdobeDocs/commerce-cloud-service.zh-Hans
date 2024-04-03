---
title: 用于阻止请求的自定义VCL
description: 使用带有自定义VCL片段的边缘访问控制列表(ACL)，按IP地址阻止传入请求。
feature: Cloud, Configuration, Security
exl-id: 1f637612-3858-49d0-91f7-9b8823933cc9
source-git-commit: 0e9ace747cc56808108781e42b97c86756089818
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# 用于阻止请求的自定义VCL

您可以使用适用于Magento2的Fastly CDN模块创建一个边缘ACL，其中包含要阻止的IP地址列表。 然后，您可以将该列表与VCL代码段一起使用来阻止传入的请求。 该代码检查传入请求的IP地址。 如果与ACL列表中包含的IP地址匹配，Fastly将阻止请求访问您的站点并返回 `403 Forbidden error`. 允许访问所有其他客户端IP地址。

**先决条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要阻止的客户端IP地址列表

## 创建边缘ACL以阻止客户端IP地址

创建边缘ACL以定义要阻止的IP地址列表。 创建ACL后，您可以在自定义VCL代码片段中使用它来管理对暂存或生产站点的访问。

通过在两个环境中创建同名的Edge ACL来管理暂存和生产站点的访问。 VCL代码片段代码适用于这两个环境。

1. 登录到管理员。
1. 导航到 **商店** >设置> **配置** > **高级** > **系统** > **全页缓存** > **Fastly配置**.
1. 展开 **边缘ACL** 部分。
1. 单击 **添加ACL** 创建列表。 列入阻止列表在此示例中，将列表命名为“”。
1. 在列表中输入IP地址值。 添加到此列表的任何客户端IP地址都将被阻止，并且无法访问该站点。
1. （可选）选择 **已否定** 复选框（如果需要）。

在VCL代码片段中按名称引用Edge ACL。

## 列入阻止列表为创建自定义VCL

>[!NOTE]
>
>此示例向高级用户展示如何创建VCL代码段来配置自定义阻止规则以上载到Fastly服务。 您可以使用根据Adobe Commerce管理员提供的国家/地区来配置阻止列表或允许列表 [阻止](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) 可在Fastly CDN中用于Magento2模块的功能。

定义Edge ACL后，可以使用它创建VCL代码片段，以阻止对ACL中指定的IP地址的访问。 您可以在暂存环境和生产环境中使用相同的VCL代码片段，但必须分别将该代码片段上传到每个环境。

列入阻止列表以下自定义VCL代码片段（JSON格式）显示了使用与ACL中的地址匹配的客户端IP地址阻止传入请求的逻辑。

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

在基于此示例创建代码片段之前，请查看值以确定是否需要进行任何更改：

- `name`：VCL代码片段的名称。 在本例中，我们使用名称 `blocklist`.

- `priority`：确定VCL代码片段的运行时间。 优先级为 `5` 立即运行并检查管理员请求是否来自允许的IP地址。 该代码片段在任何默认MagentoVCL代码片段(`magentomodule_*`)分配了优先级50。 根据您希望代码片段运行的时间，将每个自定义代码片段的优先级设置为高于或低于50。 优先级较低的代码片段首先运行。

- `type`：指定VCL代码片段的类型，以确定代码片段在生成的VCL代码中的位置。 在此示例中，我们使用 `recv`，将VCL代码插入 `vcl_recv` 副例程，位于样板VCL下方，以及任何对象上方。 请参阅 [Fastly VCL代码段引用](https://docs.fastly.com/api/config#api-section-snippet) 以获取代码片段类型的列表。

- `content`：要运行的VCL代码片段，用于检查客户端IP地址。 如果IP位于边缘ACL中，则会通过 `403 Forbidden` 整个网站出错。 允许访问所有其他客户端IP地址。

查看并更新环境的代码后，使用以下任一方法将自定义VCL代码段添加到Fastly服务配置中：

- [添加来自管理员的自定义VCL代码片段](#add-the-custom-vcl-snippet). 如果您可以访问管理员，则建议使用此方法。 (需要 [Fastly版本1.2.58](fastly-configuration.md#upgrade-fastly-module) 或更高版本。)

- 将JSON代码示例保存到文件(例如， `blocklist.json`)和 [使用Fastly API上传](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). 如果您无法访问管理员，请使用此方法。

## 添加自定义VCL代码片段

{{admin-login-step}}

1. 单击 **商店** >设置> **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **自定义VCL代码片段**.

1. 单击 **创建自定义代码片段**.

1. 添加VCL代码片段值：

   - **名称** — `blocklist`

   - **类型** — `recv`

   - **优先级** — `5`

   - 添加 **VCL** 代码片段内容：

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. 单击 **创建** 生成具有名称模式的VCL代码段文件 `type_priority_name.vcl`例如 `recv_5_blocklist.vcl`

1. 重新加载页面后，单击 **将VCL上传到Fastly** 在 *Fastly配置* 部分将文件添加到Fastly服务配置。

1. 上传后，根据页面顶部的通知刷新缓存。

Fastly在上传过程中验证VCL代码的更新版本。 如果验证失败，请编辑自定义VCL代码片段以解决该问题。 然后，再次上传VCL。

## 阻塞请求的其他VCL示例

以下示例显示如何使用内联条件语句而不是ACL列表来阻止请求。

>[!WARNING]
>
>在这些示例中，VCL代码的格式为JSON有效负荷，该有效负荷可以保存到文件中并在Fastly API请求中提交。 您可以提交 [管理员的VCL代码片段](#add-the-custom-vcl-snippet)或作为JSON字符串使用Fastly API。 要防止在将Fastly API与JSON字符串一起使用时进行验证，必须使用反斜杠对特殊字符进行转义。

请参阅 [使用动态VCL片段](https://docs.fastly.com/vcl/vcl-snippets/) 在Fastly VCL文档中。

### VCL代码示例：按国家/地区代码分块

此示例使用双字符ISO 3166-1国家/地区代码来表示与IP地址关联的国家/地区。

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>您可以使用Fastly代替使用自定义VCL代码片段 [阻止](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) Adobe Commerce on cloud infrastructure中的功能管理员可按国家/地区代码或国家/地区代码列表配置阻止。

### VCL代码示例： Block by HTTP User-Agent请求标头

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
