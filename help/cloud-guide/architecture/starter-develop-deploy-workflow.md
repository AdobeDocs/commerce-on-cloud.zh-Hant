---
title: 入門專案工作流程
description: 瞭解如何使用入門程式開發和部署工作流程。
feature: Cloud, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '2103'
ht-degree: 0%

---

# 入門專案工作流程

雲端基礎結構上的Adobe Commerce包含單一Git存放庫，此存放庫具有用於生產環境的`master`分支，可以分支以建立測試和開發工作的中繼和多個整合環境。 您最多可以有四個使用中的環境，包括生產伺服器的`master`環境。 如需概觀，請參閱[入門架構](starter-architecture.md)。

針對您的環境，請依照[!UICONTROL Development > Staging > Production]工作流程來開發和部署您的網站。

- **生產環境（即時網站）** — 提供完整的生產環境，其中包含從`master`分支上的程式碼建立及部署的所有服務。
- **中繼環境** — 提供符合生產環境的完整中繼環境，以及您從`master`複製而建立的`staging`分支所建置和部署的所有服務。
- **整合環境** — 提供您從`staging`分支建立的兩個使用中開發環境。 `integration`環境不支援Fastly和New Relic等協力廠商服務。

對於您的分支，您可以遵循任何開發方法。 例如，您可以遵循敏捷方法（例如Scrum），為每個衝刺建立分支。

從每個衝刺中，您可以為每個使用者劇本建立分支。 所有劇本都可以測試。 您可以持續合併至衝刺分支，並持續驗證該分支。 當衝刺結束時，您可以將衝刺分支合併到`master`以將所有衝刺變更部署到生產環境，而不必處理測試瓶頸。

## 開發工作流程

Starter計畫的開發和部署從您的初始專案開始。 您可以使用「空白網站」建立專案，這是雲端基礎結構範本程式碼存放庫上的Adobe Commerce，具有完全準備好的存放區。 這會建立`master`分支，其中包含您生產環境中的程式碼副本。

開發工作流程包括下列專案：

- 從`master`複製[和分支](#clone-and-branch)以建立`staging`和開發分支
- [開發程式碼](#develop-code)並在開發分支中本機安裝擴充功能，包括[!DNL Composer]更新
- [設定](#configure-store)您的商店和擴充功能設定
- [產生組態](#generate-configuration-management-files)管理檔案
- [推送程式碼](#push-code-and-test)和設定以建置並部署至`staging`和`production`環境

![開發及部署工作流程](../../assets/starter/workflow.png)

您也有幾個可選步驟可協助開發及測試您的程式碼和存放區資料：

- [將範例資料](#optional-install-sample-data)安裝到您的存放區
- [將生產存放區資料](#optional-pull-production-data)提取至環境

此程式假設您已設定[本機開發人員工作區](../development/overview.md)。

### 原地複製和分支

針對新的入門計畫專案，從雲端基礎結構上的Adobe Commerce複製了`master`分支Git存放庫。 若要開始分支和使用程式碼，請將`master`分支複製至您的本機環境。

Git複製命令的格式為：

```bash
git fetch origin
```

```bash
git pull origin <environment-ID>
```

您第一次開始使用入門專案的分支時，請建立`staging`分支。 這樣會建立符合`master`分支的程式碼分支，該分支會部署到中繼環境以測試設定和程式碼變更，然後再部署到生產環境。

接下來，從`staging`建立分支，以開發程式碼、新增擴充功能及設定協力廠商整合。 無論您何時開發自訂程式碼、新增擴充功能、與協力廠商服務整合，都可在從`staging`分支建立的開發分支中工作。 您有四個可用的作用中整合環境。 當您推送使用中的分支時，其中一個整合環境會自動部署您的程式碼以進行測試。

Git分支命令的格式為：

```bash
git checkout <branch-name>
```

雲端CLI `branch`命令的格式為：

```bash
magento-cloud environment:branch <environment-name> <parent-environment-ID>
```

從主版![分支](../../assets/starter/branching.png)

### 開發程式碼

在雲端基礎結構程式碼上使用Adobe Commerce的基本分支，您可以開始安裝擴充功能、開發自訂程式碼、新增主題等。

使用分支策略進行開發工作。 使用一個分支同時完成所有工作可能會使測試變得困難。 例如，您可以遵循持續整合和衝刺方法來運作：

- 新增一些擴充功能，並使用您的第一個分支進行設定
- 推送此程式碼、測試並合併至測試環境，然後再推送至生產環境
- 在`services.yaml`中完整設定您的服務並新增主題
- 推送此程式碼、測試並合併至測試環境，然後再推送至生產環境
- 與協力廠商服務整合
- 推送此程式碼、測試並合併至測試環境，然後再推送至生產環境

在您完全建置、設定好商店並準備啟動之前。 但請繼續閱讀，您的存放區和程式碼設定有許多選項。

>[!NOTE]
>
>請勿在本機工作站中完成任何設定。

![從本機](../../assets/starter/push-code.png)推送程式碼

### 設定存放區

當您準備好設定存放區時，請將所有程式碼推送到`integration`環境。 從管理員針對整合環境（而非本機環境）設定商店設定。 您可以按一下[!DNL Cloud Console]中的&#x200B;**存取網站**&#x200B;來尋找URL

如需設定的最佳資訊，請檢閱Adobe Commerce的檔案和已安裝的擴充功能。 以下是可協助您開始使用的一些連結和想法：

- [儲存區組態的最佳實務](../store/best-practices.md)，以取得雲端中的特定最佳實務
- [商店管理員存取、名稱、語言、貨幣、品牌、網站、商店檢視等的基本設定](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/store-details)
- [佈景主題](https://experienceleague.adobe.com/en/docs/commerce-admin/content-design/content-menu#design-features)，讓您瞭解網站和存放區的外觀與風格，包括CSS和版面配置
- 角色、工具、通知及資料庫加密金鑰的[系統組態](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/guide-overview)
- 使用其檔案的擴充功能設定

除了商店設定外，您還可以進一步設定多個網站和商店、設定的服務等。 請參閱[設定您的商店](../store/overview.md)。

### 產生組態管理檔案

如果您熟悉Adobe Commerce，可能會擔心如何從開發中的資料庫取得組態設定，以用於中繼和生產環境。 之前，您必須將所有組態設定複製到紙張或檔案中，然後手動將這些設定套用至其他環境。 或者，您可能已傾印資料庫並將該資料推送至其他環境。

雲端基礎結構上的Adobe Commerce提供了一組兩個[設定管理](../store/store-settings.md)命令，可將設定從您的環境匯出到檔案中。 這些命令僅適用於雲端基礎結構2.2和更新版本上的&#x200B;**Adobe Commerce**。

- `php .vendor/bin/ece-tools config:dump` — 僅將您從預設值輸入或修改的組態設定匯出到組態檔。 _建議_。
- `php bin/magento app:config:dump` — 將每個組態設定（包括已修改和預設的設定）匯出至組態檔。

產生的檔案是`app/etc/config.php`。

在您設定Adobe Commerce的整合環境中產生檔案。 逐步完成產生檔案、將其新增至分支及部署的流程。

有關組態管理的&#x200B;**重要注意事項**：

- 從`app:config:dump`命令產生的檔案中包含的任何組態設定都會被鎖定，無法在已部署的環境中進行編輯或唯讀。 這是Adobe建議使用`.vendor/bin/ece-tools config:dump`命令的原因之一。

  例如，您在開發環境中安裝Fastly的模組。 您只能在中繼和生產環境中設定此模組。 當您部署開發變更至測試與生產環境時，使用`.vendor/bin/ece-tools config:dump`命令可讓這些預設欄位保持可編輯狀態。

- 產生的檔案可能會很長，具體取決於部署的大小。 `.vendor/bin/ece-tools config:dump`命令產生的檔案比`app:config:dump`命令產生的檔案小。

如果您是使用Adobe Commerce 2.2版或更新版本，設定管理命令會提供額外功能來保護敏感資料，例如PayPal模組的沙箱認證。 在匯出程式中，包含敏感資料的任何值都會匯出到`app/etc/`目錄中的個別組態檔 — `env.php`。 此檔案會保留在您的本機環境中，而且不會在您推送程式碼至其他分支時複製。 您也可以在雲端基礎結構版本的所有Adobe Commerce中使用CLI命令來建立環境變數。

![環境變數產生](../../assets/starter/env-variables.png)

請參閱[組態管理](../store/store-settings.md)。

### 推播程式碼和測試

此時，您應該已開發程式碼分支，而且組態檔（`config.local.php`或`config.php`）已準備好進行測試。

每次您從本機環境推送程式碼時，都會執行一系列建置和部署指令碼。 這些指令碼會產生新程式碼並將其部署到遠端環境。 例如，如果您將開發分支從本機環境推送到遠端分支，符合的環境會更新服務、程式碼和靜態內容。

您可以使用商店URL、管理員URL和SSH直接存取此環境。 這些環境包括網頁伺服器、資料庫和設定的服務。 準備就緒後，您可在預備環境中開始部署和測試。

如需詳細資訊，請參閱[部署工作流程](#deployment-workflow)。

### 可選：安裝範例資料

如果您在開發存放區時需要一些範例資料，可以安裝範例資料。 此資料會模擬使用中存放區，包括客戶、產品和其他資料。 建立專案時，此範例資料最適合用於雲端基礎結構範本安裝的「空白網站」Adobe Commerce。 最佳實務是在上線前移除範例資料。 請參閱[安裝選擇性範例資料](../test/sample-data.md)。

![安裝選用的範例資料](../../assets/starter/sample-data.png)

### 選用：提取生產資料

將您的所有產品、目錄、網站內容等直接新增至`production`環境。 將此資料新增到生產環境，您就可以為客戶提供更新的價格、折價券、存貨量、銷售公告、未來優惠方案相關資訊等。 此資料不包含您在本機開發分支中設定的擴充功能設定。

當您開發功能、新增擴充功能和設計主題時，擁有可使用的真實資料會很有幫助。 您隨時可以從生產環境[建立資料庫傾印](../storage/database-dump.md)，並視需要將其推送到您的中繼和整合環境。

若要協助將生產資料匯出為測試資料，以用於中繼和整合環境：

- [使用您的Adobe Commerce加密金鑰匯出客戶和存放區資料的受保護備份時，執行支援公用程式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html) CLI命令（建議）

- 用於產生及匯出資料的[資料彙集](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/tools/support#data-collector)工具

若要移轉此資料，請參閱[移轉及部署靜態檔案和資料](../deploy/staging-production.md#migrate-static-files)。

![提取並處理生產資料](../../assets/starter/data-code-process.png)

>[!NOTE]
>
>將資料推送至另一個環境之前，您應該考慮清除資料。 您有幾個選項，包括[使用支援公用程式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/run-support-utilities.html)或開發指令碼以清除客戶資料。

>[!WARNING]
>
>請勿將資料庫從整合或中繼環境推送到生產環境。 如果這樣做，來自整合或測試環境的資料會覆寫您的即時生產資料，包括銷售、訂單、新客戶和更新客戶等。

## 部署工作流程

如架構資訊中詳述，雲端基礎結構上的Adobe Commerce是Git導向。 在雲端基礎結構上部署Adobe Commerce，是分支Git推送流程的一部分。

當您從本機環境推送分支程式碼至遠端分支時，會開始一系列的建置和部署指令碼。

建置指令碼：

- 目標環境中的網站會在建置期間繼續執行

- 在雲端基礎結構修補程式和Hotfix上檢查並執行Adobe Commerce

- 使用建置和部署記錄編譯程式碼

- 如果在此階段中發生靜態內容部署，請檢查「組態管理」

- 建立或使用未變更程式碼的概要，以加快程式

- 布建所有後端服務與應用程式

部署指令碼：

- 將您的網站置於維護模式的目標環境

- 如果未在建置期間完成，則部署靜態內容

- 在雲端基礎結構上安裝或更新Adobe Commerce

- 設定流量的路由

完全完成後，您的商店會重新上線、上線，並包含所有更新的程式碼和設定。

請參閱[部署程式](../deploy/process.md)。

### 推送至測試環境並進行測試

一律將反複專案中的程式碼推送到`staging`環境以進行完整測試。 第一次使用此環境時，您必須設定一些服務，包括[Fastly](/help/cloud-guide/cdn/fastly.md)和[New Relic](../monitor/new-relic-service.md)。 此外，使用沙箱或測試認證來設定付款閘道、運送、通知和其他重要服務。

預備是生產前的環境，會儘可能提供接近生產環境的所有服務和設定。 徹底測試每項服務、驗證您的效能測試工具、以管理員和客戶的身分執行UAT測試，直到您認為您的商店已準備好投入生產為止。

請參閱[部署您的存放區](../deploy/staging-production.md)。

### 推送至生產環境

當您推送至`master`分支時，就是推送至`production`環境。 完成生產環境中的設定和測試活動，就像在中繼環境中做的一樣，但有一個重要差異。 在生產環境中，使用即時認證進行設定和測試。 一旦您啟動網站，客戶即可完成購買，管理員即可管理即時商店。

請參閱[部署您的存放區](../deploy/staging-production.md)。

### 網站啟動

您可以使用明確的逐步說明來與您的網站一同上線。 完成這些步驟後，您的商店可以立即提供自訂主題中的產品以供銷售。

檢視[網站啟動](../launch/overview.md)。

## 持續整合

依照您的分支和開發方法，您可以輕鬆開發新功能、設定變更和新增擴充功能，以持續開發和部署更新。

所有雲端基礎結構環境都支援持續整合，以便持續更新。 此工作流程支援一天多次發行，或根據您的業務需求依排程發行。

- 建立具有未來功能和變更的開發分支

- 在您的`integration`環境中測試程式碼

- 在`staging`環境中部署和測試

- 部署至`production`環境
