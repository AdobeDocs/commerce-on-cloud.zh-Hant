---
title: 新增網站地圖和搜尋引擎自動機制
description: 瞭解如何在雲端基礎結構上將網站地圖和搜尋引擎機器人新增到Adobe Commerce。
feature: Cloud, Configuration, Search, Site Navigation
exl-id: 060dc1f5-0e44-494e-9ade-00cd274e84bc
source-git-commit: 8626364ec7bcaaa0e17a3380ec0b9b73110c4574
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---

# 新增網站地圖和搜尋引擎自動機制

嘗試產生`sitemap.xml`檔案並將其寫入根目錄會導致下列錯誤：

```
Please make sure that "/" is writable by the web-server.
```

使用雲端基礎結構上的Adobe Commerce，您只能寫入特定目錄，例如`var`、`pub/media`、`pub/static`或`app/etc`。 當您使用[管理]面板產生`sitemap.xml`檔案時，必須指定`/media/`路徑。

您不必產生`robots.txt`檔案，因為它會隨選產生`robots.txt`內容並將其儲存在資料庫中。 您可以使用`<domain.your.project>/robots.txt`或`<domain.your.project>/robots`連結在瀏覽器中檢視內容。

這需要ECE-Tools 2002.0.12版和更新版本，並包含更新的`.magento.app.yaml`檔案。 在[magento-cloud存放庫](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49)中檢視這些規則的範例。

**若要產生2.2版和更新版本的`sitemap.xml`檔案**：

1. 存取「管理員」。
1. 在&#x200B;_行銷_&#x200B;功能表上，按一下&#x200B;_SEO和搜尋_&#x200B;區段中的&#x200B;**網站地圖**。
1. 在&#x200B;_網站地圖_&#x200B;檢視中，按一下&#x200B;**新增網站地圖**。
1. 在&#x200B;_新網站地圖_&#x200B;檢視中，輸入下列值：

   - **檔案名稱**：`sitemap.xml`
   - **路徑**：`/media/`

1. 按一下&#x200B;**儲存並產生**。 新的網站地圖可在&#x200B;_網站地圖_&#x200B;格線中使用。
1. 按一下Google _資料行_&#x200B;連結中的路徑。

**若要將內容新增至`robots.txt`檔案**：

1. 存取「管理員」。
1. 在&#x200B;_Content_&#x200B;功能表上，按一下&#x200B;_設計_&#x200B;區段中的&#x200B;**組態**。
1. 在&#x200B;_設計組態_&#x200B;檢視中，按一下&#x200B;_動作_&#x200B;欄位中的網站&#x200B;**編輯**。
1. 在&#x200B;_主要網站_&#x200B;檢視中，按一下&#x200B;**搜尋引擎機器人**。
1. 更新robots.txt **欄位的**&#x200B;編輯自訂指令。
1. 按一下&#x200B;**儲存組態**。
1. 驗證瀏覽器中的`<domain.your.project>/robots.txt`檔案或`<domain.your.project>/robots` URL。

>[!NOTE]
>
>如果`<domain.your.project>/robots.txt`檔案產生`404 error`，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以移除從`/robots.txt`到`/media/robots.txt`的重新導向。

## 使用Fastly VCL程式碼片段重新寫入

如果您有不同的網域，而且需要個別的網站地圖，您可以建立VCL以路由至適當的網站地圖。 如上所述，在管理面板中產生`sitemap.xml`檔案，然後建立自訂Fastly VCL程式碼片段以管理重新導向。 請參閱[自訂Fastly VCL片段](../cdn/fastly-vcl-custom-snippets.md)。

>[!NOTE]
>
> 您可以從管理員UI或使用Fastly API上傳自訂VCL程式碼片段。 請參閱[自訂VCL程式碼片段範例和教學課程](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code)。

### 使用Fastly VCL程式碼片段重新導向

建立自訂VCL程式碼片段，以使用`type`和`content`機碼值組將`sitemap.xml`的路徑重寫為`/media/sitemap.xml`。

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

下列範例示範如何將`robots.txt`和`sitemap.xml`的路徑重寫為`/media/robots.txt`和`/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**若要針對特定網域重新導向**&#x200B;使用Fastly VCL程式碼片段：

建立`pub/media/domain_robots.txt`檔案（網域為`domain.com`），並使用下一個VCL程式碼片段：

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

VCL程式碼片段路由`http://domain.com/robots.txt`並顯示`pub/media/domain_robots.txt`檔案。

若要在單一程式碼片段中設定`robots.txt`和`sitemap.xml`的重新導向，請建立`pub/media/domain_robots.txt`和`pub/media/domain_sitemap.xml`檔案，其中網域為`domain.com`，並使用下一個VCL程式碼片段：

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

在`sitemap`管理設定中，您必須使用`pub/media/` （而非`/`）指定檔案的位置。

### 依搜尋引擎設定索引

若要在生產環境中啟用`robots.txt`自訂，您必須啟用Cloud Console上專案設定中的&#x200B;`<environment-name>`**的**&#x200B;按搜尋引擎索引為開啟選項：

![使用[!DNL Cloud Console]管理環境](../../assets/robots-indexing-by-search-engine.png)

您也可以使用magento-cloud CLI來更新此設定：

```bash
magento-cloud environment:info -p <project_id> -e production restrict_robots false
```

>[!NOTE]
>
>- 搜尋引擎的索引只能在生產中啟用，但無法在任何較低層環境中啟用。
>
>- 如果您使用PWA Studio但無法存取已設定的`robots.txt`檔案，請將`robots.txt`新增至[前方名稱允許清單](https://github.com/magento/magento2-upward-connector#front-name-allowlist)，位於&#x200B;**商店** >設定> **一般** > **網頁** >上層PWA設定。

