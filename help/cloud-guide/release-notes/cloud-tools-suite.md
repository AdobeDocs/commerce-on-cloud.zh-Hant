---
title: Cloud Tools Suite發行說明
description: 瞭解適用於Adobe Commerce的Cloud Tools套裝的最新改善。
feature: Cloud, Release Notes
exl-id: ee2bc2e9-bdf4-4f7b-9724-8f4dd1e61378
source-git-commit: 843420a49699caa5def4e8251f47872b300672b5
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# Commerce Cloud Tools Suite發行說明

此發行資訊詳細說明適用於Commerce套件的Cloud Tools Suite最新改善，這些改善旨在部署和管理Cloud平台上的Adobe Commerce安裝和升級。

| 發行說明 | 版本 | 說明 | Source |
| ----------------- |----------| ---------------------------------------- | --------------------------- |
| [ece-tools套件](ece-tools-package.md) | 2002.2.8 | 一組用來管理和部署雲端專案的指令碼和工具 | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.2.8) |
| 適用於Commerce的[雲端修補程式](cloud-patches.md) | 1.1.11 | 一組修補程式，可改善所有Adobe Commerce版本與雲端環境的整合。 此套件包含Adobe Commerce修補程式和使用`ece-tools`部署時套用的可用Hotfix | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.1.11) |
| 適用於Commerce的[Cloud Docker](cloud-docker.md) | 1.4.5 | Docker映像將Adobe Commerce部署到本地雲端環境的功能和設定檔案 | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.4.5) |
| [Commerce的雲端元件](cloud-components.md) | 1.1.3 | 針對部署在雲端基礎結構上的網站延伸Adobe Commerce核心功能 | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.1.3) |

當您更新至ECE-Tools 2002.1.0或更新版本時，會自動更新至`ece-tools`套件的相依性的其他套件的最新版本。 如需相依性清單，請參閱[雲端中繼](../development/overview.md#cloud-metapackage)。

新版`ece-tools` (2002.2.0)僅適用於PHP 8.1和更高版本(8.2、8.3)。 舊版PHP已過時(7.4、7.3、7.2)。 您可以使用舊版`ece-tools`搭配舊版PHP。

