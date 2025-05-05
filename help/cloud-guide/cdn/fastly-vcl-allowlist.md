---
title: 允許要求的自訂VCL
description: 使用Fastly Edge ACL清單和自訂VCL程式碼片段，篩選傳入的請求並允許透過IP位址存取Adobe Commerce網站。
feature: Cloud, Configuration, Security
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# 允許要求的自訂VCL

您可以使用Fastly Edge ACL清單搭配自訂VCL程式碼片段，篩選傳入的請求並允許依IP位址存取。 ACL清單指定要允許的IP位址。

建立允許清單來限制對測試環境的存取，以便只允許來自指定之IP位址的請求，提供給內部開發人員和核准的外部服務。 您也可以建立允許清單，以安全地存取中繼和生產環境上的Admin。

下列範例說明如何使用具有[Fastly存取控制清單(ACL)](https://docs.fastly.com/guides/access-control-lists/about-acls)的自訂VCL程式碼片段，來保護雲端基礎結構專案環境上Adobe Commerce管理員的存取權。 當您將自訂VCL片段新增到雲端環境時，Fastly只允許來自ACL中包含的IP位址的請求。

>[!TIP]
>
>對於不應公開存取的測試和整合環境，請使用[[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface)中可用的HTTP存取控制選項，依IP位址管理對整個網站的存取。

**必要條件：**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- 要包含在允許清單中的使用者端IP位址清單

## 建立Edge ACL以允許使用者端IP位址

Edge ACL會建立IP位址清單，用於管理對您網站的存取。 在此範例中，您會建立Edge ACL，並新增允許存取專案環境管理員的使用者端IP位址清單。

{{admin-login-step}}

1. 按一下&#x200B;**商店** >設定> **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly設定** > **Edge ACL**。

1. 建立ACL容器：

   - 按一下&#x200B;**新增ACL**。

   - 在&#x200B;*ACL容器*&#x200B;頁面上，輸入&#x200B;**ACL名稱**—`allowlist`。

   - 選取&#x200B;**在變更後啟動**&#x200B;以將您的變更部署至您正在編輯的Fastly服務組態版本。

   - 按一下&#x200B;**上傳**&#x200B;將ACL附加至您的Fastly服務設定。

1. 新增允許存取管理員的IP位址清單：

   - 按一下`allowlist` ACL的[設定]圖示。

   - 新增並儲存每個使用者端IP位址的&#x200B;*IP值*。

   - 按一下&#x200B;**取消**&#x200B;以返回系統設定頁面。

1. 按一下&#x200B;**儲存設定**。

1. 根據頁面頂端的通知重新整理快取。

## 建立自訂VCL程式碼片段以保障管理員存取權

下列自訂VCL程式碼片段代碼（JSON格式）顯示邏輯，用於篩選給管理員的請求，以及允許使用者端IP位址符合`allowlist` ACL中的位址時允許存取。

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

在此範例中[建立自訂程式碼片段](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html?lang=zh-Hant#add-the-custom-vcl-snippet)之前，請檢閱值以判斷是否需要進行任何變更。 然後在個別欄位中輸入每個值，例如，在[型別]欄位中輸入`type`，在[內容]欄位中輸入`content`。

- `name` — VCL程式碼片段名稱。 在此範例中，`allowlist`。

- `priority` — 決定VCL程式碼片段何時執行。 優先順序為`5`，可立即執行並檢查管理員要求是否來自允許的IP位址。 程式碼片段會在任何預設MagentoVCL程式碼片段(`magentomodule_*`)被指派優先順序為50之前執行。 視您希望程式碼片段執行的時間而定，將每個自訂程式碼片段的優先順序設定為高於或低於50。 優先順序較低的程式碼片段會先執行。

- `type` — 指定將程式碼片段插入已建立版本VCL程式碼的位置。 此VCL是`recv`程式碼片段型別，將程式碼片段新增至預設Fastly VCL程式碼下方的`vcl_recv`副程式中，以及任何物件上方。

- `content` — 要執行的VCL程式碼片段。 在此範例中，程式碼會篩選對管理員的請求，並允許使用者端IP位址符合`allowlist` ACL中的位址時允許存取。 如果位址不符，要求會因`403 Forbidden`錯誤而遭到封鎖。

  如果管理員的URL已變更，請將範例值`/admin`取代為您的環境URL。 例如，`/company-admin`。

在程式碼範例中，使用[原始遮蔽](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)時，條件`!req.http.Fastly-FF`很重要。 請勿移除或編輯此程式碼。

檢閱並更新您環境的程式碼後，請使用下列其中一種方法，將自訂VCL程式碼片段新增至您的Fastly服務設定：

- [從Admin](#add-the-custom-vcl-snippet)新增自訂VCL程式碼片段。 如果您可以存取Admin，則建議使用此方法。 (需要Magento2 1.2.58[&#128279;](fastly-configuration.md#upgrade)或更新版本的Fastly CDN模組。)

- 將JSON程式碼範例儲存至檔案（例如，`allowlist.json`）並使用Fastly API[&#128279;](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api)上傳。 如果您無法存取Admin，請使用此方法。

## 新增自訂VCL片段

{{admin-login-step}}

1. 按一下&#x200B;**商店** >設定> **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly組態** > **自訂VCL程式碼片段**。

1. 按一下&#x200B;**建立自訂程式碼片段**。

1. 新增VCL程式碼片段值：

   - **名稱** — `allowlist`

   - **型別** — `recv`

   - **優先順序** — `5`

   - 新增&#x200B;**VCL**&#x200B;程式碼片段內容：

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. 按一下&#x200B;**建立**&#x200B;以產生名稱模式為`type_priority_name.vcl`的VCL程式碼片段檔案，例如`recv_5_allowlist.vcl`

1. 頁面重新載入後，按一下&#x200B;*Fastly組態*&#x200B;區段中的&#x200B;**上傳VCL到Fastly**，以將檔案新增到Fastly服務組態。

1. 上傳完成後，請根據頁面頂端的通知重新整理快取。

Fastly會在上傳過程中驗證VCL程式碼的更新版本。 如果驗證失敗，請編輯自訂VCL程式碼片段以修正問題。 然後，再次上傳VCL。

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
