# Skill: 模板版本追蹤器 (Template Version Tracker)

## 角色 (Role)

作為一個版本管理專家和最佳實踐顧問，負責監控公開模板倉庫的進化，並為專案提供智慧化的跟版建議。

## 目標 (Goal)

定期檢查公開模板倉庫的更新，分析其對當前專案的影響，並生成詳細的更新建議報告。

## 前置條件 (Preconditions)

*   可存取公開模板倉庫 `winny-tpi/code-review`。
*   專案倉庫中存在 `VERSION` 檔案記錄當前模板版本。

## 輸入 (Input)

*   `current_repo_owner`: 當前專案倉庫擁有者。
*   `current_repo_name`: 當前專案倉庫名稱。
*   `template_repo_owner`: 模板倉庫擁有者 (例如 `winny-tpi`)。
*   `template_repo_name`: 模板倉庫名稱 (例如 `code-review`)。

## 步驟 (Steps)

1.  **獲取當前模板版本**：
    *   Agent 讀取當前專案倉庫根目錄下的 `VERSION` 檔案，獲取專案當前使用的模板版本號。
2.  **獲取最新模板版本**：
    *   Agent 讀取 `{{template_repo_owner}}/{{template_repo_name}}` 倉庫根目錄下的 `VERSION` 檔案，獲取最新模板版本號。
3.  **比對版本與變更**：
    *   如果最新版本號高於當前版本號，Agent 讀取模板倉庫的 `CHANGELOG.md`，解析自當前版本以來的變更內容。
4.  **差異分析**：
    *   Agent 下載模板倉庫的關鍵文件（例如 `.github/agents/` 下的所有 `.agent.md`、`scripts/issue_analysis_core.py`、`.github/ISSUE_TEMPLATE/feature_request.md`、`.github/workflows/issue_analysis_cron.yml`）。
    *   Agent 將這些文件與當前專案倉庫中對應的文件進行內容差異比對。
    *   Agent 評估這些差異對專案現有流程的潛在影響（例如功能改進、錯誤修復、性能優化、潛在的破壞性變更）。
5.  **生成更新建議報告**：
    *   Agent 構建一份詳細的更新建議報告，以 GitHub Issue 或 Pull Request 的形式提交到當前專案知識倉庫。
    *   報告內容應包含：
        *   模板倉庫的新版本號和 `CHANGELOG.md` 的摘要。
        *   具體的差異點和代碼片段。
        *   建議的更新方式和理由。
        *   更新可能帶來的收益和潛在風險。
        *   如果更新涉及配置變更，提供詳細的配置指南。
6.  **完成通知**：
    *   向專案團隊報告已生成更新建議，並建議審閱。

## 錯誤處理 (Error Handling)

*   若在獲取版本或比對差異過程中發生錯誤，向專案團隊報告錯誤並提供解決建議。
