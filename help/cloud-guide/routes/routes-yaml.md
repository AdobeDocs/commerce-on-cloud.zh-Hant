---
title: 設定路由
description: 瞭解如何在雲端基礎結構環境中為Adobe Commerce定義傳入HTTPS要求的路由。
feature: Cloud, Configuration, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# 設定路由

`.magento/routes.yaml`目錄中的`routes.yaml`檔案定義了雲端基礎結構整合、中繼和生產環境上Adobe Commerce的路由。 路由決定應用程式如何處理傳入的HTTP和HTTPS要求。

預設`routes.yaml`檔案指定在擁有單一預設網域的專案以及針對多個網域設定的專案上以HTTPS格式處理HTTP要求的路由範本：

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

使用`magento-cloud` CLI檢視已設定路由的清單：

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Pro環境的設定更新

{{pro-self-service-warning}}

## 路由範本

`routes.yaml`檔案是樣板化路由及其設定的清單。 您可以在路由範本中使用下列預留位置：

- `{default}`預留位置代表設定為專案預設值的限定網域名稱。

  例如，具有預設網域`example.com`和下列路由範本的專案：

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  這些範本會解析為生產環境中的下列URL：

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- `{all}`預留位置代表專案設定的所有網域名稱。

  例如，具有`example.com`和`example1.com`個網域的專案，其路由範本如下：

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  這些範本會針對專案中的所有網域解析為下列路由：

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  `{all}`預留位置適用於針對多個網域設定的專案。 在非生產分支中，`{all}`會取代為每個網域的專案ID和環境ID。

  如果專案未設定任何網域（這在開發期間很常見），`{all}`預留位置的行為與`{default}`預留位置的行為相同。

Adobe Commerce也會為每個使用中的整合環境產生路由。 若為整合環境，`{default}`預留位置會取代為下列網域名稱：

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

例如，在`us`叢集中代管之`mswy7hzcuhcjw`專案的`refactorcss`分支有下列網域：

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>如果您的雲端專案支援多個商店，請依照[多個網站或商店](../store/multiple-sites.md)的路由設定指示操作。

### 尾隨斜線

路由定義包含尾隨斜線，用以表示資料夾或目錄；不過，相同的內容可以有尾隨斜線或不有尾隨斜線。 下列URL解析相同，但可以解譯為&#x200B;_兩個不同的_ URL：

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>最好在目錄中使用結尾斜線，但無論您選擇哪種方法，都務必&#x200B;**保持一致**&#x200B;以避免產生兩個位置。

## 路由通訊協定

所有環境都會自動支援HTTP和HTTPS。

- 如果設定僅指定HTTP路由，則會自動建立HTTPS路由，允許從HTTP和HTTPS提供網站服務，而不需要重新導向。

  例如，具有預設網域`example.com`和下列路由範本的專案：

  ```text
  http://{default}/
  ```

  此範本解析至下列URL：

  ```text
  http://example.com/
  
  https://example.com/
  ```

- 如果設定僅指定HTTPS路由，則所有HTTP請求都會重新導向至HTTPS。

  例如，具有預設網域`example.com`的專案具有下列路由範本：

  ```text
  https://{default}/
  ```

  此範本解析至下列URL：

  ```text
  https://example.com/
  ```

  它也會處理下列重新導向：

  `http://example.com/` ==> `https://example.com/`

透過TLS提供所有頁面。 對於此設定，您必須使用下列其中一種方法，將所有未加密的請求重新導向設定為等效的TLS：

- 在`routes.yaml`檔案中將通訊協定變更為HTTPS。

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- 對於測試和生產環境，請從管理UI啟用[強制Fastly上的TLS](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html?lang=zh-Hant)選項。 使用此選項時，Fastly會處理到HTTPS的重新導向，因此您不必更新`routes.yaml`設定。

## 路由選項

使用下列屬性分別設定每個路由：

| 屬性 | 說明 |
| ---------------- | ----------- |
| `type: upstream` | 為應用程式提供服務。 此外，它有`upstream`屬性，指定了應用程式的名稱（如`.magento.app.yaml`中所定義），後面接著`:http`端點。 |
| `type: redirect` | 重新導向至其他路由。 它後面接著的是`to`屬性，這是HTTP重新導向至由其範本識別的另一個路由。 |
| `cache:` | 控制路由[&#128279;](caching.md)的快取。 |
| `redirects:` | 控制[重新導向規則](redirects.md)。 |
| `ssi:` | 控制項啟用[伺服器端包含](server-side-includes.md)。 |

## 簡單路由

在下列範例中，路由設定會將Apex網域和`www`子網域路由到`mymagento`應用程式。 此路由不會重新導向HTTPS請求。

**範例1：**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

在此範例中，請求路由會遵循下列規則：

- 伺服器會使用下列URL模式直接回應要求：

  ```text
  http://example.com/path
  ```

- 伺服器針對具有下列URL模式的要求發出&#x200B;_301重新導向_：

  ```text
  http://www.example.com/mypath
  ```

  這些要求會重新導向至Apex網域，例如：

  ```text
  http://example.com/mypath
  ```

在下列範例中，路由設定不會將URL從www網域重新導向至Apex網域。 相反地，會同時從www和apex網域提供請求。

**範例2：**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## 萬用字元路由

雲端基礎結構上的Adobe Commerce支援萬用字元路由，因此您可以將多個子網域對應至相同的應用程式。 這適用於重新導向與上游路由。 您以星號(\*)作為路由的前置詞。 例如，下列路由至相同的應用程式：

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

這可在即時環境中當成是所有網域。

### 路由未對應的網域

您可以使用點(`.`)來分隔子網域，路由到未對應到網域的系統。

**範例：**

具有`add-theme`分支的專案路由到下列URL：

```text
http://add-theme-projectID.us.magento.com/
```

如果您定義下列繞線範本：

```text
http://www.{default}/
```

路由解析至下列URL：

```text
http://www.add-theme-projectID.us.magento.com/
```

您可以在點之前插入任何子網域，而且路由會解析。

**範例：**

定義下列路由範本：

```text
http://*.{default}/
```

此範本會解析下列兩個URL：

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

您可以建立與環境的SSH連線，並使用`magento-cloud` CLI列出路由，來檢視未對應網域的路由模式：

```bash
vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## 重新導向與快取

如在[重新導向](redirects.md)中詳細討論的，您可以管理複雜的重新導向規則，例如&#x200B;_部分重新導向_，並指定路由式[快取](caching.md)的規則：

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
