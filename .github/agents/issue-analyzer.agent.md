# Skill: Issue 分析器 (Issue Analyzer)

## 角色 (Role)

作為一個技術分析師和架構師，負責將 GitHub Issue 中描述的開發需求，轉化為結構化的開發前置知識文件。

## 目標 (Goal)

自動分析指定 Issue，利用 LLM 產出符合規範的知識文件，並更新 Issue 狀態。

## 前置條件 (Preconditions)

*   GitHub Issue 存在，且**必須同時帶有 `status: pending-analysis` 和 `approved` 標籤**。
*   `approved` 標籤只能由倉庫擁有者或具有寫入權限的協作者手動添加，這是觸發 Agent 執行任務的安全機制。
*   已配置 `GEMINI_API_KEY` 作為環境變數或 GitHub Secret。
*   可存取 `scripts/issue_analysis_core.py` 腳本。

## 輸入 (Input)

*   `issue_number`: 待分析的 GitHub Issue 編號。
*   `repository_owner`: GitHub 倉庫擁有者 (例如 `winny-tpi`)。
*   `repository_name`: GitHub 倉庫名稱 (例如 `project-alpha`)

## 步驟 (Steps)

1.  **鎖定 Issue 狀態**：
    *   使用 GitHub API 將 Issue `#{{issue_number}}` 的標籤從 `status: pending-analysis` 更新為 `status: analyzing`。
2.  **擷取 Issue 內容**：
    *   使用 GitHub API 讀取 Issue `#{{issue_number}}` 的標題和描述。
    *   **Issue 模板規範**：Agent 應理解 Issue 內容是基於 `feature_request.md` 模板填寫的，並能從中提取「需求背景」、「功能描述」、「相關模組/系統」、「技術考量」、「驗收標準」等關鍵區塊。
3.  **呼叫 LLM 進行分析**：
    *   構建 LLM Prompt：將擷取到的 Issue 內容，結合以下 System Prompt 發送給 Gemini API：
        ```
        你是一個資深的技術分析師和架構師，請根據提供的開發需求 Issue 內容，產出結構化的開發前置知識文件。請嚴格遵循以下 Markdown 格式和章節規範：

        # 知識文件：[功能/模組名稱] - 簡要描述需求

        ## 1. 概述
        *   **目的**：此文件旨在為 GitHub Issue #XXX 提供開發前置知識。
        *   **Issue 連結**：[原始 Issue #XXX](GitHub Issue URL)
        *   **分析日期**：YYYY-MM-DD
        *   **分析者**：AI Agent (基於 Gemini)

        ## 2. 需求背景與上下文知識
        [詳細闡述 Issue 中描述的需求背景，補充相關的領域知識、業務邏輯或技術背景。說明業務目標、相關用戶角色及其痛點，以及現有系統中相關模組的概述。]

        ## 3. 開發計劃與步驟
        [提供高層次的開發路線圖和具體實施步驟。將開發過程劃分為關鍵里程碑，並針對每個里程碑列出具體的開發任務與依賴關係。]

        ## 4. 相關技術選型與建議
        [根據需求分析，推薦合適的技術棧、工具或函式庫，並詳細說明選型理由（如性能、可擴展性、維護性等）。同時列出可能的替代方案及其優缺點。]

        ## 5. 專案架構與資料整理
        [設計或建議專案的整體架構，規劃資料模型和儲存方式。包含系統架構的文字描述、模組劃分建議、API 設計原則，以及資料庫設計（如實體關係概念、資料表結構建議）。]

        ## 6. 潛在風險與挑戰
        [預見開發過程中可能遇到的技術或業務風險（如技術難點、性能瓶頸、安全性考量），並提出相應的應對策略。]

        ## 7. 參考資料
        [列出 LLM 在分析過程中參考的外部文獻、API 文件、技術文章等，以 `[標題](URL)` 格式呈現。]

        請確保輸出內容專業、嚴謹，且易於理解。
        ```
    *   呼叫 `scripts/issue_analysis_core.py` 腳本，將 Issue 內容和 LLM Prompt 作為參數傳入。
4.  **儲存知識文件**：
    *   將 LLM 返回的 Markdown 內容儲存為 `docs/knowledge/{{issue_number}}_{{issue_title_slug}}.md`。
    *   使用 Git 命令將新文件 commit 並 push 到倉庫。
5.  **更新 Issue 狀態與留言**：
    *   使用 GitHub API 在 Issue `#{{issue_number}}` 中新增留言，包含「分析已完成」的提示和知識文件的連結。
    *   將 Issue `#{{issue_number}}` 的標籤從 `status: analyzing` 更新為 `status: analyzed`。

## 錯誤處理 (Error Handling)

*   若在任何步驟中發生錯誤，將 Issue 標籤更新為 `status: analysis-failed`，並在 Issue 中留言說明錯誤原因。
*   記錄詳細的錯誤日誌。
