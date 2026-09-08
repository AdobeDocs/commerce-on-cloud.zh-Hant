---
title: 瞭解應用程式
description: 瞭解Adobe Commerce流量分析如何運作、如何使用篩選器推動、如何測量其資料，以及其資料限制和效能。
feature: Cloud, Observability
role: Admin
source-git-commit: 09318645dd341a74d72237f4d4162d1d26d03651
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# 瞭解應用程式

[!DNL Adobe Commerce Traffic Insights]應用程式會將原始Fastly內容傳遞網路(CDN)存取記錄視覺化成商店邊緣流量的圖片。 圖表會分組到下列索引標籤中：

- **頻寬** — 在一段時間內流量頻寬如何跨網域、內容和資源型別以及雲端專案分配。
- **全頁快取效能** — 在邊緣快取產品詳細資料頁面(PDP)、產品清單頁面(PLP)和內容管理系統(CMS)頁面的動態店面HTML的效率如何。
- **機器人活動和要求分析** — 依已知機器人代理程式、地理位置、IP/子網路、URL和Fastly Next-Gen Web應用程式防火牆(WAF)訊號劃分的流量。

第四個應用程式內&#x200B;**檔案**&#x200B;索引標籤包含概念備註和[調查行動手冊](investigation-playbook.md)。

## 本指南適用對象？

- **網站操作員與網站可靠性工程(SRE)**&#x200B;正在調查CDN頻寬超額、流量尖峰或來源負載。
- **開發人員**&#x200B;調整全頁快取(FPC)涵蓋範圍和點選率，或實作Fastly Varnish Configuration Language (VCL)規則。
- **管理員和安全工程師**&#x200B;識別並緩解不想要的機器人、刮刀和惡意的自動化流量。

假設熟悉[!DNL Adobe Commerce on Cloud Infrastructure]、Fastly CDN概念以及基本的New Relic導覽。

## 運作方式

從頁面頂端的平台控制項選取帳戶和時間範圍。 選用的&#x200B;**專案識別碼**&#x200B;可進一步將圖表縮小至特定雲端專案。 在主帳戶或合作關係設定中，能夠在下拉式清單中檢視帳戶並不意味著您可以查詢它。 如果圖表回報許可權錯誤，請切換到您擁有New Relic查詢語言(NRQL)存取權的帳戶。

您可繼續套用篩選器，將整體概述轉換為重點調查。 按一下Facet欄中的值（例如機器人、IP、子網路、國家/地區或內容型別）以新增[全域篩選器](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/filter-new-relic-one-dashboards-facets/#example-use)。 作用中的篩選器會顯示在格線的頂端，並同時套用至每個標籤中的每個Widget。 若要擴大範圍，請移除篩選條件。

**逐步解說** — 假設趨勢超過合約許可的&#x200B;*總頻寬*，而您想知道是誰在驅動它：

1. 開啟&#x200B;**機器人活動和要求分析**&#x200B;索引標籤並讀取&#x200B;**頻寬結構**，以檢視有多少流量是自動流量還是有機流量。
1. 如果機器人似乎有更多的流量，請開啟&#x200B;**依頻寬區分的已知機器人**，然後按一下最重的命名機器人，例如刮刀。 這新增了一個篩選器，這表示現在每個介面工具集的範圍都限制在該機器人。
1. 閱讀&#x200B;**已知機器人影響詳細資料**&#x200B;的請求率、狀態組合和FPC點選率。
1. 若要檢視機器人的來源位置，請檢查&#x200B;**依國家/地區的頻寬**。 若要檢視機器人正在擷取的內容，請參閱&#x200B;**依頻寬的URL**。
1. 如果流量集中在單一網路中，請按一下&#x200B;**依IP子網路統計資料**&#x200B;以確認執行者會在單一區塊中跨位址旋轉。
1. 您現在已具備撰寫針對性緩解措施所需的人員、內容和地點。 繼續瀏覽[調查行動手冊](investigation-playbook.md)，瞭解如何繼續。

相同的篩選方法適用於任何起始面向：可疑的國家/地區、單一IP、內容型別或URL路徑區段。

## 如何測量資料

瞭解一些測量選擇有助於讓數字更易於信任和理解。

- **Bandwidth (BW)**&#x200B;是CDN針對相符要求所服務的位元組總數，會計算&#x200B;**回應標頭與內文**。 這是計入合約備抵額的標題成本量度。
- **個要求（需要）** 但是，在啟用Fastly [遮蔽](https://www.fastly.com/documentation/guides/concepts/shielding/)的情況下，單一要求會記錄&#x200B;**兩次**，一次記錄於下列各項：
  - 內部遮蔽[存在點(POP)](https://www.fastly.com/documentation/guides/getting-started/concepts/using-fastlys-global-pop-network/)
  - EDGE POP
    除非回應直接來自本機POP快取，或遮蔽本身就充當傳送者位置的POP，否則會發生此情況。 為避免重複計算這些`HIT,MISS`和`MISS,MISS`個案例，應用程式的查詢會在`request_id`欄位上彙總為[`uniqueCount`](https://docs.newrelic.com/docs/nrql/nrql-syntax-clauses-functions/#func-uniqueCount)。 這會傳回接近&#x200B;**的近似值**，其預期錯誤邊界為&#x200B;**~5%**，而不是確切計數。
- **CDN網路區段**&#x200B;的壓縮方式不同。 傳送給使用者端的回應已壓縮，但shield-to-POP流量為[未壓縮](https://www.fastly.com/documentation/guides/concepts/compression/#compression-at-the-edge)，以保留[Edge Side Include (ESI)](https://www.fastly.com/documentation/reference/vcl/statements/esi/)支援。 因此，低快取命中率會讓內部區段膨脹得比使用者端區段大，因為未快取的內容必須以完整、未壓縮的大小重複拉過遮罩。 此壓縮是為什麼&#x200B;**CDN網路區段頻寬** Widget和FPC點選率是相同基礎成本的兩個檢視。

## 資料限制和效能

- **30天保留** — 根據訂閱計畫，Fastly CDN記錄檔在New Relic中保留&#x200B;**30天**。 您挑選的任何時段都必須位於過去30天內。 若是較長期的&#x200B;**總計**&#x200B;頻寬，請在[!DNL Adobe Commerce admin]面板中使用直接的Fastly整合，**儀表板> Fastly >頻寬>總計**，但會考慮它會報告每個服務ID，因此必須針對每個環境收集資料，並彙總以與合約額度進行比較。
- **60秒的查詢限制** — 每個圖表的NRQL都有[60秒的執行限制](https://docs.newrelic.com/docs/nrql/using-nrql/rate-limits-nrql-queries/#query-duration)。 對於非常高流量的帳戶，Widget在掃描太多記錄檔記錄時可能會逾時。 如果發生這種情況，請縮短時間範圍並重新載入圖表。 對於較輕的標籤，您可以再次展開它。
