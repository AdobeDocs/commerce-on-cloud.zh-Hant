---
title: 專案結構
description: 瞭解雲端基礎結構上Adobe Commerce的檔案結構和專案範本。
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# 專案結構

雲端基礎結構專案上的Adobe Commerce包含認證和應用程式設定的基本檔案。 根據Adobe Commerce版本，這些檔案可在中以範本的形式提供。 在[`magento/magento-cloud` GitHub存放庫](https://github.com/magento/magento-cloud)中檢視以Adobe Commerce版本為基礎的雲端範本。

下表說明雲端專案中包含的檔案：

| 檔案 | 說明 |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | 將`www`重新導向至Apex網域和`php`應用程式以提供HTTP的組態檔。 請參閱[設定路由](../routes/routes-yaml.md)。 |
| `/.magento/services.yaml` | 定義MySQL執行個體(MariaDB)、Redis和OpenSearch或Elasticsearch的組態檔。 請參閱[設定服務](../services/services-yaml.md)。 |
| `/app` | `code`資料夾用於自訂模組。 `design`資料夾用於[自訂主題](../store/custom-theme.md)。 `etc`資料夾包含應用程式的組態檔。 |
| `/m2-hotfixes` | 用於自訂修補程式。 |
| `/update` | 支援模組使用的服務資料夾。 |
| `.gitignore` | 指定要忽略的檔案和目錄。 請參閱[`.gitignore`參考](#ignoring-files)。 |
| `.magento.app.yaml` | 定義建置應用程式之屬性的組態檔。 請參閱[設定應用程式](../application/configure-app-yaml.md)。 |
| `.magento.env.yaml` | 建置、部署和部署後階段的設定檔。 `ece-tools`封裝包含此檔案的範例。 請參閱[設定環境](../environment/configure-env-yaml.md)。 |
| `composer.json` | 擷取Adobe Commerce和設定指令碼，以準備您的應用程式。 請參閱[必要的封裝](../development/overview.md#required-packages)。 |
| `composer.lock` | 儲存每個套件的版本相依性。 請參閱[必要的封裝](../development/overview.md#required-packages)。 |
| `magento-vars.php` | 用來定義[多個商店](../store/multiple-sites.md)和使用變數的網站。 |

{style="table-layout:auto"}

>[!NOTE]
>
>當您將本機變更推送到遠端伺服器時，部署指令碼會使用`.magento`目錄中組態檔定義的值，然後指令碼會刪除目錄及其內容。 您的本機開發環境不受影響。

## 應用程式根目錄

應用程式根目錄的位置取決於環境。

- **入門和Pro整合**： `/app`
- **入門產品**： `/<project-ID>`
- **Pro暫存**： `/<project-ID>_stg`
- **Pro Production**： `/<project-ID>`

### 可寫入的目錄

遠端整合、測試和生產環境為唯讀。 基於安全理由，下列目錄是&#x200B;*僅*&#x200B;可寫入的目錄：

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>在生產和中繼環境中，三節點叢集中的每個節點都有一個`/tmp`目錄，不與其他節點共用。

## 忽略檔案

雲端基礎結構專案存放庫中有具有Adobe Commerce的基底`.gitignore`檔案。 檢視magento-cloud存放庫](https://github.com/magento/magento-cloud/blob/master/.gitignore)中的最新[.gitignore檔案。 若要新增位於`.gitignore`清單中的檔案，您可以在暫存認可時使用`-f` （強制）選項：

```bash
git add <path/filename> -f
```

## 變更基礎範本

您可使用下列步驟來變更現有專案的結構，以反映雲端基礎結構上Adobe Commerce的最新基礎範本。

1. 將專案複製到本機工作站。

1. 使用下列`extra`區段的值更新`composer.json`檔案。

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. 新增為基底範本設計的`.gitignore`檔案。 例如，如果您需要2.2.6版範本的`.gitignore`檔案，請使用2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore)檔案的[.gitignore作為參考。

1. 清除Git快取。

   ```bash
   git rm -r --cached .
   ```

1. 新增並認可變更。

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
