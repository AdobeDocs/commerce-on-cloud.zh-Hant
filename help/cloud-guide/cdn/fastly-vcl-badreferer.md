---
title: 封鎖轉介垃圾訊息
description: 使用Fastly Edge字典和自訂VCL程式碼片段，封鎖來自您網站的轉介垃圾郵件。
feature: Cloud, Configuration, Security
exl-id: 4ed47a71-7fee-4f37-a7da-3e30052004df
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# 封鎖轉介垃圾訊息

下列範例說明如何使用自訂VCL程式碼片段設定[Fastly Edge Dictionary](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api)，以封鎖雲端基礎結構網站上來自Adobe Commerce的反向連結垃圾訊息。

>[!NOTE]
>
>我們建議您將自訂VCL設定新增到測試環境，您可以在測試環境中測試它們，然後再針對生產環境執行它們。

**必要條件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 檢閱您的網站記錄檔以找出虛假的轉介URL，並建立要封鎖的網域清單。

## 建立反向連結封鎖清單

Edge字典會建立VCL程式碼片段處理期間VCL函式可存取的鍵值值組。 在此範例中，您會建立邊緣字典，提供要封鎖的反向連結網站清單。

{{admin-login-step}}

1. 按一下&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly設定** > **Edge字典**。

1. 建立字典容器：

   - 按一下&#x200B;**新增容器**。

   - 在&#x200B;*容器*&#x200B;頁面上，輸入&#x200B;**字典名稱**—`referrer_blocklist`。

   - 選取&#x200B;**在變更後啟動**&#x200B;以將您的變更部署至您正在編輯的Fastly服務組態版本。

   - 按一下&#x200B;**上傳**，將字典附加至您的Fastly服務組態。

1. 新增要封鎖的網域名稱清單至`referrer_blocklist`字典：

   - 按一下`referrer_blocklist`字典的[設定]圖示。

   - 在新字典中新增並儲存索引鍵/值組。 在此範例中，每個&#x200B;**Key**&#x200B;是要封鎖的反向連結URL的網域名稱，**值**&#x200B;是`true`。

     ![新增錯誤的反向連結字典專案](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - 按一下&#x200B;**取消**&#x200B;以返回系統設定頁面。

1. 按一下&#x200B;**儲存設定**。

1. 根據頁面頂端的通知重新整理快取。

如需Edge字典的詳細資訊，請參閱Fastly檔案中的[建立及使用Edge字典](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api)和[自訂VCL片段](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples)。

## 建立自訂VCL程式碼片段以封鎖反向連結垃圾訊息

下列自訂VCL片段程式碼（JSON格式）顯示檢查及封鎖要求的邏輯。 VCL程式碼片段會將反向連結網站的主機擷取到標題中，然後將主機名稱與`referrer_blocklist`字典中的URL清單比較。 如果主機名稱相符，則封鎖要求並出現`403 Forbidden`錯誤。

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if (req.http.Referer ~ \"^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$\") {set req.http.Referer-Host = re.group.2;}if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {error 403 \"Forbidden\";}"
}
```

根據此範例建立程式碼片段之前，請檢閱值以判斷是否需要進行任何變更：

- `name` — VCL程式碼片段名稱。 在此範例中，我們使用`block_bad_referrer`。

- `dynamic` — 值0表示[一般程式碼片段](https://docs.fastly.com/en/guides/using-regular-vcl-snippets)，可上傳到Fastly設定的已建立版本VCL。

- `priority` — 決定VCL程式碼片段何時執行。 優先順序為`5`，可在指派優先順序為50的任何預設Magento VCL程式碼片段(`magentomodule_*`)之前執行此程式碼片段。 視您希望程式碼片段執行的時間而定，將每個自訂程式碼片段的優先順序設定為高於或低於50。 優先順序較低的程式碼片段會先執行。

- `type` — 指定在VCL版本中插入程式碼片段的位置。 在此範例中，VCL片段是`recv`片段。 將程式碼片段插入VCL版本時，它會新增到`vcl_recv`副程式中、預設Fastly VCL程式碼的下方，以及任何物件的上方。

- `content` — 要在一行中執行的VCL程式碼片段，沒有分行符號。

檢閱並更新您環境的程式碼後，請使用下列其中一種方法，將自訂VCL程式碼片段新增至您的Fastly服務設定：

- [從Admin](#add-the-custom-vcl-snippet)新增自訂VCL程式碼片段。 如果您可以存取Admin，則建議使用此方法。 （需要[Fastly 1.2.58](fastly-configuration.md#upgrade)或更新版本。）

- 將JSON程式碼範例儲存至檔案（例如，`allowlist.json`）並使用Fastly API[上傳](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)。 如果您無法存取Admin，請使用此方法。

## 新增自訂VCL片段

{{admin-login-step}}

1. 按一下&#x200B;**商店** >設定> **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly組態** > **自訂VCL程式碼片段**。

1. 按一下&#x200B;**建立自訂程式碼片段**。

1. 新增VCL程式碼片段值：

   - **名稱** — `block_bad_referrer`

   - **型別** — `recv`

   - **優先順序** — `5`

   - **VCL**&#x200B;程式碼片段內容 — 

     ```conf
     if (req.http.Referer ~ "^(.*:)//([A-Za-z0-9\-\.]+)(:[0-9]+)?(.*)$") {
       set req.http.Referer-Host = re.group.2;  
     }
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. 按一下&#x200B;**建立**。

   ![建立自訂反向連結區塊VCL程式碼片段](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. 頁面重新載入後，在&#x200B;**Fastly組態**&#x200B;區段中按一下&#x200B;*上傳VCL到Fastly*。

1. 上傳完成後，請根據頁面頂端的通知重新整理快取。

Fastly會在上傳過程中驗證更新的VCL版本。 如果驗證失敗，請編輯您的自訂VCL程式碼片段以修正任何問題。 然後，再次上傳VCL。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
