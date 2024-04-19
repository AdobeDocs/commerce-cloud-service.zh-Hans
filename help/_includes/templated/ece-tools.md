---
source-git-commit: 6d8c082d78259f8f7adb0fb7f11ff4fcdb234124
workflow-type: tm+mt
source-wordcount: '4030'
ht-degree: 0%

---
# ece-tools

<!-- The template to render with above values -->
**版本**：2002.1.18

此参考包含34个命令，这些命令可通过 `ece-tools` 命令行工具。
初始列表是使用 `ece-tools list` 云基础架构上的Adobe Commerce上的命令。

>[!NOTE]
>
>此引用从应用程序代码库生成。 要更改内容，可以在中更新相应命令实施的源代码 [代码库](https://github.com/magento/magento-cloud-cli) 存储库并提交更改以供审阅。 另一种方法是 _向我们提供反馈_ （在右上角找到链接）。 有关贡献准则，请参阅 [代码贡献内容](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

## `_complete`

用于提供外壳完成建议的内部命令

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

### `--shell`， `-s`

壳类型(“bash”、“fish”、“zsh”)

- 需要一个值

### `--input`， `-i`

输入令牌的数组（例如COMP_WORDS或argv）

- 默认： `[]`
- 需要一个值

### `--current`， `-c`

光标所在的“输入”数组的索引（例如COMP_CWORD）

- 需要一个值

### `--api-version`， `-a`

完成脚本的API版本

- 需要一个值

### `--symfony`， `-S`

已弃用

- 需要一个值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `build`

生成应用程序。

```bash
ece-tools build
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `completion`

转储shell完成脚本

```bash
ece-tools completion [--debug] [--] [<shell>]
```


### `shell`

如果未提供shell类型（例如“bash”），则将使用“$SHELL”环境变量的值


### `--debug`

跟踪完成调试日志

- 默认： `false`
- 不接受值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `db-dump`

创建数据库备份。

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```


### `databases`

用于备份的数据库。 可用值： [主报价销售]. 如果未指定参数值，则将使用存储在中的凭据创建数据库备份 `MAGENTO_CLOUD_RELATIONSHIP` 环境变量或/和 `stage.deploy.DATABASE_CONFIGURATION` 属性。

- 默认： `[]`

- 数组

### `--remove-definers`， `-d`

从数据库转储中删除定义符

- 默认： `false`
- 不接受值

### `--dump-directory`， `-a`

使用替代目录保存转储

- 需要一个值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `deploy`

部署应用程序。

```bash
ece-tools deploy
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `help`

显示命令的帮助

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```


### `command_name`

命令名称

- 默认： `help`


### `--format`

输出格式（txt、xml、json或md）

- 默认： `txt`
- 需要一个值

### `--raw`

输出原始命令帮助

- 默认： `false`
- 不接受值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `list`

列出命令

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```


### `namespace`

命名空间名称


### `--raw`

输出原始命令列表

- 默认： `false`
- 不接受值

### `--format`

输出格式（txt、xml、json或md）

- 默认： `txt`
- 需要一个值

### `--short`

跳过描述命令的参数

- 默认： `false`
- 不接受值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `patch`

应用自定义修补程序。

```bash
ece-tools patch
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `post-deploy`

在部署操作后执行。

```bash
ece-tools post-deploy
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `run`

执行方案。

```bash
ece-tools run <scenario>...
```


### `scenario`

方案

- 默认： `[]`

- 必填
- 数组

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `backup:list`

显示备份文件的列表。

```bash
ece-tools backup:list
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `backup:restore`

还原重要的配置文件。 运行backup：list以显示备份文件列表。

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

### `--force`， `-f`

在还原备份期间覆盖现有文件

- 默认： `false`
- 不接受值

### `--file`

特定文件恢复路径

- 接受值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `build:generate`

为构建阶段生成所有必需的文件。

```bash
ece-tools build:generate
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `build:transfer`

将生成的文件传输到init目录中。

```bash
ece-tools build:transfer
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cloud:config:create`

创建 `.magento.env.yaml` 具有指定的生成、部署和部署后变量配置的文件。 覆盖任何现有的 `.magento,.env.yaml` 文件。

```bash
ece-tools cloud:config:create <configuration>
```


### `configuration`

JSON格式的配置

- 必填

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cloud:config:update`

更新现有的 `.magento.env.yaml` 具有指定配置的文件。 创建 `.magento.env.yaml` 文件是否存在。

```bash
ece-tools cloud:config:update <configuration>
```


### `configuration`

JSON格式的配置

- 必填

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cloud:config:validate`

验证 `.magento.env.yaml` 配置文件

```bash
ece-tools cloud:config:validate
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `config:dump`

用于静态内容部署的转储配置。

```bash
ece-tools config:dump
```


```bash
ece-tools dump
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cron:disable`

禁用所有Magentocron进程并终止所有正在运行的进程。

```bash
ece-tools cron:disable
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cron:enable`

启用Magentocron流程。

```bash
ece-tools cron:enable
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cron:kill`

终止所有Magentocron进程。

```bash
ece-tools cron:kill
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `cron:unlock`

解锁卡在“正在运行”状态的cron作业。

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

### `--job-code`

要解锁的Cron作业代码。

- 默认： `[]`
- 接受多个值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `dev:generate:schema-error`

从schema.error.yaml文件生成dist/error-codes.md文件。

```bash
ece-tools dev:generate:schema-error
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `dev:git:update-composer`

从Git更新部署的编辑器。

```bash
ece-tools dev:git:update-composer
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `env:config:show`

显示编码的云配置环境变量。

```bash
ece-tools env:config:show [<variable>...]
```


### `variable`

要显示的环境变量，可能的选项：服务、路由、变量

- 默认： `[]`

- 数组

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `error:show`

按错误ID显示有关错误的信息或显示有关上次部署以来所有错误的信息。

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```


### `error-code`

错误代码（如果未传递）命令显示有关上次部署的所有错误的信息


### `--json`， `-j`

用于以JSON格式获取结果

- 默认： `false`
- 不接受值

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `module:refresh`

刷新配置以启用新添加的模块。

```bash
ece-tools module:refresh
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `schema:generate`

生成架构*.dist文件。

```bash
ece-tools schema:generate
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:ideal-state`

验证配置的理想状态。

```bash
ece-tools wizard:ideal-state
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:master-slave`

验证主从配置。

```bash
ece-tools wizard:master-slave
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:scd-on-build`

验证生成配置的SCD。

```bash
ece-tools wizard:scd-on-build
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:scd-on-demand`

验证SCD on demand配置。

```bash
ece-tools wizard:scd-on-demand
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:scd-on-deploy`

在部署配置时验证SCD。

```bash
ece-tools wizard:scd-on-deploy
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值


## `wizard:split-db-state`

验证剥离数据库的能力，以及数据库是否已剥离。

```bash
ece-tools wizard:split-db-state
```

### `--help`， `-h`

显示给定命令的帮助。 未给出任何命令时，显示list命令的帮助

- 默认： `false`
- 不接受值

### `--quiet`， `-q`

不输出任何消息

- 默认： `false`
- 不接受值

### `--verbose`， `-v|-vv|-vvv`

增加消息的详细程度：1表示正常输出，2表示更多详细输出，3表示调试

- 默认： `false`
- 不接受值

### `--version`， `-V`

显示此应用程序版本

- 默认： `false`
- 不接受值

### `--ansi`

强制（或禁用 — no-ansi） ANSI输出

- 不接受值

### `--no-ansi`

否定“ — ansi”选项

- 默认： `false`
- 不接受值

### `--no-interaction`， `-n`

不要问任何交互式问题

- 默认： `false`
- 不接受值

