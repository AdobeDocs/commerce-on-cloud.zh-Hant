---
title: 資料擷取
description: 瞭解如何在New Relic中檢視和管理Commerce資料擷取。
feature: Cloud, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '210'
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
