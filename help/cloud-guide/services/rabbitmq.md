---
title: 设置RabbitMQ服务
description: 了解如何启用RabbitMQ服务以管理云基础架构上Adobe Commerce的消息队列。
feature: Cloud, Services
exl-id: 85794b8f-2260-4a6e-b5a6-a1b4c356594e
source-git-commit: d4c36b084094846cfad69adc2bffd567a58fab26
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 设置 [!DNL RabbitMQ] 服务

此 [消息队列框架(MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) 是Adobe Commerce中的一种系统，它允许 [模块](https://glossary.magento.com/module) 将消息发布到队列。 它还定义了异步接收消息的消费者。

MQF使用 [RabbitMQ](https://www.rabbitmq.com/) 作为报文传送代理，它提供用于发送和接收报文的可伸缩平台。 它还包括用于存储未传递消息的机制。 [!DNL RabbitMQ] 基于高级消息队列协议(AMQP) 0.9.1规范。

>[!WARNING]
>
>如果您希望使用现有的基于AMQP的服务，例如 [!DNL RabbitMQ]，而不是依靠Adobe Commerce基础架构来为您创建，请使用 [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) 环境变量，以将其连接到您的站点。

{{service-instruction}}

**启用RabbitMQ**：

1. 将所需的名称、类型和磁盘值（以MB为单位）添加到 `.magento/services.yaml` 文件以及已安装的RabbitMQ版本。

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. 在中配置关系 `.magento.app.yaml` 文件。

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. 添加、提交和推送代码更改。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [验证服务关系](services-yaml.md#service-relationships).

{{service-change-tip}}

## 连接到RabbitMQ以进行调试

出于调试目的，通过下列方式之一直接连接到服务实例会很有用：

- 从本地开发环境连接
- 从应用程序连接
- 从PHP应用程序连接

### 从本地开发环境连接

1. 登录到 `magento-cloud` cli和项目：

   ```bash
   magento-cloud login
   ```

1. 查看已安装并配置RabbitMQ的环境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 使用SSH连接到云环境：

   ```bash
   magento-cloud ssh
   ```

1. 从检索RabbitMQ连接详细信息和登录凭据 [$MAGENTO_云关系](../application/properties.md#relationships) 变量：

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在响应中，查找RabbitMQ信息，例如：

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. 启用到RabbitMQ的本地端口转发。

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   访问RabbitMQ管理Web界面的示例，位于 `http://localhost:15672` 为：

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. 会话打开时，您可以从本地工作站启动所选的RabbitMQ客户端，并将其配置为连接到 `localhost:<portnumber>` 使用MAGENTO_CLOUD_RELATIONSHIPS变量中的端口号、用户名和密码信息。

### 从应用程序连接

要连接到应用程序中运行的RabbitMQ，请安装客户端，例如 [amqp-utils](https://github.com/dougbarth/amqp-utils)，作为您项目中的依赖项 `.magento.app.yaml` 文件。

例如，

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

登录PHP容器时，输入任意 `amqp-` 命令可用于管理队列。

### 从PHP应用程序连接

要使用PHP应用程序连接到RabbitMQ，请添加PHP [库](https://glossary.magento.com/library) 到源树。
