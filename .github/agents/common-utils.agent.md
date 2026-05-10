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
