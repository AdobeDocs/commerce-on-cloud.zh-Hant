---
title: 測試和生產測試
description: 瞭解如何在測試環境和生產環境中測試。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# 測試和生產測試

將程式碼、檔案和資料成功移轉至測試或生產環境後，請使用環境URL來測試您的網站和存放區。 以下提供有關驗證記錄、測試Fastly設定、使用者驗收測試(UAT)等的資訊。

{{second-staging}}

## 記錄檔

如果您在部署上遇到錯誤，或在測試時遇到其他問題，請檢視記錄檔。 記錄檔位於`var/log`目錄下。

部署記錄檔在`/var/log/platform/<prodject-ID>/deploy.log`中。 `<project-ID>`的值取決於專案ID，以及環境是中繼環境還是生產環境。 例如，專案識別碼為`yw1unoukjcawe`時，暫存使用者為`yw1unoukjcawe_stg`，生產使用者為`yw1unoukjcawe`。

在生產或中繼環境中存取記錄時，請使用SSH登入三個節點中的每一個以找出記錄。 或者，您可以使用[New Relic記錄管理](../monitor/log-management.md)檢視及查詢所有節點的彙總記錄資料。 檢視[檢視記錄](log-locations.md#application-logs)。

## 檢查程式碼基底

確認您的程式碼庫已正確部署到中繼和生產環境。 環境應具有相同的程式碼基底。

## 驗證組態設定

透過「管理員」面板檢查組態設定，包括基本URL、基本管理員URL、多網站設定等。 如果您必須進行任何其他變更，請在本機Git分支中完成編輯，然後推送至「整合」、「測試」和「生產」中的`master`分支。

## 檢查Fastly快取

[設定Fastly](../cdn/fastly-configuration.md)需要注意詳細資訊：使用正確的Fastly服務ID和Fastly API權杖認證、上傳Fastly VCL代碼、更新DNS設定，以及將SSL/TLS憑證套用至您的環境。 完成這些設定任務後，您可以驗證中繼和生產環境上的Fastly快取。

**驗證Fastly服務組態**：

1. 使用包含`/admin`的URL或[更新的管理員URL](../environment/variables-admin.md#admin-url)，登入Admin for Staging and Production。

1. 瀏覽至&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。 捲動並按一下&#x200B;**整頁快取**。

1. 請確定&#x200B;**快取應用程式**&#x200B;值設定為&#x200B;_Fastly CDN_。

1. 測試Fastly認證。

   - 按一下&#x200B;**Fastly組態**。

   - 驗證Fastly服務ID和Fastly API權杖憑證的值。 檢視[取得Fastly認證](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials)。

   - 按一下&#x200B;**測試認證**。

   >[!WARNING]
   >
   >請確定您在測試環境和生產環境中輸入了正確的Fastly服務ID和API權杖。 Fastly憑證會根據服務環境建立和對應。 如果您在生產環境中輸入測試認證，則無法上傳VCL代碼片段、快取無法正常運作，且快取設定指向錯誤的伺服器和存放區。

**若要檢查Fastly快取行為**：

1. 使用`dig`命令列公用程式檢查標頭，以取得有關站台組態的資訊。

   您可以使用任何具有`dig`命令的URL。 下列範例使用Pro URL：

   - 正在暫存： `dig https://mcstaging.<your-domain>.com`
   - 生產： `dig https://mcprod.<your-domain>.com`

   如需其他`dig`測試，請參閱Fastly的[測試，然後再變更DNS](https://docs.fastly.com/en/guides/working-with-domains)。

1. 使用`cURL`驗證回應標頭資訊。

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   請參閱[檢查回應標頭](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers)，以取得有關驗證標頭的詳細資訊。

1. 在您上線後，請使用`cURL`檢查您的上線網站。

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## 完成UAT測試

在中繼和生產環境中完成使用者驗收測試(UAT)。 以下測試是可能的任務和區域快速清單，以作為商家和客戶進行測試。 您的清單可能會更長，且包含自訂模組、擴充功能和協力廠商整合的其他測試。 測試時，請使用桌上型電腦、筆記型電腦和行動裝置。

如果您遇到問題，請儲存您的重製步驟、錯誤訊息、奇怪的熒幕擷取和連結。 使用此資訊來調查和修正整合環境程式碼和設定或環境設定中的問題。

<table>
<tr>
<td style="width:150px">使用者管理</td>
<td>
<ul>
<li>建立和編輯客戶帳戶，驗證電子郵件</li>
<li>為商家建立管理員角色</li>
<li>建立具有特定角色的商家帳戶</li>
<li>測試每個角色的商家帳戶存取權</li>
</ul>
</td>
</tr>
<tr>
<td>目錄與產品</td>
<td>
<ul>
<li>建立具有關聯產品的目錄</li>
<li>為您的店面建立產品，包括所有產品型別：簡單、可設定、套件式</li>
<li>新增產品影像、色票、影片和其他媒體選項</li>
<li>設定價格、折扣、訂價規則 </li>
<li>設定進階功能，包括價格範圍、精選產品、推出日期</li>
<li>修改存貨並驗證每增加一次和完成一次購買時顯示和變更的正確值</li>
</ul>
</td>
</tr>
<tr>
<td>購物車與結帳</td>
<td>
<ul>
<li>搜尋產品並選取篩選選項</li>
<li>從搜尋結果、類別頁面、產品頁面將產品新增到購物車</li>
<li>測試所有產品型別</li>
<li>檢視購物車並透過移除或變更金額來修改內容 </li>
<li>透過結帳，根據購物車及產品資訊確認訂單金額</li>
<li>確認已正確計算購物車的稅捐</li>
<li>使用不同的選項完成購買：新增優惠券、選取送貨、輸入送貨與帳單資訊，以及付款資訊</li>
<li>結帳時驗證付款閘道和選項</li>
<li>檢查熒幕通知、客戶帳戶中列出的訂單及電子郵件通知</li>
<li>測試來賓和客戶結帳</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>建立客戶的訂單</li>
<li>搜尋並檢視訂單</li>
<li>新增與移除產品、變更金額、修改出貨與帳單資訊，以修改訂單</li>
<li>處理退款</li>
<li>取消訂單</li>
<li>套用優惠券代碼和折扣</li>
</ul>
</td>
</tr>
<tr>
<td>網站內容</td>
<td>
<ul>
<li>檢查所有主題和資產是否正確載入</li>
<li>驗證CSS是否正確顯示，包括回應式媒體大小</li>
<li>檢視條款與條件、退款政策及其他政策資訊</li>
<li>檢查連絡資訊、連結，以及您公司的更多資訊</li>
<li>搜尋產品和內容，檢查篩選結果</li>
<li>驗證頁尾區塊和頂端導覽區塊</li>
<li>測試404和維護頁面</li>
</ul>
</td>
</tr>
<tr>
<td>擴充功能</td>
<td>
<ul>
<li>驗證所有擴充功能設定，尤其是任何稅捐、出貨及付款模組（例如：傳送至倉儲及財務管理系統的訂單）</li>
<li>測試所有自訂模組及已安裝的擴充功能互動</li>
<li>檢查應完成之任何互動的資料（付款、訂單、電子郵件通知）</li>
<li>針對您的擴充功能檢查每個環境的設定</li>
<li>驗證模組與擴充功能之間的相依性</li>
<li>以商家和客戶的身分檢查所有動作</li>
</ul>
</td>
</tr>
<tr>
<td>協力廠商整合</td>
<td>
<ul>
<li>確認資料已正確儲存至Adobe Commerce中，並且可由協力廠商服務存取或進行匯出、推播作業（例如：訂單顯示在協力廠商訂單管理系統中）</li>
<li>驗證每次整合的任何設定和互動</li>
<li>執行源自Adobe Commerce和您的第三方服務的來回測試</li>
<li>確認驗證完成</li>
<li>檢查是否有任何記錄問題，以更新控制面板中的程式碼整合或錯誤訊息</li>
</ul>
</td>
</tr>
<tr>
<td>後端測試</td>
<td>
<ul>
<li>測試並清除快取 </li>
<li>執行重新索引並驗證結果</li>
<li>檢查cron工作，檢查是否有任何cron_schedule錯誤</li>
<li>驗證並檢查是否有任何Shell指令碼問題</li>
<li>檢查是否有任何記錄的問題：應用程式記錄、PHP記錄、MySQL記錄、電子郵件記錄</li>
</ul>
</td>
</tr>
</table>

## 負載與壓力測試

在啟動之前，最好在測試環境和生產環境中執行廣泛的流量和效能測試。 考慮對前端和後端流程進行效能測試。

開始測試之前，請輸入票證並附上支援建議，告知您正在測試的環境、您使用的工具以及時間範圍。 使用結果和資訊更新票證以追蹤效能。 當您完成測試時，請新增更新後的結果，並在票證測試中註明完成並附上日期和時間戳記。

檢閱[Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)選項，作為啟動前整備程式的一部分。

為達到最佳效果，請使用下列工具：

- [應用程式效能測試](../environment/variables-post-deploy.md#ttfb_tested_pages) — 設定`TTFB_TESTED_PAGES`環境變數以測試網站回應時間，以測試應用程式效能。
- [圍攻](https://www.joedog.org/siege-home/) — 流量塑造和測試軟體以將您的商店推到極限。 使用可設定的模擬使用者端數目點選您的網站。 圍困支援基本驗證、Cookie、HTTP、HTTPS和FTP通訊協定。
- [Jmeter](https://jmeter.apache.org) — 優異的負載測試，可協助評估尖峰流量的效能，如快閃銷售。 建立針對您的網站執行的自訂測試。
- [New Relic](../monitor/new-relic-service.md) （已提供） — 透過每個動作（例如傳輸資料、查詢、Redis等）的追蹤逗留時間，協助找出造成效能緩慢的網站程式與區域。
- [WebPageTest](https://www.webpagetest.org)與[Pingdom](https://www.pingdom.com) — 即時分析不同來源位置的網站頁面載入時間。 Pingdom可能需要付費。 WebPageTest是免費工具。

## 功能測試

您可以使用Magento功能測試架構(MFTF)從Cloud Docker環境完成Adobe Commerce的功能測試。 請參閱&#x200B;_Commerce適用的Cloud Docker指南_&#x200B;中的[應用程式測試](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/)。

## 設定安全性掃描工具

您的網站有免費的安全性掃描工具。 若要新增網站並執行工具，請參閱[安全性掃描工具](../launch/overview.md#set-up-the-security-scan-tool)。
