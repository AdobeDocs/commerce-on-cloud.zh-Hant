---
title: 還原環境
description: 瞭解如何從雲端基礎結構專案解除安裝Adobe Commerce應用程式，並將環境還原至穩定狀態。
role: Developer
topic: Development
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 0%

---

# 還原環境

如果您在整合環境中遇到問題，而且沒有[有效的備份](../storage/snapshots.md)，或是想要將環境重設為空白狀態，您可以使用下列其中一種方法來還原/重設環境：

- 重設或還原Git分支中的程式碼
- 解除安裝[!DNL Commerce]應用程式
- 強制重新部署
- 手動重設資料庫

{{stuck-deployment-tip}}

## 重設Git分支

重設您的Git分支會將程式碼回復到過去的穩定狀態。

**若要重設您的分支**：

1. 在本機工作站上，變更至專案目錄。

1. 檢閱Git認可歷史記錄。 使用`--oneline`在一行中顯示縮寫的認可：

   ```bash
   git log --oneline
   ```

   範例回應：

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. 選擇代表程式碼最後已知穩定狀態的認可雜湊。

   若要將分支重設為原始初始化狀態，請尋找建立分支的第一個認可。 您可以使用`--reverse`以反向時間順序顯示歷史記錄。

1. 使用硬重設選項來重設您的分支。 使用此命令時請小心，因為它會捨棄自選取認可後所做的所有變更。

   ```bash
   git reset --hard <commit>
   ```

1. 推送您的變更以觸發重新部署，並重新安裝Adobe Commerce。

   ```bash
   git push --force <origin> <branch>
   ```

## 解除安裝Commerce

解除安裝[!DNL Commerce]應用程式會還原資料庫、移除部署組態並清除`var/`子目錄，讓您的環境回到原始狀態。 本指引也會將您的Git分支重設為先前的穩定狀態。 如果您最近沒有備份，但可以使用SSH存取遠端環境，請依照下列步驟還原您的環境：

- 停用設定管理
- 解除安裝Adobe Commerce
- 重設Git分支

解除安裝Adobe Commerce軟體會捨棄並還原資料庫、移除部署設定，以及清除`var/`子目錄。 請務必停用[組態管理](../store/store-settings.md)，這樣它就不會在下次部署期間自動套用先前的組態設定。 確定您的`app/etc/`目錄不包含`config.php`檔案。

**若要解除安裝Adobe Commerce軟體**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 移除組態檔。
   - 對於Adobe Commerce 2.2和更新版本：

     ```bash
     rm app/etc/config.php
     ```

   - 若為Adobe Commerce 2.1：

     ```bash
     rm app/etc/config.local.php
     ```

1. 解除安裝Adobe Commerce應用程式。

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. 確認Adobe Commerce已成功解除安裝。

   系統會顯示下列訊息，確認解除安裝成功：

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. 清除`var/`子目錄。

   ```bash
   rm -rf var/*
   ```

1. 登出。

>[!TIP]
>
>或者，這是清理組建快取的良好做法。
>
>```bash
>magento-cloud project:clear-build-cache
>```

## 強制重新部署

如果您嘗試解除安裝Adobe Commerce，但部署持續失敗，您可以嘗試手動強制重新部署。

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## 重設資料庫

如果您嘗試解除安裝Adobe Commerce，但命令失敗或無法完成，您可以手動重設資料庫。

**要重設資料庫**：

1. 在本機工作站上，變更至專案目錄。

1. 使用SSH登入遠端環境。

   ```bash
   magento-cloud ssh
   ```

1. 連線到資料庫。

   ```bash
   mysql -h database.internal
   ```

1. 卸除`main`資料庫。

   ```shell
   drop database main;
   ```

1. 建立空的`main`資料庫。

   ```shell
   create database main;
   ```

1. 刪除下列組態檔。

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. 登出並觸發重新部署。

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
