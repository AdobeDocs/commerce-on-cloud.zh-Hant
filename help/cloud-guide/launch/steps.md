---
title: 啟動步驟
description: 瞭解如何完成網站啟動。
exl-id: e7a3cd6b-32de-4fd0-9fbd-da8299e77114
TQID: https://experienceleague.adobe.com/Nl8YFJUZUkxtsm0eqBgJklsTysKuUE31PkEDsJmvBMU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 0%

---

# 啟動步驟

測試並完成啟動檢查清單後，您就可以開始啟動的最後步驟。 這些步驟包括輸入票證、傳輸網站存取權，以及最後在商店上線時測試。

Adobe支援人員會透過程式與您合作、檢查狀態，並在發生任何問題時提供協助。 應使用票證追蹤所有問題，以便最妥善地擷取所發生的情況以及問題的解決方式。 當您開始持續反複地將更新部署到已啟動商店時，可能會再次發生類似問題。 這些票證有助於查明問題並幫助調整部署任務。

## 聯絡Adobe以啟動您的網站

聯絡Adobe Commerce支援，並以預期的日期和時間更新任何網站啟動（上線）票證，以切換並啟動您的商店。

## 將DNS切換至新站台

存留時間變更值對於檢查您變更的網域很重要。 當您修改A和CNAME記錄時，更新需要TTL設定的時間才能正確更新。 如需DNS設定的詳細資訊，請參閱[DNS設定](checklist.md#update-dns-configuration-with-production-settings)。

### 若要變更為新網站：

1. 存取您的DNS服務。

1. 更新網域和主機名稱的A和CNAME記錄。

1. 請等候TTL時間通過，然後重新啟動您的網頁瀏覽器。

1. 使用storefront網域存取您的商店。

## 測試即時存放區

在即時存放區中完成一些UAT測試，以確認所有內容都在載入中，且動作正確完成。 如需測試清單，請參閱[測試部署](../test/staging-and-production.md#complete-uat-testing)。

## 啟動後

Adobe會檢查及監控網站，確保所有服務和存取許可權皆為綠色。 我們會隨時為您提供所需資訊，協助您瀏覽及檢查所有系統記錄、服務、快取及功能是否都能依您及客戶需求運作。

如果發生任何問題，請透過支援建立並追蹤問題。 儘可能加入更多資訊，包括日期/時間、有問題的特定功能、症狀和奇怪的行為、擴充功能等。 我們會調查記錄、問題，並與您合作以儘快解決。
