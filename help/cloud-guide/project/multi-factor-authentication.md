---
title: 啟用SSH存取的多重驗證
description: 瞭解如何管理雲端基礎結構環境中SSH存取Adobe Commerce的驗證需求。
feature: Cloud, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# 啟用SSH存取的多重驗證

為了提高安全性，雲端基礎結構上的Adobe Commerce提供多重要素驗證(MFA)實作，以管理對雲端環境的SSH存取的驗證需求。

在專案上啟用MFA時，所有具有SSH存取權的使用者帳戶都需要雙因素驗證(TFA)代碼或API權杖和SSH憑證才能存取環境。

>[!NOTE]
>
>雲端專案預設不會啟用MFA。 雲端基礎結構專案上Adobe Commerce的帳戶擁有者必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)才能啟用它。 啟用MFA時，所有使用者必須在雲端基礎結構帳戶上的Adobe Commerce上啟用雙因素驗證(TFA)，才能以ssh存取專案環境。

## SSH存取的憑證

MFA可讓使用者將OAUTH存取權杖與Adobe雲端認證者API產生的短期SSH憑證交換。 如果使用者擁有管理員或貢獻者角色、有效的SSH金鑰和有效的TFA代碼或API權杖，雲端基礎結構上的Adobe Commerce會使用這些憑證來產生暫時SSH憑證。 憑證到期時間設為一小時，但會在目前的工作階段期間自動重新整理。

使用MFA登入專案後，使用者必須使用`magento-cloud` CLI來產生SSH憑證：

```bash
magento-cloud ssh-cert:load
```

`ssh-cert:load`命令會產生SSH憑證，並將其安裝在本機使用者的SSH代理程式中。

### 登入時自動產生憑證

您可以設定本機環境，在您向`magento-cloud` CLI驗證時自動產生SSH憑證。

**若要將SSH憑證自動產生新增到您的`magento-cloud` CLI設定**：

1. 在本機工作站上，在首頁目錄的`.magento-cloud`資料夾中建立名為`config.yaml`的檔案（如果不存在）。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 將下列設定新增至`config.yaml`檔案。

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. 使用`magento-cloud` CLI再次驗證：

   >登出：

   ```bash
   magento-cloud logout
   ```

   >登入：

   ```bash
   magento-cloud login
   ```

   >請依照回應操作：

   ```
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## 透過TFA使用SSH連線至環境

在專案上啟用MFA時，您必須先在帳戶上啟用TFA，才能使用SSH連線到遠端環境。 請參閱[啟用TFA](user-access.md#enable-tfa-for-cloud-accounts)。

>[!BEGINSHADEBOX]

**必要條件：**

對於啟用MFA強制的專案，SSH存取需要下列許可權和帳戶設定：

- [管理員或貢獻者對環境的存取權](user-access.md)
- [帳戶上設定的SSH存取金鑰](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA已啟用（帳戶上）](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**若要使用SSH與TFA使用者帳戶認證連線**：

1. 登入[您的帳戶](https://console.adobecommerce.com)。

1. 在您的本機工作站上，使用`magento-cloud` CLI來產生SSH憑證。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 範例回應：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. 使用SSH連線到遠端環境。

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## 透過TFA使用SSH管理原始程式碼

在雲端基礎結構專案上管理Adobe Commerce的原始程式碼時，您會使用SSH來驗證專案的Git存放庫。 如果您的專案已啟用MFA強制執行，您必須先產生SSH憑證，才能使用Git存放庫執行命令列作業。

**若要使用SSH與TFA使用者帳戶認證連線**：

1. 登入[您的帳戶](https://console.adobecommerce.com)並使用TFA進行驗證。

   >[!NOTE]
   >
   >如果您的帳戶未啟用TFA，則必須啟用。 請參閱[在雲端帳戶](user-access.md#enable-tfa-for-cloud-accounts)上啟用TFA。

1. 在您的本機工作站上，使用`magento-cloud` CLI來產生SSH憑證。

   ```bash
   magento-cloud ssh-cert:load
   ```

   > 範例回應：

   ```
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. 複製專案環境的Git存放庫：

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > 範例回應：

   ```
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## 透過API權杖使用SSH連線至環境

在專案上啟用MFA時，需要SSH存取雲端環境的自動化程式需要API權杖。 您可以透過雲端基礎結構帳戶上的Adobe Commerce ，以專案的管理員或貢獻者存取權產生代號。

使用API權杖進行驗證仍需要產生SSH憑證。 自動化程式也必須自動產生SSH憑證。

>[!BEGINSHADEBOX]

**必要條件：**

- [在雲端基礎結構環境中以管理員或貢獻者身分存取Adobe Commerce](user-access.md)
- [帳戶上可用的有效API權杖](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**若要使用SSH與API權杖認證連線**：

1. 使用API金鑰驗證登入雲端專案。

   ```bash
   magento-cloud auth:api-token
   ```

1. 在提示下，輸入有效API權杖的值。

   ```
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### 範例：自動化SSH指令碼

儲存API權杖有兩個選項。

>[!NOTE]
>
>如果儲存API權杖，`magento-cloud` CLI會自動驗證，而且不需要執行`magento-cloud login`命令。

**選項1**：建立環境變數以儲存API權杖

將權杖寫入您的bash_profile

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**選項2**：將權杖新增至`config.yaml`檔案

1. 在本機工作站上，在首頁目錄的`.magento-cloud`資料夾中建立名為`config.yaml`的檔案（如果不存在）。

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. 將下列設定新增至`config.yaml`檔案。

   ```yaml
   api:
      token: <your api token>
   ```

>bash指令碼範例

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## 疑難排解

使用下列資訊解決SSH連線要求失敗，因為驗證錯誤，例如`access requires MFA`或`permission denied`。

### 您的要求未提供有效的憑證

如果您的要求未提供有效的憑證，系統會顯示類似下列的訊息：

```
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

請嘗試下列疑難排解程式來解決連線問題：

- 驗證帳戶TFA設定
- 再次驗證，然後重新載入憑證

**若要驗證TFA組態和驗證**：

1. 登入[您的帳戶](https://console.adobecommerce.com)。

1. 在右上角帳戶功能表中，按一下&#x200B;**[!UICONTROL My Profile]**。

1. 在&#x200B;_我的設定檔_&#x200B;頁面上，按一下「**[!UICONTROL Security]**」標籤。

   如果TFA已啟用，安全性區段會提供管理TFA設定的選項。

1. 如果未設定TFA，請按一下&#x200B;**[!UICONTROL Set up application]**&#x200B;並依照指示啟用它。 請參閱[啟用TFA](user-access.md#enable-tfa-for-cloud-accounts)。

1. 如果設定了TFA，請嘗試再次驗證。

**若要驗證及重新載入SSH憑證**：

1. 使用`magento-cloud` CLI再次驗證：

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. 重新載入SSH憑證：

   ```bash
   magento-cloud ssh-cert:load
   ```

### 許可權遭拒

如果SSH金鑰遺失或無效，SSH連線要求會傳回`Permission denied (publickey)`錯誤。

```
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

若要修正此問題，請將SSH金鑰新增至您目前的作業階段，或更新SSH設定檔案以自動載入SSH金鑰。 請參閱[新增公開SSH金鑰](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)。

### 無法在沒有MFA的情況下存取專案

如果您對已啟用多重驗證(MFA)的專案進行驗證，則在連線至不需要MFA的其他專案時，可能會收到下列錯誤：

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

範例回應：

```
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

在SSH憑證產生期間，`magento-cloud` CLI會將額外的SSH金鑰新增至您的本機環境。 如果您的本機SSH設定不包含用於專案存取的SSH金鑰，則預設會使用該金鑰。

**若要將您的SSH金鑰新增至本機設定**：

1. 建立`config`檔案（若不存在）。

   ```bash
   touch ~/.ssh/config
   ```

1. 新增`IdentityFile`設定。

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>您可以新增多個`IdentityFile`專案至您的設定，以指定多個SSH金鑰。
