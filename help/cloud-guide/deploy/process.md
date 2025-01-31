---
title: 部署流程
description: 瞭解部署如何適用於Adobe Commerce的雲端基礎結構專案。
feature: Cloud, Build, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# 部署流程

當您執行環境的合併、推播或同步處理時，或當您觸發[手動重新部署](../dev-tools/cloud-cli-overview.md#redeploy-the-environment)時，部署程式就會開始。 部署過程需要時間，但最佳化部署的方法取決於您是開發、測試還是使用即時網站。 最明顯的是，您可以控制[靜態內容部署](static-content.md)。

部署流程有三個不同的階段：建置、部署和部署後。 每個階段會使用有限的資源執行特定動作：

## ![組建階段](../../assets/status-build.png)組建階段

_組建_&#x200B;階段會為組態檔中定義的服務組裝容器、根據`composer.lock`檔案安裝相依性，以及執行`.magento.app.yaml`檔案中定義的組建掛接。 由於無法連線到任何服務或存取資料庫，所以組建階段會視環境所限定的資源而定。

## ![部署階段](../../assets/status-deploy.png)部署階段

_部署_&#x200B;階段會暫時保留傳入的要求，並將網站轉換成[維護模式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html)。 部署階段會使用新的容器，掛載檔案系統之後會開啟網路連線、啟用`.magento.app.yaml`檔案的`relationships`區段中定義的服務，以及執行`.magento.app.yaml`檔案中定義的部署掛接。 除了`.magento.app.yaml`檔案中定義的目錄之外，所有專案都是&#x200B;_唯讀_。 依預設，[`mounts`屬性](../application/properties.md#mounts)包含下列目錄：

- `app/etc` — 包含`env.php`與`config.php`組態檔
- `pub/media` — 包含所有媒體資料，例如產品或類別
- `pub/static` — 包含產生的靜態檔案
- `var` — 包含執行期間建立的暫存檔案

所有其他目錄具有唯讀許可權。 當新網站從維護模式轉換出，並解除對傳入請求的臨時保留時，會在部署階段結束時變為使用中。

在部署階段中，`app/etc/config.php`和`app/etc/env.php`部署組態檔的復本會以BAK副檔名儲存。 請參閱[存放區設定](../store/store-settings.md#restore-configuration-files)，瞭解如何還原這些檔案。

## ![部署後階段](../../assets/status-post-deploy.png)部署後階段

_部署後_&#x200B;階段會執行`.magento.app.yaml`檔案中定義的部署後掛接。 在此階段執行任何動作都會影響網站效能；不過，您可以使用[WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages)環境變數來填入快取。

## ![驗證狀態](../../assets/status-verify.png)驗證設定

您可以執行[智慧型精靈](smart-wizards.md)，測試專案狀態的最佳設定。

>[!NOTE]
>
>透過`ece-tools` 2002.1.0和更新版本，您可以使用情境式部署功能，在雲端基礎結構專案上自訂Adobe Commerce的建置、部署和後續部署程式。 請參閱[以案例為基礎的部署](scenario-based.md)。
