# winny-tpi/code-review: Agent 驅動的程式碼審查與知識自動化模板

## 歡迎來到 `code-review` 模板倉庫！

這個公開的 GitHub 倉庫 (`winny-tpi/code-review`) 是一個創新的 Agent 驅動型自動化流程模板，旨在協助開發團隊自動化開發前置知識的產出與管理。它不僅是一個可運作的示範專案，更是一套詳細的機制說明文件，指導任何兼容的 AI Agent 如何協同工作，提升開發效率與品質。

## 核心機制：Issue 驅動 → Agent 執行 → PR 回覆 → 人類審核

本倉庫的核心理念是將程式碼審查與知識產出的流程，轉化為一套由 Agent Skill 定義的自動化工作流：

1.  **Issue 驅動**：開發者在專案倉庫中提交開發需求 Issue。
2.  **Agent 執行**：專案 Agent 讀取 Issue 內容，並根據預設的 Agent Skill（例如 `issue-analyzer.agent.md`）呼叫大型語言模型 (LLM) 進行分析，產出結構化的開發前置知識文件。
3.  **PR 回覆**：Agent 將生成的知識文件以 Pull Request (PR) 的形式提交到專案倉庫，或直接在 Issue 中留言回覆。
4.  **人類審核**：人類開發者審核 Agent 產出的知識文件，確保其準確性與實用性，並據此進行實際開發。

這套機制類似於 GitHub Copilot Cloud Agent 的程式碼審查概念，但其應用範圍更廣，不限於程式碼審查，而是專注於開發前置知識的自動化生成。它為 AI Agent 提供了清晰的執行指南，也賦予了各專案團隊在自主進化的同時，能夠參考並學習最佳實踐的能力。

## 如何參考此模板建立您的專案

當您需要為新的開發專案建立一套自動化的知識管理流程時，可以指示您的 AI Agent 參考本 `code-review` 模板倉庫。您的 Agent 將會：

1.  **讀取模板 Skill**：Agent 會解析本倉庫 `.github/agents/` 目錄下的所有 Agent Skill 定義，理解其運作邏輯。
2.  **建立新專案倉庫**：Agent 將為您的新專案建立一個全新的私有 GitHub 倉庫（例如 `winny-tpi/您的專案名稱`），而不是簡單的 fork 或 clone。
3.  **複製與適配**：Agent 會根據本模板的規範，將必要的 Agent Skill 檔案、Issue 模板、排程配置、核心腳本和目錄結構複製到您的新專案倉庫中，並進行客製化調整。
4.  **持續跟版**：您的專案 Agent 會定期檢查本 `code-review` 模板倉庫的更新，並向您提供智慧化的跟版建議，確保您的專案能持續學習並採用最新的最佳實踐。

## 目錄結構說明

本倉庫的目錄結構設計，旨在清晰地組織 Agent Skill、說明文件和相關腳本：

```
winny-tpi/code-review/
├── .github/                      # GitHub 相關配置
│   ├── agents/                   # Agent Skill 定義核心目錄
│   │   ├── AGENTS.md             # Agent 入口文件，說明整套機制
│   │   ├── issue-analyzer.agent.md # Issue 分析 Agent Skill
│   │   ├── project-creator.agent.md # 專案建立 Agent Skill
│   │   ├── version-tracker.agent.md # 模板版本追蹤與跟版建議 Agent Skill
│   │   └── common-utils.agent.md # 共用工具或輔助 Skill
│   ├── ISSUE_TEMPLATE/           # 標準 Issue 模板 (由 issue-analyzer.agent.md 引用)
│   │   └── feature_request.md
│   └── workflows/                # GitHub Actions 範例 (由 issue-analyzer.agent.md 引用)
│       └── issue_analysis_cron.yml
├── docs/                         # 說明文件與規範 (供人類閱讀，Agent 亦可參考)
│   ├── architecture/             # 系統架構總覽
│   │   └── system_overview.md
│   ├── standards/                # 人類可讀的規範文件
│   │   └── llm_output_format_guide.md # LLM 知識文件格式指南
│   └── knowledge/                # 範例知識文件
│       └── demo_issue_1_knowledge.md
├── scripts/                      # 核心自動化腳本 (由 Agent Skill 引用)
│   └── issue_analysis_core.py    # 實際執行 Issue 分析的 Python 腳本
├── CHANGELOG.md                  # 模板版本更新紀錄
├── README.md                     # 倉庫總覽與快速入門 (您正在閱讀的文件)
└── VERSION                       # 當前模板版本號 (例如：1.0.0)
```

我們鼓勵您探索本倉庫的內容，並利用其強大的 Agent 驅動能力，提升您的開發工作流程！
