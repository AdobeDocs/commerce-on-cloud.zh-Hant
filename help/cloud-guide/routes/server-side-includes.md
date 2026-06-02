---
title: 伺服器端包含
description: 瞭解如何在雲端基礎結構上將伺服器端包含與Adobe Commerce搭配使用。
feature: Cloud, Routes
exl-id: 826a9c9a-d082-4ec4-8fd2-00ca357522ab
TQID: https://experienceleague.adobe.com/iLal9p5QiG4U0sHrskzFV8buCCVWZAa3FLixND0Nw24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: fd3ef8201c368f889344452e334976070a6c7157
workflow-type: tm+mt
source-wordcount: 182
ht-degree: 0%

---

# 伺服器端包含

[伺服器端包含](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI)是HTML頁面中的指示，會在頁面轉譯時在伺服器上評估。 SSI可讓您將動態產生的內容新增至現有的HTML頁面，而不需要提供整個頁面。

您可以在您的`.magento/routes.yaml`中以每個路由為基礎啟用或停用SSI；例如：

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI可讓您在HTML回應指令中加入，讓伺服器填入HTML的部分，並遵守任何現有的[快取設定](caching.md)。

下列範例說明如何在頁面頂端插入動態日期控制項，並在底部插入另一個日期控制項，每600秒更新一次：

新增下列內容至任何頁面，例如`/index.php`：

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

將下列專案新增至`time.php`：

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

瀏覽至新增控制項的頁面。 重新整理頁面幾次，並注意頁面頂端的時間會變更，而底部的時間則只會每600秒變更一次。
