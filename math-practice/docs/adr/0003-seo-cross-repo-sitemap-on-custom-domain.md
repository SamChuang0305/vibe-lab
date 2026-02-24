# ADR 0003: 自訂網域下主站與子路徑專案的 SEO Sitemap 協作策略

- 狀態：Accepted
- 日期：2026-02-24
- 決策者：vibe-lab/math-practice 維護者

## 背景

目前使用同一個自訂網域 `sam.webspace.tw`，但內容由兩個 GitHub Pages Repo 提供：

1. 主站 Repo：`<username>.github.io`，由 Hexo 產生 Blog，Hexo 會自動產生 `/sitemap.xml`。
2. 子專案 Repo：提供 `/vibe-lab/math-practice/` 靜態頁面。

限制與問題：

1. Hexo 產生的 `sitemap.xml` 會在更新時覆蓋，不適合手動維護跨專案內容。
2. `robots.txt` 應以網域根目錄 `https://sam.webspace.tw/robots.txt` 為準。
3. 若未明確宣告子專案 sitemap，子專案收錄速度與穩定性可能受影響。

## 決策

採用「雙 sitemap 宣告」策略，不手動修改 Hexo 產生的 sitemap 內容。

1. 主站維持 Hexo 自動產生 `https://sam.webspace.tw/sitemap.xml`。
2. 子專案 Repo 自行維護 `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`。
3. 在根網域 `robots.txt` 宣告兩份 sitemap：
   - `https://sam.webspace.tw/sitemap.xml`
   - `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
4. 子專案頁面 canonical 固定為：
   - `https://sam.webspace.tw/vibe-lab/math-practice/`

## 為何不選擇其他方案

### 方案 A：手動把子專案網址寫進 Hexo sitemap

- 優點：看似集中管理。
- 缺點：每次 Hexo 重新產生會覆蓋，維護風險高。
- 決議：不採用。

### 方案 B：只提交主站 sitemap，不宣告子專案 sitemap

- 優點：設定最少。
- 缺點：子專案收錄依賴爬蟲自行發現，控制性較差。
- 決議：不採用。

## 影響與後續

### 正面影響

1. 主站與子專案可各自獨立維護，不互相覆蓋。
2. 搜尋引擎可穩定發現子專案 URL。
3. 符合 GitHub Pages + Hexo 的實際維運模式。

### 風險與限制

1. 需要可修改根網域 `robots.txt` 的權限。
2. 若 canonical 或 sitemap URL 設錯，會造成收錄訊號分散。

### 後續執行要點

1. 根網域 robots 由主站（Hexo）管理。
2. 子專案 Repo 維護自己的 `sitemap.xml` 與頁面 meta（canonical/og/json-ld）。
3. Google Search Console 提交兩份 sitemap 並驗證索引狀態。
