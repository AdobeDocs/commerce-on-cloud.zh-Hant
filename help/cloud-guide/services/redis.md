---
title: 設定Redis服務
description: 瞭解如何在雲端基礎結構上為Adobe Commerce設定及最佳化Redis做為後端快取解決方案。
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: df2792f9d653c4561e4e40cbc71499095f63ff71
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 0%

---

# 設定Redis服務

[Redis](https://redis.io)是選用的後端快取解決方案，可取代Adobe Commerce預設使用的`Zend Framework Zend_Cache_Backend_File`。

>[!IMPORTANT]
>
>Adobe Commerce 2.4.9或更新於2.4.5-p16、2.4.6-p14、2.4.7-p9和2.4.8-p4的修補程式版本不支援Redis快取。 在不支援Redis的快取設定中使用[Valkey](valkey.md)。 如需依版本支援的快取服務，請參閱[系統需求](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)。

{{service-instruction}}

## 啟用Redis

若要啟用Redis，請更新下列檔案：

- `.magento/services.yaml`
- `.magento.app.yaml`

### 設定服務

在`.magento/services.yaml`中，新增Redis服務定義。 以您的Adobe Commerce版本和目前的雲端範本支援的Redis版本取代`<version>`。

```yaml
cache:
  type: redis:<version>
```

例如，對於支援Redis 7.2的Commerce版本和雲端範本：

```yaml
cache:
  type: redis:7.2
```

範例版本並非通用。 實際的預設和支援服務版本取決於您的Adobe Commerce版本、修補程式層級和目前的雲端範本。 驗證[系統需求](https://experienceleague.adobe.com/en/docs/commerce-operations/installation-guide/system-requirements)中支援的組合以及目前的專案範本。

### 設定服務關係

在`.magento.app.yaml`中，設定應用程式與Redis服務之間的關係：

```yaml
runtime:
  extensions:
    - redis

relationships:
  redis: "cache:redis"
```

關聯性索引鍵`redis`是應用程式用來存取服務的名稱。 值`cache:redis`包含在`.magento/services.yaml`中定義的服務識別碼(`cache`)和服務型別(`redis`)。

### 認可並部署變更

新增、認可及推送設定變更：

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Redis service"
git push origin <branch-name>
```

部署完成後，請確認Redis服務關係可用。

{{service-change-tip}}

## 驗證服務關係

部署組態之後，從應用程式容器執行下列命令，以顯示已解碼的`MAGENTO_CLOUD_RELATIONSHIPS`物件：

使用SSH連線到遠端雲端環境，然後執行：

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

命令會顯示所有已設定的服務關係。 找出`redis`關聯以識別Redis連線詳細資料。

下列縮寫範例顯示`redis`關係。 這不是通用結構描述。

```json
{
   "database" : [
      {
         "host" : "database.internal",
         "port" : 3306,
         "path" : "main",
         "scheme" : "mysql"
      }
   ],
   "opensearch" : [
      {
         "host" : "opensearch.internal",
         "port" : 9200,
         "path" : null,
         "scheme" : "http"
      }
   ],
   "redis" : [
      {
         "host" : "redis.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "redis"
      }
   ]
}
```

輸出會因環境和服務設定而異。 請勿在此範例中硬式編碼主機名稱、連線埠、IP位址、叢集名稱、服務版本、使用者名稱或密碼。 在目標環境中使用`MAGENTO_CLOUD_RELATIONSHIPS`傳回的值。

如果`jq`可用，請使用以下命令以僅顯示Redis關係：

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{redis: .redis}'
```

如需有關服務關係的詳細資訊，請參閱[設定服務](services-yaml.md)。

## 自訂Redis設定

如需快取、工作階段、L2和復本連線建議，請參閱&#x200B;_實作Playbook最佳作法指南_&#x200B;中的[Valkey和Redis服務組態的最佳作法](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

## 使用Redis CLI

假設您的Redis關係名稱為`redis`，請使用`MAGENTO_CLOUD_RELATIONSHIPS`傳回的主機與連線埠連線到Redis。

在安裝及設定Redis的情況下連線到環境，然後執行以下命令：

```terminal
redis-cli -h <host> -p <port>
```

**範例**

```terminal
redis-cli -h redis.internal -p 6379
```

## 取得已安裝的Redis版本

>[!BEGINTABS]

>[!TAB 整合環境]

在整合環境中，使用`redis`關聯性傳回的主機與連線埠來執行：

```terminal
redis-cli -h <host> -p <port> info | grep version
```

**範例回應**

```text
redis_version:<installed-version>
gcc_version:<gcc-version>
```

版本和組建詳細資料會因環境而異。 請勿將顯示的範例版本視為必要或通用服務版本。

>[!TAB Pro測試和生產]

在Pro測試和生產環境中，執行：

```terminal
redis-server -v
```

**範例回應**

```text
Redis server v=<installed-version> ...
```

版本和組建詳細資料會因環境而異。 請勿將顯示的範例版本視為必要或通用服務版本。

>[!ENDTABS]

## 疑難排解Redis

請參閱下列Adobe Commerce支援文章，以取得疑難排解Redis問題的說明：

- [Adobe Commerce上的受管理警報： Redis記憶體警告警報](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-warning-alert)
- [Adobe Commerce上的受管理警報：Redis記憶體嚴重警報](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/managed-alerts-for-adobe-commerce/managed-alerts-on-magento-commerce-redis-memory-critical-alert)

### 快取清除錯誤參考Valkey設定的快取上的Redis

預先部署快取清除失敗會顯示錯誤代碼`[107]` (`clean-redis-cache`)和`Connection to Redis`訊息，即使`cache`服務設定為Valkey亦然。 `ece-tools`將這個舊版Redis導向的錯誤碼和訊息用於快取清除步驟，無論哪個服務支援`cache`關係，因此措辭不會表示已安裝Redis。

如果基礎錯誤是DNS失敗（例如關聯性主機的`Name or service not known`），則部署步驟會在服務關聯性可用之前執行，或`.magento.app.yaml`中的關聯性名稱與`.magento/services.yaml`中的服務識別碼不符。 請參閱[驗證服務關係](#verify-the-service-relationship)。
