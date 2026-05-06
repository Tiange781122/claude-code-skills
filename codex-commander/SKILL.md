---
name: codex-commander
description: >
  作為 Codex CLI 的指揮官，接收用戶的自然語言指令（文字或圖片+文字），
  將其優化成結構清晰、涵蓋完整的 AI 可讀指令，傳送給 Codex CLI 執行並回傳輸出。
  當用戶說 /codex-commander、/codex、「用 codex」、「交給 codex」、「codex 幫我」時觸發。
  特別適合純程式碼任務、需要沙箱安全執行、程式碼審查、或 OpenAI 模型強項任務。
---

# Codex Commander

你是 Codex CLI 的指揮官。你的工作是：**接收用戶意圖 → 優化成精確指令 → 執行 Codex CLI → 回傳結果**。

目標：用你的嚴謹度補足用戶輸入的模糊性，並善用 Codex 擅長程式碼生成與沙箱執行的特性。

---

## 執行流程

### Step 1：解析輸入

**偵測 `@skill-name` 引用（含連字號的 `@` 標記）：**
若輸入中含有如 `@obsidian-note-writer` 的連字號名稱，記錄所有 skill 名稱，進入 Step 1.5。

**若輸入包含圖片路徑：**
Codex 原生支援圖片輸入，**不需要 OCR**，直接將圖片路徑記錄下來，在 Step 4 用 `-i <path>` 傳入。
若用戶提供的是截圖或架構圖，補充說明：「請分析這張圖片並根據其內容完成以下任務」。

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

在心中評估用戶指令的完整度，特別考量 Codex 的強項：

- **任務目標**：用戶想達成什麼？
- **程式碼範疇**：語言？框架？目標檔案？輸出格式？
- **是否需要執行**：只生成程式碼，還是需要實際執行驗證？
- **隱含的要求**：型別標注？測試？錯誤處理？
- **驗收條件**：什麼叫「做完」？

### Step 3：建構優化指令

根據任務複雜度選擇格式：

**簡單任務（單一明確動作）→ 增強版純文字：**
```
原始：「幫我重構這段程式碼」
優化：「重構 main.py 中的 process_data() 函式，消除重複邏輯，保持功能不變，加入型別標注，並附上前後對照說明」
```

**複雜任務（多步驟 / 多條件）→ YAML 結構化：**
```yaml
task: <一句話描述任務>
context:
  working_directory: <若已知>
  language: <程式語言>
  framework: <框架>
  target_files: <目標檔案，若已知>
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

### Step 4：執行 Codex CLI

**直接執行，不需要向用戶確認優化後的指令。**

**純文字任務（無圖片）：**
```bash
codex exec "$(cat <<'PROMPT'
<你的優化指令>
PROMPT
)" 2>&1 | sed 's/\x1b\[[0-9;]*[mGKJH]//g; s/\x1b\[?[0-9]*[hl]//g'
```

**含圖片任務：**
```bash
codex exec -i <圖片路徑> "$(cat <<'PROMPT'
<你的優化指令>
PROMPT
)" 2>&1 | sed 's/\x1b\[[0-9;]*[mGKJH]//g; s/\x1b\[?[0-9]*[hl]//g'
```

使用 Bash 工具執行，完整捕捉輸出。
- `exec`：非互動式執行子命令
- 預設 `approval: never`，Codex 自動執行所有工具操作，無需額外 flag
- `sed`：清除 ANSI 色碼，保留所有文字內容（含 session header 與 tokens used）

### Step 5：回傳結果

直接將 Codex CLI 的完整輸出（含 session header、tokens used）回傳給用戶，不加摘要或解釋。

---

## 優化原則

1. **補足不補改**：不改變用戶意圖，只補充遺漏的 context
2. **程式碼要精確**：明確指定語言版本、目標檔案、輸出位置
3. **越具體越好**：模糊的「幫我改好一點」→ 明確的「消除重複、加型別標注、通過現有測試」
4. **指定邊界**：明確說明「只改這個函式」避免 Codex 動到不該動的地方
5. **使用繁體中文**：除非用戶的任務涉及英文文件，指令本身用繁體中文

---

## 範例

**用戶輸入：** `/codex-commander 幫我審查 auth.py 這支檔案的安全性`

**優化後傳給 Codex CLI：**
```yaml
task: 對 auth.py 進行安全性程式碼審查
context:
  target_files: auth.py
requirements:
  - 讀取 auth.py 的完整內容
  - 檢查常見安全漏洞：SQL injection、硬編碼密碼/金鑰、不安全的雜湊演算法、未驗證輸入
  - 每個問題標示嚴重程度（高/中/低）
  - 提供具體修正建議，附上修正後的程式碼片段
constraints:
  - 只審查 auth.py，不修改檔案內容
  - 不執行任何程式碼
acceptance_criteria:
  - 輸出為繁體中文
  - 若無安全問題也明確說明
```

---

## 注意事項

- Codex 輸出包含 session header（model、workdir、session id、tokens used），這是正常行為
- 若 Codex CLI 回傳錯誤（exit code ≠ 0），直接顯示錯誤訊息，不要重試
- Codex 預設在 workspace-write 沙箱中執行，操作限於當前目錄、`/tmp` 與 `~/.codex/memories`
