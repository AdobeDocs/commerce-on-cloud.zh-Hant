---
title: 自訂VCL以略過Fastly快取
description: 建立自訂VCL程式碼片段來略過Fastly快取，以疑難排解到原始伺服器的請求流量。
feature: Cloud, Configuration, Cache
exl-id: 4e19d6d4-b5a1-4623-b0be-804ddc81ff3d
source-git-commit: d08ef7d46e3b94ae54ee99aa63de1b267f4e94a0
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# 自訂VCL以略過Fastly快取

您可以建立自訂VCL程式碼片段，略過Fastly快取，這樣您就可以疑難排解原始伺服器的請求流量。 例如，您可以建立程式碼片段來判斷網站問題是否由快取引起，或是為了疑難排解標題。

您可以設定此程式碼片段，略過Fastly快取來自特定IP位址或URL的請求。

>[!NOTE]
>
>將自訂VCL組態合併到生產環境之前，請務必在中繼環境中測試程式碼。

**必要條件：**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**若要根據IP位址或URL略過Fastly快取**：

{{admin-login-step}}

1. 按一下&#x200B;**商店** >設定> **組態** > **進階** > **系統**。

1. 展開&#x200B;**完整頁面快取** > **Fastly組態** > **自訂VCL程式碼片段**。

1. 按一下&#x200B;**建立自訂程式碼片段**。

1. 新增VCL程式碼片段值：

   - **名稱** — `bypass_fastly`

   - **型別** — `recv`

   - **優先順序** — `5`

   - **VCL**&#x200B;程式碼片段內容 — 

     以下範例略過Fastly尋找特定的IP位址：

     ```conf
     if (client.ip == "<Your IPv4 IP address>" || client.ip == "<Your IPv6 IP address>") {
       return(pass);
     }
     ```

     以下範例為特定URL模式略過Fastly：

     ```conf
     if (req.url ~ "/media/feeds/GoogleShoppingHiVisNew.xml") {  return (pass);}
     ```

     若要取得完全相符的URL，請使用`==`運運算元，而不是`~`運運算元。 如需詳細資訊，請參閱[Fastly VCL參考]。

1. 按一下&#x200B;**建立**。

   ![建立Fastly略過VCL程式碼片段](/help/assets/cdn/fastly-create-bypass-snippet.png)

1. 頁面重新載入後，在&#x200B;**Fastly組態**&#x200B;區段中按一下&#x200B;*上傳VCL到Fastly*。

1. 上傳完成後，請根據頁面頂端的通知重新整理快取。

   Fastly會在上傳過程中驗證更新的VCL版本。 如果驗證失敗，請編輯您的自訂VCL程式碼片段以修正任何問題。 然後，再次上傳VCL。

新增VCL程式碼片段後，您可以使用cURL命令，從指定的IP位址或URL將請求提交至原始伺服器，如下列範例所示：

```bash
curl -svo /dev/null www.example.com/index.html
```

然後，檢查回應以疑難排解未快取內容的問題。

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}

<!--External link definitions-->

[Fastly VCL參照]: https://docs.fastly.com/vcl/

<!-- Last updated from includes: 2025-01-27 17:16:28 -->
