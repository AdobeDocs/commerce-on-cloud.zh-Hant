---
title: 升級專案的最佳實務
description: 檢視升級專案檔案的最佳實務清單。
feature: Cloud, Best Practices, Upgrade
exl-id: 64f92739-9170-4cbf-90ef-aab6593a37ca
source-git-commit: 31494a956babaf15320d0ffa86fcba9e845d53a1
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# 升級專案的最佳實務

遵循建置和部署的最佳實務，並使用[升級和修補程式](../development/commerce-version.md)工作流程來升級您的應用程式。 請使用下列准則來規劃您的升級與升級後的工作：

- **備份您的專案** — 在升級Adobe Commerce及任何協力廠商或自訂擴充功能之前，請先在整合、測試和生產環境中備份資料庫。 請參閱[備份資料庫](../development/commerce-version.md#project-backup)。

- **檢查相容性問題**-

   - 確認所有自訂主題都與新的Adobe Commerce版本相容

   - 升級協力廠商和自訂擴充功能後，請先使用`magento-cloud local:build`命令驗證撰寫器相依性，再進行部署，然後執行[升級相容性工具](#use-the-upgrade-compatibility-tool)，找出您目前版本和目標版本之間的程式碼層級不相容情形。 然後使用[升級相容性工具](https://fluffyjaws.adobe.com/#use-the-upgrade-compatibility-tool)，在部署至整合、中繼或生產之前，識別並優先處理程式碼層級的不相容性。

   - 請檢閱Adobe Commerce發行說明和擴充功能檔案，以確保您已實作任何必要的因應措施或設定變更，以解決與升級Adobe Commerce版本和擴充功能相關的已知功能問題和錯誤。

   - 請確認已安裝的服務版本與新的Adobe Commerce版本相容，並視需要升級服務。 請參閱[服務](../services/services-yaml.md)。

   - 測試您的資料庫，解決Adobe Commerce版本和擴充功能更新所導致的任何問題。

   - 在部署到遠端環境之前，對環境特定的設定進行任何必要的更新。

   - 請確定搜尋服務版本與PHP使用者端版本相容。 請參閱[設定Elasticsearch](../services/elasticsearch.md)或[設定OpenSearch](../services/opensearch.md)。

- **檢查遠端環境中的資料庫連線能力與可用儲存空間**-

   - 使用SSH登入遠端伺服器並驗證與MySQL資料庫的連線。 請參閱[連線到資料庫](../services/mysql.md#connect-to-the-database)。

   - 驗證遠端環境中的可用儲存空間 — 使用`disk free`命令檢視和管理雲端環境中的可用磁碟空間。 請參閱[管理磁碟空間](../storage/manage-disk-space.md)。

      - 檢查升級資料庫的大小，並確認`services.yaml`檔案有足夠的磁碟空間配置。

      - 釋放磁碟空間 — 清除快取，並在部署之前清除`/log`和`/tmp`目錄。

- **在本機和整合環境中規劃並執行成功的升級，然後部署到中繼環境** — 在升級之後，測試您的部署並解決任何問題。

- 將程式碼合併到測試環境，然後再合併到生產環境&#x200B;**— 在將變更推送到生產環境之前，請先測試並解決測試環境中的任何問題。**

- **完成升級後的工作**-

   - 使用SSH登入遠端伺服器並驗證下列專案：

      - 視需要檢查索引器狀態並重新索引。 請參閱&#x200B;_設定指南_&#x200B;中的[管理索引子](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html)。

      - 檢查Adobe Commerce資料庫中的`cron`記錄檔和`cron_schedule`資料表以驗證cron狀態，並視需要重新執行cron工作。
請參閱_設定指南_&#x200B;中的[記錄](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging)。

   - 在中繼和生產環境中完成升級後使用者驗收測試UAT，並修正與協力廠商和自訂擴充功能升級相關的任何問題。

## 使用升級相容性工具

執行升級相容性工具(UCT)作為升級前分析的一部分，以瞭解升級的範圍和影響。

- UCT會將您目前的執行個體與目標Adobe Commerce版本比較，傳回在升級之前必須修正的嚴重問題、錯誤和警告清單。
- 使用`--coming-version (-c)`與您的計畫目標版本進行比較，並使用`--ignore-current-version-compatibility-issues`僅聚焦於升級所引入的新問題。
- 將UCT HTML報表視為升級檢查清單的輸入專案，以及擴充功能相容性、服務版本和資料庫檢查。

如需設定和使用情況的詳細資訊，請參閱：

- [升級相容性工具概覽](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/overview)
- [執行升級相容性工具](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/run)

對於使用全網站分析工具的雲端商家，您也可以從控制面板觸發UCT，並直接從Widget下載HTML報表。 請參閱整合[全網站分析工具](https://experienceleague.adobe.com/en/docs/commerce-operations/upgrade-guide/upgrade-compatibility-tool/use-upgrade-compatibility-tool/integrate-analysis-tool)。
