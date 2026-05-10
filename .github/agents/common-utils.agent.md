# Skill: 共用工具與規範 (Common Utilities and Standards)

## 角色 (Role)

提供其他 Agent Skill 可引用的共用功能、規範和資料結構，確保一致性。

## 目標 (Goal)

集中管理共用元素，簡化其他 Skill 的定義。

## 內容 (Content)

### 標籤管理規範 (Label Management Standard)

所有專案知識倉庫應遵循以下標籤規範：

| 標籤名稱 | 顏色建議 | 意義與用途 |
| :--- | :--- | :--- |
| `status: pending-analysis` | 黃色 | 表示這是一個新提交的 Issue，等待排程系統擷取並交由 LLM 進行分析。這是觸發自動化流程的關鍵標籤。 |
| `status: analyzing` | 藍色 | 表示排程系統已擷取該 Issue，目前 LLM 正在進行分析與文件生成。這可防止排程系統重複處理同一個 Issue。 |
| `status: analyzed` | 綠色 | 表示 LLM 已完成分析，知識文件已生成並提交至倉庫，且已在 Issue 中留言回覆。開發者可以開始參考文件進行開發。 |
| `status: analysis-failed` | 紅色 | 表示在擷取 Issue、呼叫 LLM API 或提交文件過程中發生錯誤。需要人工介入檢查日誌並排除問題。 |
| `approved` | 紫色 | **人類授權標籤**：表示該 Issue 已由倉庫擁有者或協作者審核並批准，Agent 可以開始處理。這是觸發 Agent 執行任務的關鍵安全機制。 |

### 安全策略：人類授權機制 (Human Authorization Mechanism)

為確保 Agent 在公開倉庫中的操作安全，避免惡意或無效的 Issue 消耗資源，引入「人類授權」機制：

*   **公開倉庫 (`winny-tpi/code-review`)**：
    *   Agent **只會處理**帶有 `approved` 標籤的 Issue。
    *   `approved` 標籤只能由倉庫擁有者或具有寫入權限的協作者手動添加。
    *   任何沒有 `approved` 標籤的 Issue 將被 Agent 完全忽略，不進行任何處理。
    *   流程：用戶發 Issue → 倉庫擁有者審核並手動添加 `approved` 標籤 → Agent 處理。

*   **私有專案倉庫 (`winny-tpi/<project-name>`)**：
    *   由於私有倉庫的 Issue 提交者通常已是授權成員，可以選擇性地啟用 `approved` 標籤機制。
    *   預設情況下，私有倉庫的 Agent 可以自動處理所有帶有 `status: pending-analysis` 標籤的 Issue，無需額外的 `approved` 標籤。
    *   若專案團隊希望在私有倉庫中也引入額外的審核層，則可選擇啟用 `approved` 標籤機制。

### LLM API 呼叫規範 (LLM API Call Standard)

*   **模型**：建議使用 `Gemini` 或其他兼容 OpenAI API 的 LLM。
*   **API Key**：應透過 `GEMINI_API_KEY` 或類似的環境變數/Secret 提供。
*   **Prompt 結構**：應包含明確的 System Prompt 指示 LLM 角色和輸出格式。

### 文件命名規範 (Document Naming Standard)

LLM 產出的知識文件應儲存於 `docs/knowledge/` 目錄下，並遵循以下命名格式：

`{{issue_number}}_{{issue_title_slug}}.md`

例如：`123_feature-x-development-plan.md`

### 排程頻率建議 (Scheduling Frequency Recommendation)

*   **Issue 分析排程**：建議每 30 分鐘或每小時執行一次。
*   **模板更新檢查排程**：建議每週執行一次。
