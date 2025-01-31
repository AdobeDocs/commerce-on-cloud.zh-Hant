---
title: 設定MySQL服務
description: 瞭解如何在雲端基礎結構上使用Adobe Commerce管理用於永久資料儲存的MySQL服務。
feature: Cloud, Services, Storage
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# 設定MySQL服務

`mysql`服務提供以[MariaDB](https://mariadb.com/)版本10.2到10.4為基礎的持續性資料儲存，支援[XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html)儲存引擎，並重新實作MySQL 5.6和5.7的功能。

與其他MariaDB或MySQL版本相比，在MariaDB 10.4上重新索引需要更多時間。 請參閱&#x200B;_效能最佳實務_&#x200B;指南中的[索引子](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers)。

>[!WARNING]
>
>將MariaDB從10.1版升級至10.2版時請小心。MariaDB 10.1是支援&#x200B;_XtraDB_&#x200B;做為儲存引擎的最後一個版本。 MariaDB 10.2使用&#x200B;_InnoDB_&#x200B;做為儲存引擎。 從10.1升級至10.2後，您無法復原變更。 Adobe Commerce同時支援兩種儲存引擎；不過，您必須檢查擴充功能及專案使用的其他系統，以確保其與MariaDB 10.2相容。請參閱[10.1與10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102)之間的不相容變更。

{{service-instruction}}

**若要啟用MySQL**：

1. 將必要的名稱、型別和磁碟值（以MB為單位）新增至`.magento/services.yaml`檔案。

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >磁碟空間不足可能導致MySQL錯誤，例如`PDO Exception: MySQL server has gone away`。 請確認您已在[`.magento/services.yaml`](services-yaml.md#disk)檔案中為服務配置足夠的磁碟空間。

1. 設定`.magento.app.yaml`檔案中的關聯性。

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. 新增、提交和推送您的程式碼變更。

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [驗證服務關係](services-yaml.md#service-relationships)。

{{service-change-tip}}

## 設定MySQL資料庫

設定MySQL資料庫時，有以下選項：

- **`schemas`** — 結構描述會定義資料庫。 預設結構描述是`main`資料庫。
- **`endpoints`** — 每個端點代表具有特定許可權的認證。 預設端點是`mysql`，具有`main`資料庫的`admin`存取權。
- **`properties`** — 屬性用於定義其他資料庫組態。

以下是`.magento/services.yaml`檔案中的基本範例設定：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

上述範例中的`properties`將預設`optimizer`設定修改為「效能最佳實務指南](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers)」中的[建議。

**MariaDB組態選項**：

| 選項 | 說明 | 預設值 |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | 預設字元集。 | utf8mb4 |
| `default_collation` | 預設定序。 | utf8mb4_unicode_ci |
| `max_allowed_packet` | 封包大小上限（以MB為單位）。 範圍`1`到`100`。 | 16 |
| `optimizer_switch` | 設定查詢最佳化程式的值。 請參閱[MariaDB檔案](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch)。 | |
| `optimizer_use_condition_selectivity` | 選取最佳化處理程式使用的統計資料。 範圍`1`到`5`。 請參閱[MariaDB檔案](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity)。 | 10.4及更新版本為4 |

### 設定多個資料庫使用者

您可以選擇設定多個具有不同許可權的使用者來存取`main`資料庫。

依預設，有一個名為`mysql`的端點擁有資料庫的管理員存取權。 若要設定多個資料庫使用者，您必須在`services.yaml`檔案中定義多個端點，並在`.magento.app.yaml`檔案中宣告關係。 對於Pro測試和生產環境，[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)以請求其他使用者。

使用巢狀陣列來定義特定使用者存取的端點。 每個端點可以指定一個或多個結構描述（資料庫）的存取權，以及每個結構描述的不同許可權層級。

有效的許可權層級為：

- `ro`：僅允許SELECT查詢。
- `rw`：允許SELECT查詢和INSERT、UPDATE和DELETE查詢。
- `admin`：允許所有查詢，包括DDL查詢（CREATE TABLE、DROP TABLE等）。

例如：

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

在上述範例中，`admin`端點提供對`main`資料庫的管理員層級存取權，`reporter`端點提供唯讀存取權，`importer`端點提供讀寫存取權，這表示：

- `admin`使用者擁有資料庫的完整控制權。
- `reporter`使用者只有SELECT許可權。
- `importer`使用者具有SELECT、INSERT、UPDATE和DELETE許可權。

將上述範例中定義的端點新增至`.magento.app.yaml`檔案的`relationships`屬性。 例如：

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>如果您設定一個MySQL使用者，則無法針對預存程式與檢視使用[`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html)存取控制機制。

## 連線到資料庫

直接存取MariaDB資料庫需要您使用SSH登入遠端雲端環境，並連線至資料庫。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 從[$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships)變數中的`database`和`type`屬性擷取MySQL登入認證。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   在回應中，尋找MySQL資訊。 例如：

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. 連線到資料庫。

   - 首先，使用以下命令：

     ```bash
     mysql -h database.internal -u <username>
     ```

   - 對於Pro，請使用下列命令，其中包含從`$MAGENTO_CLOUD_RELATIONSHIPS`變數擷取的主機名稱、連線埠號碼、使用者名稱和密碼。

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>您可以使用`magento-cloud db:sql`命令連線到遠端資料庫並執行SQL命令。

## 連線到次要資料庫

>[!IMPORTANT]
>
>此功能僅適用於Pro Production和Staging叢集。

有時候，您必須連線到次要資料庫，以改善資料庫效能或解決資料庫鎖定問題。 如果需要此組態，請使用`"port" : 3304`建立連線。 請參閱&#x200B;_實作最佳實務_&#x200B;指南中的[設定MySQL從屬連線的最佳實務](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html)主題。

## 疑難排解

請參閱下列Adobe Commerce支援文章，以取得MySQL問題疑難排解的說明：

- [正在檢查緩慢的查詢和處理MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [在雲端上建立資料庫傾印](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [資料移轉工具疑難排解](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Adobe Commerce升級：壓縮至動態表格2.2.x、2.3.x至2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
