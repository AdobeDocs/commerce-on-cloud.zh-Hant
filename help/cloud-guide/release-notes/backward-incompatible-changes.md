---
title: 與舊版不相容的變更
description: 瞭解在升級現有Cloud專案時的回溯相容性。
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 3f3c1036-bfd0-4c70-8309-6c5e442134cd
source-git-commit: de50fda78c28a57d76e5c0a4d5dac0f8d4d844a0
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# 與舊版不相容的變更

當您升級到最新版本的`ece-tools`套件或其他Commerce套件的雲端工具套裝時，與回溯不相容的變更可能會要求您調整現有雲端專案的雲端設定和程式。

## `ece-tools`套件的變更

先前包含在`ece-tools`封裝中的某些功能現在會以個別封裝提供。 這些套件是`ece-tools`的撰寫器相依性，會在您安裝或更新ece-tools時自動安裝及更新。

新架構不應影響您的安裝或更新流程。 不過，在雲端基礎結構專案上使用Adobe Commerce時，您可能需要變更一些命令語法和程式。 如需詳細資訊，請檢閱下列與舊版不相容的變更資訊，以及[Cloud Tools Suite發行說明](cloud-tools-suite.md)。

### 服務版本需求變更

我們將使用`ece-tools` v2002.1.0和更新版本的雲端專案的最低PHP版本要求從7.0.x變更為7.1.x。 如果您的環境組態指定PHP 7.0，請更新[檔案中的](../application/php-settings.md)php組態`.magento.app.yaml`。

>[!WARNING]
>
>由於PHP版本需求變更，`ece-tools` 2002.1.0在執行Adobe Commerce 2.1.15或更新版本的雲端基礎結構專案上僅支援Adobe Commerce。 如果您的專案使用舊版，您必須[升級](../development/commerce-version.md)，才能更新至`ece-tools` 2002.1.0。

### 環境設定變更

下表提供在`ece-tools` v2002.1.0中已移除或棄用的環境變數和其他環境組態檔的相關資訊。

| 專案 | 替代方案 |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES`變數 | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS`變數 | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT`變數 | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK`變數 | 無。 現在，組建一律會建立指向靜態內容目錄`pub/static`的符號連結。 |
| `build_options.ini`檔案 | 使用[`.magento.env.yaml`](../application/configure-app-yaml.md)檔案來設定環境變數，以管理所有環境中的建置和部署動作。<p>如果您建置包含`build_options.ini`檔案的雲端環境，建置會失敗。 |

### CLI命令變更

下表總結了ECE-Tools v2002.1.0中CLI指令的變更，這些變更可能需要您更新指令或指令碼。

| 命令 | 替代方案 |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

在舊版ECE-Tools中，您可以使用`m2-ece-build`和`m2-ece-deploy`命令來設定`.magento.app.yaml`檔案中的部署掛接。 當您更新至v2002.1.0時，請檢查`hooks`檔案中的`.magento.app.yaml`設定是否有過時的命令，並視需要加以取代。

## 雲端修補程式變更

- **移除已下載的修補程式**- `magento/magento-cloud-patches`套件會將[軟體下載](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html?lang=zh-Hant)頁面上所有可用的修補程式套裝，並在您部署至雲端時自動套用這些修補程式。 為避免升級至ECE-Tools 2002.1.0或更新版本後發生修補程式衝突，請移除您手動下載並新增至專案的Adobe提供的任何修補程式。

- **更新套用修補程式命令** — 我們將套用修補程式的命令從`vendor/bin/ece-tools`目錄移至`vendor/bin/ece-patches`目錄。 如果您使用此指令來手動套用修補程式，請使用新路徑。

  > 手動套用修補程式

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Cloud Docker變更

- **PHP的最低版本要求現在是PHP 7.1** — 如果您的Commerce雲端檔案管理員主機執行的是舊版，請升級至PHP v7.1或更新版本。

- **適用於Commerce的雲端Docker命令變更**-

   - **正在更新Docker建置作業中Commerce命令的Cloud Docker** — 我們已將適用於Commerce命令的Cloud Docker從`vendor/bin/ece-tools`目錄移至`vendor/bin/ece-docker`目錄。 更新指令碼和命令以使用新路徑。

     升級至`ece-tools` 2002.1.0之後，請使用下列命令檢視可用的`ece-docker`命令。

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **正在更新Cloud Docker-compose命令** — 我們將命令檔案的路徑從`./bin/docker`重新命名為`./bin/magento-docker`。 更新指令碼和命令以使用新路徑。

   - **Cron容器不再包含在預設Docker設定中** — 現在，您必須將`--with-cron`選項新增到`ece-docker build:compose`命令以在Docker環境設定中包含Cron容器。 請參閱[Commerce適用的Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs)指南中的&#x200B;_管理cron作業_。

     先前使用cron作業產生容器的指令碼現在不含cron容器。

   - **使用暫存容器** — 在舊版中，`bin/magento-docker`命令作業所建立的容器並未移除，因此您可以將其用於其他作業。 現在，`magento-docker`命令會移除命令完成後所建立的任何容器。

     如果要保留由Docker-Compose操作建立的容器，請使用`docker-compose run`命令，而不是`bin/magento-docker`命令。

   - **正在執行部署後鉤點**- `cloud-deploy`命令不再執行部署後鉤點。 使用新的`cloud-post-deploy`命令在您部署後執行部署後掛接。 更新您的指令碼以新增命令以執行部署後鉤點。

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     或者，如果您直接使用`docker-compose`命令，請在部署命令之後執行`docker-compose run deploy cloud-post-deploy`命令。

- **正在重新整理資料庫** — 資料庫容器現在儲存在`magento-db`永久Docker磁碟區。 當您重新整理Docker環境時，資料庫不再自動刪除。 如有需要，請使用下列其中一個指令來手動移除它。

   - 移除`magento-db`容器：

     ```bash
     docker volume rm magento-db
     ```

   - 關閉Docker容器時移除所有關聯的磁碟區：

     ```bash
     docker-compose down -v
     ```

- **覆寫封存和備份檔案的檔案同步設定** — 使用docker-sync或mutagen時，具有下列副檔名的封存和備份檔案不再同步：SQL、GZ、ZIP和BZ2。 您可以將檔案重新命名為以不同的副檔名結尾，以覆寫這些檔案型別的預設檔案同步化。 例如： `synchronize-me.zip-backup`
