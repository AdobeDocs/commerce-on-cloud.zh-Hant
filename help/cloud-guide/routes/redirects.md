---
title: 重新導向
description: 瞭解如何在雲端基礎結構專案上管理Adobe Commerce的重新導向規則。
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 重新導向

管理重新導向規則是Web應用程式的常見需求，尤其是在您不想失去隨著時間變更或移除的傳入連結的情況下。

下列範例示範如何使用`routes.yaml`設定檔管理雲端基礎結構專案上Adobe Commerce的重新導向規則。 如果本主題中討論的重新導向方法並不適合您，您可以使用快取標頭來做同樣的事情。

{{route-placeholder}}

## Pro環境的更新

{{pro-self-service-warning}}

>[!WARNING]
>
>對於雲端基礎結構專案上的Adobe Commerce，在`routes.yaml`檔案中設定許多非規則運算式重新導向和重新寫入，可能會導致效能問題。 如果`routes.yaml`檔案大於或等於32 KB，請將非規則運算式重新導向解除安裝並重新寫入Fastly。 檢視&#x200B;_Adobe Commerce說明中心_&#x200B;中的[解除安裝非Regex重新導向至Fastly而非Nginx （路由）](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html)。

## 整個路由重新導向

使用整個路由重新導向，您可以使用`routes.yaml`檔案來定義簡單路由。 例如，您可以從Apex網域重新導向至`www`子網域，如下所示：

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## 部分路由重新導向

在`.magento/routes.yaml`檔案中，您可以根據模式比對，將部分重新導向規則新增到現有路由：

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

部分重新導向適用於任何型別的路由，包括應用程式直接提供的路由。

在`redirects`下有兩個可用的金鑰：

- **過期** — 選擇性，指定在瀏覽器中快取重新導向的時間長度。 有效值的範例包括`3600s`、`1d`、`2w`、`3m`。

- **路徑** — 指定部分路由重新導向規則組態的一或多個機碼值組。

  對於每個重新導向規則，索引鍵是用來篩選重新導向之要求路徑的運算式。 值是指定重新導向的目標目的地以及處理重新導向的選項的物件。

  value物件具有下列屬性：

  | 屬性 | 說明 |
  | ---------- | ----------- |
  | `to` | 必要項，部分絕對路徑、具有通訊協定和主機的URL，或指定重新導向規則之目標目的地的模式。 |
  | `regexp` | 選擇性，預設為`false`。 指定是否應將路徑索引鍵解譯為PCRE規則運算式。 |
  | `prefix` | 指定重新導向是否同時適用於路徑及其所有子系，或只適用於路徑本身。 預設為`true`。 如果`regexp`為`true`，則不支援此值。 |
  | `append_suffix` | 決定是否使用重新導向來傳遞尾碼。 預設為`true`。 如果`regexp`索引鍵是`true`或* （如果`prefix`索引鍵是`false`），則不支援此值。 |
  | `code` | 指定HTTP狀態代碼。 有效的狀態碼為[`301` （永久移動）](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2)、[`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3)、[`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8)和[`308`](https://www.rfc-editor.org/rfc/rfc7238)。 預設為`302`。 |
  | `expires` | 選擇性，指定在瀏覽器中快取重新導向的時間長度。 預設為直接在`redirects`機碼下定義的`expires`值，但在此層級中，您可以微調個別部分重新導向的快取有效期。 |

## 部分路由重新導向的範例

下列範例說明如何使用各種`paths`組態選項在`routes.yaml`檔案中指定部分路由重新導向。

### 規則運算式模式比對

使用以下格式，根據規則運算式設定重新導向要求。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

此設定會根據規則運算式篩選要求路徑，並將相符要求重新導向至`https://example.com`。 例如，對`https://example.com/regexp/a/b/c/match`的請求會重新導向至`https://example.com/a/b/c`。

### 字首模式比對

使用以下格式，為以指定首碼模式開頭的路徑設定重新導向要求。

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

此設定的運作方式如下：

- 將符合模式`/from`的要求重新導向至路徑`http://{default}/to`。

- 將符合模式`/from/another/path`的要求重新導向至`https://{default}/to/another/path`。

- 如果您將`prefix`屬性變更為`false`，符合`/from`模式的要求會觸發重新導向，但符合`/from/another/path`模式的要求不會觸發。

### 字尾模式比對

使用下列格式設定重新導向要求，以便將要求中的路徑尾碼附加至目標目的地：

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

此設定的運作方式如下：

- 將符合模式`/from/path/suffix`的要求重新導向至路徑`https://{default}/to`。

- 如果您將`append_suffix`屬性變更為`true`，則符合`/from/path/suffix`的要求會重新導向至路徑`https://{default}/to/path/suffix`。

### 路徑特定的快取設定

使用以下格式來自訂從特定路徑快取重新導向的時間：

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

此設定的運作方式如下：

- 第一個路徑(`/from`)的重新導向會快取一天。

- 快取第二個路徑(`/here`)的重新導向，為期兩週。
