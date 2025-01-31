---
title: 設定應用程式部署
description: 瞭解如何在應用程式設定檔案中設定屬性，以控制 [!DNL Commerce] 應用程式建置和部署到雲端環境的方式。
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 0%

---

# 設定應用程式部署

`.magento.app.yaml`檔案控制應用程式建置和部署的方式。 雖然雲端基礎結構上的Adobe Commerce支援每個專案使用多個應用程式，但通常一個專案有一個應用程式，且存放庫根目錄中有`.magento.app.yaml`個檔案。

`.magento.app.yaml`有許多預設值，請參閱[範例`.magento.app.yaml`檔案](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml)。 請一律檢閱已安裝版本的`.magento.app.yaml`。 此檔案在雲端基礎結構版本上可能因Adobe Commerce而異。

使用`.magento.app.yaml`檔案來定義下列組態值：

- [屬性](properties.md) — 定義應用程式執行個體的屬性值。
- [變數屬性](variables-property.md) — 檢閱[!DNL Commerce]應用程式版本所需的環境變數。
- [PHP設定](php-settings.md) — 設定執行階段PHP選項。
- [設定靜態檔案的快取](set-cache.md) — 設定您媒體和靜態檔案的快取TTL。

>[!NOTE]
>
>`.magento.app.yaml`檔案是在本機或Git存放庫中管理的。 系統僅會針對部署和建置流程讀取設定，並在部署完成後移除設定，因此您在伺服器上找不到該設定。
