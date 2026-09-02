---
source-git-commit: 79ac13115bd3f275651a5477f2939c8f00a5a985
workflow-type: tm+mt
source-wordcount: '704'
ht-degree: 0%

---
# 專業服務支援與客戶可用性

## Pro服務支援

若要在「測試」或「生產」中請求並完成Pro服務升級，請遵循下列步驟：

1. **若要僅在`Staging`和`Production`環境中安裝或更新[服務](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml)**，請提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)。

   在票證中，指定所需的服務變更，包括更新的`.magento.app.yaml`和`.magento/services.yaml`檔案，並記下目標PHP版本。

   PHP版本、Composer更新、擴充功能和環境設定是自助服務變更。 Adobe可能需要更新New Relic代理程式，以取得PHP版本的相容性。 檢視&#x200B;_應用程式組態_&#x200B;中的[PHP設定](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/php-settings)。

   >[!IMPORTANT]
   >
   >在票證表單中選取&#x200B;**[!UICONTROL Environment]**&#x200B;欄位時，請使用Adobe的環境命名。 例如，即使您在內部呼叫該環境&#x200B;**Dev**，也選取「暫存」。 您可以在說明中提及您的內部名稱，但[!UICONTROL Environment]欄位必須使用Adobe的命名法。

1. **透過Adobe的兩部分程式確認升級排程**：您先確認要求的日期與時間，然後支援將之提交至基礎結構團隊以進行最終確認。

   生產變更（僅限Pro）需要至少提前兩個營業日通知，週末除外。 例如，Cloud Infrastructure團隊必須在前一個星期三前確認星期一升級。 在需求尖峰期間預期額外的前置時間。 為避免延遲，請在視窗之前至少48小時回應初始請求。 在您收到最終確認之前，不會將升級視為已排程。

   >[!NOTE]
   >
   >提供UTC格式的維護時段。 中繼升級不會預先排程，通常會在請求當天完成。
   >
   >RabbitMQ升級後，重新部署環境以重新初始化訊息佇列。

1. **先在測試或整合環境中驗證升級**，再在生產環境中進行排程。

   在服務升級後的重新部署期間，由協力廠商模組、自訂程式碼或相依性相容性所導致的問題經常會出現。 若要一次驗證多個服務升級，合理的順序為Valkey或Redis、RabbitMQ、OpenSearch和MariaDB。 這不是必要順序。 資料庫升級具有最高的作業影響，因此應受到最慎重的影響。

   Adobe無法保證生產維護期間能提早完成，因為時間取決於環境及所涉及的服務。 在規劃「生產」視窗時，請使用測試升級所花的時間作為實際估計。

1. **在Adobe完成服務升級後，重新部署環境**，讓變更生效，即使Adobe Commerce應用程式版本未變更。

   如果升級包含OpenSearch，也要計畫完整重新索引。 Adobe無法保證服務升級的零停機時間，因此請規劃維護時段，以便有時間重新部署、視需要重新索引，以及在重新開啟網站之前驗證店面和管理員。

## 客戶在升級期間的可用性

**在排定的生產升級期間，您的團隊或實作合作夥伴的代表必須線上上。** 在低流量期間進行排程並不會讓升級作業自動進行。 Adobe可管理雲端基礎結構升級，但無法驗證您的應用程式行為、整合、自訂程式碼或業務工作流程。

可用的代表必須能夠：

- **監視**&#x200B;升級期間和升級後的店面與重要業務交易。
- **回應** Adobe支援或雲端基礎結構團隊的問題。
- **確認**&#x200B;整合、擴充功能、自訂、cron工作、佇列和其他客戶特定功能是否如預期般運作。
- **驗證**&#x200B;業務關鍵工作流程，例如簽出、目錄檢視、搜尋、登入及訂單處理。
- 當升級內容與記錄檔仍然可用時，立即報告&#x200B;**意外行為**。

>[!TIP]
>
>對於Pro專案，「生產」中的服務升級也需要預先排程和包含Adobe支援的兩部分確認流程。 請參閱[專業服務支援](#pro-services-support)。

### 維護模式

**維護模式無法取代客戶可用性。** 維護模式會封鎖店面存取，但不會驗證應用程式服務、整合、佇列、cron工作、結帳或其他客戶專屬功能。

如果計畫工作需要維護模式，請與Adobe支援協調其使用，並遵循該升級的指示。 之後，在考量工作完成之前，請確認店面與重要工作流程正常運作。
