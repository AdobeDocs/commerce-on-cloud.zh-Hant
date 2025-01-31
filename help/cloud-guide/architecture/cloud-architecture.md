---
title: Commerce的雲端架構
description: 瞭解雲端基礎結構上Commerce的Starter和Pro專案架構對比情形。
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Commerce的雲端架構

雲端基礎結構上的Adobe Commerce有入門和Pro計畫。 每個計畫都有獨特的架構，可推動您的Adobe Commerce開發和部署流程。 Starter plan和Pro plan架構都會部署資料庫、Web伺服器和快取伺服器，跨多個環境進行端對端測試，同時支援持續整合。

為了進行比較，每個計畫都包含下列基礎架構功能及支援的產品。

|          | 入門者 | Pro |
| -------- | --------------------| ------------------ |
| 核心功能 | <ul><li>[所有Adobe Commerce功能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal入門工具</li><li>[Commerce報告](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[所有Adobe Commerce功能](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>PayPal入門工具</li><li>[Commerce報告](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[B2B模組](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| 基礎結構和部署 | <ul><li>持續雲端整合工具，使用者不限</li><li>Fastly Content Delivery Network (CDN)、影像最佳化(IO)，以及寬頻寬裕量的額外安全性。 Web應用程式防火牆(WAF)服務僅適用於生產環境。</li><li>3個分支上的[New Relic](../monitor/new-relic-service.md) APM （效能監視）：您選擇的`master`和2<br>Platform as a Service (PaaS)生產、測試和開發環境（共4個使用中環境），已針對Adobe Commerce最佳化</li><li>輸出篩選（輸出防火牆）</li></ul> | <ul><li>持續雲端整合工具，使用者不限</li><li>Fastly Content Delivery Network (CDN)、影像最佳化(IO)，以及寬頻寬裕量的額外安全性。 Web應用程式防火牆(WAF)服務僅適用於生產環境。</li><li>生產環境上的[New Relic](../monitor/new-relic-service.md)基礎結構+中繼和生產環境上的APM （效能監視）。 Adobe Commerce原則的[受管理警示原則](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)實作監視最佳實務，以主動通知您影響網站效能的應用程式和基礎結構問題。</li><li>以Platform as a Service (PaaS)為基礎，針對Adobe Commerce最佳化的[整合開發](pro-architecture.md#integration-environment)環境（共2個使用中環境）</li><li>基礎架構即服務(IaaS)：專為中繼和生產環境提供的虛擬基礎架構</li></ul> |
| 高可用性基礎建設 | | [高可用性架構](pro-architecture.md#redundant-hardware)在基礎基礎基礎建設即服務(IaaS)中設定三部伺服器，以提供企業級可靠性和可用性 |
| 專屬硬體 | | 基礎基礎基礎建設服務(IaaS)中的獨立專用硬體，可提供更高等級的可靠性和可用性 |
| 全年無休電子郵件支援 | 核心應用程式和雲端基礎建設的全年無休監控和電子郵件支援 | 核心應用程式和雲端基礎建設的全年無休監控和電子郵件支援 |
| 專屬的客戶技術顧問(CTA) | | 初始啟動期間的專屬技術帳戶管理，從您的訂閱開始，直到您的初始網站啟動為止 |
| 附加元件\* | <ul><li>[B2B模組](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _額外收費_

## 入門專案

[入門計畫架構](starter-architecture.md)有四個環境：

- **整合** — 整合環境提供兩個可測試的環境。 每個環境都包含作用中的Git分支、資料庫、網頁伺服器、快取、部分服務、環境變數和設定。

- **測試** — 當程式碼和擴充功能通過您的測試時，您可以將`integration`分支合併到測試環境，這會成為您的生產前測試環境。 它包含`staging`作用中分支、資料庫、Web伺服器、快取、協力廠商服務、環境變數、設定，以及Fastly和New Relic等服務。

- **生產** — 當程式碼準備就緒並測試時，所有程式碼都會合併到`master`，以部署到生產上線網站。 此環境包含您的作用中`master`分支、資料庫、Web伺服器、快取、協力廠商服務、環境變數和設定。

- **非使用中** — 您有不限數量的非使用中分支。

## Pro專案

[Pro計畫架構](pro-architecture.md)具有包含三個環境的全域`master`：

- **整合** — 整合環境提供可測試的環境，包括資料庫、網頁伺服器、快取、部分服務、環境變數和設定。 您可以開發、部署和測試程式碼，然後再合併至測試環境。

   - _非作用中_ — 根據`integration`環境，您可以有不限數量的非作用中分支，但只能有一個作用中分支（不包括`integration` ）。

- **暫存** — 暫存環境是用於生產前的測試，包括資料庫、Web伺服器、快取、協力廠商服務、環境變數、設定以及服務（例如Fastly）。

- **生產** — 生產環境包含適用於您的資料、服務、快取和存放區的三節點高可用性架構。 生產環境是您的即時公用存放區環境，包含環境變數、設定和協力廠商服務。

## 支援的軟體與服務

雲端基礎結構上的Adobe Commerce使用：

- 作業系統：Debian GNU/Linux
- 網頁伺服器：Nginx
- 資料庫：MySQL (MariaDB)
- 內容傳遞網路(CDN)： Fastly CDN

您可以設定下列服務：

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>如需建議的版本，請參閱&#x200B;_安裝指南_&#x200B;中的[系統需求](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)。

Fastly CDN模組用於中繼和生產環境上的CDN和快取服務。 請參閱[設定Fastly服務](../cdn/fastly.md)。

如需設定要在實施中使用的軟體版本的相關資訊，請參閱雲端基礎結構設定檔案上的下列Adobe Commerce：

- [應用程式設定(.magento.app.yaml)](../application/configure-app-yaml.md)
- [環境設定(.magento.env.yaml)](../environment/configure-env-yaml.md)
- [路由設定(routes.yaml)](../routes/routes-yaml.md)
- [服務組態(services.yaml)](../services/services-yaml.md)
