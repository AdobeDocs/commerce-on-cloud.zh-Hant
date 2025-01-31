---
title: Pro架構
description: 瞭解Pro架構支援的環境。
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 0%

---

# Pro架構

雲端基礎結構上的Adobe Commerce Pro架構支援多種環境，可用於開發、測試和啟動您的商店。

- **Master** — 提供部署至Platform as a Service (PaaS)容器的`master`分支。
- **整合** — 提供單一`integration`分支以進行開發，但您可以建立一個額外的分支。 這允許最多兩個&#x200B;_活動_&#x200B;分支部署到Platform as a Service (PaaS)容器。
- **暫存** — 提供單一`staging`分支，可部署至專用基礎結構即服務(IaaS)容器。
- **生產** — 提供單一`production`分支以部署至專用基礎結構即服務(IaaS)容器。

下表總結了環境之間的差異：

|                                        | 整合 | 分段 | 生產 |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| 支援[!DNL Cloud Console]中的設定管理 | 是 | 有限 | 有限 |
| 支援多個分支 | 是 | 否（僅限分段） | 否（僅限生產） |
| 使用YAML檔案進行設定 | 是 | 否 | 否 |
| 在專用的IaaS硬體上執行 | 否 | 是 | 是 |
| 包含Fastly CDN | 否 | 是 | 是 |
| 包含New Relic服務 | 否 | APM | APM + NRI |
| 自動備份 | 否 | 是 | 是 |

>[!NOTE]
>
>Adobe提供適用於Commerce的Cloud Docker工具，用於部署至本機Cloud Docker環境，以便您可以開發和測試Adobe Commerce專案。 請參閱[Docker開發](../dev-tools/cloud-docker.md)。

## 環境架構

您的專案是單一Git存放庫，具有三個主要環境分支： `integration`、`staging`和`production`。 下圖顯示Pro環境的階層式關係：

![Pro環境架構的高階檢視](../../assets/pro-branch-architecture.png)

### 主要環境

在Pro專案上，`master`分支會提供使用中的PaaS環境以及您的生產環境。 請一律將生產計畫碼的副本推送到`master`環境，這樣您就可以在不中斷服務的情況下對生產環境進行偵錯。

**警告：**

- 請&#x200B;**不**&#x200B;根據`master`分支建立分支。 使用整合環境建立用於開發的有效分支。

- 請勿使用`master`環境進行開發、UAT或效能測試

### 整合環境

整合環境會在名為PaaS的伺服器網格上的Linux容器(LXC)中執行。 每個環境都包含網站伺服器和資料庫，用以測試您的網站。 如需AWS和Azure IP位址清單，請參閱[地區IP位址](../project/regional-ip-addresses.md)。

**建議的使用案例：**

整合環境是專為有限的測試和開發而設計，以便將變更移至中繼和生產環境。 例如，您可以使用整合環境來完成下列工作：

- 確保對持續整合(CI)流程的變更與雲端相容

- 在關鍵頁面上測試關鍵工作流程，例如首頁、類別、產品詳細資料頁面(PDP)、結帳和管理員

若要在整合環境中取得最佳效能，請遵循下列最佳實務：

- 限制目錄大小 — 作為參考，範例資料包含約2,048種產品。 請嘗試將目錄大小縮減至約4,000至5,000種產品。
若要檢查目錄中的產品數目，請執行下列MySQL查詢：

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- 減少客戶群組數量 — 擁有過多客戶群組可能會影響索引效能和整體效能。

- 限制使用一或兩名同時使用者

- 停用cron工作並根據需要手動執行

**警告：**

- 無法在整合環境中存取Fastly CDN和New Relic服務

- 整合環境架構不符合測試和生產架構

- 請勿使用`integration`環境進行開發測試、效能測試或使用者驗收測試(UAT)

- 請勿使用`integration`環境來測試B2B的Adobe Commerce功能

- 您無法從資料庫生產或中繼還原整合環境中的資料庫

{{enhanced-integration-envs}}

### 中繼環境

中繼環境提供測試網站的近乎生產環境。 此環境由專屬的IaaS硬體託管，並包含所有服務，例如Fastly CDN、New Relic APM和搜尋。

**建議的使用案例：**

此環境符合生產架構，且設計用於UAT、內容測試和最終稽核，然後再將功能推送到`production`環境。 例如，您可以使用`staging`環境來完成下列工作：

- 針對生產資料進行回歸測試

- 啟用Fastly快取的效能測試

- 測試新組建，而非在生產環境中修補

- 新組建的UAT測試

- 測試Adobe Commerce的B2B

- 自訂cron設定和測試cron工作

請參閱[部署工作流程](pro-develop-deploy-workflow.md#deployment-workflow)和[測試部署](../test/staging-and-production.md)。

**警告：**

- 啟動生產網站後，請先使用預備環境測試修補程式，以進行生產關鍵性錯誤修正。

- 您無法從`staging`分支建立分支。 而是將程式碼變更從`integration`分支推送到`staging`分支。

{{second-staging}}

### 生產環境

生產環境會執行您的公開單一和多網站店面。 此環境在專用的IaaS硬體上執行，具備備援的高可用性節點，可為您的客戶提供持續存取和容錯移轉保護。 生產環境包含中繼環境中的所有服務，以及[New Relic基礎結構(NRI)](../monitor/new-relic-service.md#new-relic-infrastructure)服務，該服務會自動連線應用程式資料和效能分析，以提供動態伺服器監視。

**警告：**

您無法從`production`分支建立分支。 而是將程式碼變更從`staging`分支推送到`production`分支。

### 生產技術棧疊

生產環境有三個虛擬機器器(VM)，位於由每個VM的HAProxy管理的Elastic Load Balancer後面。 每台VM都包含下列技術：

- **Fastly CDN**—HTTP快取和CDN

- **NGINX** — 使用PHP-FPM的網頁伺服器，一個執行個體具有多個背景工作

- **GlusterFS** — 用來管理所有靜態檔案部署及與四個目錄掛載同步化的檔案伺服器：

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis** — 每個VM一台伺服器，只有一個作用中，另外兩個做為復本

- **Elasticsearch** — 在雲端基礎結構2.2到2.4.3-p2上搜尋Adobe Commerce

- **OpenSearch** — 在雲端基礎結構2.3.7-p3、2.4.3-p2、2.4.4和更新版本上搜尋Adobe Commerce

- **Galera** — 資料庫叢集，每個節點有一個MariaDB MySQL資料庫，每個資料庫的唯一識別碼的自動增加設定為3

下圖顯示生產環境中使用的技術：

![生產技術棧疊](../../assets/az-stack-diagram.png)

## 備援硬體

雲端基礎結構上的Adobe Commerce不是執行傳統的、主動 — 被動`master`或主要 — 次要設定，而是執行&#x200B;_備援架構_，其中所有三個執行個體都接受讀取和寫入。 此架構在擴充時可提供零停機時間，並確保交易完整性。

由於獨特的備援硬體，Adobe可提供三部閘道伺服器。 大部分的外部服務可讓您將多個IP位址新增至允許清單，因此擁有多個固定IP位址不成問題。 三個閘道對應至生產環境叢集中的三個伺服器，並保留靜態IP位址。 它完全備援，而且每個層級都具備高可用性：

- DNS
- 內容傳遞網路(CDN)
- 彈性負載平衡器(ELB)
- 包含所有Adobe Commerce服務（包括資料庫和網頁伺服器）的三伺服器叢集

## 備份與災難回覆

雲端基礎結構上的Adobe Commerce使用高可用性架構，該架構會複製三個獨立AWS或Azure可用性區上的每個Pro專案，每個區有一個獨立的資料中心。 除了此備援之外，Pro中繼和生產環境還會接收定期的即時備份，這些備份是針對&#x200B;_災難性失敗_&#x200B;的情況而設計。

**自動備份**&#x200B;包含來自所有執行中服務的永久資料，例如MySQL資料庫和儲存在掛載磁碟區上的檔案。 備份會儲存至與生產環境位於相同區域的加密彈性區塊儲存(EBS)。 無法公開存取自動備份，因為它們儲存在不同的系統中。

>[!NOTE]
>
>掛接的磁碟區僅包含/參考[可寫入的掛接](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/configure/app/properties/properties#mounts)，不會包含所有`app/`目錄。 至於其他檔案，它們是由[建置和部署程式](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow)所建立/產生，您也必須檢查Git存放庫中的剩餘檔案。

{{pro-backups}}

您可以使用CLI命令，為中繼和生產環境建立資料庫的&#x200B;**手動備份**。 請參閱[備份資料庫](../storage/database-dump.md)。 針對`integration`環境，Adobe建議在雲端基礎結構專案上存取Adobe Commerce後，以及套用任何重大變更之前，先建立備份，作為第一步。 請參閱[備份管理](../storage/snapshots.md)。

### 復原點目標

請連絡您的Adobe客戶成功案例經理，以取得復原點目標上次備份時間的詳細資訊。 備份的頻率取決於您計畫的備份排程，以及寫入儲存服務的變更量。

### 保留原則

Adobe會根據下列資料保留原則保留自動備份：

| 時段 | 備份保留原則 |
| ------------------ | ----------------------- |
| 第1天至第3天 | 每小時一次備份 |
| 第4天至第7天 | 每天一次備份 |
| 第2週至第6週 | 每週一次備份 |
| 第8週至第12週 | 每兩週備份一次 |
| 第3個月到第5個月 | 每月一次備份 |

此政策可能會因您的雲端基礎結構計畫而異。

### 復原時間目標

RTO取決於儲存的大小。 大型EBS磁碟區需要更多時間來還原。 還原時間可能會因資料庫大小而異。 如需詳細資訊，請聯絡您的Adobe客戶成功案例經理。

## Pro叢集縮放

Pro叢集大小和&#x200B;_計算_&#x200B;設定會依所選的雲端提供者(AWS、Azure)、地區和服務相依性而有所不同。 Adobe雲端基礎結構可以擴展Pro叢集，以因應流量期望和服務需求變化。

備援架構可讓Adobe雲端基礎建設得以升級，無需停機。 升級時，這三個執行個體都會輪換以升級容量，而不會影響網站作業。 例如，如果限制在PHP層級，而不是資料庫層級，則可以將額外的Web伺服器新增到現有的叢集。 這提供&#x200B;_水平縮放_，以補充資料庫層級上額外CPU所提供的垂直縮放。 請參閱[縮放架構](scaled-architecture.md)。

如果您預期因事件或其他原因而導致流量大幅增加，可請求暫時增加容量。 請參閱[如何在&#x200B;_Commerce說明中心_&#x200B;中要求暫時性的大小調整](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html)。
