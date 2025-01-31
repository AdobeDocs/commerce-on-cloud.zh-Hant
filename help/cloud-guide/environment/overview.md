---
title: 組態檔概述
description: 瞭解如何設定雲端基礎結構環境，以支援部署和管理您的自訂Adobe Commerce存放區。
feature: Cloud, Configuration, Services, Iaas, Paas
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# 組態檔概述

雲端基礎結構上的Adobe Commerce中的環境包括包含應用程式、服務和資料庫的容器，可為您的Adobe Commerce應用程式程式碼基底和檔案提供完整系統。

您可以使用以下組態檔來設定應用程式設定、路由、建置和部署動作以及通知，以支援您的專案環境：

| 設定 | 檔案名稱 | 說明 |
| ------------- | -------- | ----------- |
| [應用程式](../application/configure-app-yaml.md) | `.magento.app.yaml` | 定義如何建置和部署Adobe Commerce，包括服務、鉤點和cron工作。 |
| [環境](configure-env-yaml.md) | `.magento.env.yaml` | 使用環境變數，集中管理所有環境的建置和部署動作，包括Pro測試和生產。 |
| [路由](../routes/routes-yaml.md) | `.magento/routes.yaml` | 設定快取、重新導向以及伺服器端包含。 |
| [服務](../services/services-yaml.md) | `.magento/services.yaml` | 依名稱和版本定義Adobe Commerce使用的服務。 例如，此檔案可能包含MariaDB、PHP副檔名、Redis、RabbitMQ以及Elasticsearch或OpenSearch的版本。 您必須開啟支援票證，將這些變更推送到Pro計畫測試和生產環境。 |
| [PHP設定](../application/php-settings.md#configure-php) | `php.ini` | 可新增至專案的選用檔案。 此檔案中包含的設定會附加至雲端基礎結構所維護的設定。 |

{style="table-layout:auto"}

## Pro環境的設定更新

對於雲端基礎結構專業測試和生產環境上的Adobe Commerce，您可以更新本機開發環境中的許多設定選項，並提交變更以將其套用至這些環境。 不過，您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以更新下列組態選項：

- 在`.magento/services.yaml`檔案中安裝或更新服務。
- 變更`.magento.app.yaml`檔案中`mounts`和`disk`屬性的組態。

{{pro-self-service-warning}}
