---
title: 雲端CLI
description: 瞭解magento-cloud CLI，以及它如何協助您在雲端基礎結構專案上管理Adobe Commerce的本機開發環境。
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---


# 雲端CLI

`magento-cloud` CLI工具可讓開發人員和系統管理員管理Cloud專案和環境、執行常式，以及在本機執行自動化工作。 `magento-cloud` CLI擴充[[!DNL Cloud Console]](../../get-started/cloud-console.md)的特性和功能。 在您在本機工作站上安裝`magento-cloud` CLI之後，就可以在雲端基礎結構Starter和Pro整合環境中用它來管理您的Adobe Commerce。

>[!NOTE]
>
>這是本機工具，無法使用此方法安裝在雲端環境（唯讀）上。 您只能透過&#x200B;**部署工作流程**&#x200B;在雲端環境上安裝模組
>- [專業部署工作流程](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)
>- [入門部署工作流程](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/starter-develop-deploy-workflow)

**若要安裝`magento-cloud` CLI**：

1. 在您的&#x200B;_本機工作站_&#x200B;上，變更至您要複製雲端專案的目錄，且[檔案系統擁有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)具有&#x200B;_寫入_&#x200B;存取權。

1. 安裝`magento-cloud` CLI。

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. 將`magento-cloud` CLI新增至Bash設定檔。

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. 重新載入更新的bash設定檔。

   ```bash
   . ~/.bash_profile
   ```

1. 若要起始CLI，請呼叫`magento-cloud`，並在系統提示時輸入您的Cloud帳戶認證。

   ```bash
   magento-cloud
   ```

   ```
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. 確認`magento-cloud`命令位於您的路徑中。 下列範例列出可用的命令。

   ```bash
   magento-cloud list
   ```

## 常用命令

Adobe設計這些命令來管理雲端整合環境，並建議您從專案目錄執行`magento-cloud` CLI，以便省略`-p <project-ID>`引數。

下列常用的`magento-cloud` CLI命令清單僅包含必要的選項。 您可以搭配任何命令使用`--help`選項來檢視詳細資訊。

| 命令 | 說明 |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | 登入專案。 |
| `magento-cloud list` | 列出CLI工具的可用指令。 |
| `magento-cloud environment:list` | 列出目前專案中的環境。 |
| `magento-cloud environment:checkout` | 檢視現有環境。 |
| `magento-cloud environment:merge -e` | 將此環境中的變更與其父項合併。 |
| `magento-cloud variables` | 此環境中的清單變數。 |
| `magento-cloud ssh` | 使用SSH連線到遠端環境。 |
| `magento-cloud url` | 在瀏覽器中開啟Adobe Commerce店面。 |
| `magento-cloud web` | 開啟[!DNL Cloud Console]。 |

## 環境命令

環境&#x200B;_名稱_&#x200B;與環境&#x200B;_ID_&#x200B;不同，前提是在環境名稱中使用空格或大寫字母。 環境ID包含所有小寫字母、數字和允許的符號。 環境名稱中的大寫字母會在ID中轉換為小寫；環境名稱中的空格會轉換為破折號。

環境名稱&#x200B;_不能_&#x200B;包含保留給Linux shell或規則運算式的字元。 禁止使用的字元包括大括弧(`{ }`)、括弧、星號(`*`)、角括弧(`< >`)、&amp;符號(`&`)、% (`%`)及其他字元。

`magento-cloud environment:list`命令顯示環境階層，而`git branch`則不顯示。 如果您有任何巢狀環境，請使用下列專案：

```bash
magento-cloud environment:list
```

### 重新部署環境

不使用推播觸發重新部署。 驗證並確認要重新部署的環境。 如果有處於擱置狀態的組建，請勿使用重新部署。

```bash
magento-cloud environment:redeploy
```

範例回應：

```
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Git命令

您可能會注意到其中有些指令類似於Git指令。 `magento-cloud`命令會直接連線至具有其他功能的Git型Cloud專案。 如果您未使用`magento-cloud` CLI建立分支，則不會「啟動」，而且當您推送變更至遠端環境時，也不會自動建置。 `magento-cloud` CLI命令包含啟用。

若要建立分支，請使用`magento-cloud`命令，讓分支啟動。

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

針對分支狀態：

- 使用`magento-cloud env`命令檢視環境分支的清單及其狀態：作用中或非作用中。
- 使用`magento-cloud environment:activate`命令啟動環境分支。

推送空的Git認可以觸發部署。 例如：

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

有些動作（例如新增使用者）不會進行部署。

### 建立環境分支

下列步驟會示範如何交替使用CLI和Git指令來管理您的本機環境：

1. 在本機工作站上，變更至專案目錄。

1. 切換至[檔案系統擁有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html)。

1. 登入您的專案。

   ```bash
   magento-cloud login
   ```

1. 列出您的專案。

   ```bash
   magento-cloud project:list
   ```

1. 列出專案中的環境。 每個環境都包含一個作用中的Git分支，其中包含您的程式碼、資料庫、環境變數、設定和服務。

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >使用`magento-cloud environment:list`命令很重要，因為它顯示環境階層，而`git branch`命令不顯示。

1. 擷取來源分支以取得最新的程式碼。

   ```bash
   git fetch origin
   ```

1. 簽出或切換至特定分支和環境。

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   Git命令只會檢查Git分支。 `magento-cloud checkout`命令會簽出分支並切換到使用中環境。

   >[!TIP]
   >
   >您可以使用`magento-cloud environment:branch <environment-name> <parent-environment-ID>`命令語法來建立環境分支。 建立及啟用環境分支可能需要一些額外的時間。

1. 使用環境ID將任何更新的程式碼提取到您的本機。 如果環境分支是新的，則不需要這樣做。

   ```bash
   git pull origin <environment-ID>
   ```

1. （_選擇性_）建立環境的[快照](../storage/snapshots.md)作為備份。

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## 更新CLI

`magento-cloud` CLI會在您登入時檢查可用的更新，但您可以使用`self:update`命令來檢查更新。 如果有可用的更新，請依照指示更新CLI。

如果您的`magento-cloud` CLI是最新的，您會看到下列回應：

```bash
magento-cloud update
```

```
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
