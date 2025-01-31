---
title: 部署最佳實務
description: 探索在雲端基礎結構上部署Adobe Commerce的最佳作法。
feature: Cloud, Deploy, Best Practices
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# 部署最佳實務

將程式碼合併至遠端環境時，組建和部署指令碼會啟用。 這些指令碼使用環境[組態檔](../environment/overview.md)和應用程式程式碼來布建雲端基礎結構，並提供適當的資料和服務。 此外，這些指令碼也用於在雲端環境中安裝或更新Adobe Commerce應用程式、協力廠商服務和自訂擴充功能。

每個計畫的建置和部署流程都略有不同：

- **入門計畫** — 對於整合環境，每個使用中的分支都會建置並部署至完整環境，以進行存取和測試。 合併至`staging`分支後，請完整測試您的程式碼。 若要啟動您的網站，請將`staging`推送到`master`以部署到生產環境。 透過[!DNL Cloud Console]和CLI命令，您可以完整存取所有分支。

- **專業計畫** — 對於整合環境，每個使用中的分支都會建置並部署至完整環境，以進行存取和測試。 合併您的程式碼到`integration`分支再合併到中繼和生產環境。 您可以使用[!DNL Cloud Console]或使用SSH和`magento-cloud` CLI命令合併至測試和生產環境。

## 追蹤程式

您可以使用終端機或在部署過程中顯示的[!DNL Cloud Console]狀態訊息（`in-progress`、`pending`、`success`或`failed`）即時追蹤建置和部署動作。 您可以在記錄檔中檢視詳細資訊。 檢視[檢視記錄](../test/log-locations.md)。

如果您使用外部GitHub存放庫，作業記錄不會顯示在GitHub工作階段中。 不過，您仍然可以關注外部存放庫和[!DNL Cloud Console]介面中的活動。 請參閱[整合](../integrations/overview.md)。

>[!NOTE]
>
>在整合環境中，您無法從[!DNL Cloud Console]檢視部署記錄檔。 此功能僅適用於生產和測試環境。 不過，您可以使用[建置和部署](../test/log-locations.md#build-and-deploy-logs)記錄檔，檢視任何環境中每個部署階段的記錄。 如需疑難排解資訊，請參閱[部署錯誤參考](../dev-tools/error-reference.md)。

您可以使用New Relic](../monitor/track-deployments.md)啟用[追蹤部署，以監視部署事件並分析部署之間的效能。

## 建置和部署的最佳實務

檢閱部署流程的這些最佳實務和考量事項：

- **請確定您執行的是`ece-tools`封裝的最新版本**

  請參閱ECE-Tools](../release-notes/ece-tools-package.md)的[發行說明。

- **遵循建置和部署程式**

  確保您在每個環境中都有正確的程式碼，以避免在環境之間合併程式碼時覆寫設定。 例如，若要將設定變更套用至所有環境，請在部署至遠端整合環境之前，修改並測試本機環境中的變更。 然後，在部署到生產環境之前，在中繼環境中部署和測試變更。 從一個環境合併到另一個環境時，部署會覆寫環境中的所有程式碼，但環境特定的組態和設定除外。

- **在環境中使用相同的變數**

  這些變數的值可能因環境而異；不過，您通常在每個環境中都需要相同的變數。 檢視存放區設定的[組態管理](../store/store-settings.md)。

- **將敏感的設定值和資料保留在環境特定的變數中**

  這些值包含使用Cloud CLI、[!DNL Cloud Console]指定的變數，或新增至`env.php`檔案的變數。 請參閱[變數層級](../environment/variable-levels.md)。

- **確定所有程式碼都可在環境分支**&#x200B;中使用

  參照其他分支的程式碼（例如私人分支）可能會在建置和部署過程中造成問題。 例如，如果您從私人分支參考主題，則無法存取主題，且無法使用應用程式程式碼建置。

- **在疊代的分支中新增擴充功能、整合功能和程式碼**

  在本機進行並測試變更，推送到`integration`，然後推送到`staging`和`production`。 在將更新合併到下一個環境之前，測試和解決每個環境中的問題。 由於相依性，部分擴充功能和整合功能必須以特定順序啟用和設定。 在群組中新增和測試可讓您的建置和部署流程更容易，並有助於判斷發生問題的位置。

- **驗證服務版本和關聯性以及連線能力**

  確認應用程式可用的服務，並確定您使用的是最新的相容版本。 如需建議的版本，請參閱&#x200B;_安裝指南_&#x200B;中的[服務關係](../services/services-yaml.md#service-relationships)和[系統需求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)。

- **在部署到中繼和生產環境之前，先在本機和整合環境中測試**

  識別並修正本機與整合環境中的問題，以防止您部署至中繼與生產環境時的長時間停機時間。

  >[!TIP]
  >
  >有[個智慧精靈](../deploy/smart-wizards.md)命令可用來驗證您的雲端專案設定是否遵循建置和部署設定(包括靜態內容部署(SCD)策略)的最佳實務。

- **在本機和整合環境中完成測試後，在中繼環境中部署和測試**

  請參閱[測試和生產測試](../test/staging-and-production.md)。

- **檢查生產環境設定**

  在部署到生產之前，請完成以下任務：

   - 確保您可以使用[SSH](../development/secure-connections.md)連線到生產環境中的所有三個節點。

   - 確認索引子已設定為&#x200B;_排程更新_。 請參閱&#x200B;_擴充功能開發人員指南_&#x200B;中的[索引模式](https://developer.adobe.com/commerce/php/development/components/indexing/)。

   - 請更新生產程式碼中的任何環境特定變數、驗證服務可用性和相容性，並進行任何其他必要的設定變更，以準備環境。

- **監視部署程式**

  檢閱部署狀態訊息，並視需要緩解問題。 檢閱雲端[記錄檔](../test/log-locations.md#)的詳細記錄檔訊息。

## 整合建置和部署的五個階段

您的本機開發環境和整合環境中會發生下列階段。 對於Pro計畫，程式碼不會在這些初始階段部署到中繼或生產環境。

### 階段1：程式碼和設定驗證

當您初次設定專案時，[雲端基礎結構範本](https://github.com/magento/magento-cloud)會提供程式碼檔案的基礎。 此程式碼存放庫已複製至您的專案做為`master`分支。

- **對於入門者**—`master`分支是您的生產環境。
- **對於Pro**—`master`以整合環境的原始分支開始。

從`master`為您的自訂程式碼、擴充功能和模組，以及協力廠商整合建立分支。 有一個遠端整合環境，可在雲端中測試您的程式碼。

將程式碼從本機工作區推播至遠端存放庫時，一連串檢查和程式碼驗證會在建置和部署指令碼開始前完成。 內建Git伺服器會驗證您正在推送的內容並進行變更。 例如，如果您新增OpenSearch服務，內建的Git伺服器會驗證叢集的拓撲是否已適當地修改。

如果您在設定檔案中發生語法錯誤，Git伺服器會拒絕推送。 請參閱[保護性區塊](../development/protective-block.md)。

此階段也會執行`composer install`以擷取相依性。

### 階段2：建置

>[!NOTE]
>
>在建置階段，網站未處於維護模式，如果發生錯誤或問題，網站也不會停機。 它只會建置自上次建置以來已變更的程式碼。

此階段會建置程式碼基底，並在`.magento.app.yaml`的`build`區段中執行鉤點。 預設的組建掛接是`php ./vendor/bin/ece-tools`命令，並執行下列動作：

- 在`vendor/magento/ece-patches`中套用修補程式，以及在`m2-hotfixes`中套用選擇性專案特定修補程式
- 使用`bin/magento setup:di:compile`重新產生程式碼和[相依性插入](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)組態（亦即`generated/`目錄，包含`generated/code`和`generated/metapackage`）。
- 檢查程式碼基底中是否存在[`app/etc/config.php`](../store/store-settings.md)檔案。 如果Adobe Commerce在建置階段期間未偵測到此檔案，且包含模組和擴充功能清單，則會自動產生此檔案。 如果存在，建置階段會照常繼續，使用GZIP壓縮靜態檔案並進行部署，以減少部署階段的停機時間。 請參考[建置選項](../environment/variables-build.md)，瞭解如何自訂或停用檔案壓縮。

>[!WARNING]
>
>此時尚未建立叢集，因此請勿嘗試連線到資料庫，或假設有作用中的協助程式處理作業。

應用程式建置後，它會掛接在&#x200B;**唯讀檔案系統**&#x200B;上。 您可以設定要讀取/寫入的特定掛載點。 您無法以FTP傳送至伺服器並新增模組。 您必須將程式碼新增到本機存放庫並執行`git push`，以建置和部署環境。 如需專案結構，請參閱[本機專案目錄結構](../project/file-structure.md)。

### 階段3：準備概要

建置階段的結果是唯讀檔案系統，稱為&#x200B;_slug_。 此階段會建立封存並將概要(Slug)置於永久儲存中。 下次您推送程式碼時，如果服務未變更，則會使用封存中的概要。

- 透過重複使用未變更的程式碼，讓持續整合的建置速度更快
- 如果程式碼變更，會更新下個組建的概要，以重複使用
- 如有需要，可即時還原部署
- 如果`app/etc/config.php`檔案存在於程式碼基底中，則包含靜態檔案

概要包含所有檔案和資料夾&#x200B;**，但不包括`magento.app.yaml`中設定的下列**&#x200B;個裝載：

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### 階段4：部署Slug和叢集

您的應用程式和所有[後端](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary)服務布建如下：

- 在容器中掛載每個服務，例如Web伺服器、OpenSearch、[!DNL RabbitMQ]
- 掛載讀寫檔案系統（掛載在高可用性分散式儲存格線上）
- 設定網路，讓服務可以「看到」彼此（而且只能看到彼此）

>[!NOTE]
>
>在所有建置和部署完成後在Git分支中進行變更，然後再次推送。 所有環境檔案系統都是&#x200B;_唯讀_。 唯讀系統可確保確定性部署，並大幅提升網站安全性，因為沒有任何處理程式可以寫入檔案系統。 它也能確保您的程式碼在整合、測試和生產環境中完全相同。

### 階段5：部署鉤點

>[!NOTE]
>
>此階段會將[!DNL Commerce]應用程式置於維護模式，直到部署完成。

最後一個步驟會執行部署指令碼，您可使用它來匿名化開發環境中的資料、清除快取以及查詢外部持續整合工具。 此指令碼執行時，您可以存取環境中的所有服務，例如Redis。

如果`app/etc/config.php`檔案不存在程式碼基底中，則使用`gzip`壓縮靜態檔案，並在此階段部署，這會增加部署階段和網站維護的長度。

>[!NOTE]
>
>請參考[部署變數](../environment/variables-deploy.md)，瞭解如何自訂或停用檔案壓縮。

有兩個部署鉤點。 `pre-deploy.php`連結完成必要的清理和擷取在建置連結中產生的資源和程式碼。 `php ./vendor/bin/ece-tools deploy`掛接執行一系列命令和指令碼：

- 如果Adobe Commerce為&#x200B;**未安裝**，則會安裝`bin/magento setup:install`、更新部署組態`app/etc/env.php`以及您指定環境的資料庫，例如Redis和網站URL。 **重要：**&#x200B;當您在安裝期間完成[首次部署](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/launch/overview.html)時，Adobe Commerce已安裝在所有環境中並已部署。

- 如果已安裝Adobe Commerce ****，請執行任何必要的升級。 部署指令碼執行`bin/magento setup:upgrade`以更新資料庫結構描述和資料（在擴充功能或核心程式碼更新後需要），也會更新您環境的部署組態、`app/etc/env.php`和資料庫。 最後，部署指令碼會清除Adobe Commerce快取。

- 指令碼可選擇使用命令`magento setup:static-content:deploy`產生靜態網頁內容。

- 針對靜態內容部署策略，使用預設設定為`quick`的領域（組建指令碼中的`-s`旗標）。 您可以使用環境變數[`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy)自訂策略。 如需這些選項和功能的詳細資訊，請參閱[部署靜態檢視檔案](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)的[靜態檔案部署策略](../deploy/static-content.md)和`-s`旗標。

>[!NOTE]
>
>部署指令碼使用`.magento`目錄中組態檔定義的值，然後指令碼會刪除目錄及其內容。 您的本機開發環境不受影響。

### 部署後：設定路由

部署執行時，程式會在進入點暫停傳入流量60秒，並重新設定路由，讓您的網路流量到達您新建立的叢集。

成功部署會移除維護模式，以允許正常存取，並為`app/etc/env.php`和`app/etc/config.php`組態檔建立備份(BAK)檔案。

使用`SCD_ON_DEMAND`變數啟用靜態內容產生，並設定[`post_deploy`連結](../application/hooks-property.md)，以便它清除快取並在&#x200B;_之後預先載入（加溫）快取_，容器開始接受連線和&#x200B;_在_&#x200B;正常傳入流量期間。

若要檢視組建和部署記錄，請參閱[檢視記錄](../test/log-locations.md#view-and-manage-logs)。
