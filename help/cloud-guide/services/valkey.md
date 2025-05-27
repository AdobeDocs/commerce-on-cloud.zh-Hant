---
title: 設定Valkey服務
description: 瞭解如何在Cloud Infrastructure上為Adobe Commerce設定及最佳化Valkey作為後端快取解決方案。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
source-git-commit: 242582ea61d0d93725a7f43f2ca834db9e1a7c29
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 設定Valkey服務

[Valkey](https://valkey.io)是選用的後端快取解決方案，可取代Adobe Commerce預設使用的`Zend Framework Zend_Cache_Backend_File`。

請參閱&#x200B;_設定指南_&#x200B;中的[設定Valkey](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/valkey/config-valkey.html){target="_blank"}。

{{service-instruction}}

**啟用Valkey**：

1. 將必要的名稱和型別新增到`.magento/services.yaml`檔案。

   ```yaml
   cache:
       type: valkey:<version>
   ```

   若要提供您自己的Valkey設定，請在`.magento/services.yaml`檔案中新增`core_config`金鑰：

   ```yaml
   cache:
       type: valkey:8.0
   ```

1. 設定`.magento.app.yaml`檔案中的關聯性。

   ```yaml
   relationships:
       valkey: "cache:valkey"
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 使用Valkey CLI

假設您的Valkey關聯性名稱為`valkey`，您可以使用`valkey-cli`工具加以存取。

1. 使用SSH連線至已安裝並設定Valkey的整合環境。

1. 開啟連線至主機的SSH通道。

   ```bash
   valkey-cli -h valkey.internal
   ```

## 取得已安裝的Valkey版本

使用以下命令取得安裝在整合環境上的Valkey版本：

```bash
valkey-cli -h valkey.internal info | grep version
```

回應：

```
valkey_version:8.0.1
gcc_version:12.2.0
```

### Pro測試與生產環境上的Valkey

若要在測試或生產環境中安裝Valkey版本，請使用`valkey-server`命令：

```bash
valkey-server -v
```

```bash
Valkey server v=8.0.1 ...
```

使用以下命令，取得Pro測試或生產環境中安裝的Valkey組態：

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

回應：

```json
"valkey" : [
    {
        "cluster" : "project-master-abc0003",
        "epoch" : 0,
        "fragment" : null,
        "host" : "valkeycache.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.cache.service._.magentosite.cloud",
        "instance_ips" : [
        "123.456.789.012"
        ],
        "ip" : "123.456.789.012",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "valkey",
        "scheme" : "valkey",
        "service" : "cache",
        "type" : "valkey:8.0",
        "username" : null
    }
]
```
