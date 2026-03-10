# 算你厲害

## 文件說明
- 使用說明：本文件（`README.md`）
- 產品規格：`spec.md`
- 終端使用者操作說明頁：`user-guide.html`

## 使用技術
- HTML
- JavaScript
- Tailwind CSS（透過 CLI 輸出靜態 `styles/tailwind.css`）

## 快速開始
- 直接以瀏覽器開啟 `index.html`。
- 建議使用本機靜態伺服器：
  - `npm run serve`
  - 開啟 `http://localhost:40000`
  - 或使用：`python3 -m http.server 8000` 後開啟 `http://localhost:8000`

## 樣式建置（Tailwind CLI）
- 安裝相依套件：
  - `npm install`
- 產生正式樣式檔：
  - `npm run build:css`
- 開發時即時編譯：
  - `npm run watch:css`
- 產線頁面載入 `styles/tailwind.css?v=<buildId>`，不使用 `cdn.tailwindcss.com`。
- `index.html`、`user-guide.html` 的 `?v=` 必須與目前 `buildId` 一致，避免部署後沿用舊 CSS 快取。

## SEO 與收錄
- 子專案正式網址（canonical）：`https://sam.webspace.tw/vibe-lab/math-practice/`
- 首頁網址正規化：若使用者進入 `.../index.html`，前端會自動改寫為 `.../`（保留 query/hash），降低 GA4 路徑分流。
- 本 repo 維護子專案 sitemap：`https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
- 社群分享預覽圖統一使用：`https://sam.webspace.tw/vibe-lab/math-practice/images/social_preview_1200x630.png`（1200x630 PNG），並由 `index.html` 的 Open Graph / Twitter meta 引用。
- `index.html` 與 `user-guide.html` 頁面頂部共用同一張 Banner：`./images/social_preview_1200x630.png`。
- 主網域 `robots.txt` 由主站（Hexo）維護，建議在 `https://sam.webspace.tw/robots.txt` 宣告：
  - `https://sam.webspace.tw/sitemap.xml`
  - `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
- 子專案收錄檢查建議：
  1. 到 Google Search Console 提交 `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
  2. 以 URL 檢查工具測試 `https://sam.webspace.tw/vibe-lab/math-practice/` 是否可建立索引

## Google Analytics（GA4）
- `index.html` 與 `user-guide.html` 皆已導入 GA4 `gtag.js` 基本追蹤碼。
- `index.html` 另包含核心事件追蹤：
  - `mp_quiz_start`
  - `mp_answer_submit`
  - `mp_quiz_finish`
  - `mp_pdf_download_click`
- 目前程式碼使用正式 GA4 Measurement ID：`G-EYGSR07X7W`。
- 管理員交接文件：`docs/google-analytics-handoff.md`

## 使用流程
1. 首頁主標題顯示為 `算你厲害｜關卡設定`。
2. 初次使用可先點擊設定頁的 `操作說明`，開啟 `user-guide.html` 查看完整新手教學。
3. 在設定頁依序調整題目種類、加、減法出題數字、乘、除法出題範圍、答題時間限制、秒數、題目數量、血條設定、積分顯示、音效。
   - 若勾選多種題目種類，系統會平均分配各題型題數；若有餘數，依加法、減法、乘法、除法順序補齊。
   - 一般十位數調整為 `10~99`，一般百位數調整為 `100~999`。
   - 加、減法出題數字可選 `基礎練習（10 的合成與分解）`，加法題型為 `x + ? = 10`，減法題型為 `10 - x = ?`。
   - 加、減法出題數字可選 `基礎練習（合十加法、拆十減法）`，加法題型為 `a + b = ?`（`a`、`b` 為 `1~9` 且 `a + b >= 11`，共 `36` 題），減法題型為 `a - b = ?`（`a` 為 `11~18`、`b` 為 `1~9` 且 `a - b < 10`，共 `36` 題）。
   - 加、減法出題數字新增 `十位數困難版（加法必進位、減法必借位）`（10~99）。
   - `說明` 區塊最後會顯示目前程式版本（格式：`YYYYMMDD.N`）。
4. 若要快速回到預設值，可點擊 `重置預設值`（位於 `開始練習` 右側）。
5. 點擊 `開始練習` 進入作答頁。
   - 切換到作答頁時，畫面會自動回到頂部。
6. 輸入答案後點擊 `送出`，或直接按 `Enter` 送出（若開啟積分顯示，作答區右側會顯示目前積分）。
   - 作答輸入僅接受數字（`0~9`），其他字元（含空白、`.`、`+`、`-`）會自動忽略。
   - 設定頁與作答頁的數字/文字輸入框已停用自動填滿（`autocomplete`、`autocorrect`、`autocapitalize`、`spellcheck`）。
   - 點擊 `離開` 會顯示全黑遮罩確認視窗；確認視窗開啟期間倒數會暫停，按下 `取消`（或 `Esc`）後會把目前題目移到最後一題、改顯示原本下一題，並重新計算該題作答時間。
7. 全部題目完成（或提早離開）後進入結果頁查看統計、題目明細與學習建議。
   - 切換到結果頁時，畫面會自動回到頂部。
   - `本次關卡設定` 會顯示本輪的程式版本（格式：`YYYYMMDD.N`）。
8. 可點擊 `重新開始` 回到設定頁開始新一輪。
   - 回到設定頁時，畫面會自動回到頂部。
   - 系統會在此時檢查是否有新版；若有會顯示「立即更新」提示，點擊後重新載入頁面。

## 版本更新機制（buildId）
- 版本識別使用 `buildId` 字串（格式：`YYYYMMDD.N`，例如 `20260302.2`）。
- 使用者端僅在「回到設定頁」時檢查 `version.json` 是否有新 `buildId`，不會在作答中強制刷新。
- 版本比較採「字串是否相等」，不做時間或時區判斷。
- `Date.now()` 只用於 `version.json` 請求的防快取參數，不參與版本比較。

### 發布時必做
1. 更新根目錄 `version.json` 的 `buildId`。
2. 同步更新 `index.html` 內的 `CURRENT_BUILD_ID`。
3. 同步更新 `index.html`、`user-guide.html` 的 `styles/tailwind.css?v=<buildId>`。
4. 確認三者（`version.json`、`CURRENT_BUILD_ID`、`tailwind.css?v=`）完全一致後再部署。

## 操作說明頁（給終端使用者）
- 檔案：`user-guide.html`
- 入口：`index.html` 設定頁按鈕列中的 `操作說明`。
- 返回：`user-guide.html` 底部提供 `開始練習` 按鈕可回首頁根路徑（`./`）。
- 維護規則：後續只要功能、按鈕名稱或操作流程異動，需同步更新 `user-guide.html`。

## 積分規則
- 設定頁可切換 `積分顯示`（預設開啟）。
- 每答對一題 `+100` 分。
- 每連續答對每滿 `5` 題，依連續區間加送：第 `5` 題 `+250`、第 `10` 題 `+500`、第 `15` 題 `+750`，依此類推。
- 答錯或超時不扣分，但連續答對數量歸 `0` 重算。
- 範例：
  - 連續答對 `5` 題：`500 + 250 = 750`
  - 連續答對 `10` 題：`1000 + 250 + 500 = 1750`
  - 連續答對 `15` 題：`1500 + 250 + 500 + 750 = 3000`
  - 連續答對 `20` 題：`2000 + 250 + 500 + 750 + 1000 = 4500`
- 關閉 `積分顯示` 時僅隱藏「目前積分 / 總得分」，背景仍照常計分。

## 下載 PDF 操作
1. 完成作答後，進入 `作答結果` 頁面。
2. 點擊 `下載 PDF` 按鈕。
3. 瀏覽器會開啟列印視窗，選擇「另存為 PDF」。
4. 確認紙張、邊界與頁面範圍後儲存。
5. 若使用 Android/iOS 裝置，請參考結果頁的「Android/iOS 檔名提示」，可點擊建議檔名快速複製，並在儲存時手動套用。
6. 若點擊 `下載 PDF` 後沒有反應，建議改用 Chrome 瀏覽器再試一次。

作答結果頁在 `學習建議` 區塊下方提供「下載 PDF 說明」區塊，說明下載留存與檢視弱項的用途；該區塊僅供畫面閱讀，不會列入匯出 PDF 內容。

## 已知限制（PDF 匯出）
- PDF 匯出依賴瀏覽器原生列印功能，版面會受瀏覽器版本與列印設定影響。
- 桌面瀏覽器通常可套用 `算你厲害-YYYYMMDD-HHmmss.pdf`；行動裝置（如 Android、iOS）可能改由瀏覽器或系統決定檔名。
- 題目數量很多（例如 200 題）時，列印預覽與輸出時間會增加。
