---
title: New Relic記錄管理
description: 瞭解如何使用New Relic記錄檔
feature: Cloud, Logs, Observability
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# New Relic記錄管理

所有雲端基礎結構專案都包含[New Relic記錄管理](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)。 此服務已預先設定為彙總測試和生產環境的所有記錄資料，並在集中式記錄管理儀表板中顯示。

彙總資料包含來自以下記錄的資訊：

- 來自`~/var/log`目錄的所有`ece-tools`和應用程式記錄
- 來自`var/log/platform/<project-ID>`目錄的雲端服務記錄
- Fastly CDN和WAF

當您的專案已連線至New Relic時，您可以使用New Relic記錄檔服務來完成如下的工作：

- 使用New Relic查詢來搜尋彙總記錄檔資料
- 透過New Relic記錄檔應用程式視覺化記錄檔資料
- 建立自訂圖表、控制面板和警報
- 疑難排解單一控制面板的效能問題

## 檢視和分析記錄檔資料

使用New Relic記錄檔應用程式來搜尋彙總記錄檔資料，並疑難排解應用程式、基礎結構、CDN和WAF錯誤。 您可以使用從New Relic APM和基礎結構服務收集的記錄資料來建立圖表、控制面板和警報。

**若要使用New Relic記錄檔應用程式**：

1. 登入您的[New Relic帳戶](https://login.newrelic.com/login)。

1. 從Explorer導覽功能表選取&#x200B;**記錄檔**。

1. 確認您的帳戶已選取在&#x200B;_所有記錄_&#x200B;檢視的頂端。

1. 選取記錄檔查詢的時間範圍。

1. 若要檢閱雲端服務的基礎結構記錄檔資料（來自`~/var/log/`的記錄檔），請在&#x200B;_搜尋記錄檔_&#x200B;欄位中輸入查詢字串`has: "filePath"`。 然後，按一下&#x200B;**[!UICONTROL Query logs]**。

   記錄檔的名稱儲存在`filePath`欄中，包含記錄檔的完整路徑。

   ![雲端專案New Relic服務記錄檔資料](../../assets/new-relic/var-log-query.png)

1. 若要檢閱Fastly記錄檔資料，請在&#x200B;_搜尋記錄檔_&#x200B;欄位中輸入查詢字串`has: "client_ip"`。 然後，按一下&#x200B;**[!UICONTROL Query logs]**。

1. 若要依國家代碼篩選Fastly記錄結果，請按一下&#x200B;**[!UICONTROL Add column]**，然後選取&#x200B;**[!UICONTROL geo_country_code]**。

   ![雲端專案New Relic CDN記錄屬性篩選器](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>您可以從&#x200B;_已儲存的檢視_&#x200B;下拉式清單中儲存查詢檢視。 按一下&#x200B;**[!UICONTROL Create new]**、提供名稱、選取選項，然後按一下&#x200B;**[!UICONTROL Save view]**。
>
>請參閱&#x200B;_New Relic檔案_&#x200B;網站上的[開始使用記錄管理](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/)和[New Relic查詢語言簡介](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/)。
