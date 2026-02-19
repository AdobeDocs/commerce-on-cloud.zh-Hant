---
title: New Relic帳戶管理
description: 瞭解如何存取您的New Relic帳戶，並管理雲端基礎結構專案上Adobe Commerce的存取權、整合和工具使用。
feature: Cloud, Observability
role: Admin
exl-id: 7aeedd12-7a81-47eb-a82f-3079e16ecb06
source-git-commit: 558c645e353e38ce8455ef17e1d0e9fa99b22c6e
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# New Relic帳戶管理

Adobe布建雲端基礎結構專案時，授權擁有者會收到New Relic的電子郵件，其中包含存取New Relic帳戶的憑證和指示。 如果您沒有收到電子郵件，請使用授權擁有者電子郵件地址來重設New Relic密碼。

如果授權擁有者已變更，而新授權擁有者目前無法存取New Relic，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。

## 管理使用者存取權（管理員角色）

>[!NOTE]
>
>僅授予嚴格要求存取完整功能集的使用者完整存取權。

**若要存取New Relic中的使用者管理**：

1. 登入您的[New Relic帳戶](https://login.newrelic.com/login)。

1. 從左下方導覽中選取您的使用者名稱。

1. 按一下&#x200B;**[!UICONTROL Administration]**&#x200B;並從清單中選取下列其中一項：

   - **[!UICONTROL User management]**&#x200B;新增使用者並管理作用中使用者和擱置的邀請。

   - **[!UICONTROL Access management]**&#x200B;管理使用者群組、角色和帳戶。

請參閱[New Relic](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/)檔案中的&#x200B;_使用者管理_。

## 為入門環境設定New Relic

>[!NOTE]
>
>**Pro環境**&#x200B;已預先設定為使用New Relic服務，且可略過啟用和連線指示。 如果中繼和生產環境未安裝New Relic APM，或生產環境中無法使用New Relic基礎架構，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以請求安裝。

對於入門環境，您必須檢查`.magento.app.yaml`檔案以確認`runtime`區段包含New Relic擴充功能。 如果尚未設定擴充功能，請新增下列專案：

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### 套用授權金鑰

若要將雲端環境連線至New Relic，請將New Relic授權金鑰新增至環境。

- 針對&#x200B;**Pro專案**，Adobe會在布建程式期間將授權金鑰新增至您的生產和中繼環境。 您可以登入您的[New Relic帳戶](https://login.newrelic.com/login)，以驗證雲端基礎結構網站上的Adobe Commerce與New Relic之間的連線。

- 針對&#x200B;**入門專案**，您擁有最多可支援&#x200B;_三個_&#x200B;環境的New Relic授權金鑰。 您必須手動將金鑰新增到您的環境設定。 入門環境未預先布建為使用New Relic服務。

對於入門環境，請將New Relic授權金鑰新增至環境設定，以啟用New Relic整合。 將金鑰新增到測試和生產環境，以及您選擇的其他一個環境。 設定僅需要New Relic授權金鑰。 您可以在[New Relic使用手冊](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html)的&#x200B;_Adobe Commerce報表_&#x200B;主題中找到其他設定選項的相關資訊。

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Adobe Commerce帳戶頁面或與專案相關聯的New Relic授權的登入認證
>- [要設定的入門環境之管理員層級存取權](../project/user-access.md)
>- 存取環境[Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html)的認證

**若要為入門環境設定New Relic**：

1. 從[!DNL Cloud Console]或雲端CLI尋找您的New Relic授權金鑰。

   **[!DNL Cloud Console]方法**：

   - 開啟您的雲端專案[帳戶頁面](https://accounts.magento.cloud/user)。

   - 在&#x200B;_專案_&#x200B;索引標籤上，尋找您的專案。

   - 按一下&#x200B;**檢視詳細資料**&#x200B;以取得專案基礎結構資訊。

   - 展開&#x200B;**New Relic服務**&#x200B;區段以檢視授權金鑰。

   - 複製授權金鑰。

   **雲端CLI方法**：

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. 使用`magento-cloud` CLI將New Relic授權金鑰新增至環境。

   - 變更至需要授權金鑰的環境。
   - 使用下列`magento-cloud` CLI命令更新變數值：

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   您可以選擇從[Commerce管理員](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store)新增它。

1. 登入您的[New Relic帳戶](https://login.newrelic.com/login)，以確認您可以從Adobe Commerce環境檢視資料。 請參閱[調查效能](investigate-performance.md)。

### 移除授權金鑰

您只能在三個使用中環境中使用New Relic授權金鑰。 如果金鑰在三個環境中使用，您必須從其中一個環境中移除金鑰，然後才能將其新增到其他環境。

**若要從環境中移除授權金鑰**：

1. 列出環境變數。

   ```bash
   magento-cloud variable:list
   ```

   範例回應：

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >如果您將授權金鑰新增為&#x200B;_專案_&#x200B;變數，則必須移除該專案層級的變數。 專案變數會將授權新增至每個已建立的&#x200B;_環境分支_，這會消耗或超過授許可權制。 若要列出專案變數： `magento-cloud variable:list --level project`

1. 刪除授權變數。

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```

## 變更雲端上New Relic的帳戶擁有者

若要變更雲端基礎結構專案中Adobe Commerce的New Relic帳戶擁有者：

1. **在New Relic UI中變更擁有者**。 請參閱New Relic檔案中的[變更帳戶擁有者](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/account-user-mgmt-tutorial/)。

2. **如果使用者不在您的帳戶中，請先新增使用者**。 請參閱New Relic檔案中的[新增和更新使用者](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/#add-users)。

3. **需要協助嗎？**&#x200B;如果沒有任何現有的擁有者或管理員可以提供協助，任何有權存取[Adobe Commerce合作夥伴擁有者帳戶](https://account.newrelic.com/accounts/1311131/users)的Adobe Commerce使用者都可以代表您新增使用者。

如需詳細資訊，請參閱[New Relic服務總覽](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service)。
