---
source-git-commit: 67ed09e3b7c5f5218407b6648e8ca2c32933bbda
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---
# 雲端代碼片段

## Elasticsearch警告 {#elasticsearch-support}

>[!WARNING]
>
>雲端基礎結構上的Adobe Commerce不支援Elasticsearch 7和更新版本。 Adobe Commerce 2.4.4和更新版本支援OpenSearch服務。

## 增強型整合 {#enhanced-integration-envs}

>[!NOTE]
>
>在2020年6月5日之前布建的專案具有多個較小的整合環境。 如果您需要更大的整合環境以進行測試和開發，請要求升級至增強型整合環境。 如需詳細資訊，請參閱&#x200B;_Adobe Commerce說明中心_&#x200B;中的[整合環境要求](https://experienceleague.adobe.com/zh-hant/docs/experience-cloud-kcs/kbarticles/ka-27242)文章。

## 合併選項 {#merge-options}

依照預設，部署程式會覆寫`env.php`檔案中的所有設定；不過，您可以選擇合併服務組態的一或多個值，而不覆寫所有值。

將`_merge`選項設定為下列其中一項：

- `true`—**將設定的服務值與環境變數值合併**。
- `false`—**以環境變數值覆寫**&#x200B;設定的服務值。

## 私人存放庫 {#private-repository}

>[!NOTE]
>
>Adobe建議您在雲端基礎結構專案上為Adobe Commerce使用私人存放庫，以保護任何專屬資訊或開發工作，例如擴充功能和敏感設定。

## 專業自助服務警告 {#pro-self-service-warning}

>[!WARNING]
>
>有些&#x200B;**Pro專案**&#x200B;需要Adobe支援的協助，才能更新`routes.yaml`檔案中的路由設定和`.magento.app.yaml`檔案中的cron設定。 Adobe建議先在整合環境中進行及驗證所有YAML設定變更，然後將其部署至中繼環境。
>
>
>如果重新部署後您的變更未反映在測試網站上，且記錄檔中沒有相關的錯誤訊息，則您&#x200B;**必須** [提交Adobe Commerce支援票證](https://experienceleague.adobe.com/zh-hant/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。 在票證中，清楚說明您嘗試的組態變更，並在票證中附加任何更新的YAML組態檔。

## 專業備份 {#pro-backups}

>[!TIP]
>
>若要在Pro測試和生產環境中擷取特定備份，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/zh-hant/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)，並在票證中註明日期、時間和時區。
>
>Adobe **不會**&#x200B;從自動備份還原任何環境。 請參閱[從測試或生產還原資料庫快照](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production)，以取得選擇還原測試或生產快照的方法。

## 重新部署警告 {#redeploy-warning}

>[!WARNING]
>
>當您執行環境的合併、推播或同步處理時，或當您觸發手動重新部署時（期間的[!DNL Commerce]應用程式處於維護模式），部署程式即會開始。 在生產環境中，Adobe建議您在離峰時間完成這項工作，以避免服務中斷。

## 路由預留位置 {#route-placeholder}

>[!NOTE]
>
>下列路由組態範例使用帶有預留位置的路由範本。 `{default}`預留位置代表為您的網站設定的預設網域。 如果您的專案有多個網域，請使用`{all}`預留位置來設定預設網域和所有別名的路由。 請參閱[設定路由](/help/cloud-guide/routes/routes-yaml.md)。

## SCD時間 {#scd-timing-warning}

>[!WARNING]
>
>如果您在部署後應用程式中的靜態內容檔案出現問題（例如遺失自訂主題檔案），請將最大預期執行時間增加至900秒或以上。

## 以案例為基礎的部署 {#scenarios}

>[!NOTE]
>
>透過[!DNL ECE-Tools] 2002.1.0和更新版本，您可以使用情境式部署功能，在雲端基礎結構專案上自訂Adobe Commerce的建置、部署和後續部署程式。 請參閱[以案例為基礎的部署](/help/cloud-guide/deploy/scenario-based.md)。

## 第二次分段 {#second-staging}

>[!NOTE]
>
>有些專案需要更複雜的開發工作流程。 為了支援此需求，Adobe提供[額外的中繼環境](/help/cloud-guide/test/second-staging.md)，作為您雲端基礎結構的附加選項。

## 服務指示 {#service-instruction}

使用下列指示在Pro整合環境與入門環境（包括`master`分支）上進行服務設定。

>[!NOTE]
>
>若要變更Pro生產和中繼環境上的服務組態，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/zh-hant/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。 如需排程需求與客戶可用性指引，請參閱&#x200B;_設定服務_&#x200B;中的[專業服務支援](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support)。

## 服務變更 {#service-change-tip}

>[!TIP]
>
>初始服務安裝之後，您可以更新`services.yaml`和`.magento.app.yaml`組態檔，以變更已安裝服務的軟體版本。 請參閱[變更服務版本](/help/cloud-guide/services/services-yaml.md#change-service-version)以取得升級或降級服務的指引。 此自助式方法不適用於Pro測試或生產環境 — 請參閱&#x200B;_設定服務_&#x200B;中的[Pro服務支援](https://experienceleague.adobe.com/en/docs/cloud-guide/services/services-yaml.md#pro-services-support)。

## 停滯的部署提示 {#stuck-deployment-tip}

>[!TIP]
>
>若要取得停滯部署的協助，請使用&#x200B;_Adobe Commerce說明中心_&#x200B;中的[Commerce部署疑難排解員](https://experienceleague.adobe.com/zh-hant/docs/experience-cloud-kcs/kbarticles/ka-29640)。

## ECE-Tools更新 {#ece-tools-package}

>[!NOTE]
>
>若要移除雲端基礎結構上不包含`ece-tools`套件的Adobe Commerce版本上已棄用的套件，您必須對您的雲端專案執行[一次性升級](/help/cloud-guide/dev-tools/install-package.md)。 如果您目前使用`ece-tools`套件，而且需要更新它，請參閱[更新ECE-Tools套件](/help/cloud-guide/dev-tools/update-package.md)。

## 升級秘訣 {#upgrade-tip}

>[!TIP]
>
>在開始升級或修補程式之前，請從整合環境建立使用中分支，並將新分支簽出至您的本機工作站。 將分支專用於升級或修補程式，有助於避免干擾您正在進行的工作。

## New Relic中的Valkey {#valkey-newrelic}

>[!NOTE]
>
>即使在移轉至Valkey後，New Relic仍可能會顯示Redis。
>
>即使在環境已移轉至Valkey後，預計New Relic仍會繼續將快取服務稱為Redis。
>
>Valkey是Redis的開放原始碼復本，而且有些工具和整合專案會繼續使用Redis命名來識別服務，而非不同的Valkey標籤。 此行為並不一定表示仍安裝Redis。

<!-- Fastly-related snippets begin -->

## 管理員登入 {#admin-login-step}

1. [登入](/help/get-started/onboarding.md#access-your-admin-panel)管理員。

## 自動部署自訂VCL程式碼片段 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>您可以新增程式碼片段至環境中的`$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom`目錄，而不必手動上傳自訂VCL程式碼片段。 當您在Commerce Admin中按一下&#x200B;_將VCL上傳至Fastly_&#x200B;時，此目錄中的程式碼片段會自動上傳。 請參閱Magento 2檔案之Fastly CDN模組中的[自動自訂VCL片段部署](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)。

<!-- Fastly-related snippets end -->
