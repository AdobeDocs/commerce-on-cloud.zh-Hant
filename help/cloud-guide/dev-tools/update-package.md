---
title: 更新ECE工具套件
description: 瞭解如何升級ECE-Tools套件，以運用雲端基礎結構上套用至Adobe Commerce的最新修正和功能。
feature: Cloud, Upgrade
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# 更新ECE工具套件

`ece-tools`套件的更新也會更新Commerce套件的其他[雲端工具套裝](../release-notes/cloud-tools-suite.md)，它們是`ece-tools`的相依性。 因此，您必須在支援`ece-tools`套件的雲端基礎結構上使用版本的Adobe Commerce。

{{ece-tools-package}}

**必要條件**：

- 更新`ece-tools`之前，請先檢閱Commerce的[Cloud Tools Suite發行說明](../release-notes/cloud-tools-suite.md)。
- 如果您是從`ece-tools` 2002.0.22或更舊版本更新至2002.1.0，請檢閱[回溯不相容的變更](../release-notes/backward-incompatible-changes.md)，並對雲端基礎結構專案上的Adobe Commerce進行任何必要的變更。
- 檢閱[升級和修補程式](../development/commerce-version.md#upgrade-from-older-versions)，以決定與雲端基礎結構專案上的Adobe Commerce相容的ECE-Tools版本。

{{upgrade-tip}}

**若要更新`ece-tools`封裝**：

1. 在您的本機工作站上，使用Composer執行更新。

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >如果您無法更新超過`ece-tools` 2002.0.8版，請參閱[升級專案以使用ECE-Tools套件](install-package.md)。

1. 新增、提交和推送程式碼變更。

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. 測試驗證後，將此分支合併至整合分支。
