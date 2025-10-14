---
title: 設定ActiveMQ服務
description: 瞭解如何啟用ActiveMQ Artemis服務，以管理雲端基礎結構上Adobe Commerce的訊息佇列。
feature: Cloud, Services
source-git-commit: ef22de6873b49f0fb9adfa9fc343a8d738a543e9
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# 設定[!DNL ActiveMQ]服務

[Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)是Adobe Commerce中的系統，可讓[模組](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)將訊息發佈至佇列。 它也會定義非同步接收訊息的消費者。

MQF可以使用[ActiveMQ Artemis](https://activemq.apache.org/components/artemis/)做為傳訊代理人，提供可擴充的平台來傳送及接收訊息。 它也包括儲存未傳遞訊息的機制。 [!DNL ActiveMQ Artemis]支援訊息的STOMP （串流文字導向訊息通訊協定）通訊協定。

[!DNL ActiveMQ Artemis]可用作RabbitMQ管理訊息佇列的替代方案。 當您需要ActiveMQ的特定功能或想要使用STOMP通訊協定時，這個功能特別有用。

>[!IMPORTANT]
>
>如果您偏好使用現有的訊息代理程式服務（例如[!DNL ActiveMQ]），而不仰賴Adobe Commerce在雲端基礎結構上為您建立它，請使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境變數將其連線到您的網站。

{{service-instruction}}

**若要啟用ActiveMQ Artemis**：

1. 將所需的名稱、型別和磁碟值（以MB為單位）連同已安裝的ActiveMQ版本新增至`.magento/services.yaml`檔案。

   ```yaml
   activemq-artemis:
       type: activemq-artemis:<version>
       disk: 1024
   ```

1. 設定`.magento.app.yaml`檔案中的關聯性。

   ```yaml
   relationships:
       activemq-artemis: "activemq-artemis:activemq-artemis"
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable ActiveMQ Artemis service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 連線到ActiveMQ以進行偵錯

針對偵錯目的，您可以透過下列其中一種方式直接連線至服務執行個體：

- 從您的本機開發環境連線
- 從應用程式連線
- 從您的PHP應用程式連線

### 從您的本機開發環境連線

1. 登入`magento-cloud` CLI與專案：

   ```bash
   magento-cloud login
   ```

1. 檢視已安裝並設定ActiveMQ Artemis的環境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 使用SSH連線至雲端環境：

   ```bash
   magento-cloud ssh
   ```

1. 從[$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)變數擷取ActiveMQ連線詳細資料和登入認證：

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在回應中，尋找ActiveMQ資訊。 關聯性名稱取決於您在`.magento.app.yaml`檔案中的設定方式。 例如，如果您使用`activemq-artemis`作為關聯性名稱：

   ```json
   {
      "activemq-artemis" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "stomp",
            "port" : 61616,
            "host" : "activemq-artemis.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. 啟用本機連線埠轉送至ActiveMQ （如果您的專案位於不同的區域，例如US-3、EU-5或AP-3區域，請將``us-3``替換為``eu-5``/``ap-3``/``us``）

   ```bash
   ssh -L <port-number>:activemq-artemis.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   存取位於`http://localhost:8161`的ActiveMQ Artemis Web主控台的範例是：

   ```bash
   ssh -L 8161:activemq-artemis.internal:8161 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   >[!NOTE]
   >
   >ActiveMQ Artemis使用連線埠61616進行STOMP傳訊，並使用連線埠8161進行Web主控台。

1. 在工作階段開啟時，您可以使用MAGENTO_CLOUD_RELATIONSHIPS變數中的使用者名稱和密碼來存取位於`http://localhost:8161`的ActiveMQ Artemis網頁主控台。

### 從應用程式連線

若要連線到應用程式中執行的ActiveMQ，請在PHP應用程式中安裝STOMP使用者端程式庫。

### 從您的PHP應用程式連線

若要使用PHP應用程式連線到ActiveMQ，請將PHP STOMP程式庫新增到來源樹狀結構中。 Adobe Commerce會使用STOMP通訊協定進行ActiveMQ通訊，而且當ActiveMQ Artemis偵測到已設定的服務時，會在部署期間自動設定設定。

## 通訊協定支援

雲端基礎結構上Adobe Commerce中的ActiveMQ Artemis使用STOMP （串流文字導向傳訊通訊協定）通訊協定：

- **STOMP**：用於佇列作業的傳訊通訊協定(連線埠61616)
- **網頁主控台**：可透過HTTP存取的管理介面（連線埠8161）

## 與RabbitMQ的差異

雖然[!DNL ActiveMQ Artemis]和[!DNL RabbitMQ]都可作為Adobe Commerce的訊息代理人，但有一些差異：

- **通訊協定**： ActiveMQ Artemis使用STOMP通訊協定，而RabbitMQ使用AMQP
- **設定**：設定ActiveMQ Artemis時，Adobe Commerce會自動使用STOMP通訊協定
- **優先順序**：如果已設定ActiveMQ和RabbitMQ，則ActiveMQ優先於STOMP作業，而AMQP作業則使用RabbitMQ
- **網頁主控台**： ActiveMQ提供網頁式管理主控台，用於監視佇列和訊息

## 佇列設定

當ActiveMQ Artemis設定為服務時，Adobe Commerce會自動設定佇列系統以使用STOMP通訊協定。 組態會在部署期間寫入`app/etc/env.php`檔案：

```php
'queue' => [
    'stomp' => [
        'host' => 'activemq-artemis.internal',
        'port' => '61616',
        'user' => 'guest',
        'password' => 'guest'
    ],
    'default_connection' => 'stomp'
]
```

您可以視需要使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境變數覆寫此設定。

