---
title: 設定Elasticsearch服務
description: 瞭解如何在雲端基礎結構上啟用Adobe Commerce的Elasticsearch服務。
feature: Cloud, Search, Services
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '788'
ht-degree: 0%

---

# 設定Elasticsearch服務

[Elasticsearch](https://www.elastic.co)是開放原始碼產品，可讓您從任何來源取得資料、任何格式，並即時搜尋和視覺化資料。

{{elasticsearch-support}}

若為Adobe Commerce 2.4.4版或更新版本，請參閱[設定OpenSearch服務](opensearch.md)。

- Elasticsearch會對產品目錄中的產品執行快速和進階搜尋
- Elasticsearch分析器支援多種語言
- 支援停用字和同義字
- 在重新索引操作完成之前，索引不會影響客戶

>[!TIP]
>
>Adobe建議您一律為雲端基礎結構專案上的Adobe Commerce設定Elasticsearch，即使您打算為Adobe Commerce應用程式設定協力廠商搜尋工具亦然。 設定Elasticsearch會在第三方搜尋工具失敗時提供後援選項。

{{service-instruction}}

**若要啟用Elasticsearch**：

1. 對於入門專案，將`elasticsearch`服務新增到Elasticsearch版本且已配置磁碟空間為MB的`.magento/services.yaml`檔案。

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   對於Pro專案，您必須提交Adobe Commerce支援票證以在預備和生產環境中變更Elasticsearch版本。

1. 設定`.magento.app.yaml`檔案中的`relationships`屬性。

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. 新增、提交和推送程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   如需這些變更如何影響您環境的詳細資訊，請參閱[服務](services-yaml.md)。

1. 部署程式完成後，請使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 重新索引目錄搜尋索引。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 清除快取。

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Elasticsearch軟體相容性

在雲端基礎結構專案上安裝或升級Adobe Commerce時，請務必檢查Adobe Commerce的Elasticsearch服務版本與[ElasticsearchPHP](https://github.com/elastic/elasticsearch-php)使用者端之間的相容性。

- **首次安裝** — 確認`services.yaml`檔案中指定的Elasticsearch版本與為Adobe Commerce設定的ElasticsearchPHP使用者端相容。

- **專案升級** — 確認新應用程式版本中的ElasticsearchPHP使用者端與安裝在雲端基礎結構上的Elasticsearch服務版本相容。

雲端基礎結構上Adobe Commerce的服務版本和相容性支援取決於雲端基礎結構上部署的版本，有時與Adobe Commerce內部部署支援的版本不同。 請參閱[服務版本](services-yaml.md#service-versions)。

**若要檢查Elasticsearch軟體相容性**：

1. 在本機工作站上，變更至專案目錄。

1. 顯示使用中環境的Elasticsearch詳細資訊。

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. 或者，您可以使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 檢查`elasticsearch/elasticsearch`的Composer封裝版本。

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   在回應中，檢查`versions`屬性中已安裝的版本。

   ```
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   您也可以在環境根目錄的`composer.lock`檔案中找到ElasticsearchPHP使用者端版本。

1. 從命令列擷取Elasticsearch服務連線詳細資料。

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   在回應中，尋找Elasticsearch服務端點的IP位址：

   ```
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. 從服務端點擷取已安裝的Elasticsearch服務`version:number`。

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```json
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. 檢查Elasticsearch服務和PHP使用者端之間的版本相容性。

   如果版本不相容，請對您的環境設定進行下列其中一項更新：

   - 將ElasticsearchPHP使用者端變更為與Elasticsearch服務版本相容的版本。

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - 將`services.yaml`檔案中的Elasticsearch服務版本變更為與ElasticsearchPHP使用者端相容的版本。

     {{pro-update-service}}

## 重新啟動Elasticsearch服務

如果您需要重新啟動[Elasticsearch](https://www.elastic.co)服務，必須聯絡Adobe Commerce支援。

## 其他搜尋設定

- 根據預設，雲端環境的搜尋設定會在您每次部署時重新產生。 您可以使用`SEARCH_CONFIGURATION`部署變數，在部署之間保留自訂搜尋設定。 請參閱[部署變數](../environment/variables-deploy.md#search_configuration)。

- 在您為專案設定Elasticsearch服務後，請使用管理員UI來測試Elasticsearch連線並自訂Adobe Commerce的Elasticsearch設定。

### 新增Elasticsearch的外掛程式

您可以選擇將`configuration:plugins`區段新增至`.magento/services.yaml`檔案中的Elasticsearch服務，以新增外掛程式以進行Elasticsearch。 例如，下列程式碼會啟用ICU分析和注音分析外掛程式。

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

如果您使用Elastic Suite協力廠商外掛程式，您必須[將`ece-tools`套件](../dev-tools/update-package.md)更新至2002.0.19或更新版本。
設定Elastic Suite時，請將組態設定新增至`ELASTICSUITE_CONFIGURATION`部署變數。 此設定可跨部署儲存設定。

### 移除Elasticsearch的外掛程式

從`.magento/services.yaml`中的`elasticsearch:`移除外掛程式專案，並不會如您預期解除安裝或停用它們。 您必須重新索引Elasticsearch資料。 此行為旨在防止依賴這些外掛程式的資料可能遺失或損毀。

**若要移除Elasticsearch外掛程式**：

1. 從您的`.magento/services.yaml`檔案中移除Elasticsearch外掛程式專案。
1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 將`.magento/services.yaml`變更提交至您的雲端存放庫。
1. 重新索引目錄搜尋索引。

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. 清除快取。

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>如需搭配Adobe Commerce使用或疑難排解Elastic Suite外掛程式的詳細資訊，請參閱[Elastic Suite檔案](https://github.com/Smile-SA/elasticsuite)。

## 疑難排解

請參閱下列Adobe Commerce支援文章，以取得疑難排解Elasticsearch問題的說明：

- 已設定[Elasticsearch5，但搜尋頁面未載入，並顯示「Fielddata已停用……」錯誤](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html)
- Adobe Commerce疑難排解員中的[Elasticsearch](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Elasticsearch索引狀態為`yellow`或`red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html)
