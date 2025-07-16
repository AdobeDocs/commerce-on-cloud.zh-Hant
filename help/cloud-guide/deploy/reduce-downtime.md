---
title: 零停機部署
description: 瞭解如何在雲端基礎結構專案上部署Adobe Commerce時減少整體停機時間。
feature: Cloud, Deploy, SCD, Themes
exl-id: c216c5e9-d787-4428-b67a-b6aee814ded5
source-git-commit: b831bc5bce0f76ec8972b3578c500508dd4d7d41
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 0%

---

# 零停機部署

雲端基礎結構上的Adobe Commerce會在部署階段期間以&#x200B;[_維護_&#x200B;模式](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html?lang=zh-Hant#production-mode)執行應用程式，讓您的網站離線，直到部署完成。 您的生產網站處於維護模式的時間長度取決於網站大小、部署期間套用的變更數量，以及靜態內容部署的設定。 您可以設定專案，使其部署時具有&#x200B;**零**&#x200B;停機時間效應。

在部署過程中，所有連線都會佇列長達5分鐘，保留任何作用中工作階段和擱置中的動作，例如加入購物車或結帳。 部署後，佇列會釋放，連線會持續進行而不會中斷。 若要使用此&#x200B;_連線保留_&#x200B;以利您並將部署減少到&#x200B;_零_&#x200B;停機時間，您必須設定專案以使用最有效的部署策略。

>[!NOTE]
>
>若要確認您的Cloud專案是否已最佳設定為將部署停機時間減至最少，請使用[智慧精靈](smart-wizards.md)。 智慧型精靈會檢查您目前的設定，並引導您進行建議的設定調整，以啟用零停機部署的最佳實務。

使用下列步驟來減少存放區將更新部署到生產環境所需的時間：

1. [升級至`ece-tools`封裝](../dev-tools/install-package.md)或[更新`ece-tools`版本](../dev-tools/update-package.md)
您的雲端基礎結構專案上的Adobe Commerce必須有最新的`ece-tools`套件，好讓您可以利用工具來設定最佳部署。 如果您有最新的`ece-tools`，請繼續進行下一個步驟。

   >[!NOTE]
   >
   >即使使用最新`ece-tools`套件是最佳做法，零停機部署方法可搭配`ece-tools` [版本2002.0.13](../release-notes/cloud-release-archive.md#v2002013)和更新版本使用。

1. [設定靜態內容部署](static-content.md)
如果靜態內容部署在部署階段失敗，您的網站會卡在維護模式。 當在建置階段期間發生失敗時，該流程會避免停機，因為它永遠不會開始部署階段。 [使用極簡化的HTML在建置階段產生靜態內容](static-content.md#setting-the-scd-on-build) （也稱為理想狀態）是零停機部署的最佳設定，如果發生失敗，_可避免_&#x200B;停機時間。

1. [設定部署後連結](../application/hooks-property.md)
您必須設定部署後掛接以清除並預熱快取。 根據預設，當網站關閉時，快取清理會在部署階段進行。 將快取清理移至部署後階段表示您的快取會維持作用中狀態，直到部署階段完成，然後您就可以安全地清理快取。

   使用[WARM_UP_PAGES環境變數](../environment/variables-post-deploy.md#warmuppages)自訂用來預先載入快取的頁面清單。

1. [減少佈景主題檔案](../environment/variables-deploy.md#scdmatrix)
您可以藉由設定SCD\_MATRIX環境變數來減少不必要的佈景主題檔案數目。

1. [加速靜態內容部署](../environment/variables-deploy.md#scdthreads)
您可以更新SCD\_THREADS環境變數來增加靜態內容部署的執行緒數量，以加快部署程式。

>[!NOTE]
>
>您可以透過執行[理想狀態精靈](smart-wizards.md#verifying-an-ideal-configuration)，來驗證您的專案組態是否為最佳部署。
