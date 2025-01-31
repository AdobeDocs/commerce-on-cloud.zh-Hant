---
title: 伺服器端包含
description: 瞭解如何在雲端基礎結構上將伺服器端包含與Adobe Commerce搭配使用。
feature: Cloud, Routes
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# 伺服器端包含

[伺服器端包含](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI)是HTML頁面中的指令，在頁面轉譯時在伺服器上評估。 SSI可讓您將動態產生的內容新增至現有的HTML頁面，而不需要提供整個頁面。

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

SSI可讓您在HTML回應指示中納入導致伺服器填入HTML部分的指示，並遵循任何現有的[快取組態](caching.md)。

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
