---
source-git-commit: 3d3df68ab5e1781974348190b30372c0b8169d1d
workflow-type: tm+mt
source-wordcount: '689'
ht-degree: 0%

---
# 影像最佳化的預先提交鉤點

此目錄包含預先提交掛接，可在影像提交至存放庫之前自動最佳化影像。

## 鉤子會做什麼

- **自動偵測**&#x200B;個暫存的影像檔案(`.png`、`.jpeg`、`.jpg`、`.gif`、`.svg`)
- **執行`image_optim`**&#x200B;以壓縮和最佳化點陣影像(`.png`， `.jpeg`， `.jpg`， `.gif`)
- **自動重新存放最佳化的影像**
- **確定所有認可的點陣化影像**&#x200B;已正確最佳化
- **根據大小限制檢查分段SVG**，如果從`help/`中的任何檔案參照了過大的SVG，則中止認可（否則只警告）

## 優點

- 縮小存放庫大小
- 加速說明檔案的頁面載入
- 所有貢獻者的影像品質一致
- 不需要手動最佳化

## 先決條件

- Ruby 3.0或更新版本
- 套件組合工具
- Git

## 設定

### 自動設定（建議）

```bash
.githooks/setup-hooks.sh
```

### 手動設定

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### 完成專案設定

1. 複製存放庫：

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. 啟用預先提交掛接：

   ```bash
   .githooks/setup-hooks.sh
   ```

3. 安裝Jekyll相依性：

   ```bash
   cd _jekyll
   bundle install
   ```

## 測試鉤點

1. 新增影像檔案至您的存放庫
2. 分段： `git add <image-file>`
3. 嘗試認可： `git commit -m 'test'`
4. 勾點應該會自動最佳化影像

### 預期輸出

```bash
Found 1 staged image(s). Running optimization...

Checking images ...
path/to/your/image.png    100.00%
Pre-commit image checks complete!
```

### 單元測試

掛接的SVG連結偵測邏輯（可決定是否從`help/`參考過大的SVG）由只需要Ruby的套件式`minitest`的單元測試涵蓋 — 沒有gem或`_jekyll`設定：

```bash
ruby .githooks/test/svg_link_checker_test.rb
```

## 影像指導方針

- **PNG**：用於熒幕擷取畫面和UI元素（將會自動最佳化）
- **JPEG**：用於像片（將自動最佳化）
- **GIF**：用於動畫（將會自動最佳化）
- **SVG**：用於圖示和簡單圖形（未最佳化，但已根據大小限制檢查；只有從`help/`連結過大的SVG時，認可才會失敗）

預先認可掛接會在認可時自動最佳化`.png`、`.jpeg`/`.jpg`和`.gif`影像，並將依據大小限制(140 KB)檢查階段SVG。

如果暫存的SVG超過限制，而且是從`help/`中的檔案引用，則認可會中止。 如果`help/`中的任何地方都沒有參照過大的SVG，則連結只會列印警告，而認可會繼續進行。 將過大的SVG轉換為PNG：

```bash
cd _jekyll
bundle exec rake images:svg_to_png path=../help/assets/image.svg
```

路徑是相對於`_jekyll`的，因此`help/`下的影像會參照為`../help/...`。

## 手動最佳化

針對手動影像最佳化：

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## 設定

掛接使用組態檔`_jekyll/.image_optim.yml`來自訂最佳化設定：

- **PNG**：使用`advpng`、`optipng`和`pngquant`
- **JPEG**：使用`jhead`、`jpegoptim`和`jpegtran`
- **GIF**：使用`gifsicle`
- **SVG**：未最佳化（從`image_optim`排除，以保留向量圖形和動畫），但檢查140 KB大小限制

## 疑難排解

### 連結未執行

- 檢查掛接設定： `git config core.hooksPath`
- 確定掛接檔案是可執行檔： `chmod +x .githooks/pre-commit`
- 確認您位於含有`_jekyll`目錄的正確存放庫

### 最佳化失敗

- 驗證`bundle install`已在`_jekyll`目錄中執行
- 檢查是否已安裝`adobe-comdox-exl-rake-tasks` gem （提供掛接執行的`images:optimize`、`images:check_size`和`images:svg_to_png` Rake工作）
- 檢閱`.image_optim.yml`設定檔

### SVG超出大小限制

- 如果暫存的SVG超過140 KB，且從`help/`中的檔案參考，則認可會中止（否則勾點只會警告且認可會繼續進行）
- 將SVG轉換為PNG： `cd _jekyll && bundle exec rake images:svg_to_png path=../help/assets/image.svg` （路徑是相對於`_jekyll`的，因此`help/`下的影像會參照為`../help/...`）
- 然後暫存PNG以取代SVG並再次認可

### 效能問題

- 在`_jekyll/.image_optim.yml`中調整執行緒計數
- 設定`DEBUG=1`環境變數以取得詳細的錯誤資訊

## 運作方式

1. **預先認可觸發器**：當您執行`git commit`時，掛接會自動執行
2. **影像偵測**：掃描暫存檔案的影像副檔名
3. **最佳化**：在每個階段式PNG、JPEG或GIF上執行`image_optim`
4. **重新暫存**：自動將最佳化的影像新增回暫存區域
5. **SVG大小檢查**：根據140 KB大小限制檢查每個分段的SVG
6. **認可繼續進行**：如果最佳化成功，而且沒有從`help/`參考過大的SVG，則認可會正常繼續；否則認可會中止（沒有從`help/`參考過大的SVG只會觸發警告）

## 支援的影像格式

- **PNG** (`.png`) — 無失真和失真壓縮
- **JPEG** (`.jpg`， `.jpeg`) — 包含中繼資料清理的失真壓縮
- **GIF** (`.gif`) — 動畫和靜態最佳化
- **SVG** (`.svg`) — 未最佳化（以原樣提交以保留品質），但已針對140 KB大小限制進行檢查；如果超過限制且從`help/`參照SVG，則會中止提交（否則勾點只會警告）

## 最佳實務

1. **測試鉤點**：請先嘗試認可小型影像以確保其運作
2. **檢閱變更**：檢查Git差異以檢視最佳化結果
3. **監視器效能**：大型影像可能需要一段時間才能最佳化
4. **版本控制**：掛接儲存在此`.githooks/`目錄中

## 支援

若為預先提交鉤點的問題：

1. 檢查掛接輸出中的錯誤訊息
2. 驗證您的`image_optim`安裝程式是否正常運作
3. 先使用手動Rake作業進行測試
4. 檢閱掛接記錄檔和設定
5. 檢查掛接設定： `git config core.hooksPath`
