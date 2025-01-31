---
title: 最佳化雲端部署
description: 瞭解在雲端基礎結構專案上最佳化Adobe Commerce部署流程的方法，包括減少停機時間、靜態內容部署、情境式部署和智慧型精靈。
feature: Cloud, Deploy, SCD
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '229'
ht-degree: 0%

---

# 最佳化部署

網站效能在部署過程中可能會受到影響。 部署到生產網站時，網站處於維護模式的時間長度取決於許多因素，例如環境設定和網站包含的內容量。 最佳化雲端部署的第一個最佳實務是[升級以使用`ece-tools`](../dev-tools/install-package.md)來受益於封裝功能，例如建立資料庫備份及驗證環境設定的命令。

下列主題可協助您更清楚瞭解如何最佳化部署程式：

- [雲端部署程式](process.md)
雲端部署流程有三個階段，您可以利用每個階段的優缺點，從中獲益。

- [零停機部署](reduce-downtime.md)
瞭解部署期間會發生什麼情況，以及如何在更新生產環境期間減少商店體驗的停機時間。

- [靜態內容部署](static-content.md)
最佳化雲端部署的最佳方式是控制如何及何時產生靜態內容。

- [智慧型精靈](smart-wizards.md)
`ece-tools`套件提供智慧型精靈命令，可快速評估您的專案組態。

- [使用New Relic追蹤部署](../monitor/track-deployments.md)
使用New Relic服務來監控部署事件，並分析部署對整體效能的影響。
