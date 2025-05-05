---
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '2554'
ht-degree: 4%

---
# ECE-工具錯誤代碼

<!--Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository.-->

## 嚴重錯誤

嚴重錯誤表示 Commerce on 雲端基礎結構 項目配置問題導致部署失敗，例如，所需設置的配置不正確、不受支持或缺失。 在部署之前，必須更新配置以解決這些錯誤。

### 建置階段

| 錯誤代碼 | 建立步驟 | 錯誤說明（標題） | 建議的動作 |
| - | - | - | - |
| 2 |  | 無法寫入`./app/etc/env.php`檔案 | 部署指令碼無法對`/app/etc/env.php`檔案進行必要的變更。 檢查您的檔案系統許可權。 |
| 3 |  | 檔案中未定義 `schema.yaml` 設定 | 配置未在 `./vendor/magento/ece-tools/config/schema.yaml` 檔中定義。 檢查設定變數名稱是否正確且已定義。 |
| 4 |  | 無法剖析`.magento.env.yaml`檔案 | `./.magento.env.yaml`檔案格式無效。 使用YAML剖析器來檢查語法並修正任何錯誤。 |
| 5 |  | 無法讀取`.magento.env.yaml`檔案 | 無法讀取`./.magento.env.yaml`檔案。 檢查檔案許可權。 |
| 6 |  | 無法讀取`.schema.yaml`檔案 | 無法讀取`./vendor/magento/ece-tools/config/magento.env.yaml`檔案。 檢查檔案許可權並重新部署(`magento-cloud environment:redeploy`)。 |
| 7 | refresh-modules | 無法寫入`./app/etc/config.php`檔案 | 部署指令碼無法對`/app/etc/config.php`檔案進行必要的變更。 檢查您的檔案系統許可權。 |
| 8 | validate-config | 無法讀取`composer.json`檔案 | 無法讀取`./composer.json`檔案。 檢查檔案許可權。 |
| 9 | validate-config | `composer.json`檔案遺失必要的自動載入區段 | `composer.json`檔案中缺少必要的`autoload`區段。 比較自動載入區段與雲端範本中的`composer.json`檔案，並新增缺少的設定。 |
| 10 | 驗證設定 | 該檔 `.magento.env.yaml` 包含未在綱要中聲明的選項，或者使用無效值或階段配置的選項 | 該檔 `./.magento.env.yaml` 包含無效配置。 有關詳細資訊，請查看錯誤日誌。 |
| 11 | refresh-modules | 命令失敗： `/bin/magento module:enable --all` | 嘗試在本機執行`composer update`。 然後，認可並推播更新的`composer.lock`檔案。 另外，請檢視`cloud.log`以取得詳細資訊。 如需更詳細的命令輸出，請將`VERBOSE_COMMANDS: '-vvv'`選項新增至`.magento.env.yaml`檔案。 |
| 12 | 套用修補程式 | 無法套用修補程式 |  |
| 13 | set-report-dir-nesting-level | 無法寫入檔案`/pub/errors/local.xml` |  |
| 14 | 複製採樣數據 | 無法複製範例數據檔 |  |
| 15 | 編譯-di | 命令失敗： `/bin/magento setup:di:compile` | 如需詳細資訊，請檢視`cloud.log`。 添加到 `VERBOSE_COMMANDS: '-vvv'` 以獲得 `.magento.env.yaml` 更詳細的命令輸出。 |
| 16 | 傾印 — 自動載入 | 命令失敗： `composer dump-autoload` | 命令失敗 `composer dump-autoload` 。 如需詳細資訊，請檢視`cloud.log`。 |
| 17 | run-baler | 執行JavaScript套裝的`Baler`的命令失敗 | 檢查`SCD_USE_BALER`環境變數，確認已設定並啟用JS套件組合的Baler模組。 如果您不需要Baler模組，請設定`SCD_USE_BALER: false`。 |
| 18 | compress-static-內容 | 找不到所需的公用程式 （逾時、bash） |  |
| 19 | deploy-static-content | 命令`/bin/magento setup:static-content:deploy`失敗 | 如需詳細資訊，請檢視`cloud.log`。 如需更詳細的命令輸出，請將`VERBOSE_COMMANDS: '-vvv'`選項新增至`.magento.env.yaml`檔案。 |
| 20 | compress-static-content | 靜態內容壓縮失敗 | 如需詳細資訊，請檢視`cloud.log`。 |
| 21 | backup-data： static-content | 無法將靜態內容複製到`init`目錄中 | 如需詳細資訊，請檢視`cloud.log`。 |
| 22 | backup-data： writable-dirs | 無法將部分可寫入的目錄複製到`init`目錄 | 無法將可寫入的目錄複製到`./init`資料夾。 檢查您的檔案系統許可權。 |
| 23 |  | 無法建立記錄器物件 |  |
| 24 | backup-data： static-content | 無法清除`./init/pub/static/`目錄 | 無法清除`./init/pub/static`資料夾。 檢查您的檔案系統許可權。 |
| 25 |  | 找不到撰寫器套件 | 如果直接從 GitHub 存放庫安裝了 Adobe Systems Commerce 應用程式 版本，請驗證是否已 `DEPLOYED_MAGENTO_VERSION_FROM_GIT` 配置 環境 變數。 |
| 26 | 驗證設定 | 拿掉Magento Braintree模組配置，Adobe Systems Commerce和 Magento Open Source 2.4 及更高版本中不再支援該配置。 | Magento 2.4.0 及更新版本不再支援 Braintree 模組。 從`.magento.app.yaml`檔案的變數區段中移除CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL變數。 如需Braintree付款支援，請改用Commerce Marketplace的正式擴充功能。 |

### 部署階段

| 錯誤碼 | 部署步驟 | 錯誤說明（標題） | 建議的動作 |
| - | - | - | - |
| 101 | 預先部署：快取 | 不正確的快取設定（遺失連線埠或主機） | 快取設定遺漏必要的引數`server`或`port`。 如需詳細資訊，請檢視`cloud.log`。 |
| 102 |  | 無法寫入 `./app/etc/env.php` 檔案 | 部署文稿無法對 `/app/etc/env.php` 檔進行所需的更改。 檢查檔案系統許可權。 |
| 103 |  | 檔案中未定義 `schema.yaml` 設定 | 未在`./vendor/magento/ece-tools/config/schema.yaml`檔案中定義設定。 檢查設定變數名稱是否正確，以及是否已定義。 |
| 104 |  | 剖析 `.magento.env.yaml` 檔案失敗 | 配置未在 `./vendor/magento/ece-tools/config/schema.yaml` 檔中定義。 檢查設定變數名稱是否正確且已定義。 |
| 105 |  | 無法讀取`.magento.env.yaml`檔案 | 無法讀取`./.magento.env.yaml`檔案。 檢查檔案許可權。 |
| 106 |  | 無法讀取`.schema.yaml`檔案 |  |
| 107 | pre-部署： clean-redis-cache | 無法清理 Redis 快取 | 無法清理 Redis 快取。 檢查 Redis 快取配置是否正確，以及 Redis 服務是否可用。 請參閱[安裝Redis服務](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/configure/service/redis)。 |
| 140 | 預先部署： clean-valkey-cache | 無法清除Valkey快取 | 無法清除Valkey快取。 檢查Valkey快取設定是否正確，以及Valkey服務是否可用。 請參閱[Setup Valkey服務](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/configure/service/valkey)。 |
| 108 | 預先部署：set-production-mode | 命令`/bin/magento maintenance:enable`失敗 | 查看瞭解更多資訊 `cloud.log` 。 有關更詳細的命令輸出，請將選項添加到`VERBOSE_COMMANDS: '-vvv'` `.magento.env.yaml`檔中。 |
| 109 | 驗證設定 | 資料庫配置不正確 | 檢查`DATABASE_CONFIGURATION`環境變數是否已正確設定。 |
| 110 | validate-config | 不正確的工作階段設定 | 檢查`SESSION_CONFIGURATION`環境變數是否已正確設定。 配置必須至少 `save` 包含參數。 |
| 111 | validate-config | 不正確的搜尋設定 | 檢查`SEARCH_CONFIGURATION`環境變數是否已正確設定。 組態至少必須包含`engine`引數。 |
| 112 | 驗證設定 | 資源配置不正確 | `RESOURCE_CONFIGURATION`檢查是否正確配置環境變數。配置必須至少 `connection` 包含參數。 |
| 113 | 驗證配置：ElasticSuite-integrity | ElasticSuite已安裝，但Elasticsearch服務無法使用 | 檢查`SEARCH_CONFIGURATION`環境變數是否已正確設定，並驗證Elasticsearch服務是否可用。 |
| 114 | validate-config：elasticsuite-integrity | ElasticSuite已安裝，但使用的是其他搜尋引擎 | ElasticSuite已安裝，但已設定其他搜尋引擎。 更新`SEARCH_CONFIGURATION`環境變數以啟用Elasticsearch，並在`services.yaml`檔案中驗證Elasticsearch服務設定。 |
| 115 |  | 資料庫查詢執行失敗 |  |
| 116 | install-update： setup | `/bin/magento setup:install`命令失敗 | `cloud.log`查看和 `install_upgrade.log` 瞭解更多資訊。有關更詳細的命令輸出，請將選項添加到`VERBOSE_COMMANDS: '-vvv'` `.magento.env.yaml`檔中。 |
| 117 | 安裝-更新： 設定-匯入 | `app:config:import`命令失敗 | 如需詳細資訊，請檢視`cloud.log`。 如需更詳細的命令輸出，請將`VERBOSE_COMMANDS: '-vvv'`選項新增至`.magento.env.yaml`檔案。 |
| 118 |  | 找不到所需的公用程式 （逾時、bash） |  |
| 119 | 安裝-更新： 部署-静態-內容 | 命令`/bin/magento setup:static-content:deploy`失敗 | 如需詳細資訊，請檢視`cloud.log`。 如需更詳細的命令輸出，請將`VERBOSE_COMMANDS: '-vvv'`選項新增至`.magento.env.yaml`檔案。 |
| 120 | compress-static-content | 靜態內容壓縮失敗 | 查看瞭解更多資訊 `cloud.log` 。 |
| 121 | deploy-static-content：generate | 無法更新已部署的版本 | 無法更新`./pub/static/deployed_version.txt`檔案。 檢查您的檔案系統許可權。 |
| 122 | clean-static-content | 無法清除靜態內容檔案 |  |
| 123 | install-update： split-db | 命令`/bin/magento setup:db-schema:split`失敗 | 如需詳細資訊，請檢視`cloud.log`。 有關更詳細的命令輸出，請將選項添加到`VERBOSE_COMMANDS: '-vvv'` `.magento.env.yaml`檔中。 |
| 124 | 清潔視圖預處理 | 無法清理 `var/view_preprocessed` 資料夾 | 無法清理 `./var/view_preprocessed` 資料夾。 檢查檔案系統許可權。 |
| 125 | install-update： reset-password | 無法更新`/var/credentials_email.txt`檔案 | 無法更新 `/var/credentials_email.txt` 檔案。 檢查檔案系統許可權。 |
| 126 | install-update： update | 命令`/bin/magento setup:upgrade`失敗 | 如需詳細資訊，請檢視`cloud.log`和`install_upgrade.log`。 如需更詳細的命令輸出，請將`VERBOSE_COMMANDS: '-vvv'`選項新增至`.magento.env.yaml`檔案。 |
| 127 | clean-cache | `/bin/magento cache:flush`命令失敗 | 查看瞭解更多資訊 `cloud.log` 。 有關更詳細的命令輸出，請將選項添加到`VERBOSE_COMMANDS: '-vvv'` `.magento.env.yaml`檔中。 |
| 128 | 禁用維護模式 | 命令`/bin/magento maintenance:disable`失敗 | 如需詳細資訊，請檢視`cloud.log`。 將`VERBOSE_COMMANDS: '-vvv'`新增至`.magento.env.yaml`，以取得更詳細的命令輸出。 |
| 129 | install-update： reset-password | 無法讀取重設密碼範本 |  |
| 130 | install-update： cache_type | 命令失敗： `php ./bin/magento cache:enable` | 命令`php ./bin/magento cache:enable`只有在安裝Adobe Commerce時執行，但在部署開始時有`./app/etc/env.php`檔案不存在或空白。 如需詳細資訊，請檢視`cloud.log`。 將`VERBOSE_COMMANDS: '-vvv'`新增至`.magento.env.yaml`，以取得更詳細的命令輸出。 |
| 131 | install-update | `crypt/key`索引鍵值不存在`./app/etc/env.php`於檔案或`CRYPT_KEY`雲端環境變數 | 如果 Commerce 部署 開始時檔不存在Adobe Systems或者`crypt/key`值未定義，則`./app/etc/env.php`會發生此錯誤。如果從其他環境遷移資料庫，請從該環境中檢索加密密鑰值。 接著，將該值新增至目前 [環境中環境變數的CRYPT_KEY雲端](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/configure/env/stage/variables-deploy#crypt_key) 。 請參閱 [Adobe Systems商務加密金鑰](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/develop/overview#gather-credentials)。 如果不小心刪除了 `./app/etc/env.php` 該文件，請使用以下命令從從上一個部署創建的備份文件還原該文件： `./vendor/bin/ece-tools backup:restore` CLI 命令。 |
| 132 |  | 無法連接到 Elasticsearch 服務 | 檢查有效的Elasticsearch憑據並驗證服務是否正在運行 |
| 137 |  | 無法連線到OpenSearch服務 | 檢查有效的OpenSearch認證，並確認服務執行中 |
| 133 | validate-config | 拿掉Magento Braintree 模組 配置，Adobe Systems Commerce 或 Magento Open Source 2.4 及更高版本中不再支援該配置。 | Adobe Systems Commerce 或 Magento Open Source 2.4.0 及更高版本不再包含對 Braintree 模組 的支援。 拿掉檔案變數區段 `.magento.app.yaml` 的CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL變數。 如需Braintree支援，請改用 Commerce Marketplace 的官方 Braintree 付款擴展。 |
| 134 | 驗證設定 | Adobe Commerce和Magento Open Source 2.4.0需要安裝Elasticsearch服務 | 安裝Elasticsearch服務 |
| 138 | validate-config | Adobe Commerce和Magento Open Source 2.4.4需要安裝OpenSearch或Elasticsearch服務 | 安裝OpenSearch服務 |
| 135 | validate-config | Adobe Commerce的搜尋引擎必須設為Elasticsearch，而Magento Open Source >= 2.4.0 | 檢查`engine`選項的SEARCH_CONFIGURATION變數。 如果已配置，請刪除該選項，或將值設置為“elasticsearch”。 |
| 136 | 驗證設定 | 拆分資料庫已從 Adobe Systems Commerce 和 Magento Open Source 2.5.0 開始移除。 | 如果使用拆分資料庫，則必須還原或遷移到單個資料庫，或者使用替代方法。 |
| 139 | validate-config | 不正確的搜尋引擎 | 此Adobe Commerce或Magento Open Source版本不支援OpenSearch。 使用2.3.7-p3、2.4.3-p2或更新版本 |

### Post-部署 階段

| 錯誤碼 | 部署後步驟 | 錯誤說明（標題） | 建議的動作 |
| - | - | - | - |
| 201 | is-deploy-failed | 部署階段失敗 |  |
| 202 |  | `./app/etc/env.php`檔不可寫 | 部署文稿無法對 `/app/etc/env.php` 檔進行所需的更改。 檢查檔案系統許可權。 |
| 203 |  | 未在`schema.yaml`檔案中定義設定 | 未在`./vendor/magento/ece-tools/config/schema.yaml`檔案中定義設定。 檢查設定變數名稱是否正確，以及是否已定義。 |
| 204 |  | 無法剖析`.magento.env.yaml`檔案 | `./.magento.env.yaml`檔案格式無效。 使用YAML剖析器來檢查語法並修正任何錯誤。 |
| 205 |  | 無法讀取`.magento.env.yaml`檔案 | 檢查檔案許可權。 |
| 206 |  | 無法讀取`.schema.yaml`檔案 |  |
| 207 | 熱身 | 無法預先載入某些熱身頁面 |  |
| 208 | 第一位元組時間 | 無法測試到第一個位元組的時間(TTFB) |  |
| 227 | clean-cache | 命令`/bin/magento cache:flush`失敗 | 如需詳細資訊，請檢視`cloud.log`。 將`VERBOSE_COMMANDS: '-vvv'`新增至`.magento.env.yaml`，以取得更詳細的命令輸出。 |

### 一般

| 錯誤碼 | 一般步驟 | 錯誤說明 （標題） | 建議的動作 |
| - | - | - | - |
| 243 |  | 檔案中未定義 `schema.yaml` 設定 | 檢查設定變數名稱是否正確，以及它是否已定義。 |
| 244 |  | 無法剖析`.magento.env.yaml`檔案 | `./.magento.env.yaml`檔案格式無效。 使用YAML剖析器來檢查語法並修正任何錯誤。 |
| 245 |  | 無法讀取`.magento.env.yaml`檔案 | 無法讀取`./.magento.env.yaml`檔案。 檢查檔案許可權。 |
| 246 |  | 無法讀取`.schema.yaml`檔案 |  |
| 247 |  | 無法產生事件的模組 | 查看瞭解更多資訊 `cloud.log` 。 |
| 248 |  | 無法啟用事件的模組 | 如需詳細資訊，請檢視`cloud.log`。 |
| 249 |  | 無法產生AdobeCommerceWebhookPlugins模組 | 如需詳細資訊，請檢視`cloud.log`。 |
| 250 |  | 無法啟用 AdobeCommerceWebhookPlugins 模組 | 如需詳細資訊，請檢視`cloud.log`。 |

## 警告錯誤

警告錯誤指出雲端基礎結構專案設定上的Commerce發生問題，例如不正確、已棄用、不支援或遺失可能影響網站作業的選用功能的設定值。 雖然警告不會導致部署失敗，但您應檢閱警告訊息並更新設定以解決問題。

### 建置階段

| 錯誤碼 | 建立步驟 | 錯誤說明 （標題） | 建議的動作 |
| - | - | - | - |
| 1001 | validate-config | 檔案app/etc/config.php不存在 |  |
| 1002 | 驗證設定 | 「 」。不再支援/build_options.ini檔案 |  |
| 1003 | validate-config | 共用設定檔案中缺少模組區段 |  |
| 1004 | validate-config | 此設定與此版本的Magento不相容 |  |
| 1005 | 驗證設定 | 已忽略 SCD 選項 |  |
| 1006 | validate-config | 設定的狀態不理想 |  |
| 1007 | run-baler | 無法使用Baler JS套裝 |  |

### 部署階段

| 錯誤碼 | 部署步驟 | 錯誤說明（標題） | 建議的動作 |
| - | - | - | - |
| 2001 | pre-部署：cache | 緩存是為不可用的 Redis 服務配置的。 配置被忽略。 |  |
| 2032 | 預先部署：快取 | 快取已針對無法使用的Valkey服務進行設定。 已忽略設定。 |  |
| 2002 | validate-config | 設定的狀態不理想 |  |
| 2003 | validate-config | 尚未設定錯誤報告的目錄巢狀層級值 |  |
| 2004 | 驗證設定 | 中的配置無效。/pub/errors/local.xml 檔案。 |  |
| 2005 | 驗證設定 | 管理資料僅在初始安裝期間用於建立管理使用者。 在升級程式期間，將忽略對管理員資料所做的任何變更。 | 初始安裝後，您可以從設定中移除管理員資料。 |
| 2006 | validate-config | 未建立管理員使用者，因為未設定管理員電子郵件 | 安裝後，您可以手動建立管理員使用者：使用ssh連線至您的環境。 然後，執行`bin/magento admin:user:create`命令。 |
| 2007 | validate-config | 將php版本更新至建議的版本 |  |
| 2008 | validate-config | Adobe Commerce和Magento Open Source 2.1已不再支援Solr。 |  |
| 2009 | validate-config | Adobe Commerce和Magento Open Source 2.2或更新版本不再支援Solr。 |  |
| 2010 | validate-config | Elasticsearch服務安裝在基礎結構層，但不用作搜尋引擎。 | 請考慮從基礎結構層中刪除Elasticsearch服務，以優化資源使用方式。 |
| 2011 | validate-config | 基礎結構層上的Elasticsearch服務版本與您的Adobe Commerce應用程式使用的elasticsearch/elasticsearch模組目前版本不相容。 |  |
| 2012 | validate-config | 目前的設定與此版本的Adobe Commerce不相容 |  |
| 2013 | validate-config | SCD 選項被忽略，因為部署進程未在版本編號階段運行 |  |
| 2014 | validate-config | 此設定包含過時的變數或值 |  |
| 2015 | validate-config | 環境設定無效 |  |
| 2016 | validate-config | JSON型別設定無法解碼 |  |
| 2017 | validate-config | 目前的設定與此版本的 Adobe Systems Commerce 不相容 |  |
| 2018 | 驗證設定 | 部分服務已通過 EOL |  |
| 2019 | validate-config | MySQL搜尋組態選項已過時 | 請改用Elasticsearch。 |
| 2029 | validate-config | Adobe Commerce和Magento Open Source 2.4.2已棄用分割資料庫，2.5將移除這些資料庫。 | 如果您使用分割資料庫，您應該開始計畫回覆或移轉至單一資料庫，或使用替代方法。 |
| 2020 | install-update | Adobe Commerce安裝完成，但`app/etc/env.php`設定檔遺失或空白。 | 從環境設定和 .magento.env.yaml 檔案還原所需數據。 |
| 2021 | install-update：db-connection | 對於使用自定義連接的拆分資料庫 |  |
| 2022 | install-update：db-connection | 您已更改為與從屬連接不相容的資料庫配置。 |  |
| 2023 | install-update：split-db | 跳過啟用拆分資料庫。 |  |
| 2024 | install-update：split-db | SPLIT_DB變數缺少分割連線對型別的組態。 |  |
| 2025 | install-update：split-db | 未設定從屬連線。 |  |
| 2026 | 預先部署：restore-writable-dirs | 無法將建置階段期間產生的部分資料還原至掛載的目錄 | 如需詳細資訊，請檢視`cloud.log`。 |
| 2027 | validate-config：mage-mode-variable | 不支援MAGE_MODE環境變數的模式值 | 移除MAGE_MODE環境變數，或將其值變更為「生產」。 Adobe Systems Commerce on 雲端基礎結構 僅支持「生產」模式。 |
| 2028 | 遠端儲存 | 無法啟用遠端儲存。 | 驗證遠端儲存憑證。 |
| 2030 | validate-config | Elasticsearch和OpenSearch服務都安裝在基礎建設層。 Adobe Commerce和Magento Open Source 2.4.4及更新版本預設使用OpenSearch | 請考慮從基礎結構層移除Elasticsearch或OpenSearch服務，以最佳化資源使用。 |

### 部署後階段

| 錯誤碼 | 部署後步驟 | 錯誤說明（標題） | 建議的動作 |
| - | - | - | - |
| 3001 | validate-config | 已在 Adobe Systems Commerce 中啟用除錯記錄 | 為節省磁盘空間，請勿為生產環境啟用偵錯記錄。 |
| 3002 | 熱身 | 無法擷取存放區url |  |
| 3003 | 熱身 | 無法擷取商店 url |  |
| 3004 | 備份 | 無法建立備份檔案 |  |

### 一般

| 錯誤代碼 | 一般步驟 | 錯誤說明 （標題） | 建議的動作 |
| - | - | - | - |
| 4001 |  | 無法取得系統處理器計數： |  |
