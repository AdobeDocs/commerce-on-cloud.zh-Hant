---
title: 變數層級和選項
description: 瞭解在雲端基礎結構專案執行階段環境中自訂Adobe Commerce時所使用的不同變數層級和選項。
feature: Cloud, Configuration, Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# 變數層級

專案變數適用於專案內的所有環境。 環境變數會套用至特定環境或分支。 環境&#x200B;_從父環境繼承_&#x200B;變數定義。

您可以特別為環境定義變數，藉此覆寫繼承的值。 例如，若要設定開發所需的變數，請在整合環境的`.magento.env.yaml`檔案中定義變數值。 所有從整合環境分支的環境都會繼承這些值。 請參閱[部署組態](configure-env-yaml.md)，以取得使用`.magento.env.yaml`檔案設定環境的詳細資訊。

>[!BEGINTABS]

>[!TAB CLI]

**若要使用Cloud CLI設定變數**：

- **專案特定變數** — 若要為專案中的&#x200B;_所有_&#x200B;環境設定相同的值。 這些變數可在所有環境的建置和執行階段使用。

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **環境特定變數** — 為&#x200B;_特定_&#x200B;環境設定唯一值。 這些變數可在執行階段使用，並由子環境繼承。 使用`-e`選項在命令中指定環境。

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

設定專案特定變數後，您必須手動重新部署遠端環境，變更才會生效。 推送新認可以觸發重新部署。

>[!TAB 主控台]

**若要使用[!DNL Cloud Console]**&#x200B;設定變數：

1. 在&#x200B;_[!DNL Cloud Console]_&#x200B;中，按一下專案導覽右側的設定圖示。

   ![設定專案](../../assets/icon-configure.png){width="36"}

1. 若要設定專案層級的變數，請在&#x200B;_專案設定_&#x200B;下按一下&#x200B;**變數**。

   ![專案變數](../../assets/ui-project-variables.png)

1. 若要設定環境層級變數，請在&#x200B;_環境_&#x200B;清單中選取一個環境，然後按一下&#x200B;**[!UICONTROL Variables]**&#x200B;索引標籤。

   ![環境變數標籤](../../assets/ui-environment-variables.png)

1. 按一下&#x200B;**[!UICONTROL Create variable]**。

1. 提供變數的名稱和值。 從下列選項中選擇：

   - 在執行階段期間可用
   - 可在建置期間使用
   - JSON值
   - 敏感變數（隱藏在主控台和CLI回應中的值）
   - 讓可繼承（子環境可繼承環境層級變數）

1. 按一下&#x200B;**[!UICONTROL Create variable]**。

>[!CAUTION]
>
>在[!DNL Cloud Console]中設定環境特定的變數會自動重新部署環境。

>[!ENDTABS]

## 可見度

您可以使用`--visible-<build|runtime>`命令，在建置或執行階段限制變數的可見度。 此外，也有設定繼承和敏感度的選項。

使用下列選項來防止變數被看到或繼承：

- `--inheritable false` — 停用子環境的繼承。 這對於在`master`分支上設定僅限生產值，以及允許所有其他環境使用相同名稱的專案層級變數很有用。
- `--sensitive true` — 在[!DNL Cloud Console]中將變數標示為&#x200B;_無法讀取_。 您無法在使用者介面中檢視變數；不過，您可以從應用程式容器中檢視變數，就像任何其他變數一樣。

下列範例示範無法看見或繼承變數的特定案例。 您只能在CLI中指定這些選項。 此案例並非適用於所有可用的環境變數。

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## 驗證變數層級和值

您可以使用Cloud CLI檢視現有變數的清單。

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
