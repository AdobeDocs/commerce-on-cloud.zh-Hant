---
title: 在雲端上布建Commerce
description: 瞭解如何準備Adobe客戶技術顧問，以在雲端基礎結構專案上布建您的Adobe Commerce。
recommendations: noDisplay, catalog
role: Admin
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 0%

---

# 雲端布建先決條件上的Commerce

讓我們開始並在雲端基礎結構上初始化您的Commerce專案！

在雲端專案環境中Adobe布建Commerce之前，建議您考量下列策略並準備答案，以首次諮詢Adobe帳戶團隊。 請使用下列章節作為檢查清單，協助您準備與客戶技術顧問的對話，以布建雲端專案：

## 網域定義

**問題1**： _您打算使用哪些網域或網域來啟動網站？_

為Pro測試和生產環境定義您的頂層網域和子網域，以準備整合Fastly和nginx服務。 初次設定後，您只能透過提交Adobe Commerce支援票證來新增網域，因此建議您準備好網域資訊。

生產網域和測試網域的範例：

- `www.your-store.com`
- `your-store.com`
- `mcprod.your-store.com`
- `mcstaging.your-store.com`

請參閱&#x200B;_雲端基礎結構上的Commerce_&#x200B;指南中的[設定多個網站或商店](../cloud-guide/store/multiple-sites.md)，以取得多個或唯一網域的進一步指引。

如果您有現有的Fastly帳戶連結您Adobe Commerce網站上使用的相同頂點和子網域，請參閱[多個Fastly帳戶和指派的網域](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/cdn/fastly#multiple-fastly-accounts-and-assigned-domains){target="_blank"}。

## 異動電子郵件網域

**問題2**： _您打算使用哪些網域或網域來處理異動電子郵件？_

雲端上的Adobe Commerce使用SendGrid簡單郵件傳輸通訊協定(SMTP) Proxy服務，此服務提供傳出電子郵件驗證和信譽監視服務。 SendGrid會代表您傳送交易式電子郵件，因此需要網域資訊。

SendGrid網域的範例： `example@your-store.com`

請參閱&#x200B;_雲端基礎結構上的Commerce_&#x200B;指南中的[SendGrid郵件服務](../cloud-guide/project/sendgrid.md)，取得異動電子郵件和網域設定的進一步指引。

## 儲存空間配置

**問題3**： _您打算為檔案上傳和資料庫配置多少合約儲存空間？_

雲端基礎結構上的Adobe Commerce使用儲存空間作為檔案結構、搜尋索引和資料庫。 您可以視需要從每個磁碟分割的10 GB開始將儲存裝置細分。

您為雲端專案所訂約的儲存空間量，代表每個環境的總儲存空間。 例如，如果您購買50 GB的儲存空間，則每個環境有50 GB的儲存空間。 生產、測試和每個整合環境分別有50 GB的個別儲存空間。

您可以隨時增加合約儲存空間。 對於Pro生產和中繼環境，您必須提交Adobe Commerce支援票證以變更磁碟空間分配。 Pro生產和中繼環境的大小增加只能在特定的間隔進行。 根據您目前的磁碟空間使用量，支援團隊可能會建議將磁碟空間配置增加至少10 GB。 配置之後，Pro Staging和生產環境的儲存空間增加無法&#x200B;**還原**。

請參閱&#x200B;_雲端基礎結構上的Commerce_&#x200B;指南中的[管理磁碟空間](../cloud-guide/storage/manage-disk-space.md)。

## 雲端服務區域

**問題4**： _哪個雲端服務區域最方便接近您？_

在雲端基礎結構Pro專案上，選擇Amazon Web Services (AWS)或Microsoft Azure作為您Adobe Commerce的基礎結構即服務(IaaS)基礎。 每個服務提供者會在多個區域運作，並提供多個可用區域。 選擇適合您所在位置的區域，降低延遲和成本提升的可能性。

檢視[Adobe Commerce雲端區域](../cloud-guide/overview.md)的地圖。

## 連線服務

**問題5**： _您需要PrivateLink服務嗎？ 若是，PrivateLink連線位於哪個區域？_

雲端基礎結構上的Adobe Commerce支援與AWS PrivateLink或Azure Private Link服務整合。 雖然此服務為選購專案，PrivateLink仍可用來在雲端基礎結構環境（服務與託管於外部系統的應用程式）之間建立安全的私人通訊。

請務必考量您的雲端服務策略(AWS或Azure)，以便Adobe Commerce執行個體可在相同地區記憶體取。 如需地區協助工具的進一步說明，請參閱&#x200B;_雲端基礎結構上的Commerce_&#x200B;指南中的[PrivateLink服務](../cloud-guide/development/privatelink-service.md)。

## Target網站啟動

**問題6**： _您預計的目標啟動日期是多久？_

啟動網站需要反複的設定和測試，以確保您的網站啟動成功。 設定目標日期可協助您和您的Adobe帳戶團隊為最後啟動前的活動做好準備，其中包括協調最終步驟的呼叫。

請參閱&#x200B;_雲端基礎結構上的Commerce_&#x200B;指南中的[啟動網站概觀](../cloud-guide/launch/overview.md)，以檢閱完整程式並下載啟動檢查清單副本。

>[!TIP]
>
> 快速檢視雲端入口網站，並存取您的新雲端專案。
>
>**下一步**： [加入Commerce](onboarding.md)
