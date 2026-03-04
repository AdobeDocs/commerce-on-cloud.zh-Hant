---
title: 管理員變數
description: 請參閱在雲端基礎結構上安裝Adobe Commerce時使用的環境變數清單。
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: d2746185-bc59-4d30-a088-73df1bd2c0b2
source-git-commit: 4e751f02b92f954cd41d5523237da295a068661a
workflow-type: tm+mt
source-wordcount: '758'
ht-degree: 0%

---

# 管理員變數

對雲端基礎結構專案具有Adobe Commerce管理存取許可權的使用者可以使用以下專案環境變數覆寫管理使用者帳戶的組態設定，以存取管理UI。

## 管理員認證

您可以在安裝Commerce期間使用下表中的管理員變數覆寫管理員使用者認證。

如果您要在安裝後變更值，請使用SSH連線至您的環境，並使用Adobe Commerce CLI [`admin:user`命令](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=zh-Hant)來建立或編輯管理員使用者認證。

| 變數 | 預設 | 說明 |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | 授權擁有者電子郵件地址 | 管理員使用者的使用者名稱，可建立其他使用者，包括管理員使用者。 |
| `ADMIN_EMAIL` |                             | 管理使用者的電子郵件地址。 此位址用於傳送密碼重設通知。 |
| `ADMIN_PASSWORD` |                             | 管理使用者的密碼。 建立專案時，會產生隨機密碼並傳送電子郵件給授權所有者。 在專案建立期間，授權擁有者應該已經變更密碼。 請連絡授權擁有者，以取得更新的密碼。 |
| `ADMIN_LOCALE` | `en_US` | 管理員使用的預設地區設定。 |

## 管理員URL

使用以下環境變數來保護對管理員UI的存取。 若指定，此值會在安裝期間覆寫預設URL。 在雲端基礎結構上的[!DNL Adobe Commerce]中，您必須使用（[!DNL Cloud Console]或[!DNL Cloud CLI]）中的`ADMIN_URL`變數來設定或變更管理員URL。 從[!DNL Admin]修改設定僅適用於內部部署安裝。

`ADMIN_URL` — 存取管理員UI的相對URL。 預設URL為`/admin`。

### 變更管理員URL

根據預設，[Commerce管理員](https://experienceleague.adobe.com/docs/commerce-admin/start/admin/admin.html?lang=zh-Hant) URL已設定為&#x200B;*&lt;網域名稱>/管理員*。 基於安全考量，Adobe建議將其變更為不容易猜測的唯一自訂管理員URL。

**在雲端基礎結構**&#x200B;上的[!DNL Adobe Commerce]中，您必須使用（[!DNL Cloud Console]或[!DNL Cloud CLI]）中的`ADMIN_URL`環境變數變更管理員URL。 從[!DNL Admin]修改設定僅適用於內部部署安裝。 對於內部部署安裝，請遵循[使用自訂管理URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html?lang=zh-Hant#use-a-custom-admin-url)。

Adobe建議您在安裝後變更Admin URL的環境層級變數。 基於安全考量設定此設定，然後再從複製的`master`環境進行分支。 除非您將繼承設定為false，否則從`master`分支建立的所有分支都會繼承環境層級變數及其值。

使用[!DNL Cloud Console]或[!DNL Cloud CLI]來設定或更新`ADMIN_URL`。

#### 選項A：使用[!DNL Cloud Console]變更管理員URL

##### 整合環境

從[雲端主控台](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=zh-Hant)，新增變數並包含：

- **名稱：** `ADMIN_URL`
- **值：**&#x200B;您的新管理員URL （例如，`magento_A8v10`）

- 如需詳細步驟，請參閱開發人員檔案中的[新增環境變數](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html?lang=zh-Hant#configure-environment)或[環境變數](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-admin.html?lang=zh-Hant)。

##### 在[!DNL Cloud Console]中設定管理員URL

1. 登入[雲端主控台](https://console.adobecommerce.com/)。
2. 從&#x200B;**[!UICONTROL All projects]**&#x200B;清單中選取專案。
3. 在專案概述中，選取環境並按一下設定圖示。
4. 選取&#x200B;**[!UICONTROL Variables]**&#x200B;標籤。
5. 按一下&#x200B;**[!UICONTROL Create Variable]** (或編輯現有的`ADMIN_URL`變數（如果有的話）。
6. 輸入下列內容：
   - **變數名稱：** `ADMIN_URL`
   - **值：**&#x200B;您的新管理員路徑（例如，`magento_A8v10`）。

   預設會選取&#x200B;**[!UICONTROL Available during runtime]**&#x200B;和&#x200B;**[!UICONTROL Make inheritable]**。 若要防止子環境繼承此值，請清除此變數的&#x200B;**[!UICONTROL Make inheritable]**。
7. 按一下&#x200B;**[!UICONTROL Create variable]** （或&#x200B;**[!UICONTROL Save]**）並等候部署完成。 只有當必填欄位包含值時，才會顯示按鈕。

##### 當[!DNL Cloud Console]中無法使用測試和生產時

[提交支援票證](https://experienceleague.adobe.com/zh-hant/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)，要求為您的測試或生產環境新增`ADMIN_URL`變數。 如果可從[!DNL Cloud Console]存取暫存和生產環境，請依照[整合環境](#integration-environment)中的說明新增變數。

#### 選項B：使用[!DNL Cloud CLI]變更管理員URL

使用`magento-cloud variable:update`命令更新變數。 （`variable:set`命令已棄用，無法使用。）

下列範例將`master`環境`ADMIN_URL`更新為`newAdmin_A8v10`並防止子環境繼承值：

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master --inheritable false
```

- **重新部署：**&#x200B;變更[!DNL Cloud CLI]中的`ADMIN_URL`變數會觸發環境重新部署。
- **繼承：**&#x200B;變數預設為可繼承。 若要防止子環境繼承值，請使用`--inheritable false`選項，如下所示。 如需詳細資訊，請參閱[變數層級可見度](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/variable-levels.html?lang=zh-Hant#visibility)。

>[!NOTE]
>
>`ADMIN_URL`值接受字母(a-z、A-Z)、數字(0-9)和底線字元(_)。 不接受空格或其他字元。
