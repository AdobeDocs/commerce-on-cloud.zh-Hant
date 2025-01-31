---
title: 設定通知
description: 瞭解如何在雲端基礎結構環境中設定Adobe Commerce的通知。
feature: Cloud, Configuration, Logs
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# 設定通知

根據預設，雲端基礎結構上的Adobe Commerce會將建置和部署動作寫入Adobe Commerce根應用程式目錄內的`app/var/log/cloud.log`檔案。 您可以選擇將日誌傳送至傳訊系統(例如Slack和電子郵件)以接收即時通知。

例如，您可以傳送Slack訊息，在部署失敗時提醒一組人員，並提示調查哪裡出了問題。

## 計畫通知

在設定通知之前，請考量下列事項：

- 您要接收哪些通知(Slack訊息、電子郵件等)？
- 您想在記錄中檢視多少詳細資訊？
- 您要在哪個位置設定通知（整合、測試、生產）？

例如，在初始開發期間，您可能會偏好使用顯示整合環境詳細記錄的電子郵件通知，以協助您在部署到中繼環境之前對問題進行偵錯。 當您準備好要部署到中繼或生產環境時，您可能偏好使用包含較不詳細資訊的Slack訊息。

>[!NOTE]
>
>用來設定通知的組態檔位於專案目錄的根目錄，因此當您推送變更至任何環境時，就會套用到該組態檔。 如果您想要根據環境自訂通知，則必須先修改設定檔案，才能將其推送至該環境。

## 設定通知

若要設定通知：

1. 在本機工作站上，變更至專案目錄。
1. 在專案根目錄中的`.magento.env.yaml`檔案中，新增您的傳訊系統設定，包括偏好的通知[記錄層級](log-handlers.md#log-levels)。

   例如，若要設定Slack _和_&#x200B;電子郵件設定，請使用下列專案：

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >雲端基礎結構上的Adobe Commerce只會在部署階段傳送電子郵件。

1. 認可變更並將變更推播至遠端伺服器。

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Slack設定範例

以下範例顯示僅限Slack的設定：

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` — 您的Slack[使用者權杖](https://api.slack.com/docs/token-types#user)。 您的使用者權杖會在雲端基礎結構上授權Adobe Commerce以傳送訊息。
- `channel` — 雲端基礎結構上的Adobe CommerceSlack頻道名稱會傳送通知。
- `username` — 雲端基礎結構上的Adobe Commerce使用者名稱用來以Slack傳送通知訊息。
- `min_level` — 通知訊息的最小記錄層級。 我們建議使用`info`。

### 電子郵件設定範例

以下範例顯示僅用於電子郵件的設定：

>[!NOTE]
>
>雲端基礎結構上的Adobe Commerce只會在部署階段傳送電子郵件。

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` — 雲端基礎結構上的Adobe Commerce電子郵件地址會傳送通知訊息。
- `from` — 傳送通知訊息給收件者的電子郵件地址。
- `subject` — 電子郵件的說明。
- `min_level` — 通知訊息的最小記錄層級。 我們建議使用`notice`或`warning`。
