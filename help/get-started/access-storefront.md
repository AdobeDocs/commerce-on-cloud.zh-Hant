---
title: 存取您的Commerce管理面板
description: 瞭解如何存取您的Commerce管理面板。
recommendations: noDisplay, catalog
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 0%

---

# 存取您的Commerce管理面板

擁有Commerce管理員面板管理存取權的使用者可以新增使用者、設定商店服務、完成商店設定和自訂工作等。

若為新專案，收到歡迎電子郵件後的第一個步驟是變更授權擁有者帳戶的密碼，以保護管理員對專案的存取權。 此帳戶的預設使用者名稱是授權擁有者的電子郵件地址。

您可以使用下列其中一種方法來提交密碼變更請求：

- 找到傳送至授權擁有者電子郵件地址的歡迎電子郵件，然後按照連結變更您的密碼。

- 將商店URL從[[!DNL Cloud Console]](../cloud-guide/project/overview.md)複製到瀏覽器。 然後，將`/admin`附加到URL的結尾以開啟登入頁面。 按一下&#x200B;**忘記密碼？**&#x200B;連結，可將密碼變更要求傳送至授權擁有者電子郵件地址。

提交密碼變更請求後，請檢視電子郵件中的密碼重設通知。 如果您沒有收到電子郵件，請檢查您的垃圾郵件資料夾。

>[!TIP]
>
>如果密碼重設失敗或您無法登入「管理員」面板，具有管理員存取許可權的使用者可以使用SSH連線至專案，並使用`admin:user:create` CLI命令新增管理員使用者。 請參閱&#x200B;_安裝指南_&#x200B;中的[建立、編輯或解除鎖定系統管理員帳戶](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html?lang=zh-Hant)。

## 監視網站健康狀況

[全網站分析工具](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/site-wide-analysis-tool/intro)是主動式自助服務工具和中央存放庫，其中包含詳細的系統分析和建議，以確保您的Adobe Commerce安裝的安全性和可操作性。 它提供全天候的即時效能監控、報告和建議，以找出潛在問題，並更清楚地瞭解網站健康狀況、安全性和應用程式設定。 這有助於縮短解決時間，並改善網站穩定性與效能。 您可以直接從[管理面板](https://experienceleague.adobe.com/zh-hant/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel)存取全網站分析工具。
