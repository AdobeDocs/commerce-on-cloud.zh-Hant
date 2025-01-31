---
title: 工作者
description: 瞭解如何在 [!DNL Commerce] 應用程式設定檔案中設定Worker屬性。
feature: Cloud, Configuration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Workers屬性

您可以定義工作程式獨立於Web執行個體執行，而不需要執行中的Nginx執行個體；但是，工作程式確實使用[!DNL Commerce]應用程式使用的相同網路儲存空間。 您不需要在背景工作執行個體上設定Web伺服器（使用Node.js或Go），因為路由器無法將公開要求導向背景工作。 這使得工作者執行個體非常適合用於背景工作或持續執行工作，這些工作可能會阻礙部署。

## 設定背景工作

背景工作僅適用於Pro預備和生產環境。 Pro整合與入門環境可以選擇使用[CRON_CONSUMERS_RUNNER](../environment/variables-deploy.md#cron_consumers_runner)變數。

若要在Pro測試或生產環境中設定背景工作，[請提交Adobe Commerce支援票證](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket)並包含下列資訊：

- 專案ID
- 環境ID
- 工作者名稱
- 啟動命令

您可以為每個工作者設定一個處理序。 `.magento.app.yaml`檔案中的基本通用背景工作設定可能如下所示：

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

此範例定義名為`queue`的單一背景工作，其資源配置的小（大小S）層級，並在啟動時執行`php ./bin/magento`命令。 然後，背景工作`queue`會以背景工作處理序的形式在每個節點上執行。 如果命令結束，就會自動重新啟動。

## 命令和覆寫

使用Worker應用程式啟動命令需要`commands.start`鍵。 您可以使用任何有效的shell命令，不過最好使用應用程式的語言。 如果`start`機碼指定的命令終止，它會自動重新啟動。

>[!IMPORTANT]
>
>`deploy`和`post_deploy`掛接和`crons`命令只在Web容器上執行，不在背景工作執行個體中執行。

### 繼承

背景工作會繼承`size`、`relationships`、`access`、`disk`和`mount`以及`variables`屬性的定義，除非明確覆寫。

下列屬性最常用來覆寫[最上層設定](properties.md)：

- `size` — 將較少的資源配置給單一背景處理序
- `variables` — 指示應用程式以不同方式執行

### 時間與佇列

雖然每個工作者佇列在彼此後面，但以下設定會在`var/time.txt`檔案中產生一致的兩秒時間戳記分隔，不論PHP程式碼內是否8秒休眠：

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
