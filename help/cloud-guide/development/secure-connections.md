---
title: 安全連線
description: 瞭解如何在雲端基礎結構專案上將SSH金鑰套用至您的Adobe Commerce並登入遠端環境。
role: Developer
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# 安全連線到遠端環境

Secure Shell (SSH)是用來安全登入遠端伺服器和系統的常見通訊協定。 您可以使用SSH存取遠端環境，以管理Adobe Commerce應用程式及存取遠端環境記錄。 Adobe僅支援使用SSH公開金鑰的安全FTP (sFTP)連線。 不支援FTP連線。

## 產生SSH金鑰組

在需要存取專案原始程式碼和環境的每台電腦和工作區上建立SSH金鑰組。 SSH金鑰可讓您連線至GitHub以管理原始程式碼並連線至雲端伺服器，而不需持續提供您的使用者名稱和密碼。 如需建立SSH金鑰組的進一步指示，請參閱[使用SSH連線至GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)。

- _公開金鑰_&#x200B;可安全供您存取網站、SSH及sFTP。
- _私密金鑰_&#x200B;在工作站上仍為私密。

>[!CAUTION]
>
>**永遠不要共用您的私密金鑰。**&#x200B;請勿將其新增至票證、複製到聊天，或附加至電子郵件。

## 新增SSH公開金鑰至您的帳戶

在雲端基礎結構帳戶上將SSH公開金鑰新增到Adobe Commerce後，請在您的帳戶上重新部署所有作用中的環境來安裝金鑰。

您可以使用下列其中一種方法將SSH金鑰新增至您的帳戶： Cloud CLI或[!DNL Cloud Console]。

>[!BEGINTABS]

>[!TAB CLI]

### 使用雲端CLI新增SSH金鑰

1. 在本機工作站上，變更至專案目錄。

1. 登入您的專案：

   ```bash
   magento-cloud login
   ```

1. 新增公開金鑰。

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>您可以使用Cloud CLI命令`ssh-key:list`和`ssh-key:delete`列出並刪除SSH金鑰。

>[!TAB 主控台]

### 使用[!DNL Cloud Console]新增您的SSH金鑰

**新增SSH金鑰至新專案**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 按一下&#x200B;**[!UICONTROL No SSH key]**。 此圖示位於命令欄位的右側，並在專案不包含SSH金鑰時顯示。

1. 在&#x200B;**公開金鑰**&#x200B;欄位中複製並貼上您的公開SSH金鑰的內容。

1. 按照其餘的提示進行。

**若要將SSH金鑰新增至您的雲端設定檔**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 在右上角帳戶功能表中，按一下&#x200B;**我的設定檔**。

1. 在&#x200B;_SSH金鑰_&#x200B;檢視中，按一下&#x200B;**新增公開金鑰**。

1. 在&#x200B;_新增SSH金鑰_&#x200B;表單中，將金鑰命名為&#x200B;**Title**，然後將公開SSH金鑰貼到&#x200B;**Key**&#x200B;欄位中。

1. 按一下&#x200B;**儲存**。

>[!ENDTABS]

## 連線到遠端環境

您可以使用`magento-cloud` CLI或SSH命令連線到遠端環境。 `magento-cloud` CLI命令只能用於Starter和Pro整合環境。

### 使用Cloud CLI

**若要登入遠端整合環境**：

1. 在本機工作站上，變更至專案目錄。

1. 列出該專案中的環境。

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### 使用SSH命令

[!DNL Cloud Console]包含每個環境的Web和SSH存取命令清單。

**若要複製SSH命令**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 選取環境。

1. 按一下&#x200B;**[!UICONTROL SSH]**。

1. 在&#x200B;_SSH_&#x200B;標籤中，按一下[複製]按鈕，將完整的SSH命令複製到剪貼簿。

1. 開啟終端機，然後貼上SSH命令以建立連線。

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>對於Pro測試和生產環境，SSH命令看起來可能像這樣：
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

雲端基礎結構上的Adobe Commerce支援使用SSH驗證的sFTP （安全FTP）存取您的環境。 使用支援sFTP之SSH金鑰驗證的使用者端，並使用您的公開SSH金鑰。 您的公開SSH金鑰必須新增至目標環境。 對於入門環境和Pro整合環境，您可以[透過 [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface)新增它。

唯讀sFTP連線&#x200B;_不支援_；依預設會提供&#x200B;_寫入_&#x200B;許可權的sFTP存取。

設定sFTP時，請使用SSH存取環境命令中的資訊： `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **使用者名稱**：您SSH存取目的地中`@`之前的所有內容。
- **密碼**：您不需要sFTP的密碼。 sFTP存取使用SSH金鑰驗證。
- **主機**：您SSH存取權中`@`之後的所有內容。
- **連線埠**： 22，這是預設的SSH連線埠。
- **SSH**&#x200B;私密金鑰：如有必要，請提供sFTP使用者端的私密金鑰位置。 依照預設，私密金鑰會儲存在`~/.ssh`目錄中。

視使用者端而定，可能需要其他選項才能完成sFTP的SSH驗證。 檢閱所選使用者端的檔案。

針對&#x200B;**入門環境和Pro整合環境**，您可能也會考慮[新增`mount`](../application/properties.md#mounts)以存取特定目錄。 您會將掛載新增至`.magento.app.yaml`檔案。 如需可寫入目錄的清單，請參閱[專案結構](../project/file-structure.md)。 此掛接點僅適用於這些環境。

針對&#x200B;**Pro測試和生產環境**，如果您沒有環境的SSH存取權，則必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，以要求sFTP存取權和特定資料夾（例如`pub/media`）的存取權掛載點。

>[!NOTE]
>對於Pro測試和生產，如果sFTP連線是針對執行&#x200B;**不**&#x200B;的&#x200B;_一般_&#x200B;使用者，則需要[將其新增至雲端專案](../project/user-access.md)，您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，並附加其&#x200B;**公開**&#x200B;金鑰。 **絕不提供您的私人SSH金鑰。**

## SSH通道

您可以使用SSH通道從您的本機開發環境連線到服務，就像該服務在本機一樣。 在通道之前，請設定您的[SSH](#add-an-ssh-public-key-to-your-account)。

使用終端機應用程式登入並發出命令。

```bash
magento-cloud login
```

驗證是否有任何通道是使用開啟的。

```bash
magento-cloud tunnel:list
```

若要建置通道，您必須知道[應用程式名稱](../application/properties.md#name)。 您可以使用CLI檢查應用程式名稱：

```bash
magento-cloud apps
```

### 設定SSH通道

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

例如，若要在名為`mymagento`的應用程式專案中開啟與`sprint5`分支的通道，請輸入

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

範例回應：

```
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**若要顯示您通道的相關資訊**：

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### 連線到服務

建立SSH通道後，您可以連線到服務，就像在本機執行一樣。 例如，若要連線到資料庫，請使用下列命令：

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
