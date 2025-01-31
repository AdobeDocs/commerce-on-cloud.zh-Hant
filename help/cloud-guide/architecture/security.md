---
title: 雲端基礎結構安全性
description: 瞭解Adobe如何確保雲端基礎結構上的Adobe Commerce安全。
feature: Cloud, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---

# 安全性

Adobe Commerce [Pro計畫架構](pro-architecture.md)可提供高度安全的環境。 每個客戶都會部署至各自獨立的伺服器環境，與其他客戶分開。 生產環境的安全性詳細資訊如下所述。

## 網頁瀏覽器

進出雲端環境的大量流量來自消費者的網頁瀏覽器。 使用HTTPS可保護網站所有頁面的消費者流量（使用共用SSL憑證或客戶自己的SSL憑證時，需額外付費）。 結帳和帳戶頁面一律使用HTTPS提供。 最佳實務是提供HTTPS下的所有頁面。

## 內容傳遞網路

Fastly提供內容傳遞網路(CDN)和分散式拒絕服務(DDoS)保護。 Fastly CDN有助於隔離對原始伺服器的直接存取。 公用DNS只指向Fastly網路。 Fastly DDoS解決方案可抵禦高中斷性的第3層和第4層攻擊，以及更複雜的第7層攻擊。 第7層攻擊可使用根據整個HTTP/HTTPS請求以及根據使用者端和請求條件（包括標頭、Cookie、請求路徑和使用者端IP，或地理位置等指標）的自訂規則來封鎖。

請參閱[Fastly服務總覽](../cdn/fastly.md)。

## Web應用程式防火牆

Fastly Web Application Firewall (WAF)用於提供額外的保護。 Fastly的雲端型WAF使用來自商業和開放原始碼來源的第三方規則，例如OWASP核心規則集。 此外，也會採用Adobe Commerce特定規則。 客戶可免於關鍵應用程式層攻擊，包括插入式攻擊和惡意輸入、跨網站指令碼、資料匯出、HTTP通訊協定違規，以及其他OWASP前十大威脅。

若偵測到新的漏洞，Adobe Commerce會更新WAF規則，讓Managed Services能夠在軟體修補前「幾乎修正」安全性問題。 Fastly WAF不提供速率限制或機器人偵測服務。 如有需要，客戶可以授權與Fastly相容的協力廠商機器人偵測服務。

請參閱[網頁應用程式防火牆(WAF)](../cdn/fastly-waf-service.md)。

## 虛擬私人雲端

Adobe Commerce Pro計畫生產環境已設定為虛擬私人雲端(VPC)，因此生產伺服器會遭到隔離，且與雲端環境的連線能力有限。 只允許與雲端伺服器的安全連線。 SFTP或rsync等安全通訊協定可用於檔案傳輸。

客戶可以使用SSH通道來保護與應用程式的通訊。 如需存取AWS PrivateLink，需要支付額外費用。 與這些伺服器的所有連線都是使用AWS安全性群組所控制，這是限制與環境連線的虛擬防火牆。 客戶的技術資源可使用SSH存取這些伺服器。

## 加密

Amazon彈性區塊存放區(EBS)用於儲存。 所有EBS磁碟區都使用AES-256演演算法加密，這表示資料會靜態加密。 此系統也會加密CDN與來源之間以及來源伺服器之間傳輸中的資料。 客戶密碼會儲存為雜湊。 敏感認證（包括付款閘道認證）會使用SHA-256演演算法加密。

當資料不在伺服器之間靜止或傳輸中時，Adobe Commerce應用程式不支援欄或列層級的加密或加密。 客戶可以從應用程式內管理加密金鑰。 系統使用的金鑰儲存在AWS金鑰管理系統中，且必須由Managed Services管理，才能提供部分服務。

## 端點偵測和回應

[!DNL CrowdStrike Falcon]是Adobe內所有端點（包括伺服器）上安裝的輕量新一代端點偵測與回應(EDR)代理程式。 EDR代理程式可透過即時持續監控及收集來保護Adobe資料與系統，進而快速識別與回應威脅。

## 滲透測試

Managed Services會使用現成可用的應用程式，定期對Adobe Commerce雲端系統進行滲透測試。 客戶需自行負責自訂應用程式的滲透測試。

## 付款閘道

Adobe Commerce需要支付閘道整合，信用卡資料會直接從消費者的瀏覽器傳送至支付閘道。 卡片資料在任何Adobe Commerce Pro計畫Managed Services環境中都無法使用。 電子商務應用程式對交易的動作，會使用來自閘道的交易參考來完成。

## Adobe Commerce應用程式

Adobe會定期測試核心應用程式程式碼的安全性弱點。 為客戶提供缺陷和安全問題的修補程式。 產品安全性團隊會遵循OWASP應用程式安全性方針，驗證Adobe Commerce產品。 數種安全性弱點評估工具與外部廠商可用來測試及驗證合規性。 安全性工具包括：

- Veracode靜態和動態掃描
- RIPS原始程式碼掃描
- Trustwave和Alert Logic的漏洞掃描服務
- Burp Suite Pro
- OWASPZAP
- 和SqlMap

這些工具會每兩週掃描完整程式碼基底。 透過直接電子郵件、應用程式通知以及[安全性中心](https://helpx.adobe.com/security.html)通知客戶安全性修補程式。

客戶必須根據PCI准則，確保這些修補程式在發行後30天內套用至其自訂應用程式。 Adobe也提供[安全性掃描工具](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan)，可讓商家定期監視其網站，並接收已知安全性風險、惡意程式碼及未經授權存取的更新。 安全掃描工具是免費服務，可在任何版本的Adobe Commerce上執行。

為了鼓勵安全性研究人員識別和報告漏洞，除了內部測試之外，Adobe Commerce還設有[bug-bounty計畫](https://hackerone.com/magento)。 此外，如有需要，客戶會獲得應用程式的完整原始程式碼，供其自行檢閱。

## 唯讀檔案系統

所有可執行程式碼都會部署至唯讀檔案系統影像中，大幅減少可供攻擊的表面。 部署程式會建立Squash-FS影像，以減少將PHP或JavaScript程式碼插入系統或修改Adobe Commerce應用程式檔案的機會。

## 遠端部署

將可執行程式碼傳入Managed Services生產環境的唯一方法是透過布建流程執行。 布建包括將原始程式碼從您的來源存放庫推送至起始部署流程的遠端存放庫。 該部署目標的存取權已受控制，因此您可以完全控制誰可以存取部署目標。 將應用程式程式碼部署到非生產環境的所有工作均由客戶控制。

## 記錄

所有AWS活動都會登入AWS CloudTrail。 作業系統、應用程式伺服器和資料庫記錄檔會儲存在生產伺服器上，並儲存在備份中。 所有原始程式碼變更都記錄在Git存放庫中。 部署歷史記錄可在Adobe Commerce [Project Web介面](../project/overview.md)中使用。 所有支援存取許可權都會記錄下來，支援工作階段也會記錄下來。

檢視[檢視及管理記錄檔](../test/log-locations.md)。

## 敏感資料

敏感資料可能涵蓋消費者提供的個人資訊或Managed Services客戶的機密資料。 保護敏感的客戶和消費者資料是Adobe Commerce Managed Services的重要義務。 Managed Services和Adobe客戶均有與個人識別資訊相關的法律義務。 除了架構的安全性功能外，還有其他控制項可限制敏感資料的發佈和存取。

客戶擁有其資料，並控制該資料的位置。 客戶會指定其生產及開發執行個體所在的位置。 此外也需指定搭配Commerce使用Adobe Commerce報表環境的位置，以及該Adobe Commerce報表應用程式是否可存取個人識別資訊。 生產執行個體可以在大多數AWS區域，而開發和Adobe Commerce報告環境目前可以在美國或歐盟找到。

敏感資料可能會通過Fastly CDN伺服器網路，但不會儲存在Fastly網路中。 Managed Services產品包含的所有合作夥伴都有合約義務，以確保保護敏感資料。 Managed Services不會將敏感的客戶或消費者資料從客戶指定的位置移動。

在提供Managed Services產品所含的服務時，Managed Services員工需要存取包含敏感資料的生產系統。 這些員工均接受保護敏感客戶及消費者資料的培訓。 只有需要存取許可權的員工才能存取這些系統。 這些員工只能存取提供這些服務所需的時間。

## 一般資料保護規範

一般資料保護規範(GDPR)為一法律架構，其設立收集和處理歐盟(EU)境內人員之個人資訊的方針。 無論網站總部設在何處，歐盟訪客是否有權存取，這些法規均適用。

網站必須向訪客通知其收集到的資料，並明確同意收集資訊。 如果網站持有的個人資料遭到破壞，網站必須通知訪客。

經營網站的商家或公司必須有專門的資料保護人員來監督網站的資料安全性。 如果訪客要求清除其資料，資料隱私權主管（或網站管理團隊）應可聯絡。

GDPR要求收集到的任何個人識別資訊（例如姓名、種族和出生日期）進行匿名化或匿名化。

>[!NOTE]
>
>本頁提供GDPR考量事項的一般概觀。 如需Adobe Commerce如何儲存個人資訊的詳細資訊，請參閱&#x200B;_[安全性與法規遵循指南](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/overview)_。 若要確定您的企業應如何遵守任何法律義務，請諮詢您的法律顧問，或參閱[官方文字](https://eur-lex.europa.eu/eli/reg/2016/679/oj)。

## 備份

過去24小時的作業每小時執行一次備份。 24小時之後，會使用AWS EBS快照服務依排程保留備份。 請參閱[保留原則](pro-architecture.md#retention-policy)。

此服務會在備援儲存裝置上建立獨立的備份。 因為EBS磁碟區已加密，所以備份也會加密。 此外，Managed Services會根據客戶要求執行隨選備份。

您的Managed Services備份與回複方法採用高可用性架構，並結合完整系統備份。 每個專案（所有資料、程式碼和資產）都會在三個獨立的AWS可用區進行複製；每個區都有一個獨立的資料中心。

請參閱[快照與備份管理](../storage/snapshots.md)。
