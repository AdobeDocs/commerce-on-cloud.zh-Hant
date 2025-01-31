---
title: '[!DNL ECE-Tools]封裝'
description: 瞭解 [!DNL ECE-Tools] 套件，以及它如何協助管理和部署Adobe Commerce。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# ECE-Tools套件

[!DNL ECE-Tools]套件是一組指令碼和工具，設計用來管理和部署[!DNL Commerce]應用程式。 `ece-tools`封裝簡化了許多程式，例如管理cron工作、驗證專案組態以及套用Adobe修補程式和修補程式。 您可以在GitHub][ece-repo]上檢視並貢獻[開放原始碼 [!DNL ECE-Tools] 程式碼存放庫。

{{ece-tools-package}}

`ece-tools`套件與Adobe Commerce相容（從2.1.4版開始），並包含雲端基礎結構命令上的指令碼和Adobe Commerce，可協助管理您的程式碼並自動建置和部署您的專案。

下列為可用的`ece-tools`命令：

```bash
php ./vendor/bin/ece-tools list
```

## 建置和部署

`ece-tools`套件包含命令，可執行雲端基礎結構應用程式上啟動Adobe Commerce的組建、部署和部署後階段的作業。 例如，`php ./vendor/bin/ece-tools build`命令會開始應用程式建置程式。

依預設，這些`ece-tools`命令位於`.magento.app.yaml`組態檔的[鉤點屬性](../application/hooks-property.md)中。

## Docker配置生成器

`ece-tools`套件包含[magento/magento-cloud-docker]套件的相依性，其為Docker影像提供功能和設定檔案，以啟動雲端基礎結構上Adobe Commerce的Docker開發環境。 您也可以以獨立套件的形式執行Commerce適用的Cloud Docker 。 請參閱[Docker開發](../dev-tools/cloud-docker.md)。

## 服務、路由和變數

您可以使用`ece-tools`封裝來顯示有關在任何雲端環境中使用的Base64編碼的[雲端變數](../environment/variables-cloud.md)的詳細資訊。 下列指令顯示所有服務、路由和變數。

```bash
php ./vendor/bin/ece-tools env:config:show
```

若要顯示特定資訊集，請使用下列格式：

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` — 顯示來自`MAGENTO_CLOUD_RELATIONSHIPS`環境變數（定義於`services.yaml`檔案中）的關係資料。
- `routes` — 使用`MAGENTO_CLOUD_ROUTES`環境變數顯示專案的已設定路由。
- `variables` — 使用`MAGENTO_CLOUD_VARIABLES`環境變數顯示專案的已設定變數。

`services`選項的範例輸出：

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## 驗證環境設定

有一組驗證指令可用來協助評估專案的組態。 如需每個精靈命令的詳細描述，請參閱&#x200B;_最佳化部署_&#x200B;區段中的[智慧型精靈](../deploy/smart-wizards.md)。 `wizard:ideal-state`命令會在建置階段自動執行。 若要確認專案的理想狀態：

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>您必須在遠端雲端環境中執行`wizard:ideal-state`命令。 命令在本機開發環境中執行時，一律會傳回`The configured state is not ideal`錯誤。

範例輸出：

```
Ideal state is configured
```

請參閱ece-tools](../release-notes/cloud-tools-suite.md)的[發行說明。

## Adobe修補程式和自訂修補程式

`ece-tools`套件包含對[magento/magento-cloud-patches]套件的相依性，此套件提供Adobe修補程式和修補程式，可改善所有Adobe Commerce版本與雲端環境的整合，並支援快速傳送關鍵修正程式。 「 」也會提供您新增至雲端基礎結構專案Adobe Commerce的自訂修補程式。 請參閱[套用修補程式](../development/apply-patches.md)。

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
