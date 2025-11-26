---
title: 開發概覽
description: 使用Adobe Commerce在雲端基礎結構專案上準備本機開發。
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 14fb0b41-1c3a-4abc-8726-cea16ab00ba8
source-git-commit: 0d84d29c470a098c7238b6ca7cc9538463dda695
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# 開發概覽

雲端基礎結構遠端環境上的Adobe Commerce是&#x200B;**唯讀**，包括所有入門環境以及所有Pro整合、中繼和生產環境。 在本機開發環境中，您可以先撰寫及測試程式碼，再將程式碼推送至整合環境，進一步測試並部署到中繼和生產環境。

在準備本機工作區之前，請確定您有[認證](../../get-started/prepare-workspace.md)。 本機開發需要安裝PHP和Composer，除非您選擇使用適用於Commerce[的](#docker-environment)Cloud Docker。

## 必要的套件

雲端基礎結構上的Adobe Commerce使用Composer來管理專案的相依性和升級。 對於本機開發，您必須安裝與您的雲端專案相容的PHP和Composer版本。 例如，如果您使用[!DNL Commerce] 2.4.8雲端範本，您會看到[`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.8/.magento.app.yaml)組態檔使用&#x200B;**PHP 8.4**&#x200B;和&#x200B;**Composer 2.8.4**。

Composer會將專案所需的程式庫和相依性安裝在`vendor`目錄中。 下列必要的撰寫器檔案位於專案根目錄中：

- `composer.json` — 使用`composer.json`檔案管理產品安裝和升級。
- `composer.lock` — `composer.lock`檔案儲存一組完全符合版本相依性的版本，這些相依性滿足專案相依性樹狀結構中每個套件的版本限制。

**常用命令：**

| 命令 | 說明 |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | 更新至`composer.json`檔案中反映的最新版本相依性。 這會更新`composer.lock`檔案。 |
| `composer install` | 讀取`composer.lock`檔案以下載相依性。 最佳實務是在您的專案存放庫中保留`composer.lock`的最新復本。 |

{style="table-layout:auto"}

新增、認可及推播更新的程式碼後，部署程式會在`composer install`建置階段[期間自動執行](../deploy/process.md#build-phase-build-phase)命令。

### 雲端中繼

雲端基礎結構上的Adobe Commerce使用需要`magento/product-enterprise-edition`的中繼套件。 若要取得Commerce最新版本的最新更新，請使用下列限制語法：

```text
>=current_version <next_version
```

例如，若要使用最新的Adobe Commerce 2.4.9版，請在`2.4.8`檔案中將`2.4.9`設定為「目前」版本，並將`composer.json`設定為「下一個」版本：

```text
"magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
```

此中繼封裝的主要封裝如下：

- **vendor/magento/ece-tools** — 此`ece-tools`套件與Adobe Commerce 2.1.4版或更新版本相容，提供您可用來在雲端基礎結構專案上管理Adobe Commerce的豐富功能。 它包含雲端基礎結構命令上的指令碼和Adobe Commerce，旨在協助管理您的程式碼並自動建置和部署您的專案。 檢視[`ece-tools`封裝總覽](../dev-tools/package-overview.md)。
- **vendor/magento/product-enterprise-edition** — 此中繼資料需要應用程式元件，包括模組、架構、主題等。
- **vendor/fastly2/magento2** — 此模組管理Pro測試環境、生產環境和入門生產環境的Fastly CDN和服務。 檢視[Fastly服務](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2)。
- **vendor/magento/module-paypal-on-boarding** — 此模組會連線至您的PayPal商家帳戶，以提供PayPal付款閘道結帳。 請參閱[PayPal上線工具](../store/paypal.md)。
- **廠商/aem/rum** — 此模組管理[作業遙測](../monitor/operational-telemetry.md)資料收集工具。

>[!TIP]
>
>如需相依性和第三方授權清單，請參閱[Adobe Commerce發行說明](/help/cloud-guide/release-notes/cloud-packages.md)中的&#x200B;_Commerce雲端套件_。

## Docker環境

您可以使用適用於Commerce的Cloud Docker工具，在本地開發的雲端基礎結構生產和開發環境中模擬Adobe Commerce。 適用於Commerce的Cloud Docker不需要在本機安裝PHP和Composer。

- 在Adobe Developer網站中使用Cloud Docker[進行](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)本機開發
- [Docker架構和常用命令](../dev-tools/cloud-docker.md)
- [Cloud Docker發行說明](../release-notes/cloud-docker.md)

>[!TIP]
>
>如需在雲端基礎結構上搭配Adobe Commerce使用Git型託管服務的相關資訊，請參閱[整合](../integrations/overview.md)。
