---
title: 使用 [!DNL Cloud Console]管理分支
description: 瞭解如何使用 [!DNL Cloud Console]在雲端基礎結構上管理Adobe Commerce的環境分支。
role: Developer
feature: Cloud, Install
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# 使用[!DNL Cloud Console]管理分支

您可以使用[!DNL Cloud Console]或`magento-cloud` CLI管理環境。 您的專案檔案儲存在Git存放庫中。 您可以使用Git命令來管理您的程式碼，但`magento-cloud` CLI的設計是要與平台功能互動，而Git命令則否。 請參閱雲端CLI主題中的[Git命令](../dev-tools/cloud-cli-overview.md#git-commands)。

此主題討論如何使用[!DNL Cloud Console]來：

- 新增或刪除環境
- 從父環境同步(`git pull`)
- 將(`git push`)合併至父環境

>[!TIP]
>
>您無法從Pro測試和生產環境建立分支。 您可以從`master`分支中分支。

## 建立環境

分支策略會使用常見的Git工作流程，您可在其中開發程式碼並在開發分支中新增擴充功能。 請參閱[入門者](../architecture/starter-architecture.md)和[Pro](../architecture/starter-develop-deploy-workflow.md)架構概述。

- 首先，從`master`分支建立`staging`分支，然後從`staging`分支進行開發。
- 對於Pro，請從`Integration`環境建立開發分支。

您的帳戶支援有限數量的![個使用中分支](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} （非使用中）開發分支。 僅使用[!DNL Cloud Console]或雲端CLI新增或刪除分支，以管理作用中或非作用中分支。 刪除分支之前，請先停用該分支，它仍保留在&#x200B;_環境_&#x200B;清單中，做為&#x200B;_非使用中_。 您可以稍後重新啟用分支，或是在環境設定或使用Cloud CLI中[刪除分支](../dev-tools/cloud-cli-overview.md#)。

如果您需要其他使用中的環境進行開發，請提交[支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

**若要新增分支**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 選取環境。

   >[!TIP]
   >
   >您的新分支是從此環境複製。 選擇與您要建立之環境類似的父環境。

1. 按一下&#x200B;**[!UICONTROL Branch]**。

   ![建立分支](../../assets/button-branch.png){width="150"}

1. 在&#x200B;_分支來源……_&#x200B;表單中，輸入分支名稱。

   環境&#x200B;_名稱_&#x200B;與環境&#x200B;_ID_&#x200B;不同，前提是在環境名稱中使用空格或大寫字母。 環境ID包含所有小寫字母、數字和允許的符號。 環境名稱中的大寫字母會在ID中轉換為小寫；環境名稱中的空格會轉換為破折號。

   環境名稱&#x200B;**不能**&#x200B;包含保留給Linux shell或規則運算式的字元。 禁止使用的字元包括大括弧(`{ }`)、括弧、星號(`*`)、角括弧(`>`)、&amp;符號(`&`)、百分比(<code>%</code>)和其他字元。

1. 選取&#x200B;**[!UICONTROL Environment type]**。

1. 按一下&#x200B;**[!UICONTROL Create Branch]**。

1. 環境正在部署，請稍候。

   部署期間，環境狀態為&#x200B;**處理中**。 成功部署後，**success**&#x200B;的狀態會變更為綠色核取記號。

## 建立非使用中分支

您無法從Adobe Commerce Cloud主控台或CLI建立非使用中分支。 如果您想建立非作用中分支，請在Git存放庫上建立它，並在命令上使用`environment.Parent`選項進行推送。

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## 刪除環境

您必須先停用環境，才能刪除環境。 一旦環境處於非使用中狀態，您就可以將其刪除。

**若要停用環境**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 從導覽列&#x200B;_環境_&#x200B;清單中選取環境。

1. 按一下頂端導覽列右側的設定圖示，開啟環境設定。

1. 在&#x200B;_[!UICONTROL General]_標籤上，向下捲動至_[!UICONTROL Deactivate environment]_&#x200B;區段，然後按一下&#x200B;**[!UICONTROL Deactivate environment and delete data]**&#x200B;並遵循指示。

## 同步環境

同步環境（或分支）與`git pull origin <parent>`相同。 您可以從上層環境同步更新的程式碼。 您可以透過[!DNL Cloud Console]在所有入門和Pro環境中使用此功能。

對於Pro計畫，您可以從測試和生產同步到您的`master`分支。 此同步僅提取和推送程式碼，不會提取資料。 若要同步資料，請傾印資料庫資料並將其推送到另一個環境的資料庫。 請參閱[移轉及部署靜態檔案和資料](/help/cloud-guide/deploy/staging-production.md#migrate-static-files)。

**若要同步環境**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 在環境清單中，按一下要同步的分支名稱。

1. 按一下（同步）。

   ![同步環境](../../assets/button-sync.png){width="150"}

1. 選取要同步的專案。

   - 取代資料 — （資料與檔案）同步來自父分支的資料庫與內容檔案中的變更。
   - 合併(Merge) - （程式碼）從父分支同步更新的程式碼。

   這樣也會建置CLI指令供您複製和使用。

1. 按一下&#x200B;**同步**。

## 與上層環境合併

合併環境（或分支）與`git push origin`相同。 您可以合併以將更新後的程式碼從環境推送至其父環境。 您可以將此程式碼合併至`master`。 您可以使用`merge`命令部署至測試和生產環境。

**若要與父環境合併**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 在環境清單中，按一下要合併的分支名稱。

1. 按一下（合併）。

   ![合併環境](../../assets/button-merge.png){width="150"}

1. 按一下&#x200B;**合併**&#x200B;並確認動作。

## 檢視記錄

透過[!DNL Cloud Console]，您可以檢閱環境的各種記錄，包括建置、部署和部署歷史記錄。

對於&#x200B;**Starter**，您可以檢閱組建和部署記錄檔以及部署歷程記錄。 這些環境包括`master` （生產）分支以及從中建立的所有分支。

對於&#x200B;**Pro**，您可以在每個環境中檢閱下列記錄：

- 整合 — 建置、部署和部署歷史記錄
- 測試 — 建置記錄檔和部署歷史記錄。 使用SSH登入伺服器以檢視部署記錄。
- 生產 — 建置記錄檔和部署歷史記錄。 使用SSH登入伺服器以檢視部署記錄。

**若要檢視[!DNL Cloud Console]**&#x200B;中的記錄檔：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 選取環境。

   環境檢視提供[活動清單](activity-stream.md)，其中顯示&#x200B;_最近_&#x200B;個事件，每個嘗試的動作會有一個專案，包括同步、合併、分支、備份等。 按一下&#x200B;**全部**&#x200B;以取得完整的部署歷程記錄。

1. 若要檢視建置記錄，請選取帳戶上每個部署記錄的「成功」或「失敗」連結。

>[!TIP]
>
>按一下下拉式清單的&#x200B;**篩選依據**&#x200B;圖示，並選取要檢視的訊息型別。

## 從私人Git存放庫提取程式碼

您在雲端基礎結構專案上的Adobe Commerce可包含來自私人Git存放庫的程式碼。 例如，您可能在私人存放庫中擁有自訂模組或主題的程式碼。 若要這麼做，您必須將專案的公開SSH金鑰新增至您的私人Git存放庫，並更新專案`composer.json`檔案。

若要將部署金鑰新增至您的私人GitHub存放庫，您必須是該存放庫的管理員。 GitHub允許您僅對一個存放庫使用部署金鑰。

如果您偏好讓專案存取多個存放庫，您可以將SSH金鑰附加至自動使用者帳戶。 因為此帳戶不是由人類使用，所以它稱為[電腦使用者](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys)。 將電腦帳戶新增為共同作業人員，或將電腦使用者新增至具有存放庫存取權的團隊。

>[!INFO]
>
>Adobe建議將此程式碼新增並合併至專案Git存放庫。 如果您未設定連線，則可能會遇到建置問題。

**若要尋找您的SSH公開金鑰**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 按一下上方導覽列右側的設定圖示。

1. 在&#x200B;_專案設定_&#x200B;中，按一下&#x200B;**[!UICONTROL Deploy Key]**。

1. 將部署金鑰複製到剪貼簿，以便用於下列其中一個Git型方法：

>[!BEGINTABS]

>[!TAB GitHub]

### 輸入您的GitHub部署金鑰

在GitHub上，部署金鑰預設為唯讀。

**若要輸入您的專案公開金鑰做為GitHub部署金鑰**：

1. 以管理員身分登入您的GitHub存放庫。
1. 按一下存放庫&#x200B;**[!UICONTROL Settings]**&#x200B;標籤。

   >[!NOTE]
   >
   >如果沒有看到此選項，表示您不是以存放庫管理員的身分登入，且無法完成此工作。 請要求您的GitHub存放庫管理員執行此操作。

1. 在左側導覽的&#x200B;_設定_&#x200B;索引標籤上，按一下&#x200B;**[!UICONTROL Deploy Keys]**。
1. 按一下&#x200B;**[!UICONTROL Add deploy key]**。
1. 按照提示操作。

在`composer.json`中，使用`<user>@<host>:<.git</code>`格式，若使用非標準連線埠，則使用`ssh://<user>@<host>:<port>/<path>.git`。

>[!TAB 位元貯體]

### 輸入您的Bitbucket部署金鑰

**若要輸入您的專案公開金鑰做為Bitbucket部署金鑰**：

1. 以管理員身分登入您的Bitbucket存放庫。
1. 在左側導覽中，按一下&#x200B;**[!UICONTROL Settings]**。
1. 按一下「一般> **[!UICONTROL Deployment Keys]**」。
1. 按一下&#x200B;**[!UICONTROL Add Key]**。
1. 按照提示操作。

>[!TAB GitLab]

### 輸入您的GitLab部署金鑰

**若要將專案的公開SSH金鑰新增為GitLab部署金鑰**：

1. 以擁有者身分登入您的GitLab存放庫。
1. 確認已為您的專案啟用&#x200B;_管道_&#x200B;選項：

   1. 在專案設定中，展開&#x200B;**[!UICONTROL Visibility, project, features, permissions]**&#x200B;區段。
   1. 必要時，按一下&#x200B;**[!UICONTROL Pipelines]**&#x200B;以啟用選項。

1. 將您的公開SSH金鑰新增至CI/CD設定。

   1. 在左側導覽中，按一下「設定> **[!UICONTROL CI / CD]**」。
   1. 按一下部署金鑰&#x200B;**展開**&#x200B;以設定金鑰。
   1. 在&#x200B;_部署金鑰_&#x200B;表單中，將部署金鑰名稱新增至&#x200B;**[!UICONTROL Title]**&#x200B;欄位，並將您的公開SSH金鑰貼到&#x200B;**[!UICONTROL Key]**&#x200B;欄位。
   1. 按一下&#x200B;**[!UICONTROL Add Key]**&#x200B;以儲存組態。

>[!ENDTABS]

## 保護環境和分支的安全

您可以使用[!DNL Cloud Console]，透過網頁瀏覽器從任何位置存取您的專案和環境。 您可以為生產環境、商店和網站設定安全性。 本節可協助您確保嚴格針對開發人員、DBA等的整合與測試環境的安全性。

>[!WARNING]
>
>**請勿**&#x200B;使用下列方法來保護Pro測試和生產環境的安全。 這會中斷Fastly快取。 使用Adobe Commerce的Fastly CDN中可用的[封鎖](../cdn/fastly-vcl-blocking.md)功能。

**保護環境**：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。

1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。

1. 選取環境並按一下導覽列上的設定圖示。

1. 在環境設定&#x200B;_一般_&#x200B;標籤上，按一下&#x200B;**[!UICONTROL HTTP access control enabled]**&#x200B;的&#x200B;**開啟**&#x200B;以啟用安全存取。 您可以在認證或IP位址之間選擇，以篩選存取許可權。

1. 若要依認證篩選，請按一下&#x200B;**[!UICONTROL Add Login]**，輸入使用者名稱和密碼，然後按一下&#x200B;**[!UICONTROL Add Login]**&#x200B;以新增。

1. 若要依IP位址篩選，請在含有`deny`或`allow`的清單中輸入IP位址。 例如：

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. 按一下&#x200B;**[!UICONTROL Save]**。 這會重新部署環境以更新安全性和設定。 Adobe建議在完成安全性設定後測試環境。
