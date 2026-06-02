---
title: 資料擷取
description: 瞭解如何在New Relic中檢視和管理Commerce資料擷取。
feature: Cloud, Observability
exl-id: b457b4de-deeb-4e92-b95a-c2b89d6f7a05
TQID: https://experienceleague.adobe.com/60hhI0IvazUSrw6cCRoXfbGjA4fkLLPXb8Q4Q08AqeE
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: ebde5b41-29c9-4f5e-9ef6-1197e85409e3
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 209
ht-degree: 0%

---

# 資料擷取

New Relic仰賴豐富的資料來提供有效的監控和分析，但大型資料集可能會影響即時的結果、效能和合規性。 本主題提供有關管理資料擷取的一些指引，以及精簡資料的策略，以便使其最有效。

New Relic提供&#x200B;_資料管理_&#x200B;檢視，可依資料來源彙總您的計畫使用情況。

**若要檢視您的內嵌資料和來源**：

1. 從您的New Relic使用者功能表，按一下&#x200B;**[!UICONTROL Manage your data]**。
1. 按一下&#x200B;_管理_&#x200B;清單中的&#x200B;**[!UICONTROL Data management]**。

   ![資料管理](../../assets/new-relic/data-ingestion.png)

   **[!UICONTROL Data ingestion]**&#x200B;索引標籤會顯示擷取的當天資料以及資料來源。
資料保留索引標籤會顯示並控制資料儲存的時間。

1. 選取「**[!UICONTROL Limits]**」標籤，並檢視您帳戶的限制。

Adobe Commerce的資料來源包括：

- **APM事件** — 用於圖表和儀表板中的事件資料
- **基礎結構** — 處理和主機量度，例如CPU、儲存、網路
- **記錄**—CDN、APM和應用程式伺服器的記錄

記錄資料在擷取中佔很大比例。 瞭解如何[檢視和分析記錄檔資料](log-management.md#view-and-analyze-log-data)，並與您的Adobe代表合作，針對資料擷取和保留需求制定策略。 在&#x200B;_New Relic檔案_&#x200B;中進一步瞭解[管理資料擷取](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/)。
