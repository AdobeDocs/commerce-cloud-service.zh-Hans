---
source-git-commit: 2d902a3926c6bbc6a9dc8afcbd667eddeaf3be7e
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# 包含用于修改Fastly的自定义VCL的文件

## 修改自定义VCL代码片段

1. [登录](/help/get-started/onboarding.md#access-your-admin-panel) 发送给管理员。

1. 单击 **商店** > **设置** > **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **自定义VCL代码片段**.

   ![管理自定义VCL片段](/help/assets/cdn/fastly-manage-snippets.png)

1. 在 _操作_ 列中，单击要编辑的代码片段旁边的设置图标。

1. 重新加载页面后，单击 **将VCL上传到Fastly** 在 _Fastly配置_ 部分。

1. 上载完成后，根据页面顶部的通知刷新缓存。

>[!WARNING]
>
>此 _自定义VCL代码片段_ 用户界面选项仅显示通过Adobe Commerce管理员添加的代码片段。 如果您使用Fastly API添加代码片段，请使用该API来 [管理它们](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
