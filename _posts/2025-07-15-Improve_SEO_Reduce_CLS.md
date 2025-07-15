---
layout: post
title: 加強 SEO 的經驗談：避免版面位移，加強無障礙設定
date: 2025-07-15 21:00:00 +0800
categories: [Jekyll in GitHub Pages]
---

網站的版面位移和無障礙設定，都會影響 SEO 的成效。

### 版面位移：初次改善

從 Google Search Console 發現上百個網頁的 CWV (Core Web Vitals，網站核心指標) 都變成 Poor ：

![Core Web Vitals 變成 Poor](/assets/imgs/2025-07-15/cwv_poor.png)

如果 CWV 變成 Poor，就會影響到 SEO 的成效。因此我們可以用 [PageSpeed Insights](https://pagespeed.web.dev/) 來檢查，輸入網址後，首先可以看到前幾天從 「Chrome 使用者體驗報告」收集的資料：

![Chrome 使用者體驗報告總覽](/assets/imgs/2025-07-15/exp_overview.png)

然後在下面的診斷效能問題，可以看到效能比較差，問題就出在 CLS，也會指出原因：

![CLS 問題](/assets/imgs/2025-07-15/exp_cls.png)

CLS (Cumulative Layout Shift) 指的是版面突然移動，導致瀏覽者看到的資料或可以互動的元件移走，對瀏覽者來說有體驗不佳的問題。

PageSpeed Insights 會列出發生問題的元素，通常可以透過幾種方式解決：

1. 為圖片指定大小，一進入網頁時就先預留圖片空間，避免文字排版突然移動。
2. 檢查套用網路字型時，是否會導致排版變動。
3. 檢查廣告或其它會動態改變大小的內容，並限制大小。

這次先指定了網站的 Logo 圖片大小，並停用網路字型。修正完重新再跑一次測試 (用另外一個網頁當例子)：

![CLS 第一次改善](/assets/imgs/2025-07-15/exp_cls_overview_2.png)

看起來有所改善，回到 Google Search Console 驗證 CLS 問題，等待幾天來確認是否有改善。

### 版面位移：第二次檢查

過了約兩週後，發現 CLS 問題還是存在，驗證失敗。

但是用 PageSpeed Insights 觀察幾個網址，CLS 的數值都是 0，發生了什麼問題呢？

重新看一次官方說明，發現一個章節「找出載入後的 CLS 問題」有提到可以用瀏覽器的效能面板檢查 CLS。於是開啟 Edge 瀏覽器，清除快取後 (避免快取圖片影響載入結果) 用開發人員工具 (F12) 的效能面板測試：

![用開發人員工具檢查 CLS](/assets/imgs/2025-07-15/dev_cls.png)

觀測到了 CLS 問題的原因，因此根據裡面提供的原因，重新再指定廣告 (和影像) 的大小。然後再回到 Search Console 重新驗證。

(如果有使用 Google Adsense 自動廣告的話，自動廣告也有可能是 CLS 的元兇之一，建議可以停用後測試 CLS 看看。)

手動按下「驗證」兩個星期後，CLS 問題已經解決了~

![CLS 問題已解決](/assets/imgs/2025-07-15/cls_improved.png)

### 無障礙設定

另外，PageSpeed Insights 還提供了幾個無障礙設定的建議，我實作了以下兩項：

1. 在網站的 html 標籤裡指定 lang 屬性，例如 `<html lang="zh-TW">`，讓瀏覽器可以正確地顯示文字。
2. 在 Viewport 內的 `maximum-scale=5.0`  建議是 5，讓手機等小型裝置可以縮放內容。

### 參考資料

- 官方說明：[改善累計版面配置位移 - Articles - web.dev](https://web.dev/articles/optimize-cls?hl=zh-tw)
- CLS 的介紹：[CLS問題：1個改善範例學會Layout Shift的意思](https://zanzan.tw/archives/29496)
- 如何調整 Adsense 廣告造成的 CLS：[AdSense與CLS(累積布局偏移) - 祁勁松的博客👨‍💻](https://jamesqi.com/zh-hant/%E5%8D%9A%E5%AE%A2/AdSense%E4%B8%8ECLS%28%E7%B4%AF%E7%A7%AF%E5%B8%83%E5%B1%80%E5%81%8F%E7%A7%BB%29)
- HTML lang 屬性：[How The HTML Lang Attribute Helps Accessibility](https://www.boia.org/blog/how-the-html-lang-attribute-helps-accessibility )
- 關於 Viewport：[Mobile Web 前端技術筆記(一): Viewport的設定 - 烏托比亞](https://hsinyu00.wordpress.com/2011/04/05/mobile-web-viewport/ )
- 「Chrome 使用者體驗報告」又稱 CrUX，是從 Chrome 瀏覽器使用者收集網站的實際造訪情形，詳細可參考：[為什麼 CrUX 資料與我的 RUM 資料不同？  -  Articles  -  web.dev](https://web.dev/articles/crux-and-rum-differences?hl=zh-tw )
- [How to fix Cumulative Layout Shift in Adsense anchor ads - MatheusMorais.dev](https://matheusmorais.dev/blog/fixing-cls-anchor-ads-adsense )