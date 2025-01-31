---
title: 整合總覽
description: 瞭解雲端基礎結構專案上您Adobe Commerce的第三方整合選項。
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# 整合總覽

整合對於使用外部服務(例如Git託管或Slack機器人)和維護您目前的開發流程（例如使用GitHub中的程式碼檢閱提取請求函式）非常有用。 您可以在雲端基礎結構專案上新增下列整合至Adobe Commerce：

![整合](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**若要使用Cloud CLI新增整合**：

下列指令會啟動互動式提示，以選取新整合的型別和選項。

```bash
magento-cloud integration:add
```

**若要列出專案設定的整合**：

```bash
magento-cloud integration:list
```

範例回應：

```
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB 主控台]

**若要使用[!DNL Cloud Console]**&#x200B;新增整合：

1. 在&#x200B;_專案設定_&#x200B;中，按一下&#x200B;**[!UICONTROL Integrations]**。

1. 按一下整合型別或按一下&#x200B;**[!UICONTROL Add integration]**。

1. 逐步完成整合型別選取和設定步驟。

1. 新增整合後，其會出現在整合檢視的清單中。

>[!ENDTABS]

## Commerce webhook

您可以使用[ENABLE_WEBHOOKS全域變數](../environment/variables-global.md#enable_webhooks)在雲端專案中設定Commerce Webhooks。 Commerce webhook會傳送請求至外部伺服器，以回應Commerce產生的事件。 [_Webhooks指南_](https://developer.adobe.com/commerce/extensibility/webhooks)詳細說明此功能。

## 通用Webhook

您可以使用自訂Webhook整合，將`POST` JSON訊息擷取並報告至&#x200B;_Webhook_ URL的Cloud基礎結構和存放庫事件。

**若要新增webhook URL，請使用下列語法**：

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type` — 指定`webhook`整合型別。
- `url` — 提供可接收JSON訊息的webhook URL。

範例回應會顯示一系列提示，提供自訂整合的機會。 使用預設（空白）回應會傳送有關專案中所有環境之所有事件的訊息。

您可以自訂整合，以報告特定的[事件](#events-to-report)，例如推送程式碼至分支。 例如，您可以指定`environment.push`事件，以便在使用者將程式碼推播至分支時傳送訊息：

```
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

您可以選擇報告`pending`、`in_progress`或`complete`狀態的事件：

```
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

您可以&#x200B;_包含_&#x200B;或&#x200B;_排除_&#x200B;特定環境的訊息：

```
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

整合完成後，您會收到值的摘要：

```
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 更新現有的整合

您可以更新現有的整合。 例如，使用以下專案將狀態從`complete`變更為`pending`：

```bash
magento-cloud integration:update --states=pending <int-id>
```

範例回應：

```
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### 要報告的事件

| 事件 | 說明 |
| ----- | :-----------|
| `environment.access.add` | 已授予使用者存取環境的許可權 |
| `environment.access.remove` | 已從環境中移除使用者 |
| `environment.activate` | 已經使用環境「啟動」分支 |
| `environment.backup` | 使用者觸發了快照 |
| `environment.branch` | 已使用管理主控台建立分支 |
| `environment.deactivate` | 已「停用」分支。 程式碼仍存在，但環境已損毀 |
| `environment.delete` | 已刪除分支 |
| `environment.initialize` | 使用第一個認可初始化的專案的`master`分支 |
| `environment.merge` | 使用中的分支已使用管理控制檯或API合併 |
| `environment.push` | 使用者將程式碼推送至分支 |
| `environment.restore` | 使用者已還原快照 |
| `environment.route.create` | 已使用管理主控台建立路由 |
| `environment.route.delete` | 已經使用管理主控台刪除路由 |
| `environment.route.update` | 已使用管理主控台修改路由 |
| `environment.subscription.update` | `master`環境已重新調整大小，因為訂閱已變更，但此處沒有內容變更 |
| `environment.synchronize` | 環境已從其父環境中重新複製資料或程式碼 |
| `environment.update.http_access` | 已修改環境的HTTP存取規則 |
| `environment.update.restrict_robots` | 已啟用或停用block-all-robots功能 |
| `environment.update.smtp` | 已針對一個環境啟用或停用電子郵件傳送 |
| `environment.variable.create` | 已建立變數 |
| `environment.variable.delete` | 已刪除變數 |
| `environment.variable.update` | 變數已修改 |
| `project.domain.create` | 網域已建立並新增至專案 |
| `project.domain.delete` | 與專案關聯的網域已移除 |
| `project.domain.update` | 已更新與專案關聯的網域 |
