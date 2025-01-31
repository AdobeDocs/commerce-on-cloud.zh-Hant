---
title: 智慧型精靈
description: 瞭解如何使用智慧型精靈，評估雲端基礎結構專案上的Adobe Commerce是否遵循部署最佳實務。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# 智慧型精靈

智慧型精靈可協助您判斷雲端設定是否遵循最佳實務。 可用的精靈可協助進行下列設定：

- 最理想的狀態，可將部署停機時間降到最低
- 資料庫和Redis的負載平衡設定
- 隨選、建置階段或部署階段的靜態內容部署(SCD)

每個智慧型精靈指令都會提供驗證回應，以及適當設定的建議（如適用）。

| 命令 | 說明 |
| ------- | ------------|
| `wizard:ideal-state` | 檢查SCD是否在&#x200B;_組建_&#x200B;階段中，`SKIP_HTML_MINIFICATION`變數為`true`，以及在雲端環境中設定的post_deploy掛接。 不用於本機開發環境。 |
| `wizard:master-slave` | 檢查`REDIS_USE_SLAVE_CONNECTION`變數和`MYSQL_USE_SLAVE_CONNECTION`變數是否為`true`。 |
| `wizard:scd-on-demand` | 檢查`SCD_ON_DEMAND`全域環境變數為`true`。 |
| `wizard:scd-on-build` | 檢查&#x200B;_組建_&#x200B;階段的`SCD_ON_DEMAND`全域環境變數為`false`，`SKIP_SCD`環境變數為`false`。 驗證`config.php`檔案是否包含商店、商店群組和網站的資訊。 |
| `wizard:scd-on-deploy` | 檢查&#x200B;_部署_&#x200B;階段的`SCD_ON_DEMAND`全域環境變數為`false`，`SKIP_SCD`環境變數為`false`。 驗證`config.php`檔案是否包含&#x200B;_NOT_&#x200B;包含包含相關資訊的商店、商店群組和網站清單。 |

例如，您可以驗證您的設定是否正確啟用SCD隨選功能：

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

成功的設定傳回：

```
SCD on-demand is enabled
```

失敗的設定傳回：

```
SCD on-demand is disabled
```

## 驗證理想的設定

您的Cloud專案的&#x200B;_理想_&#x200B;設定可讓快取暖身，並在使用者要求時產生靜態內容，協助將部署停機時間降到最低。 此精靈會在部署過程中自動執行。 如果您的雲端未針對此&#x200B;_理想狀態_&#x200B;進行設定，則您會收到類似下列的訊息：

```
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

根據輸出，您需要對設定進行下列更正：

1. 啟用略過HTML縮制變數。

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. 設定部署後鉤點。

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. 推送您的程式碼變更，然後再次執行測試。 當您的設定是&#x200B;_理想_&#x200B;時，您會收到下列訊息。

   ```
   Ideal state is configured
   ```
