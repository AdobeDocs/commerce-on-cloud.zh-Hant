---
title: 升級Commerce版本
description: 瞭解如何將 Adobe Systems Commerce 版本升級在雲端基礎結構專案。
feature: Cloud, Upgrade
exl-id: 0cc070cf-ab25-4269-b18c-b2680b895c17
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '1547'
ht-degree: 0%

---

# 升級 Commerce 版本

您可以將 Adobe Systems Commerce 程式代碼庫升級至較新版本。 在升級專案之前，請查看安裝&#x200B;_指南中的_&#x200B;系統需求[&#128279;](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=zh-Hant)，瞭解最新的軟體版本要求。

根據您的專案配置，升級任務可能包括以下內容：

- 更新服務（如MariaDB（MySQL），OpenSearch，RabbitMQ和Redis，以便與新的Adobe Systems Commerce版本相容。
- 轉換較舊的組態管理檔案。
- 以掛接和環境變數的新設定更新`.magento.app.yaml`檔案。
- 協力廠商擴充功能升級至支援的最新版本。
- `.gitignore`更新檔案。

{{upgrade-tip}}

{{pro-update-service}}

## 從舊版本升級

如果要從早於 2.1 的 Commerce 版本開始升級，則 Adobe Systems Commerce 代碼庫中的某些限制可能會影響您 _更新_ 到特定 ECE-工具 版本或 _升級到_ 下一個受支援的 Commerce 版本的能力。 請使用下表來決定最佳路徑：

| 目前版本 | 升級路徑 |
| ----------------- | ------------ |
| 2.1.3和較舊版本 | 在繼續之前，請將Adobe Commerce升級至2.1.4版或更新版本。 然後執行[單次升級以安裝ECE-Tools](../dev-tools/install-package.md)。 |
| 2.1.4 - 2.1.14 | [更新ECE-Tools](../dev-tools/update-package.md)封裝。<br>請參閱[2002.0.9](../release-notes/cloud-release-archive.md#v200209)和2002.0.x以上版本的發行說明。 |
| 2.1.15 - 2.1.16 | [更新ECE-Tools](../dev-tools/update-package.md)封裝。<br>請參閱[2002.0.9](../release-notes/cloud-release-archive.md#v200209)和更新版本的發行說明。 |
| 2.2.x和更新版本 | [更新ECE-Tools](../dev-tools/update-package.md)封裝。<br>請參閱[2002.0.8](../release-notes/cloud-release-archive.md#v200208)和更新版本的發行說明。 |

{style="table-layout:auto"}

{{ece-tools-package}}

## 設定管理

舊版Adobe Commerce （例如2.1.4或更新版本至2.2.x或更新版本）使用`config.local.php`檔案進行組態管理。 Adobe Commerce 2.2.0版和更新版本使用`config.php`檔案，其運作方式與`config.local.php`檔案完全相同，但其組態設定不同，其中包含已啟用模組的清單和其他組態選項。

從舊版本升級時，必須遷移 `config.local.php` 檔以使用較新的 `config.php` 檔。 使用以下步驟備份配置檔並創建一個配置檔。

**若要建立暫存`config.php`檔案**：

1. 建立`config.local.php`檔案的復本，並將其命名為`config.php`。

1. 將此檔案新增至專案的`app/etc`資料夾。

1. 新增檔案並將其提交到您的分支。

1. 將檔案推送至整合分支。

1. 繼續進行升級程式。

>[!WARNING]
>
>升級之後，您可以移除`config.php`檔案並產生新的完整檔案。 您只能刪除這個檔案來取代它一次。 產生新的、完整的`config.php`檔案後，您無法刪除該檔案以產生新的檔案。 請參閱[設定管理和管道部署](../store/store-settings.md)。

### 驗證Zend Framework撰寫器相依性

從2.2.x **升級至** 2.3.x或更新版本時，請確認Zend Framework相依性已新增至`composer.json`檔案的`autoload`屬性，以支援Laminas。 此外掛程式支援Zend Framework的新需求，此架構已移轉至Laminas專案。 請參閱&#x200B;_Magento DevBlog_&#x200B;上的[將Zend架構移轉至Laminas專案](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251)。

**若要檢查`auto-load:psr-4`組態**：

1. 在本機工作站上，變更至專案目錄。

1. 請檢視您的整合分支。

1. 在文字編輯器中開啟`composer.json`檔案。

1. 檢查Zend外掛程式管理員實作的`autoload:psr-4`區段以瞭解控制器相依性。

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. 如果缺少Zend相依性，請更新`composer.json`檔案：

   - 將下列行新增至`autoload:psr-4`區段。

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - 更新專案相依性。

     ```bash
     composer update
     ```

   - 新增、提交和推送程式碼變更。

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - 先將變更合併到測試環境，然後再合併到生產環境。

## 組態檔

在升級應用程式之前，您必須更新專案設定檔，以考慮雲端基礎結構或應用程式上Adobe Commerce預設設定值的變更。 您可以在[magento-cloud GitHub存放庫](https://github.com/magento/magento-cloud)中找到最新的預設值。

### .magento.app.yaml

請一律檢閱[.magento.app.yaml](../application/configure-app-yaml.md)檔案中包含的值以取得您安裝的版本，因為它會控制應用程式建置和部署到雲端基礎結構的方式。 以下範例適用於版本2.4.8，並使用撰寫器2.8.4。`build: flavor:`屬性不用於Composer 2.x；請參閱[安裝及使用Composer 2](../application/properties.md#installing-and-using-composer-2)。

**若要更新`.magento.app.yaml`檔案**：

1. 在本機工作站上，變更至專案目錄。

1. 開啟並編輯`magento.app.yaml`檔案。

1. 更新PHP選項。

   ```yaml
   type: php:8.4
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.8.4'
   ```

1. 修改`hooks`屬性`build`和`deploy`命令。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 將下列環境變數新增至檔案結尾。

   對於 Adobe Systems Commerce 2.2.x 至 2.3.x–

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   對於 Adobe Systems Commerce 2.4.x–

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. 儲存文件。 請還不要提交或推送對遠端環境的更改。

1. 繼續進行升級程式。

### composer.json

升級之前，請一律檢查`composer.json`檔案中的相依性是否與Adobe Commerce版本相容。

**要更新 `composer.json` Adobe Systems Commerce 版本 2.4.4 及更高**&#x200B;版本的檔，請執行以下作：

1. 將以下內容 `allow-plugins` 新增到該 `config` 部份：

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. 將下列外掛程式新增至該 `require` 小節：

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. 將下列元件新增至該 `extra:component_paths` 小節：

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. 儲存文件。 尚未認可或推送變更至您的分支。

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
   >`magento-cloud db:dump`命令執行帶有`--single-transaction`旗標的[mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)命令，可讓您備份資料庫而不鎖定資料表。

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

[在升級應用程式之前，請查看服務版本](../services/services-yaml.md#service-versions)信息以了解最新的軟體版本要求。

**若要升級應用程式版本**：

1. 在本機工作站上，變更至專案目錄。

1. 使用[版本限制語法](overview.md#cloud-metapackage)設定升級版本。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >您必須使用版本限制語法才能成功更新`ece-tools`封裝。 您可以在用於升級的[應用程式範本](https://github.com/magento/magento-cloud/blob/master/composer.json)版本之`composer.json`檔案中找到版本限制。

1. 更新專案。

   ```bash
   composer update
   ```

1. 檢閱目前套用的修正程式：

   - 如果`m2-hotfixes`目錄中有安裝任何修補程式，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#support-case)，並與Adobe Commerce支援人員合作，確認哪些修補程式仍可套用至新版本。 從`m2-hotfixes`目錄移除不適用的修補程式。

   - 如果`.magento.env.yaml`檔案中套用了任何[品質修補程式]，請確認它們是否仍可套用至新版本。 從`.magento.env.yaml`檔案的`QUALITY_PATCHES`區段中移除不適用的修補程式。

   **方法1**： [驗證Quality Patches發行說明中的適用版本](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/quality-patches-tool/release-notes)

   **方法2**： [檢視可用的修補程式和狀態](https://experienceleague.adobe.com/zh-hant/docs/commerce-on-cloud/user-guide/develop/upgrade/apply-patches#view-available-patches-and-status)

   **方法3**： [搜尋修補程式](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=zh-Hant)


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

   因為Composer封送基底套件的方式，所以需要`git add -A`才能將所有變更的檔案新增到原始檔控制中。 從基底封裝（`magento/magento2-base`和`magento/magento2-ee-base`）將`composer install`和`composer update`封送檔案合併到封裝根目錄中。

   Composer封送處理的檔案屬於新版Adobe Commerce，以便覆寫這些相同檔案的過時版本。 目前，Adobe Commerce中已停用封送處理，因此您必須將封送處理檔案新增至原始檔控制。

1. 等待部署完成。

1. 使用SSH登入並檢查版本，在整合、測試或生產環境中驗證升級。

   ```bash
   php bin/magento --version
   ```

### 建立config.php檔案

如[組態管理](#configuration-management)中所述，升級之後，您必須建立更新的`config.php`檔案。 透過您整合環境中的「管理員」完成任何其他設定變更。

**若要建立系統特定的組態檔**：

1. 從終端機，使用SSH命令產生環境的`/app/etc/config.php`檔案。

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   例如，對於Pro，若要在`integration`分支上執行`scd-dump`：

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. 使用`rsync`或`scp`將`config.php`檔案傳輸至您的本機工作站。 您只能將此檔案新增至本機分支。

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. 新增、提交和推送程式碼變更。

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   這會產生具有模組清單和組態設定的更新`/app/etc/config.php`檔案。

>[!WARNING]
>
>若要升級，請刪除`config.php`檔案。 將此檔案新增到您的程式碼後，您應該&#x200B;**不**&#x200B;刪除它。 如果您必須移除或編輯設定，請手動編輯檔案。

### 升級擴充功能

檢閱Marketplace或其他公司網站中的協力廠商擴充功能和模組頁面，並確認對雲端基礎結構上的Adobe Commerce和Adobe Commerce的支援。 如果您必須升級任何協力廠商擴充功能和模組，Adobe建議您在停用擴充功能的情況下，使用新的整合分支。

**若要驗證並升級您的擴充功能**：

1. 建立您本地工作站上的分支。

1. 視需要停用您的擴充功能。

1. 如果可用，下載功能會升級。

1. 按照協力廠商文檔的說明安裝升級。

1. 啟用並測試擴充功能。

1. 添加、提交代碼更改並將其推送到遠端。

1. 推送至並在您的整合環境中測試。

1. 推送至測試環境，以便在預先生產環境中測試。

Adobe Systems強烈建議先升級生產環境 _，然後再_ 將升級后的擴展添加到網站啟動流程中。

>[!NOTE]
>
>升級 應用程式 版本時，升級過程會自動更新到最新版本的 [Fastly CDN 模組](../cdn/fastly.md#fastly-cdn-module-for-magento-2) 。

## 疑難解答升級

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
