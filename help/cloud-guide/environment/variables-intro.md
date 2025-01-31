---
title: 環境變數
description: 請參閱雲端基礎結構上Adobe Commerce專屬的環境變數清單。
feature: Cloud, Build, Configuration, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# 環境變數

雲端基礎結構上的Adobe Commerce可讓您指派環境變數來覆寫設定選項。 `ece-tools`套件根據[雲端變數](variables-cloud.md)、[!DNL Cloud Console]中設定的變數及`.magento.env.yaml`組態檔的值，在`env.php`檔案中設定值。

`.magento.env.yaml`檔案中的環境變數會覆寫您現有的Commerce設定來自訂雲端環境。 如果預設值為`Not Set`，則`ece-tools`封裝會採取&#x200B;**NO**&#x200B;動作，並使用[!DNL Commerce]預設值或來自`MAGENTO_CLOUD_RELATIONSHIPS`設定的值。 如果設定了預設值，則`ece-tools`套件會動作來設定該預設值。

環境變數的型別包括：

- [管理員](variables-admin.md) — 變數會覆寫專案管理員變數
- [MAGENTO_CLOUD](variables-cloud.md) — 雲端基礎結構特定的變數
- `.magento.env.yaml`檔案中使用的變數：
   - [全域](variables-global.md) — 變數會影響組建、部署和部署後階段
   - [建置](variables-build.md) — 變數控制建置動作
   - [部署](variables-deploy.md) — 變數控制部署動作
   - [部署後](variables-post-deploy.md) — 部署後的變數控制動作

變數是&#x200B;_階層式_，這表示如果變數未被覆寫，它會繼承自父環境。

您可以從[!DNL Cloud Console]或使用Adobe Commerce CLI設定[管理員變數](variables-admin.md)。 您可以從[`.magento.env.yaml`](configure-env-yaml.md)檔案中管理其他環境變數，以管理所有環境（包括Pro Staging和Production）中的建置和部署動作，而不需要支援票證。

>[!TIP]
>
>YAML檔案區分大小寫，不允許使用索引標籤。 請留意在`.magento.env.yaml`檔案中使用一致的縮排，否則您的設定可能無法如預期運作。 本檔案和範例檔案中的範例使用&#x200B;_雙空格縮排_。 使用[ece-tools驗證命令](configure-env-yaml.md#validate-configuration-file)檢查您的設定。
