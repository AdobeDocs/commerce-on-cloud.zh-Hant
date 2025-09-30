---
title: 雲端基礎結構上的Commerce
description: 了解如何建置、部署和管理雲端基礎結構上的 Commerce。
exl-id: a37d0403-df14-4bb9-8cc4-25436560ba0c
source-git-commit: 988b07b827c32c94a8cb2d30abb7e73fec71c4a0
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 3%

---


# 雲端基礎結構上的Commerce

雲端基礎結構上的Adobe Commerce提供自動化託管平台，在雲端原生環境中建置、部署及管理您的&#x200B;**應用程式時，可採用**&#x200B;自助服務[!DNL Commerce]方法。 雲端基礎結構上的Adobe Commerce除了內部部署Adobe Commerce和Magento Open Source平台外，還隨附其他功能：

- 預先布建的基礎結構，包含PHP、MySQL (MariaDB)、Redis、[!DNL RabbitMQ]以及支援的搜尋引擎技術。
- Git型工作流程，具有自動建置和部署功能，可在您每次推播Platform as a Service (PaaS)環境中的程式碼變更時，有效率地快速開發和持續部署。
- 可高度自訂的環境組態檔和命令列介面(CLI)管理與部署工具。
- Amazon Web Services (AWS)主機服務，為線上銷售及零售提供可擴充且安全的環境。

![雲端優點](../assets/CloudBenefits.svg)

>[!NOTE]
>
>如需安全性詳細資訊，請參閱[安全性啟動檢查清單](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/launch/checklist#security-configuration)。

詳細檢視[技術棧疊](architecture/tech-stack.md)，或進一步瞭解Commerce [的](architecture/cloud-architecture.md)雲端架構中的特定功能和支援產品。

<div id="recs-overview-body-1"></div>
<div id="recs-overview-body-2"></div>
<div id="recs-overview-body-3"></div>
<div id="recs-overview-body-4"></div>
<div id="recs-overview-body-5"></div>
<div id="recs-overview-body-6"></div>

## 雲端區域

以下章節提供雲端基礎結構上Adobe Commerce可用的不同AWS和Azure區域的詳細資訊。

## AWS地區

![顯示AWS地區的圖表](../assets/aws-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 僅限在中國及俄羅斯進行內部部署。

## Azure地區

顯示Azure區域的![圖表](../assets/azure-regions.svg){zoomable="yes"}

>[!NOTE]
>
> 僅限在中國及俄羅斯進行內部部署。 所有需要整合環境的商家都必須使用美國地區。

## Adobe Commerce檔案

Commerce雲端基礎結構指南假設您具備一些關於Adobe Commerce應用程式的工作知識和瞭解。 您可以參閱下列[!DNL Commerce]開發人員與使用手冊：

- [Adobe Commerce開發人員檔案](https://developer.adobe.com/commerce/docs/) (Adobe Developer網站) — 開發、自訂、整合、擴充及使用進階功能

- [Adobe Commerce檔案](https://experienceleague.adobe.com/docs/commerce.html) (Adobe Experience League) — 規劃、實作、操作、升級及維護您的[!DNL Commerce]專案

{{$include /help/_includes/templated/whats-new.md}}

<!-- Last updated from includes: 2025-09-19 20:32:03 -->
