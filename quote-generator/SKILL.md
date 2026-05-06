# Skill：報價單產出（quote-generator）

根據比價結果，填入公司報價單範本，輸出 Excel 與 PDF，儲存至客戶資料夾。

---

## 何時使用

使用者說出以下任何一種需求時觸發：
- 「幫我出報價單」
- 「把比價結果填進報價單」
- 「產出給客戶的報價」
- 從 Skill 2（quote-analysis）完成後，接著產出正式報價單

---

## 輸入

| 項目 | 來源 |
|------|------|
| 比價結果 | Skill 2 的輸出，或使用者直接提供的品項＋單價清單 |
| 客戶基本資料 | 使用者輸入（名稱、聯絡人、電話、Email、地址） |
| 報價單範本 | 預設：`0.inbox/業務報價程序/已完成報價單範本/YYYYMMNN_範例客戶.xlsx` |
| 報價有效期 | 預設 45 天（可由使用者修改） |

若比價結果未提供，在繼續前詢問使用者提供品項與確認單價。

---

## 報價單範本結構

範本含兩個主要 Sheet：

### Sheet「報價單」（給客戶）
| 欄位 | 儲存格位置 | 說明 |
|------|-----------|------|
| 客戶名稱 | B6 | |
| 聯絡人 | B7 | 格式：`姓名 先生/小姐` |
| 電話 | E6 | |
| Email | B8 | |
| 地址 | E8 | |
| 報價單號碼 | I9 | 格式：見下方命名規則 |
| 品項起始列 | 第 12 列起 | A=項次, B=內容, G=單價, H=數量, I=金額 |
| 總計 | I38 | `=SUM(I12:I37)` |
| 5% 稅金 | I39 | `=I38*0.05` |
| 含稅總金額 | I40 | `=I38*1.05` |
| 付款方式 | B42 | 預設「月結90天」，依客戶別填寫 |
| 發票統一編號 | F44 | |

### Sheet「訂單請購單」（內部留存）
| 欄位 | 儲存格 | 說明 |
|------|--------|------|
| 報價單號碼 | F7 | 同報價單 |
| 訂單金額 | I7 | 含稅總金額 |
| 申請日期 | A7 | 今日日期 |
| 品項（產品型號）| A11 起 | |
| 成本（a）| D11 起 | 代理商報價（未稅） |
| 售價（小計）| G11 起 | 客戶報價（未稅） |
| 廠商/工程師 | I11 起 | 代理商名稱 |
| 終端客戶名稱 | H31 | |
| 業務人員 | A43 | |

---

## 執行步驟

### Step 1：收集客戶資料

若尚未提供，詢問：
1. 客戶公司名稱
2. 聯絡人姓名（請問是先生還是小姐？）
3. 聯絡電話
4. Email
5. 收貨地址
6. 發票統一編號
7. 付款條件（預設月結90天，如需更改請說明）

### Step 2：確認品項與售價

列出比價結果中的「建議採用」品項，顯示：
- 品名
- 代理商報價（成本）
- 預設建議售價（成本 × 1.2，僅供參考）

請使用者確認或修改每項售價後再繼續。

### Step 3：產生報價單號碼

格式：`YYMM` + `流水號5碼`
- 範例：`11504360`（民國年YY + 月MM + 流水號）
- 若使用者已有報價單號，直接使用其提供的號碼

### Step 4：填寫 Excel 範本

使用 Python openpyxl 填寫：

```python
import openpyxl
from copy import copy
import datetime

def fill_quote(template_path, output_path, data):
    wb = openpyxl.load_workbook(template_path)
    
    # ── 報價單 Sheet ──
    ws_quote = wb['報價單']
    ws_quote['B6'] = data['customer_name']
    ws_quote['B7'] = data['contact_person']
    ws_quote['E6'] = data['phone']
    ws_quote['B8'] = data['email']
    ws_quote['E8'] = data['address']
    ws_quote['I9'] = data['quote_number']
    
    # 填入品項（從第12列起）
    for i, item in enumerate(data['items']):
        row = 12 + i
        ws_quote[f'A{row}'] = f'{(i+1):02d}'   # 項次
        ws_quote[f'B{row}'] = item['description']
        ws_quote[f'G{row}'] = item['unit_price']
        ws_quote[f'H{row}'] = item['quantity']
        ws_quote[f'I{row}'] = f'=G{row}*H{row}'
    
    ws_quote['B42'] = data.get('payment_terms', '月結90天')
    
    # ── 訂單請購單 Sheet ──
    ws_order = wb['訂單請購單']
    ws_order['F7'] = data['quote_number']
    ws_order['A7'] = f"申請日期：{datetime.date.today().strftime('%Y 年 %m 月 %d 日')}"
    ws_order['H31'] = data['customer_name']
    
    for i, item in enumerate(data['items']):
        row = 11 + i
        ws_order[f'A{row}'] = item['part_number']
        ws_order[f'B{row}'] = item['description']
        ws_order[f'D{row}'] = item['cost']       # 代理商成本
        ws_order[f'E{row}'] = item['quantity']
        ws_order[f'G{row}'] = item['unit_price']  # 客戶售價
        ws_order[f'I{row}'] = item.get('supplier', '')
    
    wb.save(output_path)
```

### Step 5：輸出檔案命名與儲存

**檔名格式**：`YYYYMMDD_客戶名稱_報價單`
- Excel：`20260506_Sample_Co_報價單.xlsx`
- PDF：`20260506_Sample_Co_報價單.pdf`

**儲存路徑**：
固定存至 Skill 1 建立的 `3.最終報價單/` 資料夾：

```python
import os, re

base = '<obsidian-vault-path>/7.專案與案例/7.1.客戶'
customer_name = '＜客戶名稱＞'  # 由使用者提供或從比價結果取得
entries = [d for d in os.listdir(base) if os.path.isdir(os.path.join(base, d))]
match = next((d for d in entries if customer_name in d), None)

if match:
    output_dir = os.path.join(base, match, '3.最終報價單')
else:
    # 若客戶資料夾不存在，提示先執行 Skill 1
    raise FileNotFoundError(f'找不到客戶「{customer_name}」的資料夾，請先執行詢價信生成（quote-inquiry）')

xlsx_path = os.path.join(output_dir, f'{date_str}_{customer_name}_報價單.xlsx')
pdf_path  = os.path.join(output_dir, f'{date_str}_{customer_name}_報價單.pdf')
```

若 `3.最終報價單/` 不存在（跳過 Skill 1 直接出單的情況），自動建立後繼續。

**PDF 轉換**：
```python
import subprocess
# 使用 LibreOffice 轉換（若環境有安裝）
subprocess.run(['libreoffice', '--headless', '--convert-to', 'pdf', xlsx_path])
```
若 LibreOffice 無法使用，改用 openpyxl-pdf 或告知使用者手動另存 PDF。

---

## 輸出

產出完成後顯示：
```
✅ 報價單已產出完成

📄 報價單號碼：[號碼]
👤 客戶：[客戶名稱]
💰 含稅總金額：NT$ [金額]

檔案位置：
- Excel：客戶名稱/3.最終報價單/YYYYMMDD_客戶名稱_報價單.xlsx
- PDF  ：客戶名稱/3.最終報價單/YYYYMMDD_客戶名稱_報價單.pdf
```

⚠️ 輸出後提醒：
> 請人工核對品項規格與單價無誤後，再將報價單發送給客戶。

---

## 注意事項

- 稅率固定 5%（公式寫死於範本，不另行計算）
- 售價由使用者確認，不自動帶入代理商成本
- 若品項超過 26 行（A12:A37），提醒使用者範本列數不足，需手動新增列
- 繁體中文檔名：使用 Heredoc 寫入避免亂碼
- 所有金額格式：無條件整數，不加小數點
