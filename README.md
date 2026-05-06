# Claude Code Skills Collection

一套高效、可復用的 Claude AI 技能 (Skills)，為知識管理、技術學習、業務流程提供專業化解決方案。

## 📦 Skills 列表

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **tech-evolution-storyteller** | 將任何技術概念轉化為時間線故事。接收技術主題後，自動搜索完整信息，從歷史發展 → 現代應用 → 最新技術，梳理 6+ 核心要點。 | `/tech-evolution-storyteller` |
| **tech-personification-storytelling** | 用擬人化故事講解技術（如將 TCP 比作郵件系統）。融合物理原理、歷史演進、生活比喻，深化理解。 | `/tech-personification-storytelling` |
| **knowledge-base-organizer** | 知識庫組織與優化。使用「命名即知識體」原則，檢查資料夾結構、去重、合併相似卡片、優化知識架構。 | `/knowledge-base-organizer` |
| **codex-commander** | 快速查詢和導航代碼庫中的特定模組、函數、類、API。支持 glob 模式搜索、引用查詢、代碼流追蹤。 | `/codex-commander` |
| **gemini-commander** | 在 Claude Code 中呼叫 Google Gemini CLI 進行網路搜索、圖片分析、AI 功能增強。 | `/gemini-commander` |
| **kiro-commander** | 快速代碼重構助手。識別代碼重複、優化邏輯、提升可讀性和效能。 | `/kiro-commander` |
| **exam-answer-helper** | 考試答題助手。快速生成常見考試題型的高質量答案（概念解釋、計算題、論述題等）。 | `/exam-answer-helper` |
| **graphify** | 知識圖譜生成。將任何輸入轉化為結構化的知識圖，支持可視化和關係查詢。 | `/graphify` |
| **quote-estimator** | 業務報價估算。根據客戶需求和代理商信息，快速生成成本估算和報價單。 | `/quote-estimator` |

## 🚀 快速開始

### 方式 1：複製到 Claude Code Skills 目錄

```bash
git clone https://github.com/your-username/claude-code-skills.git ~/claude-skills-download
cp -r ~/claude-skills-download/* ~/.claude/skills/
```

然後重啟 Claude Code 以加載新 skills。

### 方式 2：單個 Skill 安裝

如果只需要特定 skill，可以複製單個資料夾：

```bash
cp -r ./tech-evolution-storyteller ~/.claude/skills/
```

## 📁 目錄結構

每個 skill 遵循標準結構：

```
skill-name/
├── SKILL.md                   # 必需：Skill 定義和工作流說明
├── evals/                     # 可選：測試用例和評估配置
│   └── evals.json
├── scripts/                   # 可選：可執行腳本（如 Python 工具）
├── references/                # 可選：參考文檔
└── assets/                    # 可選：資源文件（模板、圖標等）
```

## 💡 使用示例

### 使用 tech-evolution-storyteller 講解 HTTPS

```
用 tech-evolution-storyteller 講解「HTTPS 加密如何保護數據」
```

Skill 會自動：
1. 搜索 HTTPS 歷史（SSL 1.0 → TLS 1.3）
2. 整理核心概念（公鑰加密、TLS 握手、前向保密）
3. 生成故事化 Markdown（含時間線、圖表、比喻）

### 使用 knowledge-base-organizer 優化知識庫

```
用 knowledge-base-organizer 檢查我的知識庫結構，找出重複或需要合併的卡片
```

## 🔧 開發和貢獻

### 添加新 Skill

1. 在本倉庫新建資料夾 `my-new-skill/`
2. 創建 `SKILL.md` 文件，遵循標準結構
3. （可選）添加 `evals/evals.json` 用於測試
4. 提交 PR

### 運行 Skill 評估

如果 skill 包含測試用例，可以驗證其功能：

```bash
cd skill-name
cat evals/evals.json  # 查看測試用例
```

## 📝 Skill 編寫規範

每個 `SKILL.md` 必須包含：

```yaml
---
name: skill-name
description: 簡洁描述這個 skill 的用途和觸發方式
compatibility: 所需的工具或依賴（可選）
---

## 核心工作流

[詳細的步驟說明...]

## 使用示例

[具體使用場景...]
```

詳見各 skill 的 `SKILL.md` 文件。

## 🌐 語言支持

- **繁體中文 (zh-TW)** ：主要語言，所有 skills 都支持
- **英文 (en)** ：部分 skills 支持

## 📜 許可證

MIT License - 詳見 [LICENSE](LICENSE) 文件

## 👤 作者

Bart - 知識庫和技術教學專家

---

**最後更新**：2026-05-06  
**版本**：1.0.0
