---
title: 範例資料
description: 瞭解如何在雲端基礎結構上使用Adobe Commerce安裝範例資料。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# 範例資料

如果您在開發存放區時需要一些範例資料，可以安裝範例資料。 此資料會模擬使用中的Adobe Commerce商店，其中包含客戶、產品和其他資料。 此範例資料最適合用於安裝雲端基礎結構範本的新Adobe Commerce。

最佳做法是在開發和整合環境中安裝範例資料。 如果您在測試或生產中使用範例資料，則必須在上線之前[移除](#reset-or-uninstall-sample-data)資訊和產品。

## 安裝範例資料

若要安裝範例資料：

1. 在本機工作站上，變更至專案目錄。

1. 在根目錄中，輸入下列命令以新增範例資料：

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. 等待元件更新。

1. 提交並推送變更：

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 等待專案部署。

1. 前往整合環境中的店面頁面，確認安裝成功。 您可以透過[!DNL Cloud Console]找到商店連結的URL。

1. 拍攝環境的快照：

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

您可以開始使用即時資料測試您的開發！

## 重設或解除安裝範例資料

您可以依照安裝範例資料的相同程式來重設或移除範例資料：

- 刪除範例資料： `./bin/magento sampledata:remove`
- 重設範例資料： `./bin/magento sampledata:reset`
