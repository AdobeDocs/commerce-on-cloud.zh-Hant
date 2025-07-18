---
title: 部署變數
description: 檢視在雲端基礎結構部署階段的Adobe Commerce中控制動作的環境變數清單。
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 980ec809-8c68-450a-9db5-29c5674daa16
source-git-commit: 3f2a4f7dc9c23afb3af80304023d9e742c974ccd
workflow-type: tm+mt
source-wordcount: '2483'
ht-degree: 0%

---

# 部署變數

下列&#x200B;_部署_&#x200B;變數控制項動作位於部署階段，可以繼承及覆寫來自[全域變數](variables-global.md)的值。 在`deploy`檔案的`.magento.env.yaml`階段中插入這些變數：

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

如需自訂建置和部署程式的詳細資訊：

- [部署設定](configure-env-yaml.md)
- [部署流程](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

設定Redis頁面和預設快取。 設定`cm_cache_backend_redis`引數時，您必須指定`server`、`port`和`database`選項。

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

下列範例使用[設定指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature)中定義的&#x200B;_Redis預先載入功能_：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

若要使用自訂[REDIS_BACKEND](#redis_backend)模型（不僅來自允許清單），請將`_custom_redis_backend`選項設定為`true`以啟用正確的驗證，如下列範例所示：

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **預設**—`true`
- **版本**—Adobe Commerce 2.1.4和更新版本

啟用或停用清除在建置或部署階段產生的[靜態內容檔案](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)。 在開發中使用預設值&#x200B;_true_&#x200B;作為最佳實務。

- **`true`** — 在部署更新的靜態內容之前，移除所有現有的靜態內容。
- **`false`** — 如果產生的內容包含較新的版本，部署只會覆寫現有的靜態內容檔案。

如果您透過個別程式修改靜態內容，請將值設為&#x200B;_false_。

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

如果在部署之前未清除靜態檢視檔案，則在不移除先前版本的情況下將更新部署到現有檔案時，可能會導致問題。 由於[靜態檔案遞補](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache)規則，如果目錄包含相同檔案的多個版本，則遞補作業可能會顯示錯誤的檔案。

## `CRON_CONSUMERS_RUNNER`

- **預設**—`cron_run = false`，`max_messages = 1000`
- **版本**—Adobe Commerce 2.2.0和更新版本

使用此環境變數來確認訊息佇列在部署後是否執行。

- `cron_run` — 啟用或停用`consumers_runner` cron工作（預設= `false`）的布林值。
- `max_messages` — 指定每個消費者在終止前必須處理的最大訊息數（預設值= `1000`）的數字。 您可以將值設為`0`以防止消費者終止。
- `consumers` — 字串陣列，指定要執行的使用者。 空陣列執行&#x200B;_所有_&#x200B;消費者。

- `multiple_processes` — 指定每個取用者要衍生的處理序數目。 在Commerce **2.4.4**&#x200B;或更新版本中支援。

>[!NOTE]
>
>若要傳回訊息佇列`consumers`的清單，請在遠端環境中執行`./bin/magento queue:consumers:list`命令。

執行特定`consumers`且要為每個取用者衍生`multiple_processes`的陣列範例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

執行全部`consumers`的空白陣列範例：

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

依預設，部署程式會覆寫`env.php`檔案中的所有設定。 請參閱內部部署Adobe Commerce的[Commerce設定指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html)中的&#x200B;_管理訊息佇列_。

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **預設**—`false`
- **版本**—Adobe Commerce 2.2.0和更新版本

選擇下列其中一個選項，設定`consumers`處理訊息佇列訊息的方式：

- `false`—`Consumers`處理佇列中的可用訊息，關閉TCP連線，然後終止。 即使已處理的訊息數小於`Consumers`部署變數中指定的`max_messages`值，`CRON_CONSUMERS_RUNNER`也不要等候其他訊息進入佇列。

- `true`—`Consumers`繼續處理來自訊息佇列的訊息，直到達到`max_messages`部署變數中指定的訊息數目上限(`CRON_CONSUMERS_RUNNER`)為止，然後再關閉TCP連線並終止消費者處理序。 如果佇列在到達`max_messages`之前排空，消費者會等待更多訊息到達。

>[!WARNING]
>
>如果您使用背景工作來執行`consumers`，而不是使用cron工作，請將此變數設為true。

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

>[!WARNING]
>
>透過`CRYPT_KEY`而非[!DNL Cloud Console]檔案設定`.magento.env.yaml`值，以避免公開您環境的原始程式碼存放庫中的金鑰。 請參閱[設定環境和專案變數](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/project/overview.html#configure-environment)。

將資料庫從一個環境移動到另一個環境時，若沒有安裝程式，則需要相應的密碼編譯資訊。 Adobe Commerce使用[!DNL Cloud Console]中設定的加密金鑰值做為`crypt/key`檔案中的`env.php`值。

## `DATABASE_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

如果您在[檔案的](../application/properties.md#relationships)關聯屬性`.magento.app.yaml`中定義資料庫，您可以自訂資料庫連線以進行部署。

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

您也可以設定表格首碼。

>[!WARNING]
>
>如果您未將合併選項與表格首碼搭配使用，則必須提供預設連線設定，否則部署會驗證失敗。

下列範例使用具有預設連線設定的`ece_`表格首碼，而非使用`_merge`選項：

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

範例輸出：

```
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.2.0和更新版本

在部署之間保留自訂的[!DNL Elastic Suite]服務設定，並在主要[!DNL Elastic Suite]設定的&#39;system/default/smile_elasticsuite_core_base_settings&#39;區段中使用它。 如果已安裝[!DNL Elastic Suite] Composer套件，則會自動設定它。

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

>[!NOTE]
>
>在[縮放架構](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/scaled-architecture#service-tier)上具有三個節點（或三個服務節點）的Pro測試/生產叢集上，`indices_settings`應設定如下：
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**已知限制**：

- 將搜尋引擎變更為`elasticsuite`以外的任何型別會導致部署失敗，並伴隨適當的驗證錯誤
- 移除Elasticsearch服務會導致部署失敗，並伴隨適當的驗證錯誤

>[!NOTE]
>
>如需搭配Adobe Commerce使用或疑難排解[!DNL Elastic Suite]外掛程式的詳細資訊，請參閱[[!DNL Elastic Suite] 檔案](https://github.com/Smile-SA/elasticsuite)。

## `ENABLE_GOOGLE_ANALYTICS`

- **預設**—`false`
- **版本**—Adobe Commerce 2.1.4和更新版本

在部署至中繼和整合環境時，啟用或停用Google Analytics。 依預設，Google Analytics僅適用於生產環境。 將此值設定為`true`以在測試與整合環境中啟用Google Analytics。

- **`true`** — 在測試與整合環境中啟用Google Analytics。
- **`false`** — 在測試與整合環境中停用Google Analytics。

將`ENABLE_GOOGLE_ANALYTICS`環境變數新增至`deploy`檔案中的`.magento.env.yaml`階段：

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>部署程式一律會在生產環境中啟用Google Analytics。

## `FORCE_UPDATE_URLS`

- **預設**—`true`
- **版本**—Adobe Commerce 2.1.4和更新版本

在部署至Pro或Starter Staging and Production環境時，此變數會以[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)變數指定的專案URL取代資料庫中的Adobe Commerce基底URL。 使用此設定覆寫[UPDATE_URLS](#update_urls)部署變數的預設行為，在部署到中繼或生產環境時會忽略此預設行為。

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **預設**—`file`
- **版本**—Adobe Commerce 2.2.5和更新版本

鎖定提供者可防止啟動重複的cron作業和cron群組。 在生產環境中使用`file`鎖定提供者。 入門環境和Pro整合環境不使用[MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md)變數，因此`ece-tools`會自動套用`db`鎖定提供者。

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

請參閱[安裝指南](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html)中的&#x200B;_設定鎖定_。

## `MYSQL_USE_SLAVE_CONNECTION`

- **預設**—`false`
- **版本**—Adobe Commerce 2.1.4和更新版本

>[!TIP]
>
>`MYSQL_USE_SLAVE_CONNECTION`變數僅在雲端基礎結構測試和生產Pro叢集環境的Adobe Commerce上受支援，且在入門專案上不受支援。

Adobe Commerce可以非同步讀取多個資料庫。 設定為`true`可自動使用資料庫的&#x200B;_唯讀_&#x200B;連線，以接收非主節點上的唯讀流量。 此連線透過負載平衡來改善效能，因為只有一個節點處理讀寫流量。 設定為`false`以從`env.php`檔案中移除任何現有的唯讀連線陣列。

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

當`MYSQL_USE_SLAVE_CONNECTION`變數設為`true`時，在Pro測試和生產環境的`synchronous_replication`檔案中，`true`引數預設為`env.php`。 當`MYSQL_USE_SLAVE_CONNECTION`設定為`false`時，未設定`synchronous_replication`引數。

## `QUEUE_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

使用此環境變數可保留部署間的自訂AMQP服務設定。 例如，如果您偏好使用現有的訊息佇列服務，而不仰賴雲端基礎結構為您建立它，請使用`QUEUE_CONFIGURATION`環境變數將其連線到您的網站：

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **預設**—`Cm_Cache_Backend_Redis`
- **版本**—Adobe Commerce 2.3.0和更新版本

指定Redis快取的後端模型設定。

Adobe Commerce 2.3.0版和更新版本包含下列後端模型：

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

如何設定`REDIS_BACKEND`的範例

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>如果您指定`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`作為Redis後端模型以啟用[L2快取](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html)，`ece-tools`會自動產生快取組態。 請參閱[Adobe Commerce組態指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example)中的&#x200B;_組態檔_&#x200B;範例。 若要覆寫產生的快取組態，請使用[CACHE_CONFIGURATION](#cache_configuration)部署變數。

## `REDIS_USE_SLAVE_CONNECTION`

- **預設**—`false`
- **版本**—Adobe Commerce 2.1.16和更新版本

>[!WARNING]
>
>請&#x200B;_不_&#x200B;在[縮放的架構](../architecture/scaled-architecture.md)專案上啟用此變數。 這會導致Redis連線錯誤。 Redis從屬仍然有效，但不會用於Redis讀取。 作為替代方法，Adobe建議使用Adobe Commerce 2.3.5或更新版本、實作新的Redis後端設定，以及實作Redis的L2快取。

>[!TIP]
>
>`REDIS_USE_SLAVE_CONNECTION`變數僅在雲端基礎結構測試和生產Pro叢集環境的Adobe Commerce上受支援，且在入門專案上不受支援。

Adobe Commerce可以非同步方式讀取多個Redis執行個體。 設定為`true`可自動使用與Redis執行個體的&#x200B;_唯讀_&#x200B;連線，以接收非主節點上的唯讀流量。 此連線透過負載平衡來改善效能，因為只有一個節點處理讀寫流量。 設定為`false`以從`env.php`檔案中移除任何現有的唯讀連線陣列。

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

您必須在`.magento.app.yaml`檔案和`services.yaml`檔案中設定Redis服務。

[ECE-Tools 2002.0.18](../release-notes/cloud-release-archive.md#v2002018)版和更新版本使用更多容錯設定。 如果Adobe Commerce無法從Redis _slave_&#x200B;執行個體讀取資料，則會從Redis _master_&#x200B;執行個體讀取資料。

唯讀連線無法用於整合環境，或您使用[`CACHE_CONFIGURATION`變數](#cache_configuration)。

## `VALKEY_BACKEND`

- **預設**—`Cm_Cache_Backend_Redis`
- **版本**—Adobe Commerce 2.8.0和更新版本

`VALKEY_BACKEND`指定Valkey快取的後端模型組態。

Adobe Commerce 2.8.0版和更新版本包含下列後端模型：

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

下列範例說明如何設定`VALKEY_BACKEND`：

```yaml
stage:
  deploy:
  VALKEY_USE_SLAVE_CONNECTION: true
  VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>如果您指定`\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`作為Valkey後端模型以啟用[L2快取](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html)，`ece-tools`會自動產生快取組態。 請參閱[Adobe Commerce組態指南](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example)中的&#x200B;_組態檔_&#x200B;範例。 若要覆寫產生的快取組態，請使用[CACHE_CONFIGURATION](#cache_configuration)部署變數。

## `VALKEY_USE_SLAVE_CONNECTION`

- **預設**—`false`
- **版本**—Adobe Commerce 2.4.8和更新版本

>[!WARNING]
>
>請&#x200B;_不_&#x200B;在[縮放的架構](../architecture/scaled-architecture.md)專案上啟用此變數。 這會導致Valkey連線錯誤。 Redis從屬仍然有效，但不會用於Redis讀取。 或者，Adobe建議使用Adobe Commerce 2.4.8或更新版本，實作新的Valkey後端設定，並實作Valkey的L2快取。

>[!TIP]
>
>`VALKEY_USE_SLAVE_CONNECTION`變數僅在雲端基礎結構測試和生產Pro叢集環境的Adobe Commerce上受支援，且在入門專案上不受支援。

Adobe Commerce可以非同步方式讀取多個Redis執行個體。 `VALKEY_USE_SLAVE_CONNECTION`設定為`true`可自動使用與Redis執行個體的&#x200B;_唯讀_&#x200B;連線，以接收非主節點上的唯讀流量。 此連線透過負載平衡來改善效能，因為只有一個節點處理讀寫流量。 將`VALKEY_USE_SLAVE_CONNECTION`設為`false`以從`env.php`檔案中移除任何現有的唯讀連線陣列。

```yaml
stage:
  deploy:
    VALKEY_USE_SLAVE_CONNECTION: true
```

您必須在`.magento.app.yaml`檔案和`services.yaml`檔案中設定Redis服務。

[ECE-Tools 2002.0.18](../release-notes/cloud-release-archive.md#v2002018)版和更新版本使用更多容錯設定。 如果Adobe Commerce無法從Valkey _slave_&#x200B;執行個體讀取資料，則會從Redis _master_&#x200B;執行個體讀取資料。

唯讀連線無法用於整合環境，或您使用[`CACHE_CONFIGURATION`變數](#cache_configuration)。

## `RESOURCE_CONFIGURATION`

- **預設** — 未設定
- **版本**—Adobe Commerce 2.1.4和更新版本

將資源名稱對應到資料庫連線。 此設定對應至`resource`檔案的`env.php`區段。

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **預設**—`4`
- **版本**—Adobe Commerce 2.1.4和更新版本

指定壓縮靜態內容時要使用的[gzip](https://www.gnu.org/software/gzip)壓縮等級（`0`到`9`）；`0`會停用壓縮。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **預設**—`600`
- **版本**—Adobe Commerce 2.1.4和更新版本

當壓縮靜態資產所花的時間超過壓縮逾時限制時，就會中斷部署程式。 設定靜態內容壓縮命令的執行時間上限（秒）。

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

您可以為每個主題設定多個地區設定。 此自訂功能可減少不必要的佈景主題檔案數目，進而加快部署程式。 例如，您可以部署英文版的&#x200B;_magento/backend_&#x200B;佈景主題，以及其他語言版的自訂佈景主題。

下列範例會部署`Magento/backend`佈景主題與三種地區設定：

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

此外，您可以選擇&#x200B;_不_&#x200B;部署佈景主題：

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.2.0和更新版本

可讓您增加靜態內容部署的最大預期執行時間。

依預設，Adobe Commerce會將最大預期執行時間設為900秒，但在某些情況下，您可能需要更多時間才能完成雲端專案的靜態內容部署。

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **預設**—`false`
- **版本**—Adobe Commerce 2.4.2和更新版本

在部署階段中，設定`SCD_NO_PARENT: true`，以便在部署階段中不會產生父系主題的靜態內容。 此設定將部署時間縮到最短，並防止在部署期間靜態內容建置失敗時可能發生的網站停機時間。 請參閱[靜態內容部署](../deploy/static-content.md)。

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **預設**—`quick`
- **版本**—Adobe Commerce 2.2.0和更新版本

可讓您自訂靜態內容的[部署策略](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html)。 請參閱[部署靜態檢視檔案](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html)。

如果您有多個地區設定，請只使用這些選項&#x200B;_1}：_

- `standard` — 為所有封裝部署所有靜態檢視檔案。
- `quick` — （_預設_）可縮短部署時間。
- `compact` — 節省伺服器上的磁碟空間。 在Adobe Commerce 2.2.4版和更早版本中，此設定會以`scd_threads`的值覆寫`1`的值。

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **預設** — 自動
- **版本**—Adobe Commerce 2.1.4和更新版本

設定靜態內容部署的執行緒數目。 預設值是根據偵測到的CPU執行緒計數而設定，不會超過4的值。 增加執行緒數目會加速靜態內容部署；減少執行緒數目會減慢速度。 您可以設定螺紋值，例如：

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

若要進一步縮短部署時間，請使用[組態管理](../store/store-settings.md)搭配`scd-dump`命令，將靜態部署移至建置階段。

## `SEARCH_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

使用此環境變數，可保留部署間的自訂搜尋服務設定。 例如：

Elasticsearch設定：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

OpenSearch設定(適用於Commerce 2.4.6和更新版本)：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

設定Redis工作階段存放區。 需要工作階段存放區變數的`save`、`redis`、`host`、`port`和`database`選項。 例如：

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

以下範例會將新值合併至現有設定：

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **預設**— _未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

設定為`true`可在部署階段期間略過靜態內容部署。

在部署階段中，設定`SKIP_SCD: true`，以便在部署階段中不會發生靜態內容建置。 此設定將部署時間縮到最短，並防止在部署期間靜態內容建置失敗時可能發生的網站停機時間。 請參閱[靜態內容部署](../deploy/static-content.md)。

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **預設**—`true`
- **版本**—Adobe Commerce 2.1.4和更新版本

部署時，將資料庫中的Adobe Commerce基底URL取代為[`MAGENTO_CLOUD_ROUTES`](variables-cloud.md)變數指定的專案URL。 此設定對於本機開發非常有用，因為在本機環境中會設定基本URL。 當您部署至雲端環境時，URL會更新，以便您可以使用專案URL存取店面和管理員。

如果您在部署至Pro或Starter測試和生產環境時必須更新URL，請使用[`FORCE_UPDATE_URLS`](#force_update_urls)變數。

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **預設**—_未設定_
- **版本**—Adobe Commerce 2.1.4和更新版本

啟用或停用部署階段所執行[ CLI命令的](https://symfony.com/doc/current/console/verbosity.html)Symfony`bin/magento`偵錯詳細程度。

>[!NOTE]
>
>若要使用VERBOSE_COMMANDS設定來控制命令輸出中成功和失敗的`bin/magento` CLI命令的詳細資料，您必須設定[MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`。

選擇記錄中提供的詳細資訊等級：

- `-v`=一般輸出
- `-vv`=其他詳細資訊輸出
- `-vvv` =詳細輸出，適用於偵錯

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
