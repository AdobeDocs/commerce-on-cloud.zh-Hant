---
title: 開始使用自訂VCL程式碼片段
description: 瞭解如何使用Varnish控制語言程式碼片段來自訂Adobe Commerce的Fastly服務設定。
feature: Cloud, Configuration, Services
exl-id: 90f0bea6-4365-4657-94e9-92a0fd1145fd
source-git-commit: 71fb8f5b3f32553d8b247de44fea29b1bb945584
workflow-type: tm+mt
source-wordcount: '2037'
ht-degree: 0%

---

# 開始使用自訂VCL

Fastly支援自訂的Varnish Configuration Language (VCL)版本，以根據您的需求量身打造Fastly服務組態。

自訂VCL片段是新增至上傳至Adobe Commerce網站之使用中VCL版本之VCL邏輯的區塊。 自訂VCL程式碼片段會修改Fastly快取服務回應請求流量的方式。 例如，您可以新增自訂VCL程式碼片段，以允許僅來自指定使用者端IP位址的請求流量。 或者，建立程式碼片段以封鎖來自已知會傳送轉介垃圾郵件至您Adobe Commerce網站的流量。

自訂VCL程式碼片段（產生、編譯並傳輸到所有Fastly快取記憶體）載入並啟動，不會造成伺服器停機。

>[!NOTE]
>
>將自訂VCL程式碼、邊緣字典和ACL新增到Fastly模組組態之前，請先確認Fastly快取服務可搭配預設組態運作。 請參閱[設定Fastly服務](fastly-configuration.md)。

Fastly支援兩種型別的自訂VCL程式碼片段：

- [一般程式碼片段](https://docs.fastly.com/en/guides/about-vcl-snippets) — 自訂一般VCL程式碼片段會針對特定VCL版本進行編碼。 您可以從Admin或Fastly API建立、修改和部署一般VCL程式碼片段。

- [動態代碼片段](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets) — 使用Fastly API建立的VCL代碼片段。 您可以修改和部署動態程式碼片段，而無需更新服務的Fastly VCL版本。

建議您搭配使用自訂VCL片段與Edge字典和存取控制清單(ACL)，以儲存自訂程式碼所使用的資料。

- [**Edge字典**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries) — 將資料以索引鍵值配對的形式儲存在字典容器中，以便從自訂VCL程式碼片段參照

- [**Edge ACL**](https://docs.fastly.com/guides/access-control-lists/about-acls) — 儲存使用者端IP位址資料，該資料會定義使用自訂VCL片段實作的區塊或允許規則的存取控制清單

字典和ACL資料會部署至可跨網路區域存取的Fastly Edge節點。 此外，資料可以跨網路動態更新，不需要您針對中繼或生產環境重新部署VCL程式碼。

>[!NOTE]
>
>如果您已針對該環境[設定Fastly服務](fastly-configuration.md)，則只能將自訂VCL程式碼片段新增至測試環境或生產環境。

## 教學課程

本教學課程和範例示範如何搭配使用一般的自訂VCL片段與Edge字典和Edge ACL，以自訂Adobe Commerce的Fastly服務設定。 如需更多詳細資訊，請參閱Fastly檔案：

- [Fastly VCL指南](https://docs.fastly.com/guides/vcl/guide-to-vcl) — 有關Fastly Varnish實作、Fastly VCL擴充功能的資訊，以及瞭解更多有關Varnish和VCL的資源。
- [Fastly VCL參考](https://docs.fastly.com/guides/vcl/) — 開發及疑難排解Fastly自訂VCL和自訂VCL片段的詳細程式設計參考。

您可以從Adobe Commerce管理員或使用Fastly API來建立和管理自訂VCL程式碼片段：

- [Adobe Commerce管理員](#manage-custom-vcl-from-admin) — 建議您使用Adobe Commerce管理員來管理自訂VCL片段，因為它會自動執行驗證、上傳並將VCL變更套用至Fastly服務設定的程式。 此外，您也可以從Admin檢視及編輯新增至Fastly服務組態的自訂VCL程式碼片段。

- [Fastly API](#manage-vcl-using-the-api) — 如果您無法存取Admin，請使用Fastly API來管理自訂VCL片段。 例如，使用API在網站關閉時對Fastly服務設定進行疑難排解，或新增自訂VCL程式碼片段。 此外，部分作業只能使用API完成。 例如，您必須使用API來重新啟動較舊的VCL版本，或檢視指定VCL版本中包含的所有VCL片段。 請參閱[VCL片段的API快速參考](#api-quick-reference-for-vcl-snippets)。

### 範例VCL程式碼片段

下列範例顯示依使用者端IP位址篩選流量的自訂VCL程式碼片段（JSON格式）：

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>在此範例中，VCL程式碼會格式化為JSON裝載，且可以儲存至檔案並以Fastly API請求提交。 若要防止在針對API請求以JSON形式傳送程式碼片段時發生JSON驗證錯誤，請使用反斜線將程式碼中的特殊字元逸出。 請參閱Fastly VCL檔案中的[使用動態VCL片段](https://docs.fastly.com/vcl/vcl-snippets/)。 如果您從「管理員」提交VCL程式碼片段，就不必逸出特殊字元。

`content`欄位中的VCL邏輯會執行下列動作：

- 檢查每個要求的傳入IP位址`client.ip`

- 封鎖任何IP位址包含在&#x200B;*ACLNAME* edge ACL中的要求，傳回`403 Forbidden`錯誤

下表提供自訂VCL程式碼片段之關鍵資料的詳細資訊。 如需更詳細的參考資料，請參閱Fastly檔案中的[VCL程式碼片段](https://docs.fastly.com/api/config#api-section-snippet)參考資料。

| 值 | 說明 |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | 用於存取您的Fastly帳戶的API金鑰。 請參閱[取得認證](fastly-configuration.md)。 |
| `active` | 程式碼片段或版本的作用中狀態。 傳回`true`或`false`。 如果為true，則表示正在使用程式碼片段或版本。 使用其版本號碼來復製作用中的程式碼片段。 |
| `content` | 要執行的VCL程式碼片段。 Fastly並不支援所有VCL語言功能。 此外，Fastly還提供具有自訂功能的擴充功能。 如需支援功能的詳細資訊，請參閱[Fastly VCL程式設計參考](https://docs.fastly.com/vcl/reference/)。 |
| `dynamic` | 程式碼片段的動態狀態。 傳回Fastly服務組態之版本化VCL中所包含的[一般程式碼片段](https://docs.fastly.com/en/guides/about-vcl-snippets)的`false`。 傳回[動態程式碼片段](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/)的`true`，不需要新的VCL版本即可修改和部署。 |
| `number` | 包含程式碼片段的VCL版本號碼。 Fastly在其範例值中使用&#x200B;*可編輯版本#*。 如果您從API新增自訂程式碼片段，請在API請求中包含版本號碼。 如果您從「管理員」新增自訂VCL，則會為您提供版本。 |
| `priority` | 從`1`到`100`的數值，指定自訂VCL程式碼片段執行的時機。 優先順序值較低的程式碼片段會先執行。 如果未指定，`priority`值會預設為`100`。<p>任何優先順序值為`5`的自訂VCL程式碼片段都會立即執行，最適合實作要求路由（封鎖和允許清單及重新導向）的VCL程式碼。 優先順序`100`最適合覆寫預設VCL程式碼片段。<p>Magento-Fastly模組中所包含的[預設VCL程式碼片段](fastly-configuration.md#upload-vcl-snippets)皆有`priority=50`。<ul><li>指派高優先順序（例如`100`）在所有其他VCL函式之後執行自訂VCL程式碼，並覆寫預設VCL程式碼。</li></ul> |
| `service_id` | 適用於特定測試或生產環境的Fastly服務ID。 將您的專案新增至雲端基礎結構[Fastly服務帳戶](fastly.md#fastly-service-account-and-credentials)上的Adobe Commerce時，就會指派此ID。 |
| `type` | 指定插入產生的程式碼片段的位置，例如`init` （在子常式上方）和`recv` （在子常式內）。 如需詳細資訊，請參閱Fastly [VCL程式碼片段](https://docs.fastly.com/api/config#api-section-snippet)參考。 |

## 從管理員管理自訂VCL

您可以從Admin的&#x200B;*Fastly組態* > *自訂VCL片段*&#x200B;區段中[新增自訂VCL片段](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md)。

![管理自訂VCL程式碼片段](../../assets/cdn/fastly-edit-snippets.png)

*自訂VCL程式碼片段*&#x200B;檢視只會顯示透過管理員新增的程式碼片段。 如果使用Fastly API新增程式碼片段，請使用該API來[管理它們](#manage-vcl-using-the-api)。

下列範例說明如何從管理員建立和管理自訂VCL程式碼片段，以及如何使用Fastly Edge模組和Edge字典：

- [將請求重新路由到CMS後端](fastly-vcl-wordpress.md)
- [封鎖轉介垃圾訊息](fastly-vcl-badreferer.md)
- [封鎖轉介垃圾訊息](fastly-vcl-badreferer.md)
- [IP允許清單的自訂VCL](fastly-vcl-allowlist.md)
- [IP封鎖清單的自訂VCL](fastly-vcl-blocking.md)
- [略過Fastly快取](fastly-vcl-bypass-to-origin.md)

## 使用API管理VCL

以下逐步說明如何使用Fastly API建立一般VCL程式碼片段檔案，並將其新增到您的Fastly服務設定中。 您可以從&#x200B;*終端機*&#x200B;應用程式建立和管理程式碼片段。 您不需要SSH連線至特定環境。

**必要條件：**

- 在雲端基礎結構環境中設定您的Adobe Commerce，以使用Fastly服務。 檢視[設定Fastly](fastly-configuration.md)。

- [取得Fastly API認證](fastly-configuration.md)以驗證Fastly API的要求。 請務必取得正確環境的認證：測試或生產。

- 將Fastly服務認證儲存為bash環境變數，以便在cURL命令中使用：

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  匯出的環境變數僅可在目前的bash工作階段中使用，並在關閉終端機時遺失。 您可以匯出新值來重新定義變數。 若要檢視與Fastly相關的匯出變數清單：

  ```bash
  export | grep FASTLY
  ```

## 新增VCL程式碼片段

本教學課程提供使用Fastly API新增自訂程式碼片段的基本步驟。

>[!NOTE]
>
>若要瞭解如何從Adobe Commerce Admin管理自訂VCL片段，請參閱[從Adobe Commerce Admin管理VCL](#manage-custom-vcl-from-admin)。


**必要條件**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### 步驟1：找出作用中的VCL版本

使用Fastly API [取得版本](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987)作業來取得作用中的VCL版本號碼：

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

在JSON回應中，請注意`number`索引鍵中傳回的有效VCL版本號碼，例如`"number": 99`。 當您複製VCL進行編輯時，需要版本號碼。

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

將作用中的版本號儲存在Bash環境變數中，以用於後續API請求：

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### 步驟2：復製作用中的VCL版本和所有片段

您必須先建立作用中VCL版本的復本進行編輯，才能新增或修改自訂VCL片段。 使用Fastly API [複製](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709)作業：

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

在JSON回應中，版本編號會增加，*active*&#x200B;索引鍵值為`false`。 您可以在本機修改新的非作用中VCL版本。

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

將新版本號儲存在bash環境變數中，以便用於後續命令：

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### 步驟3：建立自訂VCL程式碼片段

使用下列內容和格式在JSON檔案中建立並儲存自訂VCL程式碼：

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

值包括：

- `name`—VCL程式碼片段名稱。

- `dynamic` — 指出這是[一般程式碼片段](https://docs.fastly.com/en/guides/about-vcl-snippets)還是[動態程式碼片段](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets)。

- `type` — 指定插入產生的程式碼片段的位置，例如`init` （在子常式上方）和`recv` （在子常式內）。 如需這些值的詳細資訊，請參閱[Fastly VCL程式碼片段物件值](https://docs.fastly.com/api/config#snippet)。

- `priority` — 一個從`1`到`100`的值，可決定自訂VCL程式碼片段執行的時機。 具有較低值的自訂VCL程式碼片段會先執行。

  來自Fastly VCL模組的所有預設VCL程式碼都有`50`的`priority`。 如果您希望動作最後發生或覆寫預設VCL程式碼，請使用較高的數字，例如`100`。 若要立即執行自訂VCL程式碼片段，請將優先順序設定為較低的值，例如`5`。

- `content` — 要在一行中執行的VCL程式碼片段，沒有分行符號。 請參閱[自訂VCL程式碼片段範例](#example-vcl-snippet-code)。

### 步驟4：將VCL程式碼片段新增至Fastly組態

使用Fastly API [建立程式碼片段](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d)作業，將自訂VCL程式碼片段新增至VCL版本。

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

`<filename.json>`是您在上一步中準備的檔案名稱。 對每個VCL片段重複此指令。

如果您收到來自Fastly服務的`500 Internal Server Error`回應，請檢查JSON檔案語法以確保您上傳的是有效的檔案。

### 步驟5：驗證並啟動自訂VCL程式碼片段

新增自訂VCL程式碼片段後，Fastly會將該程式碼片段插入您正在編輯的VCL版本中。 若要套用變更，請完成下列步驟以驗證VCL程式碼片段並啟動VCL版本。

1. 使用Fastly API [驗證VCL版本](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6)作業來驗證更新的VCL程式碼。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   如果Fastly API傳回錯誤，請修正問題並再次驗證更新的VCL版本。

1. 使用Fastly API [啟動](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5)作業來啟動新的VCL版本。

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## VCL片段的API快速參考

這些API請求範例使用匯出的環境變數來提供認證以使用Fastly進行驗證。 如需這些命令的詳細資訊，請參閱[Fastly API參考](https://docs.fastly.com/api/config#vcl)。

>[!NOTE]
>
>使用這些命令來管理您使用Fastly API新增的程式碼片段。 如果您從Admin新增代碼片段，請參閱[使用Admin管理VCL代碼片段](#manage-vcl-using-the-api)。

- **取得使用中的VCL版本號碼**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **列出附加到服務的所有一般VCL程式碼片段**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **檢閱個別程式碼片段**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  `<snippet_name>`是程式碼片段名稱，例如`my_regular_snippet`。

- **更新程式碼片段**

  修改[準備的JSON檔案](#step-3-create-a-custom-vcl-snippet)，並傳送下列要求：

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **刪除個別VCL程式碼片段**

  取得程式碼片段清單，並使用下列`curl`命令搭配特定程式碼片段名稱來刪除：

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **覆寫[預設Fastly VCL程式碼中的值](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  以更新的值建立程式碼片段，並指派`100`的優先順序。

## 無法在Commerce管理員中檢視/修改的程式碼片段

您無法直接在Commerce管理員中檢視或修改部分程式碼片段。 例如，[動態代碼片段](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets)。 在「自訂VCL程式碼片段」區段中，您將不會看到Cloud Support團隊直接新增到[Fastly管理儀表板](fastly.md#fastly-service-account-and-credentials)的程式碼片段。


**若要觀察雲端支援團隊新增的程式碼片段：**

1. 移至&#x200B;**工具**&#x200B;區段。

1. 按一下&#x200B;_版本記錄_&#x200B;旁的&#x200B;**列出所有版本**。

1. 按一下適用的VCL版本旁的眼睛圖示，即可檢視現有的代碼片段。
