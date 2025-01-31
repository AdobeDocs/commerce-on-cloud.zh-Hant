---
source-git-commit: 0df07e865c3c4fc4ac14483972643eafa8814726
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# 包含用於刪除Fastly自訂VCL的檔案

## 刪除自訂VCL片段

1. [登入](/help/get-started/onboarding.md#access-your-admin-panel)管理員。

1. 按一下&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly組態** > **自訂VCL程式碼片段**。

   ![管理自訂VCL程式碼片段](/help/assets/cdn/fastly-manage-snippets.png)

1. 在&#x200B;_動作_&#x200B;欄中，按一下要刪除的程式碼片段旁的垃圾桶圖示。

1. 在下一個模型視窗中，按一下&#x200B;**DELETE**&#x200B;並啟動新版本。

>[!WARNING]
>
>_自訂VCL程式碼片段_ UI選項只會顯示透過Adobe Commerce管理員新增的程式碼片段。 如果您使用Fastly API新增程式碼片段，請使用該API來[管理它們](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api)。
