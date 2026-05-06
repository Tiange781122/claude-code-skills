# Obsidian Markdown 轉 Word 文件產生器

將 Obsidian Markdown 建議書（v2.0 格式）轉換為格式化的 Word (.docx) 文件，保留所有表格、圖片、程式碼區塊與階層結構。

## 何時使用

- 需要將 Obsidian Markdown 建議書轉為可交付客戶的 Word 文件
- 來源為 `fortigate-report-generator` skill 產出的 v2.0 格式建議書

## 前置要求

- Python 3 + `python-docx` 套件（`pip3 install python-docx`）
- 來源 Markdown 檔案（Obsidian 格式，含 `![[圖片]]` 引用）
- 圖片檔案位於 Obsidian vault 的 `12.資源/12.3.圖片資產/` 目錄

## 工作流程

### 步驟 1：讀取來源 Markdown

讀取使用者指定的 `.md` 檔案，解析以下結構：
- YAML-like 資訊表格（客戶名稱、設備型號等）
- `## 章節標題`（Heading 1）
- `### 子標題`（Sub-heading，以粗體 Normal 呈現）
- Markdown 表格（`| ... | ... |`）
- 圖片引用（`![[檔名.png]]`）
- 程式碼區塊（` ``` ... ``` `）
- 條列項目（`- ...` 或 `1. ...`）
- 一般段落文字

### 步驟 2：定位所有圖片

解析所有 `![[...]]` 引用，在 vault 的 `12.資源/12.3.圖片資產/` 目錄中搜尋對應圖片檔案。記錄每張圖片的完整路徑。

### 步驟 3：產生 Python 轉換腳本

產生一個 Python 腳本，使用 `python-docx` 建立 Word 文件。腳本必須遵循以下格式規範。

### 步驟 4：執行腳本並驗證

執行 Python 腳本產生 `.docx` 檔案，驗證檔案大小合理（應包含所有圖片）。

## 格式規範

### 頁面設定

| 項目 | 值 |
|:---|:---|
| 紙張大小 | A4（8.27" x 11.69"） |
| 上下左右邊距 | 1 英吋 |

### 字型與樣式

| 元素 | 字型 | 大小 | 粗體 | 顏色 | 對齊 |
|:---|:---|:---|:---|:---|:---|
| 文件標題 | 標楷體-繁 | 18pt | 是 | #272343 | 置中 |
| Heading 1（`##`） | 標楷體-繁 | 16pt | — | #2E74B5 | 靠左 |
| Sub-heading（`###`） | 標楷體-繁 | 15pt | 是 | #272343 | 靠左 |
| 一般段落 | 標楷體-繁 | 11.5pt | — | — | 靠左 |
| 表格標題列 | 標楷體-繁 | 11.5pt | 是 | #272343 | — |
| 表格內容 | 標楷體-繁 | 11.5pt | — | — | — |
| 程式碼區塊 | 標楷體-繁 | 11.5pt | — | — | 靠左 |
| 條列項目 | 標楷體-繁 | 11.5pt | — | — | 靠左 |

### 表格樣式

- 使用 `Normal Table` 樣式
- 邊框：全邊框（top/bottom/left/right/insideH/insideV），color=auto, size=1
- 標題列背景色：`#E7E6E6`（淺灰）
- 標題列文字：粗體，顏色 `#272343`
- 表格寬度：自動適應頁面寬度

### 圖片

- 寬度：5.73 英吋（適應頁面寬度減去邊距）
- 圖片獨立一個段落，前後各一個空段落
- 若圖片檔案不存在，插入替代文字「[圖片無法載入: 檔名]」

### 程式碼區塊

- 段落背景色（shading）：`#D3D3D3`（淺灰）
- 保留換行與縮排
- 字型同一般段落

### 資訊表格（文件首頁）

文件標題後緊接一個 2 欄資訊表格，從 Markdown 的第一個表格提取：

| 項目 | 內容 |
|:---|:---|
| 客戶名稱 | （從 Markdown 提取） |
| 設備型號 | （從 Markdown 提取） |
| 設備序號 | （從 Markdown 提取） |
| 文件版本 | （從 Markdown 提取） |
| 文件日期 | （從 Markdown 提取） |
| 建議書製作 | 逸凡科技 |

### 水平分隔線（`---`）

Markdown 中的 `---` 在 Word 中以空段落表示（不需特殊處理）。

## Python 腳本結構

腳本應包含以下函式：

```python
from docx import Document
from docx.shared import Pt, Inches, RGBColor, Cm
from docx.enum.text import WD_ALIGN_PARAGRAPH
from docx.oxml.ns import qn
import re, os

FONT_NAME = '標楷體-繁'
TITLE_COLOR = RGBColor(0x27, 0x23, 0x43)
HEADING_COLOR = RGBColor(0x2E, 0x74, 0xB5)
HEADER_BG = 'E7E6E6'
CODE_BG = 'D3D3D3'
IMG_WIDTH = Inches(5.73)
IMAGE_DIR = '圖片目錄完整路徑'

def set_run_font(run, size=Pt(11.5), bold=None, color=None):
    """設定 run 的字型屬性"""

def add_title(doc, text):
    """新增置中粗體標題"""

def add_heading1(doc, text):
    """使用 Word 內建 Heading 1 樣式（doc.add_heading(text, level=1)），以便插入目錄時可自動抓取。加入後再設定字型為標楷體-繁 16pt #2E74B5。"""

def add_subheading(doc, text):
    """新增子標題（Normal style + bold + 15pt + #272343）"""

def add_paragraph(doc, text):
    """新增一般段落"""

def add_bullet(doc, text, level=0):
    """新增條列項目（List Paragraph style）"""

def add_code_block(doc, text):
    """新增程式碼區塊（灰底段落）"""

def add_image(doc, image_name):
    """解析 ![[image_name]] 並插入圖片"""

def add_table(doc, headers, rows):
    """新增格式化表格（含標題列灰底、邊框）"""

def set_table_borders(table):
    """設定表格全邊框"""

def set_cell_shading(cell, color):
    """設定儲存格背景色"""
```

### 關鍵實作細節

1. **Heading 1 必須使用 Word 內建樣式**：使用 `doc.add_heading(text, level=1)` 產生標題，這樣 Word 插入目錄時才能自動抓取。產生後再對 run 設定字型：
   ```python
   def add_heading1(doc, text):
       h = doc.add_heading(text, level=1)
       for run in h.runs:
           set_run_font(run, size=Pt(16), color=HEADING_COLOR)
   ```

2. **字型設定**：每個 run 都必須明確設定 `run.font.name` 和 `run._element.rPr.rFonts` 的 `w:eastAsia` 屬性為 `標楷體-繁`，否則中文字型不會生效：
   ```python
   run.font.name = FONT_NAME
   r = run._element
   rPr = r.find(qn('w:rPr'))
   if rPr is None:
       rPr = OxmlElement('w:rPr')
       r.insert(0, rPr)
   rFonts = rPr.find(qn('w:rFonts'))
   if rFonts is None:
       rFonts = OxmlElement('w:rFonts')
       rPr.insert(0, rFonts)
   rFonts.set(qn('w:eastAsia'), FONT_NAME)
   ```

2. **表格邊框**：使用 XML 操作設定 `tblBorders`：
   ```python
   def set_table_borders(table):
       tbl = table._tbl
       tblPr = tbl.tblPr if tbl.tblPr is not None else OxmlElement('w:tblPr')
       borders = OxmlElement('w:tblBorders')
       for border_name in ['top', 'left', 'bottom', 'right', 'insideH', 'insideV']:
           border = OxmlElement(f'w:{border_name}')
           border.set(qn('w:val'), 'single')
           border.set(qn('w:sz'), '1')
           border.set(qn('w:space'), '0')
           border.set(qn('w:color'), 'auto')
           borders.append(border)
       tblPr.append(borders)
   ```

3. **儲存格背景色**：
   ```python
   def set_cell_shading(cell, color):
       tc = cell._tc
       tcPr = tc.get_or_add_tcPr()
       shd = OxmlElement('w:shd')
       shd.set(qn('w:fill'), color)
       shd.set(qn('w:val'), 'clear')
       tcPr.append(shd)
   ```

4. **程式碼區塊背景**：
   ```python
   def add_code_block(doc, text):
       p = doc.add_paragraph()
       pPr = p._element.get_or_add_pPr()
       shd = OxmlElement('w:shd')
       shd.set(qn('w:val'), 'clear')
       shd.set(qn('w:fill'), CODE_BG)
       pPr.append(shd)
       run = p.add_run(text)
       set_run_font(run)
   ```

5. **Markdown 解析順序**：逐行讀取 Markdown，依以下優先順序判斷：
   1. `# 標題` → 文件標題（僅第一個）
   2. `## 章節` → Heading 1
   3. `### 子標題` → Sub-heading
   4. ` ``` ` → 進入/離開程式碼區塊模式
   5. `| ... |` → 收集表格行，遇到非表格行時輸出表格
   6. `![[...]]` → 圖片
   7. `---` → 分隔線（空段落）
   8. `- ...` → 條列項目
   9. 其他 → 一般段落

6. **表格解析**：
   - 跳過分隔行（`|:---|:---|`）
   - 第一行為標題列
   - 處理行內 code（`` ` ``）時保留為純文字

7. **行內格式**：段落文字中的 `` `code` `` 不需特殊處理（保留為純文字即可，Word 中不區分 inline code）。

## 輸出

- 檔案格式：`.docx`
- 儲存位置：與來源 `.md` 檔案相同目錄
- 檔名：由使用者指定，或預設為 `{客戶名稱}{據點}FortiGate 組態安全檢視與調整建議書.docx`

## 注意事項

1. 腳本可能很長，建議分段寫入後執行。
2. 圖片路徑需使用完整絕對路徑。
3. 若 `python-docx` 未安裝，先執行 `pip3 install python-docx`。
4. 產出的 `.docx` 檔案大小應與圖片總大小相近（通常 3-8 MB）。
5. 表格中的 inline code 標記（`` ` ``）在轉換時移除，僅保留文字內容。
6. Markdown 中的粗體（`**text**`）轉為 Word 粗體 run。
