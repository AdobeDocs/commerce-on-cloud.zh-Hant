---
title: 備份資料庫
description: 瞭解如何使用ECE-tools為雲端基礎結構專案上的Adobe Commerce建立資料庫備份。
feature: Cloud, Iaas, Storage
exl-id: 351f7691-3153-4b8a-83af-8b8895b93d98
source-git-commit: 3a3b0cd6e28f3e6ed3521a86f7c7c8868be0cf83
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# 備份資料庫

您可以使用`ece-tools db-dump`命令建立資料庫的復本，而不需從服務和掛載擷取所有環境資料。 依預設，這個命令會在環境組態中所指定的所有資料庫連線的`app/var/`目錄中建立備份。 DB傾印作業會將應用程式切換到維護模式、停止使用者佇列處理序，並在傾印開始之前停用cron作業。

請考慮下列資料庫傾印的准則：

- 對於生產環境，Adobe建議在非尖峰時間完成資料庫傾印操作，以便在網站處於維護模式時儘量減少服務中斷。
- 如果在傾印作業期間發生錯誤，該命令會刪除傾印檔案以節省磁碟空間。 檢閱記錄檔以取得詳細資料(`var/log/cloud.log`)。
- 對於Pro Production環境，這個命令只會從三個高可用性節點中的&#x200B;_一個_&#x200B;傾印，因此在傾印期間寫入不同節點的生產資料可能不會被複製。 命令會產生`var/dbdump.lock`檔案，以防止命令在多個節點上執行。
- 若要備份所有環境服務，Adobe建議建立[備份](snapshots.md)。

您可以選擇將資料庫名稱附加至命令，以備份多個資料庫。 下列範例備份兩個資料庫： `main`和`sales`：

```bash
php vendor/bin/ece-tools db-dump main sales
```

使用`php vendor/bin/ece-tools db-dump --help`命令取得更多選項：

- `--dump-directory=<dir>` — 選擇資料庫傾印的目標目錄。 **請勿選擇`pub/media`或`pub/static`**&#x200B;之類的公用網頁目錄。
- `--remove-definers` — 從資料庫傾印移除DEFINER陳述式。

**若要在暫存或生產環境中建立資料庫傾印**：

1. [使用SSH登入或建立通道以連線到包含要複製的資料庫的遠端環境](../development/secure-connections.md)。

1. 列出環境關係，並記下資料庫登入資訊。

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   或

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. 建立資料庫的備份。 若要選擇資料庫傾印的目標目錄，請使用`--dump-directory`選項。

   >[!WARNING]
   >
   >如果您指定目標目錄，請勿選擇`pub/media`或`pub/static`之類的公用網頁目錄。

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   範例回應：

   ```
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /app/qxmtlseakof6y/var/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. `db-dump`命令會在遠端專案目錄中建立`dump-<timestamp>.sql.gz`封存檔案。

>[!TIP]
>
>若要將此資料推播至特定環境，請參閱[移轉資料和靜態檔案](../deploy/staging-production.md#migrate-static-files)。
