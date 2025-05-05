---
title: 設定傳出電子郵件
description: 瞭解如何在雲端基礎結構上為Adobe Commerce啟用傳出電子郵件。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 0%

---

# 設定傳出電子郵件

您可以從[!DNL Cloud Console]或命令列啟用或停用整合（以及僅供入門者使用的測試）環境的傳出電子郵件。 啟用外寄電子郵件以傳送雙因素驗證，或為Cloud專案使用者重設密碼電子郵件。

依預設，傳出電子郵件會在生產和預備（僅限Pro）環境中啟用。 不過，在您透過[命令列](#enable-emails-in-the-cli)或[雲端主控台](outgoing-emails.md#enable-emails-in-the-cloud-console)設定`enable_smtp`屬性之前，**[!UICONTROL Enable outgoing emails]**&#x200B;設定在環境設定中可能會顯示為停用，無論狀態為何。

[命令列](#enable-emails-in-the-cli)更新`enable_smtp`屬性值也會變更Cloud Console上此環境的[!UICONTROL Enable outgoing emails]設定值。

>[!NOTE]
>
>啟用/停用&#x200B;**[!UICONTROL Enable outgoing emails]**&#x200B;設定將不會在Pro測試或生產環境中啟用/停用電子郵件。

{{redeploy-warning}}

## 在Cloud Console中啟用電子郵件

在&#x200B;_設定環境_&#x200B;檢視中使用&#x200B;**[!UICONTROL Outgoing emails]**&#x200B;切換來啟用或停用電子郵件支援。

如果外寄電子郵件必須在Pro生產或測試環境中停用或重新啟用，您可以提交[Adobe Commerce支援票證](https://experienceleague.adobe.com/zh-hant/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide)。

>[!TIP]
>
>在Cloud Console中，Pro測試或生產環境可能不會反映傳出電子郵件狀態。

**若要管理來自[!DNL Cloud Console]**&#x200B;的電子郵件支援：

1. 登入[[!DNL Cloud Console]](https://console.adobecommerce.com)。
1. 從&#x200B;_所有專案_&#x200B;清單中選取專案。
1. 在「專案」控制面板上，按一下右上方的設定圖示。
1. 按一下&#x200B;**[!UICONTROL Environments]**&#x200B;並從清單中選取特定環境（Staging和Production for Pro除外）。
1. 若要啟用或停用傳出電子郵件，請切換&#x200B;_啟用傳出電子郵件_ **開啟**&#x200B;或&#x200B;**關閉**。

   ![啟用傳出電子郵件組態](../../assets/outgoing-emails.png)

變更設定後，環境會使用新設定進行建置和部署。

## 在CLI中啟用電子郵件

您可以使用`magento-cloud` CLI `environment:info`命令來設定`enable_smtp`屬性，以變更使用中環境的電子郵件設定。 啟用SMTP會以傳送郵件之SMTP主機的IP位址來更新`MAGENTO_CLOUD_SMTP_HOST`環境變數。

**若要從命令列管理電子郵件支援**：

1. 在本機工作站上，變更至專案目錄。

1. 檢查環境的傳出電子郵件設定。

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. 將`enable_smtp`環境變數設定為`true`或`false`以變更電子郵件支援設定。

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   等待環境建置和部署。

1. 使用SSH登入遠端環境。

1. 驗證電子郵件是否有效；傳送測試電子郵件至您可檢查的地址。

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. 驗證SendGrid是否擷取電子郵件。

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
