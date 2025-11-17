---
source-git-commit: 1521b83be787fd7d8e0db302bfaea48f912af572
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 3%

---
# ece-tools

**版本**： 2002.2.9

此參考包含34個可透過`ece-tools`命令列工具使用的命令。
在雲端基礎結構上的Adobe Commerce中使用`ece-tools list`命令會自動產生初始清單。

## 一般

此參考是從應用程式程式碼基底產生的。 若要變更內容，請&#x200B;_提供意見回饋_ （在右上角尋找連結）。 如需貢獻准則，請參閱[程式碼貢獻](https://developer.adobe.com/commerce/contributor/guides/code-contributions/)。

### 全域選項

#### `--help`，`-h`

顯示指定指令的說明。 當沒有指定命令時，顯示清單命令的說明

- 預設： `false`
- 不接受值

#### `--silent`

不輸出任何訊息

- 預設： `false`
- 不接受值

#### `--quiet`，`-q`

只顯示錯誤。 會隱藏所有其他輸出

- 預設： `false`
- 不接受值

#### `--verbose`，`-v|-vv|-vvv`

增加訊息的詳細程度：1代表一般輸出，2代表較詳細輸出，3代表偵錯

- 預設： `false`
- 不接受值

#### `--version`，`-V`

顯示此應用程式版本

- 預設： `false`
- 不接受值

#### `--ansi`

強制（或停用 — no-ansi） ANSI輸出

- 不接受值

#### `--no-ansi`

否定「 — ansi」選項

- 不接受值

#### `--no-interaction`，`-n`

請勿詢問任何互動式問題

- 預設： `false`
- 不接受值


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

提供殼層完成建議的內部命令

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--shell`，`-s`

殼層型別(「bash」、「fish」、「zsh」)

- 需要值

#### `--input`，`-i`

輸入權杖的陣列（例如COMP_WORDS或argv）

- 預設： `[]`
- 需要值

#### `--current`，`-c`

游標所在的「輸入」陣列索引（例如COMP_CWORD）

- 需要值

#### `--api-version`，`-a`

完成指令碼的API版本

- 需要值

#### `--symfony`，`-S`

已棄用

- 需要值


## `build`

```bash
ece-tools build
```

建置應用程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

傾印殼層完成指令碼

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### 引數

#### `shell`

如果未指定shell型別（例如&quot;bash&quot;），則會使用&quot;$SHELL&quot;環境變數的值

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--debug`

追蹤完成偵錯記錄

- 預設： `false`
- 不接受值


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

建立資料庫備份。

### 引數

#### `databases`

用於備份的資料庫。 可用值： [主要報價銷售]。 如果未指定引數值，則會使用儲存在`MAGENTO_CLOUD_RELATIONSHIP`環境變數或/和.magento.env.yaml組態檔的`stage.deploy.DATABASE_CONFIGURATION`屬性中的認證來建立資料庫備份。

- 預設： `[]`
- 陣列

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--remove-definers`，`-d`

從資料庫傾印中移除定義項

- 預設： `false`
- 不接受值

#### `--dump-directory`，`-a`

使用替代目錄來儲存傾印

- 需要值


## `deploy`

```bash
ece-tools deploy
```

部署應用程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

顯示命令的說明

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### 引數

#### `command_name`

命令名稱

- 預設： `help`

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--format`

輸出格式（txt、xml、json或md）

- 預設： `txt`
- 需要值

#### `--raw`

輸出原始指令說明

- 預設： `false`
- 不接受值


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

清單命令

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### 引數

#### `namespace`

名稱空間名稱

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--raw`

輸出原始命令清單

- 預設： `false`
- 不接受值

#### `--format`

輸出格式（txt、xml、json或md）

- 預設： `txt`
- 需要值

#### `--short`

略過描述命令引數的方式

- 預設： `false`
- 不接受值


## `patch`

```bash
ece-tools patch
```

套用自訂修補程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `post-deploy`

```bash
ece-tools post-deploy
```

在部署作業之後執行。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `run`

```bash
ece-tools run <scenario>...
```

執行案例。

### 引數

#### `scenario`

個案例

- 預設： `[]`
- 必填

- 陣列

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `backup:list`

```bash
ece-tools backup:list
```

顯示備份檔案的清單。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

還原重要的組態檔。 執行備份:list以顯示備份檔案清單。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--force`，`-f`

還原備份期間覆寫現有檔案

- 預設： `false`
- 不接受值

#### `--file`

特定檔案復原路徑

- 接受值


## `build:generate`

```bash
ece-tools build:generate
```

產生建置階段的所有必要檔案。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `build:transfer`

```bash
ece-tools build:transfer
```

將產生的檔案傳輸到init目錄中。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

使用指定的組建、部署和部署後變陣列態建立`.magento.env.yaml`檔案。 覆寫任何現有的`.magento.env.yaml`檔案。

### 引數

#### `configuration`

JSON格式的設定

- 必填

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

以指定的組態更新現有的`.magento.env.yaml`檔案。 建立`.magento.env.yaml`檔案（若不存在）。

### 引數

#### `configuration`

JSON格式的設定

- 必填

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

驗證`.magento.env.yaml`組態檔

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `config:dump`

```bash
ece-tools config:dumpdump
```

靜態內容部署的傾印設定。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cron:disable`

```bash
ece-tools cron:disable
```

停用所有Magento cron程式並終止所有正在執行的程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cron:enable`

```bash
ece-tools cron:enable
```

啟用Magento cron程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cron:kill`

```bash
ece-tools cron:kill
```

終止所有Magento cron程式。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

解鎖卡在「執行中」狀態的cron工作。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--job-code`

要解除鎖定的Cron工作代碼。

- 預設： `[]`
- 接受多個值


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

從schema.error.yaml檔案產生dist/error-codes.md檔案。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

從Git更新部署的撰寫器。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

顯示編碼的雲端設定環境變數。

### 引數

#### `variable`

要顯示的環境變數，可能的選項：服務、路由、變數

- 預設： `[]`
- 陣列

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

依錯誤ID顯示錯誤相關資訊或上次部署的所有錯誤相關資訊。

### 引數

#### `error-code`

錯誤代碼（如果未傳遞命令），顯示有關上次部署發生的所有錯誤的資訊

### 選項

如需全域選項，請參閱[全域選項](#global-options)。

#### `--json`，`-j`

用於取得JSON格式的結果

- 預設： `false`
- 不接受值


## `module:refresh`

```bash
ece-tools module:refresh
```

重新整理設定以啟用新新增的模組。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `schema:generate`

```bash
ece-tools schema:generate
```

產生結構描述*.dist檔案。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

驗證設定的理想狀態。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

驗證主從配置。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

驗證組建組態上的SCD。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

驗證SCD on demand設定。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

驗證部署組態上的SCD。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

驗證分割DB的能力以及DB是否已分割。

### 選項

如需全域選項，請參閱[全域選項](#global-options)。
