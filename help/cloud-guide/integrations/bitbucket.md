---
title: 位元貯體整合
description: 瞭解如何將雲端基礎結構專案上的Adobe Commerce與Bitbucket整合。
feature: Cloud, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# 位元貯體整合

您可以設定Bitbucket存放庫，以便在推送程式碼變更時自動建立及部署環境。 此整合會在雲端基礎結構帳戶上，將您的Bitbucket存放庫與Adobe Commerce同步。

{{private-repository}}

## 先決條件

- 雲端基礎結構專案上Adobe Commerce的管理員存取權
- 本機環境中的[`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md)工具
- 位元貯體帳戶
- Bitbucket存放庫的管理員存取權
- 位元貯體存放庫的SSH存取金鑰

## 準備您的存放庫

從現有環境複製雲端基礎結構專案上的Adobe Commerce，並將專案分支移轉至新的空位元貯體存放庫，保留相同的分支名稱。 **關鍵**&#x200B;在於要保留相同的Git樹狀結構，這樣您就不會遺失雲端基礎結構專案中Adobe Commerce的任何現有環境或分支。

1. 從終端機，在雲端基礎結構專案上登入您的Adobe Commerce。

   ```bash
   magento-cloud login
   ```

1. 列出您的專案並複製專案ID。

   ```bash
   magento-cloud project:list
   ```

1. 將專案複製到您的本機環境。

   ```bash
   magento-cloud project:get <project-ID>
   ```

1. 將您的Bitbucket存放庫新增為遠端。

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   遠端連線的預設名稱可以是`origin`或`magento`。 如果`origin`存在，您可以選擇不同的名稱，或者重新命名或刪除現有的參照。 請參閱[git-remote檔案](https://git-scm.com/docs/git-remote)。

1. 確認您已正確新增Bitbucket遠端。

   ```bash
   git remote -v
   ```

   預期回應：

   ```
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. 將專案檔案推送至新的Bitbucket存放庫。 請記得保持所有分支名稱相同。

   ```bash
   git push -u origin master
   ```

   如果您從新的Bitbucket存放庫開始，您可能需要使用`-f`選項，因為遠端存放庫不符合您的本機復本。

1. 確認您的Bitbucket存放庫包含所有專案檔案。

## 建立OAuth消費者

Bitbucket整合需要[OAuth消費者](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/)。 您需要此消費者的OAuth `key`和`secret`以完成下一節。

**若要在Bitbucket**&#x200B;中建立OAuth取用者：

1. 登入您的[Bitbucket](https://id.atlassian.com/login)帳戶。

1. 按一下&#x200B;**設定** > **存取管理** > **OAuth**。

1. 按一下&#x200B;**新增消費者**，並依照下列方式設定：

   ![Bitbucket OAuth取用者設定](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >不需要有效的&#x200B;**回撥URL**，但您必須在此欄位中輸入值，才能成功完成整合。

1. 按一下&#x200B;**儲存**。

1. 按一下消費者&#x200B;**名稱**&#x200B;以顯示您的OAuth `key`和`secret`。

1. 複製您的OAuth `key`和`secret`以設定整合。

## 設定整合

1. 從終端機，導覽至您在雲端基礎結構專案上的Adobe Commerce 。

1. 建立名為`bitbucket.json`的暫存檔案，並新增下列專案，將角括弧中的變數取代為您的值：

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >請務必使用Bitbucket存放庫的名稱，而非URL。 如果您使用URL，整合將會失敗。

1. 使用`magento-cloud` CLI工具將整合新增至專案。

   >[!WARNING]
   >
   >以下命令會使用Bitbucket存放庫中的程式碼覆寫Adobe Commerce on cloud infrastructure專案中的&#x200B;_所有_&#x200B;程式碼。 這包括所有分支，包括`production`分支。 此動作會立即發生且無法還原。 在雲端基礎結構專案上從Adobe Commerce複製所有分支，並在新增Bitbucket整合之前&#x200B;**將其推送到Bitbucket存放庫，這是最佳作法。**

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   這會傳回含有標頭的長HTTP回應。 成功的整合會傳回200或201狀態代碼。 若狀態為400或以上，表示發生錯誤。

1. 刪除暫存`bitbucket.json`檔案。

1. 驗證專案整合。

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   記下&#x200B;**連結URL**&#x200B;以在BitBucket中設定webhook。

### 在BitBucket中新增webhook

為了與您的Cloud Git伺服器通訊事件（例如推播），您的BitBucket存放庫需要有webhook。 若正確遵循Bitbucket整合的設定方法，此頁面上會詳細說明此方法並自動建立Webhook。 請務必驗證webhook以避免建立多個整合。

1. 登入您的[Bitbucket](https://id.atlassian.com/login)帳戶。

1. 按一下&#x200B;**存放庫**&#x200B;並選取您的專案。

1. 按一下「**存放庫設定**」>「**工作流程**」>「**Webhooks**」。

1. 請先驗證webhook再繼續。

   如果掛接為作用中，請略過其餘步驟，然後[測試整合](#test-the-integration)。 掛接的名稱應該類似於&#x200B;**&quot;雲端基礎結構上的Adobe Commerce &lt;project_id>&quot;**，並且掛接URL格式類似於： `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. 按一下&#x200B;**新增webhook**。

1. 在&#x200B;_新增webhook_&#x200B;檢視中，編輯下列欄位：

   - **標題**： Adobe Commerce整合
   - **URL**：使用您`magento-cloud`整合清單中的連結URL
   - **觸發器**：預設為基本的&#x200B;_存放庫推送_

1. 按一下&#x200B;**儲存**。

### 測試整合

設定Bitbucket整合後，您可以使用`magento-cloud` CLI來驗證整合運作中：

```bash
magento-cloud integration:validate
```

或者，您也可以推送簡單的變更至位元貯體存放庫以進行測試。

1. 建立測試檔案。

   ```bash
   touch test.md
   ```

1. 認可變更並將其推播至您的Bitbucket存放庫。

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. 登入[[!DNL Cloud Console]](../project/overview.md)並確認您的認可訊息已顯示，且您的專案已部署。

   ![正在測試Bitbucket整合](../../assets/bitbucket-integration.png)

## 建立雲端分支

Bitbucket整合無法在雲端基礎結構專案的Adobe Commerce中啟用新環境。 如果您使用Bitbucket建立環境，則必須手動啟動環境。 若要避免這個額外的步驟，最佳做法是使用`magento-cloud` CLI工具或[!DNL Cloud Console]建立環境。

**若要啟用以Bitbucket**&#x200B;建立的分支：

1. 使用`magento-cloud` CLI推送分支。

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. 確認環境為作用中。

   ```bash
   magento-cloud environment:list
   ```

   ```
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

建立環境後，您可以使用一般Git命令將對應分支推送至遠端Bitbucket存放庫。 對您在Bitbucket中的分支進行的後續變更會自動建置和部署環境。

## 移除整合

您可以安全地從專案移除Bitbucket整合，而不會影響您的程式碼。

**若要移除Bitbucket整合**：

1. 從終端機，在雲端基礎結構專案上登入您的Adobe Commerce。

1. 列出您的整合。 您需要Bitbucket整合ID才能完成下一步。

   ```bash
   magento-cloud integration:list
   ```

1. 刪除整合。

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

此外，您也可以登入您的Bitbucket帳戶並撤銷帳戶&#x200B;_設定_&#x200B;頁面上的OAuth授予，以移除Bitbucket整合。

## Bitbucket伺服器整合

若要使用Bitbucket伺服器整合，您需要下列專案：

- [位元貯體存取Token](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html) — 產生授與專案`read`存取權和存放庫`admin`存取權的權杖
- [Bitbucket伺服器URL](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html) — 新增Bitbucket執行個體的基底URL

雖然您可以使用Cloud CLI來進行Bitbucket伺服器整合步驟，但完整的命令看起來類似以下內容：

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

使用help命令以取得更多使用要求與選項： `magento-cloud integration:add --help`
