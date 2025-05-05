---
title: Fastly影像最佳化
description: 瞭解如何透過啟用和設定Fastly影像最佳化，來最佳化影像傳送並簡化Adobe Commerce網站的影像管理。
feature: Cloud, Configuration, Media
source-git-commit: 1e789247c12009908eabb6039d951acbdfcc9263
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Fastly影像最佳化

Fastly影像最佳化(Fastly IO)提供即時影像操控和最佳化，以加速影像傳送並簡化回應式網頁應用程式影像來源集的維護。 設定好Fastly IO後，可提供下列影像最佳化功能：

- 強制有損轉換
- 深度影像最佳化
- 最適化畫素比率
- 支援常見的影像格式：PNG、JPEG、GIF和WebP

啟用和設定Fastly IO選項之前，您必須設定Fastly服務並設定Origin shielding。

根據您的組態設定，Fastly影像最佳化(Fastly IO)程式碼片段會插入VCL程式碼以執行影像最佳化，以加快店面產品影像的傳送速度。 設定Fastly IO有三個步驟：啟用、設定和驗證。

## 啟用Fastly IO

上傳Fastly IO VCL片段，從管理面板啟用Fastly影像最佳化(Fastly IO)。 此程式碼片段提供Fastly設定指示，使用預設設定，透過影像最佳化工具處理所有影像。

**必要條件：**

- 安裝或升級至Fastly模組1.2.62版或更新版本
- [配置Fastly Origin遮蔽和後端](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**若要啟用Fastly IO**：

1. 以管理員身分登入您的本機[管理員](../../get-started/onboarding.md#access-your-admin-panel)面板。

1. 選取&#x200B;**商店** > **設定** > **組態** > **進階** > **系統**。

1. 在右窗格中，展開&#x200B;**完整頁面快取**。

1. 選取&#x200B;**Fastly組態** > **影像最佳化**&#x200B;以指定組態設定。

1. 在&#x200B;_Fastly IO片段_&#x200B;欄位中，選取&#x200B;**啟用/停用**。

1. 上傳Fastly IO程式碼片段：

   - 選取&#x200B;**預設IO設定選項**&#x200B;以開啟影像最佳化預設設定選項頁面。
   - 選取&#x200B;**上傳**&#x200B;將VCL程式碼片段上傳至您的伺服器。

## 設定Fastly IO

視需要檢閱並更新影像最佳化的預設IO組態設定。 例如，您可能會想要變更有損格式的WebP和JPEG品質等級，或將提供JPEG影像的格式變更為&#x200B;_漸進式_&#x200B;或&#x200B;_基準線_。 此外，您也可以使用Fastly IO來獲得更精細的影像最佳化功能，例如：

- 強制有損轉換
- 深度影像最佳化
- 最適化畫素比率

**要更新Fastly IO**：

1. 在&#x200B;_預設IO設定選項_&#x200B;欄位的&#x200B;_Fastly設定_&#x200B;頁面上，選取&#x200B;**設定**。

   ![檢視Fastly IO組態設定](../../assets/cdn/fastly-io-default-config.png)

1. 檢閱並更新&#x200B;_影像最佳化預設組態選項_&#x200B;頁面上的Fastly IO組態設定：

   ![檢閱Fastly IO設定](../../assets/cdn/fastly-io-config-options.png)

   - **自動WebP？** — 保留預設設定(`Yes`)，以便在支援影像的瀏覽器中將其轉換為WebP格式。 如果您將設定變更為&#x200B;**否**，Fastly會使用影像檔案型別，而非將影像轉換為WebP格式。

   - **預設WebP （失真）品質** — 保留預設設定(`85`)或輸入有損檔案格式影像的壓縮等級。 您可以指定從1到100的任何整數。

   - **預設JPEG格式控制項** — 保留預設設定(`Auto`)，或選取提供影像時要使用的JPEG型別。 如果值設定為&#x200B;_Auto_，Fastly會傳送輸出型別符合輸入型別的影像。 選取&#x200B;_基準線_&#x200B;以從左上到右下逐行顯示影像。 選取&#x200B;_漸進式_&#x200B;以顯示載入時變得清晰的模糊影像。

   - **預設JPEG品質** — 保留預設設定(`85`)，或輸入損毀檔案格式品質的壓縮等級。 指定從1到100的任何整數。

   - **是否允許升級？** — 保留預設設定(`No`)，或選取`Yes`以傳回大於原始來源檔案的影像，使其符合要求的尺寸。

   - **調整篩選器大小** — 保留預設設定(`Lancsoz3`)，或選取替代專案。 此設定會指定用來傳送調整大小影像的濾鏡。 根據選取的濾鏡，調整大小的影像可以有較高或較低的畫素數量。

      - `Lanczos3` （預設） — 提供最佳品質的影像。 它會增加偵測影像中邊緣和線性特徵的能力，並使用&#x200B;_[!DNL sinc]_&#x200B;重新取樣以提供最佳可能的重新建構。
      - `Lanczos2` — 使用與`Lancsoz3`相同的篩選器，但使用&#x200B;_[!DNL sinc]_&#x200B;重新取樣函式的近似值較不準確。
      - `Bicubic` — 將影像縮小時具有自然銳利化效果。
      - `Bilinear` — 在放大影像時具有自然的平滑效果。
      - `Nearest` — 在調整畫素圖稿大小時，具有自然的畫素化效果。

1. 指定Fastly服務的IO組態設定後，選取&#x200B;**取消**&#x200B;以返回Fastly組態設定。

1. 在影像最佳化設定&#x200B;_啟用深層影像最佳化_&#x200B;欄位中，選取&#x200B;**是**&#x200B;以開啟深層影像最佳化。

   ![啟用Fastly IO深層影像最佳化](../../assets/cdn/fastly-io-deep-image-config.png)

   預設會關閉深層影像最佳化。 啟用此功能後，Adobe Commerce中的內建調整大小功能會關閉，並且調整大小的工作會解除安裝到Fastly IO服務。 影像最佳化僅適用於產品影像。 CMS影像不會調整大小。 請參閱[Fastly檔案](#deep-image-optimization)。

1. 啟用深層影像最佳化之後，請啟用[最適化畫素比率](#adaptive-pixel-ratios)功能，以產生最佳化的影像，以用於回應式網站。

   ![啟用Fastly IO最適化畫素比率](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - 在&#x200B;_啟用最適化裝置畫素比率_&#x200B;欄位中，選取&#x200B;**是**。
   - 在&#x200B;_裝置畫素比率_&#x200B;欄位中，接受預設設定，或選取&#x200B;**系統輸入**&#x200B;核取方塊以移除設定。 然後，選取所需的比例。 較高的裝置畫素比設定可提供較大的影像。

1. 選取&#x200B;**儲存組態**。

### 強制有損轉換

根據預設，Fastly IO服務會強制將無損格式（例如PNG、BMP或WEBP）轉換為JPEG/WEBP格式。

強制有損轉換的優點在於提供的影像較小。
例如，使用JPEG或WEBp格式而不是PNG，大小可能會減少60到70%，具體取決於Fastly IO設定中指定的品質等級。

根據為影像最佳化選取的品質等級，您可能會察覺影像中的視覺差異。 例如，Alpha色版/透明度會被去除並取代為白色背景，除非您使用使用使用佈景主題背景色彩的「深度影像最佳化」。

如果您關閉有損轉換(`WebP Auto? = No`)，Fastly IO只會將JPEG影像變更為相容瀏覽器的WEBP格式。 不會變更其他影像型別。 例如，如果原始影像是PNG，則Fastly IO服務的輸出是PNG。

### 深度影像最佳化

預設會關閉深層影像最佳化。 啟用此選項會關閉內建的Adobe Commerce調整大小並將其完全解除安裝到Fastly IO服務。
此功能只會調整_產品_&#x200B;影像的大小。 CMS影像不會調整大小。

啟用深層影像最佳化會將背景色彩定義新增至主題中定義的每個影像。 因此，WebP影像會從WebP lossless切換為WebP lossy。 無損與有損之間的主要差異之一是，有損會捨棄PNG影像的Alpha色版，因而產生小得多的影像。 不過，在使用不同背景的產品和行銷活動頁面上，具有透明度的影像可能會看起來有些奇怪。

例如，下列程式碼代表Luma主題中影像的原始來源：

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

啟用Fastly IO Deep image optimization功能時，會重寫影像的原始原始程式碼，如下列範例所示：

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### 最適化畫素比率

「自我調整畫素比率」功能對於最佳化Progressive Web應用程式的影像非常有用。 它可讓您為每個產品影像新增`srcset`，從單一影像來源檔案提供多種影像大小和解析度。

啟用最適化畫素比功能時，Fastly IO服務會提供可適應變化`device-pixel-ratios`的固定寬度影像。
例如，服務會修改產品影像定義，如下列範例所示：

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

請參閱`srcset` [瀏覽器支援](https://caniuse.com/#feat=srcset)和[規格](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset)。

## 驗證Fastly IO

在您啟用和設定Fastly IO後，無論是否啟用Fastly IO，都可透過執行網頁速度測試來驗證您的設定。 同時，檢閱您商店中的影像，以檢查影像大小和外觀是否有問題。
