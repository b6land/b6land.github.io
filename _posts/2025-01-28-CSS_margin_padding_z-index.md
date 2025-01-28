---
layout: post
title: CSS 的常用屬性
date: 2025-01-28 21:00:00 +0800
categories: [Other]
---  

這篇文章介紹 CSS 中設定元素間距、Z 軸順序的屬性。

### 設定元素的間距

`margin` : 產生元素周圍的空間，在元素的邊界之**外**。

- 可以獨立設定上下左右的空間，數值可指定為 auto、長度 (如 px)、% (百分比) 或 inherit (繼承父元素)。

`padding` : 產生元素周圍的空間，在元素的邊界之**內**。

- 一樣可以獨立設定上下左右的空間，數值可指定為長度 (如 px)、% (百分比) 或 inherit (繼承父元素)。

#### 參考資料

- [CSS Margin](https://www.w3schools.com/css/css_margin.asp)  
- [CSS Padding](https://www.w3schools.com/css/css_padding.asp)  
- [【CSS 教學】什麼時候該使用 margin 、padding？前端都該要懂的盒模型！ -方格子 vocus](https://vocus.cc/article/61527178fd89780001a967da)  

### 設定重疊元素的顯示順序

`z-index` : 設定 Z 軸的順序，當多個元素重疊時，越小的順序會顯示在越前面。

- 適用於 positioned elements 和 flex elements。
- 如果兩個元素沒有指定 z-index，在 HTML 程式碼最後方的，會顯示在頂端。

#### 參考資料

- [CSS z-index property](https://www.w3schools.com/cssref/pr_pos_z-index.php)