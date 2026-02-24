# SEO 設定交接：sam.webspace.tw 主站 + /vibe-lab/math-practice 子專案

## 目的

在同一自訂網域下，讓主站 Blog（Hexo）與子專案（GitHub Pages）都能被穩定收錄，並避免 sitemap 被覆蓋造成失效。

## 架構說明

1. 主站：`https://sam.webspace.tw/`（Hexo，自動產生 sitemap）
2. 子專案：`https://sam.webspace.tw/vibe-lab/math-practice/`（另一個 Repo）

## 必做設定

### 步驟 1：在主站根目錄 robots.txt 宣告兩份 sitemap

請在主站 Hexo Repo 的 `source/robots.txt` 設定為：

```txt
User-agent: *
Allow: /

Sitemap: https://sam.webspace.tw/sitemap.xml
Sitemap: https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml
```

說明：

1. 不要手動修改 Hexo 自動產生的 `sitemap.xml`。
2. 以根網域 `robots.txt` 宣告主站 sitemap 與子專案 sitemap。

### 步驟 2：子專案 Repo 維護自己的 sitemap.xml

子專案需提供：

- `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`

最小內容範例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://sam.webspace.tw/vibe-lab/math-practice/</loc>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

### 步驟 3：子專案首頁 canonical 一致化

子專案 `index.html` 請確認：

1. `canonical` = `https://sam.webspace.tw/vibe-lab/math-practice/`
2. `og:url` = `https://sam.webspace.tw/vibe-lab/math-practice/`
3. JSON-LD `url` = `https://sam.webspace.tw/vibe-lab/math-practice/`

## 驗證清單

1. `https://sam.webspace.tw/robots.txt` 可開啟，且看得到兩條 `Sitemap` 宣告。
2. `https://sam.webspace.tw/sitemap.xml` 可開啟（Hexo 主站）。
3. `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml` 可開啟（子專案）。
4. 子專案首頁原始碼可看到 canonical、Open Graph、JSON-LD 都是同一個正式網址。

## Search Console 操作

1. 進入 `sam.webspace.tw` 的 Search Console 屬性。
2. 在 Sitemap 提交以下兩條：
   - `https://sam.webspace.tw/sitemap.xml`
   - `https://sam.webspace.tw/vibe-lab/math-practice/sitemap.xml`
3. 用 URL 檢查工具測試：
   - `https://sam.webspace.tw/vibe-lab/math-practice/`
4. 若顯示可建立索引，即設定完成。

## 常見錯誤

1. 只提交主站 sitemap，未提交子專案 sitemap。
2. canonical 寫成 `github.io` 或少了 `/vibe-lab/math-practice/`。
3. 以手動方式修改 Hexo 產生的 sitemap，導致下次部署被覆蓋。
