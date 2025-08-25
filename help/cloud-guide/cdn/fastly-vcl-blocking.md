---
title: 封鎖要求的自訂VCL
description: 使用具有自訂VCL片段的Edge存取控制清單(ACL)，依IP位址封鎖傳入要求。
feature: Cloud, Configuration, Security
exl-id: eb21c166-21ae-4404-85d9-c3a26137f82c
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# 封鎖要求的自訂VCL

您可以使用適用於Magento 2的Fastly CDN模組來建立Edge ACL，其中包含您要封鎖的IP位址清單。 然後，您可以將該清單與VCL程式碼片段搭配使用，以封鎖傳入的請求。 此程式碼會檢查傳入要求的IP位址。 如果它符合ACL清單中包含的IP位址，Fastly會封鎖存取您網站的請求，並傳回`403 Forbidden error`。 允許存取所有其他使用者端IP位址。

**必要條件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要封鎖的使用者端IP位址清單

## 建立Edge ACL以封鎖使用者端IP位址

您可以建立Edge ACL，定義要封鎖的IP位址清單。 建立ACL之後，您可以在自訂VCL程式碼片段中使用它來管理對測試或生產網站的存取。

透過在兩個環境中建立具有相同名稱的Edge ACL來管理中繼和生產網站的存取權。 VCL程式碼片段程式碼適用於兩種環境。

1. 登入管理員。
1. 導覽至&#x200B;**商店** >設定> **設定** > **進階** > **系統** > **全頁快取** > **快速設定**。
1. 展開&#x200B;**Edge ACL**&#x200B;區段。
1. 按一下&#x200B;**新增ACL**&#x200B;以建立清單。 在此範例中，將清單命名為「blocklist」。
1. 在清單中輸入IP位址值。 所有新增至此清單的使用者端IP位址都會遭到封鎖，無法存取網站。
1. 視需要選取&#x200B;**否定**&#x200B;核取方塊。

您可以在VCL程式碼片段中依名稱參照Edge ACL。

## 建立封鎖清單的自訂VCL

>[!NOTE]
>
>此範例向進階使用者說明如何建立VCL程式碼片段，以設定自訂封鎖規則以上傳到Fastly服務。 您可以使用Magento 2模組的Fastly CDN提供的[封鎖](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)功能，根據Adobe Commerce管理員提供的國家/地區來設定封鎖清單或允許清單。

定義Edge ACL後，您可以使用它來建立VCL程式碼片段，以封鎖對ACL中所指定IP位址的存取。 您可以在測試環境和生產環境中使用相同的VCL程式碼片段，但必須分別將程式碼片段上傳至每個環境。

以下自訂VCL片段代碼（JSON格式）顯示邏輯，用於封鎖使用者端IP位址符合封鎖清單ACL位址的傳入請求。

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

根據此範例建立程式碼片段之前，請檢閱值以判斷是否需要進行任何變更：

- `name`： VCL程式碼片段的名稱。 在此範例中，我們使用名稱`blocklist`。

- `priority`：決定VCL程式碼片段何時執行。 優先順序為`5`，可立即執行並檢查管理員要求是否來自允許的IP位址。 程式碼片段會在任何預設Magento VCL程式碼片段(`magentomodule_*`)被指派優先順序為50之前執行。 視您希望程式碼片段執行的時間而定，將每個自訂程式碼片段的優先順序設定為高於或低於50。 優先順序較低的程式碼片段會先執行。

- `type`：指定VCL程式碼片段的型別，以決定所產生VCL程式碼中程式碼片段的位置。 在此範例中，我們使用`recv`，它將VCL程式碼插入`vcl_recv`副程式中，在樣板VCL的下方及任何物件上方。 如需程式碼片段型別的清單，請參閱[Fastly VCL程式碼片段參考](https://docs.fastly.com/api/config#api-section-snippet)。

- `content`：要執行的VCL程式碼片段，會檢查使用者端IP位址。 如果IP位在Edge ACL中，則會封鎖整個網站的存取權，並產生`403 Forbidden`錯誤。 允許存取所有其他使用者端IP位址。

檢閱並更新您環境的程式碼後，請使用下列其中一種方法，將自訂VCL程式碼片段新增至您的Fastly服務設定：

- [從Admin](#add-the-custom-vcl-snippet)新增自訂VCL程式碼片段。 如果您可以存取Admin，則建議使用此方法。 （需要[Fastly 1.2.58](fastly-configuration.md#upgrade-fastly-module)或更新版本。）

- 將JSON程式碼範例儲存至檔案（例如，`blocklist.json`）並使用Fastly API[上傳](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)。 如果您無法存取Admin，請使用此方法。

## 新增自訂VCL片段

{{admin-login-step}}

1. 按一下&#x200B;**商店** >設定> **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly組態** > **自訂VCL程式碼片段**。

1. 按一下&#x200B;**建立自訂程式碼片段**。

1. 新增VCL程式碼片段值：

   - **名稱** — `blocklist`

   - **型別** — `recv`

   - **優先順序** — `5`

   - 新增&#x200B;**VCL**&#x200B;程式碼片段內容：

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. 按一下&#x200B;**建立**&#x200B;以產生名稱模式為`type_priority_name.vcl`的VCL程式碼片段檔案，例如`recv_5_blocklist.vcl`

1. 頁面重新載入後，按一下&#x200B;**Fastly組態**&#x200B;區段中的&#x200B;*上傳VCL到Fastly*，以將檔案新增到Fastly服務組態。

1. 上傳後，請根據頁面頂端的通知重新整理快取。

Fastly會在上傳過程中驗證VCL程式碼的更新版本。 如果驗證失敗，請編輯自訂VCL程式碼片段以修正問題。 然後，再次上傳VCL。

## 封鎖要求的其他VCL範例

以下範例說明如何使用內嵌條件陳述式而非ACL清單來封鎖請求。

>[!WARNING]
>
>在這些範例中，VCL程式碼會格式化為JSON裝載，可儲存至檔案並以Fastly API請求提交。 您可以從Admin[提交](#add-the-custom-vcl-snippet)VCL程式碼片段，或使用Fastly API以JSON字串形式提交。 若要避免在搭配JSON字串使用Fastly API時出現驗證錯誤，您必須使用反斜線來逸出特殊字元。

>[!NOTE]
>如果您要從Admin提交VCL程式碼片段，請從範例VCL程式碼中擷取個別值，並將它們輸入到對應的欄位中。 例如：
>- 名稱： `<name of the VCL>`
>- 動態： `<0/1>`
>- 型別： `<type>`
>- 優先順序： `<priority>`
>- 內容： `<content>`

請參閱Fastly VCL檔案中的[使用動態VCL片段](https://docs.fastly.com/vcl/vcl-snippets/)。

### VCL程式碼範例：依國家/地區程式碼分塊

此範例針對與IP位址相關聯的國家/地區使用兩個字元的ISO 3166-1國碼。

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>您可以透過雲端基礎結構管理員上的Adobe Commerce中的Fastly [封鎖](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md)功能，設定依國家/地區代碼或國家/地區代碼清單進行封鎖，而不使用自訂VCL程式碼片段。

### VCL程式碼範例： Block by HTTP User-Agent要求標頭

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
