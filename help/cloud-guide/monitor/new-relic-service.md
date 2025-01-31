---
title: New Relic服務
description: 瞭解您的Adobe Commerce在雲端基礎結構專案中可用的New Relic服務。
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# New Relic服務總覽

雲端基礎結構上的所有Adobe Commerce專案都包含存取New Relic服務，以協助監視效能和調查[!DNL Commerce]應用程式和雲端基礎結構的事件。

以下New Relic功能可用於生產和測試環境：

- [New Relic APM](#new-relic-apm) （Pro與Starter）
- [New Relic基礎結構](#new-relic-infrastructure) （僅限Pro）
- [New Relic記錄檔管理](#new-relic-log-management) （僅限Pro）

>[!INFO]
>
>Adobe Commerce專案沒有其他New Relic功能。

## NEW RELIC APM

[應用程式效能管理(APM)的New Relic](https://docs.newrelic.com/introduction-apm/)是軟體分析產品，可協助您分析和改善應用程式互動。 New Relic APM適用於雲端基礎結構專案的所有Adobe Commerce，並提供下列功能：

- **專注於特定交易** — 主動標示並監視您網站上的關鍵客戶動作，例如新增到購物車、結帳或處理付款。
- **資料庫查詢監視** — 尋找並監視影響效能的資料庫查詢。
- **應用程式對應** — 檢視您網站、擴充功能及外部服務中的所有應用程式相依性。
- **[!DNL Apdex]分數** — 評估績效並建立警示，識別問題並在發生時通知您，例如受快速銷售或網路事件影響的網站績效。 檢視[Apdex分數](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/)。
- **Adobe Commerce的管理警示** — 使用此New Relic警示原則，根據業界最佳實務來監控應用程式和基礎建設效能。 檢視[使用Adobe Commerce警示原則的Managed警示監視效能](investigate-performance.md/#monitor-performance-with-managed-alerts)。
- **追蹤部署** — 監視部署事件並分析部署對整體效能的影響。 請參閱[追蹤部署](track-deployments.md)。

您的雲端基礎結構專案上的Adobe Commerce包含New Relic APM服務的軟體以及授權金鑰。 您不需要購買或安裝任何其他軟體。

## New Relic基礎結構

Pro專案包含[New Relic基礎結構(NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/)服務，該服務會自動連線應用程式資料與效能分析，以提供動態伺服器監視。 此服務適用於Pro生產和中繼環境。

## New Relic記錄管理

所有雲端基礎結構專案都包含[New Relic記錄管理](log-management.md)。 此服務已預先設定為彙總測試和生產環境的所有記錄資料，並在集中式記錄管理儀表板中顯示。
