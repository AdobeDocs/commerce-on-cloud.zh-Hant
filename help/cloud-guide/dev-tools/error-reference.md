---
title: ECE-Tools套件的錯誤訊息
description: 檢視在Adobe Commerce期間雲端基礎結構建置、部署和部署後流程中可能發生的錯誤代碼和訊息清單。
recommendations: noDisplay
role: Developer
exl-id: e240c268-b171-44e9-9fa4-f0e91b0d9899
source-git-commit: 350cfea06f036f0787b330e6e40c5af46a30f5ad
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---

# ECE工具的錯誤訊息

此錯誤訊息參考提供在Adobe Commerce雲端基礎結構建置、部署和部署後程式期間可能發生的錯誤疑難排解資訊。

部署期間發生的所有嚴重和警告錯誤訊息都會寫入`var/log/cloud.log`和`/var/log/cloud.error.log`檔案。 雲端錯誤記錄檔僅包含來自最新部署的錯誤。 空白檔案表示部署成功且沒有錯誤。

在`cloud.error.log`檔案中，每個專案都會格式化為JSON字串，以便於剖析：

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

錯誤訊息會依部署階段之一進行分類：建置、部署和部署後。 每個區段都會提供相關錯誤的清單，以及每個錯誤的下列資訊：

- **錯誤碼**：錯誤訊息的Adobe Commerce指派識別碼
- **階段**：指出錯誤是否發生在建置、部署或部署後階段
- **步驟**：指出部署情境中可能傳回錯誤的步驟。 如果&#x200B;_Step_&#x200B;資料行空白，則錯誤為一般錯誤，可由多個步驟傳回，或在前置處理作業期間傳回。 請參閱[以案例為基礎的部署](../deploy/scenario-based.md)，以取得有關建置、部署和部署後步驟的詳細資訊。
- **建議**：提供疑難排解及解決錯誤的指南
- **標題（錯誤說明）**：摘要說明錯誤原因
- **型別**：指出錯誤是嚴重錯誤還是警告

{{$include /help/_includes/automated/ece-tools-error-codes.md}}
