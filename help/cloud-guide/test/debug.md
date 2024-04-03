---
title: 配置 [!DNL Xdebug]
description: 了解如何配置Xdebug扩展以便在云基础架构项目开发中调试Adobe Commerce。
exl-id: bf2d32d8-fab7-439e-8df3-b039e53009d4
source-git-commit: 751456f50e7b017b47c2ff43e008c2d04a558d96
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 0%

---

# 配置Xdebug

[!DNL Xdebug] 是用于调试PHP的扩展。 尽管可以使用您选择的IDE，但以下内容将介绍如何配置 [!DNL Xdebug] 和 [!DNL PhpStorm] 以在本地环境中调试。

>[!NOTE]
>
>您可以配置 [!DNL Xdebug] 在Cloud Docker环境中运行以进行本地调试，而无需更改云基础架构项目配置上的Adobe Commerce。 请参阅 [为Docker配置Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

要启用 [!DNL Xdebug]，您必须在Git存储库中配置文件，配置IDE，并设置端口转发。 您可以在中配置一些设置 `magento.app.yaml` 文件。 编辑后，跨所有入门环境和Pro集成环境推送Git更改以启用 [!DNL Xdebug]. [!DNL Xdebug] 在专业暂存和生产环境中已经可用。

配置完毕后，即可调试CLI命令、Web请求和代码。 请记住，所有云基础架构环境都是只读的。 将代码克隆到本地开发环境以执行调试。 有关专业暂存和生产环境，请参阅 [其他说明](#debug-for-pro-staging-and-production) 对象 [!DNL Xdebug].

## 要求

运行和使用 [!DNL Xdebug]，您需要环境的SSH URL。 您可以通过 [[!DNL Cloud Console]](../project/overview.md) 或您的 [!DNL Cloud Onboarding UI].

## 配置Xdebug

配置 [!DNL Xdebug]，请按照以下步骤操作：

- [在分支中工作以推送文件更新](#get-started-with-a-branch)
- [启用 [!DNL Xdebug] 适用于环境](#enable-xdebug-in-your-environment)
- [配置IDE](#configure-phpstorm)
- [设置端口转发](#set-up-port-forwarding)

### 分支入门

添加 [!DNL Xdebug]，Adobe建议使用 [开发分支](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### 在您的环境中启用Xdebug

您可以启用 [!DNL Xdebug] 直接连接到所有入门级环境和专业级集成环境。 专业生产和暂存环境不需要此配置步骤。 请参阅 [调试Pro暂存和生产环境](#debug-for-pro-staging-and-production).

要启用 [!DNL Xdebug] 对于您的项目，添加 `xdebug` 到 `runtime:extensions` 的部分 `.magento.app.yaml` 文件。

**启用Xdebug**：

1. 在本地终端中，打开 `.magento.app.yaml` 文件中的文本编辑器。

1. 在 `runtime` 部分，在 `extensions`，添加 `xdebug`. 例如：

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. 将更改保存到 `.magento.app.yaml` 并退出文本编辑器。

1. 添加、提交和推送更改以重新部署环境。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

部署到入门级环境和专业集成环境时， [!DNL Xdebug] 现已推出。 继续配置IDE。 有关PhpStorm的信息，请参见 [配置PhpStorm](#configure-phpstorm).

### 配置PhpStorm

此 [PhpStorm](https://www.jetbrains.com/phpstorm/) 必须配置IDE才能正确使用 [!DNL Xdebug].

**配置PhpStorm以使用Xdebug**：

1. 在PhpStorm项目中，打开 **设置** 面板。

   - _macOS_ — 选择 **PhpStorm** > **偏好设置**.
   - _Windows/Linux_ — 选择 **文件** > **设置**.

1. 在 _设置_ 面板，展开并找到 **语言和框架** > **PHP** > **服务器** 部分。

1. 单击 **+** 以添加服务器配置。 项目名称在顶部为灰色。

1. [可选] 为新服务器配置配置以下设置。 请参阅 [未配置调试服务器](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) 在 _PHPStorm_ 文档。

   - **名称** — 输入与主机名相同的。 此值必须匹配 `PHP_IDE_CONFIG` 中的变量 [调试CLI命令](#debug-cli-commands) 使用CLI进行调试。
   - **主机** — 输入主机名。
   - **端口** — 输入 `443`.
   - **调试程序** — 选择 `Xdebug`.

1. 选择 **使用路径映射**. 在 _文件/目录_ 窗格，的项目的根 `serverName` 显示。

1. 在 **服务器上的绝对路径** 列中，单击 **编辑** 图标，并根据环境添加设置。

   - 对于所有入门级环境和专业级集成环境，远程路径是 `/app`.
   - 对于Pro暂存和生产环境：

      - 生产： `/app/<project_code>/`
      - 暂存：  `/app/<project_code>_stg/`

1. 更改 [!DNL Xdebug] 端口到9000 **语言和框架** > **PHP** > **调试** > **Xdebug** > **调试端口** 面板。

1. 单击 **应用**.

### 设置端口转发

映射 `XDEBUG` 从服务器到本地系统的连接。 要执行任何类型的调试，必须将端口9000从云基础架构服务器上的Adobe Commerce转发到本地计算机。 请参阅以下部分之一：

- [Mac或UNIX上的端口转发](#port-forwarding-on-mac-or-unix)
- [Windows上的端口转发](#port-forwarding-on-windows)

#### Mac或UNIX上的端口转发®

**在Mac或UNIX®环境中设置端口转发**：

1. 打开终端。

1. 使用SSH建立连接。

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   使用 `-v` (verbose)选项，以便每当套接字连接到要转发的端口时，它都会显示在终端中。

   如果显示“无法连接”或“无法侦听远程端口”错误，则可能是另一个活动的SSH会话持续存在于占用端口9000的服务器上。 如果未使用该连接，则可以终止该连接。

**对连接进行故障排除**：

1. 使用SSH登录到远程集成、暂存或生产环境。

1. 查看SSH会话列表： `who`

1. 按用户查看现有SSH会话。 注意不要影响除您自己以外的用户！

   - 集成：用户名与 `dd2q5ct7mhgus`
   - 暂存：用户名类似于 `dd2q5ct7mhgus_stg`
   - 生产：用户名类似于 `dd2q5ct7mhgus`

1. 对于比您更早的用户会话，请查找伪终端(PTS)值，例如 `pts/0`.

1. 终止与PTS值对应的进程ID (PID)。

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   示例响应：

   ```terminal
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   要终止连接，请输入带有进程ID (PID)的kill命令。

   ```bash
   kill 3664
   ```

#### Windows上的端口转发

要在Windows上设置端口转发（SSH隧道），必须配置Windows终端应用程序。 此示例逐步说明如何使用创建SSH隧道 [腻子](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). 您可以使用Cygwin等其他应用程序。 有关其他应用程序的更多信息，请参阅随这些应用程序提供的供应商文档。

**使用Putty在Windows上设置SSH通道**：

1. 如果您尚未这样做，请下载 [腻子](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. 启动Putty。

1. 在类别窗格中，单击 **会话**.

1. 输入以下信息：

   - **主机名（或IP地址）** 字段：输入 [SSH URL](../development/secure-connections.md#connect-to-a-remote-environment) 适用于您的云服务器
   - **端口** 字段：输入 `22`

   ![设置Putty](../../assets/xdebug/putty-session.png)

1. 在 _类别_ 窗格，单击 **连接** > **SSH** > **隧道**.

1. 输入以下信息：

   - **源端口** 字段：输入 `9000`
   - **目标** 字段：输入 `127.0.0.1:9000`
   - 单击 **远程**

1. 单击 **添加**.

   ![在Putty中创建SSH通道](../../assets/xdebug/putty-tunnels.png)

1. 在 _类别_ 窗格，单击 **会话**.

1. 在 **保存的会话** 字段中，输入此SSH通道的名称。

1. 单击 **保存**.

   ![保存SSH通道](../../assets/xdebug/putty-session-save.png)

1. 要测试SSH通道，请单击 **加载**，然后单击 **打开**.

   如果显示“无法连接”错误，请验证以下各项：

   - 所有Putty设置均正确
   - 您在云基础架构上的专用Adobe Commerce SSH密钥所在的计算机上运行Putty

## 通过SSH访问Xdebug环境

要启动调试、执行设置等，您需要SSH命令来访问环境。 您可以通过以下方式获取此信息 [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) 以及您的项目电子表格。

对于入门环境和专业集成环境，您可以使用以下功能 `magento-cloud` 通过CLI命令SSH进入这些环境：

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

使用 [!DNL Xdebug]，通过SSH连接到环境，如下所示：

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

例如，

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## 调试Pro暂存和生产环境

>[!NOTE]
>
>在专业暂存和生产环境中， [!DNL Xdebug] 始终可用，因为这些环境具有特殊的 [!DNL Xdebug]. 所有普通的Web请求都被路由到不具备的专用PHP进程 [!DNL Xdebug]. 因此，这些请求会正常处理，并且在以下情况下不会出现性能降级 [!DNL Xdebug] 已加载。 发送的Web请求具有 [!DNL Xdebug] 键，它将被路由到单独的PHP进程，该进程 [!DNL Xdebug] 已加载。

使用 [!DNL Xdebug] 特别是在Pro计划暂存和生产环境中，您可以创建单独的SSH通道和Web会话，但只有您才有权访问。 此用法不同于典型访问，它仅向您提供访问权限，并不向所有用户提供访问权限。

您需要以下各项：

- SSH命令用于访问环境。 您可以通过以下方式获取此信息 [[!DNL Cloud Console]](../project/overview.md) 或您的 [!DNL Cloud Onboarding UI].
- 此 `xdebug_key` 在配置Staging和Pro环境时设置的值。

  此 `xdebug_key` 可以通过使用SSH登录到主节点并执行：

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**设置到暂存或生产环境的SSH通道**：

1. 打开终端。

1. 清理集群中每个Web节点的所有SSH会话。

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. 为群集的每个Web节点设置Xdebug的SSH通道。

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

**使用环境URL开始调试**：

1. 启用远程调试；访问浏览器中的站点并将以下内容附加到URL中， `KEY` 为的值 `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   此步骤设置发送浏览器请求以触发的Cookie [!DNL Xdebug].

1. 使用完成调试 [!DNL Xdebug].

1. 准备好结束会话后，使用以下命令删除Cookie并结束通过浏览器进行的调试，其中 `KEY` 为的值 `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >此 `XDEBUG_SESSION_START` 传递者 `POST` 不支持请求。

## 调试CLI命令

本节介绍如何调试CLI命令。

要调试CLI命令：

1. 通过SSH连接到要使用CLI命令调试的服务器。

1. 创建以下环境变量：

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   这些变量将在SSH会话结束时删除。

1. 开始调试

   在入门环境和Pro集成环境中，运行CLI命令进行调试。
您可以添加运行时选项，例如：

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   在Pro暂存和生产环境中，您必须指定 [!DNL Xdebug] 调试CLI命令时的PHP配置文件，例如：

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## 调试Web请求

以下步骤可帮助您调试Web请求。

1. 在 _扩展名_ 菜单，单击 **调试** 以启用。

1. 右键单击，选择选项菜单，然后将IDE键设置为 **PHPSTORM**.

1. 安装 [!DNL Xdebug] 浏览器上的客户端。 配置并启用它。

### 示例：Chrome设置

本节讨论如何使用 [!DNL Xdebug] 在Chrome中，使用 [!DNL Xdebug] 帮助程序扩展。 有关信息 [!DNL Xdebug] 其他浏览器的工具，请查阅浏览器文档。

**将Xdebug帮助程序与Chrome结合使用**：

1. 创建 [SSH隧道](#ssh-access-to-xdebug-environments) 到云服务器。

1. 安装 [Xdebug帮助程序扩展](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) 从Chrome商店买的。

1. 在Chrome中启用该扩展，如下图所示。

   ![在Chrome中启用Xdebug扩展](../../assets/xdebug/enable-chrome-ext.png)

1. 在Chrome中，右键单击Chrome工具栏中的绿色帮助程序图标。

1. 在弹出菜单中，单击 **选项**.

1. 从 _IDE密钥_ 列表，单击 **PhpStorm**.

1. 单击 **保存**.

   ![Xdebug帮助程序选项](../../assets/xdebug/helper-options.png)

1. 打开PhpStorm项目。

1. 在顶部导航栏中，单击 **开始侦听** 图标。

   如果未显示导航栏，请单击 **视图** > **导航栏**.

1. 在PhpStorm导航窗格中，双击PHP文件进行测试。

## 调试本地代码

由于环境是只读的，因此您必须从环境或特定Git分支将代码拉到本地工作站才能执行调试。

您选择的方法由您决定。 您可以选择以下选项：

- 从Git中签出代码并运行 `composer install`

  除非出现以下情况，否则此方法有效 `composer.json` 引用您无权访问的专用存储库中的包。 此方法可获取整个Adobe Commerce代码库。

- 复制 `vendor`， `app`， `pub`， `lib`、和 `setup` 目录

  此方法可使您拥有所有可以测试的代码。 根据您拥有的静态资产数量，这可能会导致传输时间较长，并包含大量文件。

- 复制 `vendor` 仅目录

  因为大多数代码都位于 `vendor` 目录，此方法可能会产生良好的测试，但不会测试整个代码库。

**压缩文件并将其复制到本地计算机**：

1. 使用SSH登录到远程环境。

1. 压缩文件。

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   例如，压缩 `vendor` 仅目录：

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. 在本地环境中，使用PhpStorm压缩文件。

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
