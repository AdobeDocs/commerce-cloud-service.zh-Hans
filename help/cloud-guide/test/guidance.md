---
title: 测试指南
description: 了解在云基础架构上启动Adobe Commerce的测试类型和最佳实践。
exl-id: 1d7897a2-af97-46ab-89c0-2ec54284190e
source-git-commit: aae285d85188bfadd73d5b4e1fea16afa5beb117
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# 测试指南

在云基础架构项目上配置和自定义Adobe Commerce后，最佳实践是在启动商店网站之前彻底测试您的应用程序。 测试有助于正确管理对群集规模的期望，并根据未来的业务需求进行适当扩展。

## 功能测试

在开发过程中，请务必对云基础架构项目上的Adobe Commerce执行端到端功能测试。 有关在Docker环境中执行功能测试，请参阅以下指南：

- **应用程序测试** — 使用 [Magento功能测试框架(MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) 用于在Cloud Docker环境中测试应用程序。

- **代码测试** — 使用 [PHP的代码欺骗测试框架](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) 用于验证旨在贡献到Cloud包存储库的代码。

## 启动前的最佳实践

将以下测试类型视为在站点启动之前执行的最佳实践：

- **负载测试** — 进行负载测试，以了解系统在预期负载下的行为。 例如，测试应用程序上并发的活动用户数，让每个用户在设置的持续时间内执行特定数量的事务。 此测试揭示重要业务关键事务（如数据库或应用程序服务器行为）的响应时间。 负载测试有助于确定瓶颈。

- **压力测试** — 挑战系统内的容量上限，以确定在当前负载大大超过预期最大值时，系统是否能够充分执行。

- **安全扫描**—Adobe提供免费的 [安全扫描工具](../launch/overview.md#set-up-the-security-scan-tool) 用于您的网站。

- **渗透测试** — 是对计算机系统进行的授权模拟网络攻击，旨在评估系统的安全性。 渗透测试有助于识别弱点或漏洞，包括未经授权的各方获取系统功能和数据的可能性。

>[!WARNING]
>
>客户不得对AWS基础设施或AWS服务本身进行任何安全评估。 如果您在安全评估中发现的任何AWS服务中发现了安全问题， [联系AWS安全部门](mailto:aws-security@amazon.com) 立即。 请参阅 [用于渗透测试的AWS客户政策](https://aws.amazon.com/security/penetration-testing/).
