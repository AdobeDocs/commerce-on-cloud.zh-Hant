---
title: 設定RabbitMQ服務
description: 瞭解如何啟用RabbitMQ服務，以管理雲端基礎結構上Adobe Commerce的訊息佇列。
feature: Cloud, Services
exl-id: 64af1dfa-e3f0-4404-a352-659ca47c1121
source-git-commit: 76a9721767cbd4328347311cc308810f0f7914c0
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 設定[!DNL RabbitMQ]服務

[Message Queue Framework (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html)是Adobe Commerce中的系統，可讓[模組](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary#module)將訊息發佈至佇列。 它也會定義非同步接收訊息的消費者。

MQF使用[RabbitMQ](https://www.rabbitmq.com/)作為傳訊代理人，提供可擴充的平台來傳送及接收訊息。 它也包括儲存未傳遞訊息的機制。 [!DNL RabbitMQ]是以進階訊息佇列通訊協定(AMQP) 0.9.1規格為基礎。

>[!NOTE]
>
>雲端基礎結構上的Adobe Commerce也支援[ActiveMQ Artemis](activemq.md)作為使用STOMP通訊協定的替代訊息佇列服務。

>[!IMPORTANT]
>
>如果您偏好使用現有的AMQP型服務（例如[!DNL RabbitMQ]），而不仰賴Adobe Commerce的雲端基礎結構為您建立它，請使用[`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration)環境變數將其連線到您的網站。

{{service-instruction}}

**若要啟用RabbitMQ**：

1. 將所需的名稱、型別和磁碟值（以MB為單位）連同安裝的RabbitMQ版本新增至`.magento/services.yaml`檔案。

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. 設定`.magento.app.yaml`檔案中的關聯性。

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 連線至RabbitMQ以進行偵錯

為了進行偵錯，建議您以下列其中一種方式直接連線至服務執行個體：

- 從您的本機開發環境連線
- 從應用程式連線
- 從您的PHP應用程式連線

### 從您的本機開發環境連線

1. 登入`magento-cloud` CLI與專案：

   ```bash
   magento-cloud login
   ```

1. 檢視已安裝並設定RabbitMQ的環境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 使用SSH連線至雲端環境：

   ```bash
   magento-cloud ssh
   ```

1. 從[$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)變數擷取RabbitMQ連線詳細資料和登入認證：

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在回應中，尋找RabbitMQ資訊，例如：

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. 啟用本機連線埠轉送至RabbitMQ （如果您的專案位於不同的區域，例如US-3、EU-5或AP-3區域，請將``us-3``替換為``eu-5``/``ap-3``/``us``）

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   在`http://localhost:15672`存取RabbitMQ管理Web介面的範例為：

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. 在工作階段開啟時，您可以從本機工作站啟動您選擇的RabbitMQ使用者端，該工作站已設定為使用MAGENTO_CLOUD_RELATIONSHIP變數的連線埠號碼、使用者名稱和密碼資訊連線至`localhost:<portnumber>`。

### 從應用程式連線

若要連線到應用程式中執行的RabbitMQ，請在您的[檔案中安裝使用者端（例如](https://github.com/dougbarth/amqp-utils)amqp-utils`.magento.app.yaml`）作為專案相依性。

例如，

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

當您登入PHP容器時，您輸入任何`amqp-`命令可用於管理您的佇列。

### 從您的PHP應用程式連線

若要使用PHP應用程式連線至RabbitMQ，請將PHP程式庫新增至來源樹狀結構。

## 疑難排解[!DNL RabbitMQ]服務

請參閱[無法連線到Adobe Commerce Cloud中的RabbitMQ](https://experienceleague.adobe.com/en/docs/experience-cloud-kcs/kbarticles/ka-27688)。

## 正在升級[!DNL RabbitMQ]服務

如需升級指示，請參閱[變更服務版本](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/service/services-yaml#change-service-version)。
