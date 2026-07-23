---
title: 設定Valkey服務
description: 瞭解如何在Cloud Infrastructure上為Adobe Commerce設定及最佳化Valkey作為後端快取解決方案。
feature: Cloud, Cache, Services
exl-id: f8933e0d-a308-4c75-8547-cb26ab6df947
TQID: https://experienceleague.adobe.com/-aBnwClJGQlRkEfugtChxbjLObLzTu0xl1IvkYUVRsk
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: a12aa37d41a68df2fd4c84b589a08e128e5ec103
workflow-type: tm+mt
source-wordcount: 286
ht-degree: 0%

---

# 設定Valkey服務

[Valkey](https://valkey.io)是選用的後端快取解決方案，可取代Adobe Commerce預設使用的`Zend Framework Zend_Cache_Backend_File`。 如果您覆寫Commerce 2.4.9+版或2.4.5-p16、2.4.6-p14、2.4.7-p9以上修補程式發行版本的預設值，以及2.4.8-p4版本行，則必須使用Valkey。

請參閱&#x200B;_實作行動手冊最佳實務指南_&#x200B;中的[設定Valkey](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-valkey-service-configuration){target="_blank"}。

{{service-instruction}}

**若要以Valkey取代Redis，請更新下列三個檔案中的組態**：

1. 以必要的Valkey名稱取代Redis設定，並在`.magento/services.yaml`檔案中輸入。

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

1. 設定`.magento.env.yaml`以取代Redis設定，如下所示：

   ```yaml
    stage:
        deploy:
        VALKEY_USE_SLAVE_CONNECTION: true
        VALKEY_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
   ```

   >[!TIP]
   >
   >若是Adobe Commerce 2.4.9和更新版本，您可以設定`VALKEY_BACKEND: symfony_l2`，改用現代的Symfony快取型L2快取實作，而非`RemoteSynchronizedCache`。 請參閱&#x200B;_部署變數_&#x200B;參考中的[`VALKEY_BACKEND`](../environment/variables-deploy.md#valkey_backend)。

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml .magento.env.yaml && git commit -m "Enable valkey service" && git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

{{valkey-newrelic}}

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
