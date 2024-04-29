---
layout: post
title: Jekyll 網誌與 Google 搜尋
date: 2024-01-21 12:00:00 +0800
categories: [Jekyll in GitHub Pages]
---

利用 Jekyll 建立網誌後，如果想要確認被 Google 搜尋的狀況，可以跟著以下的步驟操作。

### 使用 Google Search Console

透過 Google Search Console (下稱 Search Console)，可以檢查網站內的網頁被搜尋到，以及被點擊的次數，並藉此檢視網站的哪些網頁最受歡迎。

想要使用 Search Console，首先要先證明自己是網站的擁有者。Search Console 提供許多不同的方法證明網站的擁有者，其中一種簡易的方法，是將 Search Console 提供的檔案置入在網站的根目錄下 \[2\]：

1. 下載提供的檔案。
2. 放在網站的根目錄下。
3. 上傳至伺服器。
4. 按下「驗證」的按鈕，即可開始使用 Search Console！

![放入 Search Console 提供的檔案](/assets/imgs/2024-01-21/google_specific_html.png)

▲ 放入 Google Search Console 提供的檔案到網站根目錄下，

接下來，可以等待 Google 的爬蟲 (web crawler，自動收集網頁資料的網路機器人) 慢慢找到你的網頁，並建立這個網站的索引，讓其它使用者可以查詢。

### Jekyll 加入 Sitemap

如果 Google 的爬蟲沒辦法查到網站的所有文章，除了手動為網頁建立索引，另一種方式則是建立 Sitemap。

Sitemap 可翻為網站地圖，為 XML 格式，可以列出網站內的所有網頁，供 Google 建立索引，而不必透過爬蟲。

如果網站是放在 GitHub Pages 的話，根據 [Dependency versions - GitHub Pages](https://pages.github.com/versions/ "https://pages.github.com/versions/") 的列表，已經有 jekyll-sitemap 元件，只要在 Jekyll 內設定 `config.xml` 的 url 屬性，該元件就會自動產生包含各網頁連結的 Sitemap。範例如下：

> url: "https://example.github.io"  

設定完成後 (並將設定值 commit 至 GitHub Pages)，應該可以在 https://example.github.io/sitemap.xml \[1\] 內看到產生的結果。

此時可以回到 Search Console，在 Sitemap 頁面加入剛剛設定好的 `sitemap.xml`，讓文章都可以被查詢到。

![加入 sitemap.xml 並生效的狀況](/assets/imgs/2024-01-21/sitemap_in_serach_console.png)

▲ 加入 `sitemap.xml` 並生效的狀況 

不過，每個人的網站索引速度不同，可能需要數天 Sitemap 才能生效，然後需要再一陣子才會將網頁編入索引內。

### 參考資料

- \[1\] [幫部落格加上 SEO - GitHub Pages x Jekyll x Blog - KTing’s Blog](https://ktinglee.github.io/add-jekyll-seo/)
- \[2\] [如何在Google搜尋到我的網站？Google Search Console教學以及SEO優化](https://blog.iddle.dev/public/2023/04/09/如何在Google搜尋到我的網站？Google-Search-Console教學以及SEO優化/)