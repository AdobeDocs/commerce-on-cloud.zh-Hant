---
source-git-commit: 5236f2718e5091dfc2fc140e15d4ac87a073d1d0
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---
# 影像最佳化的預先提交鉤點

此目錄包含預先提交掛接，可在影像提交至存放庫之前自動最佳化影像。

## 鉤子會做什麼

- **自動偵測**&#x200B;個暫存的影像檔案(PNG、JPG、JPEG、GIF、SVG)
- **執行`image_optim`**&#x200B;以壓縮和最佳化影像
- **自動重新存放最佳化的影像**
- **確定所有認可的影像都已正確最佳化**

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
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## 影像指導方針

- **PNG**：用於熒幕擷取畫面和UI元素（將會自動最佳化）
- **SVG**：用於圖示和簡單圖形（預設會停用最佳化）
- **JPEG**：用於像片（將自動最佳化）
- **GIF**：用於動畫（將會自動最佳化）

預先提交掛接將會在提交時自動最佳化所有影像。

## 手動最佳化

針對手動影像最佳化：

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## 設定

掛接使用組態檔`_jekyll/.image_optim`來自訂最佳化設定：

- **PNG**：使用`advpng`、`optipng`和`pngquant`
- **JPEG**：使用`jhead`、`jpegoptim`和`jpegtran`
- **GIF**：使用`gifsicle`
- **SVG**： SVG最佳化預設為停用（可能會破壞複雜的向量圖形和動畫）

## 疑難排解

### 連結未執行

- 檢查掛接設定： `git config core.hooksPath`
- 確定掛接檔案是可執行檔： `chmod +x .githooks/pre-commit`
- 確認您位於含有`_jekyll`目錄的正確存放庫

### 最佳化失敗

- 驗證`bundle install`已在`_jekyll`目錄中執行
- 檢查是否已安裝`image_optim`和`image_optim_pack` gem
- 檢閱`.image_optim`設定檔

### 效能問題

- 在`_jekyll/.image_optim`中調整執行緒計數
- 設定`DEBUG=1`環境變數以取得詳細的錯誤資訊

## 運作方式

1. **預先認可觸發器**：當您執行`git commit`時，掛接會自動執行
2. **影像偵測**：掃描暫存檔案的影像副檔名
3. **最佳化**：在每個暫存的影像上執行`image_optim`
4. **重新暫存**：自動將最佳化的影像新增回暫存區域
5. **認可繼續進行**：如果最佳化成功，認可會正常繼續

## 支援的影像格式

- **PNG** (`.png`) — 無失真和失真壓縮
- **JPEG** (`.jpg`， `.jpeg`) — 包含中繼資料清理的失真壓縮
- **GIF** (`.gif`) — 動畫和靜態最佳化
- **SVG** (`.svg`) — 向量最佳化（預設為停用）

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
