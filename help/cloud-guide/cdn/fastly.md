---
title: Fastly服務總覽
description: 瞭解Adobe Commerce雲端基礎結構中包含的Fastly服務如何協助您最佳化及保護Adobe Commerce網站的內容傳遞作業。
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 0%

---

# Fastly服務總覽

>[!WARNING]
>
>為維持部署在雲端平台上的Adobe Commerce網站的PCI相容性，請在您的入門主要分支、Pro生產和Pro測試環境中設定Fastly。 如果您在Headless部署中使用Adobe Commerce，強烈建議您使用Fastly來快取GraphQL回應。 請參閱&#x200B;*GraphQL開發人員指南*&#x200B;中的[使用Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly)快取。

Fastly提供下列服務，以最佳化並確保Adobe Commerce在雲端基礎結構專案上的內容傳遞作業安全。 這些服務包含在雲端基礎結構的Adobe Commerce中，不需額外付費。

- **內容傳遞網路(CDN)** — 清漆式服務，可在您設定的後端資料中心快取您的網站頁面、資產、CSS等。 當客戶存取您的網站和商店時，請求會點選Fastly以更快載入快取頁面。 CDN服務提供下列功能：

- **快取管理** — 在您設定的後端資料中心快取您的網站頁面、資產、CSS等，以降低頻寬負載和成本

   - 使用[Fastly自訂VCL片段](fastly-vcl-custom-snippets.md) （符合Varnish 2.1）來修改快取回應要求的方式

   - 設定[GeoIP服務支援](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [強制未加密的請求傳送至TLS](fastly-custom-cache-configuration.md#force-tls)

   - [自訂Fastly逾時](fastly-custom-cache-configuration.md#extend-fastly-timeout)設定，以防止大量作業請求出現503個回應

   - 建立[自訂錯誤回應頁面](fastly-custom-response.md)

- **安全性** — 在您啟用Adobe Commerce網站的Fastly服務後，可以使用其他安全性功能來保護您的網站和網路：

   - [Web應用程式防火牆](fastly-waf-service.md) (WAF) — 受管理的Web應用程式防火牆服務，可提供PCI相容的保護，在惡意流量可能損害雲端基礎結構網站與網路上的生產Adobe Commerce之前，先封鎖惡意流量。 WAF服務僅適用於Pro和Starter Production環境。

   - [分散式阻斷服務(DDoS)保護](#ddos-protection) — 內建DDoS保護，可抵禦Ping of Death、Smurf攻擊和其他以ICMP為基礎的泛濫攻擊。

   - [SSL/TLS憑證](fastly-configuration.md#provision-ssltls-certificates)—Fastly服務需要SSL/TLS憑證才能透過HTTPS提供安全流量。

     Adobe Commerce提供網域驗證讓我們為每個中繼和生產環境加密SSL/TLS憑證。 Adobe Commerce會在Fastly設定過程中完成網域驗證和憑證布建。

- **原始遮罩** — 防止流量略過Fastly WAF，並隱藏原始伺服器的IP位址，以保護它們免受直接存取和DDoS攻擊。

  雲端基礎結構Pro Production專案的Adobe Commerce預設會啟用來源遮蓋。 若要在雲端基礎結構入門生產專案上啟用Adobe Commerce上的來源遮蔽，請提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 如果您的流量不需要快取，您可以自訂Fastly服務設定，以允許請求[略過Fastly快取](fastly-vcl-bypass-to-origin.md)。

- **[影像最佳化](fastly-image-optimization.md)** — 將影像處理和調整負載解除安裝到Fastly服務，讓伺服器可以更有效率地處理訂單和轉換。

- **[Fastly CDN和WAF記錄](../monitor/new-relic-service.md#new-relic-log-management)** — 對於雲端基礎結構Pro專案上的Adobe Commerce，您可以使用New Relic記錄服務來檢閱和分析Fastly CDN和WAF記錄資料。

## Magento2的Fastly CDN模組

雲端基礎結構上Adobe Commerce的Fastly服務使用安裝在以下環境中的Magento2&rbrack;的&lbrack;Fastly CDN模組： Pro Staging and Production， Starter Production （`master`分支）。

在初始布建或升級您的Adobe Commerce專案時，Adobe會在您的中繼和生產環境中安裝最新版本的Fastly CDN模組。 Fastly發行模組更新時，您會在管理員中收到環境的通知。 Adobe建議您更新環境以使用最新版本。 請參閱[Fastly升級](fastly-configuration.md#upgrade-the-fastly-module)。

## Fastly服務帳戶和認證

雲端基礎結構專案上的Adobe Commerce未獲得專用的Fastly帳戶。 Fastly服務是在註冊給Adobe的集中帳戶中進行管理，並且管理儀表板只能由雲端支援團隊存取。

相反，每個測試和生產環境都有唯一的Fastly憑證（API權杖和服務ID），可從Commerce管理員設定和管理Fastly服務。 Fastly API可用於執行Fastly服務的進階管理，這需要認證才能提交這些請求。

在專案布建期間，Adobe會將您的專案新增到雲端基礎結構上Adobe Commerce的Fastly服務帳戶，並將Fastly憑證新增到中繼和生產環境的設定中。 檢視[取得Fastly認證](fastly-configuration.md#get-fastly-credentials)。

### 變更Fastly API權杖

提交Adobe Commerce支援票證以變更Fastly API權杖認證。 當您收到新Token時，請更新您的預備或生產環境以使用新Token。

**若要變更Fastly API權杖認證**：

1. [提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，要求新的Fastly API認證。

   在雲端基礎結構專案ID和需要新認證的環境上包含您的Adobe Commerce。

1. 收到新的API Token後，請在「管理員」的[Fastly認證設定](fastly-configuration.md#test-the-fastly-credentials)中或從[[!DNL Cloud Console] 環境變數](../project/overview.md#configure-environment)更新API Token值。

1. [測試新認證](fastly-configuration.md#test-the-fastly-credentials)。

1. 更新認證後，請提交Adobe Commerce支援票證以刪除舊的API權杖。

### 多個Fastly帳戶和指派的網域

Fastly只允許您將一個頂點網域和關聯的子網域指派給一個Fastly服務和帳戶。 如果您有現有的Fastly帳戶，該帳戶連結了用於Adobe Commerce網站的相同頂點和子網域，則您有以下選項：

- 在雲端基礎結構專案環境中請求Adobe Commerce的Fastly服務憑證之前，請從現有帳戶中移除頂點和子網域。 請參閱Fastly檔案中的[使用網域]

  使用此選項將Apex網域和所有子網域連結到雲端基礎結構上Adobe Commerce的Fastly服務帳戶。

- 提交Adobe Commerce支援票證以請求網域委派，以便將Apex和子網域連結到不同的帳戶。

  如果您的Apex網域有Adobe Commerce和非Adobe Commerce網站的多個子網域，而且您想要將這些子網域連結至不同的Fastly帳戶，請使用此選項。

#### 要求網域委派

*案例1：*

Apex網域（`testweb.com`和`www.testweb.com`）連結到現有的Fastly帳戶。 您已在雲端基礎結構專案上設定Adobe Commerce，包含下列子網域： `mcstaging.testweb.com`和`mcprod.testweb.com`。 您不想在雲端基礎結構上將Apex網域移動到Adobe Commerce的Fastly服務帳戶。

提交[Fastly支援票證]，要求將子網域從現有的Fastly帳戶委派給雲端基礎結構上Adobe Commerce的Fastly帳戶。 在票證中包含您的Adobe Commerce專案ID。

委派完成後，您的專案子網域可以新增到雲端基礎結構上Adobe Commerce的Fastly服務帳戶。 檢視[取得Fastly認證](fastly-configuration.md#get-fastly-credentials)。

*案例2：*

Apex網域（`testweb.com`和`www.testweb.com`）已連結到雲端基礎結構上的Adobe Commerce Fastly服務帳戶。 您要管理其他Fastly帳戶中`service.testweb.com`和`product-updates.testweb.com`子網域的Fastly服務。

提交Adobe Commerce支援票證，要求從雲端基礎結構上的Adobe Commerce將子網域委派到Fastly服務帳戶。 在票證中包含Fastly帳戶的服務ID。

## DDoS保護

Fastly CDN服務內建了DDOS保護。 一旦您為Adobe Commerce網站啟用了Fastly服務，Fastly就會篩選所有網頁和管理程式流量，以偵測和封鎖潛在的攻擊。

- 針對第3層或第4層的攻擊，Fastly服務會根據連線埠和通訊協定篩選出流量，僅檢查HTTP或HTTPS請求。 網路邊緣會捨棄ICMP、UDP和其他網路啟動的攻擊。 這包括反射和放大攻擊，這些攻擊使用UDP服務，如SSDP或NTP。 透過提供這種層級的保護，我們有效地封鎖了多種常見的攻擊，例如Ping of Death、Smurf攻擊和其他基於ICMP的泛洪攻擊。

  Fastly在快取層管理TCP層級攻擊。 此策略為每個使用者端提供必要的規模和情境，以處理SYN泛洪攻擊及其許多變體，包括TCP棧疊、資源攻擊和Fastly系統內的TLS攻擊。

- Fastly也提供第7層攻擊的保護。 如果您的商店發生效能問題，而且您懷疑是第7層DDoS攻擊，請提交Adobe Commerce支援票證。 Adobe可以建立自訂規則並套用至Fastly服務，以根據標頭、裝載或可識別攻擊流量的屬性組合來檢查並篩選掉惡意請求。 請參閱&#x200B;*Adobe Commerce說明中心*&#x200B;中的[檢查DDoS攻擊]和[如何封鎖惡意流量]。

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[檢查DDoS攻擊]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Magento2的Fastly CDN模組]: https://github.com/fastly/fastly-magento2

[Fastly支援票證]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[如何封鎖惡意流量]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[使用網域]: https://docs.fastly.com/en/guides/working-with-domains
