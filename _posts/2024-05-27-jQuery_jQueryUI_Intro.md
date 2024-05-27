---
layout: post
title: jQuery 和 jQueryUI 的簡介
date: 2024-05-27 10:00:00 +0800
categories:  [jQuery]
--- 

jQuery 和 jQuery UI 可以快速地建立網站，常常用在官方網站、內部管理系統網站上。儘管推出已久，效能不算太高，仍是眾多開發者的選擇。

### 什麼是 jQuery

![jQuery 官網介紹](/assets/imgs/2024-05-27/jQuery_Homepage.png)
△ jQuery 官網介紹

jQuery 是一個功能豐富的 JavaScript 函式庫，目標是提升使用 JavaScript 的體驗。

它提供了更方便的選擇器 (Selector)，可以輕易地操作 DOM，例如：`$('#elementID').hide();` 可以輕鬆地隱藏帶有特定 ID 的元素。

jQuery 簡化了 JavaScript 的事件處理機制，例如 `$('#button').click(function() { alert('Button clicked!'); });`。

此外，jQuery 相容於大多數的主流瀏覽器，如 Google Chrome, Firefox 等。

### 什麼是 jQuery UI

![jQuery UI 官網的日期選擇器範例](/assets/imgs/2024-05-27/jQuery_UI_Datepicker.png)
△ jQuery UI 官網的日期選擇器範例

jQuery UI 是基於 jQuery 的擴充函式庫，提供許多可立即使用的 UI 控制項與動畫效果，例如日期選擇器 (Datepicker)、功能表 (Menu)... 等。

例如：使用日期選擇器 `$( "#datepicker" ).datepicker();`，可以將一個文字框轉換為日期選擇器。

另外，jQuery UI 也很容易實作網頁元素的拖曳、動畫功能，也能快速套用不同的佈景主題。

### 參考資料

- [jQuery是什麼，它跟JavaScript有什麼關係？它又有什麼能耐呢？](https://progressbar.tw/posts/6)
- [實用網頁工具庫 - jQuery UI (上) 元件篇 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10120587) *(註：該作者沒有寫下集)*
- [從「鄙視 jQuery」聊起 -技術鄙視從何而來？-黑暗執行緒](https://blog.darkthread.net/blog/jquery-dispise/)