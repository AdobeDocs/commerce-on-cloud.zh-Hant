---
title: Crons屬性
description: 請參閱如何在 [!DNL Commerce] 應用程式組態檔中設定'crons'屬性的範例。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1069'
ht-degree: 0%

---

# Crons屬性

Adobe Commerce使用`crons`屬性來排程重複活動。 它非常適合用於排程在一天中的特定時間執行的特定工作。 由於唯讀環境的性質，在雲端基礎結構專案的Adobe Commerce的Web執行個體上一次只能執行一個cron工作。 最佳實務是將長期執行的工作分解為較小的已排入佇列的工作。 或者，您也可以建置[背景工作執行個體](workers-property.md)。

Adobe建議您以[檔案系統擁有者](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html?lang=zh-Hant)身分執行`crons`。 請&#x200B;_不要_&#x200B;以`root`或網頁伺服器使用者的身分執行`crons`。

此設定不同於Adobe Commerce的內部部署，後者有多個預設的cron工作。 請參閱&#x200B;_設定指南_&#x200B;中的[設定cron工作](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html?lang=zh-Hant)。

## 設定cron工作

`crons`屬性說明排程觸發的處理序。 每個工作都需要名稱和下列選項：

- `spec` — 用於排程的cron運算式。
- `cmd` — 要在`start`和`stop`上執行的命令。
- `shutdown_timeout`—(_Optional_)如果cron工作已取消，這是傳送SIGKILL訊號以停止工作或處理序的秒數。 預設值為10秒。
- `timeout`—(_Optional_)逾時前cron工作可以執行的最長時間。 預設為允許的最大值86400秒（24小時）。

依預設，每個Commerce雲端專案在`.magento.app.yaml`檔案中具有下列預設`crons`設定：

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

如果您的專案需要自訂cron工作，您可以將其新增到預設`crons`設定。 請參閱[建置cron工作](#build-a-cron-job)。

### `crontab`

Adobe Commerce僅將auto-crons設定選項新增至Pro專案，以支援中繼和生產環境中的自助`crons`設定。 如果已啟用此選項，您可以使用`crontab`來檢閱cron組態。 這是&#x200B;_無法_&#x200B;用於入門專案。

雖然您可以使用`crontab`檢閱Pro專案的組態，但Adobe Commerce不會使用`crontab`針對部署在雲端基礎結構上的網站執行cron工作。

**若要檢閱Pro環境上的cron設定**：

1. 使用[SSH](../development/secure-connections.md#use-an-ssh-command)登入遠端環境。

1. 列出已排程的cron程式。

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >如果`crontab -l`命令傳回`Command not found`錯誤（僅適用於Pro測試和生產環境），您必須[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hant#submit-ticket)以在您的專案上啟用auto-crons自助設定選項。

下列範例顯示僅具有預設`crons`組態之環境的`crontab`輸出：

```
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## 建立cron工作

cron作業包括排程和計時規格以及在排程時間執行的命令。 對於入門環境和Pro `integration`環境，最小間隔為每5分鐘一次。 對於Pro測試和生產環境，最小間隔為每分鐘一次。 在雲端基礎結構上的Adobe Commerce上，您將自訂cron工作新增到`crons`區段中的`.magento.app.yaml`檔案。 一般格式為排程的`spec`，以及指定要執行的命令或自訂指令碼`cmd`。

### 規格

Adobe Commerce對`crons`規格（規格）使用五值運算式： `* * * * *`

1. 分鐘（0到59）對於所有入門和Pro環境，cron工作支援的最低頻率是五分鐘。 您可能需要在「管理員」中進行設定。
2. 小時（0到23）
3. 日期（1到31）
4. 月（1到12）
5. 星期幾（0到6） （星期日到星期六；某些系統上也有7是星期日）

部分範例：

- `00 */3 * * *`會在第一分鐘（上午12:00、凌晨3:00、早上6:00）每三小時執行一次
- `20 */8 * * *`在分鐘20每8小時執行一次（上午12:20、上午8:20、下午4:20）
- `00 00 * * *`每天午夜執行一次
- `00 * * * 1`在星期一的午夜每週執行一次。

>[!NOTE]
>
>`.magento.app.yaml`檔案中指定的`crons`時間是根據伺服器時區，而不是資料庫中存放區組態值中指定的時區。

決定排程時，請考慮完成工作所需的時間。 例如，如果您每三小時執行一次工作，而工作需要40分鐘來完成，則可以考慮變更排程時間。

### 命令

`cmd`指定要執行的命令或自訂指令碼。 指令碼格式可包含下列內容：

```text
<path-to-php-binary> <project-dir>/<script-command>
```

例如：

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

在此範例中，`<path-to-php-binary>`是`/usr/bin/php`。 包含專案識別碼的安裝目錄為`/app/abc123edf890/bin/magento`，而指令碼動作為`export:start catalog_category_product`。

### 新增自訂cron工作至您的專案

在雲端基礎結構平台上的Adobe Commerce上，您可以將自訂新增到[`.magento.app.yaml`](../application/configure-app-yaml.md)檔案的`crons`區段。

>[!NOTE]
>
>對於入門環境和Pro `integration`環境，最小間隔為每5分鐘一次。 對於Pro測試和生產環境，最小間隔為每分鐘一次。 您無法設定比預設最小值更頻繁的間隔。

在Adobe Commerce Pro專案上，您必須先在您的專案上啟用[自動程式碼功能](#set-up-cron-jobs)，才能使用`.magento.app.yaml`檔案將自訂cron工作新增到中繼和生產環境。 如果未啟用此功能，請[提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html?lang=zh-Hant#submit-ticket)以啟用自動確認。

**若要新增自訂cron工作**：

1. 在本機開發環境中，編輯Adobe Commerce `/app`目錄中的`.magento.app.yaml`檔案。

1. 在`crons`區段中，使用下列格式新增您的自訂專案：

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   在下列範例中，`productcatalog`工作每八小時匯出一次產品目錄，即該小時後的20分鐘。

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. 新增、提交和推送程式碼變更。

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### 更新cron工作

若要新增、移除或更新自訂工作，請在`.magento.app.yaml`檔案的`crons`區段中變更設定。 然後，在將變更推播到中繼和生產環境之前，在遠端`integration`環境中測試更新。

## 停用cron工作

您可以在完成維護任務（例如重新索引或清除快取）之前手動停用cron工作，以避免效能問題。 您可以使用`ece-tools` CLI命令`cron:disable`來停用所有cron工作並停止任何作用中的cron處理序。

**若要停用cron工作**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 停用cron工作並停止作用中的cron程式。

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. 完成任何必要的維護工作後，請務必再次啟用cron工作。

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## cron工作疑難排解

Adobe已更新雲端基礎結構上的Adobe Commerce套件，以最佳化雲端基礎結構上的Adobe Commerce上的cron處理並修正cron相關問題。 如果您遇到cron處理問題，請確定您的專案使用的是最新版本的`ece-tools`封裝。 請參閱[更新ECE-Tools](../dev-tools/update-package.md)。

您可以在每個環境的應用程式層級記錄檔中檢閱cron處理資訊。 檢視[應用程式記錄](../test/log-locations.md#application-logs)。

請參閱下列Adobe Commerce支援文章，以取得疑難排解cron相關問題的說明：

- [Cron任務會鎖定來自其他群組的任務](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html?lang=zh-Hant)

- [在雲端上手動重設停滯的cron工作](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html?lang=zh-Hant)
