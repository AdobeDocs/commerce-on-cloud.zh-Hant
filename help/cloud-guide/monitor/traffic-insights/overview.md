---
title: Adobe Commerce流量深入分析
description: 瞭解Adobe Commerce流量分析工具，以及它如何協助您瞭解雲端基礎結構專案上Adobe Commerce的流量。
feature: Cloud, Observability
role: Admin
source-git-commit: 119c9415abd22221e3ae785445d537f0609eba14
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---

# 流量分析

Adobe Commerce流量深入分析是一種New Relic One應用程式，可將[!DNL Adobe Commerce on Cloud Infrastructure]個Fastly CDN流量視覺化。 它會讀取已送入New Relic的Fastly CDN存取記錄行當作`Log`事件，並呈現一組精選的圖表，其範圍為您選取的New Relic帳戶和平台時間範圍。 如此可視覺化商店的邊緣流量，而不需手動撰寫New Relic的查詢語言NRQL。

## 協助您調查的內容

流量深入分析的設計目的，是為了協助您解決三個常見問題：

- **CDN頻寬超額** — 流量趨勢高於合約允許。 將磁碟區歸因於大量媒體、大型檔案、無法快取的404頁面或低效率的快取，向下歸因於特定網域、內容型別、URL或專案。
- **搜尋機器人和爬蟲載入** — 產生不成比例的請求共用的搜尋引擎或AI爬蟲，這會影響快取效率和來源載入。 檢視哪些具名機器人最活躍，以及擷取的確切內容。
- **惡意指令碼和修剪程式** — 刮花、認證填塞、卡片測試、建立假帳號或第7層濫用。 顯示Fastly Next-Gen WAF訊號，以及可疑流量背後的IP、子網路和國家/地區。

在每種情況下，應用程式都會識別流量的&#x200B;*人員、內容和位置*。 透過Fastly VCL規則、影像最佳化、快取調整、速率限制或您Commerce和Fastly組態中的Adobe [進階安全性](../../cdn/advanced-security.md)附加元件來操作該資訊。 [調查行動手冊](investigation-playbook.md)涵蓋了上述每個問題。

## 存取應用程式

- **直接連結：** [Adobe Commerce流量深入分析](https://one.newrelic.com/a9a0c3b8-3844-4ca1-8bad-c6742747be47)。
- **從New Relic One首頁畫面** (one.newrelic.com) — 帳戶訂閱應用程式後，就會在首頁上顯示為自己的磚&#x200B;**Adobe Commerce流量深入分析**。
- **從最上方的搜尋列（快速尋找）** — 搜尋`Adobe Commerce Traffic Insights`並從結果中選取它。
- **若要釘選以加快存取速度** — 使用應用程式標題或頁首上的星形或釘選控制項，將其新增至我的最愛或左側導覽。 此控制項的確切位置取決於帳戶使用的New Relic UI版本。

## 本指南內容

- **[瞭解應用程式](understanding-the-app.md)** — 什麼是流量深入分析、如何使用篩選器推動、如何測量數字，以及資料能告訴您哪些和無法告訴您。
- **[調查Playbook](investigation-playbook.md)** — 針對應用程式建置所要解決的三個問題，建議使用的方法：頻寬超額、爬蟲負載和惡意流量。 其中每一個都參考確認它的圖表，並指定當手動緩解不足時Adobe的原生[進階安全性](../../cdn/advanced-security.md)提升路徑。