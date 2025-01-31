---
title: 健康狀態通知
description: 瞭解如何在雲端基礎結構專案的Adobe Commerce上設定磁碟空間使用情形的Slack、電子郵件和尋呼通知。
feature: Cloud, Observability, Integration
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# 健康狀態通知

雲端基礎結構上的Adobe Commerce會監視您入門環境或Pro整合環境中所有應用程式和服務上的磁碟空間使用情況。 空間不足的資料庫磁碟可能會導致資料損毀。 健康狀態檢查每5分鐘進行一次，可以透過電子郵件或其他外部服務通知您。 健全狀態通知有三個低磁碟警告：

- **警告** — 可用磁碟空間降低到20%以下
- **關鍵** — 可用磁碟空間下降至10%以下
- **完全清除** — 低磁碟事件後，可用磁碟空間傳回超過20%

>[!NOTE]
>
>在Pro Production環境中，您可以使用New Relic的Managed Alerts for Adobe Commerce警報原則來監視磁碟空間。 檢視[監控受管理警示的效能](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts)。

## 電子郵件通知

健全狀況電子郵件整合需要來源地址和至少一個收件者地址。 您可以對`from-address`和`recipients`位址使用相同的電子郵件地址。 以下範例向兩個收件者註冊健康情況電子郵件整合：

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Slack頻道通知

Slack是一種外部服務，使用稱為機器人的互動式應用程式在聊天室中張貼訊息。 您必須先為您的Slack群組建立自訂[機器人使用者](https://api.slack.com/bot-users)，才能以Slack接收健康情況通知。 為您的管道或管道設定機器人使用者後，請儲存Slack提供的[機器人Token](https://api.slack.com/docs/token-types#bot)以註冊您的整合。 以下範例會在Slack頻道中註冊健康情況通知：

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## PagerDuty通知

PagerDuty是一項外部服務，可將重要問題通知隨叫隨到的團隊成員。 您必須先建立使用Events API版本2的[PagerDuty整合](https://developer.pagerduty.com/v2/docs/integrating)，才能在PagerDuty中接收健康狀態通知。 使用整合金鑰或&#x200B;_路由金鑰_&#x200B;來註冊您的整合。 下列範例使用路由索引鍵註冊PagerDuty的通知：

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
