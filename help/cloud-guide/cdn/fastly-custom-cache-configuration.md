---
title: 自訂快取設定
description: 瞭解如何在Fastly服務設定完成後檢閱及自訂快取配置設定。
feature: Cloud, Configuration, Iaas, Cache
exl-id: f6901931-7b3f-40a8-9514-168c6243cc43
source-git-commit: dcf585e25a4b06ff903642e42e72a71820bad008
workflow-type: tm+mt
source-wordcount: '1857'
ht-degree: 0%

---

# 自訂快取設定

在測試和生產環境中設定並測試Fastly服務後，請檢閱並自訂快取組態設定。 例如，您可以更新設定以啟用強制TLS將HTTP請求重新導向到Fastly、更新清除設定，以及啟用基本驗證以在開發期間以密碼保護您的網站。

以下小節提供設定某些快取設定的概觀和指示。 在Magento 2[&#128279;](https://github.com/fastly/fastly-magento2/tree/master/Documentation)適用的Fastly CDN模組檔案中尋找有關可用設定選項的其他資訊。

## 強制TLS

Fastly提供&#x200B;_強制TLS_&#x200B;選項，可將未加密的請求(HTTP)重新導向至Fastly。 為您的預備或生產環境布建了[有效的SSL/TLS憑證](fastly-configuration.md#provision-ssltls-certificates)後，您可以更新存放區的Fastly設定以啟用「強制TLS」選項。 請參閱Magento 2 _的_ Fastly CDN模組檔案中的Fastly [強制TLS指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/FORCE-TLS.md)。

>[!NOTE]
>
>啟用強制TLS選項是雲端基礎結構存放區上Adobe Commerce的建議最佳作法。

## 延長Fastly逾時

Fastly服務設定為管理員的HTTPS請求指定180秒的預設逾時期間。 任何超過逾時期間的請求處理都會傳回503錯誤。 因此，您可能會收到503個錯誤，回應需要長時間處理的請求，或嘗試執行大量作業。

若要完成超過3分鐘的批次處理動作，請變更&#x200B;_管理路徑逾時_ value_以避免503錯誤。

>[!NOTE]
>
>如果您已在&#x200B;**商店** > **設定** > **進階** > **管理員** > **管理員基底URL**&#x200B;中的&#x200B;**自訂管理員路徑**&#x200B;欄位中指定自訂管理員路徑端點，您也必須將該環境中的[ADMIN_URL變數](../environment/variables-admin.md#change-the-admin-url)設定為相同的值。 如果設定不同，逾時將無法運作。
>
>若要延伸Fastly UI中管理員以外的Fastly逾時引數，請參閱[增加長工作的逾時](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-INCREASE-TIMEOUTS-LONG-JOBS.md)。

**若要延長管理員的Fastly逾時**：

{{admin-login-step}}

1. 按一下&#x200B;**存放區** >設定> **組態** > **進階** > **系統**，然後展開&#x200B;**完整頁面快取**。

1. 在&#x200B;_Fastly設定_&#x200B;區段中，展開&#x200B;**進階設定**。

1. 設定&#x200B;**管理路徑逾時**&#x200B;值（以秒為單位）。 此值不能超過10分鐘（600秒）。

1. 按一下頁面頂端的「**儲存設定**」。

1. 頁面重新載入後，在&#x200B;_Fastly組態_&#x200B;區段中選取&#x200B;**上傳VCL到Fastly**。

Fastly會擷取Admin路徑，以從`app/etc/env.php`組態檔產生VCL檔案。

## 設定清除選項

Fastly在您的Magento Cache Management頁面上提供多種型別的清除選項，包括清除產品類別、產品資產和內容的選項。 啟用後，Fastly會監視事件以自動清除這些快取。 如果停用永久刪除選項，您可以在完成更新後，透過「快取管理」頁面手動永久刪除Fastly快取。

永久刪除選項包括：

- **清除類別** — 當您新增和更新單一產品時，清除產品類別內容（非產品內容）。 您可能會想要停用此功能，並啟用清除產品，以清除產品和產品類別。
- **清除產品** — 儲存單一產品修改時，清除所有產品和產品類別內容。 啟用清除產品有助於在變更價格、新增產品選項以及產品庫存無存貨時立即向客戶取得更新。
- **清除CMS頁面** — 更新頁面並將其新增至Adobe Commerce CMS時清除頁面內容。 例如，在更新「條款與條件」或「退貨」原則時，您可能想要永久刪除。 如果您很少進行這些變更，則可以停用自動清除。
- **軟清除** — 根據過時時間，將變更的內容設為過時並清除。 除了過時的計時之外，Fastly也會在背景更新內容時，提供客戶過時的內容。

![設定清除選項](../../assets/cdn/fastly-purge-options.png)

**若要設定Fastly清除選項**：

1. 在&#x200B;_Fastly設定_&#x200B;區段中，展開&#x200B;**進階設定**&#x200B;以顯示清除選項。

1. 針對每個清除選項，選取&#x200B;**是**&#x200B;以啟用自動清除，或選取&#x200B;**否**&#x200B;以停用自動清除。

   當您停用清除選項時，必須從&#x200B;_快取管理_&#x200B;頁面手動清除該類別的快取。

1. 按一下頁面頂端的「**儲存設定**」。

1. 頁面重新載入後，在&#x200B;_Fastly組態_&#x200B;區段中選取&#x200B;**上傳VCL到Fastly**。

如需詳細資訊，請參閱[Fastly組態選項](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/CONFIGURATION.md#further-configuration-options)。

## 設定GeoIP處理

Fastly模組包含GeoIP處理，可自動重新導向訪客，或提供與所取得國家/地區代碼相符的商店清單。 如果您已使用GeoIP處理的擴充功能，則可能需要使用Fastly選項驗證功能。

**若要設定GeoIp處理**：

{{admin-login-step}}

1. 按一下&#x200B;**存放區** >設定> **組態** > **進階** > **系統**，然後展開&#x200B;**完整頁面快取**。

1. 在&#x200B;_Fastly設定_&#x200B;區段中，展開&#x200B;**進階設定**。

1. 向下捲動並選取&#x200B;**是**&#x200B;至&#x200B;**啟用GeoIP**。 會顯示其他設定選項。

1. 若為GeoIP動作，選取是否使用&#x200B;**重新導向**&#x200B;自動重新導向訪客，或提供可使用&#x200B;**對話方塊**&#x200B;選取的存放區清單。

1. 針對&#x200B;**國家/地區對應**，選取&#x200B;**新增**&#x200B;以輸入與清單中特定Adobe Commerce商店對應的雙字母國家/地區代碼。

   ![新增GeoIP國家地圖](/help/assets/cdn/fastly-geo-code.png)

1. 按一下頁面頂端的「**儲存設定**」。

1. 重新載入頁面後，在&#x200B;_Fastly組態_&#x200B;區段中選取&#x200B;**上傳VCL到Fastly**。

>[!NOTE]
>
>目前的Adobe Commerce Fastly GeoIP模組實作不支援多個網站之間的重新導向。

Fastly也提供一系列[地理位置相關的VCL功能](https://developer.fastly.com/reference/vcl/variables/geolocation/)，用於自訂地理位置編碼。

## 啟用Fastly Edge模

Fastly Edge模組是一種彈性的架構，可透過範本定義UI元件和相關聯的VCL程式碼。 這些模組可讓您透過使用者介面輕鬆自訂和擴充Fastly服務組態，而不使用自訂VCL片段。

Edge模組可讓您啟用CORS標題、Cloud Sitemap重寫等特定功能，並設定Adobe Commerce存放區與其他CMS或後端之間的整合。

若要存取Edge模組功能表以檢視、設定和管理可用的模組，請開啟&#x200B;_啟用Fastly Edge模組_&#x200B;選項。 請參閱Fastly CDN模組檔案中的[Fastly Edge模組](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULES.md)。

## 設定後端和來源遮蔽

後端設定可提供Fastly效能的微調功能，以及原點遮蔽和逾時。 _後端_&#x200B;是特定位置（IP或網域），具有已設定的Origin shield和逾時設定，可用於檢查及提供快取內容。

_來源遮蔽_&#x200B;會將您存放區的所有要求路由到特定顯示點(POP)。 收到要求時，POP會檢查快取內容並提供該內容。 如果未快取，則會繼續到Shield POP，然後到快取內容的原始伺服器。 遮罩會減少直接傳送到來源的流量。

預設Fastly VCL程式碼會指定雲端基礎結構網站上您Adobe Commerce的原始遮蔽和逾時的預設值。 在某些情況下，您可能需要修改預設值。 例如，如果您發生第一位元組時間(TTFB)錯誤，您可能需要調整&#x200B;_第一位元組逾時_&#x200B;值。

>[!NOTE]
>
>如果您的網站需要透過[Wordpress](fastly-vcl-wordpress.md)等後端整合進行功能傳送，請自訂您的Fastly服務設定，以新增後端並管理從Adobe Commerce商店到Wordpress的重新導向。 如需詳細資訊，請參閱Fastly模組檔案中的[Fastly Edge模組 — 其他CMS/後端整合](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md)。

**若要檢閱後端設定組態**：

{{admin-login-step}}

1. 按一下&#x200B;**存放區** >設定> **組態** > **進階** > **系統**，然後展開&#x200B;**完整頁面快取**。

1. 展開&#x200B;**Fastly組態**&#x200B;區段。

1. 展開&#x200B;**後端設定**&#x200B;並選取齒輪以檢查預設後端。 強制回應視窗會開啟，顯示目前設定及變更選項。

   ![修改後端](../../assets/cdn/fastly-backend.png)

1. 選取&#x200B;**Shield**&#x200B;位置（或資料中心）。

   專案的預設Fastly設定會設定最接近您的雲端服務區域的位置。 如果您需要變更，請選取靠近預設位置的位置。

1. 修改遮蔽連線的逾時值（微秒）、位元組之間的時間以及第一個位元組的時間。 我們建議保留預設逾時設定。

1. 選擇性地選取&#x200B;**在編輯或儲存後**&#x200B;啟動後端和Shield。

1. 按一下&#x200B;**上傳**&#x200B;以儲存您的變更並將其上傳到Fastly伺服器。

1. 在Admin中，選取&#x200B;**儲存設定**。

如需詳細資訊，請參閱Fastly模組檔案中的[後端設定指南](https://github.com/fastly/fastly-magento2/blob/21b61c8189971275589219d418332798efc7db41/Documentation/Guides/BACKEND-SETTINGS.md)。

## 基本驗證

基本驗證功能可保護您網站上的每個頁面和資產
使用使用者名稱和密碼。 我們&#x200B;**不建議**&#x200B;啟用基本功能
驗證您的生產環境。 您可以在測試環境上加以設定
以在開發過程中保護您的網站。 請參閱Fastly CDN模組檔案中的[基本驗證指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BASIC-AUTH.md)。

如果您新增使用者存取權並在測試環境啟用基本驗證，您仍可以
存取管理員而不需要其他認證。

## 建立自訂VCL片段

Fastly支援自訂版本的Varnish Configuration Language (VCL)以自訂Fastly服務組態。 例如，您可以使用具有邊緣和存取控制清單(ACL)字典的VCL程式碼區塊，允許、封鎖或重新導向特定使用者或IP位址的存取。

如需建立自訂VCL片段、邊緣字典和ACL的指示，請參閱[自訂Fastly VCL片段](fastly-vcl-custom-snippets.md)。

>[!NOTE]
>
>將自訂VCL程式碼、邊緣字典和ACL新增到Fastly模組組態之前，請先確認Fastly快取服務可搭配預設組態運作。 檢視[設定Fastly](fastly-configuration.md)。

## 管理網域

對於Starter和Pro專案，您可以使用[!UICONTROL Domains]選項來新增和管理商店的Fastly網域設定。

- 對於入門專案，請移至[!DNL Cloud Console]中[!UICONTROL Domains]標籤下的專案URL以新增您的專案URL。

- 若為Pro專案，請提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hant#submit-ticket)，以將網域新增至您的雲端專案設定。 支援團隊也會更新Adobe Commerce Fastly帳戶設定以新增網域。

**若要從管理員管理Fastly網域設定**：

{{admin-login-step}}

1. 選取&#x200B;**存放區** >設定> **組態** > **進階** > **系統**，並展開&#x200B;**完整頁面快取**。

1. 在管理員&#x200B;_Fastly設定_&#x200B;區段中，選取&#x200B;**網域**。

1. 按一下&#x200B;**管理網域**&#x200B;以開啟[網域]頁面。

1. 在雲端環境中新增存放區的頂層和子網域名稱。

   您只能指定已新增至雲端基礎結構設定的網域。

   ![為起始者新增Fastly網域設定](../../assets/cdn/fastly-starter-activate-domain.png)

1. 按一下&#x200B;**啟動**&#x200B;以更新Fastly網域設定。

>[!NOTE]
>
>如果在不同的Fastly帳戶上設定了相同的網域，則必須先提交Adobe Commerce支援票證以請求網域委派，然後才能將網域新增到Adobe Commerce。 檢視[多個Fastly帳戶和指派的網域](fastly.md#multiple-fastly-accounts-and-assigned-domains)。

## 啟用維護模式

使用&#x200B;_維護模式_&#x200B;選項可允許從指定的IP位址管理存取您的網站，同時針對所有其他請求傳回錯誤頁面。

**若要啟用具有管理存取權的維護模式**：

1. 開啟Admin中的&#x200B;_Fastly設定_&#x200B;區段。

1. 在&#x200B;_Edge ACL_&#x200B;區段中，以管理IP位址更新`maint_allow`存取控制清單(ACL)，此管理IP位址可在儲存區處於維護模式時存取此儲存區。

   ![更新IP維護模式允許清單](../../assets/cdn/fastly-maint-allowlist.png)

1. 在&#x200B;_維護模式_&#x200B;區段中，選取&#x200B;**啟用維護模式**。

   啟用維護模式後，除了來自`maint_allowlist` ACL中IP位址的請求之外，所有流量都會遭到封鎖。 您可以更新`maint_allowlist`以變更ACL中的IP位址。

   如需詳細的設定指示，請參閱Magento 2模組檔案的Fastly CDN中的[維護模式指南](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/MAINTENANCE-MODE.md)。
