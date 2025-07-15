---
title: 升級Commerce版本
description: 瞭解如何在雲端基礎結構專案中升級Adobe Commerce版本。
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: bcb5b00f7f203b53eae5c1bc1037cdb1837ad473
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---

# 升級Commerce版本

您可以將Adobe Commerce程式碼基底升級至較新版本。 升級專案之前，請檢閱[安裝](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html)指南中的&#x200B;_系統需求_，以取得最新的軟體版本需求。

根據您的專案組態，您的升級任務可能包括以下內容：

- 使用MariaDB (MySQL)、OpenSearch、RabbitMQ和Redis的新版本更新`.magento/services.yaml`檔案，以相容於新的Adobe Commerce版本。
- 以掛接和環境變數的新設定更新`.magento.app.yaml`檔案。
- 將協力廠商擴充功能升級至最新支援的版本。

{{upgrade-tip}}

{{pro-update-service}}

## 組態檔

在升級應用程式之前，您必須更新專案設定檔，以考慮雲端基礎結構或應用程式上Adobe Commerce預設設定值的變更。 您可以在[magento-cloud GitHub存放庫](https://github.com/magento/magento-cloud)中找到最新的預設值。

### composer.json

升級之前，請一律檢查`composer.json`檔案中的相依性是否與Adobe Commerce版本相容。

若要更新Adobe Commerce 2.4.4版及更新版本的`composer.json`檔案**：

1. 將下列`allow-plugins`新增至`config`區段：

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 將下列外掛程式新增至`require`區段：

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 將下列元件新增至`extra:component_paths`區段：

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. 儲存檔案。 尚未認可或推送變更至您的分支。

1. 繼續進行升級程式。

## 專案備份

建議您先建立專案的備份，再進行升級。 使用下列步驟來備份您的整合、測試和生產環境。

**若要備份您的整合環境資料庫和程式碼**：

1. 建立遠端資料庫的本機備份。

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >`magento-cloud db:dump`命令執行帶有[旗標的](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)mysqldump`--single-transaction`命令，可讓您備份資料庫而不鎖定資料表。

1. 備份程式碼和媒體。

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   如果您有大量靜態檔案已經在原始檔控制中，您可以選擇忽略`[--media]`。

**若要在部署之前備份您的預備或生產環境資料庫**：

1. 使用SSH登入遠端環境。

1. 建立[資料庫傾印](../storage/database-dump.md)。 若要選擇資料庫傾印的目標目錄，請使用`--dump-directory`選項。

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   傾印作業會在您的遠端專案目錄中建立`dump-<timestamp>.sql.gz`封存檔案。 請參閱[備份資料庫](../storage/database-dump.md)。

## 應用程式升級

請先檢閱[服務版本](../services/services-yaml.md#service-versions)資訊，瞭解最新的軟體版本需求，然後再升級您的應用程式。

**若要升級應用程式版本**：

1. 在本機工作站上，變更至專案目錄。

1. 為目標升級版本設定[版本限制](overview.md#cloud-metapackage)。 只有在目標版本超出現有限制時，才需要執行此步驟。

   ```bash
   composer require-commerce "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >您必須使用版本限制語法才能成功更新`ece-tools`封裝。 您可以在用於升級的`composer.json`應用程式範本[版本之](https://github.com/magento/magento-cloud/blob/master/composer.json)檔案中找到版本限制。

1. 使用核心Commerce升級版本更新您的`composer.json`檔案。

   ```bash
   composer require-commerce magento/product-enterprise-edition 2.4.8 --no-update
   ```

1. 如果您使用B2B，請以Commerce的`composer.json`支援版本[更新您的](https://experienceleague.adobe.com/en/docs/commerce-operations/release/product-availability#adobe-authored-extensions)檔案。

   ```bash
   composer require-commerce magento/extension-b2b 1.5.2 --no-update
   ```

1. 更新專案相依性。

   ```bash
   composer update
   ```

1. 檢閱目前套用的修正程式：

   - 如果`m2-hotfixes`目錄中有安裝任何修補程式，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)，並與Adobe Commerce支援人員合作，確認哪些修補程式仍可套用至新版本。 從`m2-hotfixes`目錄移除不適用的修補程式。

   - 如果[檔案中套用了任何]品質修補程式`.magento.env.yaml`，請確認它們是否仍可套用至新版本。 從`QUALITY_PATCHES`檔案的`.magento.env.yaml`區段中移除不適用的修補程式。

   **方法1**： [驗證Quality Patches發行說明中的適用版本](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法2**： [檢視可用的修補程式和狀態](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法3**： [搜尋修補程式](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=en)


1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   因為Composer封送基底套件的方式，所以需要`git add -A`才能將所有變更的檔案新增到原始檔控制中。 從基底封裝（`composer install`和`composer update`）將`magento/magento2-base`和`magento/magento2-ee-base`封送檔案合併到封裝根目錄中。

   Composer封送處理的檔案屬於新版Adobe Commerce，以便覆寫這些相同檔案的過時版本。 目前，Adobe Commerce中已停用封送處理，因此您必須將封送處理檔案新增至原始檔控制。

1. 等待部署完成。

1. 使用SSH登入並檢查版本，在整合、測試或生產環境中驗證升級。

   ```bash
   php bin/magento --version
   ```

### 升級擴充功能

檢閱Marketplace或其他公司網站中的協力廠商擴充功能和模組頁面，並確認對雲端基礎結構上的Adobe Commerce和Adobe Commerce的支援。 如果您必須升級任何協力廠商擴充功能和模組，Adobe建議您在停用擴充功能的情況下，使用新的整合分支。

**若要驗證並升級您的擴充功能**：

1. 在您的本機工作站上建立分支。

1. 視需要停用您的擴充功能。

1. 可用時，下載擴充功能升級。

1. 安裝第三方檔案記錄的升級。

1. 啟用並測試擴充功能。

1. 新增、提交程式碼變更，並將其推播至遠端。

1. 推送至並在您的整合環境中測試。

1. 推送至測試環境，以便在預先生產環境中測試。

Adobe強烈建議您在網站啟動程式中&#x200B;_之前升級您的生產環境_，包括升級的擴充功能。

>[!NOTE]
>
>當您升級應用程式版本時，升級程式會自動更新為[Fastly CDN模組](../cdn/fastly.md#fastly-cdn-module-for-magento-2)的最新版本。

## 疑難排解升級

如果升級失敗，您會在瀏覽器中收到一則錯誤訊息，指出您無法存取店面或管理面板：

```
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**若要解決錯誤**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 開啟`./app/var/report/<error number>`檔案。

1. [檢查記錄](../test/log-locations.md)並判斷問題的來源。

1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
