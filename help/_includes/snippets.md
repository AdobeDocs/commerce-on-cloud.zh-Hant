---
source-git-commit: adcdcb663db466953f085f365a38de8301840ba4
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---
# 雲端代碼片段

## Elasticsearch警告 {#elasticsearch-support}

>[!WARNING]
>
>雲端基礎結構上的Adobe Commerce不支援Elasticsearch 7.11和更新版本。 Adobe Commerce版本2.3.7-p3、2.4.3-p2以及2.4.4和更新版本支援OpenSearch服務。 內部部署安裝仍支援Elasticsearch。

## 增強型整合 {#enhanced-integration-envs}

>[!NOTE]
>
>在2020年6月5日之前布建的專案具有多個較小的整合環境。 如果您需要更大的整合環境以進行測試和開發，請要求升級至增強型整合環境。 如需詳細資訊，請參閱[Adobe Commerce說明中心](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html)中的&#x200B;_整合環境要求_&#x200B;文章。

## 合併選項 {#merge-options}

依照預設，部署程式會覆寫`env.php`檔案中的所有設定；不過，您可以選擇合併服務組態的一或多個值，而不覆寫所有值。

將`_merge`選項設定為下列其中一項：

- `true`—**將設定的服務值與環境變數值合併**。
- `false`—**以環境變數值覆寫**&#x200B;設定的服務值。

## 私人存放庫 {#private-repository}

>[!NOTE]
>
>Adobe強烈建議您在雲端基礎結構專案上為Adobe Commerce使用私人存放庫，以保護任何專屬資訊或開發工作，例如擴充功能和敏感設定。

## 專業自助服務警告 {#pro-self-service-warning}

>[!WARNING]
>
>有些&#x200B;**Pro專案**&#x200B;需要支援票證，才能更新`routes.yaml`檔案中的路由設定和`.magento.app.yaml`檔案中的cron設定。 Adobe建議您在整合環境中更新及測試YAML設定檔，然後將變更部署至測試環境。 如果您在重新部署後未將變更套用至測試網站，且記錄檔中沒有相關的錯誤訊息，則您&#x200B;**必須** [提交說明嘗試的組態變更的Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 在票證中包含任何更新的YAML設定檔案。

## Pro服務支援 {#pro-update-service}

>[!BEGINSHADEBOX]

- 對於Pro專案，您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，才能僅在[和](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/service/services-yaml.html)環境中安裝或更新`Staging`服務`Production`。

- 指示所需的服務變更，包括更新的`.magento.app.yaml`和`services.yaml`檔案，並在票證中說明PHP版本。 如需自行變更PHP版本、擴充功能或環境設定，請參閱[應用程式組態](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/php-settings.html)中的&#x200B;_PHP設定_。

- 若要變更即時生產環境（**僅限Pro**），至少需要48小時的通知。 這可讓雲端基礎結構團隊有充足的時間來調配資源並進行安全升級。 通知期間從基礎架構團隊認可請求並安排升級（不包括週末）開始。 例如，若要在星期一完成服務升級，必須在星期三收到排程升級的確認。 在需求尖峰期間，處理您的請求可能需要更多時間。

>[!ENDSHADEBOX]

## 專業備份 {#pro-backups}

>[!TIP]
>
>在Pro測試和生產環境中，您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以擷取票證中的特定備份，並註明日期、時間和時區。
>
>Adobe **不會**&#x200B;從自動備份還原任何環境。 請參閱[從測試或生產還原資料庫快照](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html)，以取得選擇還原測試或生產快照的方法。

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
>[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以變更Pro生產和中繼環境上的服務組態。

## 服務變更 {#service-change-tip}

>[!TIP]
>
>初始服務安裝之後，您可以更新`services.yaml`和`.magento.app.yaml`組態檔，以變更已安裝服務的軟體版本。 請參閱[變更服務版本](/help/cloud-guide/services/services-yaml.md#change-service-version)以取得升級或降級服務的指引。

## 停滯的部署提示 {#stuck-deployment-tip}

>[!TIP]
>
>若要取得停滯部署的協助，請使用[Adobe Commerce說明中心](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html)中的&#x200B;_Commerce部署疑難排解員_。

## ECE-Tools更新 {#ece-tools-package}

>[!NOTE]
>
>如果您在不包含`ece-tools`套件的雲端基礎結構上使用Adobe Commerce版本，則您必須對您的雲端專案執行[一次性升級](/help/cloud-guide/dev-tools/install-package.md)以移除已棄用的套件。 如果您目前使用`ece-tools`套件，而且需要更新它，請參閱[更新ECE-Tools套件](/help/cloud-guide/dev-tools/update-package.md)。

## 升級秘訣 {#upgrade-tip}

>[!TIP]
>
>在開始升級或修補程式之前，請從整合環境建立使用中分支，並將新分支簽出至您的本機工作站。 將分支專用於升級或修補程式，有助於避免干擾您正在進行的工作。

<!-- Fastly-related snippets begin -->

## 管理員登入 {#admin-login-step}

1. [登入](/help/get-started/onboarding.md#access-your-admin-panel)管理員。

## 自動部署自訂VCL程式碼片段 {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>您可以新增程式碼片段至環境中的`$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom`目錄，而不必手動上傳自訂VCL程式碼片段。 當您在Commerce Admin中按一下&#x200B;_將VCL上傳至Fastly_&#x200B;時，此目錄中的程式碼片段會自動上傳。 請參閱Magento 2檔案之Fastly CDN模組中的[自動自訂VCL片段部署](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment)。

<!-- Fastly-related snippets end -->
