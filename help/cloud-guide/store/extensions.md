---
title: 管理擴充功能
description: 瞭解如何在雲端基礎結構上的Adobe Commerce中安裝和管理擴充功能。
feature: Cloud, Extensions, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# 管理擴充功能

您可以從[Commerce Marketplace](https://marketplace.magento.com)新增擴充功能，以擴充您的Adobe Commerce應用程式功能。 例如，您可以新增佈景主題來變更店面的外觀和風格，或新增語言套件來本地化您的店面和管理員。

>[!NOTE]
>
>為避免安裝問題，所有Marketplace購買都必須使用擁有雲端專案的相同帳戶(MAGEID)完成。

## 副檔名的撰寫器名稱

雖然本節討論如何從Commerce Marketplace取得副檔名的Composer名稱和版本，但您可以在模組的Composer檔案中找到&#x200B;_any_&#x200B;模組的名稱和版本。 在文字編輯器中開啟`composer.json`檔案，並記下`"name"`和`"version"`值。

**若要從Commerce Marketplace**&#x200B;取得模組的撰寫器名稱：

1. 以您購買元件的使用者名稱和密碼登入[Commerce Marketplace](https://marketplace.magento.com)。

1. 在右上角，按一下您的使用者名稱並選取&#x200B;**我的設定檔**。

   ![存取您的Marketplace帳戶](../../assets/marketplace/my-profile.png)

1. 在&#x200B;_我的帳戶_&#x200B;頁面上，按一下&#x200B;**我的購買**。

   ![市集購買記錄](../../assets/marketplace/my-purchases.png)

1. 在&#x200B;_我的購買_&#x200B;頁面上，選取您購買的模組，然後按一下&#x200B;**技術詳細資料**。

1. 按一下「**複製**」將[!UICONTROL Component name]複製到剪貼簿。

1. 開啟文字編輯器並貼上元件名稱，然後附加冒號字元(`:`)。

1. 在&#x200B;**技術詳細資料**&#x200B;中，按一下&#x200B;**複製**&#x200B;將[!UICONTROL Component version]複製到剪貼簿。

1. 在文字編輯器中，將版本編號附加至冒號後的元件名稱。 例如：

   ```text
   extension-name/magento2:1.0.1
   ```

## 安裝擴充功能

當您將擴充功能新增至實作時，Adobe建議在開發分支中工作。 安裝擴充功能時，擴充功能名稱(`<VendorName>_<ComponentName>`)會自動插入[`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html?lang=zh-Hant)檔案中。 不需要直接編輯檔案。

**若要安裝擴充功能**：

1. 在本機工作站上，變更至專案目錄。

1. 建立或簽出開發分支。 請參閱[分支](../development/cli-branches.md)。

1. 使用撰寫器名稱和版本，將副檔名新增至`composer.json`檔案的`require`區段。

   ```bash
   composer require <extension-name>:<version> --no-update
   ```

1. 更新專案相依性。

   ```bash
   composer update
   ```

1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >安裝擴充功能時，您必須將程式碼變更推播至遠端環境時包含`composer.lock`檔案。 `composer install`命令會讀取`composer.lock`檔案，以啟用遠端環境中定義的相依性。

1. 建置和部署完成後，請使用SSH登入遠端環境並確認已安裝擴充功能。

   ```bash
   bin/magento module:status <extension-name>
   ```

   副檔名使用格式： `<VendorName>_<ComponentName>`。

   範例回應：

   ```
   Module is enabled
   ```

   如果您遇到部署錯誤，請參閱[擴充功能部署失敗](../deploy/recover-failed-deployment.md)。

## 管理擴充功能

當您使用Composer新增擴充功能時，部署程式會自動啟用擴充功能。 如果您已安裝擴充功能，可以使用CLI啟用或停用擴充功能。 管理擴充功能時，請使用格式： `<VendorName>_<ComponentName>`

登入遠端環境時，切勿啟用或停用擴充功能。

**若要啟用或停用擴充功能**：

1. 在本機工作站上，變更至專案目錄。

1. 啟用或停用模組。 `module`命令以模組要求的狀態更新`config.php`檔案。

   >啟用模組。

   ```bash
   bin/magento module:enable <module-name>
   ```

   >停用模組。

   ```bash
   bin/magento module:disable <module-name>
   ```

1. 如果您已啟用模組，請使用`ece-tools`重新整理組態。

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. 驗證模組的狀態。

   ```bash
   bin/magento module:status <module-name>
   ```

1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## 升級擴充功能

繼續進行之前，您需要該擴充功能的撰寫器名稱和版本。 此外，請確認擴充功能與您的專案和Adobe Commerce版本相容。 特別是，在您開始之前[檢查所需的PHP版本](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html?lang=zh-Hant)。

**若要更新擴充功能**：

1. 在本機工作站上，變更至專案目錄。

1. 建立或簽出開發分支。 請參閱[分支](../development/cli-branches.md)。

1. 在文字編輯器中開啟`composer.json`檔案。

1. 找到您的擴充功能並更新版本。

1. 儲存變更並退出文字編輯器。

1. 更新專案相依性。

   ```bash
   composer update
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

如果發生錯誤，請參閱[從元件失敗復原](../deploy/recover-failed-deployment.md)。 若要進一步瞭解如何將擴充功能與Adobe Commerce搭配使用，請參閱&#x200B;_管理指南_&#x200B;中的[擴充功能](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html?lang=zh-Hant)。
