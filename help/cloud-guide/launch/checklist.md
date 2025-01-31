---
title: 啟動檢查清單
description: 檢閱網站啟動時的檢查清單專案。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# 啟動檢查清單

在您部署到生產環境之前，請下載[啟動檢查清單](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf)，並搭配這些指示使用，以確認您已完成所有必要的設定和測試。 在[部署您的存放區](../deploy/staging-production.md)，檢視入門和Pro的完整部署程式概觀。

## 在生產環境中完全測試

請參閱[測試部署](../test/staging-and-production.md)，以測試網站、存放區和環境的各個層面。 這些測試包括驗證Fastly、使用者驗收測試(UAT)和效能測試。

## TLS和Fastly

Adobe為每個環境提供Let&#39;s Encrypt SSL/TLS憑證。 Fastly需要此憑證才能透過HTTPS提供安全流量。

若要使用此憑證，您必須更新您的DNS設定，以便Adobe可以完成網域驗證並將憑證套用至您的環境。 每個環境都有獨特的憑證，涵蓋Adobe Commerce在該環境中部署的雲端基礎結構網站上的網域。 我們建議在[Fastly設定程式](../cdn/fastly-configuration.md)期間完成並更新設定。

## 使用生產設定更新DNS設定

當您準備好啟動您的網站時，您必須更新DNS設定，以透過Fastly服務路由來自生產環境的流量。

**必要條件：**

- [在您的開發環境中設定和測試Fastly](../cdn/fastly-configuration.md#)

- 生產環境設定已更新所有必要的網域

  通常您會與客戶技術顧問合作，新增商店所需的所有頂層網域和子網域。 若要新增或變更生產環境的網域，[請提交Adobe Commerce支援票證](https://support.magento.com/hc/en-us/articles/360019088251)。 等候確認您的專案設定已更新。

  在入門專案中，您必須將網域新增到專案。 請參閱[管理網域](../cdn/fastly-custom-cache-configuration.md#manage-domains)。

- 為您的生產環境布建的SSL/TLS憑證。

  如果您在Fastly設定過程中新增了生產網域的ACME挑戰記錄，當您更新DNS設定以將流量路由到Fastly服務時，Adobe會自動將SSL/TLS憑證上傳到您的生產環境。 如果您未預先布建憑證，或您已更新網域，則Adobe必須完成網域驗證並布建憑證，這可能需要12小時的時間。

### 若要更新網站啟動的DNS設定：

1. 更新您生產網站的以下DNS設定：

   - 設定所有必要的重新導向，尤其是當您從現有網站移轉時

   - 設定區域的根資源記錄以處理主機名稱

   - 降低存留時間(TTL)的值以重新整理DNS資訊，將客戶導向正確的生產環境存放區

     建議在切換DNS記錄時大幅降低TTL值。 此值會告訴DNS快取DNS記錄的時間長度。 縮短後，它會更快地重新整理DNS。 例如，您可以在更新網站時，將TTL值從三天變更為10分鐘。 請注意，縮短TTL值會增加DNS基礎結構的負載。 網站啟動後，還原先前較高的值。


1. 新增CNAME記錄以將您生產環境的子網域指向Fastly服務`prod.magentocloud.map.fastly.net`，例如：

   | 網域或子網域 | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. 如果需要，請新增A記錄以將頂點網域(`<domain-name>.com`)對應到以下Fastly IP位址：

   | Apex網域 | 名稱 |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>[RFC1034](https://www.rfc-editor.org/rfc/rfc1912) （**區段2.4**）中的DNS指示指出：
>_CNAME記錄不得與任何其他資料並存。 換言之，如果suzy.podunk.xx是sue.podunk.xx的別名，您就無法同時擁有suzy.podunk.edu的MX記錄、A記錄，甚至TXT記錄。_
>
>因此，子網域的DNS記錄應是型別`CNAME`，Apex網域（根網域）的記錄應是型別`A`。 放棄此規則可能會中斷您的郵件服務或DNS傳播，因為您將失去新增其他記錄（例如MX或NS）的能力。 有些DNS提供者可能會使用內部自訂來避免這種情況，但遵循標準可確保穩定性和靈活性（例如DNS提供者的變更）。

1. 更新基底URL。

   - 使用SSH登入生產環境。

     ```bash
     magento-cloud ssh -e production
     ```

   - 使用CLI變更商店的基本URL。

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **注意**：您也可以從Admin更新基底URL。 請參閱&#x200B;_Adobe Commerce商店與購買體驗指南_&#x200B;中的[商店URL](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)。

1. 請稍候幾分鐘讓網站更新。

1. 測試您的網站。

## 驗證生產設定

進行最後階段以驗證一或多個存放區的生產設定。 您可以在生產環境中更新設定。 如果設定是唯讀的，您可能需要開啟SSH連線並使用CLI命令來變更設定，或在您的本機環境中變更設定。 完成更新後，您可以將變更部署到中繼和生產環境。

以下是建議的變更和檢查：

- [已完成傳出電子郵件的測試](../project/outgoing-emails.md)

- [管理員認證和基本管理員URL的安全設定](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- [最佳化網頁的所有影像](../cdn/fastly-image-optimization.md)

- [檢查HTML、JavaScript和CSS的縮制設定](../deploy/static-content.md)

## 驗證Fastly快取

- 測試並驗證Fastly快取是否可在生產網站上正常運作。 如需詳細的測試和檢查，請參閱[快速測試](../test/staging-and-production.md#check-fastly-caching)。

- [確認您的生產環境中已安裝適用於Commerce的最新版Fastly CDN模組](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [確保已上傳Fastly VCL程式碼的最新版本](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## 效能測試

建議您檢閱[Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit)選項，作為啟動前整備程式的一部分。

您也可以使用下列協力廠商選項進行測試：

- [圍攻](https://www.joedog.org/siege-home/)：流量塑造和測試軟體以將您的存放區推到極限。 使用可設定的模擬使用者端數目點選您的網站。 圍困支援基本驗證、Cookie、HTTP、HTTPS和FTP通訊協定。

- [Jmeter](https://jmeter.apache.org/)：極佳的負載測試，可協助評估尖峰流量的效能，如快閃銷售。 建立針對您的網站執行的自訂測試。

- [New Relic](https://support.newrelic.com/s/) （已提供）：透過每個動作（例如傳輸資料、查詢、Redis等）的追蹤逗留時間，協助找出造成效能緩慢的網站程式與區域。

- [WebPageTest](https://www.webpagetest.org/)和[Pingdom](https://www.pingdom.com/)：即時分析不同來源位置的網站頁面載入時間。 Pingdom可能需要付費。 WebPageTest是免費工具。

## 安全性設定

- [設定您的安全性掃描](overview.md#set-up-the-security-scan-tool)

- [管理員使用者的安全設定](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin)

- 管理員URL的[安全設定](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url)

- [移除雲端基礎結構專案上任何不再使用Adobe Commerce的使用者](../project/user-access.md)

- [設定雙因素驗證](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/)

## 效能監視

您可以使用New Relic服務在Pro和Starter環境中進行效能監視。 在Pro計畫帳戶上，我們提供Adobe Commerce警示原則的「受管理」警示，以使用New Relic APM和基礎架構代理程式來監控應用程式和基礎架構效能。 如需使用這些服務的詳細資訊，請參閱[使用受管理警示監視效能](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)。

### 下一步

[啟動步驟](steps.md)
