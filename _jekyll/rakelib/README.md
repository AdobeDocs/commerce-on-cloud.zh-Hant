---
source-git-commit: f75dbbc9726ff21a648b1bb3d8f06cb45a5d6b08
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---
# Rake程式庫檔案

此目錄包含依功能組織的Rake工作定義。 Rake會自動載入此目錄中的所有`.rake`檔案。

## 檔案組織

### `adobe-docs-tasks.rake`

包含Experience League上Adobe Commerce的常見需求、共用功能和非名稱空間工作，說明檔案存放庫Rake工作：

- `whatsnew` — 產生新聞摘要的資料（預設：自從上次更新後）
- `render` — 演算範本化檔案並維護包含

### `includes.rake`

包含在`:includes`名稱空間中組織的包含管理任務：

- `includes:maintain_relationships` — 探索並維護Markdown檔案中的包含關係
- `includes:maintain_timestamps` — 根據包含檔案變更新增/更新時間戳記
- `includes:maintain_all` — 依序執行兩個作業
- `includes:unused` — 尋找未使用的包含檔案

### `images.rake`

包含在`:images`名稱空間中組織的影像管理工作：

- `images:optimize` — 最佳化修改後未認可檔案中的影像
- `images:unused` — 尋找專案中未使用的影像

## 運作方式

Rake會自動探索並載入`.rake`目錄中的任何`rakelib/`檔案。 這可讓我們：

1. **依功能組織工作** — 將相關工作群組在一起
2. **保持主要Rakefile乾淨** — 專注於核心專案任務
3. **輕鬆新增工作群組** — 只需建立新的`.rake`檔案
4. **保持關注點分離** — 每個檔案處理特定網域

## 新增任務群組

若要新增一組相關工作：

1. 在此目錄中建立新的`.rake`檔案
2. 使用描述性名稱空間（例如，`namespace :images`用於影像相關工作）
3. 在名稱空間中定義您的工作
4. Rake會自動載入它們

## 範例結構

```ruby
namespace :your_namespace do
  desc 'Description of what this task does'
  task :task_name do
    # Task implementation
  end
end
```

## 使用情況

任務建立後即可立即使用：

```bash
rake your_namespace:task_name
```

## 優點

- **模組化**：每個檔案都集中在特定區域
- **可維護性**：更容易找到及修改特定工作群組
- **可擴充性**：可以新增許多工作群組，而不會造成主要Rakefile雜亂
- **團隊開發**：不同的開發人員可以處理不同的任務群組
