---
title: 自訂錯誤和維護頁面
description: 瞭解如何自訂在對Fastly原始伺服器的請求失敗時顯示的預設錯誤頁面。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# 自訂錯誤和維護頁面

當對Fastly來源的要求失敗時，Fastly會傳回預設回應頁面，其中包含基本格式和一般訊息，可能會讓使用者感到困惑。 例如，當對Fastly來源的要求因503錯誤而失敗時，Fastly會傳回以下預設錯誤頁面。

![Fastly預設錯誤頁面](../../assets/cdn/fastly-503-example.png)

您可以更新Adobe Commerce存放區設定，以擁有更友善訊息和改善HTML樣式的頁面取代某些預設回應頁面，如下列範例所示。

![Fastly自訂錯誤頁面](../../assets/cdn/fastly-new-error-page.png)

目前，您可以在雲端基礎結構專案上為Adobe Commerce自訂以下Fastly回應頁面。

- [伺服器錯誤 — 內部伺服器錯誤、逾時或網站維護中斷（錯誤代碼500或以上）](#customize-the-503-error-page)
- [WAF封鎖當WAF偵測到可疑請求流量時發生的事件（403禁止）](#customize-the-waf-error-page)

**HTML編碼需求：**

自訂頁面的HTML程式碼必須符合下列要求：

- 內容最多可包含65,535個字元。
- 指定HTML來源中的所有CSS內嵌。
- 使用base64在HTML頁面中捆綁影像，以便即使Fastly離線也能顯示。 檢視css-tricks網站](https://css-tricks.com/data-uris/)上的[資料URI。

## 自訂503錯誤頁面

客戶會在下列情況下看到預設503錯誤頁面：

- 當對Fastly來源的請求傳回大於500的回應狀態時
- 當Fastly來源關閉時，例如逾時、維護活動或健康問題

您可以調整下列HTML代碼以包含符合Adobe Commerce商店主題的樣式，並視需要修改標題和訊息，藉此自訂預設頁面。

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8">
         <title>503</title>
   </head>
   <body>
      <p>Service unavailable</p>
   </body></html>
```

驗證已修改的來源在瀏覽器中是否正確顯示。 然後，將自訂HTML程式碼新增到Fastly設定。

將自訂回應頁面新增到Fastly設定：

{{admin-login-step}}

1. 選取&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。

1. 在右窗格中，展開&#x200B;**完整頁面快取** > **Fastly設定** > **自訂合成頁面**。

   ![編輯503錯誤頁面](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. 選取&#x200B;**設定HTML**。

1. 將自訂回應頁面的原始程式碼複製並貼到HTML欄位中。

   ![更新503錯誤頁面](../../assets/cdn/fastly-customize-503-response.png)

1. 選取頁面頂端的&#x200B;**上傳**，將自訂HTML來源上傳至Fastly伺服器。

1. 選取頁面頂端的&#x200B;**儲存組態**&#x200B;以儲存更新的組態檔。

1. 重新整理快取。

   - 在頁面頂端的通知中，選取&#x200B;*快取管理*&#x200B;連結。

   - 在快取管理頁面上，選取&#x200B;**排清Magento快取**。

## 自訂WAF錯誤頁面

當對Fastly來源的要求失敗並因為[WAF](fastly-waf-service.md)封鎖事件所導致的`403 Forbidden`錯誤時，客戶會看到以下預設WAF錯誤頁面。

![WAF錯誤頁面](../../assets/cdn/fastly-waf-403-error.png)

下列程式碼範例顯示預設頁面的HTML來源：

```html
<html>
  <head>
    <title>Magento 403 Forbidden</title>
  </head>
  <body>
    <p>The requested URL was rejected.</p>
    <p>For additional information, please contact support and provide this reference ID:</p>
    <p>"} req.http.x-request-id {"</p>
    <p><button onclick='history.back();'>Go Back</button></p>
  </body>
</html>
```

您可以使用Fastly設定功能表中的&#x200B;**自訂綜合頁面** > **編輯WAF頁面**&#x200B;選項，自訂雲端基礎結構專案上Adobe Commerce的預設程式碼。 編輯程式碼時，請保留下列提供WAF封鎖事件參考ID的代碼行：

```html
<p>"} req.http.x-request-id {"</p>
```

>[!NOTE]
>
>「編輯WAF」選項只有在雲端基礎結構專案上為您的Adobe Commerce啟用「受管理的Cloud WAF」服務時才能使用。

**若要編輯WAF錯誤頁面**：

1. [登入管理員](../../get-started/onboarding.md#access-your-admin-panel)。

1. 選取&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。

1. 在右窗格中，展開&#x200B;**完整頁面快取** > **Fastly設定** > **自訂合成頁面**。

   ![編輯WAF錯誤頁面選項](../../assets/cdn/fastly-custom-synthetic-pages-edit-waf.png)

1. 選取&#x200B;**編輯WAF頁面**。

1. 填寫欄位以更新HTML。

   ![更新WAF錯誤頁面](../../assets/cdn/fastly-edit-waf-html.png)

   - **狀態** — 選取`403 Forbidden`狀態。
   - **MIME型別** — 型別`text/html`。
   - **內容** — 編輯預設HTML回應以新增自訂CSS，並視需要更新標題和訊息。

1. 選取頁面頂端的&#x200B;**上傳**，將自訂HTML來源上傳至Fastly伺服器。

1. 選取頁面頂端的&#x200B;**儲存組態**&#x200B;以儲存更新的組態檔。

1. 重新整理快取。

   - 在頁面頂端的通知中，選取&#x200B;**快取管理**&#x200B;連結。

   - 在快取管理頁面上，選取&#x200B;**排清Magento快取**。

## 顯示錯誤報告編號

依預設，Fastly會隱藏&#x200B;*503服務無法使用*&#x200B;錯誤背後的所有Adobe Commerce錯誤。 若要顯示錯誤記錄報告編號，以便找到並檢閱記錄中的錯誤詳細資料，請使用以下步驟開啟省略Fastly的網站：

1. 擷取存放區的IP位址：

   - 對於Pro測試和生產環境：

     ```bash
     nslookup {your_project_id}.ent.magento.cloud
     ```

   - 針對專業整合環境和入門環境：

     ```bash
     nslookup gw.{your_region}.magentosite.cloud
     ```

1. 將應用程式網域和IP位址新增至本機工作站的主機檔案：

   ```text
   {server_IP} {store_domain}
   ```

1. 清除瀏覽器快取和Cookie （或切換到無痕模式）。

1. 再次開啟您的商店網站以檢視錯誤代碼。

1. 使用錯誤碼在錯誤報告檔案中尋找詳細資訊：

   - [使用SSH連線至受影響的環境](../development/secure-connections.md#connect-to-a-remote-environment)

   - 找到`./var/report/{error_number}`檔案。
