---
title: 設定Valkey服務
description: 瞭解如何在雲端基礎結構上設定和最佳化Valkey做為Adobe Commerce的後端快取解決方案，包括取代Redis和自訂快取後端設定。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
source-git-commit: d5d947f9858ab15e2e5daed7848163846580f883
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# 設定Valkey服務

[Valkey](https://valkey.io)是雲端基礎結構上適用於Adobe Commerce的選用後端快取解決方案。 當您覆寫Adobe Commerce 2.4.9及更高版本上的預設快取設定，或是2.4.5-p16、2.4.6-p14、2.4.7-p9和2.4.8-p4之前的修補程式發行版本上的預設快取設定時，需要Valkey。

{{service-instruction}}

## 設定Valkey

若要使用Valkey取代Redis，請更新下列檔案：

- `.magento/services.yaml`
- `.magento.app.yaml`

### 設定服務

在`.magento/services.yaml`中，以Valkey服務定義取代Redis服務定義。 以您的Adobe Commerce版本和目前的雲端範本支援的Valkey版本取代`<version>`。

```yaml
cache:
  type: valkey:<version>
```

**範例**

```yaml
cache:
  type: valkey:8.0
```

範例版本並非通用。 實際的預設和支援服務版本取決於您的Adobe Commerce版本和目前的雲端範本。 使用目前專案範本指定的版本。 如需詳細資訊，請參閱[設定服務](services-yaml.md#service-versions)。

>[!WARNING]
>
>如果您變更服務ID，則會移除現有服務並建立新服務。 已移除服務中的現有資料會永久刪除。 在重新命名服務之前，請先備份環境。

將`type`值從`redis:<version>`變更為`valkey:<version>`時，即使您保留相同的服務ID，請勿假設快取和工作階段資料持續存在。 將移轉視為建立新的快取：無法保證會保留現有的快取和工作階段資料，而且使用者會在移轉完成後登出。

### 設定服務關係

在`.magento.app.yaml`中，設定應用程式與Valkey服務之間的關係：

```yaml
relationships:
  valkey: "cache:valkey"
```

關聯性索引鍵`valkey`是應用程式用來存取服務的名稱。 值`cache:valkey`參考了`.magento/services.yaml`中定義的服務ID和服務型別。

>[!TIP]
>
>Adobe Commerce透過`credis`使用者端程式庫與Valkey通訊，預設會透過普通PHP通訊端運作。 若要改善效能，請在`.magento.app.yaml`中啟用`redis` PHP延伸。 `credis`在可用的情況下會自動使用已編譯的副檔名。
>
>```yaml
>runtime:
>   extensions:
>       - redis
>```

### 認可並部署變更

新增、認可及推送設定變更：

```terminal
git add .magento/services.yaml .magento.app.yaml
git commit -m "Enable Valkey service"
git push origin <branch-name>
```

部署完成後，請確認Valkey服務關係可供使用。

{{service-change-tip}}

{{valkey-newrelic}}

## 自訂Valkey設定

如需快取、工作階段、L2和復本連線建議，請參閱&#x200B;_實作Playbook最佳作法指南_&#x200B;中的[Valkey和Redis服務組態的最佳作法](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

## 驗證服務關係

若要顯示已解碼的`MAGENTO_CLOUD_RELATIONSHIPS`物件，請在部署組態之後，從應用程式容器執行下列命令：

使用SSH連線到遠端雲端環境，然後執行：

```terminal
echo "$MAGENTO_CLOUD_RELATIONSHIPS" | base64 -d | json_pp
```

命令會顯示所有已設定的服務關係。 若要識別Valkey連線詳細資料，請找到valkey關係。

**範例輸出**

下列縮寫範例顯示`valkey`關係。 這不是通用結構描述。

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
   "valkey" : [
      {
         "host" : "valkey.internal",
         "port" : 6379,
         "path" : null,
         "scheme" : "valkey"
      }
   ]
}
```

輸出會因環境和服務設定而異。 請勿在此範例中硬式編碼主機名稱、連線埠、IP位址、叢集名稱、服務版本、使用者名稱或密碼。 在目標環境中使用`MAGENTO_CLOUD_RELATIONSHIPS`傳回的值。

如果`jq`可用，則僅顯示Valkey關係：

```terminal
printf '%s' "$MAGENTO_CLOUD_RELATIONSHIPS" \
  | base64 -d \
  | jq '{valkey: .valkey}'
```

如需有關服務關係的詳細資訊，請參閱[設定服務](services-yaml.md)。

## 使用Valkey CLI

假設您的Valkey關係名稱為`valkey`，請使用`MAGENTO_CLOUD_RELATIONSHIPS`傳回的主機與連線埠連線到Valkey：

```terminal
valkey-cli -h <host> -p <port>
```

**範例**

```terminal
valkey-cli -h valkey.internal -p 6379
```

## 取得已安裝的Valkey版本

>[!BEGINTABS]

>[!TAB 整合環境]

在整合環境中，使用`valkey`關聯性傳回的主機與連線埠來執行：

```terminal
valkey-cli -h <host> -p <port> info | grep version
```

**範例回應**

```text
valkey_version:<installed-version>
gcc_version:<gcc-version>
```

版本和組建詳細資料會因環境而異。 請勿將顯示的範例版本視為必要或通用服務版本。

>[!TAB Pro測試和生產]

在Pro測試和生產環境中，執行：

```terminal
valkey-server -v
```

**範例回應**

```text
Valkey server v=<installed-version> ...
```

版本和組建詳細資料會因環境而異。 請勿將顯示的範例版本視為必要或通用服務版本。

>[!ENDTABS]

## 疑難排解Valkey

### 快取清除錯誤參考Valkey設定的快取上的Redis

預先部署快取清除失敗會顯示錯誤代碼`[107]` (`clean-redis-cache`)和`Connection to Redis`訊息，即使`cache`服務設定為Valkey亦然。 無論後援快取服務是Redis或Valkey，`ece-tools`都會將此錯誤碼和訊息用於快取清除步驟。

如果基礎錯誤是DNS失敗（例如關聯性主機的`Name or service not known`），則部署步驟會在服務關聯性可用之前執行，或`.magento.app.yaml`中的關聯性名稱與`.magento/services.yaml`中的服務識別碼不符。 請參閱[驗證服務關係](#verify-the-service-relationship)。
