---
title: Adobe Commerce進階安全性
description: 瞭解進階安全性如何在雲端基礎結構上的Adobe Commerce中新增機器人管理、進階速率限制和第7層DDoS保護。
feature: Cloud, Configuration, Security
source-git-commit: 8a7c1c297092fdf2b75d22ce99c360c85eac0495
workflow-type: tm+mt
source-wordcount: '1986'
ht-degree: 0%

---

# [!DNL Adobe Commerce Advanced Security]

[!DNL Adobe Commerce Advanced Security]是與[!DNL Adobe Commerce on Cloud Infrastructure]搭配使用的產品，可讓您的線上商店保持快速、可用和安全。 這有助於保護營收、減少停機時間，並在流量尖峰事件和自動攻擊期間維持客戶信任。

[!DNL Adobe Commerce on Cloud Infrastructure]包含內建[第3層和第4層DDoS保護](./fastly.md#ddos-protection)以及[Web應用程式防火牆(WAF)](./fastly-waf-service.md)。 在[共用職責模型](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)下，第7層DDoS偵測、機器人保護和主動IP封鎖是商家職責，[!DNL Adobe Commerce Advanced Security]旨在解決這些職責。

[!DNL Advanced Security]透過Fastly支援的邊緣安全功能延伸店面保護，提供機器人管理、進階速率限制，以及第7層DDoS保護，作為整合邊緣平台的一部分，結合網路邊緣的規模、效能及安全性。

>[!NOTE]
>
>[!DNL Advanced Security]僅適用於[!DNL Adobe Commerce on Cloud Infrastructure] (PaaS)專案。

## 核心功能

[!DNL Adobe Commerce Advanced Security]包含下列額外保護：

- **[機器人管理](https://docs.fastly.com/products/bot-management)** — 識別並緩解您網頁應用程式上不要的機器人活動。 機器人管理服務會區分合法機器人（搜尋引擎爬蟲、社群媒體機器人）和惡意機器人，提供網路邊緣的即時分類，並附上封鎖、允許、挑戰或速率限制流量的選項。

- **[DDoS保護](https://docs.fastly.com/products/fastly-ddos-protection)** — 提供第7層（應用程式層） DDoS保護，超出所有[!DNL Adobe Commerce on Cloud Infrastructure]專案所包含的現有第3層和第4層保護。 DDoS Protection服務會吸收大規模體積攻擊，並確保在分散式拒絕服務(DDoS)事件期間能持續使用應用程式，保護尖峰流量期間的營收。

- **[進階速率限制](https://www.fastly.com/documentation/guides/next-gen-waf/rules/working-with-advanced-rate-limiting-rules/)** — 提供可設定的速率限制規則，可保護特定URL、API端點和應用程式資源不受濫用。 進階速率限制服務超越了透過Fastly CDN模組提供的[基本速率限制](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md)，以鎖定特定流量模式和攻擊向量，減少基礎建設緊張和雲端成本。

>[!NOTE]
>
>[!DNL Advanced Security]個設定目前需要提交支援票證。 計畫在未來版本中透過管理員UI進行自助設定。 如需詳細資訊，請參閱[要求 [!DNL Advanced Security]](#request-advanced-security)。

## 威脅涵蓋範圍

[!DNL Advanced Security]可保護店面不受各種自動化和應用程式層威脅的影響。

![Adobe Commerce安全性棧疊中的進階安全性定位](../../assets/advanced-security.svg)

### 機器人導向濫用

- **認證填滿** — 自動嘗試使用資料洩露中竊取的認證登入。
- **帳戶接管** — 嘗試未經授權存取客戶帳戶的機器人。
- **帳戶建立濫用** — 自動建立虛假帳戶，用於詐騙或濫用。
- **卡片測試** — 機器人會針對您的付款處理程式，測試被盜的信用卡號碼。
- **內容清除** — 從店面自動擷取產品資料、價格或內容。
- **庫存囤積** — 在購物車中儲存產品的機器人，以防止合法購買。

### AI機器人管理

- **AI爬蟲偵測** — 識別並管理未經同意而刮取內容以訓練大型語言模型的AI爬蟲。
- **AI擷取器控制項** — 控制即時AI 搜尋結果中使用的AI擷取器。
- **可設定的AI機器人原則** — 區分驗證和疑似具有可設定之訊號型別的AI機器人以執行原則。

### 應用程式層攻擊

- **第7層DDoS攻擊** — 目標為應用程式層的分散式攻擊，可略過內建的第3層和第4層保護。 [!DNL Advanced Security]在到達原始伺服器之前會吸收這些邊緣的體積攻擊。
- **URL和API濫用** — 目標為特定URL或API端點的攻擊分散在大量的IP位址上，且個別IP封鎖無效。
- **防快取攻擊** — 含有設計成略過CDN快取，讓原始伺服器不堪重負的查詢引數的要求。

### 其他功能

- **動態挑戰** — 自動將最佳挑戰指派給可疑流量。 運用私人存取權杖(PAT)順暢地驗證部分請求，而不會影響使用者體驗。
- **欺騙技術** — 將虛假資訊傳回給攻擊者，減少其攻擊，同時中斷其大規模作業的能力，藉此解決帳戶接管嘗試的問題。

## 選擇正確的保護

使用下列指南來判斷[!DNL Advanced Security]是否適合您的店面保護需求，或現有的保護或替代解決方案是否更適合。

### 何時使用[!DNL Advanced Security]

下列情況最好透過[!DNL Advanced Security]處理：

| 情境 | [!DNL Advanced Security]如何協助 |
|---|---|
| 您的網站會遇到機器人驅動的攻擊，例如認證填滿、內容刮取或詳細目錄囤積 | 機器人管理可在威脅抵達您的應用程式前，識別並緩解邊緣的自動化威脅 |
| 您需要第7層DDoS保護，超越內建的第3層和第4層涵蓋範圍 | DDoS Protection會吸收繞過網路層級保護的應用程式層攻擊 |
| 無法被IP封鎖的高流量分散式流量會鎖定特定URL或API端點 | 進階速率限製為特定端點和流量模式提供精細的控制項 |
| 您想要管理存取店面內容的AI爬蟲和擷取器 | 機器人管理包括可設定的AI機器人偵測和執行原則 |
| 您需要整合了Adobe支援的邊緣安全性解決方案與現有的Fastly CDN | [!DNL Advanced Security]在已經為您店面提供服務的相同Fastly Edge平台上執行 |

### 何時使用現有的保護

下列情況最適合使用現有的保護機制：

| 情境 | 建議做法 |
|---|---|
| 單一IP或小型可識別IP集正在以大量請求淹沒您的網站 | 使用Commerce Admin或Fastly API封鎖IP。 使用內建的[Layer 3/4 DDoS保護](./fastly.md#ddos-protection)和現有的[IP封鎖清單](./fastly-vcl-blocking.md) VCL片段。 |
| 您需要封鎖SQL插入、跨網站指令碼(XSS)或其他OWASP十大威脅 | 包含的[WAF服務](./fastly-waf-service.md)會自動封鎖這些威脅。 |
| 您的DDoS攻擊模式可使用基本的VCL封鎖規則來控制 | 使用Adobe Commerce已提供的現有[自訂VCL片段](./fastly-vcl-custom-snippets.md)。 |

### 何時使用替代保護

下列情況最適合使用可補充[!DNL Advanced Security]的替代保護：

| 情境 | 建議做法 |
|---|---|
| 您需要交易層級的詐騙評分或付款詐騙防護 | 使用專門的防欺詐平台。 [!DNL Advanced Security]會保護邊緣網路層級，且不會評估個別付款交易。 |
| 您需要識別與存取管理(IAM) | 實作專用的IAM解決方案。 使用者驗證和工作階段管理仍由客戶負責。 |
| 您需要靜態或動態應用程式安全性測試(SAST/DAST) | 使用專用的應用程式安全性測試工具。 未提供程式碼層級弱點掃描。 |
| 您需要超過速率限制的完整API安全性（例如結構驗證或API閘道功能） | 考慮使用專用的API安全性平台。 |
| 您需要法規遵循工具，例如PCI掃描或SOC報告 | 使用專屬的合規性管理工具。 |

>[!TIP]
>
>如果您目前使用協力廠商機器人保護提供者，合併至[!DNL Advanced Security]可以降低營運複雜性，並消除跨提供者不一致的安全性涵蓋範圍。 請連絡您的Adobe客戶團隊以評估專案的[!DNL Advanced Security]。

## 安全性棧疊定位

[!DNL Advanced Security]適合更廣的Adobe Commerce安全性架構，是額外的邊緣保護層。 它可與WAF及[!DNL Adobe Commerce on Cloud Infrastructure]已包含的第3/4層DDoS保護搭配使用，而不會取代。 以下各節將闡明其與現有保護機制的關係，以及客戶應承擔的責任。

### 包含保護

[!DNL Adobe Commerce on Cloud Infrastructure]包含下列安全性功能：

- **[Web應用程式防火牆(WAF)](./fastly-waf-service.md)** — 針對SQL插入、跨網站指令碼(XSS)和其他Open Web應用程式安全性專案(OWASP)十大威脅的受管理保護。 僅適用於生產環境。
- **[第3層和第4層DDoS保護](./fastly.md#ddos-protection)** — 內建網路層保護，可抵禦SYN泛洪、UDP泛洪、ICMP型攻擊和TCP層級攻擊。 已使用Fastly CDN自動啟用。
- **[SSL/TLS憑證](./fastly-configuration.md#provision-ssltls-certificates)** — 用於安全HTTPS流量的網域已驗證的加密憑證。
- **[原始遮罩](./fastly.md#origin-cloaking)** — 確保所有流經Fastly的流量路由，封鎖對原始伺服器的直接存取。
- **[VCL型安全性片段](./fastly-vcl-custom-snippets.md)** — 適用於IP封鎖、加入允許清單及要求篩選的自訂清漆組態語言(VCL)規則。

### [!DNL Advanced Security]

[!DNL Advanced Security]提供的保護比[!DNL Adobe Commerce on Cloud Infrastructure]包含的內建保護更強大，但需額外付費：

- **機器人管理**—Edge式的機器人偵測與AI機器人管理緩解。
- **第7層DDoS保護** — 應用程式層DDoS吸收與防禦。
- **進階速率限制**—URL和API端點的精細速率控制。
- **動態挑戰與欺騙技術** — 自動挑戰指派與帳戶接管緩解。

### 客戶責任

- **預防詐騙** — 交易層級的詐騙評分與支付詐騙偵測。
- **識別與存取管理** — 客戶驗證、授權和工作階段管理。
- **應用程式安全性測試**—SAST/DAST和弱點掃描。
- **自訂安全性組態**—VCL型規則、IP允許清單和封鎖清單。
- **法規工具**—PCI掃描、SOC法規報告及法規稽核工具。
- **應用程式層級強化** — 權杖式API驗證、查詢引數標準化，以及快取策略設計。

如需Adobe和客戶安全性責任的完整概觀，請參閱[共用責任模式](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility)。

## 常見的攻擊模式與保護

下表將常見的攻擊模式對應至Adobe Commerce安全性棧疊中的適當保護層。

| 攻擊模式 | 型別 | 保護 |
|---|---|---|
| 單一IP或一組可識別的IP，會傳送大量請求 | DDoS +機器人 | 使用Commerce管理員或Fastly API封鎖IP。 內建的第3/4層DDoS保護功能會篩選網路邊緣的流量。 |
| 攻擊橫跨大量IP的特定URL或API | DDoS +機器人 | **[!DNL Advanced Security]**：進階速率限制會限制每個URL的要求磁碟區。 機器人管理會識別和封鎖分散的機器人流量。 |
| 在未正確驗證的情況下對REST API端點進行自動攻擊 | 機器人+ DDoS | 確認API端點使用權杖型驗證。 如果Token已洩漏，請輪換認證。 **[!DNL Advanced Security]**：進階速率限制可以保護公開的端點。 |
| 使用操縱的查詢引數防快取攻擊 | 機器人+ DDoS | 從快取索引鍵排除不必要的查詢引數。 在應用程式層級標準化並限制查詢引數。 **[!DNL Advanced Security]**： Bot Management會偵測並封鎖自動的防快取流量。 |
| SQL插入或跨網站指令碼(XSS)嘗試 | WAF | 包含的[WAF服務](./fastly-waf-service.md)會使用受管理安全性規則自動封鎖這些威脅。 |

### WAF封鎖行為

下列WAF行為適用於所有[!DNL Adobe Commerce on Cloud Infrastructure]專案，無論是否已啟用[!DNL Advanced Security]。 隨附的WAF服務會針對常見攻擊訊號使用以下封鎖行為：

- 立即封鎖&#x200B;**SQL插入**&#x200B;要求，即使是單一比對要求亦然。
- 系統會立即封鎖來自已知惡意IP的識別為下列威脅訊號的請求：後門、攻擊工具、CMDEXE、Log4J JNDI、周遊和XSS。
- 顯示上述威脅訊號的非惡意IP的請求超過下列臨界值時會遭到封鎖：

| 間隔 | 臨界值 | 檢查頻率 |
|---|---|---|
| 1分鐘 | 50個請求 | 每20秒 |
| 10分鐘 | 350個請求 | 每3分鐘 |
| 1小時 | 1,800個請求 | 每20分鐘 |

## 要求[!DNL Advanced Security]

若要要求[!DNL Advanced Security]：

>[!NOTE]
>
>[!DNL Advanced Security]需額外付費，且需要有效的[!DNL Adobe Commerce on Cloud Infrastructure] (PaaS)訂閱。

1. 請聯絡您的Adobe客戶團隊或Adobe銷售代表，討論您專案的[!DNL Advanced Security]。

1. 購買[!DNL Advanced Security]後，[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)，要求[!DNL Advanced Security]啟用。 包含您的[!DNL Adobe Commerce on Cloud Infrastructure]專案ID和需要啟用的環境（例如，生產和測試）。

1. Adobe會在您的Fastly服務上啟用[!DNL Advanced Security]並設定初始保護原則。 啟用通常會在票證提交後的幾個工作日內完成。

1. 您會收到確認[!DNL Advanced Security]為作用中，以及針對您的環境啟用的保護的詳細資訊。

>[!NOTE]
>
>對[!DNL Advanced Security]的組態變更目前需要[提交支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)。 計畫在未來版本中透過管理員UI進行自助設定。

## 限制

[!DNL Advanced Security]提供邊緣層店面保護。 下列功能無法使用，最適合搭配互補解決方案使用：

- **交易層級的詐騙評分**—[!DNL Advanced Security]不會評估個別付款交易的詐騙風險。 使用專用的防欺詐平台進行交易層級的評分。
- **識別與存取管理(IAM)**—[!DNL Advanced Security]未管理使用者驗證、授權或工作階段管理。 這些仍然是客戶的責任。
- **靜態和動態應用程式安全性測試(SAST/DAST)**—[!DNL Advanced Security]不包含程式碼層級的弱點掃描或滲透測試。
- **API安全性** — 雖然進階速率限制可以保護API端點不受濫用，但是未提供如結構描述驗證和API閘道管理的完整API安全性功能。
- **完全預防詐騙**—[!DNL Advanced Security]著重於邊緣層店面保護，不是完整的詐騙管理平台。
- **法規工具**—[!DNL Advanced Security]不提供PCI掃描、SOC法規遵循報告或法規稽核功能。
