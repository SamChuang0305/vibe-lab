# Repository Guidelines

## Project Structure & Module Organization
- `index.html`: 單一入口與主要應用程式邏輯（UI、出題、作答、結果、PDF 列印）。
- `version.json`: 版本資訊檔，保存 `buildId`，需與 `index.html` 的 `CURRENT_BUILD_ID` 保持一致。
- `README.md`: 使用說明與快速上手指南。
- `spec.md`: 產品規格文件，功能或規則調整時需同步更新。
- `user-guide.html`: 終端使用者操作說明頁（給非技術使用者閱讀）。
- `docs/adr/`: 架構決策紀錄（ADR），重大設計變更請新增一份檔案。
- `styles/input.css`: Tailwind CSS 輸入檔（`@tailwind` 指令）。
- `styles/tailwind.css`: 產線使用的靜態樣式輸出檔（由 CLI 產生後提交）。
- `tailwind.config.js`: Tailwind 掃描與主題設定。
- `package.json`: Tailwind CLI 建置腳本與相依套件版本。
- `js/URL.txt`: 外部資源參考。
- 使用裝置範圍：電腦、手機、平板；行動裝置包含 Android 與 iOS。

## Build, Test, and Development Commands
- 本專案主程式無 JS 打包流程，直接以瀏覽器開啟 `index.html` 即可執行。
- 樣式建置（Tailwind CLI）：
  - `npm install`
  - `npm run build:css`
  - `npm run watch:css`（開發時即時編譯）
- 建議本機啟動靜態伺服器避免瀏覽器限制：
  - `npm run serve`
  - 開啟 `http://localhost:40000`
  - 或使用 `python3 -m http.server 8000` 後開啟 `http://localhost:8000`
- 快速檢查變更：
  - `git diff -- index.html user-guide.html README.md spec.md AGENTS.md`
  - `rg "關鍵字" index.html user-guide.html README.md spec.md AGENTS.md`
  - `rg "CURRENT_BUILD_ID|buildId" index.html version.json`
  - `git diff -- index.html version.json`

## Coding Style & Naming Conventions
- 使用 2 spaces 縮排（與既有 `index.html` 一致）。
- JavaScript 採 `camelCase` 命名；常數使用 `UPPER_SNAKE_CASE`。
- DOM id 使用語意化命名（如 `learningSuggestionList` 對應 `#learningSuggestionList`）。
- 文案以台灣常用繁體中文為主，避免中英混雜同義詞。
- 優先做小範圍、可回溯的修改，避免無關重構。

## Testing Guidelines
- 目前無自動化測試框架，採手動驗證。
- 每次修改至少檢查：
  - 設定頁 → 作答頁 → 結果頁完整流程。
  - 結果表格、學習建議、PDF 列印預覽是否符合預期。
  - 手機寬度與桌面寬度下版面是否可用。
- 關鍵流程變更（作答、計分、血量、PDF、學習建議）時，至少加測：
  - 血量歸零後 `💥 GAME OVER！` modal 的顯示、關閉與結束流程。
  - 作答輸入框 `Enter` 送出、`Esc` 清空、非數字字元過濾。
  - 學習建議「套用此建議關卡」可正確回填設定並重啟練習。
  - PDF 下載流程、失敗提示、Android/iOS 檔名提示。
- 輸入框行動裝置驗證：
  - 題目數量、秒數、作答答案欄位需停用 `autocomplete`、`autocorrect`、`autocapitalize`、`spellcheck`。
  - 手機若仍顯示候選字列，視為輸入法行為，不視為網站自動填滿回歸。
- 響應式驗收：
  - 手機版主要按鈕（開始練習、送出、清空、離開、重新開始、下載 PDF）需維持全寬。
  - 桌面版主要按鈕需維持自動寬度，避免版面回歸。
- 若變更規則邏輯，請在 PR 描述提供「前/後行為」與至少 1 組範例。

## Release Guardrails
- SEO 相關變更（`head`、canonical、OG/Twitter、JSON-LD、sitemap/robots、網址正規化）時，PR 必須附 SEO 檢查結果。
- 流程、按鈕、頁面切換異動時，必須驗證 GA4 事件仍可正確送出：
  - `mp_quiz_start`
  - `mp_answer_submit`
  - `mp_quiz_finish`
  - `mp_pdf_download_click`
- `gtag` 不存在時不得影響主流程，作答、計分、PDF 匯出需可正常運作。
- 更新提示只可在設定頁顯示；作答中與結果頁不得主動跳出更新提示或強制刷新。
- 版本比較只允許字串是否相等判斷，不做時間、時區或大小比較。

## Commit & Pull Request Guidelines
- Commit type 僅允許以下類別：
  - `feat`: 新增/修改功能（feature）
  - `fix`: 修補 bug（bug fix）
  - `docs`: 文件（documentation）
  - `style`: 格式（不影響程式碼運行，如 white-space、formatting）
  - `refactor`: 重構（非新增功能、非修 bug）
  - `perf`: 改善效能
  - `test`: 增加測試
  - `chore`: 建構程序或輔助工具變動（maintain）
  - `revert`: 撤銷先前 commit（例如 `revert: type(scope): subject`）
- 建議格式：`type: 簡短動作 + 影響範圍`（可含中括號標示功能區，如 `[下載 PDF]`）。
- 流程規則：只要該次有任何程式或規格修改，需在該次變更中同步檢查並更新 `index.html`、`user-guide.html`（功能/流程/按鈕文案異動時必須更新）、`spec.md`、`README.md`（僅使用說明）、`docs/adr/`（若有架構決策變更）與 `AGENTS.md`（若有流程規範變更），完成後執行 `git commit`。
- 版本升版規則（buildId）：
  - 只要該次 commit 有任何「程式行為、畫面互動、商業規則、文案顯示邏輯」變更，必須同步升版 `buildId`。
  - 升版時必須同時更新 `version.json` 的 `buildId` 與 `index.html` 的 `CURRENT_BUILD_ID`，且兩者值必須完全一致。
  - `buildId` 格式維持 `YYYYMMDD.N`；同日多次修改需遞增 `N`。
  - `buildId` 的日期（`YYYYMMDD`）一律以台灣時間（`UTC+08:00`）當下日期為準。
  - 若開發環境顯示為其他時區，需先換算為台灣時間後再決定 `YYYYMMDD`，不可直接沿用本機時區日期。
  - 跨日邊界時（例如本機仍為前一天、台灣已進入隔日），必須使用台灣日期；例如台灣時間為 `2026-03-03` 時，`buildId` 日期必須是 `20260303`。
  - 若僅文件調整（例如純 `README.md`、`spec.md`、`user-guide.html` 文字修正）且無任何程式行為變更，可不升版。
  - 每次提交前必做人工檢查：`rg "CURRENT_BUILD_ID|buildId" index.html version.json` 與 `git diff -- index.html version.json`。
  - 送出最終回覆時，需明確列出「本次 buildId 舊值 -> 新值」；若未升版，需說明未升版原因。
- PR 需包含：
  - 變更摘要與動機
  - 受影響檔案清單
  - 手動測試步驟與結果
  - 若屬 SEO / GA4 / 版本更新機制 / 作答流程異動，需附對應檢查紀錄（文字或截圖皆可）
  - UI 變更截圖（若有）
- 若屬設計決策變更，請同步新增 `docs/adr/` 文件。
