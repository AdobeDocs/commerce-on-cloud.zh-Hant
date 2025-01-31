---
title: 從元件失敗復原
description: 瞭解如果元件無法在雲端基礎結構上的Adobe Commerce中正確部署，如何復原。
feature: Cloud, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# 從元件失敗復原

本主題討論在元件無法正確部署時如何復原。 典型的範例包括元件具有遠端環境不滿足的相依性，例如不相容的PHP版本。

您可以透過下列任何方式，從失敗的部署中復原：

- [還原備份](../storage/snapshots.md#restore-a-snapshot)
- 清除先前變更的專案和程式碼，然後重新部署

## 清理、移除和重新部署

若要從先前的部署中清除，請識別已新增或更新之元件，然後將其移除。 首先，登入遠端環境，並手動清除`var`目錄的內容。 然後從`composer.json`檔案中移除元件，並重新部署環境。

**若要清除`var`目錄**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 清除`var`目錄。

   ```shell
   rm -rf var/*
   ```

1. 登出。

**若要移除元件**：

1. 在本機工作站上，變更至專案目錄。

1. 清除快取。

   ```bash
   composer clear-cache
   ```

1. 從`composer.json`檔案移除元件。

   ```bash
   composer remove <component-name>:<version>
   ```

   如果顯示下列訊息，您就不需要再進行任何動作：

   ```
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. 正在更新相依性，請稍候。

1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

在[還原環境](../development/restore-environment.md)中，檢視有關還原環境而不使用備份的詳細資訊。

{{stuck-deployment-tip}}
