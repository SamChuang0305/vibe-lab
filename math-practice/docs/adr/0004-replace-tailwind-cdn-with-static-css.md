# ADR 0004: 以 Tailwind CLI 靜態 CSS 取代 CDN Runtime Script

- 狀態：Accepted
- 日期：2026-02-25
- 決策者：vibe-lab/math-practice 維護者

## 背景

原本頁面於 `index.html` 使用：

- `https://cdn.tailwindcss.com`

此做法屬於 Tailwind 官方標註的開發用途 runtime 方案，會在 production console 顯示警告，且可能在 SEO 工具（如 Google Search Console 的網址檢查）中被標記。

## 決策

改採 Tailwind CLI 建置流程，正式頁面只載入靜態 CSS：

1. 新增 `styles/input.css` 作為 Tailwind 輸入檔。
2. 新增 `tailwind.config.js` 掃描 `index.html` 使用到的 class。
3. 新增 `package.json` 腳本：
   - `npm run build:css`
   - `npm run watch:css`
4. `index.html` 改為載入 `styles/tailwind.css`。
5. 移除 `js/tailwindcss/3.4.17.js`，避免誤用 runtime 版本。

## 為何不選擇其他方案

### 方案 A：維持 CDN Runtime Script

- 優點：零建置成本。
- 缺點：production 警示持續存在，不符合正式環境最佳實務。
- 決議：不採用。

### 方案 B：引入完整預編譯 CSS（不做內容掃描）

- 優點：導入較快。
- 缺點：檔案體積較大、含大量未使用樣式。
- 決議：不採用。

## 影響與後續

### 正面影響

1. 消除 production runtime 警示來源。
2. 正式站只載入靜態 CSS，載入行為可預期。
3. 建置流程明確，後續維護一致。

### 風險與限制

1. 需要可用 `npm install` 的環境才能重新編譯。
2. 若新增 class 未重編譯，可能造成樣式遺漏。

### 後續執行要點

1. 每次 UI class 變更後執行 `npm run build:css`。
2. 提交變更時同步提交更新後的 `styles/tailwind.css`。
3. 佈署後於 Search Console 重新提交網址檢查，等待重新抓取更新。
