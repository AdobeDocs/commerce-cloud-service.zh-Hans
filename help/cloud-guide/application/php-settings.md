---
title: PHP设置
description: 了解在云基础架构中用于Commerce应用程序配置的最佳PHP设置。
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# PHP设置

您可以选择哪个 [PHP的版本](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) 以在 `.magento.app.yaml` 文件：

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>如果升级到PHP 8.1及更高版本，请从删除JSON [`runtime: extensions:` 属性](properties.md#runtime) 在 `.magento.app.yaml` 文件并重新部署。 JSON扩展自PHP 8.0起即安装在云环境中。

## 配置PHP

可以使用自定义环境的PHP设置 `php.ini` 附加到由Adobe Commerce维护的配置的文件。

在您的存储库中，添加 `php.ini` 文件到应用程序的根目录（存储库根目录）。

>[!TIP]
>
>不正确配置PHP设置可能会导致问题，因此只有高级管理员才应该设置这些选项。

### 增加PHP内存限制

要增加PHP内存限制，请将以下设置添加到 `php.ini` 文件：

```ini
memory_limit = 1G
```

对于调试，请将该值增加到2G。

### 优化real路径缓存配置

设置以下内容 `realpath_cache` 用于提高应用程序性能的设置。

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

这些设置允许PHP进程缓存文件的路径，而不是在每次加载页时查找这些路径。 请参阅 [性能调整](https://www.php.net/manual/en/ini.core.php) 在PHP文档中。

>[!NOTE]
>
>有关推荐的PHP配置设置的列表，请参见 [必需的PHP设置](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) 在 _安装指南_.

### 检查自定义PHP设置

推送 `php.ini` 更改云环境后，您可以检查自定义PHP配置是否已添加到环境中。 例如，使用SSH登录到远程环境，并使用类似于以下内容的内容查看文件：

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>如果您将Cloud Docker for Commerce用于本地开发，请参阅 [Docker服务容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) 有关使用自定义设置的信息 `php.ini` 文件。

## 启用扩展

您可以在中启用或禁用PHP扩展 `runtime:extension` 部分。 此外，指定的扩展在Docker PHP容器中可用。

>[!IMPORTANT]
>
>在启用扩展之前，请务必了解PHP版本必须与托管项目的操作系统兼容。 您的项目环境可能需要基础架构团队升级操作系统，然后才能继续。

中的示例 `.magento.app.yaml` 文件：

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

使用SSH登录到环境并列出PHP扩展。

```bash
php -m
```

有关特定PHP扩展的详细信息，请参见 [PHP扩展列表](https://www.php.net/manual/en/extensions.alphabetical.php).

下表显示了在Cloud平台上部署Adobe Commerce时支持的PHP扩展。

| 默认扩展 | 已安装的扩展<br>无法卸载的 | 可安装的扩展<br>并根据需要卸载 |
| ------------------ | --------------------- | --------------------- |
| `bcmath`<br>`bz2`<br>`calendar`<br>`exif`<br>`gd`<br>`gettext`<br> `intl`<br> `mysqli`<br> `openswoole`<br> `pcntl`<br> `pdo_mysql`<br> `soap`<br> `sockets`<br>  `sysvmsg`<br> `sysvsem`<br> `sysvshm`<br> `opcache`<br> `zip` | `ctype`<br> `curl`<br>`date`<br> `dom`<br> `fileinfo`<br> `filter`<br> `ftp`<br> `hash`<br> `iconv`<br> `json`<br> `mbstring`<br> `mysqlnd`<br> `openssl`<br> `pcre`<br> `pdo`<br> `pdo_sqlite`<br> `phar`<br>`posix`<br> `readline`<br> `session`<br> `sqlite3`<br> `tokenizer`<br> `xml`<br> `xmlreader`<br> `xmlwriter`<br> | `geoip`<br>`gmp`<br> `igbinary`<br> `imagick`<br> `imap`<br> `ioncube` <br>`ldap`<br> `mailparse`<br> `mcrypt`<br> `msgpack`<br> `mysqli`<br> `oauth`<br> `pdo_mysql`<br> `propro`<br> `pspell`<br> `raphf`<br> `recode`<br> `redis`<br> `shmop` `sockets`<br> `sodium`<br> `ssh2`<br>`tidy`<br> `xdebug`<br> `xmlrpc`<br> `xsl`<br> `yaml` |

PHP模块要求与Adobe Commerce版本相关联。 请参阅 [PHP要求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### 扩展支持

对于Pro项目，需要其他支持才能安装以下扩展：

- `sourceguardian`

例如，要将PHP设置为在所有环境中仅执行受SourceGuardian保护的脚本，必须在 `php.ini` 文件：

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

请参阅 [SourceGuardian文档的第3.5节](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _这是指向PDF的链接_.

[提交Adobe Commerce支持票证](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) 以获取有关在所有生产环境和Pro暂存环境中安装这些PHP扩展的帮助。 包含您更新的 `.magento/services.yaml` 文件， `.magento.app.yaml` 更新的PHP版本和任何其他PHP扩展名的文件。 对于实时生产环境的更改，您必须至少提供48小时的通知。 Cloud Infrastructure团队更新项目最多可能需要48小时。

>[!WARNING]
>
>不支持通过调试编译的PHP，并且探测器可能与 [!DNL XDebug] 或 [!DNL XHProf]. 启用Probe时禁用这些扩展。 探测与某些PHP扩展冲突，例如 [!DNL Pinba] 或IonCube。
