---
title: ece-tools的發行說明封存
description: 瞭解ece-tools的封存改善。
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# ece-tools的發行說明封存

>[!NOTE]
>
>這些發行說明提供`ece-tools` v2002.0.22和更新版本的資訊和更新。 請參閱[Cloud Tools Suite](cloud-tools-suite.md)的發行說明，以取得`ece-tools`和其他雲端套件的最新更新。

## v2002.0.22

`ece-tools` 2002.0.22版本變更`ece-tools`套件的結構，將`Adobe Commerce on cloud infrastructure`修補程式版本與ECE-Tools版本分離。 自此發行版本開始，將使用[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)套件提供修補程式和重要修正，這是`ece-tools`套件的新相依性。 我們進行這些變更，以降低排程版本更新及處理社群貢獻的複雜性。

- ![新圖示](../../assets/new.svg) **ECE-Tools封裝的變更**

   - ![新圖示](../../assets/new.svg)已將Adobe Commerce修補程式從`ece-tools`套件移至新的[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)撰寫器套件。

   - ![新圖示](../../assets/new.svg)已更新`ece-tools`封裝的`composer.json`檔案，以新增`magento/magento-cloud-patches` v1.0.0封裝的相依性。

   - ![修正圖示](../../assets/fix.svg)修正從2.3.2-p2版或更新版本開始，在僅限安全性發行版本上套用修補程式集時，`ece-tools`修補程式中斷的問題。 此問題是由針對[僅限安全性修補程式](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/security-patches/overview).<!--MAGECLOUD-4661-->採用的新版本化Scheme所引進

- ![修正圖示](../../assets/fix.svg) **修補程式和重要修正** — 使用`ece-tools`版本2002.0.22更新您的雲端環境，以套用下列修補程式和重要修正。 這些修補程式包含在`magento/magento-cloud-patches` v1.0.0套件中。

   - ![修正圖示](../../assets/fix.svg) **2.3.1.x和2.3.2.x版本的Page Builder安全性修補程式** — 修正Page Builder預覽中的問題，該問題允許未經驗證的使用者存取某些範本化方法，這些方法可用於在網路(RCE)上觸發任意程式碼執行，從而導致全域資訊洩漏。 在Adobe Commerce 2.3.1和2.3.2.<!--MAGECLOUD-4649-->版中使用不支援的頁面產生器版本時，可能會發生此問題

   - ![修正圖示](../../assets/fix.svg) **MSI修補程式** — 修正使用預設庫存設定來管理庫存時，造成索引錯誤和效能問題的問題。<!--MAGECLOUD-4428-->

   - ![修正圖示](../../assets/fix.svg) **新郵件介面的回溯相容性** — 修正Adobe Commerce v2.3.3中引入的`Magento\Framework\Mail\EmailMessageInterface` PHP介面所造成的回溯不相容問題。在此修補程式的範圍內，新的`EmailMessageInterface`繼承自舊的`MessageInterface`，Adobe Commerce核心模組將還原為相依於`MessageInterface`。<!--MAGECLOUD-4422-->

   - ![修正圖示](../../assets/fix.svg) **目錄分頁無法在Elasticsearch6.x上運作** — 修正搜尋結果分頁的重要問題，此問題會影響使用Elasticsearch6.x做為目錄搜尋引擎的客戶。<!--MAGECLOUD-4448-->

## v2002.0.21

- ![新圖示](../../assets/new.svg) **Docker更新**—

   - ![新圖示](../../assets/new.svg) **新Docker影像** — 由2.3.3和更新版本支援<!-- MAGECLOUD-3345 -->

      - PHP 7.3.<!-- MAGECLOUD-4017 -->版

      - 清漆快取6.2.0<!-- MAGECLOUD-4017 -->

   - ![新圖示](../../assets/new.svg)已新增支援，以便在Docker環境中套用`.magento.app.yaml`中指定的自訂勾點設定。 以前，Docker環境僅支援預設掛接配置。<!-- MAGECLOUD-3505-->

   - ![新圖示](../../assets/new.svg) Docker ENV檔案在Docker建置期間不再產生，且`docker:config:convert`命令已棄用。 對應的資料現在儲存在`docker-compose.yml`檔案中。<!-- MAGECLOUD-3816-->

   - ![新圖示](../../assets/new.svg) **已更新PHP映像** — 已將Node.js新增到PHP Docker映像以支援node、npm和grunt-cli功能。<!-- MAGECLOUD-3953 -->

- ![新圖示](../../assets/new.svg) **環境變數更新**-

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**LOCK_PROVIDER**&#x200B;部署變數來設定鎖定提供者，以防止啟動重複的cron工作和cron群組。 請參閱[部署變數](../environment/variables-deploy.md#lock_provider)主題中的變數說明。<!-- MAGECLOUD-4052 -->

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**CONSUMERS_WAIT_FOR_MAX_MESSAGES**&#x200B;環境變數，以設定在使用`CRON_CONSUMERS_RUNNER`環境變數管理cron工作時，消費者如何處理來自訊息佇列的訊息。 請參閱[部署變數](../environment/variables-deploy.md#consumers_wait_for_max_messages)主題中的變數說明。<!-- MAGECLOUD-4071 -->

   - ![修正圖示](../../assets/fix.svg)修正當`consumers_runner` cron工作在不同節點上啟動相同使用者的多個執行個體時，可能導致資料庫死結錯誤的問題。 現在，如果您已在您的環境中啟用&#x200B;[**CRON_CONSUMERS_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner)&#x200B;部署變數，`consumers_runner`工作會使用`single-thread`選項，在僅一個節點上啟動每個取用者的一個執行個體。<!-- MAGECLOUD-3913 -->

   - ![修正圖示](../../assets/fix.svg)已修正影響使用預設商店URL的&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;功能的問題。 現在，如果`config:show:default-url`命令無法擷取基底URL，則會使用MAGENTO_CLOUD_ROUTES變數中的URL。<!-- MAGECLOUD-3866 -->

- ![新圖示](../../assets/new.svg)已更新`module:refresh`命令傳回的記錄資訊。 現在，您可以在`cloud.log`檔案中看到已啟用模組的詳細清單。<!-- MAGECLOUD-2514 -->

- ![新圖示](../../assets/new.svg)改善了Adobe Commerce版本與已安裝服務(例如Elasticsearch、[!DNL RabbitMQ]、Redis和DB)之間相容性問題的版本相容性驗證和警告通知。<!-- MAGECLOUD-3535 -->

- ![新圖示](../../assets/new.svg)已新增對RabitMQ 3.8.<!-- MAGECLOUD-4674-->版的支援

- ![新圖示](../../assets/new.svg)已更新服務相容性的互動式驗證，以反映新的Adobe Commerce 2.3.3和2.2.10版本支援的版本。 如需建議的版本，請參閱&#x200B;_安裝指南_&#x200B;中的[系統需求](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)。<!-- MAGECLOUD-4018 -->

- ![修正圖示](../../assets/fix.svg)改善在部署階段的cron工作管理程式嘗試停止已完成的cron工作時，傳回的記錄訊息，以澄清此問題不是錯誤。 已將記錄層級從`INFO`變更為`DEBUG`.<!-- MAGECLOUD-3653-->

- ![修正圖示](../../assets/fix.svg)修正執行`setup:upgrade`命令時，未在`app:config:import`工作期間發生失敗時中斷部署程式的問題。<!-- MAGECLOUD-3806 -->

- ![新圖示](../../assets/new.svg)已將檔案處理常式的預設記錄層級變更為`debug`，以減少[!DNL Cloud Console]中顯示的記錄詳細資訊量，同時仍提供偵錯的詳細資訊。<!-- MAGECLOUD-3871 -->

- ![修正圖示](../../assets/fix.svg)修正建置期間導致靜態內容部署錯誤的問題。 在安裝和`ece-tools`設定傾印後，如果`config.php`檔案中沒有為管理員使用者指定地區設定，則會發生錯誤。 現在，`config.php`檔案中有管理員使用者的預設地區設定。<!-- MAGECLOUD-3957 -->

- ![修正圖示](../../assets/fix.svg)修正在未設定安全URL (https)的環境中，當`magento-cloud` CLI命令失敗時發生的`Undefined index error`。 現在，如果安全URL無法使用，ECE-Tools套件會使用基底URL (http)。<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![新圖示](../../assets/new.svg) **Docker更新**—

   - ![新圖示](../../assets/new.svg)您現在可以在Docker環境中使用`ece-tools`套件執行功能測試。 請參閱[應用程式測試](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/).<!-- MAGECLOUD-3129/3684 -->

   - ![新圖示](../../assets/new.svg)已新增使用`.magento.app.yaml`檔案設定PHP模組的支援。 在`.magento.app.yaml`檔案[&#128279;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings#enable-extensions)中指定的任何PHP副檔名都可在Docker PHP容器中使用。<!-- MAGECLOUD-3357 -->

   - ![新圖示](../../assets/new.svg)有新命令可用來改善Docker命令列體驗。 檢視Docker參考&rbrack;(https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli).<!-- MAGECLOUD-3569 -->的&lbrack;`bin/magento-docker`區段

   - ![新圖示](../../assets/new.svg)已新增使用Mutagen.io在本機主機和Docker之間的開發期間同步檔案的功能。<!-- MAGECLOUD-3559 -->

   - 使用Docker環境時，![修正圖示](../../assets/fix.svg)修正預設路徑。 現在，當您使用SSH登入Docker容器時，您如預期般位於`/app`目錄中的專案根目錄。<!-- MAGECLOUD-3582 -->

   - ![修正圖示](../../assets/fix.svg)已將Na程式庫從1.0.11版更新至1.0.18版，並更新Na PHP擴充功能。<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >雲端基礎結構上的Adobe Commerce客戶必須[提交Adobe Commerce支援票證](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket)，以便在升級至Adobe Commerce 2.3.2之前，在Pro生產和中繼環境上升級libna套件。目前，您無法將入門環境升級至Adobe Commerce 2.3.2。

   - ![修正圖示](../../assets/fix.svg)已將`analysis-icu`和`analysis-phonetic`Elasticsearch外掛程式新增到所有Docker影像。<!-- MAGECLOUD-3446 -->

   - ![修正圖示](../../assets/fix.svg)已改善的驗證：使用`docker:build`命令的選項時，您必須在使用選項時提供值。 此外，在使用`docker:build run`命令時已新增節點版本的驗證。<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![新圖示](../../assets/new.svg) **環境變數更新**—

   - ![新圖示](../../assets/new.svg)已新增使用[DATABASE_CONFIGURATION環境變數](../environment/variables-deploy.md#database_configuration)的資料庫表格首碼支援。<!-- MAGECLOUD-2901 -->

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**FORCE_UPDATE_URLS**&#x200B;部署變數，以便在部署至Pro和Starter生產及中繼環境時更新基底URL。 檢視[部署變數](../environment/variables-deploy.md#force_update_urls)內容中的定義。<!-- MAGECLOUD-3602 -->

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**TTFB_TESTED_PAGES**&#x200B;部署後變數，以設定&#x200B;_第一位元組時間_&#x200B;頁面測試，以檢查部署至雲端基礎結構之網站上的應用程式效能。 檢視[部署後變數](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->中的變數說明

   - ![修正圖示](../../assets/fix.svg)修正多執行緒SCD造成靜態內容部署隨機失敗的問題。 因應措施涉及將&#x200B;**SCD_THREADS**&#x200B;變數設定為`1`。 您現在可以根據需要增加計數。 檢視[部署變數](../environment/variables-deploy.md#scd_threads)和[組建變數](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->中的定義

   - ![修正圖示](../../assets/fix.svg)您可以設定&#x200B;**WARM_UP_PAGES**&#x200B;環境變數來快取單一頁面、多個網域和多個頁面。 檢視[部署後變數](../environment/variables-post-deploy.md#warm_up_pages)內容中的展開定義。<!-- MAGECLOUD-3258 -->

- ![修正圖示](../../assets/fix.svg)已將`pub/static/.htaccess`檔案新增至排除清單。 [由PHOENIX MEDIA GmbH的Bjorn Kraus提交的修正](https://github.com/magento/ece-tools/pull/455)。<!-- MAGECLOUD-3545/Github#455 -->

- ![修正圖示](../../assets/fix.svg)修正當至少一個嚴重等級驗證器傳回錯誤時，所有驗證訊息顯示為`Critical`的錯誤。<!-- MAGECLOUD-3178 -->

- ![修正圖示](../../assets/fix.svg)修正當資料庫中不存在基底URL時，造成部署失敗的問題。<!-- MAGECLOUD-3075 -->

- ![新圖示](../../assets/new.svg)已將新的&#x200B;**`env:config:show`命令**&#x200B;新增到顯示環境服務、路由或變數的`ece-tools`封裝。 請參閱[服務、路由及變數](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/ece-tools/package-overview#services-routes-and-variables)。 [Vladimir Kerkhoff提交的功能](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![修正圖示](../../assets/fix.svg)修正當嘗試安裝Adobe Commerce 2.2.6或更舊版本（含`ece-tools`開發）並在殼層重構後發生嚴重錯誤的問題。<!-- MAGECLOUD-3665 -->

- ![修正圖示](../../assets/fix.svg)修正導致Adobe Commerce 2.1.x和2.2.x安裝失敗的問題，並警告您使用過時的Carbon版本。<!-- MAGECLOUD-3704 -->

- ![修正圖示](../../assets/fix.svg)已將Shell輸出的`cloud.log`記錄層級從`info`降低為`debug`。<!-- MAGECLOUD-3277 -->

- ![修正圖示](../../assets/fix.svg)已將`--remove-definers (-d)`選項新增至`ece-tools db-dump`命令，以從傾印檔案移除定義項。<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![修正圖示](../../assets/fix.svg)修正部署期間覆寫`env.php`檔案，導致遺失自訂設定的問題。 此更新會確保雲端基礎結構上的Adobe Commerce會在每次部署時更新`env.php`檔案，同時保留自訂設定。<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![新圖示](../../assets/new.svg) **Docker更新**—

   - ![新圖示](../../assets/new.svg)現在，Docker環境支援.magento.app.yaml檔案[&#128279;](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/crons-property)的crons屬性中定義的cron設定。<!-- MAGECLOUD-3150 -->

   - ![新圖示](../../assets/new.svg) **新Docker容器** — 已新增[TLS終止Proxy容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container)，以方便透過HTTPS終止Varnish SSL。<!-- MAGECLOUD-2890 -->

   - ![新圖示](../../assets/new.svg) **新Docker映像** — 已新增Node.js映像以支援Gulp和其他功能，例如Jasmine JS單元測試。<!-- MAGECLOUD-3345 -->

   - ![新圖示](../../assets/new.svg) **Docker建置模式** — 現在您可以選擇在[生產模式或開發人員模式](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/#launch-mode)中啟動Docker環境。 開發人員模式支援具有完整可寫入檔案系統許可權的主動式開發。<!-- MAGECLOUD-3152/3511 -->

   - ![修正圖示](../../assets/fix.svg)修正當快取設定為無法使用時，導致Docker部署失敗並出現`Name or service not known`錯誤的問題。 現在，您可以從[`.magento/services.yaml`檔案](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)移除服務。 Docker設定產生器會自動更新`docker/config.php.dist`檔案中的服務。<!-- MAGECLOUD-3369 -->

   - ![新圖示](../../assets/new.svg)已新增服務相容性的互動式驗證。 現在，如果要求的服務與Adobe Commerce版本或其他服務不相容，_互動模式_&#x200B;會以訊息提示使用者，並選擇繼續。 檢視Docker可用的[服務版本](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers)。 使用`-n`選項略過互動以進行CICD。<!-- MAGECLOUD-3251 -->

   - ![修正圖示](../../assets/fix.svg)修正清除現有傾印的Docker構成`db-dump`命令問題。<!-- MAGECLOUD-3366 -->

   - ![修正圖示](../../assets/fix.svg)修正將Redis `session`、`default`和`page_cache`快取儲存體指派給相同資料庫識別碼的問題。<!-- MAGECLOUD-3172 -->

- ![新圖示](../../assets/new.svg) **環境變數更新**—

   - ![新圖示](../../assets/new.svg)新的&#x200B;**ELASTICSUITE\_CONFIGURATION**&#x200B;環境變數會在部署之間保留您的自訂服務設定。 檢視[部署變數](../environment/variables-deploy.md#elasticsuite_configuration)內容中的定義。<!-- MAGECLOUD-3205 -->

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**SCD_MAX_EXECUTION_TIMEOUT**&#x200B;環境變數，以便您可以增加從`.magento.env.yaml`檔案完成靜態內容部署的時間。 檢視[部署變數](../environment/variables-deploy.md#scd_max_execution_time)、[組建變數](../environment/variables-build.md#scd_max_execution_time)和[全域變數](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->中的定義

      - ![新圖示](../../assets/new.svg)已新增&#x200B;**MAGENTO_CLOUD_LOCKS_DIR**&#x200B;環境變數，以設定雲端基礎結構上鎖定提供者的掛接點路徑。 鎖定提供者可防止啟動重複的cron作業和cron群組。 Adobe Commerce 2.2.5版及更新版本支援此變數，且可自動設定。 檢視[雲端變數](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->中的定義

      - ![修正圖示](../../assets/fix.svg)已變更&#x200B;**SCD_THREADS**&#x200B;環境變數預設值，以根據偵測到的CPU執行緒計數自動決定最佳值。 檢視[部署變數](../environment/variables-deploy.md#scd_threads)和[組建變數](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->中更新的定義

- ![修正圖示](../../assets/fix.svg)已修正在2002.0.16版雲端基礎結構上升級為Adobe Commerce時，造成DB隔離機制修補程式錯誤的問題。<!-- MAGECLOUD-3383 -->

- ![修正圖示](../../assets/fix.svg)已新增修補程式，將&#x200B;_Google影像圖表_&#x200B;取代為&#x200B;_影像圖表_。 請參閱DevBlog文章[M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006)的Google影像圖表過時和更新。<!-- MAGECLOUD-3456 -->

- ![修正圖示](../../assets/fix.svg)已新增[SEARCH_CONFIGURATION變數](../environment/variables-deploy.md#search_configuration)的驗證。 未設定&#39;engine&#39;選項且不需要`_merge`時，部署失敗。<!-- MAGECLOUD-3470 -->

- ![修正圖示](../../assets/fix.svg)修正發生例外狀況後，會公開敏感資料的問題。 現在已適當遮罩機密資訊。<!-- MAGECLOUD-3525 -->

- ![修正圖示](../../assets/fix.svg)已改善Magento Open Source封裝的容錯設定。 在Adobe Commerce無法從Redis `slave`執行個體讀取資料的情況下，會從Redis `master`執行個體進行讀取。 請參閱[REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>`ece-tools` 2002.0.17版包含重要的安全性修補程式。 請參閱[技術資源：Magento Open Source修補程式](https://magento.com/tech-resources/download#download2288)。

- ![新圖示](../../assets/new.svg) **服務更新** — 由下列Adobe Commerce版本支援： 2.2.8和更新版本2.2.x、2.3.1和更新版本2.3.x

   - 新增對Elasticsearch 6.x.<!-- MAGECLOUD-3196 -->版的支援

   - 新增Redis 5.0版的支援。

- ![新圖示](../../assets/new.svg) **新Docker影像** — 已將以下服務新增到Docker組建：

   - Elasticsearch6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![新圖示](../../assets/new.svg) **新環境變數** — 之前，SCD壓縮有硬式編碼逾時。 現在您可以使用&#x200B;**SCD_COMPRESSION_TIMEOUT**&#x200B;環境變數來設定SCD壓縮逾時。 檢視[組建變數](../environment/variables-build.md#scd_compression_timeout)和[部署變數](../environment/variables-deploy.md#scd_compression_timeout)內容中的定義。<!-- MAGECLOUD-2870 -->

- ![修正圖示](../../assets/fix.svg)已將`--use-rewrites`選項新增至安裝命令，以便使用網站伺服器重寫店面中產生的連結，並使用管理員存取權來改善安全性和客戶體驗。<!-- MAGECLOUD-3246 -->

- ![修正圖示](../../assets/fix.svg)已新增時間戳記至`var/log/install_upgrade.log`檔案，以便顯示安裝和升級事件的日期。<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![新圖示](../../assets/new.svg) **Docker更新**—

   - 現在，在Docker環境中生成的預設服務配置與雲端範本中的預設配置相同。<!-- MAGECLOUD-3025 -->

   - 您可以使用`sendmail`服務從Docker環境傳送郵件。<!-- MAGECLOUD-2907 -->

   - 新增[設定Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)以在Cloud Docker環境中偵錯的功能。<!-- MAGECLOUD-2891 -->

   - 修正產生`docker-compose.yml`檔案時Web服務許可權的問題。<!-- MAGECLOUD-2883 -->

- ![新圖示](../../assets/new.svg) **升級改善** — 已新增驗證，以確認`composer.json`檔案中的`autoload`屬性在升級至Adobe Commerce v2.3之前包含必要的設定變更。請參閱[升級版本](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/commerce-version)。<!-- MAGECLOUD-2392 -->

- ![新圖示](../../assets/new.svg)部署靜態內容的壓縮程式現在包含所有資產（原生產生或自訂），而且會在[`build:transfer`區段](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property)開頭的建置階段期間發生。 先前，壓縮程式會在套用自訂縮制和靜態資產套件組合前進行。 [Rafael Garcia Lepper從Tryzens Limited提交的修正](https://github.com/magento/ece-tools/pull/413)。<!-- MAGECLOUD-3104 -->

- ![修正圖示](../../assets/fix.svg)修正了在設定其他資料庫和服務關聯性後，立即在部署期間發生的資料庫連線錯誤。 此外，此修正會解決在入門版Commerce報告的設定程式中發生的問題。 首先，此升級為使用Commerce報告的「必備」。<!-- MAGECLOUD-3035 -->

- ![修正圖示](../../assets/fix.svg)修正資料庫組態導致部署程式失敗的驗證問題。<!-- MAGECLOUD-3003 -->

- ![修正圖示](../../assets/fix.svg)已使用適當版本的`symfony/yaml`封裝更新條件約束，以搭配[PHP常數](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)使用。 使用3.2之前的`symfony/yaml`封裝版本時，常數剖析無法運作。[由Vladimir Kerkhoff提交的修正](https://github.com/magento/ece-tools/pull/404)。<!-- MAGECLOUD-2956 -->

- ![新圖示](../../assets/new.svg) **環境組態檢查** — 已新增驗證，以檢查PHP版本並在使用者未使用最新建議版本時警告使用者。<!--MAGECLOUD-2903-->

- ![修正圖示](../../assets/fix.svg)修正處理格式錯誤的JSON變數的問題。 現在，如果JSON變數造成語法錯誤，`cloud.log`檔案中會顯示警告，並使用預設變數繼續部署。<!-- MAGECLOUD-2851 -->

- ![修正圖示](../../assets/fix.svg)修正停用Redis服務後立即在部署期間發生的連線錯誤。<!-- MAGECLOUD-2747 -->

- ![新圖示](../../assets/new.svg) **正在記錄變更** — 已將下列建置和部署程式事件的[記錄層級](../environment/log-handlers.md#log-levels)從`Info`更新為`Notice`：<!--MAGECLOUD-2925-->

   - 將`composer.json`中已安裝的模組與`app/etc/config.php`檔案中的共用組態設定進行協調的處理程式的開始和結束

   - 設定驗證程式的開始和結束

   - 產生類別的`setup:di:compile`處理序的開始和結束

- ![新圖示](../../assets/new.svg) **新環境變數**—

   - **[RESOURCE_CONFIGURATION部署變數](../environment/variables-deploy.md#resource_configuration)** — 使用此變數將資源名稱對應到資料庫連線。<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[X_FRAME_CONFIGURATION全域變數](../environment/variables-global.md#x_frame_configuration)** — 使用此變數變更`X-Frame-Options`標題設定，以便在`<frame>`、`<iframe>`或`<object>`中呈現Adobe Commerce頁面。<!-- MAGECLOUD-3048 -->

- ![修正圖示](../../assets/fix.svg) **環境變數更新** — 已變更下列環境變數：

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** — 新增在為Adobe Commerce存放區定義的所有網域上預先載入指定頁面快取的功能。 先前，如果您的網站設定有多個網域，後部署程式無法預先載入非預設網域上指定頁面的快取，並在後部署記錄檔中傳回下列錯誤： `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** — 已使用SCD壓縮層級的正確預設值更新檔案和範例`.magento.env.yaml`檔案。 檢視[組建變數](../environment/variables-build.md#scd_compression_level)和[部署變數](../environment/variables-deploy.md#scd_compression_level)內容中的定義。<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** — 此環境變數已棄用。 使用[SCD_MATRIX](../environment/variables-build.md#scd_matrix)控制主題組態。<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX** — 修正驗證程式，以防止在SCD_MATRIX忽略包含不同字元大小寫的主題值時發生問題。 檢視[組建變數](../environment/variables-build.md#scd_matrix)和[部署變數](../environment/variables-deploy.md#scd_matrix)內容中的定義。<!-- MAGECLOUD-2904 -->

   - **管理員變數**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - 改善使用環境變數管理管理員使用者認證時的安全性。 在升級期間，您無法再使用ADMIN_EMAIL、ADMIN_USERNAME和ADMIN_PASSWORD環境變數來覆寫管理員認證。 如果您無法存取「管理員」面板，請使用&#x200B;_忘記密碼_&#x200B;功能或`admin:user:create` CLI命令來建立新的管理員使用者。 檢視[存取您的管理面板](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/start/onboarding#admin)。

      - 升級或套用修補程式時不再需要ADMIN_EMAIL。

## v2002.0.15

- ![新圖示](../../assets/new.svg) **Docker更新**—

   - 現在，當[建置您的Docker環境](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)時，Docker產生器會使用`.magento.app.yaml`和`.magento/services.yaml`組態檔中指定的服務。 您可以使用組建引數選擇不同的服務版本。<!-- MAGECLOUD-2888 -->

   - 新增PHP 7.2影像 — 在Cloud Docker中新增對PHP 7.2的支援；更新[Launch Docker組態](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)以包含`docker:build --php`選項，以指定與您的Adobe Commerce版本相容的PHP版本。<!-- MAGECLOUD-2799 -->

   - 已根據PHP-CLI影像新增[Cron容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/cli/#cron-container)。<!-- MAGECLOUD-2565 -->

   - 已將以下服務新增到Docker構建：

      - [!DNL RabbitMQ] 3.5和3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch1.7、2.4和5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2和4.0<!-- MAGECLOUD-2886 -->

- ![新圖示](../../assets/new.svg) **使用PHP常數進行設定** — 已在`.magento.env.yaml`組態檔中新增對[PHP常數](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml#php-constants)的支援。<!-- MAGECLOUD- 2575 -->

- ![新圖示](../../assets/new.svg) **新環境變數** — 依預設，只有生產環境已啟用Google Analytics。 您可以使用[ENABLE_GOOGLE_ANALYTICS環境變數](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->在測試和整合環境中啟用Google Analytics

- ![修正圖示](../../assets/fix.svg)修正重新部署後，從`env.php`檔案移除自訂cron設定的問題。 現在，自訂cron設定安全地保留在`env.php`檔案中。<!-- MAGECLOUD-2923 -->

- ![修正圖示](../../assets/fix.svg)修正建置、部署和部署後階段的訊息和[記錄層級](../environment/log-handlers.md#log-levels)中的不一致。 針對所有階段和子階段，將開始和結束記錄訊息層級從&#x200B;**資訊**&#x200B;增加至&#x200B;**通知**。 視情況新增開始和結束記錄訊息。<!-- MAGECLOUD-2919 -->

- ![修正圖示](../../assets/fix.svg)修正設定後，cron程式無法啟動部署後階段的問題。 現在，如果您已啟用部署後掛接，就會在部署後階段開始時再次啟用cron程式。<!-- MAGECLOUD-2862 -->

- ![修正圖示](../../assets/fix.svg)解決指定自訂資料庫組態時，無法成功安裝Adobe Commerce的問題。 Magento以前，安裝程式會使用[DATABASE_CLOUD_RELATIONSHIP變數](../environment/variables-cloud.md)中的資料庫組態，即使您在[DATABASE_CONFIGURATION環境變數](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->中指定了自訂的連線資訊

- ![修正圖示](../../assets/fix.svg)已更正`config:dump`命令，使其包含`config.php`檔案之`system`區段中的每個網站地區設定。<!--MAGECLOUD-2740-->

- ![修正圖示](../../assets/fix.svg)修正來源基底URL參考在部署後階段中造成&#x200B;_熱身_&#x200B;錯誤的問題。<!--MAGECLOUD-2797-->

- ![修正圖示](../../assets/fix.svg)修正在`setup:di:compile`程式期間不正確地產生檔案的問題，這會影響Amazon Pay模組。<!--MAGECLOUD-2850-->

## v2002.0.14

- ![新圖示](../../assets/new.svg) **驗證理想狀態** — 現在`ideal-state`精靈會在每次部署期間驗證目前的組態，並提供更新組態的明確指示，以實現更快的零停機部署。<!--MAGECLOUD-2372-->

- ![修正圖示](../../assets/fix.svg) **PCI法規遵循** — 已更新雲端基礎結構上Adobe Commerce的傳訊通訊協定，以在與協力廠商傳訊服務連線時要求傳輸層安全性(TLS) 1.2版。 如果您使用的訊息服務不支援TLS 1.2版，您必須升級服務。 否則，當您的Adobe Commerce應用程式嘗試連線至郵件伺服器以傳送電子郵件時，會顯示下列錯誤訊息： `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![新圖示](../../assets/new.svg) **部署改善** — 已新增驗證，以在測試或生產環境啟用`dev`、`debug`或`debug_logging`選項時，警告客戶，以防止過度記錄活動造成的效能問題。<!--MAGECLOUD-2517-->

- ![修正圖示](../../assets/fix.svg) **部署修正**—

   - 維護模式現在會在部署階段開始時啟用，並在結束時停用。 如果部署失敗，則站點將保持維護模式，直到部署問題得到解決。 以前，即使部署失敗，網站也會回到生產模式。<!--MAGECLOUD-2603-->

      - 已重新處理部署階段驗證檢查，將下列部署問題的錯誤層級從`CRITICAL`降級為`WARNING`，以便完成部署。 以前，這些問題會導致部署失敗。

      - 環境設定包含不正確的部署或雲端變數值。

   - 雲端基礎結構上的Elasticsearch版本與雲端基礎結構上Adobe Commerce支援的elasticsearch/elasticsearch模組版本不相容。 請參閱Adobe Commerce支援知識庫中的[Elasticsearch疑難排解文章](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento)。<!--MAGECLOUD-2600-->

   - 修正`app/etc/config.php`檔案中的共用組態設定在部署期間造成`recursion detected`錯誤的問題。<!--MAGECLOUD-2173-->

- ![修正圖示](../../assets/fix.svg) **Cron相關修正**—

   - 修正了在您指定預設值（1分鐘）以外的cron頻率時，工作無法執行的cron排程問題。<!--MAGECLOUD-2602-->

   - 修正了部署階段中允許cron工作在部署期間繼續執行的問題，這可能會導致資料庫鎖定和其他嚴重問題。 現在，所有cron工作都會在部署階段開始之前停止，並在部署完成後重新啟動。&lt;！—MAGECLOUD—2537—>

   - 修正2.2.x版中的cron工作工作流程，解除鎖定凍結的cron工作，以便在開始部署之前停止。 以前，凍結的cron工作造成部署延遲。<!--MAGECLOUD-2501-->

- ![修正圖示](../../assets/fix.svg)已變更`vendor/bin/ece-tools config:dump`命令產生的`config.php`檔案格式，以使用短陣列語法和4個空格縮排，以符合Adobe Commerce編碼標準。<!--MAGECLOUD-2527-->

- ![修正圖示](../../assets/fix.svg)修正當`.magento.env.yaml`包含Web設定的`{{ base_url }}`和`{{ unsecure_base_url }}`預留位置，而不是雲端基礎結構專案上Adobe Commerce的預設URL設定時，所發生的部署錯誤。/<!--MAGECLOUD-2607-->

## v2002.0.13

- ![新圖示](../../assets/new.svg) **啟用零停機部署** — 現在，雲端基礎結構上的Adobe Commerce會在部署期間將要求與必要的資料庫變更佇列起來，並在部署完成後立即套用變更。 請求最多可保留5分鐘，以確保不會遺失任何工作階段。 檢視[靜態內容部署選項，以減少雲端](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud)上的部署停機時間。<!--MAGECLOUD-2169-->

- ![新圖示](../../assets/new.svg) **雲端適用的Docker撰寫** — 已對[Docker設定和設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)程式進行下列改進：

   - 新增命令 — `docker:config:convert`以將PHP配置檔案轉換為Docker ENV格式以簡化環境配置。 現在，您可以將PHP配置檔案複製到Docker目錄中，並將其轉換為Docker ENV檔案。 請參閱[啟動Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/).<!--MAGECLOUD-2359-->

   - Adobe Commerce雲端基礎結構安裝程式現在支援部署至唯讀和讀寫檔案系統，以便更密切地模擬雲端檔案系統。 請參閱[設定Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)。&lt;！—MAGECLOUD—2357—>

   - **Redis服務支援** — 已新增Redis映像，該映像已部署到Docker容器並自動設定為與您的Docker安裝搭配使用。&lt;！—MAGECLOUD—2442—>

   - 現在您擁有使用Cloud Docker [資料庫容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#database-container)時的資料庫傾印功能。 此外，您可以使用`docker/mnt`目錄，在主機電腦和容器之間[共用檔案](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#sharing-data-between-host-machine-and-container)。<!-- MAGECLOUD-2577 -->

   - **清漆服務支援** — 已新增清漆影像，此影像會自動部署至Docker容器。 部署後，您可以依照Adobe Commerce最佳實務手動設定Varnish。 請參閱[設定及使用清漆](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cache/varnish/config-varnish)。&lt;！—MAGECLOUD—2358—>

   - 安全網站存取 — 新增SSL支援，可存取您的Adobe Commerce商店和管理面板。&lt;！—MAGECLOUD—2360—>

- ![修正圖示](../../assets/fix.svg) **改善雲端基礎結構擴充功能支援上的Adobe Commerce** — 將雲端基礎結構上Adobe Commerce中[composer.json檔案](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/overview)的guzzlehttp/guzzle封裝的最低版本需求降級為6.2版，以便`ece-tools`封裝與更多擴充功能相容。<!--MAGECLOUD-2205-->

- ![新圖示](../../assets/new.svg) **在建置階段期間套用自訂變更至您的Adobe Commerce應用程式** — 我們將建置階段分割成兩個獨立的程式，以便您能使用鉤點來套用自訂變更至產生的靜態內容，然後再封裝應用程式以進行部署。 _build：generate_&#x200B;程式會產生程式碼、套用修補程式，並產生靜態內容。 _build：transfer_&#x200B;程式會將產生的程式碼和靜態內容傳輸到最終目的地。 檢視[應用程式鉤點](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property).<!--MAGECLOUD-2363-->

- ![修正圖示](../../assets/fix.svg) **環境設定檢查** — 已改善環境設定的驗證，以在雲端基礎結構上建置和部署Adobe Commerce之前，警告客戶版本不相容和設定錯誤。

   - 已新增版本特定驗證，以識別不支援或已棄用的環境變數和值。<!--MAGECLOUD-2183-->

   - 新增Elasticsearch相容性檢查，以警告使用者有關Elasticsearch設定問題。 現在，如果伺服器上的Elasticsearch服務版本與Adobe Commerce不相容，部署就會失敗。 以前，即使Elasticsearch版本不相容，部署也會成功，導致網站部署後出現產品目錄問題。<!--MAGECLOUD-2389-->

     您可以透過[提交支援票證](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)來解決不相容問題，以將Elasticsearch升級為相容版本，或變更Adobe Commerce組態以指定相容的ElasticsearchPHP使用者端版本。

      - 若是Adobe Commerce 2.1.x版至2.2.2版，請將Elasticsearch升級至2.4版。

      - 若是Adobe Commerce 2.2.3版或更新版本，請將Elasticsearch升級至5.2版。

      - 如果您有Elasticsearch1.x或2.x，並且不想升級，請將composer.json中的Adobe CommerceElasticsearchPHP使用者端版本要求更新為`"elasticsearch/elasticsearch": "~2.0"`。

   - 改善環境變數的驗證，以識別在建置、部署和部署後階段期間可能導致衝突的組態設定。 例如，如果靜態內容部署的全域設定與組建或部署階段的設定衝突，則在安裝和升級過程中會顯示警告訊息。<!--MAGECLOUD-2156-->

- ![修正圖示](../../assets/fix.svg) **環境變數更新** — 已變更下列環境變數：

   - **[SKIP_HTML_MINIFICATION全域變數](../environment/variables-global.md#skip_html_minification)** — 將預設值變更為`true`以啟用隨選HTML內容縮制，這可在部署到中繼和生產環境時將停機時間縮到最少。 零停機部署需要此設定。<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES部署變數](../environment/variables-deploy.md#clean_static_files)** — 新增管理在建置階段根據CLEAN_STATIC_FILES環境變數設定產生的靜態內容之清除靜態檔案處理的功能。 先前，在建置階段產生的靜態內容檔案一律會被清除。<!--MAGECLOUD-1506-->

- ![修正圖示](../../assets/fix.svg) **記錄** — 進行下列變更，以改善記錄訊息並減少記錄大小：

   - 部署失敗記錄專案現在包含來自造成失敗的作業的命令輸出，即使您的環境設定未指定偵錯層級記錄亦然。 檢視[`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - 已新增部署失敗的記錄，因為檔案系統處於唯讀狀態，所以無法正確產生某些擴充功能所需的產生處理站時，就會發生部署失敗。<!--MAGECLOUD-2209-->

   - 減少部署記錄檔大小，並修正使用互動式進度列的安裝命令所導致的格式問題。<!--MAGECLOUD-2402-->

   - 已消除不必要的詳細資訊，並更新某些記錄陳述式的優先順序層級。<!--MAGECLOUD-2227-->

- ![修正圖示](../../assets/fix.svg) **Cron特定修正**—

   - 已將歷程記錄存留期的預設cron工作組態設定從3d （4320分鐘）變更為1h （60分鐘），以防止效能問題和cron佇列過快填入時可能發生的部署失敗。<!--MAGECLOUD-2427-->

   - 改善部署階段的cron工作管理程式，以防止資料庫鎖定和其他嚴重問題。 現在，所有cron工作會在部署階段期間停止，並在部署完成後重新啟動。<!--MAGECLOUD-2445-->

   - 修正Adobe Commerce 2.2.0版和更新版本中，cron工作所啟動之排程消費者的鎖定機制問題，以防止cron工作啟動重複消費者。<!--MAGECLOUD-2464-->

- ![修正圖示](../../assets/fix.svg)修正部署程式期間參考壓縮檔案時，[靜態內容壓縮程式](../environment/variables-intro.md) (`gzip`)發生`not overwritten`和`no such file or directory`錯誤的問題。<!-- MAGECLOUD-2182-->

- ![修正圖示](../../assets/fix.svg)修正了在傾印程式期間，如果未指定存放區地區設定，`php ./vendor/bin/ece-tools config:dump`命令無法從`config.php`檔案移除多餘區段的問題。 現在您可以輕鬆在環境之間移動設定檔案。 更新至`ece-tools` v2002.0.13後，使用改良的`config:dump`命令重新產生較舊的`config.php`檔案。 檢視存放區設定的[組態管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure-store/store-settings).<!--MAGECLOUD-2444-->

- ![修正圖示](../../assets/fix.svg)修正當`.magento/routes.yaml`檔案中的路由設定從[apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/)網域重新導向至`www`網域時，在部署階段期間導致錯誤的問題。<!--MAGECLOUD-2556-->

- ![修正圖示](../../assets/fix.svg)修正[`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration)變數的`_merge`選項問題，若您未在更新的`.magento.env.yaml`組態檔中包含`engine`引數，會導致不正確的合併結果。 現在，合併作業只會正確覆寫您在更新的`.magento.env.yaml`中指定的值，而不需要您設定`engine`引數。<!--MAGECLOUD-2520-->

- ![修正圖示](../../assets/fix.svg)修正Redis設定問題，該問題導致在雲端基礎結構2.2.1版及更新版本上無法正確啟用Adobe Commerce的工作階段鎖定，進而造成效能緩慢及逾時。 現在預設會停用工作階段鎖定。 此問題是由於Redis工作階段處理常式封裝的1.3.4版中引入的`disable_locking`引數預設行為變更所導致。 請參閱[colimollenhour/php-redis-session-abstract封裝](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![新圖示](../../assets/new.svg) **Docker Compose for Cloud** — 新增命令 — `docker:build` — 以從雲端`ece-tools`存放庫產生[Docker Compose](https://developer.adobe.com/commerce/cloud-tools/docker/configure/)設定。<!-- MAGECLOUD-2250 -->

- ![新圖示](../../assets/new.svg) **變更地區設定** — 現在您可以變更存放區地區設定，而不需匯出及匯入組態程式。 當應用程式處於生產狀態且已啟用SCD_ON_DEMAND時，即可使用存放區和管理程式語言環境選項。<!-- MAGECLOUD-2019 -->

- ![新圖示](../../assets/new.svg) <!-- MAGECLOU-1998 -->**網站地圖和Robots** — 已建立[工作流程](../store/robots-sitemap.md)以新增`robots.txt`檔案並產生單一網域組態的`sitemap.xml`檔案，而不需要變更基礎結構。

- ![新圖示](../../assets/new.svg) **精靈** — 已新增兩個[精靈](../deploy/smart-wizards.md)，協助您進行雲端設定：<!-- MAGECLOUD-1910 -->

   - `ideal-state` — 設定最理想的狀態，將部署停機時間降到最低

   - `master-slave` — 設定資料庫和Redis的負載平衡

- ![新圖示](../../assets/new.svg) **模組重新整理** — 新增雲端命令 — `module:refresh` — 以啟用已停用或未明確啟用的模組，類似於在建置期間自動完成的方式。<!-- MAGECLOUD-1521 -->

- ![新圖示](../../assets/new.svg)已新增在[快取](../environment/variables-deploy.md#cache_configuration)、[工作階段](../environment/variables-deploy.md#session_configuration)、[佇列](../environment/variables-deploy.md#queue_configuration)及[搜尋](../environment/variables-deploy.md#search_configuration)組態中，選擇合併或覆寫服務組態的功能。<!-- MAGECLOUD-2105 -->`_merge`

- ![新圖示](../../assets/new.svg) **環境設定範例檔案** — 我們已將`.magento.env.yaml`範例檔案新增至ECE-Tools封裝，其中包含每個環境變數的詳細說明和可能值。<!-- MAGECLOUD-1908 -->

   - 我們也新增了`.magento.env.yaml`設定的深層驗證，以防止部署程式因未預期的值而失敗。 失敗發生時，您現在會收到詳細的錯誤訊息，開頭為： `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![新圖示](../../assets/new.svg)已新增下列&#x200B;[**環境變數**](../environment/variables-intro.md)：

   - 現在您可以使用新的[SCD_MATRIX](../environment/variables-deploy.md#scd_matrix)環境變數為每個佈景主題定義多個地區設定，這會減少要部署佈景主題檔案的數量。<!-- MAGECLOUD-1501 -->

   - 已新增[DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration)環境變數，以自訂您的資料庫連線以進行部署。<!-- MAGECLOUD-2047 -->

   - 新的[MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level)變數會覆寫所有輸出資料流的最低記錄層級，而不會變更程式碼。<!-- MAGECLOUD-2129 -->

- ![修正圖示](../../assets/fix.svg)修正造成部署與部署後階段之間停機的問題。 現在，部署後階段在部署階段結束後&#x200B;_立即_&#x200B;開始。

- ![修正圖示](../../assets/fix.svg)修正未從排程中清除成功cron工作（具有`status = success`的工作）的問題。<!-- MAGECLOUD-2268 -->

- ![修正圖示](../../assets/fix.svg)修正`post_deploy`連結在部署階段而非專案的部署後階段中清除快取的問題。<!-- MAGECLOUD-2113 -->

- ![修正圖示](../../assets/fix.svg)修正搭配多個地區設定使用SCD時發生的問題，這會在每個地區設定中產生相同的`js-translation.json`檔案。<!-- MAGECLOUD-2034 -->

- ![修正圖示](../../assets/fix.svg)已最佳化`ece-tools`封裝中的`db:dump`命令，以避免鎖定資料表並提高速度。<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>2.2.4相容性需要ECE-Tools 2002.0.11版。

- ![新圖示](../../assets/new.svg) **設定與非主節點的唯讀連線** — 此版本新增設定與非主節點的唯讀連線，以接收唯讀流量的功能（針對[MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)）。<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection)和<!--MAGECLOUD-143 -->

- ![新圖示](../../assets/new.svg) **設定精靈** — 已新增精靈，以協助驗證靜態內容部署的設定。 請參閱[智慧型精靈](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![新圖示](../../assets/new.svg) **Symfony主控台支援** — 已新增支援Adobe Commerce 2.3的Symfony主控台4。<!-- MAGECLOUD-1966 -->

- ![修正圖示](../../assets/fix.svg) **Cron排程最佳化** — 改善佇列管理並增強記錄功能，以協助偵錯cron相關問題。<!-- MAGECLOUD-1607 -->

- 如果`ADMIN_EMAIL`或`ADMIN_USERNAME`值與現有的系統管理員帳戶相同，![修正圖示](../../assets/fix.svg)部署驗證會失敗。<!-- MAGECLOUD-1221 -->

- ![修正圖示](../../assets/fix.svg)已移除2.2.x版本的SOLR支援。 2.1.x版本仍可啟用SOLR。<!-- MAGECLOUD-1282 -->

- ![修正圖示](../../assets/fix.svg) PRO專案的測試與生產環境第一次安裝現在包含不同的Elasticsearch索引首碼，以防止識別屬於每個環境的記錄時可能發生衝突。<!-- MAGECLOUD-1489 -->

- ![修正圖示](../../assets/fix.svg)修正了在靜態內容部署期間中斷舊版架構的建置階段的問題。<!-- MAGECLOUD-2021 -->

- ![修正圖示](../../assets/fix.svg) **Cron特定改善** — 重新作業cron實作：<!-- MAGECLOUD-1607 -->

   - 修正造成cron佇列快速填滿的問題。 現在能以更可靠的方式清除過時的cron工作。

   - 重新組織cron作業順序，讓不同執行緒中的所有作業在一般群組之前啟動。

   - 改善記錄功能，以便更妥善協助偵錯cron問題。

   - **注意** — 此版本解決許多cron相關問題。 如果您目前在&#x200B;_m2-hotfix_&#x200B;中使用一些與cron相關的修補程式，請移除它們。

- ![修正圖示](../../assets/fix.svg) **特定於SCD的改進**—

   - 您可以在&#x200B;_建置_&#x200B;和de_ploy階段使用`VERBOSE_COMMANDS`和`SCD_COMPRESSION_LEVEL`環境變數。<!-- MAGECLOUD-1819 -->

   - 修正當發生`SCD_COMPRESSION_LEVEL`環境變數的意外值時，導致部署失敗並出現隨機錯誤的問題。 改善設定驗證，以提供有意義的通知。 如需可接受的值，請參閱[`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level)。<!-- MAGECLOUD-2043 -->

   - 修正`SCD_COMPRESSION_LEVEL`環境變陣列態流程的行為，讓覆寫功能如預期般運作。<!-- MAGECLOUD-2044 -->

   - 修正無法在`.magento.env.yaml`檔案&#x200B;_部署_&#x200B;階段中設定`SCD_THREADS`環境變數的問題。<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![新圖示](../../assets/new.svg) **靜態內容部署(SCD)** — 有新的替代部署程式可在要求時產生靜態內容（隨選）。 這會產生最關鍵的資產，以減少停機時間，並改善快取處理。<!-- MAGECLOUD-1285 -->

   - **新環境變數** — 已新增`SCD_ON_DEMAND`全域環境變數，以便在要求時產生靜態內容。<!-- MAGECLOUD-1738 -->

   - **部署後鉤點** — 已為`.magento.app.yaml`檔案新增`post_deploy`鉤點，該鉤點會清除快取，並在&#x200B;_容器開始接受連線後，預先載入（加溫）快取_。 它僅適用於在[!DNL Cloud Console]中包含測試和生產環境的Pro專案以及入門專案。 雖然不需要，但此變數可與`SCD_ON_DEMAND`環境變數搭配使用。<!-- MAGECLOUD-1788 -->

- ![新圖示](../../assets/new.svg) **最佳化** — 在部署期間最佳化移動或複製檔案，以提高部署速度並降低檔案系統的負載。<!-- MAGECLOUD-1842 -->

- ![新圖示](../../assets/new.svg) **部署記錄** — 已新增啟用Syslog和Graylog延伸記錄格式(GELF)處理常式的功能，以便在部署過程中輸出記錄。 請參閱[記錄處理常式](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![新圖示](../../assets/new.svg)已新增下列&#x200B;[**環境變數**](../environment/variables-intro.md)：

   - `CRYPT_KEY` — 行動資料庫時，提供密碼編譯金鑰給其他環境。<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_略過複製`var/view_preprocessed`目錄中的靜態檢視檔案並在要求時產生縮制HTML的全域_&#x200B;環境變數。<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_全域_&#x200B;環境變數，以便在要求時產生靜態內容。<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` — 您可以列出要用來預先載入快取的頁面。 可用於新的[部署後變數](../environment/variables-post-deploy.md)。

- ![修正圖示](../../assets/fix.svg)修正本機套用的修補程式中斷執行個體部署的問題。 現在，ECE-Tools可以偵測到已套用修補程式。<!-- MAGECLOUD-982 -->

- ![修正圖示](../../assets/fix.svg)修正JavaScript套件組合與GZIP功能之間的衝突。 現在，這些功能可正確搭配使用。<!-- MAGECLOUD-1735 -->

- ![修正圖示](../../assets/fix.svg)修正使用舊版PHP 7.0.x時，導致ECE-Tools CLI命令失敗的問題。<!-- MAGECLOUD-1744 -->

- ![修正圖示](../../assets/fix.svg)修正無法在多個執行緒中使用壓縮策略的靜態內容部署問題。<!-- MAGECLOUD-1822 -->

- ![修正圖示](../../assets/fix.svg)修正造成管理員登入延遲的Redis工作階段鎖定問題。 此外，2.1.x.<!-- MAGECLOUD-1853 -->也有修正可用

## v2002.0.9

>[!NOTE]
>
>您必須[升級雲端基礎結構中繼資料上的Adobe Commerce](../dev-tools/install-package.md#update-the-metapackage)，才能取得此專案和所有未來的更新。

- ![新圖示](../../assets/new.svg) **ece-tools** — 此`ece-tools`套件現在支援Adobe Commerce 2.1.x。<!-- MAGECLOUD-1086 -->

- ![新圖示](../../assets/new.svg) **Redis組態** — 您現在可以使用環境變數[設定Redis](../environment/variables-deploy.md#cache_configuration)頁面以及預設快取和Redis工作階段存放區。<!-- MAGECLOUD-1552 -->

- ![新圖示](../../assets/new.svg) **搜尋、AMQP和Redis服務改善** — 我們已整合服務設定流程，讓所有服務的運作方式都相同。 不再支援手動編輯`env.php`檔案來設定服務。 您必須改用環境變數或`.magento.env.yaml`檔案。<!-- MAGECLOUD-1437 -->

- ![修正圖示](../../assets/fix.svg) **環境變數**—

   - `env:STATIC_CONTENT_THREADS`的使用已過時，並將在未來版本中移除。 請改用[SCD_THREADS](../environment/variables-deploy.md#scd_threads)。<!-- MAGECLOUD-1507 -->

   - `STATIC_CONTENT_EXCLUDE_THEMES`環境變數已過時。 您必須改用`SCD_EXCLUDE_THEMES`環境變數。<!-- MAGECLOUD-1640 -->

- ![修正圖示](../../assets/fix.svg) **記錄** — 我們簡化內建修補作業的相關記錄。<!-- MAGECLOUD-1674 -->

- ![修正圖示](../../assets/fix.svg)我們已移除`developer`模式支援和`APPLICATION_MODE`環境變數，因為這些會造成非預期的行為。<!-- MAGECLOUD-1615 -->

- ![修正圖示](../../assets/fix.svg)我們已修正造成與Redis相關的靜態內容部署失敗的問題。 現在，多執行緒靜態內容部署已如預期般執行。<!-- MAGECLOUD-1630 -->

- ![修正圖示](../../assets/fix.svg)我們已修正使用者無法儲存對Admin中設定欄位的修改的問題，這些欄位在執行`app:config:dump`命令後標籤為敏感。<!-- MAGECLOUD-1175 -->

- ![修正圖示](../../assets/fix.svg)我們已新增對舊版`symfony/yaml`的支援，以修正與某些尚未與最新版本相容的套件之間的衝突。<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>我們已將`vendor/magento/ece-patches`與此版本中的`vendor/magento/ece-tools`合併。 您不再需要另外更新`vendor/magento/ece-patches`套件。

**新功能：**

- **已改善記錄**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - 我們改良了記錄訊息功能，以便在建置或部署程式覆寫環境變數時提供更好的解釋。
   - 您現在可以即時檢視安裝和升級進度。 追蹤`install_update.log`檔案以檢視進度。 例如，

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **新cron命令** — 您現在可以解除鎖定特定的cron工作，而不是使用[`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451)命令停止並重新啟動所有工作。 在2.1.<!-- MAGECLOUD-1367 -->中無法使用

- **整合組態檔** — 您現在可以使用[`.magento.env.yaml`](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/env/configure-env-yaml)檔案來設定組建和部署階段。<!-- MAGECLOUD-1369 -->

- **備份組態檔** — 部署程式現在會在部署後自動建立`app/etc/env.php`與`app/etc/config.php`組態檔的備份。 我們也新增了[新的CLI命令](https://support.magento.com/hc/en-us/articles/360033182871)，以便從備份還原這些組態檔。<!-- MAGECLOUD-1372 -->

- **疑難排解驗證錯誤** — 我們已變更當`config.php`未包含足夠的資料以進行靜態內容部署時，您必須使用的命令來解決驗證錯誤。 以前，錯誤訊息指示您執行`bin/magento app:config:dump`。 現在，您必須執行`php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **新環境變數** — 您現在可以使用環境變數將自訂[搜尋](../environment/variables-deploy.md#search_configuration)和[以AMQP為基礎的](../environment/variables-deploy.md#queue_configuration)服務連線到您的網站。<!-- MAGECLOUD-1410 -->

- 我們實作智慧型修補。 現在，套件不是根據Adobe Commerce在雲端基礎結構版本上套用修補程式，而是根據修補的套件版本套用。<!--MAGECLOUD-1090-->

**已解決問題：**

- 我們已修正造成建置錯誤的記錄問題。<!-- MAGECLOUD-1162 -->

- 我們已修正以互動模式執行部署時，造成逾時例外的問題。<!-- MAGECLOUD-1389 -->

- 我們已修正使用壓縮策略產生靜態內容時導致錯誤的問題。 在2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->中無法使用

- 我們修正了部署指令碼無法正確識別中繼和生產環境的問題。<!-- MAGECLOUD-1493 -->

- 我們已修正導致網路問題中斷資料庫連線，並在安裝和升級過程中導致失敗的問題。<!-- MAGECLOUD-1520 -->

- 已修正您無法多次使用`app:config:dump`匯出組態檔的問題。 在2.1.<!--  MAGECLOUD-1567  -->中無法使用

- 我們已修正Redis工作階段&#x200B;_鎖定_&#x200B;造成&#x200B;_管理員_&#x200B;登入延遲的問題。 在2.1.<!--  MAGECLOUD-1582  -->中無法使用

- 我們已修正與版本設定相關的實作問題，此問題會導致與其他以撰寫器為基礎的修補模組發生衝突。<!-- MAGECLOUD-1450 -->

- 我們已修正匯入期間造成PHP記憶體問題的問題。<!-- MAGECLOUD-1310 -->

- 已移除修補程式；修正`colinmollenhour/credis` v1.6中的錯誤，以啟用雲端基礎結構2.2.1上的Adobe Commerce支援。在2.1.<!-- MAGECLOUD-1033 -->中無法使用

## v2002.0.7

**已解決問題：**

- 我們已移除`var/view_preprocessed`個符號連結，以修正造成JavaScript縮制衝突的問題。<!-- MAGECLOUD-1454 -->

## v2002.0.6

**已解決問題：**

- 已修正檔案或目錄名稱包含空格時導致`gzip`錯誤的問題。<!-- MAGECLOUD-1413 -->

- 我們修正了部署指令碼無法正確辨識及啟用模組相依性的問題。<!-- MAGECLOUD-1424 -->

## v2002.0.5

**新功能：**

- **使用環境變數設定cron消費者** — 您現在可以使用新的`CRON_CONSUMERS_RUNNER`環境變數設定cron消費者。

- **組態掃描** — 我們現在會在建置/部署程式期間掃描重要元件，並在掃描失敗時停止該程式，避免網站處於維護模式而造成不必要的停機時間。

- **建置/部署通知** — 我們新增了一個組態檔，您可用來[設定Slack和/或電子郵件通知](../environment/set-up-notifications.md)，以在您的所有環境中建置/部署動作。

- **靜態內容壓縮** — 我們現在會在建置和部署階段使用[gzip](https://www.gnu.org/software/gzip/)來壓縮靜態內容。 此壓縮搭配Fastly壓縮，有助於縮小存放區大小並提高部署速度。 如有必要，您可以使用[組建選項](../environment/variables-build.md)或[部署變數](../environment/variables-deploy.md)來停用壓縮。 如需詳細資訊，請參閱下列主題：

   - [應用程式環境變數](../application/variables-property.md)

   - [靜態內容部署效能](../deploy/static-content.md)

   - [部署程式](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)

- **組態管理** — 我們現在會在建置階段期間，在您的Git存放庫中自動產生`app/etc/config.php`檔案（如果尚未存在）。 自動產生的檔案僅包含模組和副檔名的清單。 如果檔案已經存在，則建置階段會照常繼續。 如果您稍後再執行[組態管理](../store/store-settings.md)，這些命令會更新檔案，而不需要其他步驟。 如需詳細資訊，請參閱[部署程式](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/deploy/best-practices)。

- **資料庫傾印** — 我們新增了`magento/ece-tools` CLI命令，以便在所有環境中建立資料庫傾印。 對於Pro計畫生產環境，這個命令只會從三個高可用性節點中的一個轉儲，因此在轉儲期間寫入不同節點的生產資料可能不會被複製。 我們建議在生產環境中執行資料庫傾印之前，將應用程式置於維護模式。 如需詳細資訊，請參閱[備份管理](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/storage/snapshots)。

- **已解除Cron間隔限制** — 針對us-3、eu-3和ap-3區域中布建的所有環境的預設Cron間隔為1分鐘。 所有其他地區的預設cron間隔為5分鐘（適用於Pro整合環境）和1分鐘（適用於Pro測試和生產環境）。 若要修改現有的cron工作，請在`.magento.app.yaml`中編輯您的設定，或建立生產/測試環境的支援票證。 如需詳細資訊，請參閱[設定cron工作](../application/crons-property.md#set-up-cron-jobs)。

**已解決問題：**

- 我們修正了在靜態內容部署之前，由於部署程式叫用`cache-clean`作業，而導致部署時間較長的問題。<!-- MAGECLOUD-1327 -->

- 我們修正了在生產環境中部署的靜態內容產生步驟期間造成錯誤的問題。<!-- MAGECLOUD-1322 -->

- 已修正無法將某些`magento/ece-tools`命令記錄到`stderr`.<!-- MAGECLOUD-1264 -->的問題

- 已修正無法在分支中更新`env.php`中的基底URL值的問題。<!-- MAGECLOUD-1242 -->

- 我們已修正造成`magento setup:install`命令新增不安全的首碼(`http://`)來保護基底URL的問題。<!-- MAGECLOUD-1171 -->

- 我們已修正修補程式錯誤無法導致部署失敗的問題。<!-- MAGECLOUD-1170 -->

- 我們已修正無法套用修補程式時，`ece-tools`無法停止執行並擲回例外狀況的問題。<!-- MAGECLOUD-1152 -->

- 我們已修正在Admin中啟用HTML縮制後載入店面時造成錯誤的問題。<!-- MAGECLOUD-1138 -->

## v2002.0.4

**已解決問題：**

- 您現在可以透過SSH存取，在所有環境中使用CLI命令[手動重設停滯的cron工作](https://support.magento.com/hc/en-us/articles/360033099451)。 部署程式會自動重設cron工作。<!-- MAGECLOUD-1355 -->

## v2002.0.3

**已解決問題：**

- 我們已修正由於Redis讀取/寫入時間過長而導致頁面逾時的問題。 您現在可以在Redis設定中使用`disable_locking`引數來避免此問題。<!-- MAGECLOUD-1311 -->

## v2002.0.2

**已解決問題：**

- [!DNL RabbitMQ]設定處理序現在會自動取得所有必要的引數。<!-- MAGECLOUD-1246 -->

## v2002.0.1

**新功能：**

- 雲端基礎結構上的Adobe Commerce現在支援範圍和[靜態內容部署策略](https://experienceleague.adobe.com/en/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy)。 我們已為靜態內容部署策略新增預設設定為`quick`的`–s`引數。 您可以使用環境變數[SCD_STRATEGY](../environment/variables-deploy.md)來自訂這些策略，並將這些策略用於您的建置和部署動作。 此變數支援選項`standard`、`quick`或`compact`。 如果您選取`compact`，我們會以`1`覆寫`STATIC_CONTENT_THREADS`值，這會減慢部署速度，尤其是在生產環境中。 在2.1.<!--- MAGECLOUD-1057 -->中無法使用

- 我們在環境上建立了記錄檔，以擷取及編譯建置和部署動作。 `var/log/cloud.log`檔案位於根應用程式目錄中。<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**已解決問題：**

- 已重構`ece-tools`套件，使其與雲端基礎結構2.2.0和更新版本上的Adobe Commerce相容。<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- 已修正無法套用修補程式時，`ece-tools`無法停止執行並擲回例外狀況的問題。<!-- MAGECLOUD-1186 -->

- 我們已修正導致在建置期間略過相依性插入(di)編譯時擲回例外狀況的問題。<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- 我們已修正導致部署程式覆寫`env.php`檔案中自訂Redis設定的問題。<!-- MAGECLOUD-1019 -->

- 我們已修正因預設的安全管理員停用而導致重新導向回圈的問題。<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>此套件不再與雲端基礎結構上的其他Adobe Commerce版本相容，因此&#x200B;**不應使用**。

### 初始發行

雲端基礎結構2.2.0上Adobe Commerce的`ece-tools`初始版本。
