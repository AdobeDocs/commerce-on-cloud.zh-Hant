---
title: 存放區選項和設定管理概觀
description: 在雲端基礎結構上自訂您的Adobe Commerce商店。
feature: Cloud, Configuration, Services
exl-id: e653172f-7370-4761-b2ce-3a420b33b948
TQID: https://experienceleague.adobe.com/iseYcfjh61-4ArUf9rKBGKFVuOz1dbS1xVq-FYiCxsw
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 193
ht-degree: 0%

---

# 存放區選項和設定管理概觀

自訂商店有許多方式，例如新增自訂主題、安裝擴充功能，或是在雲端基礎結構環境中強制執行特定設定。 您可以直接在中繼和生產環境中設定特定服務的設定。 您可以設定多個網站和商店。 存放區組態可協助您在本機工作站中設定這些選項，並在各個環境中部署特定設定。

若要存取您的店面，請使用`magento-cloud url`命令並回應提示。 或者，您可以在&#x200B;**存取網站**&#x200B;下的[!DNL Cloud Console]中找到URL。

## 設定存放區選項

存放區選項包括：

* [企業對企業模組(B2B)](b2b-module.md)
* [自訂主題](custom-theme.md)
* [擴充功能](extensions.md)
* [多個網站](multiple-sites.md)
* [付款服務](paypal.md)

## 設定服務與整合

有特定的[組態檔](../environment/overview.md)可管理遠端環境的特定部署行為。 您可以分別檢閱這些主題：

* [應用程式部署](../application/configure-app-yaml.md)
* [環境建置和部署動作](../environment/configure-env-yaml.md)
* [傳入要求路由](../routes/routes-yaml.md)
* [支援的服務](../services/services-yaml.md)

## 設定管理

設定存放區選項、服務和整合後，請使用設定管理在所有環境中以一致的方式部署這些設定，並將停機時間降到最低。 請參閱[組態管理](store-settings.md)。
