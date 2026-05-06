# Skill：報價解析、QA 與比價（quote-analysis）

讀取各代理商回覆的報價資料，自動進行 QA 檢查，產出經使用者確認的比價表。

---

## 何時使用

使用者說出以下任何一種需求時觸發：
- 「代理商回來了，幫我比價」
- 「幫我解析這些報價 Email」
- 「整理一下各家的報價」
- 提供 .eml 檔案路徑或貼上代理商回覆文字

---

## 輸入

| 項目 | 來源 |
|------|------|
| 代理商報價 | 客戶資料夾的 `2.代理商報價/` 子資料夾，或貼上純文字 |
| 客戶原始需求 | 本次詢價的品項清單（由使用者提供或從 Skill 1 的結果取得） |

---

## 執行步驟

### Step 0：定位客戶作業資料夾

詢問使用者客戶名稱（若尚未提供），然後掃描找出對應資料夾：

```python
import os, re

base = '<obsidian-vault-path>/7.專案與案例/7.1.客戶'
customer_name = '＜客戶名稱＞'  # 由使用者提供
entries = [d for d in os.listdir(base) if os.path.isdir(os.path.join(base, d))]
match = next((d for d in entries if customer_name in d), None)

if match:
    customer_path = os.path.join(base, match)
    quote_inbox = os.path.join(customer_path, '2.代理商報價')
else:
    # 找不到時提示使用者先執行 Skill 1 建立資料夾
    raise FileNotFoundError(f'找不到客戶「{customer_name}」的資料夾，請先執行詢價信生成（quote-inquiry）')
```

確認 `2.代理商報價/` 資料夾存在後，列出其中的所有檔案：

```python
files = os.listdir(quote_inbox)
# 告知使用者找到哪些檔案，例如：
# 找到 3 個報價檔案：代理商A報價.eml、代理商B報價.xlsx、代理商C報價.eml
```

若資料夾是空的，提示使用者：
> `2.代理商報價/` 資料夾目前沒有檔案，請將代理商回覆的 EML 或 Excel 放入後再繼續。

---

### Step 1：解析代理商報價

#### 若輸入為 .eml 檔案

使用 Python 解碼：

```python
import email
import base64
from email.header import decode_header

def parse_eml(filepath):
    with open(filepath, 'rb') as f:
        msg = email.message_from_bytes(f.read())
    
    # 解碼主旨
    subject_parts = decode_header(msg['Subject'])
    subject = ''.join(
        part.decode(enc or 'utf-8') if isinstance(part, bytes) else part
        for part, enc in subject_parts
    )
    
    # 提取寄件人
    sender = msg['From']
    
    # 提取純文字內容
    body = ''
    for part in msg.walk():
        charset = part.get_content_charset() or 'utf-8'
        if part.get_content_type() == 'text/plain':
            payload = part.get_payload(decode=True)
            if payload:
                body = payload.decode(charset, errors='replace')
                break
        elif part.get_content_type() == 'text/html' and not body:
            payload = part.get_payload(decode=True)
            if payload:
                from html.parser import HTMLParser
                class TextExtractor(HTMLParser):
                    def __init__(self):
                        super().__init__()
                        self.text = []
                    def handle_data(self, data):
                        self.text.append(data)
                    def get_text(self):
                        return ' '.join(self.text)
                p = TextExtractor()
                p.feed(payload.decode(charset, errors='replace'))
                body = p.get_text()
    
    return {'subject': subject, 'sender': sender, 'body': body}
```

#### 若輸入為貼上的純文字
直接進行品項解析，不需要解碼步驟。

### Step 2：AI 解析報價品項

從解碼後的內文中，提取每個報價品項：
- 型號 / 料號（P/N）
- 品名描述
- 單價（台幣，若為美金需標記）
- 數量
- 保固條件（若有提及）

若代理商回覆的是附件（Excel），提示使用者：
> 此封 Email 含有附件，請將附件內容複製後貼入，我將繼續解析。

---

### Step 3：QA 自動檢查

逐一比對「代理商報價品項」與「客戶原始需求」：

#### QA 規則清單

**規格比對類**
- [ ] 容量是否符合（如：要 2.4TB，報 1.2TB → ⚠️）
- [ ] 轉速/介面是否符合（如：要 SAS 10K，報 SATA → ⚠️）
- [ ] 型號世代是否符合（如：要 G10，料號顯示 G9 → ⚠️）
- [ ] Form factor 是否符合（SFF vs LFF → ⚠️）

**完整性類**
- [ ] 客戶需求的每個品項是否都有報價（漏報 → ❌）
- [ ] 代理商是否多報了不相關品項（多報 → 標示，不計入比價）

**價格異常類（需有 ≥2 家報價才能比較）**
- [ ] 某家報價比其他家高出 30% 以上 → ⚠️
- [ ] 價格為 0 或空白 → ❌

**品名對應不確定類**
- [ ] AI 信心低於 80%（型號部分字元不同，無法確認） → ⚠️ 需人工確認

---

### Step 4：輸出 QA 報告

格式如下：

```
## 🔍 QA 檢查報告

### ✅ 正常品項
| 品項 | 代理商 | 報價 | 對應需求 |
|------|--------|------|---------|
| ...  | ...    | ...  | ...     |

### ⚠️ 有疑義品項（請確認是否接受）
| 品項 | 代理商 | 疑義原因 | 報價 |
|------|--------|---------|------|
| ...  | ...    | 型號為 G9，需求為 G10 | ... |

請對每個疑義品項回覆「接受」或「排除」。

### ❌ 明確異常品項（自動排除）
| 品項 | 代理商 | 異常原因 |
|------|--------|---------|
| ...  | ...    | 漏報：未提供此品項報價 |
```

等待使用者逐條回應疑義品項後繼續。

---

### Step 5：產出最終比價表

僅包含「正常品項」＋「使用者選擇接受的疑義品項」：

```
## 📊 比價彙整表

| 品項 | 規格 | 數量 | [代理商A] | [代理商B] | [代理商C] | 最低價 | 建議採用 |
|------|------|------|-----------|-----------|-----------|--------|---------|
| ...  | ...  | N    | $XX,XXX   | $XX,XXX   | 未報價    | $XX,XXX | 代理商A |

> 幣別：新台幣（未稅）
> 最低價自動標記，若有並列最低則全部標記
```

---

## 輸出格式

- Markdown 表格呈現於對話中
- 同時詢問使用者是否儲存比價表為 Excel，儲存至 `客戶名稱/2.代理商報價/YYYYMMDD_比價表.xlsx`
- 比價結果自動帶入 Skill 3 使用

---

## 注意事項

- 幣別預設台幣；若代理商以美金報價，在該欄標記「USD」並提醒使用者換算
- 若只有一家代理商報價，仍執行規格 QA，但不進行價格異常比較
- 疑義品項若使用者未明確回應，預設為「排除」，並於比價表備注「待確認」
- 解碼 EML 若失敗（charset 錯誤），顯示錯誤訊息並請使用者直接貼上文字內容
