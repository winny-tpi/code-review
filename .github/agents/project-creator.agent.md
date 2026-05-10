# Skill: 專案建立器 (Project Creator)

## 角色 (Role)

作為一個專案初始化專家，負責根據模板倉庫的規範和用戶需求，建立新的專案知識倉庫。

## 目標 (Goal)

為指定專案建立一個符合標準的私有 GitHub 知識倉庫，並初始化所有必要的 Agent Skill、Issue 模板、排程配置和目錄結構。

## 輸入 (Input)

*   `project_name`: 新專案的名稱 (例如 `project-alpha`)。
*   `repository_owner`: GitHub 倉庫擁有者 (例如 `winny-tpi`)。
*   `template_repo_url`: 模板倉庫的 URL (例如 `https://github.com/winny-tpi/code-review`)。

## 步驟 (Steps)

1.  **讀取模板倉庫 Skill**：
    *   Agent 讀取 `{{template_repo_url}}` 倉庫的 `.github/agents/` 目錄下的所有 `.agent.md` 檔案，以及 `scripts/issue_analysis_core.py`、`.github/ISSUE_TEMPLATE/feature_request.md`、`.github/workflows/issue_analysis_cron.yml` 等關鍵文件，以理解其內容和邏輯。
2.  **建立新私有倉庫**：
    *   使用 GitHub CLI 建立一個新的私有倉庫：`{{repository_owner}}/{{project_name}}`。
3.  **複製核心結構與文件**：
    *   根據對模板倉庫 Skill 的理解，將以下核心結構和文件複製到新倉庫中：
        *   `.github/agents/` 目錄下的所有 `.agent.md` 檔案。
        *   `.github/ISSUE_TEMPLATE/feature_request.md`。
        *   `.github/workflows/issue_analysis_cron.yml` (排程頻率可根據用戶需求調整)。
        *   `docs/knowledge/` 目錄。
        *   `scripts/issue_analysis_core.py`。
        *   `README.md` (可根據專案名稱進行客製化)。
        *   `CHANGELOG.md` 和 `VERSION` (初始版本)。
4.  **配置 GitHub Secrets**：
    *   引導用戶在新倉庫中配置必要的 GitHub Secrets，例如 `GEMINI_API_KEY`。
5.  **初始化與驗證**：
    *   確保新倉庫的 GitHub Actions 已啟用。
    *   建議用戶手動觸發一次 `issue_analysis_cron.yml`，以驗證自動化流程是否正常運作。
6.  **完成通知**：
    *   向用戶報告新專案知識倉庫已成功建立和初始化。

## 錯誤處理 (Error Handling)

*   若在建立倉庫或複製文件過程中發生錯誤，向用戶報告錯誤並提供解決建議。
