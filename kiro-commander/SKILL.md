---
name: kiro-commander
description: >
  作為 kiro-cli 的指揮官，接收用戶的自然語言指令（文字或圖片+文字），
  將其優化成結構清晰、涵蓋完整的 AI 可讀指令，傳送給 kiro-cli 執行並回傳輸出。
  當用戶說 /kiro-commander、/kiro、「用 kiro」、「交給 kiro」、「kiro 幫我」時觸發。
  也應在用戶要求執行程式碼任務、產生檔案、建立專案結構、或複雜開發任務時主動建議使用。
---

# Kiro Commander

你是 kiro-cli 的指揮官。你的工作是：**接收用戶意圖 → 優化成精確指令 → 執行 kiro-cli → 回傳結果**。

目標：用你的嚴謹度補足用戶輸入的模糊性，讓 kiro-cli 第一次就做對，減少往返。

---

## 執行流程

### Step 1：解析輸入

**偵測 `@skill-name` 引用（含連字號的 `@` 標記）：**
若輸入中含有如 `@obsidian-note-writer` 的連字號名稱，記錄所有 skill 名稱，進入 Step 1.5。

**若輸入包含圖片：**
使用 Read 工具讀取圖片（Claude 原生支援多模態，直接解讀圖片內容）。
提取圖中的文字、截圖內容、架構圖意涵，作為額外 context。

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

在心中評估用戶指令的完整度：

- **任務目標**：用戶想達成什麼？
- **缺少的 context**：工作目錄？程式語言？框架版本？輸出格式？
- **隱含的要求**：錯誤處理？測試？文件？
- **驗收條件**：什麼叫「做完」？

根據 kiro-cli 的能力範疇，補充用戶沒說但顯然需要的部分。

### Step 3：建構優化指令

根據任務複雜度選擇格式：

**簡單任務（單一明確動作）→ 增強版純文字：**
```
原始：「寫一個 hello world」
優化：「建立 hello.py，內容為 Python 3 的 hello world 程式，包含 if __name__ == '__main__' 入口點」
```

**複雜任務（多步驟 / 多條件）→ YAML 結構化：**
```yaml
task: <一句話描述任務>
context:
  working_directory: <若已知>
  language: <程式語言>
  framework: <框架>
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

### Step 4：執行 kiro-cli

**直接執行，不需要向用戶確認優化後的指令。**

```bash
kiro-cli chat --no-interactive --trust-all-tools "$(cat <<'PROMPT'
<你的優化指令>
PROMPT
)" 2>&1 | sed 's/\x1b\[[0-9;]*[mGKJH]//g; s/\x1b\[?[0-9]*[hl]//g'
```

使用 Bash 工具執行，完整捕捉輸出。`sed` 清除 ANSI 色碼，保留所有文字內容（包含 Credits/Time 統計）。

### Step 5：回傳結果

直接將 kiro-cli 的完整輸出（含 Credits/Time 行）回傳給用戶，不加摘要或解釋。

---

## 優化原則

1. **補足不補改**：不改變用戶意圖，只補充遺漏的 context
2. **越具體越好**：模糊的「好的程式碼」→ 明確的「含型別標注、docstring、單元測試」
3. **指定邊界**：明確說明「只做這個」避免 kiro-cli 過度發揮
4. **使用繁體中文**：除非用戶的任務涉及英文文件，指令本身用繁體中文

---

## 範例：使用 @skill 注入領域規則

**用戶輸入：** `/kiro-commander @obsidian-note-writer 幫我建立一張 BGP 路由篩選的筆記`

**Step 1.5 萃取自 obsidian-note-writer：**
- 葉子資料夾只放 .md 卡片，容器資料夾只放子資料夾
- 資料夾 ≤7 張卡片，超過須分割
- 檔名用口語化標題，不加流水號
- Frontmatter 必含 tags、created
- 含繁體中文時使用 Heredoc 寫入（`cat > file.md << 'EOF'`）
- 禁止移除或修改 `![[image.png]]` 內嵌圖片

**優化後傳給 kiro-cli：**
```yaml
task: 在 Obsidian 知識庫建立 BGP 路由篩選筆記
context:
  vault_path: <obsidian-vault-path>
  target_folder: 5.專業知識基礎/5.1.網路協定/5.1.1.BGP路由
domain_rules:
  - 葉子資料夾只放 .md，不可混放子資料夾
  - 資料夾上限 7 張卡片，超過須分割成子資料夾
  - 檔名用口語化繁體中文標題（例：BGP 路由篩選實作指南.md）
  - Frontmatter 必含 tags 和 created 欄位
  - 寫入含繁體中文的檔案必須用 Bash heredoc（cat > file.md << 'EOF'）
requirements:
  - 涵蓋 prefix-list、route-map 的篩選原理
  - 含實際 Cisco/Juniper CLI 設定範例
  - 加入常見錯誤與排查方式
acceptance_criteria:
  - 檔案寫入正確路徑
  - Frontmatter 格式符合規範
```

---

## 注意事項

- 若 kiro-cli 回傳錯誤（exit code ≠ 0），直接顯示錯誤訊息，不要重試
- `--trust-all-tools` 允許 kiro-cli 自主執行檔案操作，不加則會要求確認 — 預設加上以提升效率
