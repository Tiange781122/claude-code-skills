# Skill：詢價信生成（quote-inquiry）

依照客戶採購需求，建立作業資料夾、查詢代理商對照表，自動產出給各代理商的詢價 Email 草稿。

---

## 何時使用

使用者說出以下任何一種需求時觸發：
- 「幫我發詢價信」
- 「客戶要買 XXX，幫我問代理商」
- 「產出詢價草稿」
- 貼上客戶需求清單並要求詢價

---

## 輸入

| 項目 | 來源 |
|------|------|
| 客戶名稱 | 使用者提供，若未提供則詢問 |
| 客戶需求清單 | 使用者直接輸入或貼上文字 |
| 代理商對照表 | Excel 檔，預設路徑：`0.inbox/業務報價程序/0.代理商販售與聯絡清單/0.代理商販售與聯絡清單.xlsx` |

---

## 執行步驟

### Step 0：建立客戶作業資料夾

收到客戶需求時，**第一步先建立資料夾**，後續三個 Skill 都依賴此結構。

#### 資料夾結構

```
7.專案與案例/7.1.客戶/客戶名稱/
├── 1.詢價草稿/          ← Skill 1 將詢價 Email 草稿存為 .md 檔
├── 2.代理商報價/        ← 使用者手動將代理商回覆（.eml / .xlsx）放入此處
└── 3.最終報價單/        ← Skill 3 將報價單 .xlsx 與 .pdf 輸出至此
```

#### 建立邏輯

```python
import os, re, datetime

base = '<obsidian-vault>/7.專案與案例/7.1.客戶'
entries = [d for d in os.listdir(base) if os.path.isdir(os.path.join(base, d))]
customer_name = '＜客戶名稱＞'  # 由使用者提供

# 1. 檢查是否已有此客戶資料夾
existing = [d for d in entries if customer_name in d]
if existing:
    customer_path = os.path.join(base, existing[0])
    # → 告知使用者「已找到既有資料夾，繼續使用」
else:
    # 2. 建立資料夾與三個子資料夾
    folder_name = customer_name
    customer_path = os.path.join(base, folder_name)
    for sub in ['1.詢價草稿', '2.代理商報價', '3.最終報價單']:
        os.makedirs(os.path.join(customer_path, sub), exist_ok=True)
```

#### 建立完成後告知使用者

```
📁 已建立客戶作業資料夾：客戶名稱/
   ├── 1.詢價草稿/       ← 詢價草稿將自動存入
   ├── 2.代理商報價/     ← 請將代理商回覆（EML 或 Excel）放入此資料夾後告知我
   └── 3.最終報價單/     ← 報價單產出後將自動存入
```

---

### Step 1：解析客戶需求

從使用者輸入中提取每個採購品項：
- 品牌（如 HPE、Dell）
- 型號或規格描述
- 數量
- 備注（如有特殊需求）

若需求模糊（品牌不明確），在繼續前詢問使用者確認。

---

### Step 2：讀取代理商對照表

```python
import openpyxl
wb = openpyxl.load_workbook('代理商對照表.xlsx')
ws = wb.active
# 提取品牌 → 代理商 → 聯絡人 → Email 的對應關係
```

---

### Step 3：品牌 → 代理商對應

- 同一品牌可能有多家代理商（全部列出，由使用者選擇，或預設選全部詢價）
- 若某品牌在對照表中找不到對應代理商，告知使用者並跳過

---

### Step 4：依代理商分組

同一代理商的多個品項合併成一封信，不要拆成多封。

---

### Step 5：產出詢價 Email 草稿

每封信格式如下：

```
收件人：[聯絡人姓名] <[Email]>
主旨：詢價 - [品牌/產品線] [今日日期]

[聯絡人姓名] 您好，

敝司目前有客戶需要採購下列產品，煩請提供報價，謝謝。

【詢價品項】
1. [型號/規格]  數量：[N] 套/顆/組
2. [型號/規格]  數量：[N] 套/顆/組
...

【回覆期限】
煩請於 [今日 + 3 個工作天] 前回覆報價，謝謝。

【備注】
[若有特殊需求則加入，否則省略此段]

如有任何問題歡迎聯繫，感謝您的協助。

[業務姓名]
[聯絡電話]
[您的公司名稱]
```

---

### Step 6：儲存草稿至資料夾

將所有詢價草稿儲存為 Markdown 檔案至 `1.詢價草稿/`：

**檔名格式**：`YYYYMMDD_詢價_[代理商名稱].md`

範例：`20260428_詢價_代理商 A.md`

```python
draft_dir = os.path.join(customer_path, '1.詢價草稿')
filename = f"{datetime.date.today().strftime('%Y%m%d')}_詢價_{dealer_name}.md"
filepath = os.path.join(draft_dir, filename)
# 使用 heredoc 或 open() 寫入草稿內容
with open(filepath, 'w', encoding='utf-8') as f:
    f.write(draft_content)
```

---

## 輸出格式

- 以 Markdown 格式呈現在對話中，每封信用 `---` 分隔
- 標題格式：`## 📧 給 [代理商名稱]（[品牌]）`
- 草稿同步儲存至 `1.詢價草稿/`（讓使用者可在 Obsidian 中開啟複製）
- ⚠️ 信件**不自動寄出**，供使用者人工審核後手動複製貼上發送

---

## 完成後的引導提示

產出草稿並儲存後，主動提示：

> ✅ 詢價草稿已儲存至 `客戶名稱/1.詢價草稿/`
>
> 📌 **下一步**：待代理商回覆後，請將回覆的 EML 或 Excel 檔放入：
> `客戶名稱/2.代理商報價/`
>
> 放好後告訴我「代理商報價收到了」，我會進行比價分析。

---

## 注意事項

- 語氣：正式、有禮，繁體中文
- 今日日期使用 `datetime.date.today()` 取得
- 回覆期限：預設今日起算 3 個工作天（不含週六日）
- 若代理商名稱包含公司全名，稱謂只用聯絡人姓名
- 繁體中文檔案內容使用 heredoc 或 `open(..., encoding='utf-8')` 寫入
