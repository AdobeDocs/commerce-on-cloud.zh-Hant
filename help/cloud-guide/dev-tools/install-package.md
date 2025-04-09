---
title: 升級專案以使用ECE-Tools
description: 瞭解如何在雲端基礎結構專案上升級Adobe Commerce，以使用ECE-Tools套件並利用最新的修正和功能。
feature: Cloud, Install
exl-id: 164c47e4-c871-41a3-b268-581d426e7a7f
source-git-commit: 1cea1cdebf3aba2a1b43f305a61ca6b55e3b9d08
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# 升級專案以使用ECE-Tools套件

Adobe已棄用`magento/magento-cloud-configuration`和`magento/ece-patches`套件，而改用`ece-tools`套件，這會簡化許多雲端程式。 如果您在雲端基礎結構專案上使用舊版的Adobe Commerce，但&#x200B;_不_&#x200B;包含`ece-tools`套件，則必須執行一次性的手動&#x200B;_升級_&#x200B;程式至您的專案。

>[!WARNING]
>
>如果您的專案包含`ece-tools`套件，您可以略過下列升級。 若要驗證，請使用本機專案根目錄中的`php vendor/bin/ece-tools -V`命令擷取[!DNL Commerce]版本。

此專案升級程式需要您更新根目錄`composer.json`檔案中的`magento/magento-cloud-metapackage`版本限制。 此限制可讓您更新雲端基礎結構中繼資料的Adobe Commerce （包括移除已棄用的套件），而不需升級您目前的Adobe Commerce版本。

{{upgrade-tip}}

## 移除已棄用的套件

在執行升級以使用`ece-tools`封裝之前，請檢查`composer.lock`檔案中是否有下列已棄用的封裝：

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## 更新中繼資料

每個Adobe Commerce版本都需要不同的限制，基礎如下：

```
>=current_version <next_version
```

- 針對`current_version`，指定要安裝的Adobe Commerce版本。
- 針對`next_version`，請在`current_version`中指定的值之後指定下一個修補程式版本。

若要安裝Adobe Commerce `2.3.5-p2`，請將`current_version`設為`2.3.5`，並將`next_version`設為`2.3.6`。 條件約束`">=2.3.5 <2.3.6"`會安裝2.3.5的最新可用套件。

您一律可以在[`magento-cloud`範本](https://github.com/magento/magento-cloud/blob/master/composer.json)中找到最新的中繼封裝條件約束。

下列範例會將雲端基礎結構中繼資料上Adobe Commerce的限制，設為大於或等於目前版本2.4.8且小於下一個版本2.4.9的任何版本：

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.8 <2.4.9"
},
```

## 升級專案

若要升級您的專案以使用`ece-tools`套件，您必須更新中繼套件和`.magento.app.yaml`鉤點屬性，然後執行Composer更新。

**若要升級專案以使用ece-tools**：

1. 更新`composer.json`檔案中的`magento/magento-cloud-metapackage`版本限制。

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.8 <2.4.9" --no-update
   ```

1. 更新中繼資料。

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. 修改`magento.app.yaml`檔案中的掛接命令。

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. 檢查並移除[已棄用的封裝](#remove-deprecated-packages)。 已棄用的套件可能會阻礙成功升級。

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. 可能需要更新`ece-tools`封裝。

   ```bash
   composer update magento/ece-tools
   ```

1. 新增並認可程式碼變更。 在此範例中，已更新下列檔案：

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. 將您的程式碼變更推送至遠端伺服器，並將此分支與`integration`分支合併。

   ```bash
   git push origin <branch-name>
   ```
