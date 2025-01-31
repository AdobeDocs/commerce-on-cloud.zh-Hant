---
title: 以案例為基礎的部署
description: 瞭解如何使用自訂組態檔在雲端基礎結構部署上自訂Adobe Commerce。
feature: Cloud, Configuration, Deploy, Build
source-git-commit: 0d9d3d64cd0ad4792824992af354653f61e4388d
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# 以案例為基礎的部署

透過`ece-tools` 2002.1.0和更新版本，您可以使用情境式部署功能來自訂預設部署行為。
此功能在設定中使用**案例**&#x200B;和&#x200B;**步驟**：

- **案例組態** — 每個部署勾點都是&#x200B;*案例*，這是描述完成部署工作的順序和組態引數的XML組態檔。 您可以在`.magento.app.yaml`檔案的`hooks`區段中設定情境。

- **步驟組態** — 每個案例都使用一系列&#x200B;*步驟*，以程式設計方式說明完成部署工作所需的作業。 您可以在以XML為基礎的案例組態檔案中設定步驟。

雲端基礎結構上的Adobe Commerce在`ece-tools`套件中提供一組[預設情境](https://github.com/magento/ece-tools/tree/2002.1/scenario)和[預設步驟](https://github.com/magento/ece-tools/tree/2002.1/src/Step)。 您可以建立自訂XML組態檔來覆寫或自訂預設組態，以自訂部署行為。 您也可以使用案例和步驟，從自訂模組執行程式碼。

## 使用建立和部署鉤點新增案例

您將建置和部署Adobe Commerce的案例新增至`.magento.app.yaml`檔案的`hooks`區段。 每個掛接會指定每個階段要執行的情境。 下列範例顯示預設案例設定。

> `magento.app.yaml`個鉤點

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>隨著`ece-tools` 2002.1.x的發行，有新的[鉤點組態](https://experienceleague.adobe.com/docs/commerce-on-cloud/user-guide/configure/app/properties/hooks-property.html)格式。 仍支援`ece-tools` 2002.0.x版的舊版格式。 不過，您必須更新為新格式，才能使用以案例為基礎的部署功能。

## 檢閱案例步驟

在掛接設定中，每個案例都是XML檔案，其中包含執行建置、部署或部署後工作的步驟。 例如，`scenario/transfer`檔案包含三個步驟： `compress-static-content`、`clear-init-directory`和`backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## 擴充預設情境

以下範例藉由將其他部署組態檔案附加至掛接組態來擴充預設部署案例。

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

在部署期間，自訂情境會根據下列規則與預設情境合併：

- 情境會根據其在掛接定義中的順序來排定優先順序，最後列出的情境會具有最高的優先順序。

  在此範例中，案例的優先順序如下：

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` （預設或基準案例）

- 最高優先順序案例中的步驟會覆寫其他案例中具有相同名稱的步驟。 新步驟將新增至設定。 相同的規則適用於兩個以上的情境，每個情境都會從右到左排序，例如(C → B → A)。

### 移除預設步驟

您使用`skip`引數從預設案例移除步驟。

例如，若要略過預設部署案例中的`enable-maintenance-mode`和`set-production-mode`步驟，請建立包含下列設定的設定檔。

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

若要使用自訂組態檔，請更新預設的`.magento.app.yaml`檔案。

> 具有自訂部署情境的`.magento.app.yaml`

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### 取代預設步驟

自訂案例可以取代預設步驟，以提供自訂實施。 要執行此操作，請使用預設步驟名稱作為自訂步驟的名稱。

例如，在[預設部署案例]中，`enable-maintenance-mode`步驟會執行預設的[EnableMaintenanceMode PHP指令碼]。

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

若要覆寫此步驟，請建立自訂案例設定檔，以便在`enable-maintenance-mode`步驟執行時執行不同的指令碼。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### 變更步驟優先順序

自訂案例可以變更預設步驟的優先順序。 下列步驟會將`enable-maintenance-mode`步驟的優先順序從`300`變更為`10`，讓步驟在部署情境中執行得更早。

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

在此範例中，`enable-maintenance-mode`步驟會移至情境的開頭，因為其優先順序低於預設部署情境中的所有其他步驟。

### 範例：擴充部署案例

下列範例使用下列變更自訂[預設部署案例]：

- 以自訂步驟取代`remove-deploy-failed-flag`步驟
- 略過預先部署步驟中的`clean-redis-cache`子步驟
- 略過`unlock-cron-jobs`步驟
- 略過`validate-config`步驟以停用重要驗證器
- 新增預先部署步驟

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

若要在您的專案中使用此指令碼，請將以下設定新增至雲端基礎結構專案上Adobe Commerce的`.magento.app.yaml`檔案：

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>您可以檢閱`ece-tools` GitHub存放庫中的[預設案例](https://github.com/magento/ece-tools/tree/2002.1/scenario)和[預設步驟設定](https://github.com/magento/ece-tools/tree/2002.1/src/Step)，以決定針對您的專案建置、部署和部署後任務自訂哪些案例和步驟。

## 新增自訂模組以延伸`ece-tools`

`ece-tools`套件提供遵循語意版本標準的預設API介面。 所有API介面都標示有&#x200B;**@api**&#x200B;註解。 您可以建立自訂模組，並視需要修改預設程式碼，藉此將預設API實作取代為您自己的API。

若要在雲端基礎結構上透過Adobe Commerce使用自訂模組，您必須在`ece-tools`套件的擴充功能清單中註冊您的模組。 註冊程式與在Adobe Commerce中註冊模組時所用的程式類似。

**若要使用`ece-tools`封裝註冊模組**：

1. 在模組的根目錄中建立或擴充`registration.php`檔案。

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. 更新模組組態檔的`autoload`區段以包含`registration.php`檔案，以便在`composer.json`中自動載入模組檔案。

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. 將`config/services.xml`檔案新增至模組。 此設定已合併到`ece-tools`封裝中的`config/services.xml`上。

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

若要深入瞭解相依性插入，請參閱[Symfony相依性插入](https://symfony.com/doc/current/components/dependency_injection.html)。

<!-- link definitions -->

[預設部署案例]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[EnableMaintenanceMode PHP指令碼]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
