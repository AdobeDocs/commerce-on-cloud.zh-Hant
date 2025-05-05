---
title: 存放區設定的最佳做法
description: 閱讀在Adobe Commerce雲端基礎結構上設定存放區的最佳實務。
feature: Cloud, Best Practices
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1087'
ht-degree: 0%

---

# 存放區設定的最佳做法

如需設定商店、網站和網站的詳細資訊，請檢閱[Adobe Commerce使用手冊](https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=zh-Hant)。 此頁面提供設定存放區、網站等的最佳實務、實用資訊和准則，以及隨著時間和不同版本張貼的其他內容。

## 行銷活動和促銷活動

此資訊有助於雲端基礎結構2.1.X和2.2.X上的Adobe Commerce。

若要建立行銷活動和促銷活動，請在[內容測試](https://experienceleague.adobe.com/docs/commerce-admin/content-design/staging/content-staging.html?lang=zh-Hant)中建立選項和設定。 此功能可讓您在將行銷活動公開給客戶銷售之前，先建立和預覽行銷活動。 下列資訊提供實用資訊。 如需確切的指示，請參閱連結的Adobe Commerce使用手冊內容。

_行銷活動_&#x200B;是季節性銷售、新產品線等行銷活動。 每個行銷活動可包含自訂主題、內容區塊、控制及顯示內容的Widget，以及與價格規則相關聯的行銷活動。 由於行銷活動的廣泛性質，您可以透過「內容分段」以開始和結束日期建立它們。

_促銷活動_&#x200B;提供折扣、單次優惠、優惠券、首次購買獎勵等等。 您建立這些促銷活動作為&#x200B;_價格規則_，設定條件、折扣和選項，以鼓勵客戶購買。 您可以在[購物車](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart.html?lang=zh-Hant)或[目錄](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog.html?lang=zh-Hant)上建立價格規則，並提供橫幅、獎勵點數等額外選項。 您可以針對促銷活動排程行銷活動，針對如新產品線或季節性銷售等主要活動套用價格規則。

以下是協助建立、更新和管理促銷活動和行銷活動的秘訣：

* 促銷活動可以是行銷活動的一部分。 行銷活動不能為促銷活動的一部分。 您可以將促銷清單作為價格規則使用，以搭配多個促銷活動使用多次。
* 當您建立促銷活動時，促銷活動一律會建立非作用中的初始促銷活動。 它有開始日期但沒有結束日期。 您可以忽略此初始行銷活動。 您可以使用正確的行銷活動排程來排程新的更新，並使其生效。
* 行銷活動具有開始和結束日期，而非促銷活動。 建立促銷時顯示的「排程器」不會設定促銷的開始和結束日期。 它可讓您在促銷活動的設定頁面上，針對此促銷活動排程促銷活動。
* 您無法直接在「階段內容」中編輯。 如果您必須編輯行銷活動中的設定和選項，請編輯原始或復本，並在「分段內容」中推送以覆寫。 例如，如果您不是行銷活動的結束日期，則必須編輯原始行銷活動並推播以更新。

## 進階定價與分段內容

此資訊有助於雲端基礎結構2.1.X和2.2.X上的Adobe Commerce。

一般而言，您可以透過Admin的&#x200B;**Products** > **Catalogs**&#x200B;區域為產品設定[進階價格](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/pricing/pricing-advanced.html?lang=zh-Hant)。 使用分階段內容，完成一些額外的步驟，將價格新增至促銷活動和行銷活動。

若要編輯「進階訂價」與更新「內容暫存」，請執行下列步驟：

1. 登入管理員。
1. 導覽至&#x200B;**產品** > **目錄**，然後選取產品並編輯。
1. 在[定價]索引標籤中，選取&#x200B;**進階定價**。 編輯價格並儲存變更。
1. 按一下頁面頂端的&#x200B;**排程新更新**。
1. 建立產品的促銷活動。
1. 完成促銷資訊。 在「排程器」中輸入開始和結束日期及時間。
1. 儲存促銷活動。 系統會建立非作用中的初始行銷活動。
1. 您可以預覽以檢閱行銷活動的特殊價格、促銷名稱、一般價格和排程日期範圍。

如需其他步驟，您可以繼續使用[型錄價格規則的排程變更](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rule-catalog-scheduled-changes.html?lang=zh-Hant)的說明。 按一下&#x200B;**下一步**&#x200B;逐步完成這些步驟。

## 價格規則

價格規則可包含邏輯和條件，如同行銷想像力一樣無限制。 熱門範例包括「買一送一」、「買一送一」50%優惠、超過$100美元的訂單有$25美元的折扣等等。

若要建立價格規則，請參閱[Adobe Commerce使用手冊](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/catalog-rules/price-rules-catalog-create.html?lang=zh-Hant)。

以下提供建立「僅限第一筆訂單」折扣的「價格規則」的範例。 針對此折扣，您需要：

* 以[客戶區段](https://experienceleague.adobe.com/zh-hant/docs/commerce-admin/customers/segments/customer-segment-price-rule)建立價格規則，條件為：訂單總數小於1
* 將此客戶區段新增為購物車規則的條件
* 選擇性 — 新增條件和規則，將折扣套用至特定的SKU或重點購買的產品類別

這可確保未購買的新客戶或現有客戶僅在第一筆訂單上獲得折扣。 您可以建立橫幅並傳送電子郵件促銷活動，以取得首次購買的折扣。

## 存放區檢視

您可以在雲端基礎結構上透過單一實施Adobe Commerce來設定和執行數個存放區。 請參閱[設定多個網站或商店](multiple-sites.md)。

對於彼此不互動的商店，您可以建立多個&#x200B;_網站_。 每個網站都有特定文章、客戶資料、結帳和購物車，沒有與Adobe Commerce中的其他網站共用。

每個網站可以包含一或多個&#x200B;_商店_，其中包含不同的類別和文章、共用的客戶資料、結帳和購物車。 對於這些商店，客戶只需結帳一次，即可在不同的產品目錄中購物。

此外，您也可以針對不同的語言、版面配置及設計建立&#x200B;_存放區檢視_。 在共用文章、客戶資料、結帳和購物車時，每個檢視可以有個別的網域、品牌和語言。

以下範例可更好地解釋：

* 單一網站有一個商店，且有兩個英文和西班牙文地區設定的檢視。 所有文章資料、客戶、結帳和購物車都會共用。

  ![存放區範例1](../../assets/example-store1.png)

* 設有女裝商店的單一網站包含兩個檢視：一個檢視英文，另一個檢視西班牙文。 童裝商店有英文版的單一商店檢視。 所有文章資料、客戶、結帳和購物車都會共用。 這些商店可能有不同的網域和主題。

  ![存放區範例2](../../assets/example-store2.png)

* 兩個網站，一個用於服裝，另一個用於擁有不同目錄和獨立文章、客戶資料和購物車的家庭裝飾。 每個網站都可以有多個商店和檢視，只能在該網站中分享文章、客戶資料、結帳和購物車。

  ![存放區範例3](../../assets/example-store3.png)

>[!WARNING]
>
>目錄資料會隨著您增加網站和商店的數量而擴展。 根據您的專案架構，其他存放區可能會導致非快取型目錄頁面的索引程式較長且回應時間較慢。 Adobe建議您密切監視網站效能。
