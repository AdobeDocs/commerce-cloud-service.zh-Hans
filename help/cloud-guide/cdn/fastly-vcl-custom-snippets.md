---
title: 开始使用自定义VCL代码片段
description: 了解如何使用Varnish控制语言代码片段自定义Adobe Commerce的Fastly服务配置。
feature: Cloud, Configuration, Services
exl-id: df0f0906-8ffa-41a1-a31c-d36deb5a6a31
source-git-commit: 89c57a486545d6165e0407f913f4fa4cf6c95abe
workflow-type: tm+mt
source-wordcount: '1947'
ht-degree: 0%

---

# 自定义VCL快速入门

Fastly支持自定义版本的Varnish Configuration Language (VCL)，以根据您的要求定制Fastly服务配置。

自定义VCL代码段是添加到已上传到Adobe Commerce站点的活动VCL版本中的VCL逻辑块。 自定义VCL代码段可修改Fastly缓存服务响应请求流量的方式。 例如，您可以添加自定义VCL代码片段，以仅允许来自指定客户端IP地址的请求流量。 或者，创建一个代码片段以阻止来自已知向Adobe Commerce网站发送推荐垃圾邮件的网站的流量。

自定义VCL片段（生成、编译并传输到所有Fastly缓存）无需服务器停机即可加载和激活。

>[!NOTE]
>
>在将自定义VCL代码、边缘词典和ACL添加到Fastly模块配置之前，请验证Fastly缓存服务是否可与默认配置配合使用。 请参阅 [配置Fastly服务](fastly-configuration.md).

Fastly支持两种类型的自定义VCL片段：

- [常规代码片段](https://docs.fastly.com/en/guides/about-vcl-snippets) — 针对特定VCL版本对自定义常规VCL代码段进行编码。 您可以从Admin或Fastly API创建、修改和部署常规VCL片段。

- [动态代码片段](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) — 使用Fastly API创建的VCL代码段。 您可以修改和部署动态代码片段，而无需更新服务的Fastly VCL版本。

我们建议将自定义VCL片段与Edge字典和访问控制列表(ACL)结合使用，以存储自定义代码中使用的数据。

- [**Edge词典**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) — 将数据作为键值对存储在可从自定义VCL代码片段引用的字典容器中

- [**边缘ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) — 存储客户端IP地址数据，该数据定义了使用自定义VCL代码段实施的块或允许规则的访问控制列表

字典和ACL数据会部署到可跨网络区域访问的Fastly Edge节点。 此外，数据可以跨网络动态更新，无需您为暂存或生产环境重新部署VCL代码。

>[!NOTE]
>
>只有在具备以下条件时，才能将自定义VCL代码段添加到暂存或生产环境： [配置的Fastly服务](fastly-configuration.md) 为了那个环境。

## 教程

本教程和示例演示了如何使用常规自定义VCL片段与Edge字典和Edge ACL来自定义Adobe Commerce的Fastly服务配置。 有关更多详细信息，请参阅Fastly文档：

- [Fastly VCL指南](https://docs.fastly.com/guides/vcl/guide-to-vcl) — 有关Fastly Varnish实施、Fastly VCL扩展以及了解有关Varnish和VCL的更多信息的资源。
- [Fastly VCL引用](https://docs.fastly.com/guides/vcl/) — 用于开发和排除Fastly自定义VCL和自定义VCL片段的详细编程参考。

您可以从Adobe Commerce管理员或使用Fastly API创建和管理自定义VCL片段：

- [Adobe Commerce管理员](#manage-custom-vcl-from-admin) — 我们建议使用Adobe Commerce管理员管理自定义VCL片段，因为它会自动验证、上传和将VCL更改应用于Fastly服务配置的过程。 此外，您还可以从Admin查看和编辑添加到Fastly服务配置的自定义VCL代码片段。

- [Fastly API](#manage-vcl-using-the-api) — 如果您无法访问管理员，请使用Fastly API管理自定义VCL代码片段。 例如，使用API在站点关闭时对Fastly服务配置进行故障排除，或添加自定义VCL代码片段。 此外，某些操作只能使用API来完成。 例如，必须使用API来重新激活较旧的VCL版本，或查看指定VCL版本中包含的所有VCL片段。 请参阅 [VCL代码片段的API快速参考](#api-quick-reference-for-vcl-snippets).

### 示例VCL代码片段

以下示例显示了按客户端IP地址过滤流量的自定义VCL片段（JSON格式）：

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>在此示例中，VCL代码的格式为JSON有效负荷，该有效负荷可以保存到文件中并在Fastly API请求中提交。 要防止在作为API请求的JSON发送代码片段时出现JSON验证错误，请使用反斜杠对代码中的特殊字符进行转义。 请参阅 [使用动态VCL片段](https://docs.fastly.com/vcl/vcl-snippets/) 在Fastly VCL文档中。 如果您从Admin提交VCL代码片段，则不必转义特殊字符。

中的VCL逻辑 `content` 字段执行以下操作：

- 检查传入的IP地址， `client.ip` 在每个请求中

- 阻止中包含IP地址的任何请求 *ACLNAME* 边缘ACL，返回 `403 Forbidden` 错误

下表提供了有关自定义VCL代码片段的关键数据的详细信息。 有关更详细的参考，请参见 [VCL代码段](https://docs.fastly.com/api/config#api-section-snippet) 在Fastly文档中引用。

| 值 | 描述 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | 用于访问您的Fastly帐户的API密钥。 请参阅 [获取凭据](fastly-configuration.md). |
| `active` | 代码片段或版本的活动状态。 返回 `true` 或 `false`. 如果为true，则表示正在使用代码片段或版本。 使用活动片段的版本号对其进行克隆。 |
| `content` | 要运行的VCL代码段。 Fastly不支持所有VCL语言功能。 此外，Fastly为扩展提供了自定义功能。 有关受支持功能的详细信息，请参见 [Fastly VCL编程参考](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | 代码片段的动态状态。 返回 `false` 对象 [常规代码片段](https://docs.fastly.com/en/guides/about-vcl-snippets) 包含在Fastly服务配置的版本化VCL中。 返回 `true` 对于 [动态代码片段](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) 可以修改和部署，而不需要新的VCL版本。 |
| `number` | 包含代码片段的VCL版本号。 Fastly使用 *可编辑的版本#* 在它们的示例值中。 如果从API添加自定义代码片段，请在API请求中包含版本号。 如果从“管理员”添加自定义VCL，则会为您提供版本。 |
| `priority` | 数字值来自 `1` 到 `100` 指定自定义VCL代码片段的运行时间。 优先级值较低的代码片段首先运行。 如果未指定，则 `priority` 值默认为 `100`.<p>任何优先级值为的自定义VCL片段 `5` 列入允许列表立即运行，这最适合用于实施请求路由（块和以及重定向）的VCL代码。 优先级 `100` 最适合覆盖默认的VCL代码片段代码。<p>全部 [默认VCL代码段](fastly-configuration.md#upload-vcl-snippets) MagentoFastly模块中包含的 `priority=50`.<ul><li>分配高优先级，如 `100` 在所有其他VCL函数之后运行自定义VCL代码并覆盖缺省VCL代码。</li></ul> |
| `service_id` | 特定暂存或生产环境的Fastly服务ID。 当您的项目添加到云基础架构上的Adobe Commerce时，会分配此ID [Fastly服务帐户](fastly.md#fastly-service-account-and-credentials). |
| `type` | 指定用于插入所生成代码片段的位置，例如 `init` （上述子程序）及 `recv` （在子例程中）。 如需详细信息，请参阅快速版 [VCL代码段](https://docs.fastly.com/api/config#api-section-snippet) 引用。 |

## 从管理员管理自定义VCL

您可以 [添加自定义VCL代码片段](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) 从 *Fastly配置* > *自定义VCL代码片段* 部分。

![管理自定义VCL片段](../../assets/cdn/fastly-edit-snippets.png)

此 *自定义VCL代码片段* 视图仅显示通过管理员添加的代码片段。 如果使用Fastly API添加代码片段，请使用API来 [管理它们](#manage-vcl-using-the-api).

以下示例显示如何从管理员创建和管理自定义VCL代码段，以及如何使用Fastly Edge模块和Edge词典：

- [将请求重新路由到CMS后端](fastly-vcl-wordpress.md)
- [阻止反向链接垃圾邮件](fastly-vcl-badreferer.md)
- [阻止反向链接垃圾邮件](fastly-vcl-badreferer.md)
- [列入允许列表 IP的自定义VCL](fastly-vcl-allowlist.md)
- [列入阻止列表 IP的自定义VCL](fastly-vcl-blocking.md)
- [绕过Fastly缓存](fastly-vcl-bypass-to-origin.md)

## 使用API管理VCL

以下演练向您展示了如何使用Fastly API创建常规VCL代码片段文件并将其添加到您的Fastly服务配置中。 您可以从以下位置创建和管理代码片段 *终端* 应用程序。 您不需要将SSH连接连接到特定环境。

**先决条件：**

- 在云基础架构环境中为Fastly服务配置Adobe Commerce。 请参阅 [设置Fastly](fastly-configuration.md).

- [获取Fastly API凭据](fastly-configuration.md) 来验证对Fastly API的请求。 确保您获得正确环境的凭据：暂存或生产。

- 将Fastly服务凭据保存为bash环境变量，以便在cURL命令中使用：

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  导出的环境变量仅在当前bash会话中可用，并在关闭终端时丢失。 您可以通过导出新值来重新定义变量。 要查看与Fastly相关的导出变量列表，请执行以下操作：

  ```bash
  export | grep FASTLY
  ```

## 添加VCL代码片段

本教程提供了使用Fastly API添加自定义代码片段的基本步骤。

>[!NOTE]
>
>要了解如何从Adobe Commerce管理员管理自定义VCL代码片段，请参阅 [从Adobe Commerce管理员管理VCL](#manage-custom-vcl-from-admin).


**先决条件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### 步骤1：找到活动的VCL版本

使用Fastly API [获取版本](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) 获取活动VCL版本号的操作：

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

在JSON响应中，请注意 `number` 键，例如 `"number": 99`. 克隆VCL进行编辑时需要版本号。

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

将活动的版本号保存在bash环境变量中，以供在后续API请求中使用：

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### 步骤2：克隆活动VCL版本和所有片段

必须先创建活动VCL版本的副本进行编辑，然后才能添加或修改自定义VCL代码段。 使用Fastly API [克隆](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) 操作：

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

在JSON响应中，版本号是递增的，并且 *活动* 键值为 `false`. 可以在本地修改新的非活动VCL版本。

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

将新版本号保存在bash环境变量中，以供在后续命令中使用：

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### 步骤3：创建自定义VCL代码片段

在JSON文件中创建并保存自定义VCL代码，该代码具有以下内容和格式：

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

这些值包括：

- `name`-VCL代码片段的名称。

- `dynamic` — 指示这是否为 [常规代码片段](https://docs.fastly.com/en/guides/about-vcl-snippets) 或 [动态代码片段](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type` — 指定用于插入生成的代码片段的位置，例如 `init` （上述子程序）及 `recv` （在子例程中）。 请参阅 [Fastly VCL代码片段对象值](https://docs.fastly.com/api/config#snippet) 以了解有关这些值的信息。

- `priority` — 值来自 `1` 到 `100` 可确定自定义VCL代码片段的运行时间。 首先运行具有较低值的自定义VCL代码段。

  Fastly VCL模块的所有缺省VCL代码都有一个 `priority` 之 `50`. 如果希望某个操作最后发生或覆盖缺省VCL代码，请使用较高的数字，例如 `100`. 要立即运行自定义VCL代码片段，请将优先级设置为较低的值，例如 `5`.

- `content` — 要在一行中运行的VCL代码片段，不带换行符。 请参阅 [自定义VCL代码片段示例](#example-vcl-snippet-code).

### 步骤4：将VCL代码段添加到Fastly配置

使用Fastly API [创建代码片段](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) 操作将自定义VCL代码段添加到VCL版本。

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

此 `<filename.json>` 是您在上一步中准备的文件名。 对每个VCL代码段重复此命令。

如果您收到 `500 Internal Server Error` 来自Fastly服务的响应，请检查JSON文件语法以确保您上传的是有效文件。

### 步骤5：验证和激活自定义VCL代码片段

添加自定义VCL代码片段后，Fastly会将该代码片段插入到正在编辑的VCL版本中。 要应用更改，请完成以下步骤以验证VCL代码段并激活VCL版本。

1. 使用Fastly API [验证VCL版本](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) 操作以验证更新的VCL代码。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   如果Fastly API返回错误，请修复问题并再次验证更新后的VCL版本。

1. 使用Fastly API [激活](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) 操作以激活新的VCL版本。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## VCL代码片段的API快速参考

这些API请求示例使用导出的环境变量提供凭据来通过Fastly进行身份验证。 有关这些命令的详细信息，请参见 [Fastly API引用](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>使用这些命令来管理您使用Fastly API添加的片段。 如果您从管理员中添加了代码片段，请参阅 [使用Admin管理VCL片段](#manage-vcl-using-the-api).

- **获取活动的VCL版本号**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **列出附加到服务的所有常规VCL片段**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **查看单个代码片段**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  此 `<snippet_name>` 是片段的名称，如 `my_regular_snippet`.

- **更新代码片段**

  修改 [已准备的JSON文件](#step-3-create-a-custom-vcl-snippet) 并发送以下请求：

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **删除单个VCL片段**

  获取代码片段列表并使用以下代码 `curl` 具有要删除的特定代码片段名称的命令：

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **覆盖中的值 [默认Fastly VCL代码](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  创建具有更新值的代码片段并分配优先级 `100`.
