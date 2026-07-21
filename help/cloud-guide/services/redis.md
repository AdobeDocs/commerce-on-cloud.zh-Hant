---
title: 設定Redis服務
description: 瞭解如何在雲端基礎結構上為Adobe Commerce設定及最佳化Redis做為後端快取解決方案。
feature: Cloud, Cache, Services
exl-id: be6f2462-0878-47e3-b906-ebdd4aa319f2
TQID: https://experienceleague.adobe.com/Q3w1Y1sRuQSwqmbxGfEBavrvHe0ecI9qWJjsfVc2yPU
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 9e1fd3699623816ea3368816820daf799a43284f
workflow-type: tm+mt
source-wordcount: 388
ht-degree: 0%

---

# 設定Redis服務

[Redis](https://redis.io)是選用的後端快取解決方案，可取代Adobe Commerce預設使用的`Zend Framework Zend_Cache_Backend_File`。

>[!IMPORTANT]
>
>Adobe Commerce 2.4.9或更新於2.4.5-p16、2.4.6-p14、2.4.7-p9和2.4.8-p4的修補程式版本不支援Redis快取。 對於不支援Redis的快取設定，請使用Valkey。 如需依版本支援的快取服務，請參閱[系統需求](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/installation-guide/system-requirements)。

{{service-instruction}}

**若要啟用Redis**：

1. 將必要的名稱和型別新增到`.magento/services.yaml`檔案。

   ```yaml
   myredis:
       type: redis:<version>
   ```

   若要提供您自己的Redis設定，請在`.magento/services.yaml`檔案中新增`core_config`金鑰：

   ```yaml
   cache:
       type: redis:<version>
   ```

1. 設定`.magento.app.yaml`檔案中的關聯性。

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 自訂Redis設定

如需自訂Redis設定的詳細資訊，請參閱&#x200B;_實作Playbook最佳實務指南_&#x200B;中的[設定Redis](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration)。

## 使用Redis CLI

假設您的Redis關係名稱為`redis`，您可以使用`redis-cli`工具來存取它。

1. 使用SSH連線至已安裝並設定Redis的整合環境。

1. 開啟連線至主機的SSH通道。

   ```bash
   redis-cli -h redis.internal
   ```

## 取得已安裝的Redis版本

使用下列命令取得Redis版本安裝在整合環境中：

```bash
redis-cli -h redis.internal info | grep version
```

範例回應：

```
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis on Pro測試和生產

若要在測試或生產環境中安裝Redis版本，請使用`redis-server`命令：

```bash
redis-server -v
```

```
Redis server v=7.0.5 ...
```

使用下列命令取得Redis組態安裝在Pro Staging或生產環境中：

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

範例回應：

```json
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## 疑難排解Redis

請參閱下列Adobe Commerce支援文章，以取得疑難排解Redis問題的說明：

- [Redis問題延遲Admin登入或結帳](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [延伸的Redis快取實作Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html?lang=zh-Hant)
- [Adobe Commerce上的受管理警報： Redis記憶體警告警報](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html?lang=zh-Hant)
- [Adobe Commerce上的受管理警報：Redis記憶體嚴重警報](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html?lang=zh-Hant)
