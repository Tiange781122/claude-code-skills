# Obsidian Canvas Creator（Advanced Canvas 增強版）

生成 Obsidian Canvas 檔案（`.canvas`），完整支援 [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/) 及 Advanced Canvas 外掛的擴展格式。

## 何時使用

- 從層級資訊生成心智圖
- 建立知識圖譜、概念圖、流程圖
- 視覺化概念之間的連接關係
- 建立簡報投影片（Presentation Mode）
- 建立看板（Project Board）
- 任何涉及 `.canvas` 檔案的建立或編輯

## 工作流程

### 建立新 Canvas

1. 建立 `.canvas` 檔案，頂層結構為 `{"nodes": [], "edges": []}`（需 Advanced Canvas 功能時加 `metadata`）
2. 為每個節點產生唯一 16 字元 hex ID（如 `"6f0ad84f44ce9c17"`）
3. 加入節點，設定必要欄位：`id`、`type`、`x`、`y`、`width`、`height`
4. 加入邊線，透過 `fromNode`/`toNode` 引用節點 ID
5. 執行碰撞偵測與驗證

### 編輯既有 Canvas

1. 讀取並解析 `.canvas` 檔案
2. 以 `id` 定位目標節點或邊線
3. 修改屬性後寫回
4. 確認新 ID 不與既有 ID 衝突，所有邊線引用仍有效

## JSON Canvas 格式規範

### 頂層結構

```json
{
  "metadata": { "version": "1.0-1.0" },
  "nodes": [],
  "edges": []
}
```

`metadata` 為 Advanced Canvas 擴展，標準 Canvas 可省略。節點陣列順序決定 z-index（首項在最底層）。

### 節點通用屬性

| 屬性 | 必要 | 類型 | 說明 |
|------|------|------|------|
| `id` | 是 | string | 唯一 16 字元 hex ID |
| `type` | 是 | string | `text`、`file`、`link`、`group` |
| `x` | 是 | integer | X 座標（px），向右為正 |
| `y` | 是 | integer | Y 座標（px），向下為正，位置為左上角 |
| `width` | 是 | integer | 寬度（px） |
| `height` | 是 | integer | 高度（px） |
| `color` | 否 | canvasColor | 預設 `"1"`-`"6"` 或 hex（如 `"#FF6B6B"`） |

### Text 節點

| 屬性 | 必要 | 說明 |
|------|------|------|
| `text` | 是 | 支援 Markdown 語法，換行用 `\n`（勿用 `\\n`） |

```json
{
  "id": "6f0ad84f44ce9c17",
  "type": "text",
  "text": "# 標題\n內容",
  "x": 0, "y": 0,
  "width": 260, "height": 100,
  "color": "1",
  "styleAttributes": {
    "textAlign": "center",
    "shape": "rectangle",
    "border": "solid"
  }
}
```

### File 節點

| 屬性 | 必要 | 說明 |
|------|------|------|
| `file` | 是 | Vault 內檔案路徑 |
| `subpath` | 否 | 連結至標題或區塊（以 `#` 開頭） |

### Link 節點

| 屬性 | 必要 | 說明 |
|------|------|------|
| `url` | 是 | 外部 URL |

### Group 節點

| 屬性 | 必要 | 說明 |
|------|------|------|
| `label` | 否 | 群組標題 |
| `background` | 否 | 背景圖片路徑 |
| `backgroundStyle` | 否 | `cover`、`ratio`、`repeat` |
| `collapsed` | 否 | `true` 可摺疊群組（Advanced Canvas） |

群組內的節點只需座標在群組範圍內即可。

## Advanced Canvas 擴展

### styleAttributes（節點）

**shape 可用值**：

| shape | 用途 |
|-------|------|
| `rectangle` | 預設矩形 |
| `pill` | 圓角膠囊（適合標題） |
| `diamond` | 菱形（決策判斷） |
| `parallelogram` | 平行四邊形（輸入/輸出） |
| `circle` | 圓形 |
| `predefined-process` | 預定義流程（雙線矩形） |
| `document` | 文件形狀（波浪底邊） |
| `database` | 圓柱體（資料庫） |

**border 可用值**：`solid`、`dashed`、`dotted`、`invisible`

**textAlign 可用值**：`left`、`center`、`right`

### 簡報模式

在 metadata 中設定 `startNode`，用邊線連接投影片順序：

```json
{
  "metadata": { "version": "1.0-1.0", "startNode": "slide1" },
  "nodes": [...],
  "edges": [...]
}
```

## 邊線屬性

| 屬性 | 必要 | 預設值 | 說明 |
|------|------|--------|------|
| `id` | 是 | — | 唯一 ID |
| `fromNode` | 是 | — | 來源節點 ID |
| `toNode` | 是 | — | 目標節點 ID |
| `fromSide` / `toSide` | 否 | — | `top`、`right`、`bottom`、`left` |
| `fromEnd` | 否 | `none` | `none` 或 `arrow` |
| `toEnd` | 否 | `arrow` | `none` 或 `arrow` |
| `fromFloating` / `toFloating` | 否 | — | `true` 自動選擇最短連接側（推薦） |
| `color` | 否 | — | canvasColor |
| `label` | 否 | — | 邊線文字標籤 |

### styleAttributes（邊線，Advanced Canvas）

```json
{
  "styleAttributes": {
    "path": "solid",
    "arrow": "triangle",
    "pathfindingMethod": "bezier"
  }
}
```

**path 樣式**：`solid`、`long-dashed`、`short-dashed`、`dotted`

**arrow 樣式**：`triangle`、`triangle-outline`、`thin-triangle`、`halved-triangle`、`diamond`、`diamond-outline`、`circle`、`circle-outline`、`blunt`

**pathfindingMethod**：`bezier`（預設曲線）、`direct`（直線）、`square`（直角）、`a-star`（A* 避障）

### 邊線使用慣例

| 連線類型 | path | color | arrow | 用途 |
|---------|------|-------|-------|------|
| 樹狀（父→子） | `solid` | 繼承父節點色 | `triangle` | 心智圖主結構 |
| 跨分支關聯 | `dotted` | `"6"` 紫色 | `triangle-outline` | 知識點之間的關聯 |
| 流程順序 | `solid` | 無 | `triangle` | 流程圖步驟 |
| 雙向關聯 | `dotted` | `"5"` 青色 | 兩端都 `arrow` | 互相影響的概念 |

跨分支關聯邊線必須使用 `fromFloating: true, toFloating: true`。

## 色彩

| 預設值 | 顏色 |
|--------|------|
| `"1"` | 紅 |
| `"2"` | 橙 |
| `"3"` | 黃 |
| `"4"` | 綠 |
| `"5"` | 青 |
| `"6"` | 紫 |

亦可使用 hex 值如 `"#FF6B6B"`。

## 節點尺寸計算規則

依文字內容精確計算：
- CJK 字元：每字 14px 寬
- ASCII 字元：每字 8px 寬
- 每行高度：30px
- 左右 padding：40px，上下 padding：16px
- 最小寬度 160px，最大 320px
- 最小高度 44px

建議尺寸參考：

| 節點類型 | 建議寬度 | 建議高度 |
|---------|---------|---------|
| 小型文字 | 200-300 | 80-150 |
| 中型文字 | 300-450 | 150-300 |
| 大型文字 | 400-600 | 300-500 |
| 檔案預覽 | 300-500 | 200-400 |
| 連結預覽 | 250-400 | 100-200 |

## 佈局規則

### 心智圖（MindMap）

- L0 中心節點：座標 (0, 0)，使用 `pill` 形狀 + 色彩 `"1"`
- L1 主分支：半徑 1500px，均勻放射分佈，使用色彩區分
- L2 子主題：半徑 900px（從 L1 出發），扇形展開 spread 16°
- L3 細節：半徑 680px（從 L2 出發），扇形展開 spread 13°
- L4 最細節：半徑 520px（從 L3 出發），扇形展開 spread 11°

### 自由佈局（Freeform）

- 使用 Group 節點做視覺分組
- 相關節點聚集在同一群組內
- 群組間保持 200px 以上間距
- 群組內節點保持 20-50px padding
- 座標可為負值（Canvas 無限延伸）
- 對齊至 10 或 20 的倍數以保持整齊

## 碰撞修正

生成後必須執行碰撞偵測：
1. 檢查所有節點對是否重疊（含 10px padding）
2. 重疊的節點沿中心連線方向互相推開 25px
3. 迭代最多 120 次直到 0 重疊

## 驗證清單

- [ ] JSON 格式正確且可解析
- [ ] 所有節點與邊線 ID 唯一（16 字元 hex）
- [ ] 所有 `fromNode`/`toNode` 引用的節點 ID 存在
- [ ] 各節點類型的必要欄位完整（`text` 需 `text`、`file` 需 `file`、`link` 需 `url`）
- [ ] `fromSide`/`toSide` 值為 `top`/`right`/`bottom`/`left`
- [ ] `fromEnd`/`toEnd` 值為 `none`/`arrow`
- [ ] 色彩值為 `"1"`-`"6"` 或有效 hex
- [ ] 0 個重疊節點
- [ ] 節點尺寸與文字內容匹配
- [ ] 跨分支關聯使用 dotted + 紫色 + floating
- [ ] 心智圖 L0 中心節點使用 pill 形狀

## 參考資料

- [JSON Canvas Spec 1.0](https://jsoncanvas.org/spec/1.0/)
- [JSON Canvas GitHub](https://github.com/obsidianmd/jsoncanvas)
- [Advanced Canvas Plugin](https://github.com/Developer-Mike/obsidian-advanced-canvas)
