# Agent 驅動的知識自動化流程總覽

歡迎來到 `winny-tpi/code-review` 倉庫的 Agent 驅動知識自動化流程。本倉庫定義了一套標準化的 Agent Skill，旨在協助開發團隊自動化開發前置知識的產出與管理。

## 核心理念

本流程的核心是透過 Agent 讀取並執行預定義的 Skill，實現：

1.  **智慧化 Issue 分析**：自動將 GitHub Issue 轉化為結構化的開發前置知識文件。
2.  **彈性專案建立**：Agent 根據模板 Skill 為新專案建立客製化的知識管理倉庫。
3.  **自主跟版進化**：各專案 Agent 定期檢查模板更新，並提供智慧化的跟版建議，確保專案能自主進化。

## 可用 Agent Skill

以下是本倉庫定義的主要 Agent Skill 及其職責：

*   **`issue-analyzer.agent.md`**：負責 GitHub Issue 的分析、LLM 呼叫、知識文件生成與 Issue 狀態更新。
*   **`project-creator.agent.md`**：負責根據用戶需求，建立新的專案知識倉庫並進行初始化配置。
*   **`version-tracker.agent.md`**：負責監控模板倉庫的更新，比對差異，並生成跟版建議。
*   **`common-utils.agent.md`**：提供一些共用的輔助功能或規範，供其他 Skill 引用。

## 如何使用

任何兼容 GitHub Agent 規範的 AI Agent，在參考本倉庫後，將能夠理解並執行上述 Skill。例如，當需要建立新專案時，可指示 Agent 參考 `project-creator.agent.md`。

## 版本與進化

本模板倉庫會持續進化。請參考 `CHANGELOG.md` 了解版本更新歷史，並指示 `version-tracker.agent.md` 監控最新變化。
