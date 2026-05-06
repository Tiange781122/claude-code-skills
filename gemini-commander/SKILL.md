---
name: gemini-commander
description: >
  作為 Gemini CLI 的指揮官，接收用戶的自然語言指令（文字或圖片+文字），
  將其優化成結構清晰、涵蓋完整的 AI 可讀指令，傳送給 Gemini CLI 執行並回傳輸出。
  當用戶說 /gemini-commander、/gemini、「用 gemini」、「交給 gemini」、「gemini 幫我」時觸發。
  特別適合需要網路搜尋、即時資料查詢、研究型任務、或多模態分析的場景。
---

# Gemini Commander

你是 Gemini CLI 的指揮官。你的工作是：**接收用戶意圖 → 優化成精確指令 → 執行 Gemini CLI → 回傳結果**。

目標：用你的嚴謹度補足用戶輸入的模糊性，並善用 Gemini 擅長搜尋與研究的特性，讓任務第一次就做對。

---

## 執行流程

### Step 1：解析輸入

**偵測 `@skill-name` 引用（含連字號的 `@` 標記）：**
若輸入中含有如 `@obsidian-note-writer` 的連字號名稱，記錄所有 skill 名稱，進入 Step 1.5。

**若輸入包含圖片：**
使用 Read 工具讀取圖片（Claude 原生支援多模態，直接解讀圖片內容）。
提取圖中的文字、截圖內容、架構圖意涵，作為額外 context 注入 prompt。

**若輸入為純文字且無 `@` 引用：**
直接進入 Step 2。

### Step 1.5：萃取 Skill 領域規則（有 `@skill` 時執行）

用 Read 工具讀取每個被引用的 skill：

```
~/.claude/skills/<skill-name>/SKILL.md
```

從內容中萃取以下類型的資訊（忽略「如何呼叫 skill」的說明，只要「產出必須符合什麼」）：

- **格式規則**：命名慣例、資料夾結構、檔案格式、Frontmatter 欄位
- **強制限制**：「禁止」、「必須」、「不得」類的規則
- **輸出規範**：編碼方式、長度上限、特定語法要求
- **領域知識**：該 skill 操作的系統/環境的特定 context（路徑、版本等）

萃取後整理成簡潔的條列式規則，作為 `domain_rules` 備用。

### Step 2：分析意圖，補足缺口

在心中評估用戶指令的完整度，特別考量 Gemini 的強項：

- **任務目標**：用戶想達成什麼？
- **是否需要搜尋**：涉及即時資料、最新版本、新聞事件 → 明確告知 Gemini 可使用 Google Search
- **缺少的 context**：工作目錄？輸出格式？語言？
- **隱含的要求**：來源引用？摘要長度？語言風格？
- **驗收條件**：什麼叫「做完」？

### Step 3：建構優化指令

根據任務複雜度選擇格式：

**簡單任務（單一明確動作）→ 增強版純文字：**
```
原始：「查一下 BGP 最新的 RFC」
優化：「使用 Google Search 搜尋 BGP 協定最新發布的 RFC 文件，列出文件編號、標題、發布日期，並簡述每份文件的主要更新內容」
```

**複雜任務（多步驟 / 多條件）→ YAML 結構化：**
```yaml
task: <一句話描述任務>
context:
  working_directory: <若已知>
  language: <程式語言或輸出語言>
search_required: <true/false，是否需要 Google Search>
domain_rules:       # 僅在有 @skill 引用時加入此區塊
  - <從 skill 萃取的規則 1>
  - <從 skill 萃取的規則 2>
requirements:
  - <具體需求 1>
  - <具體需求 2>
constraints:
  - <限制條件>
acceptance_criteria:
  - <驗收條件 1>
  - <驗收條件 2>
```

### Step 4：執行 Gemini CLI

**直接執行，不需要向用戶確認優化後的指令。**

```bash
gemini -y -p "$(cat <<'PROMPT'
<你的優化指令>
PROMPT
)" 2>&1 | sed 's/\x1b\[[0-9;]*[mGKJH]//g; s/\x1b\[?[0-9]*[hl]//g'
```

使用 Bash 工具執行，完整捕捉輸出。
- `-y`：yolo 模式，自動同意所有工具操作（等同 kiro 的 `--trust-all-tools`）
- `-p`：非互動式執行，傳入 prompt
- `sed`：清除 ANSI 色碼，保留所有文字內容

### Step 5：回傳結果

直接將 Gemini CLI 的完整輸出回傳給用戶，不加摘要或解釋。

---

## 優化原則

1. **補足不補改**：不改變用戶意圖，只補充遺漏的 context
2. **善用搜尋**：任何涉及即時資料、版本號、新聞的任務，主動在指令中提示 Gemini 使用 Google Search
3. **越具體越好**：模糊的「研究一下」→ 明確的「搜尋並整理成表格，含來源連結」
4. **指定邊界**：明確說明「只做這個」避免 Gemini 過度發揮
5. **使用繁體中文**：除非用戶的任務涉及英文文件，指令本身用繁體中文

---

## 範例

**用戶輸入：** `/gemini-commander 幫我查 FortiGate 7.4 有哪些新功能`

**優化後傳給 Gemini CLI：**
```yaml
task: 研究 FortiGate 7.4 版本的新功能
search_required: true
requirements:
  - 使用 Google Search 搜尋 FortiGate 7.4 release notes 和官方公告
  - 列出主要新功能，按類別整理（安全、網路、管理介面等）
  - 每項功能附上一句簡述
  - 如有重大變更或棄用功能也一併列出
constraints:
  - 只涵蓋 7.4.x 系列，不含 7.2 或 7.6
  - 資料來源限 Fortinet 官方文件或可信技術媒體
acceptance_criteria:
  - 輸出為繁體中文
  - 含至少一個官方來源連結
```

---

## 注意事項

- Gemini CLI 啟動時會印兩行 "YOLO mode is enabled" 警告，這是正常行為
- 若 Gemini CLI 回傳錯誤（exit code ≠ 0），直接顯示錯誤訊息，不要重試
- Gemini 輸出不含 session header，比其他 CLI 更乾淨
