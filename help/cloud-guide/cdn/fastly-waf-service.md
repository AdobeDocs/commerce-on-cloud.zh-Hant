---
title: Web應用程式防火牆(WAF)
description: 瞭解Fastly WAF服務如何偵測、記錄並封鎖惡意請求流量，以免損害Adobe Commerce網路或網站。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '930'
ht-degree: 0%

---

# Web應用程式防火牆(WAF)

雲端基礎結構上適用於Adobe Commerce的網頁應用程式防火牆(WAF)服務由Fastly提供技術支援，會偵測、記錄並封鎖惡意請求流量，以免損害您的網站或網路。 WAF服務僅適用於生產環境。

WAF服務提供下列優點：

- **PCI法規遵循**—WAF啟用可確保生產環境中的Adobe Commerce店面符合PCI DSS 6.6安全性需求。
- **預設WAF原則** — 由Fastly設定和維護的預設WAF原則，提供量身打造的安全性規則集合，可保護您的Adobe Commerce Web應用程式免受各種攻擊，包括插入攻擊、惡意輸入、跨網站指令碼、資料匯出、HTTP通訊協定違規，以及其他[OWASP十大](https://owasp.org/www-project-top-ten/)安全性威脅。
- **WAF上線和啟用** — 布建完成後的2至3週內，Adobe在您的生產環境中部署並啟用預設WAF原則。
- **操作與維護支援**—
   - Adobe和Fastly為WAF服務設定和管理您的記錄、規則和警示。
   - Adobe會分類與WAF服務問題相關的客戶支援票證，這些服務問題會將合法流量封鎖為優先順序1問題。
   - 自動升級至WAF服務版本，確保可立即涵蓋新的或不斷演變的利用漏洞行為。 請參閱[WAF維護與升級](#waf-maintenance-and-updates)。

>[!TIP]
>
>如需在雲端基礎結構存放區維護Adobe Commerce PCI法規遵循的其他資訊，請參閱[PCI法規遵循](https://business.adobe.com/products/magento/pci-compliance.html)。

## 啟用WAF

Adobe功能會在布建完成後的2至3週內，針對新帳戶啟用WAF服務。 WAF是透過Fastly CDN服務實作。 您不需要安裝或維護任何硬體或軟體。

>[!NOTE]
>
>在使用WAF服務之前，您在雲端基礎結構專案上流向Adobe Commerce的所有外部流量必須透過Fastly服務路由。 檢視[設定Fastly](fastly-configuration.md)。

## 運作方式

WAF服務與Fastly整合，並使用Fastly CDN服務中的快取邏輯來篩選Fastly全域節點的流量。 我們在您的生產環境中啟用WAF服務，預設的WAF原則是根據Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity)的[ModSecurity規則和OWASP十大安全性威脅。

WAF服務會針對WAF規則集檢查HTTP和HTTPS流量(GET和POST請求)，並封鎖惡意流量或不遵守特定規則的流量。 此服務只會檢查嘗試重新整理快取的原始繫結流量。 因此，我們會在Fastly快取中停止大多數的攻擊流量，保護原始流量免受惡意攻擊。 若僅處理原始流量，WAF服務可以保留快取效能，只對每個非快取要求引進估計為1.5毫秒至20毫秒的延遲。

## 疑難排解封鎖的請求

WAF服務啟用時，會根據WAF規則檢查所有網頁和管理員流量，並封鎖觸發規則的任何網頁請求。 當要求遭到封鎖時，要求者會看到預設`403 Forbidden`錯誤頁面，其中包含封鎖事件的參考ID。

![WAF錯誤頁面](../../assets/cdn/fastly-waf-403-error.png)

您可以從管理員自訂此錯誤回應頁面。 請參閱[自訂WAF回應頁面](fastly-custom-response.md#customize-the-waf-error-page)。

如果您的Adobe Commerce管理頁面或店面傳回`403 Forbidden`錯誤頁面以回應合法的URL請求，請提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 從錯誤回應頁面複製參考ID，並將其貼到票證說明中。

若要使用New Relic識別特定請求的WAF回應，請參閱下列內容：

- `Agent_response` — 表示WAF回應代碼（`200`表示良好，`406`表示已封鎖）
- `sigsci`標籤 — 根據請求的性質，將請求標籤到特定的Signal Sciences標籤

## WAF維護和更新

Fastly會根據商業第三方、Fastly研究和開放來源的規則更新，針對新的CVE/樣板化規則更新和推出修補程式。 Fastly會視需要或當可從其各自的來源取得規則變更時，將發佈的規則更新為原則。 此外，Fastly可以在啟用WAF服務後，將符合已發佈規則類別的規則新增到任何服務的WAF執行個體中。 這些更新可確保即時涵蓋新的或不斷演變的利用漏洞。

Adobe和Fastly管理更新流程，以確保新的或修改的WAF規則在您的生產環境中有效運作，然後再以封鎖模式部署更新。

## 問題

如果您發現WAF封鎖合法要求，這些通常是誤判，需要予以略過，或在WAF服務中實作因應措施。 提交支援票證並包含受影響的URL、重現錯誤的確切步驟，以及文字形式的錯誤參考（與熒幕擷圖相反）以避免轉譯錯誤。

## 限制

由Fastly支援的標準WAF服務不支援下列功能：

- 防止惡意程式碼或機器人降低 — 請考慮使用[存取控制清單](./fastly-vcl-allowlist.md)或協力廠商服務。
- 速率限制 — 請參閱Fastly檔案中的[速率限制](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)，或參閱&#x200B;_Commerce Web API_&#x200B;安全性章節中的[速率限制](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/)。
- 正在設定客戶的記錄端點 — 請參閱[PrivateLink服務](../development/privatelink-service.md)作為替代方法。

WAF服務可讓您根據IP位址封鎖或允許流量。 您可以將存取控制清單(ACL)和自訂VCL片段新增到Fastly服務中，以指定用於封鎖或允許流量的IP位址和VCL邏輯。 請參閱[自訂Fastly VCL片段](fastly-vcl-custom-snippets.md)。

WAF服務不支援篩選TCP、UDP或ICMP要求。 但是，此功能由Fastly CDN服務隨附的內建DDoS保護提供。 請參閱[DDoS保護](fastly.md#ddos-protection)。
