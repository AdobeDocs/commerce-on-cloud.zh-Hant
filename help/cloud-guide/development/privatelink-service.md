---
title: PrivateLink服務
description: 瞭解如何使用PrivateLink服務，在相同地區的私人雲端和Adobe Commerce雲端平台之間建立安全連線。
feature: Cloud, Iaas, Security
exl-id: 13a7899f-9eb5-4c84-b4c9-993c39d611cc
source-git-commit: 0e7f268de078bd9840358b66606a60b2a2225764
workflow-type: tm+mt
source-wordcount: '1616'
ht-degree: 0%

---

# PrivateLink服務

雲端基礎結構上的Adobe Commerce支援與[AWS PrivateLink](https://aws.amazon.com/privatelink/)或[Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/)服務的整合。 Adobe Commerce您可以使用PrivateLink在雲端基礎結構環境與託管於外部系統的服務和應用程式之間建立安全的、私人的通訊。 必須透過在同一雲端區域中相同雲端平台(AWS或Azure)上設定的虛擬私人雲端(VPC)端點，才能存取Adobe Commerce應用程式和外部系統。

>[!TIP]
>
>PrivateLink最適合用於保護非HTTP整合的連線，例如資料庫或檔案傳輸。 如果您打算將應用程式與Adobe Commerce API整合，請參閱如何在適用於Adobe Developer App Builder[的](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/)Adobe API Mesh中建立&#x200B;_API Mesh_。

## 功能和支援

雲端基礎結構專案上適用於Adobe Commerce的PrivateLink服務整合包含下列功能和支援：

- 同一雲端區域內，同一雲端平台(VPC或Adobe)上的客戶虛擬私人雲端(AWS)與VPC之間的安全連線。
- 支援Adobe和客戶VPC提供的端點服務之間的單向或雙向通訊。
- 服務啟用：

   - 在雲端基礎結構環境的Adobe Commerce中開啟必要的連線埠
   - 建立客戶與Adobe VPC之間的初始連線
   - 疑難排解啟用期間的連線問題

## 限制

- PrivateLink的支援僅適用於Pro生產和中繼環境。 無法在本機或整合環境中使用，也無法在Starter專案中使用。
- 您無法使用PrivateLink建立SSH連線。 請參閱[啟用SSH金鑰](secure-connections.md)。
- Adobe Commerce支援不包含疑難排解AWS PrivateLink初始啟用以外的問題。
- 客戶需自行負責與管理自己VPC相關的成本。
- 平台支援&#x200B;**HTTPS通訊協定（連線埠443）：**
   - **Azure私人連結**：由於[Fastly來源遮罩](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html)，您無法使用HTTPS通訊協定（連線埠443）連線到雲端基礎結構上的Adobe Commerce。
   - **AWS PrivateLink**：支援HTTPS通訊協定（連線埠443）連線。
- 無法使用PrivateDNS。

## PrivateLink連線型別

有兩種可用的PrivateLink連線型別（如下列網路圖表所示），可在您的商店與雲端環境外部託管的外部系統之間建立安全通訊。

![PrivateLink網路圖表](../../assets/privatelink-architecture-diagram.png)

在雲端基礎結構環境中，選擇最適合您Adobe Commerce的PrivateLink連線型別之一：

- **單向PrivateLink** — 選擇此設定可安全地從雲端基礎結構存放區上的Adobe Commerce擷取資料。
- **雙向PrivateLink** — 選擇此設定，在雲端基礎結構環境中，建立與Adobe Commerce外部系統的安全連線，以及建立與系統之間的安全連線。 雙向選項需要兩個連線：

   - 客戶VPC與Adobe VPC之間的連線
   - Adobe VPC與客戶VPC之間的連線

>[!TIP]
>
>請洽詢您的網路系統管理員或Cloud平台提供者，以取得選取PrivateLink連線型別的協助，或是VPC設定與管理的協助。 請參閱Cloud Platform PrivateLink檔案： [AWS PrivateLink](https://aws.amazon.com/privatelink/)或[Azure私人連結](https://learn.microsoft.com/en-us/azure/private-link/)。

## 請求PrivateLink啟用

>[!WARNING]
>
>啟用PrivateLink最多需要&#x200B;_5_&#x200B;個工作日。 提供不完整或不準確的資訊可能會延遲處理程式。

### 先決條件

![檢查](../../assets/fix.svg)雲端帳戶(AWS或Azure)，其所在區域與雲端基礎結構執行個體上的Adobe Commerce相同。

![check](../../assets/fix.svg)客戶環境中的VPC，其託管要透過PrivateLink連線的服務。 請參閱AWS或Azure檔案，以取得VPC設定的說明，或聯絡您的網路管理員。

![檢查](../../assets/fix.svg)針對雙向PrivateLink連線，您必須先建立應用程式或服務的端點服務組態，並在VPC環境中建立端點，才能要求PrivateLink啟用。 請參閱[設定雙向PrivateLink連線](#set-up-for-bidirectional-privatelink-connections)。

收集啟用PrivateLink所需的下列資料：

- **Customer Cloud帳號** (AWS或Azure) — 必須在雲端基礎結構執行個體上與Adobe Commerce相同的區域
- **雲端區域** — 提供託管帳戶的雲端區域以進行驗證
- **服務和通訊連線埠**—Adobe必須開啟連線埠，才能啟用VPC之間的服務通訊，例如SQL連線埠3306、SFTP連線埠2222
- **專案識別碼** — 提供雲端基礎結構上的Adobe Commerce Pro專案識別碼。 您可以使用下列[雲端CLI](../dev-tools/cloud-cli-overview.md)命令取得專案識別碼和其他專案資訊： `magento-cloud project:info`
- **連線型別** — 指定連線型別的單向或雙向
- **端點服務** — 針對雙向PrivateLink連線，提供Adobe必須連線之VPC端點服務的DNS URL，例如： `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **已授與端點服務存取權** — 若要連線到外部服務，請允許端點服務存取下列AWS帳戶主體： `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >如果未提供端點服務的存取權，則在VPC中與該服務的雙向PrivateLink連線會&#x200B;**未新增**，這會延遲安裝。

#### 啟用Azure私人連結專用的其他先決條件

- 提供叢集ID；使用SSH，登入遠端並使用命令： `cat /etc/platform_cluster`
- 若要使用外部服務連線至您的Adobe Commerce Pro叢集，您需要：

   - Pro叢集上要公開至新外部私人端點的連線埠清單
   - 適用於私人端點連線的Azure訂閱ID清單

- 若要將Adobe Commerce Pro叢集連線至外部服務，您需要：

   - 目標服務的資源ID清單。 外部私人連結服務ID看起來類似以下內容：

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### 啟用工作流程

以下工作流程概述PrivateLink與雲端基礎結構上Adobe Commerce整合的啟用程式。

1. **客戶**&#x200B;提交支援票證，以主旨列`PrivateLink support for <company>`要求PrivateLink啟用。 在票證中包含啟用[所需的](#prerequisites)資料。 Adobe使用支援票證在啟用過程中協調通訊。

1. **Adobe**&#x200B;可讓客戶帳戶存取Adobe VPC中的端點服務。

   - 更新Adobe端點服務設定，以接受從客戶AWS或Azure帳戶起始的請求。
   - 更新支援票證以提供要連線的Adobe VPC端點的服務名稱，例如`com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`。

1. **客戶**&#x200B;將Adobe端點服務新增至其雲端帳戶(AWS或Azure)，這會觸發與Adobe的連線要求。 如需指示，請參閱Cloud平台檔案：

   - 若為AWS，請參閱[接受及拒絕介面端點連線要求]。
   - 若為Azure，請參閱[管理連線要求]。

1. **Adobe**&#x200B;核准連線要求。

1. 在連線要求核准後，**客戶** [驗證其VPC與Adobe VPC之間的連線](#test-vpc-endpoint-service-connection)。

1. 啟用雙向連線的其他步驟：

   - **Adobe**&#x200B;提供Adobe帳戶主體(AWS或Azure帳戶的根使用者)並要求存取客戶VPC端點服務。
   - **客戶**&#x200B;可讓Adobe存取客戶VPC中的端點服務。 這假設Adobe帳戶主體具有`arn:aws:iam::402592597372:root`的存取權，如先前在&#x200B;**已授與端點服務存取權**&#x200B;先決條件中所述。

      - 更新客戶端點服務設定，以接受從Adobe帳戶起始的請求。 如需指示，請參閱Cloud平台檔案：

         - 若為AWS，請參閱[新增與移除端點服務的許可權]。
         - 若為Azure，請參閱[管理私人端點連線]

      - 為Adobe提供客戶VPC的端點服務名稱。

   - **Adobe**&#x200B;將客戶端點服務新增至Adobe平台帳戶(AWS或Azure)，這會觸發與客戶VPC的連線要求。
   - **客戶**&#x200B;核准來自Adobe的連線要求，以完成設定。
   - **客戶** [驗證Adobe VPC的連線](#test-vpc-endpoint-service-connection)。

## 測試VPC端點服務連線

您可以使用Telnet應用程式來測試與VPC端點服務的連線。

**若要測試與VPC端點服務的連線**：

1. 從專案根目錄，**簽出**&#x200B;設定為存取PrivateLink端點服務的暫存或生產環境。

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. 執行下列CURL命令：

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   範例：

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   成功回應範例：

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   失敗的回應範例：

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. 驗證服務是否正在接聽VM。

   ```bash
   netstat -na | grep <port>
   ```

1. 檢查封裝流程。

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   檢查下列內部設定，確認設定有效：

   - 端點與端點服務設定
   - 網路負載平衡器(NLB)設定
   - NLB中的目標群組，並驗證其運作狀況良好
   - 來自每個VM的netcat/curl端點URL （如上所列）

   請參閱下列文章以取得疑難排解連線問題的說明：

   - [AWS：疑難排解端點服務連線]
   - [Amazon：疑難排解Azure私人連結連線問題]

   如果您無法解決錯誤，請更新Adobe Commerce支援服務單以請求協助建立連線。

## 變更PrivateLink設定

[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以變更現有的PrivateLink設定。 例如，您可以要求進行如下的變更：

- 在雲端基礎結構Pro生產或中繼環境中，從Adobe Commerce移除PrivateLink連線。
- 變更用於存取Adobe端點服務的客戶Cloud平台帳號。
- 從Adobe VPC新增或移除PrivateLink連線，以連線至客戶VPC環境中可用的其他端點服務。

## 設定雙向PrivateLink連線

客戶VPC必須具備下列資源才能支援雙向PrivateLink連線：

- 網路負載平衡器(NLB)
- 可讓您從客戶VPC存取應用程式或服務的端點服務設定
- 允許Adobe連線到您VPC中託管之端點服務的[介面端點] (AWS)或[私人端點] (Azure)

如果客戶VPC中沒有這些資源，您必須登入Cloud Platform帳戶才能新增設定。

- Amazon VPC主控台 — `https://console.aws.amazon.com/vpc/`
- Azure入口網站 — `https://portal.azure.com`

如需PrivateLink設定指示，請參閱您的Cloud Platform檔案：

- **AWS PrivateLink檔案**
   - [建立網路負載平衡器]
   - [建立端點服務組態]
   - [建立介面端點]
   - [介面端點生命週期]

- **Azure PrivateLink檔案**
   - [建立負載平衡器]
   - [Azure私人連結工作流程]

<!--Link definitions-->

[接受和拒絕介面端點連線要求]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[新增和移除端點服務的許可權]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon：疑難排解Azure私人連結連線問題]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS：疑難排解端點服務連線]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Azure私人連結工作流程]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[建立負載平衡器]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[建立網路負載平衡器]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[建立端點服務設定]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[建立介面端點]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[介面端點]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[管理私人端點連線]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[管理連線要求]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[私用端點]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
