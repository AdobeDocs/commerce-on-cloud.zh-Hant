---
title: Cloud Docker包
description: 請參閱Cloud Docker套件最新改良的清單。
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2025-04-07T00:00:00Z
exl-id: 95cf4f30-6bce-4bac-8e11-cfe53cac2c70
source-git-commit: 5e991f974f33b35497b09c10fde36850c6279586
workflow-type: tm+mt
source-wordcount: '3710'
ht-degree: 0%

---

# Cloud Docker包

[`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker)套件提供功能和Docker影像，以將Adobe Commerce部署至本機雲端環境。 此套件是[適用於Commerce](cloud-tools-suite.md)的Cloud Tools Suite的元件，本發行說明說明將說明此套件的最新改善。

`magento/magento-cloud-docker`封裝使用以下版本順序： `<major>.<minor>.<patch>`

發行說明包括：

- ![新圖示](../../assets/new.svg)新功能
- ![修正圖示](../../assets/fix.svg)修正和改良

<!--Add release notes below-->

## v1.4.2 {#latest}

發行日期： 2025年4月7日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增`php-cli` 8.4和`php-fpm` 8.4影像。


## v1.4.1

發行日期： 2025年2月6日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增對PHP 8.4的支援。


## v1.4.0

發行日期： 2024年10月7日

- ![修正圖示](../../assets/fix.svg) **重構的程式碼** — 已移除對舊PHP版本(7.4、7.3、7.2)及相關程式庫與影像的支援。

## v1.3.7

發行日期： 2024年4月8日

- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.3和PHP 8.3影像的支援。
- ![新圖示](../../assets/new.svg) **Nginx** — 已新增影像nginx v. 1.24。
- ![新圖示](../../assets/new.svg) **Opensearch** — 已新增影像OpenSearch v. 2.12、1.3。
- ![新圖示](../../assets/new.svg) **Composer** — 已將Composer版本更新為2.2.23。

## v1.3.6

發行日期： 2023年7月31日

- ![新圖示](../../assets/new.svg) **已新增服務版本**—OpenSearch 2.5。
- ![新圖示](../../assets/new.svg) **啟用撰寫器快取** — 現在您可以延伸Docker設定，以便在啟動Docker容器時啟用撰寫器清除快取。 請參閱&#x200B;_適用於Commerce的Cloud Docker_&#x200B;指南中的[延伸Docker設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/)。

## v1.3.5

發行日期： 2023年3月10日

- ![新圖示](../../assets/new.svg) **ionCube** — 已新增PHP 8.1影像的ionCube延伸。
- ![新圖示](../../assets/new.svg) **已新增服務版本**—OpenSearch 2.3和2.4、PHP 8.2、Varnish 7.1.1。
- ![新圖示](../../assets/new.svg) **PHP 8.2**&#x200B;的增強型支援 — 已修正某些PHP 8.2.x版本的相容性問題，以支援Commerce 2.4.6。
- ![修正圖示](../../assets/fix.svg) **Composer問題** — 修正在Docker容器中更新Composer版本後發生的問題。

## v1.3.4

發行日期： 2022年10月27日

- ![新圖示](../../assets/new.svg) **已新增上光影像** — 已新增上光影像6.5、7.0和7.1。<!-- MCLOUD-7879 -->

## v1.3.3

發行日期： 2022年9月13日

- ![新圖示](../../assets/new.svg) **Apple M1 (ARM64)支援** — 已新增對Docker影像的變更，以啟用Apple M1 (ARM64)架構的支援。<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![修正圖示](../../assets/fix.svg) **Mailhog** — 修正Mailhog服務在開發人員模式中未收到電子郵件的問題。<!-- MCLOUD-8643 -->
- ![修正圖示](../../assets/fix.svg) **init-docker.sh** — 修正`init-docker.sh`指令碼中的服務版本驗證器。<!-- MCLOUD-8765 -->

## v1.3.2

發行日期： 2022年3月31日

- ![新圖示](../../assets/new.svg) **已新增Elasticsearch 7.10影像**<!-- MCLOUD-8548 -->

## v1.3.1

發行日期： 2022年3月10日

- ![新圖示](../../assets/new.svg) **支援PHP 8.1** — 新增支援PHP 8.1。
- ![新圖示](../../assets/new.svg) **OpenSearch** — 已新增OpenSearch 1.1和1.2版的影像。
- ![新圖示](../../assets/new.svg) **Composer 2.1** — 預設在PHP 8.x影像中設定composer 2.1.x。
- ![新圖示](../../assets/new.svg) **PHP影像改善**—

   - 新增PHP 8.1影像
   - 已升級xDebug 3.1.2版
   - 已升級xmlrpc 1.0.0RC3

- ![修正圖示](../../assets/fix.svg) **Elasticsearch和OpenSearch改良功能**—Elasticsearch和OpenSearch Dockerfiles中的改良功能；已移除Elasticsearch 5.2影像。
- ![修復圖示](../../assets/fix.svg) **Na延伸模組** — 預設已在所有PHP影像中啟用`sodium`延伸模組。
- ![修正圖示](../../assets/fix.svg) **Composer快取磁碟區** — 修正Composer快取磁碟區具有快取Composer套件的路徑。
- ![修正圖示](../../assets/fix.svg) **nginx**&#x200B;中的記憶體限制 — 修正NGINX影像中的記憶體限制。

## v1.3.0

發行日期： 2021年10月25日

- ![修正圖示](../../assets/fix.svg) **改善開發人員模式工作流程** — 之前，您需要在建置和部署步驟中指定模式。 現在，`build`步驟中的`--mode`選項會決定稍後`deploy`步驟中的模式。 不再需要於部署後設定模式。 檢視[開發人員模式](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/).<!-- ACMP-1086 -->
- ![修正圖示](../../assets/fix.svg) **唯讀檔案系統的改善**—<!-- ACMP-1106 -->
   - 修正啟動郵件設定的PHP容器問題。
   - 可以在INI檔案中使用環境變數。
   - 請確定PHP進入點不需要寫入許可權。
- ![修正圖示](../../assets/fix.svg) **更新節點** — 更新隨附的節點版本；在PHP-CLI影像中安裝Node時，現在會使用目前的LTS版本。<!-- ACMP-1539 -->
- ![修正圖示](../../assets/fix.svg) **更新Symfony** — 已更新Symfony設定相依性以與Adobe Commerce 2.4.4相容。<!-- ACMP-1533 -->

## v1.2.4

發行日期： 2021年7月29日

- ![新圖示](../../assets/new.svg) **新`Zookeeper`容器** — 已新增[Zookeeper容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#zookeeper-container)，以管理未部署至雲端基礎結構上Adobe Commerce之專案的鎖定提供者設定。<!--MCLOUD-8000-->

- ![新圖示](../../assets/new.svg) **已新增對Composer 2.0的支援。** — 已將Composer 2.0版新增至Composer設定檔，以支援即將終止的Composer 1.0升級。<!--MCLOUD-8003-->

## v1.2.3

發行日期： 2021年6月14日

- ![新圖示](../../assets/new.svg) **新增PHP 8.0** — 將PHP更新至8.0版，讓您能夠利用PHP 8.0包含的所有新功能和最佳化。<!--MCLOUD-7941-->
- ![新圖示](../../assets/new.svg) **已更新為Varnish 6.6和Elasticsearch 7.11.2** — 下列連結提供有關[Varnish Cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0)和Elasticsearch 7.11.2的發行資訊。<!--MCLOUD-7921-->
- ![新圖示](../../assets/new.svg) **已新增PHP 7.4影像的`ioncube`延伸模組** — 在最初從PHP 7.3升級至PHP 7.4後，`ioncube`延伸模組已重新新增至PHP 7.4影像中。 *[由mattskr](https://github.com/magento/magento-cloud-docker/pull/314)提交。*<!--PR #314-->
- ![新圖示](../../assets/new.svg) **新增檔案同步選項：`manual-native`** — 此`manual-native`檔案同步選項提供手動控制同步處理，可為macOS和Windows環境提供最佳效能。 閱讀在[開發人員模式](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/)中使用`manual-native`選項和[在Docker開發人員環境中同步資料](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/#file-synchronization-options).<!--MCLOUD-7977-->的相關資訊
- ![新圖示](../../assets/new.svg) **已從`up`和`down`命令中移除磁碟區刪除** — 已從`bin/magento-docker up`和`bin/magento-docker down`命令中移除`--volume`選項，並以帶有資料遺失警告的新`bin/magento-docker init`命令取代。 此變更有助於防止意外資料遺失。 *[由joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319)提交。*<!--PR #319-->
- ![修正圖示](../../assets/fix.svg) **已更新所產生憑證的`CN`值** — 已從Dockerfile移除硬式編碼的`CN`值。 這個值已建立憑證錯誤(`NET::ERR_CERT_INVALID`)，導致`ece-docker build:compose`命令的`--host`選項被忽略。<!--MCLOUD-7934-->

## v1.2.2

發行日期： 2021年4月20日

- ![新圖示](../../assets/new.svg) **已更新`host.docker.internal`為平台獨立** — 您現在可以為Ubuntu、Windows和macOS建立相同的Docker Compose指令碼。 在Ubuntu上使用Xdebug不再需要個別的環境變數。 由Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299)提交的[修正。<!--Issue #298-->
- ![新圖示](../../assets/new.svg) **已更新init-docker.sh** — 已將`mounts`物件新增至`MAGENTO_CLOUD_APPLICATION`環境變數。 由Chiranjevi](https://github.com/magento/magento-cloud-docker/pull/299)提交的[修正。<!--Issue #299-->
- ![新圖示](../../assets/new.svg) **已更新init-docker.sh** — 已使用PHP 7.4和Cloud Docker 1.2.1版本更新`init-docker.sh`指令碼。 [由Adarsh Manickam提交的修正](https://github.com/magento/magento-cloud-docker/pull/300)。<!--Issue #300-->
- ![新圖示](../../assets/new.svg) **預設啟用** — 預設啟用PHP Docker影像中的`sodium` PHP延伸模組。<!--MCLOUD-7548-->
- ![新圖示](../../assets/new.svg) **`custom-registry`選項** — 已將`--custom-registry`選項新增至`php ./vendor/bin/ece-docker build:compose`命令，以使用您自己的影像登入。<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![新圖示](../../assets/new.svg) **已移除舊的Elasticsearch版本** — 已從Elasticsearch影像中移除Elasticsearch版本1.7和2.4。<!--MCLOUD-7504-->
- ![新圖示](../../assets/new.svg) **自動產生NGINX憑證** — 已從NGINX影像移除現有的憑證。 NGINX憑證現在會隨著每個新部署自動產生，以提高安全性。<!--MCLOUD-7396-->
- ![修正圖示](../../assets/fix.svg) **已啟用`opcache.validate_timestamps`** — 在開發人員模式中預設啟用`opcache.validate_timestamps` PHP設定。 啟用此設定修正了Docker無法辨識檔案系統變更的問題。<!--MCLOUD-7466-->
- ![修正圖示](../../assets/fix.svg) **修正`build:custom:compose`** — 修正`build:custom:compose`命令，以在建置程式期間無法覆寫檔案時擲回錯誤。 擲回錯誤可防止`docker-compose up`使用錯誤檔案的情況。<!--MCLOUD-7457-->
- ![修正圖示](../../assets/fix.svg) **修正`--sync_engine="native"`選項** — 修正生產模式(`--mode="production"`)中，`--sync_engine="native"`選項不會在`docker.composer.yml`檔案中建立任何本機資料夾專案的問題。<!--MCLOUD-7254-->
- ![修正圖示](../../assets/fix.svg) **修正服務版本驗證錯誤** — 已將[!DNL RabbitMQ]、Elasticsearch及其他服務的服務版本新增至`MAGENTO_CLOUD_RELATIONSHIP`變數中的`type`屬性。 將這些版本新增至`relationships`變數，修正了部署階段發生的驗證錯誤。<!--MCLOUD-7572-->

## v1.2.1

發行日期： 2020年12月21日

- ![新圖示](../../assets/new.svg) **NGINX命令選項** — 已新增組建命令選項，以變更TLS和Web服務的NGINX `worker_processes`和NGINX `worker_connections`數目。 `worker_process`引數保留將值設定為`auto`的能力。 範例： <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![新圖示](../../assets/new.svg) **TLS命令選項** — 已新增組建命令選項，以便在沒有TLS服務的情況下建立組態。 範例： <!--MCLOUD-7259-->

  ```bash
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![新圖示](../../assets/new.svg) **NGINX記憶體耗用量** — 已減少NGINX處理序對TLS和Web服務所耗用的記憶體。<!--MCLOUD-7259-->

- ![新圖示](../../assets/new.svg) **Blackfire** — 預設已在Cloud Docker映像中停用Blackfire PHP延伸模組。

- ![修正圖示](../../assets/fix.svg) **PHP-FPM容器** — 將`WEB_PORT`從`80`變更為`8080`以修正PHP-FPM容器健康情況檢查。<!--MCLOUD-7232-->

- ![修正圖示](../../assets/fix.svg) **無效的磁碟區命名** — 修正開發人員模式中無效的磁碟區命名錯誤。<!--MCLOUD-7442-->

- ![修復圖示](../../assets/fix.svg) **NGINX上游連線埠** — 已更新Docker NGINX 1.19影像以使用連線埠8080，以避免無限回圈。 [由Adarsh Manickam提交的修正](https://github.com/magento/magento-cloud-docker/pull/296)。<!--Issue 295-->

## v1.2.0

發行日期： 2020年11月9日

- ![新圖示](../../assets/new.svg) **容器更新 —**

   - ![新圖示](../../assets/new.svg) **PHP-FPM容器** — 已新增對gnupg PHP擴充功能的支援。 由G Arvind從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![修正圖示](../../assets/fix.svg) **資料庫容器** — 將必要的資料庫密碼加入健康狀態檢查命令，修正資料庫容器健康狀態檢查。<!--MCLOUD-7122-->

   - ![新圖示](../../assets/new.svg) **Elasticsearch容器**

      - 新增對Elasticsearch 7.9的支援，以與即將發行的Adobe Commerce版本相容。<!--MCLOUD-7190-->

      - **Elasticsearch外掛程式組態** — 新增支援，以便使用`services.yaml`檔案中的Elasticsearch外掛程式組態資訊來產生Commerce環境適用的Cloud Docker的`docker-compose.yaml`檔案。 請參閱[Elasticsearch外掛程式](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Elasticsearch外掛程式支援** — 已新增對下列Elasticsearch外掛程式的支援： `analysis-icu`、`analysis-phonetic`、`analysis-stempel`和`analysis-nori`。 預設會安裝`analysis-icu`和`analysis-phonetic`外掛程式。 您可以視需要新增或移除`analysis-stempel`和`analysis-nori`外掛程式。<!--MCLOUD-2789-->

   - ![新圖示](../../assets/new.svg) **CLI容器**

      - **在Docker PHP容器內執行命令** — 現在您可以使用Cloud Docker CLI在Docker環境中的PHP容器內執行命令，而無需在主機上安裝PHP。 例如，下列命令會建置組態： `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`。 請參閱[Cloud Docker CLI](https://developer.adobe.com/commerce/cloud-tools/docker/quick-reference/#cloud-docker-cli)。 由G Arvind從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - 將OpenSSH-client新增至PHP CLI容器。 現在，如果`composer.json`檔案包含私人Git存放庫，且需要ssh使用者端才能使用Composer命令，您就可以使用Composer的ssh代理程式轉送。<!--MCLOUD-6008-->

   - ![修正圖示](../../assets/fix.svg) **TLS容器** — 現在，[TLS容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container)是以`https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Docker影像為基礎，而非CentOS影像。 此變更修正了在Cloud Docker環境中的容器之間傳送HTTPS請求時出現錯誤的問題。<!--MCLOUD-6469-->

   - ![新圖示](../../assets/new.svg) **測試容器** — 已新增用於應用程式測試的測試容器，並新增`--with-test`選項至Docker `build:compose`命令，以僅在Docker環境中測試時建立容器。 請參閱[應用程式測試](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/).<!--MCLOUD-6394-->

   - ![新圖示](../../assets/new.svg) **FPM-XDEBUG容器**

      - ![新圖示](../../assets/new.svg) **在Linux上設定Xdebug** — 將`--set-docker-host`選項新增至`ece-docker build:compose`命令，以設定Xdebug容器中的`host.docker.internal`值。 必須在Linux系統上使用Xdebug才能使用此選項。 請參閱[為Docker設定Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/)。<!--MCLOUD-6430-->

      - ![修正圖示](../../assets/fix.svg)修正Docker ENTRYPOINT的Xdebug變數設定，以解決記錄中的`uninitialized "with_xdebug" variable`個錯誤。 [由Florent Olivaud提交的修正](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![新圖示](../../assets/new.svg) **Docker設定變更**

   - **MailHog組態** — 現在您可以使用下列`ece-docker build:compose`命令選項來停用MailHog並指定連線埠： `--no-mailhog`、`--mailhog-http-port`和`--mailhog-smtp-port`。 請參閱[設定電子郵件](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - 對於Commerce 1.2.0及更高版本的Cloud Docker，Adobe現在為每個修補版本提供Docker影像，並且Docker配置生成器使用指定的修補版本建立Docker配置，而不是使用最新的修補版本。 以前，Docker配置生成器使用最新修補版本構建配置，這可能破壞使用早期版本構建的Commerce環境的Cloud Docker。<!--MCLOUD-7093-->

   - **在自訂Cloud Docker組態中指定自訂影像和版本** — 更新了`build:custom:compose`命令，其中包含產生自訂Docker撰寫組態檔(`docker-compose.yaml`)時指定自訂影像和版本的選項。 請參閱[建置自訂Docker撰寫設定](https://developer.adobe.com/commerce/cloud-tools/docker/configure/custom-docker-compose/)。<!--MCLOUD-7089-->

   - 更新Docker主機設定以公開連線埠443，以便啟用從所有CLI容器存取Adobe Commerce (`https://magento2.docker`)。 產生Docker組態檔時，您可以新增`--tls-port`選項來變更預設連線埠。<!--MCLOUD-6806-->

- ![修正圖示](../../assets/fix.svg)修正在`app/etc/env.php`檔案存在時，導致Commerce組建的Cloud Docker失敗的問題。<!--MCLOUD-6732-->

- ![修正圖示](../../assets/fix.svg)已更新組建組態，以使用一般磁碟區取代具名磁碟區，以防止在Linux上部署Cloud Docker for Commerce或部署Windows Subsystem for Linux (WSL2)時發生問題。<!--MCLOUD-6732-->

- ![修正圖示](../../assets/fix.svg)已更新Commerce功能測試的Cloud Docker以支援Composer 2.0。<!--MCLOUD-7183-->

## v1.1.2

發行日期： 2020年9月9日

- ![新圖示](../../assets/new.svg)已新增對Elasticsearch 7.7<!--MCLOUD-6219-->的支援

## v1.1.1

發行日期： 2020年8月5日

- ![修正圖示](../../assets/fix.svg) **已更新電子郵件設定** — 已更新Commerce的預設雲端Docker設定，以支援MailHog服務，而不使用SendMail。 請參閱[設定電子郵件](https://developer.adobe.com/commerce/cloud-tools/docker/configure/#set-up-email).<!--MCLOUD-5624-->

- ![修正圖示](../../assets/fix.svg)已將PS資料庫還原到Cloud Docker環境設定以修正`ps:  command not found`個錯誤。<!--MCLOUD-6621-->

- ![修正圖示](../../assets/fix.svg)更新Commerce的預設Cloud Docker設定，移除自動掛載的資料庫入口點和MariaDB磁碟區，以修正啟動Cloud Docker環境時可能發生的`Cannot create container for service db`個錯誤。

  現在，您可以透過將以下選項新增到`ece-docker build:compose`命令來設定Cloud Docker環境以掛載資料庫目錄： `--with-entry-point`和`with-mariadb-conf`。 檢視[服務組態選項](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-6424-->

- ![新圖示](../../assets/new.svg) **CLI命令更新**

| 動作 | 命令 |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| 將入口點新增至資料庫容器，以便從備份還原資料庫 | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| 新增MariaDB設定磁碟區 | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

發行日期： 2020年6月25日

- ![新圖示](../../assets/new.svg) **新增對分割資料庫效能解決方案的支援** — 現在您可以在Cloud Docker環境中使用分割資料庫效能解決方案來設定和部署存放區。<!--MCLOUD-3740-->

- ![新圖示](../../assets/new.svg) **支援Adobe Commerce和Magento Open Source部署** — 現在您可以使用適用於Commerce的Cloud Docker，為雲端基礎結構上未託管在Adobe Commerce上的專案部署本機開發環境。<!--MCLOUD-5667-->

- ![新圖示](../../assets/new.svg) **Blackfire.io支援** — 已新增支援，以便使用[Blackfire.io擴充功能](https://developer.adobe.com/commerce/cloud-tools/docker/test/blackfire/)進行自動化效能測試。 由Adarsh Manickam從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![新圖示](../../assets/new.svg) **容器更新**

   - **Varnish** — 現在，當您使用支援的雲端應用程式範本版本，在Cloud Docker環境中部署Adobe Commerce時，Varnish是預設的快取。 檢視[光澤容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container).<!--MCLOUD-2634-->

   - 新增產生Cloud Docker設定檔時略過Varnish服務安裝的`--no-varnish`選項。<!--MCLOUD-2634-->

   - ![新圖示](../../assets/new.svg) **資料庫**

      - 新增對MySQL資料庫的支援。 現在，您可以使用MariaDB或MySQL來設定Cloud Docker環境。 檢視[服務組態選項](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-configuration-options).<!--MCLOUD-5691-->

      - 新增在產生Docker構成檔案時設定資料庫複製的增量與位移設定的功能。 檢視[服務容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/#service-containers).<!--MCLOUD-5735-->

   - ![新圖示](../../assets/new.svg) **PHP-FPM**

      - 新增對PHP 7.4的支援。[Mohanela Murugan從Zilker Technology提交的修正](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - 新增將根專案目錄中的`php.ini`檔案複製到Cloud Docker環境以及套用自訂PHP設定到PHP-FPM和CLI容器的功能。 請參閱[自訂PHP設定](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#customize-php-settings)。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - 新增容器健康狀態檢查。 [由Visanth Sampath從Zilker Technology提交的修正](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![修正圖示](../../assets/fix.svg) **Node.js** — 將預設Node.js版本從版本8更新至版本10以提高安全性。 Node.js版本8已過時，不再透過錯誤修正或安全性修補程式進行更新。 由Mohan Elamurugan從Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183)提交的[修正。<!--MCLOUD-5586-->

   - ![新圖示](../../assets/new.svg) **Elasticsearch**

      - 新增對Elasticsearch 6.8、7.2、7.5和7.6的支援。<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - 新增產生Docker構成設定檔案時自訂[Elasticsearch容器設定](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container)的功能。<!--MCLOUD-3059-->

      - 已將`--no-es`選項新增到服務組態選項中，用於產生Docker構成組態檔。 使用此選項可略過Elasticsearch容器安裝，並改用MySQL搜尋。 只有Adobe Commerce 2.3.5版和更舊版本才支援此選項。<!--MCLOUD-3766-->

   - ![新圖示](../../assets/new.svg) **FPM-XDEBUG容器** — 已新增服務組態選項，以便在雲端Docker環境中安裝及設定PHP除錯Xdebug。 請參閱[設定Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).<!--MCLOUD-4098-->

- ![新圖示](../../assets/new.svg) **Docker設定變更**

   - 已新增PHP-FPM、Redis、Elasticsearch和MySQL Docker服務容器的健康狀態檢查。<!--MCLOUD-3335 and MCLOUD-5856-->

   - 在開發人員模式下將預設檔案同步處理模式變更為`native`。<!--MCLOUD-3890 -->

   - 產生`docker-compose.yml`檔案時，已將版本資訊新增至一般Docker服務容器影像。<!--MCLOUD-3878-->

   - 透過增加Nginx伺服器的`fastcgi_buffers`值，改善處理來自上游PHP-FPM容器的大型回應的能力。<!--MCLOUD-5980-->

   - 透過新增第二個同步工作階段來同步`vendor`目錄中的檔案，改善突變檔案同步處理效能。 此變更可防止誘變在檔案同步程式期間卡住。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![新圖示](../../assets/new.svg) **CLI命令更新**

| 動作 | 命令 |
| -------- | --------------- |
| 清除Redis快取 | `bin/magento-docker flush-redis` |
| 清除清漆快取 | `bin/magento-docker flush-varnish` |
| 略過預設清漆安裝 | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [自訂Elasticsearch選項](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [移除Elasticsearch設定](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| 使用MySQL 5.6或5.7版設定資料庫容器 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| 指定自訂基底URL | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [新增Xdebug設定的容器](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![修正圖示](../../assets/fix.svg)修正了mutagen檔案同步處理的設定，以防止產生mutagen建立過時的工作階段。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![修正圖示](../../assets/fix.svg)修正啟動PHP-FPM容器時，造成Docker撰寫記錄中語法錯誤的設定問題。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![修正圖示](../../assets/fix.svg)修正使用多個Docker環境時有時發生的磁碟區衝突錯誤。 由G Arvind從Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168)提交的[修正。

- ![修正圖示](../../assets/fix.svg)修正組態包含Blackfire.io時，`ece-docker build:compose`命令失敗的問題。 [由G Arvind從Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199)提交的修正。<!--MCLOUD-5797-->

- ![修正圖示](../../assets/fix.svg)已更新PHP CLI影像設定，以防止使用Commerce適用的Cloud Docker安裝多個套件時發生記憶體不足錯誤。 由Mohan Elamurugan從Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197)提交的[修正。*<!--MCLOUD-5818-->

- ![修正圖示](../../assets/fix.svg)已在Cloud Docker環境中新增對多個MySQL使用者的支援。 在舊版中，如果`magento.app.yaml`檔案指定多個資料庫使用者，`build:compose`作業會失敗。 由G Arvind從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![修正圖示](../../assets/fix.svg)已從Commerce PHP容器的Cloud Docker中移除`rsyslog`，以解決在部署期間導致警告通知的相容性問題。 Cloud Docker不使用rsyslog公用程式。<!--MCLOUD-6173-->

## v1.0.0

發行日期：2020年2月5日

- ![新圖示](../../assets/new.svg) **已建立要傳遞`Cloud Docker for Commerce`**&#x200B;的個別套件 — 已將要傳遞Commerce Cloud Docker的原始程式碼從`ece-tools`存放庫移至[新`magento-cloud-docker`存放庫](https://github.com/magento/magento-cloud-docker)，以維持程式碼品質並提供獨立的版本。 新套件為ECE-Tools v2002.1.0和更新版本的相依性。

  當您更新ece-tools時，也會將`magento/magento-cloud-docker`套件更新至1.0.0版。如果您使用Commerce適用的Cloud Docker搭配舊版`ece-tools` (2002.0.x)，請檢閱[向後不相容性](backward-incompatible-changes.md)，並視需要以指令碼、命令和流程更新您的專案。

- ![新圖示](../../assets/new.svg) **已新增版本設定至Docker影像** — 您現在必須更新`magento/magento-cloud-docker`套件才能取得更新的影像。<!--MAGECLOUD-4737-->

- ![新圖示](../../assets/new.svg) **容器更新**—

   - ![新圖示](../../assets/new.svg) **PHP-FPM容器**—

      - ![新圖示](../../assets/new.svg) **新增Node.js支援** — 已更新PHP-FPM影像以支援PHP容器內的node、npm和grunt-cli功能。<!--MAGECLOUD-3953-->

      - ![新圖示](../../assets/new.svg) **已新增對[ionCube](https://www.ioncube.com/)**&#x200B;的支援 — 已更新預設Docker設定，以在本機Docker開發環境中支援ionCube。<!--MAGECLOUD-4354-->

   - ![新圖示](../../assets/new.svg) **Web容器**—

      - ![新圖示](../../assets/new.svg) **自訂NGINX設定** — 新增將自訂`nginx.conf`檔案掛載到Commerce環境適用的Cloud Docker的功能。 檢視[網頁容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#web-container).<!--MAGECLOUD-4204-->

      - ![新圖示](../../assets/new.svg) **自動產生的NGINX憑證**—Docker設定檔現在包含自動產生Web容器NGINX憑證的設定。<!--MAGECLOUD-4258-->

   - ![新圖示](../../assets/new.svg) **新Selenium容器** — 已新增[Selenium容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#selenium-container)，以支援使用Magento功能測試架構(MFTF)進行Adobe Commerce應用程式測試。<!--MAGECLOUD-4040-->

   - ![新圖示](../../assets/new.svg) **[!DNL RabbitMQ]版本支援** — 已更新[!DNL RabbitMQ]容器組態以支援[!DNL RabbitMQ] 3.8版本。<!--MAGECLOUD-4674-->

   - ![修復圖示](../../assets/fix.svg) **永久資料庫容器** — 在您停止並移除Docker組態並在重新啟動Docker組態時復原後，`magento-db: /var/lib/mysql`資料庫磁碟區現在會持續存在。 現在，您必須手動刪除資料庫磁碟區。 請參閱[資料庫容器].<!--MAGECLOUD-3978-->

   - ![新圖示](../../assets/new.svg) **TLS容器**—

      - ![新圖示](../../assets/new.svg) **已更新容器基礎影像以使用正式影像**— [雲端TLS容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#tls-container)影像現在是以正式的`debian:jessie` Docker影像為基礎。—<!--MAGECLOUD-4163-->

      - ![新圖示](../../assets/new.svg) **已新增對[Pound TLS Termination Proxy]**&#x200B;的支援 — [Pound組態檔](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/)已新增下列ENV變數，以自訂TLS容器的Docker組態：

         - **`TimeOut`** — 設定第一位元組時間(TTFB)逾時值。 預設值為300秒。

         - **`RewriteLocation`** — 決定Pound Proxy是否預設將位置重寫至要求URL。 預設為`0`，以防止重寫中斷重新導向至外部網站，例如外部SSO網站。 由Sorin Sugar提交的[修正](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![新圖示](../../assets/new.svg)將TLS容器設定中的逾時值從15秒增加到300秒。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![新圖示](../../assets/new.svg) **塗漆容器**—

      - ![新圖示](../../assets/new.svg) **已更新容器基礎影像以使用正式影像**— [雲端上光容器](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#varnish-container)現在是以正式的`centos` Docker影像為基礎。<!--MAGECLOUD-4163-->

      - ![新圖示](../../assets/new.svg) **已改善預設逾時設定** — 已將`.first_byte_timeout`和`.between_bytes_timeout`設定新增至Varnish容器。 這兩個逾時值預設為`300s` （5分鐘）。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![修正圖示](../../assets/fix.svg) **在Xdebug工作階段期間略過清漆** — 已更新Varnish容器設定，以便在啟用Xdebug時收到要求時傳回`pass`。 在舊版中，如果Docker環境包含Varnish，則無法使用Xdebug。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![新圖示](../../assets/new.svg) **Docker設定變更**—

   - ![新圖示](../../assets/new.svg) **管理專案的掛載和磁碟區** — 新增在啟動Docker環境以進行本機開發時管理掛載和磁碟區的功能。 請參閱[共用專案資料].<!--MAGECLOUD-3248-->

   - ![新圖示](../../assets/new.svg) **網路橋接器模式的支援** — 新增網路橋接器模式的支援，以透過本機網路啟用Docker容器之間的連線。<!--MAGECLOUD-4165-->

   - ![新圖示](../../assets/new.svg) **預設為停用Cron容器** — 為了改善效能，當您建置Docker環境時，不再預設設定Cron容器。 您可以在Docker構建命令上使用`--with-cron`選項將Cron容器新增到您的環境中。 請參閱[管理cron工作](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/).<!--MAGECLOUD-5181-->

   - ![新圖示](../../assets/new.svg) **停止同步處理大型備份檔案** — 已將資料庫傾印和封存檔案（ZIP、SQL、GZ和BZ2）新增至`dist/docker-sync.yml`和`dist/mutagen.sh`檔案的排除清單。 同步處理大型檔案(>1 GB)可能會造成一段閒置時間，而且備份檔案通常不需要同步處理，因為您可以重新產生它們。<!--MAGECLOUD-3979-->

- ![新圖示](../../assets/new.svg) **命令變更**—

   - ![修正圖示](../../assets/fix.svg)將`./bin/docker`檔案重新命名為`./bin/magento-docker`以修正由於`./bin/docker`檔案覆寫現有的Docker二進位檔案而導致部分Docker環境中斷的問題。 這是[向後不相容的變更](backward-incompatible-changes.md)，需要更新您的指令碼和命令。<!-- MAGECLOUD-4038 -->

   - ![新圖示](../../assets/new.svg) **已新增服務組態選項，以將資料庫連線埠公開給主機** — 在建置`docker-compose.yml`檔案時，請使用`--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>`選項將資料庫連線埠公開給主機： `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![新圖示](../../assets/new.svg) **新的部署後命令** — 之前，在您使用`cloud-deploy`命令將Adobe Commerce部署到Cloud Docker容器後，`.magento.app.yaml`檔案中定義的部署後掛接會自動執行。 現在，您必須發出單獨的`cloud-post-deploy`命令，才能在您部署後執行部署後掛接。 檢視[開發人員](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/developer-mode/)和[生產](https://developer.adobe.com/commerce/cloud-tools/docker/deploy/production-mode/)模式的更新啟動指示。<!--MAGECLOUD-3996-->

   - ![新圖示](../../assets/new.svg)已將`--rm`選項新增至組建和部署容器的`./bin/magento-docker`命令。 這會在工作完成後移除容器。<!--MAGECLOUD-4205-->

   - ![新圖示](../../assets/new.svg) **`build:compose`命令的更新**—

      - ![新圖示](../../assets/new.svg)已將`--sync-engine="native"`選項新增到`docker-build`命令，以在您以開發人員模式產生Docker撰寫設定檔案時停用檔案同步。 在Linux系統上開發時，使用此選項，這些系統不需要本機Docker開發的檔案同步。 請參閱[在Docker環境中同步處理資料](https://developer.adobe.com/commerce/cloud-tools/docker/setup/synchronize-data/).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![新圖示](../../assets/new.svg)已將預設檔案同步處理設定從`docker-sync`變更為`native`。 由Mathew Beane從Zilker Technology提交的[修正](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![新圖示](../../assets/new.svg) **驗證改善**—

   - ![新圖示](../../assets/new.svg)已新增本機Docker開發環境的部署程式驗證，以驗證雲端環境設定是否包含解密資料庫所需的加密金鑰。 現在，如果環境設定未指定加密金鑰的值，記錄中會顯示錯誤訊息。<!--MAGECLOUD-4423-->

   - ![新圖示](../../assets/new.svg)已新增容器健康狀態檢查至Elasticsearch服務，以確保服務準備就緒，再繼續建置和部署處理。 如果健康情況檢查傳回錯誤，容器會自動重新啟動。<!--MAGECLOUD-4456-->
