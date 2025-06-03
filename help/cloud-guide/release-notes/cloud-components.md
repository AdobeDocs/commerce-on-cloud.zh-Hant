---
title: 適用於Commerce的雲端元件
description: 請參閱雲端元件套件最新改良的清單。
recommendations: noDisplay, catalog
exl-id: 34aec593-e2ea-4060-a6b9-6f4cb95a11c0
source-git-commit: dcf71ffbdafae46e6a02735c090c33a8fe248bc6
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 0%

---

# 適用於Commerce的雲端元件

[雲端元件](https://github.com/magento/magento-cloud-components)套件為部署在雲端基礎結構上的網站提供延伸的Adobe Commerce核心功能。 此套件是ECE-Tools套件的相依性。 此套件是[適用於Commerce](cloud-tools-suite.md)的Cloud Tools Suite的元件，本發行說明說明將說明此套件的最新改善。

`magento/magento-cloud-components`封裝使用以下版本順序： `<major>.<minor>.<patch>`

發行說明包括：

- ![新圖示](../../assets/new.svg)新功能
- ![修正圖示](../../assets/fix.svg)修正和改良

<!--Add release notes below-->

## v1.1.2 {#latest}

發行日期： 2025年6月3日

- ![修正圖示](../../assets/fix.svg) **改善與2.4.8的相容性** — 更新協力廠商程式庫的相容性，以與2.4.8<!-- MCLOUD-13707	 - -->更相容

## v1.1.1

發行日期： 2025年2月6日

- ![新圖示](../../assets/new.svg) **PHP 8.4** — 已新增對PHP 8.4的支援。<!-- MCLOUD-13148	 - -->
- ![修正圖示](../../assets/fix.svg) **修正快取熱身** — 修正快取熱身期間類別URL的問題。<!-- MCLOUD-12454 - -->


## v1.1.0

發行日期： 2024年10月7日

- ![修正圖示](../../assets/fix.svg) **重構的程式碼** — 已移除舊版PHP 7.4、7.3、7.2及相關程式庫的支援。<!-- MCLOUD-9278 - -->
- ![修正圖示](../../assets/fix.svg) **升級的Monolog版本** — 新增對monolog 3.6的支援。<!-- MCLOUD-12855 - -->

## v1.0.14

發行日期： 2024年4月8日

- ![新圖示](../../assets/new.svg) **PHP** — 已新增對PHP 8.3的支援。

## v1.0.13

發行日期： 2023年3月10日

- ![新圖示](../../assets/new.svg) **PHP 8.2**&#x200B;的增強型支援 — 已修正某些PHP 8.2.x版本的相容性問題，以支援Commerce 2.4.6。

## v1.0.12

發行日期： 2022年9月13日

- ![修正圖示](../../assets/fix.svg) **熱身錯誤** — 修正當管理員中的頁面可見度設為&#x200B;[**個別不可見**](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-attributes-product#simple-product-csv-file-structure)時，嘗試[熱身](../environment/variables-post-deploy.md#warm_up_pages)的問題，導致部署記錄中出現`ERROR: Warming up failed: <link to page>`個錯誤。<!-- MCLOUD-9134 -->

## v1.0.11

發行日期： 2022年8月4日

- ![修正圖示](../../assets/fix.svg) **已新增Symfony 5.4相容性的支援** — 修正Symfony 5.4的相容性。<!-- AC-3550 -->

## v1.0.10

發行日期： 2022年3月10日

- ![新圖示](../../assets/new.svg) **支援PHP 8.1** — 已新增對PHP 8.1的支援，並捨棄對PHP 7.1的支援。

## v1.0.9

發行日期： 2021年10月25日

- ![修正圖示](../../assets/fix.svg) **更新獨白** — 已將`monolog`封裝所需的最低版本更新為`^2.3`。<!-- ACMP-1263 -->

## v1.0.8

發行日期： 2021年7月29日

- ![修正圖示](../../assets/fix.svg) **從自動產生的URL移除結尾斜線** — 從快取預熱期間產生的類別頁面URL移除結尾斜線。<!--MCLOUD-7192-->

## v1.0.7

發行日期： 2020年9月9日

- ![新圖示](../../assets/new.svg) **記錄改善** — 縮小`cache.log`檔案的大小以改進效能。<!--MCLOUD-6859-->

- ![修正圖示](../../assets/fix.svg)修正快取組態值中導致`php bin/magento cache:evict` CLI命令失敗的型別錯誤。

## v1.0.6

發行日期： 2020年8月5日

- ![新圖示](../../assets/new.svg) **改善Redis效能** — 已新增`./bin/magento cache:evict`命令以移除過期的Redis金鑰，這會減少Redis記憶體使用量以提升效能。<!--MCLOUD-6023-->

- ![修正圖示](../../assets/fix.svg)已移除內容中&#x200B;*New Relic記錄檔*&#x200B;的支援，以修正效能問題。<!--MCLOUD-6422-->

## v1.0.5

發行日期： 2020年6月25日

- ![修正圖示](../../assets/fix.svg)修正magento/magento-cloud-components版本1.0.4中發生的問題，此問題導致排清快取作業在部署階段失敗，並中斷部署程式。

## v1.0.4

發行日期： 2020年6月25日

- ![新圖示](../../assets/new.svg) **在內容中實作New Relic記錄檔**—Adobe Commerce產生的應用程式記錄檔現在顯示在New Relic的追蹤中，以改善疑難排解功能。<!--MCLOUD-6029-->

- ![新圖示](../../assets/new.svg) **已改善記錄** — 已新增記錄以追蹤快取失效和完整重新索引事件。<!--MCLOUD-6157-->

## v1.0.3

發行日期： 2020年2月27日

- ![修正圖示](../../assets/fix.svg)修正相容性問題，以支援使用舊版PHP的`ece-tools` 2002.0.x版本。

## v1.0.2

發行日期： 2020年2月6日

- ![新圖示](../../assets/new.svg)已擴充`WARM_UP_PAGES`環境變數的功能，以支援特定產品頁面的快取預先載入。 如需詳細的功能說明，請參閱[部署後變數](../environment/variables-post-deploy.md#warm_up_pages)主題。<!--MAGECLOUD-4444-->

- ![修正圖示](../../assets/fix.svg)修正使用`WARM_UP_PAGES`功能填入快取時，無效的存放區URL會導致後部署掛接失敗的問題。 只有在URL重寫停用時，才會發生此問題。<!-- MAGECLOUD-4094 -->

## v1.0.1

發行日期： 2019年7月23日

- ![修正圖示](../../assets/fix.svg)已修正影響使用預設商店URL的&#x200B;[**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages)&#x200B;功能的問題。 現在，如果`config:show:default-url`命令無法擷取基底URL，則會使用MAGENTO_CLOUD_ROUTES變數中的URL。<!-- MAGECLOUD-3866 -->

## v1.0.0

發行日期： 2019年6月12日

這是[`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components)封裝的第一個版本，這是`ece-tools`封裝版本2002.0.20和更新版本的新相依性。

- ![新圖示](../../assets/new.svg)已新增使用規則運算式模式來設定&#x200B;**WARM_UP_PAGES**&#x200B;環境變數，以快取單一頁面、多個網域和多個頁面的功能。 檢視[部署後變數](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
