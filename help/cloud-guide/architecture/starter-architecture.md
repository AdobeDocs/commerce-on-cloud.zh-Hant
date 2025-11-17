---
title: 入門架構
description: 瞭解入門架構支援的環境。
feature: Cloud, Paas
exl-id: 2f16cc60-b5f7-4331-b80e-43042a3f9b8f
source-git-commit: 2236d0b853e2f2b8d1bafcbefaa7c23ebd5d26b3
workflow-type: tm+mt
source-wordcount: '1017'
ht-degree: 0%

---

# 入門架構

雲端基礎結構上的Adobe Commerce入門架構支援最多&#x200B;**四個**&#x200B;環境，包括包含初始專案程式碼的`master`環境、中繼環境和最多兩個整合環境。

所有環境都在PaaS (Platform as a service)容器中。 這些容器部署在伺服器格線上的高度受限容器內。 這些環境是唯讀的，會接受從您本機工作區推送的分支所部署的程式碼變更。 每個環境都提供資料庫和Web伺服器。

>[!NOTE]
>
>無法變更任何入門環境中唯讀資料夾的許可權。 此限制可保護應用程式的完整性和安全性。 無法變更這些唯讀檔案系統上的檔案夾許可權 — 即使「支援」無法修改這些許可權。 任何變更都必須從您本機開發環境的分支進行，並推送至應用程式環境。 您可以使用任何您喜歡的開發和分支方法。 當您取得專案的初始存取權時，請從`staging`環境建立`master`環境。 然後，從`integration`分支以建立`staging`環境。

## 入門環境架構

下圖顯示入門者環境的階層式關係。

![入門專案的高階檢視](../../assets/starter/architecture.png)

## 生產環境

生產環境提供原始程式碼，可將Adobe Commerce部署至執行公開單一和多網站商店的雲端基礎結構。 生產環境使用來自`master`分支的程式碼來設定及啟用網頁伺服器、資料庫、設定的服務以及您的應用程式程式碼。

因為`production`環境是唯讀的，請使用`integration`環境進行程式碼變更，跨架構從`integration`部署到`staging`，最後部署到`production`環境。 請參閱[部署您的存放區](../deploy/staging-production.md)和[網站啟動](../launch/overview.md)。

Adobe建議在推送至`staging`分支（部署至`master`環境）之前，先在您的`production`分支中進行完整測試。

## 中繼環境

Adobe建議從`staging`建立名為`master`的分支。 `staging`分支將程式碼部署到中繼環境，以提供預先生產環境來測試程式碼、模組和擴充功能、付款閘道、運送、產品資料等。 此環境提供所有服務的設定以符合生產環境，包括Fastly、New Relic APM和搜尋。

本指南的其他章節提供在安全的預備環境中進行最終程式碼部署和測試生產層級互動的說明。 為進行最佳效能和功能測試，請將資料庫復寫至測試環境。

>[!WARNING]
>
>Adobe建議先在中繼環境中測試每個商家和客戶的互動，然後再部署到生產環境。 請參閱[部署您的存放區](../deploy/staging-production.md)和[測試部署](../test/staging-and-production.md)。

## 整合環境

開發人員使用`integration`環境來開發、部署和測試：

- Adobe Commerce應用程式程式碼

- 自訂程式碼

- 擴充功能

- 服務

**建議的使用案例：**

整合環境是專為有限的測試和開發所設計。 例如，您可以使用整合環境來完成下列工作：

- 確保對持續整合(CI)流程的變更與雲端相容

- 在關鍵頁面上測試關鍵工作流程，例如首頁、類別、產品詳細資料頁面(PDP)、結帳和管理員

若要在整合環境中取得最佳效能，請遵循下列最佳實務：

- 限制目錄大小 — 作為參考，範例資料包含約2,048種產品。 請嘗試將目錄大小縮減至約4,000至5,000種產品。
若要檢查目錄中的產品數目，請執行下列MySQL查詢：

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- 減少客戶群組數量 — 擁有過多客戶群組可能會影響索引效能和整體效能。

- 限制使用一或兩名同時使用者

- 停用cron工作並根據需要手動執行

您最多可以有&#x200B;**兩個**&#x200B;使用中的整合環境。 您可以從`staging`分支建立分支，以建立整合環境。 當您建立整合環境時，環境名稱會與分支名稱相符。 整合環境包含網頁伺服器和資料庫。 其中並未包含所有服務，例如無法使用Fastly CDN和New Relic。

您可以有不限數量的非使用中分支用於程式碼儲存。 若要存取、檢視和測試非使用中分支，您必須將其啟用

{{enhanced-integration-envs}}

## 生產和測試技術棧疊

生產和測試環境包含下列技術。 您可以透過[`.magento.app.yaml`](../application/configure-app-yaml.md)檔案修改及設定這些技術。

- Fastly用於HTTP快取和CDN
- 與PHP-FPM對話的Nginx網頁伺服器，一個執行個體具有多個背景工作
- Redis伺服器
- 適用於Adobe Commerce 2.2至2.4.3-p2目錄搜尋的Elasticsearch
- Adobe Commerce 2.3.7-p3、2.4.3-p2及2.4.4和更新版本的目錄搜尋OpenSearch
- 輸出篩選（輸出防火牆）

### 服務

雲端基礎結構上的Adobe Commerce目前支援下列服務：PHP、MySQL (MariaDB)、Elasticsearch (Adobe Commerce 2.2到2.4.3-p2)、OpenSearch （2.3.7-p3、2.4.3-p2、2.4.4和更新版本）、Redis和[!DNL RabbitMQ]。

每個服務會在個別、安全的容器中執行。 容器在專案中一起管理。 有些是標準服務，例如：

- HTTP路由器（處理傳入要求，以及快取和重新導向）

- PHP應用程式伺服器

- Git

- 安全殼層(SSH)

### 軟體版本

雲端基礎結構上的Adobe Commerce使用Debian GNU/Linux作業系統和NGINX網頁伺服器。 您無法升級此軟體，但可以設定下列版本：

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

在測試和生產環境中，您會將Fastly用於CDN和快取。 最新版本的Fastly CDN擴充功能會在專案的初始布建期間安裝。 您可以升級擴充功能以取得最新的錯誤修正和改善專案。 檢視Magento 2[的](https://github.com/fastly/fastly-magento2)Fastly CDN模組。 此外，您還有權存取[New Relic](../monitor/account-management.md)以進行效能監視。

使用下列檔案來設定您要在實施中使用的軟體版本。

- [&#39;.magento.app.yaml&#39;](../application/configure-app-yaml.md)

- [&#39;routes.yaml&#39;](../routes/routes-yaml.md)

- [&#39;services.yaml&#39;](../services/services-yaml.md)

### 備份與災難回覆

您可以使用[!DNL Cloud Console]或CLI建立資料庫和檔案系統的備份。 請參閱[備份管理](../storage/snapshots.md)。

## 準備開發

以下工作流程總結了分支您的程式碼、開發及部署存放區的程式：

1. 設定您的本機環境

1. 將`master`分支複製至您的本機環境

1. 從`staging`建立`master`分支

1. 從`staging`建立開發分支

1. 將程式碼推送到Git，以便建置和部署至環境進行測試

請參閱下列章節，以取得開發、測試和部署存放區的詳細指示和逐步說明：

- [入門開發與部署工作流程](starter-develop-deploy-workflow.md)

- [Docker開發](../dev-tools/cloud-docker.md) (Cloud Docker for Commerce啟用的本機開發環境)

- [管理分支](../project/console-branches.md)

- [部署您的存放區](../deploy/staging-production.md)

- [網站啟動](../launch/overview.md)
