---
title: 阻止反向链接垃圾邮件
description: 使用Fastly Edge词典和自定义VCL代码片段阻止来自您站点的反向链接垃圾邮件。
feature: Cloud, Configuration, Security
exl-id: 665bac93-75db-424f-be2c-531830d0e59a
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# 阻止反向链接垃圾邮件

以下示例显示如何配置 [Fastly Edge词典](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) 一个自定义VCL代码片段，用于阻止云基础架构网站上来自Adobe Commerce的反向垃圾邮件。

>[!NOTE]
>
>我们建议将自定义VCL配置添加到暂存环境中，您可以在暂存环境中测试这些配置，然后再针对生产环境运行它们。

**先决条件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 审查网站日志中的虚假引用URL，并列出要阻止的域。

## 创建反向链接阻止列表

Edge词典用于创建在VCL代码段处理期间可供VCL函数访问的键值对。 在本例中，您会创建一个Edge词典，其中提供要阻止的反向链接网站列表。

{{admin-login-step}}

1. 单击 **商店** > **设置** > **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **Edge词典**.

1. 创建字典容器：

   - 单击 **添加容器**.

   - 在 *容器* 页面，输入 **字典名称**—`referrer_blocklist`.

   - 选择 **更改后激活** 以部署您对正在编辑的Fastly服务配置版本所做的更改。

   - 单击 **上传** 将字典附加到您的Fastly服务配置。

1. 将要阻止的域名列表添加到 `referrer_blocklist` 词典：

   - 单击的“设置”图标 `referrer_blocklist` 词典。

   - 在新词典中添加和保存键值对。 在本例中，每个 **键** 是要阻止的反向链接URL的域名，并且 **值** 是 `true`.

     ![添加错误的反向链接词典项目](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - 单击 **取消** 以返回系统配置页面。

1. 单击 **保存配置**.

1. 根据页面顶部的通知刷新缓存。

有关Edge词典的更多信息，请参阅 [创建和使用边缘词典](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) 和 [自定义VCL代码片段](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) 在Fastly文档中。

## 创建自定义VCL代码片段以阻止反向链接垃圾邮件

以下自定义VCL代码片段代码（JSON格式）显示了用于检查和阻止请求的逻辑。 VCL代码段将反向链接网站的主机捕获到标头中，然后将主机名与 `referrer_blocklist` 词典。 如果主机名匹配，则会使用 `403 Forbidden` 错误。

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "set req.http.Referer-Host = regsub(req.http.Referer, \"^https?:\/\/?([^:\/s]+).*$\", \"\\1\"); if (table.lookup(referrer_blocklist, req.http.Referer-Host)) { error 403 \"Forbidden\"; }"
}
```

在基于此示例创建代码片段之前，请查看值以确定是否需要进行任何更改：

- `name` — VCL代码片段的名称。 在本例中，我们使用 `block_bad_referrer`.

- `dynamic`  — 值0表示 [常规代码片段](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) 上传到Fastly配置的版本化VCL。

- `priority`  — 确定VCL代码片段的运行时间。 优先级为 `5` 在任何默认MagentoVCL代码片段之前运行此代码片段(`magentomodule_*`)分配了优先级50。 根据您希望代码片段运行的时间，将每个自定义代码片段的优先级设置为高于或低于50。 优先级较低的代码片段首先运行。

- `type`  — 指定在VCL版本中插入代码片段的位置。 在此示例中，VCL代码片段是 `recv` 代码片段。 将代码片段插入VCL版本后，该代码片段将添加到 `vcl_recv` 副例程，位于缺省Fastly VCL代码下方，以及任何对象上方。

- `content` — VCL代码片段以一行运行，不带换行符。

查看并更新环境的代码后，使用以下任一方法将自定义VCL代码段添加到Fastly服务配置中：

- [添加来自管理员的自定义VCL代码片段](#add-the-custom-vcl-snippet). 如果您可以访问管理员，则建议使用此方法。 (需要 [Fastly版本1.2.58](fastly-configuration.md#upgrade) 或更高版本。)

- 将JSON代码示例保存到文件(例如， `allowlist.json`)和 [使用Fastly API上传](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). 如果您无法访问管理员，请使用此方法。

## 添加自定义VCL代码片段

{{admin-login-step}}

1. 单击 **商店** >设置> **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **自定义VCL代码片段**.

1. 单击 **创建自定义代码片段**.

1. 添加VCL代码片段值：

   - **名称** — `block_bad_referrer`

   - **类型** — `recv`

   - **优先级** — `5`

   - **VCL** 代码片段内容 — 

     ```conf
     set req.http.Referer-Host = regsub(req.http.Referer,
     "^https?://?([^:/\s]+).*$", "1");
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. 单击 **创建**.

   ![创建自定义反向链接块VCL代码片段](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. 重新加载页面后，单击 **将VCL上传到Fastly** 在 *Fastly配置* 部分。

1. 上载完成后，根据页面顶部的通知刷新缓存。

Fastly在上传过程中验证更新的VCL版本。 如果验证失败，请编辑自定义VCL代码片段以修复所有问题。 然后，再次上传VCL。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
