---
layout: post
title: 改善 Google SEO 的方式，以及相關的 Jekyll 教學
date: 2025-06-01 17:00:00 +0800
categories: [Jekyll in GitHub Pages]
---

如何在 Google 搜尋引擎的索引中，為自己網站改善搜尋的排名，增加被看見的機會。如果有用 Jekyll 架站的話，也可以參考相關的教學。

### 官方的入門指南

每個想調整自家網站 SEO 的人，都應該先從 Google 提供的 [搜尋引擎最佳化 (SEO) 入門指南：基本概念 - Google 搜尋中心](https://developers.google.com/search/docs/fundamentals/seo-starter-guide?hl=zh-tw)  開始！

### 「已檢索-目前尚未建立索引」的解決方式

如果出現了「已檢索-目前尚未建立索引」(Crawled, Currently Not Indexed ) 的相關網頁，可以從以下幾點著手進行：

1. 建立更多內部連結，不要出現孤兒頁面。
2. 網站的整體結構應該要像一棵樹 (主頁面 - 眾多子頁面 or 文章)。
3. 建立 Sitemap (站台地圖)。
4. 要求 Google Robot 不要索引一些不重要的網頁，例如：第幾頁、分類頁、標籤頁等文章列表，有可能會降低你的網站整體品質。
5. 改進你的網頁內容，寫出好的文章，引用不錯的外部連結。
6. 手動索引 ... 一個一個把網頁加進去吧！

上方內容節錄自 [GSC > Crawled, Currently Not Indexed : r/SEO](https://www.reddit.com/r/SEO/comments/1afgcr6/gsc_crawled_currently_not_indexed/) 內網友的回應。

### Sitemap 沒有更新的解決方式

如果在 Google Search Console 內發現 Sitemap 停止更新，或者送出以後沒有更新，不用擔心。

根據 [Sitemap has not been read while everything is fine. - Google Search Central Community](https://support.google.com/webmasters/thread/309854712/sitemap-has-not-been-read-while-everything-is-fine?hl=en) 內專家的說法：

- Google Search Console 的 Sitemap 並非搜尋引擎必須去做檢查的。
- Google 搜尋引擎會嘗試每 90 天把網站內所有網頁都索引一次，且 Google 會更常索引首頁和知名的網頁。
- 可以致力於提升流量，增加爬蟲的數量。

需要時，可以用 URL Inspection 功能，手動為網頁加入 Google 索引。

### 安裝 Jekyll 的 SEO Tag 和 Sitemap 套件

如果有用 Jekyll 架站，以下兩個套件建議安裝：

- [GitHub - jekyll/jekyll-seo-tag](https://github.com/jekyll/jekyll-seo-tag) 可以為網頁加入搜尋引擎需要的標籤，GitHub Pages 支援這個套件。
- [GitHub - jekyll/jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap) 套件將網頁加入 Sitemap，讓 Google 搜尋引擎建立索引。GitHub Pages 也支援這個套件。

這兩個套件的說明可以參考 [幫部落格加上 SEO - GitHub Pages x Jekyll x Blog - KTing’s Blog](https://ktinglee.github.io/add-jekyll-seo/)，有些 Jekyll 的佈景主題預設會啟用這兩個套件。

上面有提到不要索引不重要的網頁，所以如果有建立第幾頁的分頁，可以參考以下兩個網頁，在 Jekyll Sitemap 套件內把分頁從 Sitemap 移除掉。不要等到被索引以後才處理，減少對索引品質的影響：

- [Exclude Pagination Pages · Issue #163 · jekyll/jekyll-sitemap](https://github.com/jekyll/jekyll-sitemap/issues/163 )
- [pagination - Removing Paginated URLs From jekyll-sitemap - Stack Overflow](https://stackoverflow.com/questions/74641804/removing-paginated-urls-from-jekyll-sitemap)

### 教學與經驗談


- [A Beginner's Guide to SEO optimization in a Jekyll static website · Juliette Sinibardy](https://jsinibardy.com/optimize-seo-jekyll#install-jekyll-seo-tag)：這裡面有提到一些改善 SEO 的作法，包含：
    - 減少網頁載入的時間，作法有：
        - 減少 Google Analytics 的連線時間
        - 減少影像大小
        - 使用內建的語法 Highlight 功能
        - 移除不需要的字型
    - 增加網頁的 Accessibility
    - 以及其它技巧
- [改善 SEO 的幾種方法 - GitHub Pages x Jekyll x Blog - KTing’s Blog](https://ktinglee.github.io/how-improve-jekyll-seo/)：如果是使用 Jekyll 架站的話，這些基本設定應該要先處理好！
- [突然想做一個嘗試，把我的網站外銷到 8 個國家🚀🚀🚀](https://codelove.tw/@howtomakeaturn/post/k31Rra)：這個系列是「站長阿川」對於網站調整 SEO 的一些心得和建議。