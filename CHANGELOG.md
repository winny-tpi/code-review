# CHANGELOG

## 1.0.0 - 2026-05-10

*   Initial release of the Agent-driven Code Review template repository.
*   Includes core Agent Skills: `issue-analyzer`, `project-creator`, `version-tracker`, `common-utils`.
*   Standardized Issue template (`feature_request.md`).
*   Placeholder for `issue_analysis_core.py` script.
*   Initial `README.md` and `VERSION` file.

## 1.1.0 - 2026-05-10

*   新增 `template-advisor.agent.md` Skill，提供模板諮詢與優化功能，並引入 PR 審核安全機制。
*   更新 `project-creator.agent.md`，建議新專案建立前諮詢模板顧問。
*   更新 `version-tracker.agent.md`，確保跟版建議透過 PR 提交。
*   在 `AGENTS.md` 中加入 `template-advisor.agent.md` 的說明。
*   **重要安全更新**：所有對模板的修改都必須透過 PR 審核。

## 1.2.0 - 2026-05-10

*   **重要安全更新**：引入「人類授權」機制，Agent 僅處理帶有 `approved` 標籤的 Issue。
*   更新 `common-utils.agent.md`，定義 `approved` 標籤並區分公開與私有倉庫的安全策略。
*   更新 `template-advisor.agent.md`，加入 `approved` 標籤的前置條件。
*   更新 `issue-analyzer.agent.md`，加入 `approved` 標籤的前置條件。
*   更新 `AGENTS.md`，在總覽中說明新的安全機制。
