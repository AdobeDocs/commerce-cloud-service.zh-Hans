---
source-git-commit: 8f1ed3067f6daed897151052c8b9f987d3df3a50
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# 用于删除Fastly的自定义VCL的Include文件

## 删除自定义VCL代码片段

1. [登录](/help/get-started/onboarding.md#access-your-admin-panel) 发送给管理员。

1. 单击 **商店** > **设置** > **配置** > **高级** > **系统**.

1. 展开 **全页缓存** > **Fastly配置** > **自定义VCL代码片段**.

   ![管理自定义VCL片段](/help/assets/cdn/fastly-manage-snippets.png)

1. 在 _操作_ 列中，单击要删除的代码片段旁边的垃圾桶图标。

1. 在下一个模式窗口中，单击 **DELETE** 并激活新版本。

>[!WARNING]
>
>此 _自定义VCL代码片段_ 用户界面选项仅显示通过Adobe Commerce管理员添加的代码片段。 如果您使用Fastly API添加代码片段，请使用该API来 [管理它们](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
