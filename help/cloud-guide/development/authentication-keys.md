---
title: 驗證金鑰
description: 瞭解如何在雲端基礎結構上將驗證金鑰套用至Adobe Commerce中的開發專案。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 驗證金鑰

您必須擁有驗證金鑰才能存取Adobe Commerce存放庫，並在雲端基礎結構專案上為Adobe Commerce啟用安裝和更新命令。 指定Composer授權認證有兩個方法。

- **驗證檔案** — 在雲端基礎結構根目錄上的Adobe Commerce中，包含您的Adobe Commerce [授權認證](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html)的檔案。
- **環境變數** — 一個環境變數，可在Adobe Commerce雲端基礎結構專案中設定驗證金鑰，以防止意外曝光。

>[!BEGINSHADEBOX]

**安全性注意事項**

Adobe建議將[環境變數](#composer-auth-environment-variable)方法與您的雲端專案搭配使用，以防止您的授權認證意外曝光。

使用Commerce的Cloud Docker作為本機開發工具時，驗證檔案方法非常理想，但請注意，不要將`auth.json`檔案上傳到公用的Git型存放庫。 您可以將`auth.json`檔案新增至[`.gitignore`檔案](../project/file-structure.md#ignoring-files)。

>[!ENDSHADEBOX]

## 驗證檔案

**若要建立`auth.json`檔案**：

1. 如果您的專案根目錄中沒有`auth.json`檔案，請建立一個。

   - 使用文字編輯器，在您的專案根目錄中建立`auth.json`檔案。
   - 將[範例`auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample)的內容複製到新的`auth.json`檔案中。

1. 將`<public-key>`和`<private-key>`取代為您的Adobe Commerce驗證認證。

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 儲存變更並退出文字編輯器。

## Composer驗證環境變數

以下方法是防止在公開Git型存放庫中意外暴露敏感憑證的最佳方法。

**若要使用環境變數**&#x200B;新增驗證金鑰：

1. 在&#x200B;_[!DNL Cloud Console]_&#x200B;中，按一下專案導覽右側的設定圖示。

   ![設定專案](../../assets/icon-configure.png){width="36"}

1. 在&#x200B;_專案設定_&#x200B;清單中，按一下&#x200B;**[!UICONTROL Variables]**。

1. 按一下&#x200B;**[!UICONTROL Create variable]**。

1. 在&#x200B;**[!UICONTROL Variable name]**&#x200B;欄位中，輸入`env:COMPOSER_AUTH`。

1. 在&#x200B;_Value_&#x200B;欄位中，新增下列專案，並將`<public-key>`和`<private-key>`取代為您的Adobe Commerce驗證認證：

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. 選取&#x200B;**[!UICONTROL Available during buildtime]**&#x200B;並取消選取&#x200B;**[!UICONTROL Available during runtime]**。

1. 按一下&#x200B;**[!UICONTROL Create variable]**。

1. 從每個環境中移除`auth.json`檔案。
