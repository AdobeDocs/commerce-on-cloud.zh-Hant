---
title: 自訂主題
description: 瞭解如何在雲端基礎結構上安裝具有Adobe Commerce的自訂主題。
feature: Cloud, Themes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# 自訂主題

您可以安裝一或多個主題，用於專案中一或所有商店和網站。 主題包含多個靜態檔案(包括影像、字型、CSS、JavaScript、PHP等)，以便完整設計您的存放區。 您可以將主題解壓縮程式碼至檔案系統或使用Composer來新增主題。

## 手動安裝主題

若要手動安裝佈景主題，佈景主題的程式碼必須在壓縮封存檔中或類似下列的目錄結構中：

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**若要手動安裝佈景主題**：

1. 複製店面主題的`<Project root dir>/app/design/frontend`底下的主題程式碼或管理主題的`<Project root dir>/app/design/adminhtml`。 請確認最上層目錄為`<VendorName>`；否則，主題未正確安裝。

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. 確認複製到正確位置的主題。

   * 店面主題： `ls <project-root>/app/design/frontend`
   * 管理主題： `ls <project-root>/app/design/adminhtml`

   範例如下：

   範例佈景主題Adobe Commerce

1. 新增及提交檔案。

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. 將檔案推送至您的分支。

   ```bash
   git push origin <branch name>
   ```

1. 等待部署完成。
1. 登入管理員。
1. 按一下&#x200B;**內容** >設計> **佈景主題**。

   主題會顯示在右窗格中。

## 使用撰寫器安裝主題

使用Composer安裝主題與使用Composer安裝任何其他擴充功能相同。 如需詳細資訊，請參閱[安裝、管理和升級模組](extensions.md)。

若要使用Composer安裝主題：

1. 從Commerce Marketplace購買主題。
1. 取得主題的撰寫器名稱。
1. 變更至Adobe Commerce根目錄並輸入命令：

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   例如，

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. 等待相依性更新。
1. 輸入下列命令：

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. 登入管理員。
1. 按一下&#x200B;**內容** >設計> **佈景主題**。

   主題會顯示在右窗格中。

## 多個主題

使用多個主題時（例如每個地區設定使用不同的主題），請檢閱`SCD_MATRIX`環境變數以自訂主題部署。 檢視[環境設定](../environment/configure-env-yaml.md)中的[組建](../environment/variables-build.md#scd_matrix)或[部署](../environment/variables-deploy.md#scd_matrix)階段。
