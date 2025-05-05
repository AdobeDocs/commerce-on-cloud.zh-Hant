---
title: 準備開發
description: 收集認證，並瞭解可用於設定開發工作區的工具，以便與雲端基礎結構專案上的Commerce搭配使用。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# 準備開發

無論您是Commerce的新手還是目前移至雲端基礎結構的Commerce擁有者，請依照這些步驟來準備雲端專案的開發工作區。 如果您已完成其中部分的步驟，或擁有現有的Adobe Commerce開發人員環境，請檢閱下列的預期結果，然後繼續下一個步驟。 有些設定和工作流程與典型的內部部署安裝不同。

## 認證

設定工作區之前，請先收集下列金鑰和帳戶存取權：

- **驗證金鑰（撰寫器金鑰）**

  驗證金鑰是32個字元的驗證權杖，可提供對Adobe Commerce Composer存放庫(`repo.magento.com`)以及應用程式開發所需的任何其他Git服務（例如GitHub）的安全存取。 您的帳戶可以有多個驗證金鑰。 對於工作區設定，請從程式碼存放庫的一個特定鍵開始。 如果您沒有任何金鑰，請連絡專案所有者，或自行建立[驗證金鑰](../cloud-guide/development/authentication-keys.md)。

- **雲端專案帳戶**

  專案所有者應邀請您加入雲端基礎結構專案的Adobe Commerce 。 當您收到電子郵件邀請時，請按一下連結，然後依照提示建立您的帳戶。 請參閱[上線](onboarding.md)。

- **Adobe Commerce加密金鑰**

  僅匯入現有系統時，請擷取用來保護您存取和資料庫資料的加密金鑰。 如需此金鑰的詳細資訊，請參閱[解決加密金鑰的問題](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html?lang=zh-Hant)

## 開發人員工具

- **安裝雲端CLI**

  安裝`magento-cloud` CLI，以便您可以管理雲端環境並執行自動化工作。 如需安裝指示，請參閱[雲端CLI](../cloud-guide/dev-tools/cloud-cli-overview.md)。

- **安裝Docker以進行本機開發和測試**

  可選擇使用Docker環境來模擬雲端基礎結構`integration`環境上的Commerce以進行本機開發。 有三個基本元件：Adobe Commerce v2範本、Docker Compose和`ece-tools`套件。

   - [Docker架構和常用命令](../cloud-guide/dev-tools/cloud-docker.md)
   - [啟動Docker開發環境](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [ECE-Tools套件](../cloud-guide/dev-tools/package-overview.md)

- **整合Git型服務**

  可選擇在雲端基礎結構上整合Git型託管服務（例如GitHub或GitLab）與Adobe Commerce。 請參閱[整合](../cloud-guide/integrations/overview.md)。

## 專案程式碼

安全連線對於與遠端環境互動至關重要。 若為新專案，[登入 [!DNL Cloud Console]](https://console.adobecommerce.com)並按一下&#x200B;**[!UICONTROL No SSH key]**。 此圖示位於命令欄位的右側，並在專案不包含SSH金鑰時顯示。 請參閱[安全連線](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account)。

**若要將程式碼基底複製到本機工作站**：

1. 在[[!DNL Cloud Console]](https://console.adobecommerce.com)中，按一下&#x200B;**[!UICONTROL code]**&#x200B;並選取&#x200B;**[!UICONTROL Git]**&#x200B;索引標籤。

   ![複製您的程式碼](../assets/ui-git-code.png){width="450"}

1. 複製提供的`git clone ...`命令。

1. 在終端機中，建立並變更您的工作目錄。

1. 貼上並執行`git clone ...`命令。

>[!TIP]
>
>Adobe使用範本存放庫布建初始專案環境，其中包含特定版本Adobe Commerce的套件指示。 檢閱[專案檔案結構](../cloud-guide/project/file-structure.md)主題，並深入瞭解重要的專案檔案和雲端範本。
