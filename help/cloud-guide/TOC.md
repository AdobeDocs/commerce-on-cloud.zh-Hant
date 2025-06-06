---
user-guide-title: 雲端型 Commerce 的指南
user-guide-description: 了解如何在雲端基礎結構上管理 Adobe Commerce 應用程式。
product: magento
feature: Cloud
source-git-commit: 3347ad0a5fe202cbd80d08b7289c20a1c98ed1e3
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 8%

---


# 雲端基礎結構上的Commerce {#user-guide}

+ [Commerce](overview.md)
+ 架構 {#architecture}
   + [雲端基礎結構](architecture/cloud-architecture.md)
   + [安全性](architecture/security.md)
   + [技術棧疊](architecture/tech-stack.md)
   + [入門架構](architecture/starter-architecture.md)
   + [入門工作流程](architecture/starter-develop-deploy-workflow.md)
   + [Pro架構](architecture/pro-architecture.md)
   + [專業工作流程](architecture/pro-develop-deploy-workflow.md)
   + [擴充架構](architecture/scaled-architecture.md)
   + [自動縮放](architecture/autoscaling.md)
+ [開始使用](https://experienceleague.adobe.com/docs/commerce-on-cloud/start/overview.html?lang=zh-Hant)
+ 發行說明 {#release-notes}
   + [雲端工具套裝](release-notes/cloud-tools-suite.md)
   + [ECE-Tools套件](release-notes/ece-tools-package.md)
   + [雲端修補程式](release-notes/cloud-patches.md)
   + [Cloud Docker包](release-notes/cloud-docker.md)
   + [雲端元件](release-notes/cloud-components.md)
   + [雲端套件](release-notes/cloud-packages.md)
   + [與舊版不相容的變更](release-notes/backward-incompatible-changes.md)
   + [發行說明封存](release-notes/cloud-release-archive.md)
+ 雲端專案 {#project}
   + [專案概述](project/overview.md)
   + [專案結構](project/file-structure.md)
   + [使用者存取權](project/user-access.md)
   + [多重要素驗證](project/multi-factor-authentication.md)
   + [活動資料流](project/activity-stream.md)
   + [傳出電子郵件](project/outgoing-emails.md)
   + [SendGrid電子郵件服務](project/sendgrid.md)
   + [主控台分支管理](project/console-branches.md)
   + [地區IP位址](project/regional-ip-addresses.md)
+ 開發人員工具 {#dev-tools}
   + [概觀](dev-tools/overview.md)
   + 雲端CLI {#cloud-cli}
      + [CLI概述](dev-tools/cloud-cli-overview.md)
      + [CLI參考](dev-tools/cloud-cli-reference.md)
   + [Cloud Docker](dev-tools/cloud-docker.md)
   + ECE-Tools {#ece-tools}
      + [套件概述](dev-tools/package-overview.md)
      + [一次性升級以使用ECE-Tools](dev-tools/install-package.md)
      + [更新ECE工具套件](dev-tools/update-package.md)
      + [CLI參考](dev-tools/ece-tools-cli-reference.md)
      + [錯誤參考](dev-tools/error-reference.md)
   + 整合 {#integrations}
      + [概觀](integrations/overview.md)
      + [位元貯體](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [健康狀態通知](integrations/health-notifications.md)
+ 開發 {#develop}
   + [概觀](development/overview.md)
   + [驗證金鑰](development/authentication-keys.md)
   + [CLI分支管理](development/cli-branches.md)
   + [安全連線](development/secure-connections.md)
   + 部署 {#deploy}
      + [部署流程](deploy/process.md)
      + [最佳化](deploy/optimization.md)
      + [最佳實務](deploy/best-practices.md)
      + [以案例為基礎的部署](deploy/scenario-based.md)
      + [零停機部署](deploy/reduce-downtime.md)
      + [靜態內容部署](deploy/static-content.md)
      + [智慧型精靈](deploy/smart-wizards.md)
      + [部署至測試與生產](deploy/staging-production.md)
      + [從元件失敗復原](deploy/recover-failed-deployment.md)
   + 測試 {#test}
      + [測試指南](test/guidance.md)
      + [記錄檔](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [範例資料](test/sample-data.md)
      + [測試和生產](test/staging-and-production.md)
      + [第二個中繼環境](test/second-staging.md)
   + [PrivateLink服務](development/privatelink-service.md)
   + [保護區塊](development/protective-block.md)
   + [還原環境](development/restore-environment.md)
   + 儲存 {#storage}
      + [管理磁碟空間](storage/manage-disk-space.md)
      + [設定檔資料庫查詢](storage/profile-database-queries.md)
      + [備份資料庫](storage/database-dump.md)
      + [備份管理](storage/snapshots.md)
   + 升級與修補程式 {#upgrade}
      + [最佳實務](development/best-practices.md)
      + [升級Commerce版本](development/commerce-version.md)
      + [套用修補程式](development/apply-patches.md)
+ 設定 {#configure}
   + [概觀](environment/overview.md)
   + 應用 {#app}
      + [設定應用程式部署](application/configure-app-yaml.md)
      + [PHP設定](application/php-settings.md)
      + 屬性 {#properties}
         + [應用程式屬性](application/properties.md)
         + [Crons](application/crons-property.md)
         + [防火牆（僅限入門者）](application/firewall-property.md)
         + [勾點](application/hooks-property.md)
         + [變數](application/variables-property.md)
         + [Web](application/web-property.md)
         + [工作者](application/workers-property.md)
      + [設定靜態檔案的快取](application/set-cache.md)
   + 環境 {#env}
      + [設定環境部署](environment/configure-env-yaml.md)
      + [變數層級和選項](environment/variable-levels.md)
      + 覆寫變數 {#stage}
         + [環境變數](environment/variables-intro.md)
         + [管理員](environment/variables-admin.md)
         + [雲端變數](environment/variables-cloud.md)
         + [全域](environment/variables-global.md)
         + [建置](environment/variables-build.md)
         + [部署](environment/variables-deploy.md)
         + [部署後](environment/variables-post-deploy.md)
      + 設定通知 {#log}
         + [通知](environment/set-up-notifications.md)
         + [記錄處理常式](environment/log-handlers.md)
   + 路由 {#routes}
      + [設定路由](routes/routes-yaml.md)
      + [快取](routes/caching.md)
      + [重新導向](routes/redirects.md)
      + [伺服器端包含](routes/server-side-includes.md)
   + 服務 {#service}
      + [設定服務](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
      + [Valkey](services/valkey.md)
+ Fastly服務 {#cdn}
   + [概觀](cdn/fastly.md)
   + Fastly設定 {#setup-fastly}
      + [設定Fastly服務](cdn/fastly-configuration.md)
      + [自訂快取設定](cdn/fastly-custom-cache-configuration.md)
      + [自訂錯誤和維護頁面](cdn/fastly-custom-response.md)
   + [Web應用程式防火牆](cdn/fastly-waf-service.md)
   + [影像最佳化](cdn/fastly-image-optimization.md)
   + 使用VCL自訂 {#custom-vcl-snippets}
      + [開始使用](cdn/fastly-vcl-custom-snippets.md)
      + [將請求重新路由到CMS後端](cdn/fastly-vcl-wordpress.md)
      + [封鎖轉介垃圾訊息](cdn/fastly-vcl-badreferer.md)
      + [IP允許清單](cdn/fastly-vcl-allowlist.md)
      + [IP封鎖清單](cdn/fastly-vcl-blocking.md)
      + [略過Fastly快取](cdn/fastly-vcl-bypass-to-origin.md)
   + [Fastly疑難排解](cdn/fastly-troubleshooting.md)
+ 商店設定 {#configure-store}
   + [概觀](store/overview.md)
   + [最佳實務](store/best-practices.md)
   + [自訂主題](store/custom-theme.md)
   + [擴充功能](store/extensions.md)
   + [B2B模組](store/b2b-module.md)
   + [多個網站](store/multiple-sites.md)
   + [網站地圖和搜尋引擎自動機制](store/robots-sitemap.md)
   + [PayPal付款方法](store/paypal.md)
   + [設定管理](store/store-settings.md)
+ 啟動網站 {#launch}
   + [概觀](launch/overview.md)
   + [啟動檢查清單](launch/checklist.md)
   + [啟動步驟](launch/steps.md)
+ 監視網站 {#monitor}
   + [效能](monitor/performance.md)
   + [作業遙測](monitor/operational-telemetry.md)
   + New Relic服務 {#new-relic}
      + [New Relic概觀](monitor/new-relic-service.md)
      + [帳戶和使用者管理](monitor/account-management.md)
      + 調查績效 {#investigate}
         + [原則、警示和工作流程](monitor/investigate-performance.md)
         + [資料擷取](monitor/ingest-data.md)
         + [追蹤部署](monitor/track-deployments.md)
      + [記錄管理](monitor/log-management.md)
