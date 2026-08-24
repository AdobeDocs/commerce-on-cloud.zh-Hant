---
title: 自動縮放
description: 瞭解雲端基礎結構上的Adobe Commerce如何擴展以滿足資源需求。
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 11bfde40-79d1-4d51-9233-150c4cfb80fd
TQID: https://experienceleague.adobe.com/uL--0lHHJ-4SN3BkFU8reAefWhpMQOLBRVG7fX3jTM8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2:
  - id: db6b6496-d1b5-4ad4-9e18-dea78dae3aa8
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a542dac902dc0de7c0836c1e5e4aece40fc6cbee
workflow-type: tm+mt
source-wordcount: 979
ht-degree: 0%

---

# 自動縮放

自動縮放功能會自動新增或移除雲端基礎建設的資源，以維持最佳效能及合理成本。 Adobe為[!DNL Adobe Commerce on cloud infrastructure]個專案提供兩種型別的自動縮放：

- [水準自動縮放](#horizontal-auto-scaling) （僅適用於縮放架構） — 新增或移除縮放架構專案的網頁伺服器節點。
- [垂直自動縮放](#vertical-auto-scaling) （適用於標準Pro架構或縮放架構） — 調整現有節點的CPU容量以因應需求變化。


## 啟用自動縮放

若要啟用或停用您[!DNL Adobe Commerce on cloud infrastructure]專案的水平或垂直自動縮放，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。 在票證中選擇以下原因：

- **連絡原因**：基礎結構變更要求
- **Adobe Commerce基礎結構聯絡原因**：其他基礎結構變更要求

>[!IMPORTANT]
>
>自動縮放功能會擷取未預期的事件。 即使您已啟用自動縮放，如果您預計即將發生事件，Adobe仍建議您繼續[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。

### 載入測試

Adobe會先在雲端專案&#x200B;_暫存_&#x200B;叢集上啟用自動縮放。 在您的環境中執行並完成負載測試後，Adobe就會在您的生產叢集上啟用自動縮放。 如需負載測試的指南，請參閱[效能測試](../launch/checklist.md#performance-testing)。

## 水準自動縮放

目前，此功能僅適用於設定了[縮放架構](scaled-architecture.md)的專案。

水準自動縮放可新增或移除已縮放架構專案的網頁伺服器節點。 或者，[垂直自動縮放](#vertical-auto-scaling)會調整現有節點的CPU容量大小，以因應需求的變更。

### Web伺服器節點

[Web層](scaled-architecture.md#web-tier)可調整規模，以因應處理作業要求的增加以及較高的流量需求。 目前，自動縮放功能只能透過新增或移除Web伺服器節點來水平縮放。

當CPU使用和流量達到預先定義的臨界值時，就會發生自動縮放事件：

- **新增的節點** — 所有使用中Web節點的CPU/核心在1分鐘內都達到75%的容量，流量在連續5分鐘內增加20%。
- **節點已移除** — 所有使用中Web節點的CPU/核心以60%載入20分鐘。 節點會依照其新增順序移除。

最小值和最大臨界值是根據每個商家的合約資源限制來決定和設定的；這降低了無限擴展的風險。

### 使用New Relic監控臨界值

您可以使用[New Relic服務](../monitor/new-relic-service.md)來監視某些臨界值，例如主機計數和CPU使用量。 下列New Relic查詢對`cluster-id`使用變數標籤法僅供範例用途。

>[!TIP]
>
>如需建立查詢的參考，請參閱&#x200B;_New Relic_&#x200B;檔案中的[NRQL語法、子句和函式](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/)。
>使用您的查詢來建置[New Relic儀表板](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/)。

#### 主機計數

以下範例New Relic查詢顯示環境內的主機計數：

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

在下列熒幕擷圖中，**看到的APM主機**&#x200B;是指在選取期間記錄交易的主機數目。

![個New Relic主機計數](../../assets/new-relic/host-count.png)

#### CPU使用情況

以下範例New Relic查詢顯示網頁節點的CPU使用情形：

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![New Relic Web節點CPU使用情形](../../assets/new-relic/web-node-cpu-usage.png)

### IP允許清單

啟用自動縮放後，輸出Web節點流量會源自服務節點的IP位址。 如果您使用允許清單搭配未在雲端基礎結構專案上與Adobe Commerce繫結的第三方服務，請驗證第三方服務允許清單中的IP位址。

例如：

- 如果允許清單包含服務節點（1、2和3）的IP位址，則不需要採取任何動作。
- 如果允許清單包含服務節點（1、2和3）和Web節點（4、5和6）的IP位址（在此例中是全部六個節點），則不需要採取任何動作。
- 如果允許清單包含您Web節點（4、5和6）的IP位址&#x200B;_only_，則必須更新允許清單以包含服務節點的IP位址。

## 垂直自動縮放

除了傳統的[水準自動縮放](#auto-scaling)之外，[!DNL Adobe Commerce on cloud infrastructure]也為標準Pro架構和縮放架構專案提供垂直自動縮放。

垂直自動縮放不會新增或移除節點，而是會調整現有節點的CPU容量大小，以因應需求的變更。 這補充了水準自動縮放，為縮放的架構專案新增或移除網頁伺服器節點。

- **節點已新增**：不適用。 垂直自動縮放可調整現有節點的大小，而不會新增節點。
- **節點大小調整**：當記憶體壓力超過定義的臨界值時，節點會調整為下一個較大的執行個體大小。 每個縮放事件只會套用一次大小增加。
- **節點縮減大小**：節點會在需求結束後自動縮減大小。 最小和最大大小是根據每個專案的使用模式和合約資源限制所設定，這降低了不必要擴充的風險。

### 自動縮放臨界值

垂直自動縮放事件是使用Linux上記憶體的Pressure Stall Information (PSI)觸發，可測量系統因記憶體壓力而停頓的時間。 Adobe會根據您專案的合約資源限制和使用模式設定臨界值；商家目前無法設定這些臨界值。

### 使用New Relic監控臨界值

您可以使用[!DNL New Relic]服務來監視基礎結構執行個體詳細資料，包括執行個體大小和型別。 在New Relic中設定警報，以便在垂直自動縮放事件變更執行個體大小或型別時收到通知。

### 對您環境的影響

垂直自動縮放對您的環境有下列影響：

- **停機時間**：節點調整大小時，預計不會發生停機時間。
- **計時**：調整節點大小通常需要20到30分鐘。 調整大小時，節點會暫時從負載平衡器中移除。
