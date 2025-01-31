---
title: Web屬性
description: 請參閱如何在 [!DNL Commerce] 應用程式組態檔中設定Web屬性的範例。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Web屬性

`web`屬性定義您的應用程式如何向網頁公開（在HTTP中）、決定網頁應用程式如何提供內容，以及透過在每個位置&#x200B;_區塊_&#x200B;中設定規則來控制應用程式容器如何回應傳入的要求。 區塊代表以正斜線(`/`)開頭的絕對路徑。

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

您可以使用每個`locations`區塊的下列索引鍵值來微調`locations`組態：

| 屬性 | 說明 |
| :--- | :--- |
| `allow` | 提供不符合「規則」的檔案。 預設值= `true` |
| `expires` | 設定要在瀏覽器中快取內容的秒數。 此金鑰可啟用靜態內容的`cache-control`和`expires`標頭。 如果未設定此值，則在提供靜態內容檔案時，不會包含`expires`指示詞和產生的標頭。 負1 (`-1`)值會導致無快取，而且是預設值。 您可以使用下列單位來表示時間值： `ms` （毫秒）、`s` （秒）、`m` （分鐘）、`h` （小時）、`d` （天）、`w` （周）、`M` （月、30天）或`y` （年、365天） |
| `headers` | 針對從這個位置提供的靜態內容，設定自訂標頭，例如`X-Frame-Options`。 |
| `index` | 列出要提供給應用程式的靜態檔案，例如`index.html`檔案。 此索引鍵必須是集合。 只有在此位置的`allow`或`rules`金鑰「允許」存取一或多個檔案時，才能使用此功能。 |
| `rules` | 指定位置的覆寫。 使用規則運算式來比對請求。 如果傳入的請求符合規則，則規則的索引鍵會覆寫對請求的一般處理。 |
| `passthru` | 設定在找不到靜態檔案或PHP檔案時使用的URL。 一般而言，此URL是您應用程式（例如`/index.php`或`/app.php`）的前端控制器。 |
| `root` | 設定相對於在網頁上公開之應用程式根目錄的路徑。 雲端專案的公用目錄（位置&quot;/&quot;）預設為「pub」。 |
| `scripts` | 允許在此位置載入指令碼。 將值設定為`true`以允許指令碼。 |

預設設定允許以下專案：

- 從根(`/`)路徑，只能存取網頁和媒體
- 從`~/pub/static`和`~/pub/media`路徑中，可存取任何檔案

下列範例顯示`.magento.app.yaml`檔案中，與[`mounts`屬性](properties.md#mounts)中的專案相關的一組網頁可存取位置的預設設定：

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>此範例顯示雲端專案的預設Web設定，該專案設定為支援單一網域。 針對需要支援多個網站或商店的專案，`web`設定必須設定為支援共用網域。 請參閱[設定共用網域的位置](../store/multiple-sites.md#configure-locations-for-shared-domains)。
