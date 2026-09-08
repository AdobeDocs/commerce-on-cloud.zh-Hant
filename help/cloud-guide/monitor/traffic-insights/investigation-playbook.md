---
title: 調查行動手冊
description: 瞭解如何使用Adobe Commerce流量分析調查CDN頻寬超額、搜尋機器人和爬蟲負載，以及惡意流量，並瞭解何時上報。
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 0%

---

# 調查行動手冊

[!DNL Adobe Commerce Traffic Insights]應用程式的設計用途是協助您調查下列問題：

- 頻寬超額
- 爬蟲載入
- 惡意流量

或者，您可以要求[進階安全性：原生機器人管理、第7層DDoS和速率限制](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)，這是當手動緩解不足時Adobe的原生升級路徑。 每個步驟都會參考顯示症狀的Widget，讓您能夠從量度移至具體動作。

>[!WARNING]
>
>本頁上的建議僅為准則。 在部署封鎖規則之前，請務必針對您自己的流量驗證該規則。

## CDN頻寬超額

在考量頻寬使用量之前，請先瞭解頻寬的計費方式。 與[!DNL Adobe Commerce on Cloud Infrastructure]帳戶合併的&#x200B;**所有** Fastly服務的流量（包括每個生產&#x200B;**和**&#x200B;中繼環境）計入一般使用量，並與您合約中的年度津貼相比。 從&#x200B;**頻寬>總頻寬**&#x200B;開始，然後依內容型別&#x200B;**和**&#x200B;依網域詳細資料&#x200B;**的頻寬，將磁碟區歸因為**&#x200B;頻寬。

### 媒體內容

有些存放區因為有目錄而合法地提供大量的頻寬做為媒體。 如果&#x200B;**依內容型別的頻寬**&#x200B;顯示大量的媒體頻寬，請考慮下列緩解措施：

- 嘗試使用[Fastly有損轉換](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#force-lossy-conversion)，提供較小、品質較低的影像。
- 調查[Fastly深度影像最佳化](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/fastly-image-optimization#deep-image-optimization)以在內容傳遞網路(CDN)端產生調整大小的影像。

### 大型檔案

某些網站包含大型檔案或特定且繁重的回應，例如Enterprise Resource Planning (ERP)整合或匯出。 使用頻寬&#x200B;**的** URL檢閱&#x200B;**BW**&#x200B;和&#x200B;**平均大小**&#x200B;欄，以尋找這些大型檔案。 您可以將&#x200B;**路徑區段lvl 1 X頻寬**&#x200B;用於較高層級的檢視。

### 重型404s

找不到Adobe Commerce **404頁面**&#x200B;通常是繁重、主題樣式的頁面(~1.5 MB)和&#x200B;**無法快取**，因此重複的404可能會產生異常流量。 即使是像`favicon.ico`這樣微不足道的遺失資源，也可以變成繁重的`404`頁面，而不是小型檔案。 使用&#x200B;**依網域詳細資料之頻寬**、**依頻寬之URL**、**依頻寬之頂端IP**&#x200B;和&#x200B;**依IP子網路之統計資料**&#x200B;中的&#x200B;**404**&#x200B;和&#x200B;**404 BW**&#x200B;資料行，尋找產生404磁碟區的使用者端、IP和URL。 然後減少或限制該存取權，例如，傳回輕量型`403`。

### 低FPC命中率

[!DNL Adobe]建議啟用Fastly [遮蔽](https://www.fastly.com/documentation/guides/concepts/shielding/)，讓主要CDN快取彙總器提供來源，讓較少的要求從最接近使用者端的本機Points of Presence ([POP](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/))到達。 請參閱[檢查您的組態](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration#configure-back-ends-and-origin-shielding)。

POP對使用者端和遮蔽對POP流量會分開計算，而當使用者端回應經過壓縮時，遮蔽對POP流量[不會經過壓縮](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge)，以保留Edge Side Include ([ESI](https://www.fastly.com/documentation/reference/vcl/statements/esi/))支援。 這表示低的Full Page Cache (FPC)點選率會在動態頁面上帶來更高的頻寬。 使用&#x200B;**FPC點選率**、**依網域的FPC統計資料**&#x200B;和&#x200B;**CDN網路區段頻寬**&#x200B;來確認症狀。

低點選率通常是由大量搜尋引擎爬蟲所驅動（請參閱[搜尋機器人和爬蟲](#search-bots-and-crawlers)）。 另一種緩解方法是[在可用時提供過時快取給爬蟲](https://www.fastly.com/documentation/reference/vcl/variables/cache-object/stale-exists/)。 如果原因是廣泛、頻繁的快取失效，請使用&#x200B;**依標籤的快取失效**&#x200B;和&#x200B;**依頂端URL的FPC存留時間**&#x200B;來尋找流失的標籤/URL。

## 搜尋機器人和爬蟲

若要測量爬蟲影響，請從&#x200B;**依頻寬區分的已知機器人**&#x200B;和&#x200B;**已知機器人影響詳細資料**&#x200B;開始，檢視哪些機器人最活躍，然後[依特定機器人篩選](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use)，僅研究其要求。

### 太多請求

搜尋機器人傳送過多請求的最常見原因是剖析載有`<meta name="robots" content="index,follow">`的頁面時。 機器人可以近乎無窮無盡的回圈中，遵循頂端導覽和階層式導覽連結。 請考慮下列選項以解決此問題：

>[!WARNING]
>
> 在限制爬蟲活動之前，請先洽詢搜尋引擎最佳化(SEO)專家。 重新訓練可能會對您的SEO產生負面影響。

- 將`nofollow`新增至頂端導覽與階層式導覽連結，例如`<a rel="nofollow" href="https://example.com/sales.html">Sales</a>`。
- 將頁面Meta標籤變更為`index,nofollow` — 作為一般[設計組態設定](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/marketing/seo/seo-overview#configure-robotstxt)或具有自訂副檔名的每個頁面型別。 保持`sitemap.xml`準確，以便機器人一律有要索引的最新頁面清單。
- 更新`robots.txt`以封鎖路徑和資源機器人不應存取。
- 請注意，`crawl-delay`指示詞不是官方Robots Exclusion Protocol的一部分，但確實適用於某些機器人，例如Bingbot、Slurp、SEMrushBot和其他幾個機器人。 GoogleBot會忽略此指令。
- 新增速率限制規則。 Fastly模組中有原生[濫用爬蟲保護](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#abusive-crawler-protection)。 為了更精細的控制，對於具有個別速率限制的使用者代理程式規則運算式，[自訂清漆組態語言(VCL)程式碼片段](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-custom-snippets)可傳回`429` （過多請求）或`405` （不允許方法）。 請檢視爬蟲說明檔案，瞭解偏好方法和回應代碼。 請參閱Fastly的[速率限制VCL指引](https://www.fastly.com/documentation/reference/vcl/functions/rate-limiting/ratelimit-check-rate/)。
- AI和大型語言模型(LLM)爬蟲是不斷成長的特殊情況。 它們並不總是能一致地識別自己，因此VCL使用者代理程式規則可能會落後。 Adobe的[進階安全性](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/advanced-security)附加元件有[原生機器人管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)，可以區分邊緣的可疑AI爬蟲和擷取程式，單靠VCL無法區分它們。

### 封鎖不想要的爬蟲

如果某些搜尋引擎產生大量流量，而對業務並不重要，則可完全封鎖：

- 某些機器人在重新讀取和更新其剖析規則後，會在1-2天後跟隨`robots.txt`變更。
- 如果爬蟲忽略`robots.txt`，請使用自訂VCL程式碼片段([example](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level#block-traffic-by-user-agent))加以封鎖。 有些爬蟲會明確記錄這是頻率控制的慣用或唯一方法。

## 惡意指令碼與刮刀程式

使用流量分析應用程式來識別常見的攻擊方向，並視需要依焦點區域篩選。 如果紅色標幟的請求主要來自特定IP、子網路或地理位置（**依請求數排名的最佳IP**、**依IP子網路統計資料**、**依國家/地區統計資料**），請考慮使用自訂Fastly VCL封鎖它們。

無論您進行任何設定，每個雲端基礎結構專案都已擁有自動保護的基準。 隨附的Web應用程式防火牆(WAF)會立即封鎖SQL插入和已知的惡意IP訊號（後門、攻擊工具、CMDEXE、Log4J-JNDI、周遊、XSS），並在其他非惡意IP超過50個請求/分鐘、350個請求/10分鐘或1,800個請求/小時時時時時時時限制其速率。 該基準線是&#x200B;**WAF回應要求**&#x200B;以及此應用程式表格中的WAF訊號欄所指示的內容。 這些欄中的尖峰並不一定表示您未受到保護。

- 留意認證填滿、帳戶接管、假帳戶建立、卡片測試、內容擷取和詳細目錄/購物車囤積。 這些機器人導向的濫用模式出現在&#x200B;**機器人活動和請求分析**&#x200B;索引標籤中。 點選登入、帳戶、結帳或目錄端點的高數量、低多樣性流量是要在依請求計數&#x200B;**和**&#x200B;已知機器人影響詳細資料&#x200B;**排名的**&#x200B;前IP中尋找的簽章。
- 使用[Google reCAPTCHA](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/systems/security/captcha/security-google-recaptcha)保護簽出和簽出API端點不受機器人攻擊。
- 使用Fastly模組的原生速率限制[路徑保護](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md#path-protection)。
- 在逗號分隔的`Sigsci_Tags`欄位中檢查[下一代WAF訊號](https://www.fastly.com/documentation/guides/next-gen-waf/signals/using-system-signals/)，並將相關的訊號比對結合至目標封鎖規則。 可疑要求的值可能類似於`BOT-ANALYSIS,DATACENTER,SIGSCI-IP,SITE-FLAGGED-IP,SUSPECTED-BAD-BOT`。 WAF在開始自動封鎖之前，會先將IP的`SITE-FLAGGED-IP`標籤到臨界值。 由WAF回應的&#x200B;**WAF攻擊和異常訊號**、**WAF機器人訊號**&#x200B;和&#x200B;**要求**&#x200B;介面工具集，以及IP、子網路和國家表格中的WAF欄，會顯示這些介面。
- 請參閱Adobe在[封鎖Fastly層級](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level)的Adobe Commerce惡意流量上的文章，以瞭解常見方法。
- 針對無法採用手動封鎖的複雜案例，例如持續的機器人行銷活動、散佈在許多IP/API的攻擊，或第7層分散式拒絕服務(DDoS)，請先考慮Adobe的[進階安全性](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/advanced-security)附加元件（請參閱[原生機器人管理](#advanced-security-native-bot-management-layer-7-ddos--rate-limiting)）。 它在服務您店面的相同Fastly邊緣執行。 如果您需要超出其範圍的功能，建議使用協力廠商受管理的機器人緩解服務，與原生Fastly整合，例如[Datadome](https://docs.datadome.co/docs/module-fastly)或[HUMAN Bot Defender](https://www.fastly.com/documentation/guides/integrations/non-fastly-services/human-bot-defender/) （原稱PerimeterX）。 所有這些選項都會增加額外成本。

## 進階安全性：原生機器人管理、第7層DDoS和速率限制

前幾節將討論如何使用流量分析應用程式的資料和手動Fastly VCL。 對於不夠的情況，例如持續或不斷發展的機器人行銷活動、第7層（應用程式層）DDoS，或濫用幾乎分散在許多IP和API端點，Adobe提供[進階安全性](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/advanced-security)。

進階安全性是[!DNL Adobe Commerce on Cloud Infrastructure]的付費附加元件，可在已提供店面的相同Fastly平台上新增邊緣機器人管理（包括AI爬蟲和擷取器偵測）、第7層DDoS保護和進階速率限制。 請參閱[進階安全性](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/advanced-security)，以取得完整功能、目前限制及要求方式。

購買並啟用後，請使用流量分析應用程式來驗證進階安全性是否正常運作。 透過&#x200B;**WAF攻擊和異常訊號**、**WAF機器人訊號**&#x200B;和&#x200B;**WAF回應提出之要求**&#x200B;後的相同`Sigsci_Tags`和`Agent_response`欄位報告其決策。 比較啟用前後這些Widget，確認其是否對您的流量執行主動動作。
