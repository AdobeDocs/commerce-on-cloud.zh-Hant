---
title: ECE-Tools發行說明
description: 請參閱ECE-Tools套件的最新改良清單。
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 3cbfe698-d75d-4a16-877a-52c214595344
source-git-commit: 33f89e5c9af7c172ad0592b61343e285b456fc1a
workflow-type: tm+mt
source-wordcount: '3022'
ht-degree: 0%

---

# ECE-Tools發行說明

[ece-tools](https://github.com/magento/ece-tools)套件是一組指令碼和工具，用來管理和部署雲端專案。 此發行說明說明說明說明此套件的最新改善，此套件隸屬於Commerce適用的[雲端工具套裝](cloud-tools-suite.md)。

>[!NOTE]
>
>請參閱[升級ECE-Tools](../dev-tools/update-package.md)，以取得有關更新至最新版`ece-tools`套件的資訊。

`ece-tools`封裝使用下列發行版本設定順序： `200<major>.<minor>.<patch>`

發行說明包括：

- ![新圖示](../../assets/new.svg)新功能
- ![修正圖示](../../assets/fix.svg)修正和改良

<!--Add release notes below-->

## v2002.2.1 {#latest}


發行日期： 2024年2月6日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增對PHP 8.4的支援。<!-- MCLOUD-13145	 - -->
- ![修正圖示](../../assets/fix.svg) **Opensearch的驗證器** — 修正產生錯誤服務版本之錯誤訊息的驗證器。<!-- MCLOUD-13184	 - -->


## v2002.2.0

發行日期： 2024年10月7日

- ![新圖示](../../assets/new.svg) **MariaDB 11.4** — 新增MariaDB 11.4的支援。
- ![修正圖示](../../assets/fix.svg) **重構的程式碼** — 已移除舊版PHP 7.4、7.3、7.2及相關程式庫的支援。<!-- MCLOUD-9278 -->
- ![修正圖示](../../assets/fix.svg) **升級的Monolog版本** — 新增對monolog 3.6的支援。<!-- MCLOUD-12855 -->
- ![修正圖示](../../assets/fix.svg) **RabbitMQ、MariaDB及PHP的驗證器** — 修正產生錯誤服務版本之錯誤訊息的驗證器。

## v2002.1.19

發行日期： 2024年5月21日

- ![新圖示](../../assets/new.svg) **Lua** — 已為CACHE_CONFIGURATION新增選項useLua。
- ![修正圖示](../../assets/fix.svg) **驗證器** — 已更新Redis和RabbitMQ新版本的驗證器。

## v2002.1.18

發行日期： 2024年4月8日

- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.3的支援。
- ![修正圖示](../../assets/fix.svg) **驗證器** — 已更新EOL驗證器。

## v2002.1.17

發行日期： 2024年1月16日

- ![修正圖示](../../assets/fix.svg) **Elasticsearch與OpenSearch的驗證器** — 修正啟用LiveSearch時，產生錯誤訊息以安裝搜尋服務的驗證器。<!-- MCLOUD-10167 -->
- ![修正圖示](../../assets/fix.svg) **部署警告** — 修正導致非空白資料夾出現部署警告的問題。<!-- MCLOUD-8958 -->

## v2002.1.16

發行日期： 2023年10月16日

- ![新圖示](../../assets/new.svg) **ENABLE_WEBHOOKS全域環境變數** — 已新增[ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks)全域變數，以搭配Commerce webhooks使用來連線至外部端點，例如App Builder執行階段動作或協力廠商詳細目錄管理系統。

## v2002.1.15

發行日期： 2023年7月31日

- ![修正圖示](../../assets/fix.svg) **錯誤碼** — 已更新錯誤碼結構描述和錯誤碼檔案產生器。
- ![修正圖示](../../assets/fix.svg) **自訂Redis模型的驗證器** — 已更新自訂Redis後端模型的驗證器。 [檢視快取組態的範例](../environment/variables-deploy.md#cache_configuration)。
- ![修正圖示](../../assets/fix.svg) **RabbitMQ的驗證器** — 新增RabbitMQ 3.11的支援
- ![修正圖示](../../assets/fix.svg) **修正錯誤的連結** — 修正歡迎電子郵件範本中上線檔案的錯誤連結。

## v2002.1.14

發行日期： 2023年3月10日

- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.2的支援。
- ![新圖示](../../assets/new.svg) **服務的驗證器** — 已更新Commerce 2.4.6必要服務的驗證器： MariaDB 10.6、Redis 7.0、PHP 8.2、OpenSearch 2.x和RabbitMQ 3.9。
- ![修正圖示](../../assets/fix.svg) **ece-tools db-dump** — 修正`db-dump`作業過早停止的問題。

## v2002.1.13

發行日期： 2022年10月27日

- ![新圖示](../../assets/new.svg) **已新增Adobe Commerce的Adobe I/O Events支援**。 擴充功能開發人員現在可以使用[Adobe I/O Events](https://developer.adobe.com/events/docs/)架構，將Commerce事件資訊從雲端執行個體傳送至其為[AdobeApp Builder](https://developer.adobe.com/app-builder/docs/overview/)撰寫的應用程式。 適用於Adobe Commerce的Adobe I/O Events在合作夥伴預覽中。<!-- CEXT-932 -->
- ![新圖示](../../assets/new.svg) **OPcache組態的驗證器** — 新增驗證器以檢查排除路徑的OPcache組態。<!-- MCLOUD-9485 -->
- ![修正圖示](../../assets/fix.svg) **修正GraphQL快取組態的問題** — 現在ECE-Tools會將GraphQL `id_salt`值保留在`app/etc/env.php`檔案的`cache`組態中。<!-- MCLOUD-9486 -->

## v2002.1.12

發行日期： 2022年9月13日

- ![新圖示](../../assets/new.svg) **啟用`synchronous_replication`** — 啟用`MYSQL_USE_SLAVE_CONNECTION`時，ECE-Tools會在`app/etc/env.php`檔案中設定`synchronous_replication=>true`。 此設定僅影響Commerce 2.4.6+。 檢視[部署變數](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->中的`MYSQL_USE_SLAVE_CONNECTION`變數說明
- ![新圖示](../../assets/new.svg) **OpenSearch** — 新增功能以設定和設定下一個Adobe Commerce版本2.4.6的`opensearch`引擎。請參閱[設定OpenSearch服務](../services/opensearch.md)。<!-- MCLOUD-9236 -->

## v2002.1.11

發行日期： 2022年8月4日

- ![修正圖示](../../assets/fix.svg) **ElasticSuite Validator與OpenSearch** — 修正安裝OpenSearch時的ElasticSuite完整性檢查驗證器問題。<!-- MCLOUD-8767 -->
- ![修正圖示](../../assets/fix.svg) **部署命令的傳回型別** — 修正部署命令的傳回型別。<!-- AC-3208 -->
- ![修正圖示](../../assets/fix.svg) **[!DNL RabbitMQ]新Commerce 2.4.5安裝的問題** — 修正新Commerce 2.4.5安裝上的[!DNL RabbitMQ]當機問題。<!-- MCLOUD-9059 -->

## v2002.1.10

發行日期： 2022年3月31日

- ![修正圖示](../../assets/fix.svg) **Elasticsearch7.10** — 更新驗證器以支援7.10版本的Elasticsearch。<!-- MCLOUD-8548 -->

## v2002.1.9

發行日期： 2022年3月10日

- ![新圖示](../../assets/new.svg) **OpenSearch** — 已新增對Adobe Commerce 2.4.4、2.4.3-p2和2.3.7-p3版本的OpenSearch的支援。<!-- MCLOUD-8296 -->
- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.1的支援。
- ![修正圖示](../../assets/fix.svg) **symfony/process** — 已新增與symfony/process ^5.3的相容性。<!-- MCLOUD-8283 -->

- ![新圖示](../../assets/new.svg) **取用者多個處理序** — 已新增`multiple_processes`選項，以便您可以指定每個取用者要衍生的處理序數目。 檢視[部署變數](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->中的`CRON_CONSUMERS_RUNNER`變數說明
- ![新圖示](../../assets/new.svg) **OpenSearch配置與完整主機路徑** — 已新增設定Elasticsearch配置與完整主機路徑的功能。
- ![修正圖示](../../assets/fix.svg) **AWS S3** — 變更AWS S3啟用方法。
- ![修正圖示](../../assets/fix.svg) **修正driver_options讀取器** — 已新增由`ece-tools`從`env.php`檔案讀取驗證器之DB連線的driver_options組態。<!-- MCLOUD-8420 -->

## v2002.1.8

發行日期： 2021年10月25日

- ![新圖示](../../assets/new.svg) **替代傾印位置** — 已新增`--dump-directory`選項，讓您可以選擇資料庫傾印的目標目錄。 現在`/app/var/dump-main`是DB傾印的預設目標目錄。 請參閱[備份管理：傾印您的資料庫](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![修正圖示](../../assets/fix.svg) **更新獨白** — 已將`monolog`封裝所需的最低版本更新為`^2.3`。<!-- ACMP-1263 -->
- ![修正圖示](../../assets/fix.svg) **更新Symfony** — 已更新Symfony相依性以與Adobe Commerce 2.4.4相容。<!-- ACMP-1533 -->
- ![修正圖示](../../assets/fix.svg) **功能/解決自動載入** — 修正部署至整合環境並看到`CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`錯誤時所發生的問題。<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

發行日期： 2021年7月29日

**設定更新**—

- ![新圖示](../../assets/new.svg)已新增對Composer 2.0的支援。<!--MCLOUD-8003-->

- ![修正圖示](../../assets/fix.svg) **已更新`symphony/console`**&#x200B;的撰寫器需求 — 已更新`symphony/console`套件的ECE-Tools `composer.json`版本需求，以修正導致`di:compile`命令失敗並出現下列錯誤的問題： `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![修正圖示](../../assets/fix.svg)已更新終止軟體檢查(`eol.yaml`)以包含Elasticsearch7.9.x。<!--MCLOUD-7938-->

## v2002.1.6

發行日期： 2021年4月20日

- ![新圖示](../../assets/new.svg) **Redis驗證認證** — 已新增在部署階段從`relationships`屬性讀取Redis授權認證的功能。<!--MCLOUD-7694-->

- ![新圖示](../../assets/new.svg) **Elasticsearch授權認證** — 已新增在部署階段從`relationships`屬性讀取Elasticsearch授權認證的能力。<!--MCLOUD-7695-->

- ![新圖示](../../assets/new.svg) **專用工作階段存放服務** — 已新增`redis-session`作為工作階段存放的第二個選項。 您可以使用`redis-session`服務來儲存工作階段資訊，並使用`redis`服務來快取，以提供更好的效能。<!--MCLOUD-7698-->

- ![新圖示](../../assets/new.svg) **已棄用的SPLIT_DB訊息** — 已針對Adobe Commerce 2.4.2的已棄用`SPLIT_DB`選項新增驗證器警告和嚴重訊息，並將其在Adobe Commerce 2.5.0中移除。<!--MCLOUD-7806-->

- ![修正圖示](../../assets/fix.svg) **來自關係的Elasticsearch版本** — 修正服務驗證器，以從Cloud Docker和整合環境中的`relationships`屬性擷取正確的Elasticsearch版本。<!--MCLOUD-7572-->

- ![修正圖示](../../assets/fix.svg) **彈性Redis連線埠驗證**—Redis現在可以從`server` URL驗證自訂快取連線中的連線埠。 例如，您可以將連線埠號碼新增至伺服器URL，如下所示： `server: 'tcp://rfs-store-simple-page-cache:26379'`。 這有助於避免發生`port`選項遺失或不正確的驗證錯誤。<!--MCLOUD-7722-->

- ![修正圖示](../../assets/fix.svg) **升級至Adobe Commerce 2.4.2** — 修正使用者在升級至Adobe Commerce 2.4.2後必須手動執行`bin/magento setup:upgrade`才能使其網站運作的問題。<!--MCLOUD-7776-->

## v2002.1.5

發行日期： 2021年2月1日

- ![新圖示](../../assets/new.svg) **遠端儲存** — 已新增`REMOTE_STORAGE`環境變數，以啟用雲端專案來使用儲存服務(例如AWS S3)遠端儲存媒體檔案。 此設定選項是ECE-Tools套件的一部分，但在雲端基礎結構上的Adobe Commerce上不受支援。<!--MCLOUD-7153-->

- ![新圖示](../../assets/new.svg) **新`cloud:config:validate`命令** — 已新增命令`php vendor/bin/ece-tools cloud:config:validate`，以便在推送變更至遠端雲端環境之前驗證`.magento.env.yaml`設定。<!--MCLOUD-7120-->

- ![新圖示](../../assets/new.svg) **正在排清opcache** — 已新增對`opcache.enable_cli` PHP選項的支援，可在執行部署掛接之前排清OPcache。 此組態會重設快取組態，以確保目前的組態設定會套用到每個部署。<!--MCLOUD-7015-->

- ![新圖示](../../assets/new.svg) **Aurora DB的驗證** — 已更新資料庫服務驗證，使其與Aurora資料庫相容。<!--MCLOUD-7269-->

- ![新圖示](../../assets/new.svg) **新SCD_NO_PARENT環境變數** — 已新增`SCD_NO_PARENT`環境變數(適用於Adobe Commerce >=2.4.2)以管理父系主題的靜態內容產生。<!--MCLOUD-7284-->

- ![修正圖示](../../assets/fix.svg) **記憶體限制和命令** — 修正`cloud.log`檔案大小超過PHP memory_limit時，`php vendor/bin/ece-tools`命令無法運作的問題。 現在我們只從記錄檔讀取較小的資料子集，而不將整個`cloud.log`檔案讀取到記憶體中。<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![修正圖示](../../assets/fix.svg) **自訂資料庫連線** — 修正未使用為`DATABASE_CONFIGURATION`定義的自訂資料庫連線的`.magento.env.yaml`組態問題。 未將連線設定新增到`app/etc/env.php`.<!--MCLOUD-7426-->

- ![修正圖示](../../assets/fix.svg) **空的錯誤記錄檔** — 修正在`cloud.error.log`為空時導致部署失敗的問題。<!--MCLOUD-7296-->

- ![修正圖示](../../assets/fix.svg) **MariaDB 10.3驗證** — 修正Adobe Commerce 2.3.6-p1的MariaDB 10.3驗證。<!--MCLOUD-7416-->

- ![修正圖示](../../assets/fix.svg) **快取：排清記錄** — 已改善記錄專案以指出`cache:flush`步驟的開始和完成。<!--MCLOUD-7503-->

## v2002.1.4

發行日期： 2020年11月19日

- ![修正圖示](../../assets/fix.svg)修正當`SEARCH_CONFIGURATION`環境變數中指定的搜尋引擎不是`elasticsearch`時，造成部署失敗的問題。<!--MCLOUD-7283-->

## v2002.1.3

發行日期： 2020年11月9日

**基礎結構更新**—

- ![新圖示](../../assets/new.svg)已新增當靜態內容設定為在建置階段中部署時，唯讀`pub/static`目錄的ECE-Tools支援。<!--MC-37699-->

- ![新圖示](../../assets/new.svg)已新增對Elasticsearch 7.9和Redis 6的支援，以便與即將發行的Adobe Commerce版本相容。<!--MCLOUD-7191-->

- ![修正圖示](../../assets/fix.svg)已更新ECE-Tools `composer.json`，為品質修補程式工具新增必要的相依性。 這會修正ECE-Tools和magento-cloud-patches套件之間存在的循環相依性。<!--MCLOUD-6910-->

**驗證和記錄改善**—

- ![新圖示](../../assets/new.svg)已新增搜尋引擎驗證，以確保在雲端基礎結構2.4和更新版本上為Adobe Commerce設定`elasticsearch`。 如果驗證失敗，部署將停止，並顯示嚴重錯誤訊息，建議修正問題。 檢視[嚴重錯誤，部署階段](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![新圖示](../../assets/new.svg)已新增Elasticsearch驗證，以檢查Elasticsearch服務版本與Adobe Commerce版本之間的相容性。<!--MCLOUD-7193-->

- ![新圖示](../../assets/new.svg)已更新Elasticsearch相容性錯誤訊息，以顯示與Adobe CommerceElasticsearch模組相容的Elasticsearch版本。 錯誤訊息現在會提供要在您的雲端基礎結構中安裝的特定Elasticsearch版本，以便與您的Adobe Commerce版本使用的Elasticsearch模組相容。 檢視[警告錯誤，部署階段](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![新圖示](../../assets/new.svg)已針對無效的`MAGE_MODE`環境變數設定新增警告錯誤`2026`和`2027`。 唯一有效值為`production`。 在此修正之前，`MAGE_MODE`可以設定為`developer`且沒有部署錯誤，只會在稍後嘗試寫入唯讀檔案時造成錯誤。 檢視[警告錯誤](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![修正圖示](../../assets/fix.svg)修正Redis、RabbitMQ和MySQL服務的驗證，以確保這些版本與Adobe Commerce版本相容。 這些服務的有效版本現在已寫入`cloud.log`.<!--MCLOUD-7098-->

- ![修正圖示](../../assets/fix.svg)已更新`cloud.log`以納入快取熱身期間傳送要求的並行要求限制。 此值是在[WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency)部署後變數中設定。<!--MCLOUD-5563-->

**CLI命令更新**—

- ![新圖示](../../assets/new.svg)已新增CLI命令（`cloud:config:create`和`cloud:config:update`），以使用可包含一或多個組建、部署和部署後變數的設定來建立和更新`.magento.env.yaml`檔案。 請參閱[從CLI建立組態檔](../environment/configure-env-yaml.md#create-configuration-file-from-cli)。<!--MCLOUD-7072-->

**環境變數更新**—

- ![新圖示](../../assets/new.svg)已新增[SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload)組建變數。 將變數設為`true`會在Commerce的Cloud Docker安裝期間停止應用程式執行`composer dump-autoload`命令。 此變數僅與具有可寫入檔案系統（使用`./vendor/bin/ece-docker build:compose --with-test`為測試和開發而建立）之Commerce容器的Cloud Docker相關。 使用這類安裝，略過`composer dump-autoload`命令可防止執行其他命令時嘗試從已刪除的`generated`目錄存取檔案時發生錯誤。<!--MCLOUD-6939-->

## v2002.1.2

發行日期： 2020年8月5日

**驗證和記錄改善**—

- ![新圖示](../../assets/new.svg)已新增`schema.error.yaml`檔案，其中包含建置、部署和部署後程式期間可能發生的所有錯誤和警告通知，以及解決錯誤的建議。 此檔案中的資訊也可在Commerce的&#x200B;_雲端指南_&#x200B;中取得。 檢視ece-tools](../dev-tools/error-reference.md)的[錯誤訊息參考。<!--MCLOUD-5878-->

- ![新圖示](../../assets/new.svg)已將雲端錯誤記錄(`/var/log/cloud.error.log`)專案變更為JSON格式，以便以程式設計方式更輕鬆地剖析記錄。<!--MCLOUD-5879-->

- ![新圖示](../../assets/new.svg)已新增其他錯誤檢查以建置、部署和部署後處理，並改善現有檢查：

   - 錯誤碼2026 — 無法將在建置階段產生的一些資料還原到掛載的目錄

   - 錯誤碼3004 — 無法建立備份檔案

   - 錯誤碼102 — 新增當`env.php`檔案不可寫入<!--MCLOUD-6221-->時所發生問題的額外檢查

- ![新圖示](../../assets/new.svg)已新增&#x200B;**QUALITY_PATCH**&#x200B;環境變數，以指定一或多個要在部署過程中套用的品質修補程式。 檢視[建置變數](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

發行日期： 2020年6月25日

- ![新圖示](../../assets/new.svg) **基礎結構更新**—

   - ![新圖示](../../assets/new.svg) **記錄改善** — 將退出代碼指派給嚴重的部署錯誤，並在錯誤訊息通知和記錄事件中公開退出代碼，藉此改善記錄追蹤功能。 檢視ece-tools](../dev-tools/error-reference.md)的[錯誤訊息參考。<!-- MCLOUD-5637, 5531-->

   - ![新圖示](../../assets/new.svg)改善資料庫傾印程式(`vendor/bin/ece-tools db-dump`)和更新的記錄訊息，以釐清資料庫傾印作業會將應用程式切換到維護模式、停止消費者佇列程式，以及在傾印開始之前停用cron工作。<!--MCLOUD-5324, MCLOUD-2062-->

   - ![修正圖示](../../assets/fix.svg)已修正問題，以確保在部署至中繼和生產環境時，專案URL會正確更新。 現在，`ece-tools`使用專案路由設定中設定了`primary:true`屬性的路由URL。 請參閱[部署變數](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![修正圖示](../../assets/fix.svg)已更新套用修補程式的`generate.xml`組建案例工作流程。 必須先套用修補程式，才能更新Adobe Commerce，修正可能導致`di:compile`和`module:refresh`步驟失敗的任何問題。<!--MCLOUD-5941-->

   - ![修正圖示](../../assets/fix.svg)修正安裝過程中傳回`Crypt key missing`錯誤的問題。 `crypt/key`值會在安裝期間自動產生。<!--MCLOUD-6120-->

- ![新圖示](../../assets/new.svg) **服務更新**—

   - ![新圖示](../../assets/new.svg)已新增對PHP 7.4和MariaDB 10.4的支援。<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![新圖示](../../assets/new.svg) **環境變數更新**—

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**SCD_USE_BALER**&#x200B;變數，以在Adobe Commerce雲端基礎結構建置程式期間啟用JavaScript套件組合的Baler模組。 檢視[組建變數](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->中的變數說明

   - ![新圖示](../../assets/new.svg)已新增&#x200B;**REDIS_BACKEND**&#x200B;環境變數，以設定Adobe Commerce 2.3.5或更新版本的Redis快取的Redis後端模型。 檢視[部署變數](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->中的變數說明

- ![新圖示](../../assets/new.svg) **CLI命令更新**—

   - ![新圖示](../../assets/new.svg)已更新下列CLI命令，其中包含更詳細的記錄選項：

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     每個呼叫的記錄層級由`.magento.env.yaml`檔案中[`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands)變數的組態決定。<!--MCLOUD-3503-->

- ![新圖示](../../assets/new.svg) **驗證改善**—

   - ![新圖示](../../assets/new.svg) **Elasticsearch7.x相容性檢查** — 已更新Elasticsearch7.x軟體相容性檢查的Elasticsearch驗證。<!--MCLOUD-5542-->

   - ![新圖示](../../assets/new.svg) **已更新服務版本和EOL驗證檢查** — 已更新驗證以根據Adobe Commerce 2.4檢查已安裝的服務版本。<!--MCLOUD-6144-->

   - ![修正圖示](../../assets/fix.svg)修正驗證問題，因此只有在`.magento.app.yaml`檔案中缺少`post-deploy`連結設定時，才會顯示下列部署後警告訊息：

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![新圖示](../../assets/new.svg) **已新增Zend Framework相依性的驗證** — 已新增已移轉至Laminas專案的Zend Framework的撰寫器相依性驗證。 如果缺少必要的相依性，則在建置過程中會顯示以下錯誤訊息。

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     請參閱[驗證Zend Framework相依性](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![新圖示](../../assets/new.svg) **已新增`env.php`檔案和資料的驗證** — 在安裝和升級過程中已新增`env.php`檔案和資料的檢查。<!--MCLOUD-5991-->

      - 如果安裝遺失`env.php`檔案，且`.magento.app.yaml`檔案中未指定`crypt/key`值，則部署會失敗，並出現下列通知：

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - 如果安裝不包含`env.php`檔案，或組態只包含一個快取型別，則在升級過程中會執行`cron:enable`命令，以還原包含所有`cache_types`的檔案。 下列通知已新增至記錄檔：

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

發行日期： 2020年2月6日

- ![新圖示](../../assets/new.svg) **基礎結構更新**—

   - ![新圖示](../../assets/new.svg) **新增適用於Commerce的Cloud Docker的個別套件** — 將Docker套件與`ece-tools`套件脫鉤，以維持程式碼品質並提供獨立的發行版本。 從[magento-cloud-docker](https://github.com/magento/magento-cloud-docker) GitHub存放庫管理與`ece-tools`相關的更新與修正。<!--MAGECLOUD-2927-->

   - ![新圖示](../../assets/new.svg) **更新的修補功能** — 將修補功能從ECE-Tools套件移至單獨的[magento-cloud-patches](https://github.com/magento/magento-cloud-patches)套件。 部署期間，`ece-tools`會使用新套件套用修補程式。 請參閱[雲端修補程式發行說明](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![新圖示](../../assets/new.svg) **已更新撰寫器相依性** — 已更新雲端基礎結構上Adobe Commerce的`composer.json`檔案與`magento/magento-cloud-docker`套件的相依性。 現在，`ece-tools`包含[`Cloud Tools Suite for Commerce`](cloud-tools-suite.md)中所有套件的相依性。 當您安裝或更新`ece-tools`時，會自動安裝及更新這些套件。

- ![新圖示](../../assets/new.svg) **情境式部署的支援**—<!--MAGECLOUD-4101-->

   - ![新圖示](../../assets/new.svg)現在您可以使用XML組態檔自訂建置、部署和部署後程式，以覆寫或自訂預設組態。

   - ![新圖示](../../assets/new.svg) **已在`.magento.app.yaml`**&#x200B;中變更`hooks`設定 — 我們已更新`hooks`設定格式以支援案例部署。 舊版ECE-Tools 2002.0.x仍受支援。 不過，您必須更新為新格式，才能使用以案例為基礎的部署功能。 請參閱[案例部署](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks)。

>[!NOTE]
>
>在更新至ECE-Tools 2002.1.0版之前，請先向後檢閱[   不相容的變更](backward-incompatible-changes.md)，瞭解可能需要您執行的變更   在雲端基礎結構專案設定或流程上更新Adobe Commerce。

- ![新圖示](../../assets/new.svg) **服務更新**—

   - ![新圖示](../../assets/new.svg)已新增對PHP 7.3的支援。<!--MAGECLOUD-4022-->

   - ![新圖示](../../assets/new.svg)已新增對RabbitMQ 3.8的支援。<!--MAGECLOUD-4674-->

   - ![新圖示](../../assets/new.svg)已新增驗證，以對照每項服務的EOL日期檢查已安裝的服務版本。 現在，如果服務版本在EOL日期後的三個月內，客戶將會收到通知，如果EOL日期是過去，客戶將會收到警告。<!--MAGECLOUD-4076-->

   - ![修正圖示](../../assets/fix.svg)修正Elasticsearch設定問題，以確保在所有環境中都設定了正確的Elasticsearch設定。<!--MAGECLOUD-4474-->

>[!NOTE]
>
>請參閱[服務版本](../services/services-yaml.md#service-versions)，以取得雲端基礎結構上Adobe Commerce中所使用的服務清單，及其版本與雲端範本的相容性。

- ![新圖示](../../assets/new.svg) **環境變數更新**—

   - ![新圖示](../../assets/new.svg)已擴充`WARM_UP_PAGES`環境變數的功能，以支援特定產品頁面的快取預先載入。 請參閱[部署後變數](../environment/variables-post-deploy.md#warm_up_pages)主題中的展開定義。<!--MAGECLOUD-4444-->

   - ![新圖示](../../assets/new.svg)已新增`ERROR_REPORT_DIR_NESTING_LEVEL`環境變數，以簡化`<magento_root>/var/report/`目錄中的錯誤報告資料管理。 請參閱[組建變數](../environment/variables-build.md#error_report_dir_nesting_level)主題中的變數說明。

   - ![修正圖示](../../assets/fix.svg)已移除`SCD_EXCLUDE_THEMES`、`STATIC_CONTENT_THREADS`、`DO_DEPLOY_STATIC_CONTENT`和`STATIC_CONTENT_SYMLINK`環境變數。 請參閱[回溯不相容的變更](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![修正圖示](../../assets/fix.svg)修正Elastic Suite組態程式中的問題，以便在您設定不含`_merge`選項的`ELASTICSUITE_CONFIGURATION`部署變數時，依照預期覆寫預設組態。<!--MAGECLOUD-4388-->

- ![新圖示](../../assets/new.svg) **CLI命令更新**—

   - ![新圖示](../../assets/new.svg) **新cron命令** — 您現在可以使用`cron:disable`和`cron:enable`命令，在雲端基礎結構環境上的Adobe Commerce中手動管理cron處理。 使用disable命令可停止所有作用中的cron處理序，並停用所有cron工作。 使用enable指令可在準備就緒時重新啟用cron作業。 請參閱[停用cron工作](../application/crons-property.md#disable-cron-jobs)。

   - ![新圖示](../../assets/new.svg) **已改善錯誤報告** — 已針對ECE-Tools處理期間發生的CLI命令失敗新增更佳的記錄。<!--MAGECLOUD-4849-->

   - ![新圖示](../../assets/new.svg) **移除已棄用的組建命令** — 移除下列組建命令： `m2-ece-build`、`m2-ece-deploy`、`m2-ece-scd-dump`，並將`ece-tools docker`命令重新命名為`ece-docker`。 請參閱[回溯不相容的變更](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![新圖示](../../assets/new.svg)已移除已棄用的`build_options.ini`檔案，並新增驗證，以便在檔案存在時讓組建失敗。 使用[.magento.env.yaml](../environment/configure-env-yaml.md)檔案來設定組建選項。

- ![修正圖示](../../assets/fix.svg)修正當`config.php`檔案為空時，造成建置流程失敗的問題。<!--MAGECLOUD-4127-->

## 2002.0.23

發行日期： 2020年2月27日

- ![修正圖示](../../assets/fix.svg)修正`ece-tools` 2002.0.x發行版本的相容性問題，此問題導致隨選靜態內容產生無法在生產模式中成功完成。

## 較舊的版本

請參閱2002.0.22版及舊版的[發行說明封存](cloud-release-archive.md)。
