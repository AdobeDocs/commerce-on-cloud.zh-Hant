---
title: 部署後變數
description: 請參閱環境變數清單，這些變數可控制Adobe Commerce在雲端基礎結構部署後階段的動作。
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# 部署後變數

下列&#x200B;_部署後_&#x200B;變數控制部署後階段中的動作，可以繼承及覆寫來自[全域變數](variables-global.md)的值。 在`.magento.env.yaml`檔案的`post-deploy`階段中插入這些變數：

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

如需自訂建置和部署程式的詳細資訊：

- [部署設定](configure-env-yaml.md)
- [部署流程](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **預設**— `[]` （空陣列）
- **版本**—Adobe Commerce 2.1.4和更新版本

設定指定頁面的&#x200B;_第一位元組時間_ (TTFB)測試，以測試您的網站效能。 針對需要測試的每個頁面，指定絕對路徑參照，或具有通訊協定和主機的URL。

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

在您指定要測試及認可變更的頁面後，_第一個位元組時間_&#x200B;測試會在部署後階段執行，並針對每個路徑將結果張貼至雲端記錄檔：

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

對於重新導向的路徑，記錄會報告重新導向目標的路徑，而非在環境變數中設定的路徑。 如果您指定的路徑無效，記錄會顯示警告訊息。

## `WARM_UP_CONCURRENCY`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

指定在快取暖機作業期間要傳送的同步要求限制，以減少伺服器負載。 此值會限制平行連線的數量，對於環境組態非常有用，其中`WARM_UP_PAGES`後部署變數會指定快取預先載入的多個頁面。

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **預設**— `index.php`
- **版本**—Adobe Commerce 2.1.4和更新版本

自訂用來在`post_deploy`階段預先載入快取的頁面清單。 您必須設定部署後鉤點。 檢視`.magento.app.yaml`檔案的[鉤點](../application/hooks-property.md)區段。

- **單一頁面** — 指定單一頁面以新增至快取。 您不需要指定預設基底URL。 下列範例快取`BASE_URL/index.php`頁面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **多個網域** — 列出多個URL。 以下範例會從兩個網域快取頁面：

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **多個頁面** — 使用以下格式，根據特定規則運算式模式快取多個頁面：

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`：可能的變體`category`、`cms-page`、`product`、`store-page`
   - `pattern|url|product_sku`：使用`regexp`模式或完全符合`url`來篩選URL，或對所有頁面使用星號(\*)。 對`product`實體型別使用產品SKU
   - `store_id|store_code`：使用商店的識別碼或代碼，或所有商店的星號(\*)，您可以傳遞數個以`|`分隔的商店識別碼或代碼

  以下範例會根據這些條件來快取`category`和`cms-page`實體型別：
   - ID為`1`之商店的所有類別頁面
   - 程式碼為`store1`和`store2`之商店的所有類別頁面
   - 程式碼為`store_en`之商店的類別頁面`cars`
   - 所有商店的cms頁面`contact`
   - 識別碼為`1`和`2`之存放區的CMS頁面`contact`
   - 任何包含`car_`且結尾為`html`且儲存識別碼為2的類別頁面
   - 任何包含`tires_`且代碼為`store_gb`之存放區的類別頁面

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  以下範例會根據這些條件來快取`product`實體型別：
   - 所有商店的所有產品（以程式設計方式限製為每家商店100項，以避免效能問題）
   - 商店`store1`的所有產品
   - 所有商店中具有`sku1`的產品
   - 程式碼為`store1`和`store2`之商店的`sku1`產品
   - 程式碼為`store1`和`store2`之商店的`sku1`、`sku2`和`sku3`產品

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  以下範例會根據這些條件來快取`store-page`實體型別：
   - 所有商店的頁面`/contact-us`
   - 識別碼為`1`之存放區的頁面`/contact-us`
   - 程式碼為`code1`和`code2`之商店的頁面`/contact-us`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
