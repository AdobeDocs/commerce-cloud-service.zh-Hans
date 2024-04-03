---
title: SendGrid电子邮件服务
description: 了解云基础架构上适用于Adobe Commerce的SendGrid电子邮件服务，以及如何测试您的DNS配置。
exl-id: 30d3c780-603d-4cde-ab65-44f73c04f34d
source-git-commit: 7c22dc3b0e736043a3e176d2b7ae6c9dcbbf1eb5
workflow-type: tm+mt
source-wordcount: '1019'
ht-degree: 0%

---

# SendGrid电子邮件服务

SendGrid简单邮件传输协议(SMTP)代理服务提供出站电子邮件身份验证和信誉监视服务，包括支持：

* 所有出站事务性电子邮件
* 专用IP地址
* 域注册、用于电子邮件域验证的域密钥识别邮件(DKIM)签名（仅适用于专业暂存和生产环境）
* 自定义域注册（仅适用于Pro）
* 简易和专业集成环境的自动化集成。 专业生产和暂存环境需要在基础架构即服务(IaaS)硬件配置过程中进行手动配置和配置

SendGrid SMTP代理不能用作接收传入电子邮件的通用电子邮件服务器，也不能用于电子邮件营销活动。

>[!TIP]
>
>您可以在以下位置找到帐户的SendGrid详细信息： [载入UI](https://cloud.magento.com) 并选择 **项目详细信息** > **托管信息** 选项卡。

## 启用或禁用电子邮件

默认情况下，在Pro生产和暂存环境中启用传出电子邮件。 此 [!UICONTROL Outgoing emails] 在设置之前，无论状态如何，都会显示在环境设置中 `enable_smtp` 属性。 您可以为其他环境启用传出电子邮件，以向Cloud项目用户发送双重身份验证电子邮件。 请参阅 [配置电子邮件以进行测试](outgoing-emails.md).

## SendGrid仪表板

所有云项目都在中央帐户下管理，因此仅支持人员有权访问SendGrid功能板。 SendGrid不提供子帐户限制功能。

要查看“活动”日志中的投放状态或退回、拒绝或阻止的电子邮件地址列表， [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). 支持团队 **无法** 检索超过30天的活动日志。

如有可能，请在请求中包含以下信息：

* 受影响的电子邮件地址
* 相关时间范围（仅限过去30天内）
* 电子邮件的主题

## 域密钥识别邮件(DKIM)

DKIM是一种电子邮件身份验证技术，它使Internet服务提供商(ISP)能够识别合法和虚假发件人地址，这种技术通常用于网络钓鱼和电子邮件诈骗。 DKIM依赖于管理DNS记录的域所有者。 使用DKIM时，发件人服务器使用私钥对邮件进行签名。 此外，域所有者会添加一个DKIM记录，该记录已被修改 `TXT` 记录，到发件人域的DNS记录。 此 `TXT` 记录包含公钥，收件人邮件服务器使用它来验证邮件的签名。 DKIM公钥加密过程使收件人能够验证发送者的真实性。 请参阅 [解释DKIM记录](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>SendGrid DKIM签名和域身份验证支持仅适用于Pro项目，而不适用于Starter。 因此，出站事务型电子邮件可能会被垃圾邮件过滤器标记。 使用DKIM可以提高经过身份验证的电子邮件发件人的投放率。 为了提高邮件投放率，您可以从Starter升级到Pro，或者使用您自己的SMTP服务器或电子邮件投放服务提供商。 请参阅 [配置电子邮件连接](https://experienceleague.adobe.com/docs/commerce-admin/systems/communications/email-communications.html) 在 _管理系统指南_.

### 发件人和域身份验证

要让SendGrid代表您从专业生产或暂存环境发送事务性电子邮件，您必须将DNS设置配置为包含三个SendGrid子域DNS条目。 每个SendGrid帐户都分配有一个唯一的 `TXT` 用于验证出站电子邮件的记录。

**启用域验证**：

1. 提交 [支持服务单](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 请求为特定域启用DKIM (**仅限专业暂存和生产环境**)。
1. 使用更新您的DNS配置 `TXT` 和 `CNAME` 支持票证中提供给您的记录。

**示例 `TXT` 具有帐户ID的记录**：

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**示例 `CNAME` 记录**：

| 域 | 指向 | 记录类型 |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2。_domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM签名和自动安全性

设置经过身份验证的域时，您可以在自动和手动安全之间进行选择。 如果您选择自动安全，SendGrid会自动管理您的DKIM和SPF记录。 当您将新的专用发送IP地址添加到帐户时，SendGrid会立即更新您的DNS设置和DKIM签名。 如果关闭自动安全功能，则只要更改发送域，您就应负责更新DKIM签名。

**启用了自动化安全性的示例**：

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**禁用自动安全示例**：

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

设置域身份验证后，SendGrid会自动为您处理安全策略框架(SPF)和DKIM记录。 在SendGrid提供 `CNAME` 要添加到DNS记录的记录，您可以添加专用IP地址并进行其他帐户更新，而无需手动管理SPF记录。 请参阅 [自动安全性和您的DKIM签名](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

要测试DNS配置，请执行以下操作：

```terminal
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## 事务性电子邮件阈值

事务性电子邮件阈值是指在特定时间段内可从Pro环境发送的事务性电子邮件消息数，例如每月从非生产环境发送12,000封电子邮件。 阈值旨在防止发送垃圾邮件并防止可能对您的电子邮件信誉造成损害。

只要发件人信誉得分超过95%，就可以在“生产”环境中发送的电子邮件数量就没有任何硬性限制。 信誉受退回或拒绝的电子邮件数量以及基于DNS的垃圾邮件注册是否已将您的域标记为潜在垃圾邮件来源的影响。 请参阅 [在Adobe Commerce上超过SendGrid信用时未发送电子邮件](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded.html) 在 _Commerce支持知识库_.

**检查是否已超过最大积分**：

1. 在本地工作站上，转到您的项目目录。

1. 使用SSH登录到远程环境。

   ```bash
   magento-cloud ssh
   ```

1. 查看 `/var/log/mail.log` 对象 `authentication failed : Maxium credits exceeded` 个条目。

   如果您看到任何 `authentication failed` 日志条目和 **电子邮件发送信誉** 至少95个，你可以 [提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 请求增加信用额度。

### 电子邮件发送信誉

电子邮件发送信誉是由Internet服务提供商(ISP)为发送电子邮件的公司分配的分数。 分数越高，ISP将消息发送到收件人收件箱的可能性就越大。 如果分数低于某个级别，ISP可能会将邮件路由到收件人的垃圾邮件文件夹，甚至完全拒绝邮件。 信誉得分由多个因素决定，例如IP地址相对于其他IP地址的30天平均排名以及垃圾邮件投诉率。 请参阅 [检查发送信誉的5种方法](https://sendgrid.com/blog/5-ways-check-sending-reputation/).
