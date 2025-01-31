---
title: 勾點屬性
description: 請參閱如何在 [!DNL Commerce] 應用程式組態檔中設定hooks屬性的範例。
feature: Cloud, Configuration, Build, Deploy
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# 勾點屬性

使用`hooks`區段在建置、部署和部署後階段執行殼層命令：

- **`build`** — 在&#x200B;_封裝您的應用程式之前_&#x200B;執行命令。 資料庫或Redis等服務無法使用，因為應用程式尚未部署。 在&#x200B;_前新增自訂命令_&#x200B;預設的`php ./vendor/bin/ece-tools`命令，讓自訂產生的內容繼續進入部署階段。

- **`deploy`** — 在&#x200B;_封裝並部署應用程式之後執行命令_。 此時您可以存取其他服務。 由於預設`php ./vendor/bin/ece-tools`命令會將`app/etc`目錄複製到正確的位置，因此您必須在&#x200B;_部署命令之後新增自訂命令，以防止自訂命令失敗。_

- **`post_deploy`** — 在部署應用程式&#x200B;_之後執行命令_，在&#x200B;_容器開始接受連線之後執行命令_。 `post_deploy`掛接會清除快取並預先載入（加溫）快取。 您可以使用[部署後階段](../environment/variables-post-deploy.md)中的`WARM_UP_PAGES`變數來自訂頁面清單。 雖然不需要，但此變數可與`SCD_ON_DEMAND`環境變數搭配使用。

下列範例顯示`.magento.app.yaml`檔案中的預設組態。 在`ece-tools`命令之前&#x200B;_的`build`、`deploy`或`post_deploy`區段_&#x200B;下新增CLI命令：

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

此外，您也可以使用`generate`和`transfer`命令，在特別建置程式碼或移動檔案時執行其他動作，以進一步自訂建置階段。

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` — 導致第一個失敗的命令上的掛接失敗，而不是最後一個失敗的命令。
- `build:generate` — 如果針對建置階段啟用SCD，則套用修補程式、驗證組態、產生DI及產生靜態內容。
- `build:transfer` — 將產生的程式碼和靜態內容傳輸到最終目的地。

命令從應用程式(`/app`)目錄執行。 您可以使用`cd`命令來變更目錄。 如果掛接中的最終命令失敗，掛接就會失敗。 若要使其在第一個失敗的命令上失敗，請將`set -e`新增至掛接的開頭。

**若要使用grunt**&#x200B;編譯Sass檔案：

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

在建置期間進行靜態內容部署之前，使用`grunt`編譯Sass檔案。 將`grunt`命令放在`build`命令之前。

{{scenarios}}
