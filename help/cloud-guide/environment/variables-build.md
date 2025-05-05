---
title: 建立變數
description: 請參閱在雲端基礎結構建置階段控制Adobe Commerce中動作的環境變數清單。
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# 建立變數

下列&#x200B;_組建_&#x200B;變數控制建置階段中的動作，可以繼承及覆寫來自[全域變數](variables-global.md)的值。 在`.magento.env.yaml`檔案的`build`階段中插入這些變數：

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

如需自訂建置和部署程式的詳細資訊：

- [部署設定](configure-env-yaml.md)
- [部署流程](../deploy/process.md)

下列變數已在v2.2中移除：

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **預設**—`1`
- **版本**—Adobe Commerce 2.1.4和更新版本

設定儲存錯誤報告檔案的目錄巢狀層級，以避免報告目錄填入數萬個檔案，這會使管理和檢閱資料變得困難。 此設定預設為`1`。 一般而言，除非您在管理`<magento_root>/var/report/`目錄中的錯誤報告檔案時遇到問題，否則不需要變更預設值。

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

指定部署期間要套用的Adobe Commerce品質修補程式清單。

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

下列範例指定部署期間要套用的三個修補程式。

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

請參閱[套用修補程式](../development/apply-patches.md)。

## `SCD_COMPRESSION_LEVEL`

- **預設**—`6`
- **版本**—Adobe Commerce 2.1.4和更新版本

指定壓縮靜態內容時要使用的[gzip](https://www.gnu.org/software/gzip)壓縮等級（`0`到`9`）；`0`會停用壓縮。

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **預設**—`600`
- **版本**—Adobe Commerce 2.1.4和更新版本

當壓縮靜態資產所花的時間超過壓縮逾時限制時，就會中斷部署程式。 設定靜態內容壓縮命令的執行時間上限（秒）。

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **預設**—`false`
- **版本**—Adobe Commerce 2.4.2和更新版本

設定為`true`以防止在建置階段產生父系主題的靜態內容。

在建置階段設定`SCD_NO_PARENT: false`，以便父系主題產生靜態內容不會影響網站部署或造成不必要的網站停機。 請參閱[靜態內容部署](../deploy/static-content.md)。

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

您可以為每個主題設定多個地區設定。 此自訂功能可減少不必要的佈景主題檔案數目，有助於加速建置流程。 例如，您可以建置英文的&#x200B;_magento/backend_&#x200B;佈景主題，以及其他語言的自訂佈景主題。

下列範例會建置具有三種地區設定的`Magento/backend`主題：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

下列範例會建置三個主題和三個地區設定：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

或者，您可以選擇&#x200B;_不_&#x200B;部署佈景主題：

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.2.0和更新版本

可讓您增加靜態內容部署的最大預期執行時間。

根據預設，雲端基礎結構上的Adobe Commerce將最大預期執行設為900秒，但在某些情況下，您可能會需要更多時間來完成雲端專案的靜態內容部署。

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **預設**—`quick`
- **版本**—Adobe Commerce 2.2.0和更新版本

自訂靜態內容的[部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)。 請參閱[部署靜態檢視檔案](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)。

如果您有多個地區設定，請只使用這些選項&#x200B;_1&rbrace;：_

- `standard` — 為所有封裝部署所有靜態檢視檔案。
- `quick` — （_預設_）可縮短部署時間。
- `compact` — 節省伺服器上的磁碟空間。 在Adobe Commerce 2.2.4版和更早版本中，此設定會以`1`的值覆寫`scd_threads`的值。

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **預設** — 自動
- **版本**—Adobe Commerce 2.1.4和更新版本

設定靜態內容部署的執行緒數目。 預設值是根據偵測到的CPU執行緒計數而設定，不會超過4的值。 增加執行緒數目會加速靜態內容部署；減少執行緒數目會減慢速度。 您可以設定螺紋值，例如：

```yaml
stage:
  build:
    SCD_THREADS: 2
```

若要進一步縮短部署時間，請使用[組態管理](../store/store-settings.md)搭配`scd-dump`命令，將靜態部署移至建置階段。

## `SCD_USE_BALER`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.3.0和更新版本

[Baler](https://github.com/magento/baler)會掃描您產生的JavaScript程式碼並建立最佳化的JavaScript套件組合。 將最佳化的套件組合部署至您的網站，可以減少載入網站時的網路要求數目，並改善頁面載入時間。

設定為`true`以執行靜態內容部署後執行Baler。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>由於Baler是Alpha版本，不建議在生產環境中使用。

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **預設**— _未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

設定為`true`以在Cloud Docker安裝期間略過`composer dump-autoload`命令。 此變數僅與具有可寫入檔案系統的Cloud Docker容器相關。 在這種情況下，略過命令可防止其他命令嘗試從已刪除的`generated`目錄存取程式碼時發生錯誤。

當Adobe Commerce執行`composer dump-autoload`時，它會建立自動載入檔案，並包含到`generated`資料夾中產生的類別的連結，這在具有唯讀檔案系統的生產環境中不是問題。 但是，對於具有可寫入檔案系統的Cloud Docker安裝（僅針對使用`./vendor/bin/ece-docker build:compose --with-test`的測試和開發而建立），您可以執行`bin/magento -n setup:upgrade`命令而不使用`--keep-generated`選項，這會刪除`generated`目錄。 如果刪除目錄，則`composer dump-autoload`命令會失敗，因為自動載入包含已刪除目錄中檔案的連結。

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **預設**— _未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

設為`true`會在建置階段期間略過靜態內容部署。

如果您已在使用[組態管理](../store/store-settings.md)的建置階段中部署靜態內容，您可以略過靜態內容部署以進行快速建置測試。

在建置階段中，設定`SKIP_SCD: false`，以便在建置階段期間進行靜態內容建置，該程式不會影響網站部署或造成不必要的網站停機。 請參閱[靜態內容部署](../deploy/static-content.md)。

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

啟用或停用部署階段所執行`bin/magento` CLI命令的[Symfony](https://symfony.com/doc/current/console/verbosity.html)偵錯詳細程度。

>[!NOTE]
>
>若要使用VERBOSE_COMMANDS來控制命令輸出中成功和失敗的`bin/magento` CLI命令的詳細資料，您必須設定[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`。

選擇記錄中提供的詳細資訊等級：

- `-v`=一般輸出
- `-vv`=其他詳細資訊輸出
- `-vvv` =詳細輸出，適用於偵錯

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
