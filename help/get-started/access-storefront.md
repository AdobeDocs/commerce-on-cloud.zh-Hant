---
title: 存取您的Commerce管理面板
description: 瞭解如何存取您的Commerce管理面板。
recommendations: noDisplay, catalog
exl-id: 827417b0-9048-44d8-8c82-07befba476c7
TQID: https://experienceleague.adobe.com/V3BXuCc9aqT5YuyIS8WAZgUdPAYNhQunAgg2i2FCaOs
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 361
ht-degree: 0%

---

# 存取您的Commerce管理面板

擁有Commerce管理員面板管理存取權的使用者可以新增使用者、設定商店服務、完成商店設定和自訂工作等。

若為新專案，收到歡迎電子郵件後的第一個步驟是變更授權擁有者帳戶的密碼，以保護管理員對專案的存取權。 此帳戶的預設使用者名稱是授權擁有者的電子郵件地址。

您可以使用下列其中一種方法來提交密碼變更請求：

- 找到傳送至授權擁有者電子郵件地址的歡迎電子郵件，然後按照連結變更您的密碼。

- 將商店URL從[[!DNL Cloud Console]](../cloud-guide/project/overview.md)複製到瀏覽器。 然後，將`/admin`附加到URL的結尾以開啟登入頁面。 按一下&#x200B;**忘記密碼？** 將密碼變更請求傳送到授權擁有者電子郵件地址的連結。

提交密碼變更請求後，請檢視電子郵件中的密碼重設通知。 如果您沒有收到電子郵件，請檢查您的垃圾郵件資料夾。

>[!TIP]
>
>如果密碼重設失敗或您無法登入「管理員」面板，具有管理員存取許可權的使用者可以使用SSH連線至專案，並使用`admin:user:create` CLI命令新增管理員使用者。 請參閱&#x200B;_安裝指南_&#x200B;中的[建立、編輯或解除鎖定系統管理員帳戶](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html)。

## 監視網站健康狀況

[全網站分析工具](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro)是主動式自助服務工具和中央存放庫，其中包含詳細的系統分析和建議，以確保您的Adobe Commerce安裝的安全性和可操作性。 它提供全天候的即時效能監控、報告和建議，以找出潛在問題，並更清楚地瞭解網站健康狀況、安全性和應用程式設定。 這有助於縮短解決時間，並改善網站穩定性與效能。 您可以直接從[管理面板](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel)存取全網站分析工具。
