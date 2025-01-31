---
title: 快取
description: 瞭解如何在雲端基礎結構環境中啟用Adobe Commerce的快取。
feature: Cloud, Cache, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# 快取

您可以在雲端基礎結構專案環境中啟用快取。 如果停用快取，Adobe Commerce會直接提供檔案。

{{route-placeholder}}

## 設定快取

透過在`.magento/routes.yaml`檔案中設定快取規則來啟用應用程式的快取，如下所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## 路由型快取

為數個路由分別設定快取規則，以啟用微調快取，如下列範例所示：

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

上述範例快取下列路由：

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

而且下列路由是&#x200B;**不是**&#x200B;快取：

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>路由中的規則運算式&#x200B;**不支援**。

## 快取持續時間

快取期間由`Cache-Control`回應標頭值決定。 如果回應中沒有`Cache-Control`標頭，則會使用`default_ttl`金鑰。

## 快取金鑰

為決定如何快取回應，Adobe Commerce會根據數個因素建立快取金鑰，並儲存與此金鑰相關聯的回應。 當請求帶有相同的快取金鑰時，將重複使用回應。 其用途與HTTP [`Vary`標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44)類似。

引數`headers`和`cookies`金鑰可讓您變更此快取金鑰。

這些鍵的預設值如下：

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## 快取屬性

### `enabled`

設定為`true`時，啟用此路由的快取。 設定為`false`時，停用此路由的快取。

### `headers`

定義快取索引鍵必須相依的值。

例如，如果`headers`索引鍵如下：

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

然後Adobe Commerce會為`Accept` HTTP標頭的每個值快取不同的回應。

### `cookies`

`cookies`索引鍵定義快取索引鍵必須相依的值。

例如：

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

快取金鑰取決於要求中`value` Cookie的值。

如果`cookies`機碼具有`["*"]`值，則為特殊情況。 此值表示任何具有Cookie的請求都會繞過快取。 這是預設值。

>[!NOTE]
>
>Cookie名稱中不能使用萬用字元。 請使用精確的Cookie名稱，或比對具有星號(`*`)的所有Cookie。 例如，`SESS*`或`~SESS`目前是&#x200B;**不是**&#x200B;個有效值。

Cookie有下列限制：

- 您最多可以在系統中設定&#x200B;**50 Cookie**。 否則，應用程式會擲回`Unable to send the cookie. Maximum number of cookies would be exceeded`例外狀況。
- Cookie大小上限為&#x200B;**4096位元組**。 否則，應用程式會擲回`Unable to send the cookie. Size of '%name' is %size bytes`例外狀況。

### `default_ttl`

如果回應沒有`Cache-Control`標頭，則會使用`default_ttl`索引鍵來定義快取持續時間（以秒為單位）。 預設值為`0`，這表示不會快取任何內容。
