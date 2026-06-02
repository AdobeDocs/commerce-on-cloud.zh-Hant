---
title: 最佳化雲端部署
description: 瞭解在雲端基礎結構專案上最佳化Adobe Commerce部署流程的方法，包括減少停機時間、靜態內容部署、情境式部署和智慧型精靈。
feature: Cloud, Deploy, SCD
exl-id: 4315e2f4-06af-4a5c-9db9-e7b2f63660df
TQID: https://experienceleague.adobe.com/bd9n9CFrpyn1UZG6SX8qkoZGBOFd2N7z9Hoa1hQ8rew
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 230
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
