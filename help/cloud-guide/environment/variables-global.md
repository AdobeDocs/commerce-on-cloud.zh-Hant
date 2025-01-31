---
title: 全域變數
description: 請參閱環境變數清單，這些變數可控制Adobe Commerce對雲端基礎結構部署程式的動作。
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# 全域變數

全域變數可控制[!DNL Commerce]部署流程每個階段的動作：建置、部署和部署後。 由於全域變數會影響每個階段，因此您必須在`.magento.env.yaml`檔案的`global`階段中設定它們：

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

如需自訂建置和部署程式的詳細資訊：

- [部署設定](configure-env-yaml.md)
- [部署流程](../deploy/process.md)

## `ENABLE_EVENTING`

- **預設**-_未設定_
- **版本**—Adobe Commerce 2.4.5和更新版本

設定為`true`時，可讓cron執行訊息佇列取用者。 適用於Adobe Commerce的Adobe I/O Events使用訊息佇列來加速傳送關鍵事件。

Adobe建議您也將[`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner)變數新增至`.magento.env.yaml`檔案的`deploy`階段（其中`cron_run`設定為`true`）。

下列範例顯示完整設定的`ENABLE_EVENTING`變數。

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **預設**-_未設定_
- **版本**—Adobe Commerce 2.4.4和更新版本

設定為`true`時，啟用Commerce Webhook。 webhook會在外部端點上執行，例如App Builder執行階段動作或第三方清查管理系統。 [_Webhooks指南_](https://developer.adobe.com/commerce/extensibility/webhooks)詳細說明此功能。

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

覆寫所有輸出資料流的最低記錄層級，而不變更程式碼，這在疑難排解部署問題時很有幫助。 例如，如果您的部署失敗，您可以使用此變數在全域提高記錄粒度。 請參閱[記錄層級](log-handlers.md#log-levels)。 記錄處理常式中的`min_level`值會覆寫此設定。

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>`MIN_LOGGING_LEVEL`變數的設定不會變更檔案處理常式的記錄層級組態，預設為`debug`。

## `SCD_ON_DEMAND`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

當使用者(SCD)要求時，啟用產生靜態內容的功能。 隨選靜態內容最適合用於開發和測試工作流程，因為這可縮短部署時間。

使用[`post_deploy`鉤點](../application/hooks-property.md)預先載入快取，可減少網站停機時間。 快取暖功能僅適用於[!DNL Cloud Console]中包含「測試」與「生產」環境的Pro專案，以及入門專案。 將`SCD_ON_DEMAND`環境變數新增至`.magento.env.yaml`檔案中的`global`階段：

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

`SCD_ON_DEMAND`變數在兩個階段（建置和部署）中都會略過SCD，清除`pub/static`和`var/view_preprocessed`資料夾，並將下列內容寫入`app/etc/env.php`檔案：

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.2.0和更新版本

可讓您增加靜態內容部署的最大預期執行時間。

依預設，Adobe Commerce會將最大預期執行時間設為900秒，但在某些情況下，您可能需要更多時間才能完成雲端專案的靜態內容部署。

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.4.2和更新版本

設定為`true`以防止在建置和部署階段期間產生父系主題的靜態內容。 當此選項設定為`true`時，產生的靜態內容較少，這會改善您的整體建置和部署時間。

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.3.0和更新版本

[Baler](https://github.com/magento/baler)是掃描您產生的JavaScript程式碼並建立最佳化JavaScript套件的模組。 將最佳化的套件組合部署至您的網站，可以減少載入網站時的網路要求數目，並改善頁面載入時間。

設定為`true`以執行靜態內容部署後執行Baler。

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>使用此功能之前，請先安裝並設定Baler模組。 因為Baler是Alpha發行版本，僅在中繼環境中啟用此選項。

## `SKIP_HTML_MINIFICATION`

- **預設**：
   - `true` — 適用於`ece-tools` 2002.0.13和更新版本
   - `false` — 適用於舊版`ece-tools`
- **版本**—Adobe Commerce 2.1.4和更新版本

啟用或停用在建置階段結束時複製靜態檢視檔案至`<magento_root>/init/`目錄。 如果設為`true`，將不會複製檔案，而且可依請求使用HTML縮制。 將此值設為`true`可減少部署至中繼和生產環境時的停機時間。

- **`false`** — 在建置階段結束時，將`view_preprocessed`目錄複製到`<magento_root>/init/`目錄，並在部署階段開始時還原`<magento_root>/var`目錄中的目錄。
- **`true`** — 啟用隨選HTML縮制；會&#x200B;_不會_&#x200B;將`<magento_root>var/view_preprocessed`複製到建置階段結束時的`<magento_root>/init/`目錄。

將`SKIP_HTML_MINIFICATION`環境變數新增至`.magento.env.yaml`檔案中的`global`階段：

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

使用`X_FRAME_CONFIGURATION`變數來變更Adobe Commerce網站的[`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html)標頭設定。 此設定控制瀏覽器如何呈現`<frame>`、`<iframe>`或`<object>`中的頁面。 使用下列其中一個選項：

- `DENY` — 頁面無法顯示在框架中。
- `SAMEORIGIN`—(預設Adobe Commerce設定。) 頁面只能在與頁面本身相同原點的框架中顯示。

>[!WARNING]
>
>`ALLOW-FROM <uri>`選項已過時，因為Adobe Commerce支援的瀏覽器不再支援。 檢視[瀏覽器相容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)。

將`X_FRAME_CONFIGURATION`環境變數新增至`.magento.env.yaml`檔案中的`global`階段：

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
