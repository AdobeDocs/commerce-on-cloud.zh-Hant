---
title: 設定服務
description: 瞭解如何在雲端基礎結構上設定Adobe Commerce使用的服務。
feature: Cloud, Configuration, Services
exl-id: ddf44b7c-e4ae-48f0-97a9-a219e6012492
source-git-commit: 322f7af2c79dd4eeeabafa2ba7e5a32cbd8b1925
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 0%

---

# 設定服務

`services.yaml`檔案定義Adobe Commerce在雲端基礎結構上支援及使用的服務，例如MySQL、Redis和Elasticsearch或OpenSearch。 您不需要訂閱外部服務提供者。

>[!NOTE]
>
>`.magento/services.yaml`檔案是在您專案的`.magento`目錄中本機管理的。 在建置過程中可存取設定，以便僅在整合環境中定義所需的服務版本，部署完成後便會移除設定，導致您在伺服器上找不到這些設定。

部署指令碼使用`.magento`目錄中的組態檔，以設定的服務布建環境。 如果服務包含在[`relationships`](../application/properties.md#relationships)檔案的`.magento.app.yaml`屬性中，您的應用程式便可使用它。 `services.yaml`檔案包含&#x200B;_型別_&#x200B;和&#x200B;_磁碟_&#x200B;值。 服務型別定義服務&#x200B;_名稱_&#x200B;和&#x200B;_版本_。

變更服務設定會讓部署在環境中布建更新的服務，進而影響下列環境：

- 所有入門環境，包括生產`master`
- Pro整合環境

{{pro-update-service}}

## 預設與支援的服務

雲端基礎結構支援和部署以下服務：

- [ActiveMQ](activemq.md)
- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

>[!NOTE]
>
>升級至新版RabbitMQ後，請觸發完整部署，以確保在RabbitMQ中重新建立自訂訊息佇列。

您可以在目前的[預設`services.yaml`檔案](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml)中檢視預設版本和磁碟值。 下列範例顯示`mysql`組態檔中定義的`redis`、`opensearch`、`elasticsearch`或`rabbitmq`、`activemq-artemis`及`services.yaml`服務：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024

activemq-artemis:
    type: activemq-artemis:2.42
    disk: 1024
```

## 服務值

您必須提供服務識別碼和服務型別組態`type: <name>:<version>`。 如果服務使用永久儲存體，則必須提供磁碟值。

使用以下格式：

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

`service-id`值會識別專案中的服務。 您只能使用小寫字母數字字元： `a`到`z`和`0`到`9`，例如`redis`。

此&#x200B;_service-id_&#x200B;值用於[`relationships`](../application/properties.md#relationships)組態檔的`.magento.app.yaml`屬性：

```yaml
relationships:
    redis: "<name>:redis"
```

您可以為每種服務型別的多個執行個體命名。 例如，您可以使用多個Redis例項，一個用於作業階段，一個用於快取。

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

重新命名`services.yaml`檔案&#x200B;**中的服務將會永久移除**&#x200B;下列專案：

- 使用您指定的新名稱建立服務之前的現有服務。
- 服務的所有現有資料都會被移除。 Adobe強烈建議您[先備份您的入門環境](../storage/snapshots.md)，然後再變更現有服務的名稱。

### `type`

`type`值指定服務名稱和版本。 例如：

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

`disk`值指定要配置給服務的永久磁碟儲存大小（以MB為單位）。 使用永久儲存體的服務（例如MySQL）必須提供磁碟值。 使用記憶體而非永久儲存體的服務（例如Redis）不需要磁碟值。

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

目前每個專案的預設儲存容量為5 GB，即512 0MB。 您可以在應用程式及其每項服務之間分配此金額。

## 服務關係

在雲端基礎結構專案的Adobe Commerce中，在[檔案中設定的服務](../application/properties.md#relationships)關係`.magento.app.yaml`會決定哪些服務可供您的應用程式使用。

您可以從[`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md)環境變數擷取所有服務關係的設定資料。 設定資料包括服務名稱、型別和版本，以及任何必要的連線詳細資訊，例如連線埠號碼和登入認證。

**驗證本機環境中的關係**：

1. 在您的本機環境中，顯示使用中環境的關係。

   ```bash
   magento-cloud relationships
   ```

1. 確認回應中的`service`和`type`。 回應會提供連線資訊，例如IP位址和連線埠號碼。

   >縮寫的範例回應

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**若要驗證遠端環境中的關係**：

1. 使用SSH登入遠端環境。

1. 列出在環境中設定的所有服務的關係設定資料。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或者，使用下列`ece-tools`命令檢視關係：

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. 確認回應中的`service`和`type`。 回應會提供連線資訊，例如IP位址和連線埠號碼，以及任何必要的使用者名稱和密碼認證。

## 服務版本

雲端基礎結構上Adobe Commerce的服務版本和相容性支援取決於雲端基礎結構上部署和測試的版本，有時與Adobe Commerce內部部署支援的版本不同。 請參閱[安裝](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)指南中的&#x200B;_系統需求_，以取得Adobe已針對特定Adobe Commerce和Magento Open Source版本測試的協力廠商軟體相依性清單。

### 軟體EOL檢查

在部署程式期間，`ece-tools`套件會根據每個服務的生命週期結束(EOL)日期檢查已安裝的服務版本。

- 如果服務版本在EOL日期後的三個月內，部署記錄中會顯示通知。
- 如果EOL日期是過去，則會顯示警告通知。

為維護商店安全性，請在已安裝軟體版本到達EOL之前更新這些版本。 您可以在[ece-tools&#39; `eol.yaml`檔案](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml)中檢閱EOL日期。

### 移轉至OpenSearch

{{elasticsearch-support}}

若為Adobe Commerce 2.4.4版或更新版本，請參閱[設定OpenSearch服務](opensearch.md)。

## 變更服務版本

您可以升級已安裝的服務版本，使其與雲端環境中部署的Adobe Commerce版本相容。

您無法直接將已安裝服務的服務版本降級。 不過，您可以建立具有所需版本的服務。 請參閱[降級服務版本](#downgrade-version)。

### 升級已安裝的服務版本

您可以更新`services.yaml`檔案中的服務組態來升級已安裝的服務版本。

1. 變更[`type`](#type)檔案中服務的`.magento/services.yaml`值：

   > 原始服務定義

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > 已更新服務定義

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### 降級版本

您無法直接降級已安裝的服務。 您有兩個選項：

1. 以新版本重新命名現有服務，移除現有服務和資料，並新增新服務和資料。

1. 建立服務並儲存現有服務的資料。

當您變更服務版本時，您必須更新`services.yaml`檔案中的服務組態，並更新`.magento.app.yaml`檔案中的關聯性。

**若要藉由重新命名現有服務來降級服務版本**：

1. 重新命名`.magento/services.yaml`檔案中的現有服務並變更版本。

   >[!WARNING]
   >
   >重新命名現有服務會取代現有服務並刪除所有資料。 如果您需要保留資料，請建立服務，而非重新命名現有服務。

   例如，若要將&#x200B;_mysql_&#x200B;服務的MariaDB版本從10.4版降級為10.3版，請變更現有的&#x200B;_service-id_&#x200B;和&#x200B;_type_&#x200B;組態。

   > 原始`services.yaml`定義

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > 新`services.yaml`定義

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. 更新`.magento.app.yaml`檔案中的關聯性。

   > 原始`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 已更新`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 新增、提交和推送您的程式碼變更。

**若要藉由建立服務來降級服務**：

1. 將服務定義新增至具有降級版本規格的專案的`services.yaml`檔案。 請參閱下列範例中的&#x200B;_mysql2_：

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. 變更`.magento.app.yaml`檔案中的關係設定以使用新服務。

   > 原始`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > 新`.magento.app.yaml`設定

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. 新增、提交和推送您的程式碼變更。
