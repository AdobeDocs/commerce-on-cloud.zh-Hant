---
title: 靜態內容部署
description: 瞭解在Adobe Commerce上雲端基礎結構專案部署靜態內容（例如影像、指令碼和CSS）的策略。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---

# 靜態內容部署策略

靜態內容部署(SCD)對存放區部署程式有重大影響，這取決於要產生多少內容（例如影像、指令碼、CSS、影片、主題、區域設定和網頁）以及何時產生內容。 例如，當網站處於維護模式時，預設策略會在[部署階段](process.md#deploy-phase-deploy-phase)期間產生靜態內容；然而，此部署策略需要一些時間才能將內容直接寫入掛接的`pub/static`目錄。 您有多種選項或策略可協助您根據需求改善部署時間。

## 最佳化JavaScript和HTML內容

您可以使用套件和縮制在靜態內容部署期間建置最佳化的JavaScript和HTML內容。

### 將內容縮制

如果您略過複製`var/view_preprocessed`目錄中的靜態檢視檔案，並在要求時產生&#x200B;_縮制的_ HTML，則可以改善部署程式期間的SCD載入時間。 您可以在`.magento.env.yaml`檔案中將[SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification)全域環境變數設定為`true`來啟動此專案。

>[!NOTE]
>
>從`ece-tools`封裝版本2002.0.13開始，SKIP_HTML_MINIFICATION變數的預設值設為`true`。

您可以減少不必要的佈景主題檔案數目，以節省&#x200B;**更多**&#x200B;部署時間和磁碟空間。 例如，您可以部署英文版的`magento/backend`佈景主題，以及其他語言版的自訂佈景主題。 您可以使用[SCD_MATRIX](../environment/variables-deploy.md#scdmatrix)環境變數來設定這些主題設定。

## 選擇部署策略

部署策略會因您在&#x200B;_建置_&#x200B;階段、_部署_&#x200B;階段或&#x200B;_隨選_&#x200B;階段期間選擇產生靜態內容而有所不同。 如下圖所示，在部署階段期間產生靜態內容是最不理想的選擇。 即使使用最小化HTML，每個內容檔案都必須複製到掛接的`~/pub/static`目錄，這可能需要很長的時間。 隨選產生靜態內容似乎是最佳選擇。 不過，如果內容檔案不存在於其產生之快取中，則會在請求該檔案時增加使用者體驗的載入時間。 因此，在建置階段期間產生靜態內容是最理想的作法。

![SCD載入比較](../../assets/scd-load-times.png)

### 在建置時設定SCD

在建置階段期間使用最小化HTML產生靜態內容是&#x200B;[**零停機時間**&#x200B;部署](reduce-downtime.md)的最佳設定，也稱為&#x200B;**理想狀態**。 它不會將檔案複製到已掛載的磁碟機，而是從`./init/pub/static`目錄建立符號連結。

產生靜態內容需要存取主題和區域設定。 Adobe Commerce會將主題儲存在檔案系統中（可在建置階段存取），但Adobe Commerce會將地區設定儲存在資料庫中。 資料庫在建置階段中&#x200B;_無法使用_。 為了在建置階段產生靜態內容，您必須使用`ece-tools`封裝中的`config:dump`命令，將地區設定移至檔案系統。 它會讀取地區設定並將它們儲存在`app/etc/config.php`檔案中。

**若要將專案設定為在組建**&#x200B;上產生SCD：

1. 在本機工作站上，變更至專案目錄。
1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 將地區設定移至檔案系統，然後更新[`config.php`檔案](../development/commerce-version.md#create-a-configphp-file)。

1. `.magento.env.yaml`組態檔應包含下列值：

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)為`true`
   - 組建階段上的[SKIP_SCD](../environment/variables-build.md#skip_scd)為`false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy)為`compact`

1. 驗證`.magento.app.yaml`檔案中[部署後鉤點](../application/hooks-property.md)的設定。

1. 執行[智慧型精靈以取得理想狀態](smart-wizards.md)來驗證您的設定。

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### 隨選設定SCD

針對整合環境中的開發工作流程，建議依需求產生SCD。 這可縮短部署時間，讓您快速檢閱實作並執行整合測試。 在`.magento.env.yaml`檔案的全域階段中啟用[SCD_ON_DEMAND](../environment/variables-global.md#scdondemand)環境變數。 SCD_ON_DEMAND變數會覆寫與SCD相關的所有其他設定，並清除`~/pub/static`目錄中的現有內容。

使用SCD隨選策略時，有助於使用您預期請求的頁面（例如首頁）預先載入快取。 在`.magento.env.yaml`檔案的部署後階段的[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages)環境變數中，新增您的預期頁面清單。

>[!WARNING]
>
>請勿在生產環境中使用SCD隨選策略。

### 跳過SCD

有時您可以選擇完全略過產生靜態內容。 您可以在全域階段中設定[SKIP_SCD](../environment/variables-build.md#skipscd)環境變數，以忽略與SCD相關的其他設定。 這不會影響`~/pub/static`目錄中的現有內容。
