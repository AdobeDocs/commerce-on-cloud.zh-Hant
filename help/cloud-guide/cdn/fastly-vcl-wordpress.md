---
title: 將請求重新路由到CMS後端
description: 瞭解如何使用Fastly邊緣模組將來自Adobe Commerce商店的傳入請求重新路由到單獨的WordPress網站。
feature: Cloud, Configuration, Routes
exl-id: ef024c68-395b-4d47-9362-a8404a93dbbe
TQID: https://experienceleague.adobe.com/zRM-iTFGNPgSmT5xu1B9Lo3-onUtCHh-tVY-WPPiVC8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: d863fc70609dcc66d21eb95e709db80e29114714
workflow-type: tm+mt
source-wordcount: 322
ht-degree: 0%

---

# 將請求重新路由到CMS後端

使用Fastly Edge模組&#x200B;_其他CMS/後端整合_&#x200B;搭配Edge字典，將來自Adobe Commerce商店的傳入要求重新路由到不同的WordPress網站。 您可以依照類似的程式，將請求重新路由至其他CMS後端。

使用Fastly Edge模組從管理員建立和上傳自訂VCL程式碼，而不是手動撰寫VCL程式碼並使用Fastly API上傳。

>[!NOTE]
>
>我們建議您將自訂VCL設定新增到測試環境，您可以在其中測試它們，然後在生產環境中更新Fastly服務設定。

**必要條件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**若要將請求從Adobe Commerce重新路由到WordPress**：

1. 在中繼或生產環境中啟用Fastly Edge模組。

   - 登入管理員。

   - 瀏覽至&#x200B;**商店** >設定> **設定** > **進階** > **系統** > **完整頁面快取** > **快速設定** > **進階設定**。

   - 將&#x200B;**Fastly Edge模組**&#x200B;的值設定為&#x200B;**是**。

   - 儲存設定。

1. 識別要重新路由至WordPress後端的URL路徑。

1. 完成下列工作以設定Fastly服務並建立自訂VCL程式碼，將要求重新路由至WordPress後端。

   - 建立指定要從Edge存放區重新路由傳送到後端的Adobe Commerce字典。

   - 將WordPress後端新增至Fastly服務設定，並附加URL重寫的要求條件。

   - 設定&#x200B;_其他CMS/後端整合_ Edge模組，處理從Adobe Commerce到WordPress後端的URL重寫。

     如需詳細指示，請參閱Magento 2 _檔案的_ Fastly CDN模組中的[Fastly Edge模組 — 其他CMS/後端整合](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md)。

1. 更新Fastly服務設定後，請測試您的Adobe Commerce存放區，以確保WordPress的指定URL請求已正確重新路由。

<!-- Last updated from includes: 2025-01-27 17:16:28 -->

