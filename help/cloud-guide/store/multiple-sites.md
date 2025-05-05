---
title: 設定多個網站或商店
description: 瞭解如何在雲端基礎結構上為Adobe Commerce設定多個網站或商店。
feature: Cloud, Configuration, Routes, Site Navigation
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# 設定多個網站或商店

您可以將Adobe Commerce設定為擁有多個網站或商店，例如英文商店、法文商店和德文商店。 請參閱[瞭解網站、商店和商店檢視](best-practices.md#store-views)。

>[!WARNING]
>
>目錄資料會隨著您增加網站和商店的數量而擴展。 根據您的專案架構，其他存放區可能會導致非快取型目錄頁面的索引程式較長且回應時間較慢。 Adobe建議您密切監視網站效能。

設定多個存放區的程式取決於您選擇使用唯一或共用網域。

具有唯一網域的多家商店：

```
https://first.store.com/
https://second.store.com/
```

具有相同網域的多個商店：

```
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>若要將商店檢視新增至網站基底URL，您不必建立多個目錄。 請參閱&#x200B;_組態指南_&#x200B;中的[將存放區程式碼新增至基底URL](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=zh-Hant)。

## 新增網域

自訂網域可以新增到Pro Staging和任何生產環境；它們無法新增到整合環境。

新增網域的程式取決於雲端帳戶的型別：

- 適用於Pro測試與生產

  將新網域新增到Fastly，請參閱[管理網域](../cdn/fastly-custom-cache-configuration.md#manage-domains)，或開啟支援票證以請求協助。 此外，您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hant#submit-ticket)，才能要求將新網域新增至叢集。

- 僅供入門級生產使用

  新增網域至Fastly，請參閱[管理網域](../cdn/fastly-custom-cache-configuration.md#manage-domains)，或[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hant#submit-ticket)以要求協助。 此外，您必須新增網域至[!DNL Cloud Console]中的&#x200B;**網域**&#x200B;索引標籤： `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## 設定本機安裝

若要設定本機安裝以使用多個商店，請參閱&#x200B;_設定指南_&#x200B;中的[多個網站或商店][config-multiweb]。

成功建立並測試本機安裝以使用多個存放區後，您必須準備整合環境：

1. **設定路由或位置** — 指定Adobe Commerce處理傳入URL的方式

   - [不同網域的路由](#configure-routes-for-separate-domains)
   - [共用網域的位置](#configure-locations-for-shared-domains)

1. **設定網站、商店和商店檢視** — 使用Adobe Commerce管理UI設定
1. **修改變數** — 在`magento-vars.php`檔案中指定`MAGE_RUN_TYPE`和`MAGE_RUN_CODE`變數的值
1. **部署和測試環境** — 部署和測試`integration`分支

>[!TIP]
>
>您可以使用本機環境來設定多個網站或商店。 請參閱Cloud Docker指示，以[設定多個網站或商店](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/)。

### Pro環境的設定更新

{{pro-self-service-warning}}

### 設定不同網域的路由

路由會定義如何處理傳入的URL。 多個具有唯一網域的存放區需要您定義`routes.yaml`檔案中的每個網域。 您設定路由的方式取決於您想要的網站運作方式。

**若要在整合環境中設定路由**：

1. 在本機工作站上，以文字編輯器開啟`.magento/routes.yaml`檔案。

1. 定義網域和子網域。 `mymagento`上游值與`.magento.app.yaml`檔案中的name屬性值相同。

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. 將變更儲存至`routes.yaml`檔案。

1. 繼續[設定網站、商店和商店檢視](#set-up-websites-stores-and-store-views)。

### 設定共用網域的位置

路由設定會定義如何處理URL，`.magento.app.yaml`檔案中的`web`屬性會定義您的應用程式如何公開至網路。 網頁&#x200B;_位置_&#x200B;允許傳入要求的詳細程度。 例如，如果您的網域是`store.com`，您可以針對共用網域的兩個不同存放區之要求，使用`/first` （預設網站）和`/second`。

**若要設定新的網站位置**：

1. 為根(`/`)建立別名。 在此範例中，第3行的別名為`&app`。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. 為網站(`/website`)建立傳遞，並使用上一步驟中的別名來參照根目錄。

   別名可讓`website`從根位置存取值。 在此範例中，網站`passthru`在第21行上。

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**若要使用不同的目錄設定位置**：

1. 為根(`/`)和靜態(`/static`)位置建立別名。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. 在`pub`目錄下建立網站的子目錄： `pub/<website>`

1. 將`pub/index.php`檔案複製到`pub/<website>`目錄中並更新`bootstrap`路徑(`/../../app/bootstrap.php`)。

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. 為`index.php`檔案建立傳遞。

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. 提交並推送已變更的檔案。

   - `pub/<website>/index.php` （如果此檔案位於`.gitignore`，則推送可能需要強制選項。）
   - `.magento.app.yaml`

### 設定網站、商店和商店檢視

在&#x200B;_管理UI_&#x200B;中，設定您的Adobe Commerce **網站**、**商店**&#x200B;和&#x200B;**商店檢視**。 請參閱&#x200B;_設定指南_&#x200B;的Admin[&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html?lang=zh-Hant)中的設定多個網站、商店和商店檢視。

當您設定本機安裝時，請務必使用管理員提供的相同名稱和程式碼，代表您的網站、商店和商店檢視。 更新`magento-vars.php`檔案時需要這些值。

### 修改變數

請使用專案根目錄中的`magento-vars.php`檔案傳遞`MAGE_RUN_CODE`和`MAGE_RUN_TYPE`變數，而不設定NGINX虛擬主機。

**若要使用`magento-vars.php`檔案傳遞變數**：

1. 在文字編輯器中開啟`magento-vars.php`檔案。

   [預設`magento-vars.php`檔案](https://github.com/magento/magento-cloud/blob/master/magento-vars.php)應該如下所示：

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. 移動註解的`if`區塊，使其位於`function`區塊的&#x200B;_之後_&#x200B;且不再註解。

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. 取代`if (isHttpHost("example.com"))`區塊中的下列值：
   - `example.com` — 使用您&#x200B;_網站_&#x200B;的基本URL
   - `default` — 具有您&#x200B;_網站_&#x200B;或&#x200B;_商店檢視_&#x200B;的唯一代碼
   - `store` — 使用下列其中一個值：
      - `website` — 載入店面中的&#x200B;_網站_
      - `store` — 在店面中載入&#x200B;_商店檢視_

   針對使用唯一網域的多個網站：

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   對於具有相同網域的多個網站，您必須檢查&#x200B;_主機_&#x200B;和&#x200B;_URI_：

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. 將變更儲存至`magento-vars.php`檔案。

### 在整合伺服器上部署和測試

在雲端基礎結構整合環境中將變更推送至您的Adobe Commerce，並測試您的網站。

1. 新增、提交程式碼變更並將其推播至遠端分支。

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. 等待部署完成。

1. 部署後，在網頁瀏覽器中開啟您的商店URL。

   具有唯一網域，請使用格式： `http://<magento-run-code>.<site-URL>`

   例如， `http://french.master-name-projectID.us.magentosite.cloud/`

   在共用網域中，使用格式： `http://<site-URL>/<magento-run-code>`

   例如， `http://master-name-projectID.us.magentosite.cloud/french/`

1. 徹底測試您的網站，並將程式碼合併至`integration`分支以進行進一步部署。

## 部署至測試與生產

執行[部署至中繼及生產環境](../deploy/staging-production.md)的部署程式。 對於入門和Pro環境，您可使用[!DNL Cloud Console]跨環境推送程式碼。

Adobe建議先在中繼環境中進行全面測試，然後再推送至生產環境。 在整合環境中變更程式碼，然後再次開始跨環境部署的程式。

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html?lang=zh-Hant
