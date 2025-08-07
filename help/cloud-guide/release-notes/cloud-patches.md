---
title: Commerce雲端修補程式
description: 請參閱「雲端修補程式」套裝軟體的最新改良專案清單。
recommendations: noDisplay, catalog
last-substantial-update: 2025-08-07T00:00:00Z
exl-id: a4454ebc-72a4-42c1-b591-6237c97fe913
source-git-commit: b90959335c91dd0631d270ebb522524cf1db6ff0
workflow-type: tm+mt
source-wordcount: '2486'
ht-degree: 0%

---

# Commerce雲端修補程式

[雲端修補程式](https://github.com/magento/magento-cloud-patches)套件提供一組必要的修補程式，可改善所有Adobe Commerce版本與雲端環境的整合，並支援快速傳送重要修正。

Commerce套件的雲端修補程式相依於ECE-Tools套件，會在您安裝或更新ECE-Tools套件時安裝與更新。 您也可以使用和管理Commerce的雲端修補程式做為獨立的套件，將修補程式套用至不在雲端平台上的Adobe Commerce專案。 以下發行說明說明說明此套裝軟體的最新改善。

>[!TIP]
>
>為確保您的專案具有所有必要的修補程式，請更新至[最新版本的ece-tools](../dev-tools/update-package.md)。

>[!NOTE]
>
>如需將修補程式套用至專案的指示，請參閱[套用修補程式](../development/apply-patches.md)。

`magento/magento-cloud-patches`封裝使用以下版本順序： `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.1.10 {#latest}

發行日期： 2025年8月7日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增功能測試。<!-- MCLOUD-13312 -->

## v1.1.9

發行日期： 2025年6月9日

- ![修正圖示](../../assets/fix.svg) **已改善的類別檢視** — 已改善類別檢視CVE-2025-47109<!-- MCLOUD-13752	 - -->
- ![修正圖示](../../assets/fix.svg) **已改善管理快取**-Improved-Admin-cache-efficiency CVE-2025-47110<!-- MCLOUD-13753	 - -->

## v1.1.8

發行日期： 2025年6月3日

- ![修正圖示](../../assets/fix.svg) **改善與2.4.8的相容性** — 更新協力廠商程式庫的相容性，以與2.4.8<!-- MCLOUD-13707	 - -->更相容

## v1.1.7

發行日期： 2025年5月5日

- ![新圖示](../../assets/new.svg) **已將Commerce 2.4.4的修補程式更新至2.4.8** — 這是[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/increased-execution-time-for-bulk-asynchronous-web-endpoints-post-apsb25-08-security-patch)的更新修補程式，已在1.1.7<!-- MCLOUD-13619 -->中發行

## v1.1.6

發行日期： 2025年4月24日

- ![新圖示](../../assets/new.svg) **已將Commerce 2.4.4的修補程式更新至2.4.7** — 這是[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)的更新修補程式，已在1.1.4<!-- MCLOUD-13240 -->中發行

## v1.1.5

發行日期： 2025年4月15日

- ![新圖示](../../assets/new.svg) **已新增B2B 1.5.2**&#x200B;的修補程式 — 修正問題ACP2E-3833 (含B2B模組1.5.2和MariaDB 10.6<!-- MCLOUD-13605	-->

## v1.1.4

發行日期： 2025年2月13日

- ![新圖示](../../assets/new.svg) **已新增Commerce 2.4.4至2.4.7**&#x200B;的修補程式 — 此更新修補程式[CVE-2025-24434](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb25-08)。<!-- MCLOUD-13240	 - -->

## v1.1.3

發行日期： 2025年2月6日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增對PHP 8.4的支援。<!-- MCLOUD-13149	 - -->

## v1.1.2

發行日期： 2024年11月5日

- ![修正圖示](../../assets/fix.svg) **已新增Commerce 2.4.4至2.4.7**&#x200B;的修補程式 — 此更新修正使用B2B模組時Adobe Commerce的嚴重[CVE-2024-45115](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-73)漏洞。<!-- MCLOUD-12980 - -->

## v1.1.1

發行日期： 2024年11月5日

- ![修正圖示](../../assets/fix.svg) **已新增Commerce 2.4.4至2.4.7**&#x200B;的修補程式 — 此更新可修補嚴重的[CVE-2024-34102](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/security-update-available-for-adobe-commerce-apsb24-40-revised-to-include-isolated-patch-for-cve-2024-34102?lang=en) CosmicSting漏洞。<!-- MCLOUD-12980 - -->

## v1.1.0

發行日期： 2024年10月7日

- ![修正圖示](../../assets/fix.svg) **重構的程式碼** — 已移除對舊PHP版本(7.4、7.3、7.2)及相關程式庫的支援。<!-- MCLOUD-9278 - -->
- ![修正圖示](../../assets/fix.svg) **升級的Monolog版本** — 新增對monolog 3.6的支援。<!-- MCLOUD-12855 - -->
- ![修正圖示](../../assets/fix.svg) **應用程式伺服器的修補程式** — 解決GraphQL應用程式伺服器的已知問題。 具體而言，版本2.4.7中的`CatalogGraphQl\\Model\\Config\\AttributeReader`包含錯誤，可能會導致GraphQL要求根據過時的屬性設定擷取回應。<!-- ACPT-1876 -->

## v1.0.27

發行日期： 2024年5月21日

- **支援PHP 8.3** — 此修補程式解決了php 8.3與Composer套件版本之間的相容性錯誤。

## v1.0.26

發行日期： 2024年4月8日

- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.3的支援。

## v1.0.25

發行日期： 2024年1月16日

- **快取改善** — 此修補程式增強配置快取效率，大幅降低Adobe Commerce 2.4.4和更新版本的記憶體使用量。<!-- MCLOUD-11514 -->
- **CRON工作改善** — 此修補程式修正了遺失工作不必要等候Adobe Commerce 2.4.4版或更新版本的cron工作鎖定的問題。<!-- MCLOUD-11329 -->

## v1.0.24

發行日期： 2023年9月15日

- **效能改善** — 此修補程式將Adobe Commerce 2.4.6的相同部署組態載入次數減少至2.4.6-p1<!-- MCLOUD-10604 -->，藉此修正影響效能的問題

## v1.0.23

發行日期： 2023年7月31日

- **已移除修補程式MCLOUD-10604** — 此修補程式已移至QPT。<!-- MCLOUD-10736 -->

## v1.0.22

發行日期： 2023年6月19日

- **增強型QPT CLI精靈/輸出** — 在QPT CLI精靈/輸出中新增警告，提醒您確認是否有相依性的修補程式詳細資料和需求。<!-- ACP2E-1963 -->
- **已新增Commerce 2.4.6的修補程式：**
   - 修正`regexp cache tag`驗證。<!-- MCLOUD-10226 -->
   - 透過減少相同部署組態載入的次數來改善效能。<!-- MCLOUD-10604 -->
- **已新增Commerce 2.3.7至2.4.6**&#x200B;的修補程式 — 修正`catalog_product_entity_*`資料表的隨機值增加，而非1增加的問題。<!-- MCLOUD-10032 -->
- **已新增Commerce 2.4.0至2.4.6**&#x200B;的修補程式 — 修正從Admin排清JS/CSS快取時發生的`The file can't be deleted. Warning!unlink: No such file or directory`錯誤。<!-- MCLOUD-10279 -->

## v1.0.21

發行日期： 2023年3月10日

- **PHP 8.2**&#x200B;的增強支援 — 修正某些PHP 8.2.x版本的相容性問題，以支援Commerce 2.4.6。

## v1.0.20

發行日期： 2022年10月27日

- **新增L2快取改善修補程式** — 此修補程式修正了為Commerce 2.4.0版和2.4.1版排清本機L2快取的問題。<!-- MCLOUD-7845 -->

## v1.0.19

發行日期： 2022年9月13日

- **PHP 8.1**&#x200B;的增強支援 — 修正某些PHP 8.1.x版本的相容性問題。

## v1.0.18

發行日期： 2022年8月11日

Adobe Commerce 2.4.5的重要修補程式：

- **使用Braintree付款的訂單問題** — 此修補程式解決管理員無法下新訂單或重新訂購的重大問題。<!-- MCLOUD-9137 -->

請參閱[啟用Braintree付款時，管理員無法建立訂單/重新排序](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html)。

## v1.0.17

發行日期： 2022年5月24日

修正`patches.json`檔案中安全性修補程式的限制。

## v1.0.16

發行日期： 2022年3月31日

Adobe Commerce 2.3.3-p1及更高版本的重要修補程式：

已更新修補程式，以解決導致未驗證遠端程式碼執行的&#x200B;**嚴重**&#x200B;漏洞。<!-- MCLOUD-8479 -->

請參閱[Adobe安全性公告APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html)。

## v1.0.15

發行日期： 2022年3月10日

- **支援PHP 8.1** — 新增對PHP 8.1的支援，並放棄對PHP 7.0和7.1的支援。
- **新增Adobe Commerce 2.3.3**&#x200B;的修補程式 — 修正產品頁面上顯示的貨幣。

## v1.0.14

發行日期： 2022年2月13日

Adobe Commerce 2.3.3-p1及更高版本的重要修補程式：

已新增修補程式，以解決導致未驗證遠端程式碼執行的&#x200B;**嚴重**&#x200B;漏洞。<!-- MCLOUD-8461 -->

請參閱[Adobe安全性公告APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html)。

## v1.0.13

發行日期： 2021年10月25日

- **更新獨白** — 已將`monolog`封裝所需的最低版本更新為`^2.3`。<!-- ACMP-1263 -->
- **不相容的PHP方法** — 修正Adobe Commerce 2.4.3和2.3.7-p1.<!-- AC-384 -->版不相容的PHP方法
- **PHP錯誤** — 修正嘗試套用修補程式時發生的`PHP error 'Undefined variable: errorMessage' ...`錯誤。<!-- ACP2E-138 -->

## v1.0.12

發行日期： 2021年8月12日

Adobe Commerce 2.4.3和2.3.7-p1的關鍵修補程式：

- **API速率限制問題** — 此修補程式更正了預設速率限制，該限制導致Web API無法處理陣列中超過20個專案的請求。 此修補程式會提高速率限制的預設值。 請參閱Adobe Commerce [2.4.3發行說明](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/adobe-commerce/2-4-3#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

發行日期： 2021年7月29日

- **修正套用B2B分層導覽修補程式所造成的問題** — 對於已套用B2B分層導覽修補程式的客戶，此修正會解決在切換商店檢視後顯示在「搜尋」頁面上的`Undefined offset`錯誤。<!--MCLOUD-5287-->

- **Paypal結帳修補程式** — 修正PayPal Express顯示先前訂單價格的Adobe Commerce 2.3.7問題。<!--MC-42674-->

- **修補程式類別支援** — 已新增處理指派給品質修補程式的修補程式類別與來源支援。 類別可讓客戶在使用[品質修補程式工具](https://github.com/magento/quality-patches)和全網站分析工具(SWAT)時，使用篩選器和排序來更快速地尋找修補程式。<!--MC-38577-->

## v1.0.10

發行日期： 2021年5月10日

- **與Adobe Commerce 2.3.7**&#x200B;的相容性 — 解決在Adobe Commerce 2.3.7上安裝的撰寫器相依性衝突。<!--MC-42131-->
- **修正多次套用隨附修補程式所造成的問題** — 多次套用隨附修補程式（包含其他已棄用的修補程式）可能會回覆隨附的已棄用套件。 所有修補程式現在只會套用一次。 嘗試再次套用相同的套件時，會顯示已套用修補程式的訊息。<!--MC-41912-->
- **B2B階層式導覽修補程式** — 修正當使用者啟用B2B共用目錄時，階層式導覽無法顯示所有產品選項的另一個問題。<!--MCLOUD-7742-->

## v1.0.9

發行日期： 2021年2月1日

- **B2B階層式導覽修補程式** — 修正啟用B2B共用目錄時，階層式導覽無法顯示所有產品選項的問題。<!--MCLOUD-6923-->
- **與PHP 7.4**&#x200B;的相容性 — 修正PHP 7.4的雲端修補程式相容性問題。<!--MCLOUD-7367-->
- **已棄用的修補程式會變為可見** — 修正了雲端修補程式的問題，此問題導致在套用包含已棄用修補程式全部內容的取代修補程式後，已棄用的修補程式會顯示在修補程式表格中。 如果套用的修補程式結合了數個其他修補程式，就可能發生這種情況。<!--MC-40626-->
- **套用修補程式時發生無訊息失敗** — 修正`git apply`命令在某些環境中無法無訊息套用修補程式的雲端修補程式問題。<!--MC-40529-->

## v1.0.8

發行日期： 2020年10月14日

- **magento/magento-cloud-patches的相容性更新** — 更新`symfony`檔案中的`semver`和`composer.json`版本限制，以便與Adobe Commerce 2.4.1和更新版本相容。<!--MCLOUD-7111-->

## v1.0.7

發行日期： 2020年10月14日

- **適用於Adobe Commerce 2.3.0的Redis修補程式至2.3.5、2.4.0** — 已更新Redis修補程式，以支援在實作2級快取時將產品新增至類別。<!--MCLOUD-6659-->

- **Braintree VBE修補程式** — 修正管理員嘗試檢視Braintree結算報告時發生錯誤的問題。<!--MCLOUD-6684-->

- 現在，如果主機系統上沒有Git，則`ece-patches apply`命令會使用Unix `patch`命令來套用修補程式。<!--MCLOUD-7069-->

## v1.0.6

發行日期：

- **適用於Adobe Commerce 2.3.0 - 2.3.4**&#x200B;的Redis修補程式 — 最佳化通訊並改善效能
   - 縮小Redis和Adobe Commerce之間的網路傳輸規模
   - 修正Redis載入和寫入作業的競爭條件
   - 重寫基底快取配接器以處理儲存時的錯誤
   - 減少Redis CPU耗用量<!--MCLOUD-6139-->

- **適用於Adobe Commerce 2.3.0 - 2.3.5**&#x200B;的Redis修補程式 — 改善效能並修正錯誤
   - 修正快取鎖定實作以防止無限鎖定
   - 改善目前的鎖定機制
   - 實作已簽署的鎖定以防止解除鎖定並行請求
   - 修正Redis寫入作業中發生的下列錯誤： `OOM command not allowed when used memory > maxmemory`
   - 修正產品更新期間所執行`cat_p`標籤清除快取的處理<!--MCLOUD-6110-->

- 已修正在使用Adobe Commerce v2.2.6或2.3.5 （不包括此模組）的雲端基礎結構專案上，將必要的`amzn/amazon-pay-module`修補程式套用至Adobe Commerce時導致錯誤的問題。 現在，如果未安裝模組，修補程式會略過`amzn/amazon-pay-module`修補程式。<!--MCLOUD-6588-->

## v1.0.5

發行日期： 2020年6月26日

- **Redis效能改善** — 將Redis最佳化功能新增至Adobe Commerce 2.3.3和2.3.4版。這些修正包含在Adobe Commerce 2.3.5版本<!--MCLOUD-5771-->中。

- **New Relic記錄擴充器** — 新增必要的Monolog ProcessorInterface，以支援Commerce 1.0.4版雲端元件中引進的New Relic記錄功能改善。部署Adobe Commerce 2.1.x需要此修補程式。如果未套用修補程式，則建置會在`di:compile`處理序期間失敗。<!--MCLOUD-6029-->

## v1.0.4

發行日期： 2020年5月12日

- **Amazon Pay結帳** — 修正Amazon Pay付款Widget的問題，此問題導致客戶無法在結帳程式期間，於&#x200B;_檢閱與付款_&#x200B;步驟變更付款方式。<!--MCLOUD-5930-->

- **類別頁面上的產品顯示** — 修正產品無法在&#x200B;_顯示所有頁面_&#x200B;檢視的類別頁面上顯示的問題。<!--MCLOUD-5684-->

- **頁面產生器影像上傳** — 修正頁面產生器介面問題，此問題有時會在將影像上傳至影像庫時導致下列錯誤： `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **隱藏不必要的Sitemap產生警告** — 在產生Sitemap期間發生錯誤時新增重試嘗試，並在可自動復原錯誤的情況下略過客戶電子郵件通知。<!--MCLOUD-3025-->

- **網站效能改善** — 修正`Magento\Framework\App\DeploymentConfig\Reader::load`函式的效能問題，該函式會定期經歷影響網站效能的長時間載入。<!--MCLOUD-5650-->

- 更新付款方法修補程式的修補程式指派，以付款模組為目標，而非Magento基本套件(magento/magento2-base)，因此只有在付款模組存在時才套用付款修補程式。<!--MCLOUD-5666-->

- 已更新與Magento Open Source相容的修補程式。<!--MCLOUD-5701-->

## v1.0.3

發行日期： 2020年4月28日

- 已針對「部署期間已停用FPC」修補程式新增修正，以支援Adobe Commerce 2.3.5。

## v1.0.2

發行日期： 2020年2月27日

此版本包含下列修補程式和重要修正：

- **magento/magento-cloud-patches的相容性更新**

   - 已更新`symfony`檔案中的`semver`和`composer.json`版本限制，以便與Adobe Commerce 2.4和更新版本相容。<!--MAGECLOUD-5127-->

   - 已更新`composer.json`中的條件約束，以與`ece-tools` 2002.0.22和更新的2002.0.x版本相容。

- **PayPal Express簽出** — 發佈於2020年2月12日，此修補程式解決了PayPal Express簽出的訂單所遇到的問題，其中訂單的送貨地址指定已手動輸入文字欄位，而非從「送貨」頁面的下拉式功能表中選取的國家區域。 請參閱修正程式下載頁面上的完整修正程式說明。

- **應用程式部署修正** — 新增修補程式，以修正部署過程中停用整頁快取的問題。 此修補程式適用於Adobe Commerce 2.3.2和更新版本。

- 非同步/大量API的&#x200B;**範圍引數** — 更新此修補程式以修正`composer.json`檔案中的語法錯誤。 此修補程式適用於Magento Open Source 2.3.1和2.3.2。請參閱修正程式下載頁面上的完整修正程式說明。

## v1.0.1

發行日期： 2020年2月6日

我們已包含magento/magento-cloud-patches v1.0.1版中軟體下載頁面上的所有Magento Open Source 2.x修補程式。 如果您先前將任何修補程式複製到專案中，請移除它們以避免衝突。

此版本包含下列修補程式和重要修正：

- **修正Cron死結並改善Cron鎖定**—

   - 修正由於`cron_schedule`表格中的狀態值不正確，導致部分cron工作無法執行的問題。 現在，我們使用Adobe Commerce鎖定架構來檢查和更新cron工作狀態，而不是使用`cron_schedule`表格。 已結束並出現錯誤狀態的Cron工作會在下次Cron執行時重試，而不是等待24小時。

   - 新增&#x200B;_重試_&#x200B;作業，以避免更新`cron_schedule`資料表中的資料時發生死結。

- **更新`magento/magento-cloud-patches`以包含Magento Open Source 2.x的所有可用修補程式** — 更新magento/magento-cloud-patches套件以包含軟體下載頁面上可用的所有Magento Open Source 2.x修補程式。 如果您先前在雲端基礎結構專案上將任何Magento Open Source修補程式複製到Adobe Commerce，請移除它們以避免衝突。<!--MAGECLOUD-4606-->

- **Elasticsearch目錄分頁修正** — 以更有效的修正取代magento/magento-cloud-patches v1.0提供的Elasticsearch目錄分頁修補程式。<!--MAGECLOUD-4847-->

- **Page Builder修補程式** — 在Commerce 1.0.0的雲端修補程式中，我們隨附了Page Builder修補程式，以解決已知的Page Builder遠端程式碼執行(RCE)漏洞，並根據Adobe Commerce 2.3.3進行初始修正。我們已更新這些修補程式，並根據Adobe Commerce 2.3.4採用更穩定的實作，其中包括修正問題的多項最佳化。<!--MAGECLOUD-4884-->

  如果您有magento/magento-cloud-patches 1.0.0套件，仍可防止Page Builder RCE弱點問題。 如果您更新至1.0.1或更新版本，相同修正的實作效果會更好。

## v1.0.0

發行日期： 2019年11月14日

這是[`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches)封裝的第一個版本，這是`ece-tools`封裝版本2002.0.22或更新版本的新相依性。

此版本包含下列修補程式和重要修正：

- **2.3.1.x和2.3.2.x版本的Page Builder安全性修補程式** — 修正Page Builder預覽中的問題，該問題允許未經驗證的使用者存取某些範本化方法，這些方法可用來透過網路(RCE)觸發任意程式碼執行，導致全域資訊洩漏。 在Adobe Commerce 2.3.1和2.3.2.<!--MAGECLOUD-4649-->版中使用不支援的頁面產生器版本時，可能會發生此問題

- **MSI修補程式** — 修正使用預設庫存設定來管理庫存時，導致索引錯誤和效能問題的問題。<!--MAGECLOUD-4428-->

- **新郵件介面的回溯相容性** — 修正Adobe Commerce v2.3.3中引入的`Magento\Framework\Mail\EmailMessageInterface` PHP介面所導致的回溯不相容問題。在此修補程式的範圍內，新的`EmailMessageInterface`繼承自舊的`MessageInterface`，Adobe Commerce核心模組將還原為相依於`MessageInterface`。<!--MAGECLOUD-4422-->

- **目錄分頁在Elasticsearch 6.x**&#x200B;上無法運作 — 修正搜尋結果分頁的重要問題，此問題影響使用Elasticsearch 6.x做為目錄搜尋引擎的客戶。<!--MAGECLOUD-4448-->
