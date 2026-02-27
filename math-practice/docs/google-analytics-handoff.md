# Google Analytics 設定交接（GA4）

## 目的

為 `算你厲害` 專案建立 GA4 基本流量與核心互動追蹤，並提供後續管理員可直接接手的設定與驗證步驟。

## 追蹤範圍

1. 頁面瀏覽（`page_view`）：`index.html`、`user-guide.html`
2. 核心事件（僅 `index.html`）
   - `mp_quiz_start`
   - `mp_answer_submit`
   - `mp_quiz_finish`
   - `mp_pdf_download_click`

## 佈署前必要動作

1. 本專案目前使用正式 GA4 Measurement ID：`G-EYGSR07X7W`。
2. 若未來需更換 ID，請同步更新以下兩個檔案中的 Measurement ID：
   - `index.html`
   - `user-guide.html`
3. 確認 `gtag/js` script URL 與 `gtag('config', ...)` 使用同一個正式 ID。

## 事件規格

### 1) `mp_quiz_start`

- 觸發時機：按下 `開始練習`，且設定驗證通過並成功進入作答頁後。
- 參數：
  - `operation_types`：題型清單（逗號分隔）
  - `question_count`：題目數量
  - `digit_length`：加減法數字位數
  - `mul_div_ranges`：乘除範圍（逗號分隔；無則 `none`）
  - `time_mode`：`limited` / `unlimited`
  - `time_limit`：秒數（不限時時為 `0`）
  - `hp_mode`：`off` / `1` / `2` / `3` / `4` / `5`
  - `score_mode`：`on` / `off`
  - `sound_mode`：`on` / `off`

### 2) `mp_answer_submit`

- 觸發時機：使用者提交有效答案（不含空值、格式錯誤）。
- 參數：
  - `question_index`：第幾題（從 1 開始）
  - `operation_type`：`add` / `sub` / `mul` / `div`
  - `is_correct`：是否答對（boolean）
  - `elapsed_seconds`：該題作答秒數
  - `current_streak`：提交後連對數
  - `current_score`：提交後目前積分
  - `remaining_hp`：提交後剩餘血量（無敵模式為 `-1`）

### 3) `mp_quiz_finish`

- 觸發時機：進入結果頁時。
- 參數：
  - `end_reason`：`normal` / `early_leave` / `hp_zero`
  - `total_questions`：總題數
  - `correct_count`：答對數
  - `wrong_count`：答錯數
  - `accuracy`：正確率（0-100）
  - `duration_seconds`：整場作答秒數
  - `final_score`：最終積分

### 4) `mp_pdf_download_click`

- 觸發時機：結果頁點擊 `下載 PDF` 並進入列印流程前。
- 參數：
  - `has_result_visible`：固定 `true`
  - `total_questions`：總題數
  - `correct_count`：答對數
  - `wrong_count`：答錯數

## 管理員後台額外設定（GA4）

### A. 不用額外設定也會生效的項目

1. `page_view` 與 4 個自訂事件只要前端有正確送出，GA4 就會自動接收。
2. 管理員不需要先在 GA4 建立事件名稱，事件也會先出現在 `即時` 報表或 `DebugView`。

### B. 建議設定 1：建立自訂定義（Custom definitions）

用途：讓事件參數可在 GA4 介面中穩定用於探索報表、比較與篩選。

操作路徑：

1. 進入 GA4 後台 `管理`。
2. 在 `資料顯示` 區塊進入 `自訂定義`。
3. 點擊 `建立自訂維度`（範圍選 `事件`）。
4. 依下列建議參數建立自訂維度（可分批建立）：
   - `end_reason`
   - `operation_type`
   - `time_mode`
   - `hp_mode`
   - `score_mode`

若需要做數值分析，可另外建立自訂指標（事件範圍）：

- `accuracy`
- `duration_seconds`
- `final_score`

### C. 建議設定 2：標記關鍵事件（Key events）

用途：把重要行為列為轉換指標。

操作路徑：

1. 進入 GA4 後台 `管理`。
2. 在 `資料顯示` 區塊進入 `事件`。
3. 找到事件名稱後，開啟「標記為關鍵事件」。

本專案建議：

1. 先標記 `mp_quiz_finish`（代表一輪練習完成）。
2. 視需求再標記 `mp_pdf_download_click`（代表有下載留存行為）。

## 驗證流程（管理員）

1. 發布後開啟網站：
   - `https://sam.webspace.tw/vibe-lab/math-practice/`
   - `https://sam.webspace.tw/vibe-lab/math-practice/user-guide.html`
2. 在瀏覽器 DevTools Network 確認有載入：
   - `https://www.googletagmanager.com/gtag/js?id=你的正式ID`
3. 到 GA4 `即時` 報表確認可見 `page_view`。
4. 到 GA4 `DebugView`，實際走一次流程（開始練習、送答、完成、下載 PDF），確認可見上述 4 個自訂事件與主要參數（例如 `end_reason`、`accuracy`）。
5. 若已建立自訂定義，等待生效後到 GA4 `探索` 驗證可用於欄位與篩選。
6. 若已標記關鍵事件，到標準報表確認關鍵事件計數有成長。

補充：

1. `即時` 與 `DebugView` 幾乎即時。
2. 標準報表與探索資料可能有延遲，屬正常現象。

## 常見問題排查

1. 事件完全沒進來：
   - 檢查兩頁中的 Measurement ID 是否為最新正式值（目前為 `G-EYGSR07X7W`）。
2. 只有 `page_view` 沒有自訂事件：
   - 檢查前端是否有 JS 錯誤中斷。
   - 檢查是否被瀏覽器擋追蹤（廣告阻擋套件）。
3. 事件看得到，但探索報表找不到參數欄位：
   - 檢查是否已建立對應的自訂定義。
   - 自訂定義剛建立後可能需要一些時間才會在報表可用。
4. 已標記為關鍵事件，但數字沒有增加：
   - 檢查事件名稱是否完全一致（含大小寫）。
   - 檢查測試流程是否真的有觸發該事件。
5. 測試環境與正式環境資料混在一起：
   - 建議建立獨立 GA4 Data Stream 或在事件參數加入環境標記（另開需求）。

## 維運建議

1. 若新增按鈕或流程節點，請同步更新：
   - `index.html` 事件觸發程式碼
   - 本交接文件的「事件規格」
   - `spec.md` 追蹤規格段落
2. 若未來需改用 GTM，建議保留目前事件命名，降低報表斷層風險。
