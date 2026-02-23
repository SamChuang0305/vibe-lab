# Repository Guidelines

## Project Structure & Module Organization
- `index.html`: 單一入口與主要應用程式邏輯（UI、出題、作答、結果、PDF 列印）。
- `README.md`: 使用說明與快速上手指南。
- `spec.md`: 產品規格文件，功能或規則調整時需同步更新。
- `docs/adr/`: 架構決策紀錄（ADR），重大設計變更請新增一份檔案。
- `js/tailwindcss/3.4.17.js`: 本地 Tailwind 腳本備援；目前主要透過 CDN 載入。
- `js/URL.txt`: 外部資源參考。

## Build, Test, and Development Commands
- 本專案無打包流程，直接以瀏覽器開啟 `index.html` 即可執行。
- 建議本機啟動靜態伺服器避免瀏覽器限制：
  - `python3 -m http.server 8000`
  - 開啟 `http://localhost:8000`
- 快速檢查變更：
  - `git diff -- index.html README.md spec.md AGENTS.md`
  - `rg "關鍵字" index.html README.md spec.md AGENTS.md`

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
- 若變更規則邏輯，請在 PR 描述提供「前/後行為」與至少 1 組範例。

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
- 流程規則：只要該次有任何程式或規格修改，需在該次變更中同步檢查並更新 `index.html`、`spec.md`、`README.md`（僅使用說明）、`docs/adr/`（若有架構決策變更）與 `AGENTS.md`（若有流程規範變更），完成後執行 `git commit`。
- PR 需包含：
  - 變更摘要與動機
  - 受影響檔案清單
  - 手動測試步驟與結果
  - UI 變更截圖（若有）
- 若屬設計決策變更，請同步新增 `docs/adr/` 文件。
