---
title: 設定靜態檔案的快取
description: 瞭解如何在[!DNL Commerce]應用程式設定檔案中設定快取儲存選項。
feature: Cloud, Configuration, Cache, SCD
exl-id: 0f577974-85d7-4972-8f03-856aa6accaae
TQID: 'https://experienceleague.adobe.com/ZA0WRB9p4Gpi7kjWxNPS5uCSfaTmrkrqxc4SgC9y9og'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: d05f97c9-0a96-5792-92cf-f66ce7326e3a
    internal-label: SCD
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: e6e0bd8e116b2f0b93557b6aeb2aac7cbb8e1d8a
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%
---
# 設定靜態檔案的快取

您媒體和靜態檔案的快取TTL （存留時間）是使用`expires`索引鍵在`.magento.app.yaml`設定檔案中設定的。

>[!NOTE]
>
>在更新生產環境之前，請務必在中繼環境中測試變更。 [提交Adobe Commerce支援票證](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-commerce-support/adobe-commerce-help-center-user-guide#submit-ticket)，以取得更新這些環境設定的協助。

1. 在`.magento.app.yaml`檔案的[`web`屬性](web-property.md)中指定TTL時間（秒）。 您可以在`locations`底下或在`"/media"`與`"/static"`底下新增`expires`金鑰。

   若要防止快取到期，請使用`expires: -1`機碼值組。 請參閱下列範例：

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```

