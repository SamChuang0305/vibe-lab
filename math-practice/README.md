# 數學計算練習

## 文件說明
- 使用說明：本文件（`README.md`）
- 產品規格：`spec.md`

## 使用技術
- HTML
- JavaScript
- Tailwind CSS

## 快速開始
- 直接以瀏覽器開啟 `index.html`。
- 建議使用本機靜態伺服器：
  - `python3 -m http.server 8000`
  - 開啟 `http://localhost:8000`

## SEO 與收錄
- 子專案正式網址（canonical）：`https://sam.webspace.tw/vibe-lab/math-practice/`
- 本 repo 維護子專案 sitemap：`https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
- 主網域 `robots.txt` 由主站（Hexo）維護，建議在 `https://sam.webspace.tw/robots.txt` 宣告：
  - `https://sam.webspace.tw/sitemap.xml`
  - `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
- 子專案收錄檢查建議：
  1. 到 Google Search Console 提交 `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
  2. 以 URL 檢查工具測試 `https://sam.webspace.tw/vibe-lab/math-practice/` 是否可建立索引

## 使用流程
1. 在設定頁依序調整題目種類、加、減法出題數字、乘、除法出題範圍、答題時間限制、秒數、題目數量、血條設定、積分顯示、音效。
2. 若要快速回到預設值，可點擊 `重置預設值`（位於 `開始練習` 右側）。
3. 點擊 `開始練習` 進入作答頁。
4. 輸入答案後點擊 `送出`，或直接按 `Enter` 送出（若開啟積分顯示，作答區右側會顯示目前積分）。
5. 全部題目完成（或提早離開）後進入結果頁查看統計、題目明細與學習建議。
6. 可點擊 `重新開始` 回到設定頁開始新一輪。

## 積分規則
- 設定頁可切換 `積分顯示`（預設開啟）。
- 每答對一題 `+100` 分。
- 每連續答對每滿 `5` 題再加送 `250` 分（可持續累積）。
- 答錯或超時不扣分，但連續答對數量歸 `0` 重算。
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
- 桌面瀏覽器通常可套用 `數學計算練習-YYYYMMDD-HHmmss.pdf`；行動裝置（如 Android、iOS）可能改由瀏覽器或系統決定檔名。
- 題目數量很多（例如 200 題）時，列印預覽與輸出時間會增加。
