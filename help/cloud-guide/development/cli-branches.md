---
title: 使用CLI管理分支
description: 瞭解如何使用Cloud CLI在雲端基礎結構上管理Adobe Commerce的環境分支。
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# 使用CLI管理分支

若要安裝`magento-cloud` CLI，請參閱[雲端CLI參考](../dev-tools/cloud-cli-overview.md)。 安裝`magento-cloud` CLI並設定SSH金鑰以遠端存取雲端基礎結構後，您就可以使用`magento-cloud` CLI命令來管理專案的環境。 如需有關環境架構的資訊，請參閱[入門架構](../architecture/starter-architecture.md)或[專業架構](../architecture/pro-architecture.md)。

若要使用[!DNL Cloud Console]管理分支和環境，請參閱[使用 [!DNL Cloud Console]](../project/console-branches.md)管理分支。

## 使用CLI命令

`magento-cloud` CLI命令與Git命令類似。 您可以使用這些專案來連線至您的專案並管理您的環境。 雖然您可以從任何目錄執行命令，但建議您從專案目錄執行這些命令。 從專案目錄執行時，您可以省略`-p <project-ID>`引數。 請參閱[雲端CLI參考](../dev-tools/cloud-cli-overview.md)。

## 複製專案

下列指示使用`magento-cloud` CLI指令與Git指令的組合，將專案複製到本機工作站。 若要檢視`magento-cloud` CLI命令的完整清單，請使用`magento-cloud list`命令。

>[!IMPORTANT]
>
>有些Git命令無法在Adobe Commerce的雲端基礎結構專案中完成動作。 例如，您可以使用Git指令建立分支，但無法建立和啟動新環境。 您必須使用`magento-cloud environment:branch <branch-name>`命令建立環境，環境才能變成&#x200B;_作用中_。 或者，您可以使用[!DNL Cloud Console]來建立使用中的環境。 請參閱[雲端CLI參考](../dev-tools/cloud-cli-overview.md#git-commands)。

**若要複製專案`master`環境**：

1. 使用[檔案系統擁有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=zh-Hant)帳戶登入您的本機工作站。

1. 變更至網頁伺服器或虛擬主機&#x200B;_docroot_&#x200B;目錄。

1. 使用`magento-cloud` CLI登入。

   ```bash
   magento-cloud login
   ```

1. 列出您的專案。

   ```bash
   magento-cloud project:list
   ```

1. 複製專案。

   ```bash
   magento-cloud project:get <project-ID>
   ```

   出現提示時，請提供目錄名稱。

1. 變更至`magento2`目錄。

1. 列出專案的可用環境。

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >`magento-cloud environment:list`命令顯示環境階層，而`git branch`命令不顯示。

1. 擷取遠端分支。

   ```bash
   git fetch origin
   ```

1. 提取更新的程式碼。

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>如需在雲端基礎結構上搭配Adobe Commerce使用Git型託管服務的相關資訊，請參閱[整合](../integrations/overview.md)。

## 建立開發分支

複製專案並更新Adobe Commerce管理員帳戶設定後，您可以分支進行開發。 如前所述，您必須使用`magento-cloud environment:branch <branch-name>`命令或[!DNL Cloud Console]建立環境，環境才能變成&#x200B;_使用中_。

- 對於[入門者](../architecture/starter-develop-deploy-workflow.md#clone-and-branch)，請考慮為`staging`建立分支，然後根據`staging`分支建立開發分支。
- 針對[Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow)，根據`Integration`分支建立開發分支。

**若要建立開發分支**：

1. 在本機工作站上，變更至專案目錄。

1. 根據專案工作流程建議的分支建立環境。

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. 更新相依性。

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_選擇性_]&#x200B;建立環境的[備份](../storage/snapshots.md)。

### 合併分支

完成開發後，將此分支合併至父級：

1. 認可和推送程式碼變更：

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 與上層環境合併：

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### 刪除環境

只有在您確定不再需要某個環境時，才將其刪除。 刪除環境後，就無法復原環境。

>[!WARNING]
>
>您無法刪除任何專案的`master`分支。

您必須是專案管理員、環境管理員或帳戶擁有者才能執行此工作。 請參閱[管理雲端專案的使用者存取權](../project/user-access.md)。

當您刪除環境時，環境會設為&#x200B;_非使用中_。 程式碼仍可在Git分支中使用，但不再包含服務或資料庫。 若要完全刪除環境，您也必須刪除對應的遠端Git分支。

**若要刪除環境**：

1. 在本機工作站上，變更至專案目錄。

1. 從遠端伺服器擷取更新。

   ```bash
   git fetch
   ```

1. 刪除環境分支。

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   您可以選擇將多個環境ID新增至delete命令，一次刪除多個環境。

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. 回應刪除本機環境和對應遠端環境的提示。

   ```
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   刪除環境會使其處於&#x200B;_非作用中_&#x200B;狀態。

   ```
   Delete the remote Git branch too? [Y/n]
   ```

   刪除遠端Git分支會從專案移除環境。

1. 等待環境刪除。

   ```
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>若要啟用非使用中的環境，請使用`magento-cloud environment:activate`命令。

## 與遠端環境互動

在您[設定SSH金鑰](../development/secure-connections.md)之後，您可以[從本機工作區連線到遠端環境](../development/secure-connections.md#connect-to-a-remote-environment)，並與您的專案服務互動並修改設定。
