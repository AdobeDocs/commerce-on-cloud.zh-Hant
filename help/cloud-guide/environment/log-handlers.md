---
title: 記錄處理常式
description: 瞭解如何在雲端基礎結構上為Adobe Commerce設定記錄處理常式。
feature: Cloud, Logs, Configuration
role: Developer
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# 記錄處理常式

您可以設定記錄處理常式，將訊息傳送至遠端記錄伺服器。 記錄處理常式會推送建立及部署記錄至其他系統，類似於將記錄推送至Slack及電子郵件的方式。 您可以啟用&#x200B;_syslog_&#x200B;處理常式，非常適合記錄與硬體相關的訊息，或啟用Graylog延伸記錄格式(GELF)處理常式，適合記錄來自軟體應用程式的訊息。

下列範例將設定新增至`.magento.env.yaml`檔案，以設定這兩個處理常式。 如需最低記錄層級(`min_level`)值，請參閱[記錄層級](#log-levels)。

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## 記錄層級

記錄層級決定通知訊息中的詳細資訊層級。 下列記錄層級類別包含其下方的每個記錄層級。 例如，`debug`層級包含每個層級的記錄，而`alert`層級只會顯示警示和緊急狀況。

- **偵錯** — 詳細的偵錯資訊
- **資訊** — 有趣的事件，例如使用者登入或SQL記錄
- **通知** — 正常但重要的事件
- **警告** — 非錯誤的特殊發生次數，例如，使用過時的API或不良使用API
- **error** — 不需要立即動作的執行階段錯誤
- **critical** — 嚴重狀況，例如無法使用的應用程式元件或意外的例外狀況
- **警示** — 需要立即執行的動作，例如網站已關閉或資料庫無法使用 — 會觸發簡訊警示
- **緊急** — 系統無法使用
