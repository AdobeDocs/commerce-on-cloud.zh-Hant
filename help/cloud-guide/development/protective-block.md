---
title: 保護區塊
description: 瞭解Adobe Commerce在雲端基礎結構上的保護封鎖功能，以及此功能如何保護您的網站免受已知安全漏洞的攻擊。
feature: Cloud, Configuration, Security
topic: Security
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# 保護區塊

雲端基礎結構上的Adobe Commerce具有保護性封鎖功能，可在特定情況下限制對具有安全漏洞的網站的存取。 此部分封鎖方法可防止利用已知的安全性弱點。 過時的軟體通常包含利用漏洞，因此透過部分封鎖對這些網站的存取來防止這些利用漏洞是很重要的。

## 保護區塊如何運作

Adobe Commerce會維護開放原始碼軟體（通常部署在雲端基礎結構上）中已知安全性弱點的簽名資料庫。 安全性檢查只會分析開放原始碼專案中的已知漏洞；無法檢查您編寫的自訂。

安全性掃描執行：

- 將新程式碼推送到Git時
- 將新弱點新增到資料庫時

如果您的應用程式偵測到嚴重漏洞，則會拒絕Git推送。

執行的區塊型別有兩種：

1. **完成區塊** — 用於開發網站。 `git push`隨附的錯誤訊息提供有關漏洞的詳細資訊。

1. **部分割槽塊** — 用於生產網站，可讓網站維持大部分連線。 根據漏洞的性質，請求的各個部分（例如查詢字串、Cookie或任何其他標頭）可能會從GET請求中移除。 所有其他請求可能會完全封鎖，例如登入、表單提交或產品結帳。

解除封鎖會在解決安全性風險時自動進行。 在您套用安全性升級以移除漏洞後，區塊就會立即移除。

## 選擇退出保護區塊

保護區塊是用來保護您，使其免受在雲端基礎結構上部署Adobe Commerce之軟體中的已知漏洞之害。

不過，您可以將下列專案新增至[`.magento.app.yaml`](../application/configure-app-yaml.md)以選擇退出保護區塊：

```yaml
   preflight:
      enabled: false
```

例如，您可以明確選擇退出特定檢查：

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
