---
title: SendGrid電子郵件服務
description: 瞭解雲端基礎結構上適用於Adobe Commerce的SendGrid電子郵件服務，以及如何測試您的DNS設定。
exl-id: 06236068-df32-468f-99ec-c379984be136
source-git-commit: 0cb86dd5e4fe627b198ac3c1a6b14607f377a9a3
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 0%

---

# SendGrid電子郵件服務

SendGrid簡單郵件傳輸通訊協定(SMTP) Proxy服務提供輸出電子郵件驗證和信譽監視服務，包括支援：

* 所有傳出異動電子郵件
* 專用IP位址
* 網域註冊、用於電子郵件網域驗證的DomainKeys Indified Mail (DKIM)簽名（僅適用於Pro測試和生產環境）
* 自訂網域註冊（僅適用於Pro）
* 簡易與專業整合環境的自動化整合。 Pro生產和測試環境需要在基礎建設即服務(IaaS)硬體布建程式期間手動布建和設定

SendGrid SMTP Proxy並非旨在作為一般用途電子郵件伺服器來接收傳入電子郵件，或是用於電子郵件行銷活動。 如果您打算匯入並傳送歡迎電子郵件給大量客戶（例如，10,000或更多），請將匯入和電子郵件傳送分割為多天。 此做法有助於遵循每日傳送限制，並保護您的寄件者聲譽。

>[!TIP]
>
>請前往「商店>設定>一般」 ，確認您已在「管理員」中設定適當的商店電子郵件地址，以避免傳遞能力與網域驗證的問題。 您必須取消核取&#x200B;**[!UICONTROL Use Default]**，並將預設值取代為您擁有的網域。 當透過Sendgrid傳送電子郵件時，不應將公用/共用網域電子郵件服務(例如gmail.com和outlook.com)設定為寄件者電子郵件地址。

## 啟用或停用電子郵件

您可以從Cloud Console或命令列為每個環境啟用或停用傳出電子郵件。

依預設，在Pro生產和中繼環境中會啟用外寄電子郵件。 不過，在您透過[!UICONTROL Outgoing emails]命令列`enable_smtp`或[雲端主控台](outgoing-emails.md#enable-emails-in-the-cli)設定[屬性之前，](outgoing-emails.md#enable-emails-in-the-cloud-console)可能在環境設定中顯示為停用。 您可以為整合和中繼環境啟用傳出電子郵件，以傳送雙因素驗證或為雲端專案使用者重設密碼電子郵件。 請參閱[設定電子郵件以進行測試](outgoing-emails.md)。

如果外寄電子郵件必須在Pro生產或測試環境中停用或重新啟用，您可以提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)。

>[!TIP]
>
>[!UICONTROL enable_smtp]命令列[更新](outgoing-emails.md#enable-emails-in-the-cli)屬性值也會在[!UICONTROL Enable outgoing emails]雲端主控台[上變更此環境的](outgoing-emails.md#enable-emails-in-the-cloud-console)設定值。

## SendGrid控制面板

所有雲端專案都可在中央帳戶下管理，因此只有「支援人員」可以存取SendGrid儀表板。 SendGrid不提供附屬帳戶限制功能。

若要檢閱活動記錄檔的傳遞狀態或已退回、已拒絕或已封鎖電子郵件地址的清單，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)。 支援團隊&#x200B;**無法**&#x200B;擷取超過30天的活動記錄。

可能的話，請在請求中加入下列資訊：

* 受影響的電子郵件地址
* 有問題的時間範圍（僅限過去30天內）
* 電子郵件的主旨

## 網域金鑰識別郵件(DKIM)

DKIM是一種電子郵件驗證技術，可讓網際網路服務提供者(ISP)識別合法和虛假的寄件者地址，此技術通常用於網路釣魚和電子郵件詐騙。 DKIM仰賴管理DNS記錄的網域擁有者。 使用DKIM時，傳送者伺服器會使用私密金鑰來簽署訊息。 此外，網域擁有者會將修改過的`TXT`記錄DKIM記錄新增至寄件者網域的DNS記錄。 此`TXT`記錄包含收件者郵件伺服器用來驗證郵件簽章的公開金鑰。 DKIM公開金鑰加密程式可讓收件者驗證傳送者的真實性。 請參閱[DKIM記錄說明](https://docs.sendgrid.com/ui/account-and-settings/dkim-records)。

>[!WARNING]
>
>SendGrid DKIM簽名和網域驗證支援僅適用於Pro專案的生產和中繼環境，不適用於所有入門環境。 因此，傳出異動電子郵件可能會被垃圾郵件篩選器標幟。 使用DKIM可改善已驗證電子郵件寄件者的傳送率。 若要改善郵件傳送率，您可以從Starter升級為Pro，或使用您自己的SMTP伺服器或電子郵件傳送服務提供者。 請參閱[系統管理系統指南](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications)中的&#x200B;_設定電子郵件連線_。

### 傳送者與網域驗證

若要讓SendGrid代表您從Pro Production或Staging環境傳送交易式電子郵件，您必須設定DNS設定以包含三個SendGrid子網域DNS專案。 每個SendGrid帳戶都會被指派唯一的`TXT`記錄，用於驗證傳出電子郵件。

>[!TIP]
>
>請確定您使用&#x200B;**[!UICONTROL S中的適當網域來設定]**&#x200B;儲存電子郵件地址&#x200B;**[!UICONTROL Stores > Configuration > General > Store Email Addresses]**。 對寄件者的電子郵件地址執行網域驗證。 如果設定了預設設定(`example.com`)，Sendgrid將會封鎖來自`example.com`的電子郵件。

**若要啟用網域驗證**：

1. 提交[支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以請求為特定網域啟用DKIM （**僅限Pro測試和生產環境**）。
1. 使用在支援票證中提供給您的`TXT`和`CNAME`記錄更新您的DNS設定。

**帳戶識別碼為`TXT`的範例**&#x200B;記錄：

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**範例`CNAME`記錄**：

| 網域 | 指向 | 記錄型別 |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1。_domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2。_domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### DKIM簽名和自動化安全性

設定已驗證的網域時，您可以在自動和手動安全性之間選擇。 如果您選擇自動安全性，SendGrid會自動管理您的DKIM和SPF記錄。 當您新增專屬傳送IP位址至帳戶時，SendGrid會立即更新您的DNS設定和DKIM簽名。 如果您關閉自動安全性，您有責任在每次變更傳送網域時更新DKIM簽名。

**已啟用自動化安全性的範例**：

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**已停用自動化安全性範例**：

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

設定網域驗證後，SendGrid會自動為您處理安全性原則架構(SPF)和DKIM記錄。 在SendGrid提供要新增至您的DNS記錄的`CNAME`記錄之後，您可以新增專用的IP位址並進行其他帳戶更新，而無需手動管理您的SPF記錄。 請參閱[自動安全性與您的DKIM簽章](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature)。

若要測試您的DNS設定：

```
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## 異動電子郵件臨界值

交易式電子郵件臨界值是指在特定時段內您可從Pro環境傳送的交易式電子郵件訊息數量，例如每月從非生產環境傳送12,000封電子郵件。 此臨界值旨在防止傳送垃圾郵件，並防止可能對您的電子郵件信譽造成損害。

只要寄件者信譽分數超過95%，生產環境中可傳送的電子郵件數量就沒有嚴格限制。 信譽受退回或拒絕的電子郵件數量以及基於DNS的垃圾郵件註冊是否將您的網域標籤為潛在垃圾郵件來源的影響。 檢視[Commerce支援知識庫](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded)中Adobe Commerce _超過SendGrid信用額時未傳送的_&#x200B;電子郵件。

**若要檢查是否已超過最大積分**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 檢查`/var/log/mail.log`中是否有`authentication failed : Maxium credits exceeded`個專案。

   如果您看到任何`authentication failed`個記錄專案，且&#x200B;**電子郵件傳送信譽**&#x200B;至少為95，您可以[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以要求增加信用配額。

>[!NOTE]
>
>`var/log/mail.log`檔案是執行記錄檔&#x200B;*的*。 當加入新專案時，較舊的專案會隨著時間從檔案中移除。 記錄中只有最近的記錄活動可用。 從mail.log移除後，不會封存或保留較舊的記錄專案。

### 電子郵件傳送信譽

電子郵件傳送信譽是由網際網路服務提供者(ISP)指派給傳送電子郵件訊息之公司的分數。 分數越高，ISP將訊息傳送到收件者收件匣的可能性就越大。 如果分數低於特定等級，ISP可能會將郵件路由到收件者的垃圾郵件資料夾，或甚至完全拒絕郵件。 信譽分數由數個因素決定，例如您的IP位址排名與其他IP位址的30天平均值和垃圾郵件投訴率。 檢視[8檢查電子郵件傳送信譽的方法](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation)。

### 電子郵件隱藏清單

電子郵件隱藏清單是如果傳送電子郵件會影響您的傳送信譽和傳送率，則不應向其傳送電子郵件的收件者清單。 CAN-SPAM Act要求確保電子郵件寄件者有選擇退出收件者的方法，這些收件者會取消訂閱電子郵件或將電子郵件標示為垃圾郵件。 隱藏清單也會收集跳出、遭封鎖或無效的電子郵件。

若要防止電子郵件從一開始就傳送到垃圾郵件資料夾，請遵循Sendgrid的最佳實務文章，[為什麼我的電子郵件會傳送至垃圾郵件？](https://sendgrid.com/en-us/blog/10-tips-to-keep-email-out-of-the-spam-folder)。

如果某些收件者沒有收到您的電子郵件，您可以[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket)以要求檢閱隱藏清單，並視需要移除收件者。

如需詳細資訊，請參閱[什麼是隱藏清單？](https://sendgrid.com/en-us/blog/what-is-a-suppression-list)
