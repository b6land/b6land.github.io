---
layout: post
title: jQuery 條列式基礎介紹
date: 2023-06-14 12:00:00 +0800
categories:  [jQuery]
--- 

本篇文章用條列式整理了 W3Schools 內 jQuery 測驗的大部分內容，可作為基礎入門參考。文末收錄幾個不錯的教學網頁。

### 內容

1. JQuery 是 JavaScript 函式庫，在 Client 端運行，可以和 AJAX 一同使用。
2. 預設的 `jQuery.ajax()` 即為非同步作業。
3. 使用 CSS 選擇器選擇元素。
4. 使用 `$` 作為存取元素的捷徑。`$("div")` 會取得所有 div 標籤。
5. 使用 `.css()` 選擇樣式，如 `$("p").css("background-color","red");` 可以將所有 p 元素的背景色改為紅色。
6. `$("div.intro")` 會選取所有 class = intro 的 div 元素。
  `$("div p")` 會選取div 元素裡面的所有 p 元素。
  `$("p#intro")` 會選取 id = intro 的 p 元素。
7. 可以使用 `hide()` 和 `show()` 隱藏和顯示元素。
8. `$("div").height(100)` 將所有 div 元素高度設為 100。
9. 從 Google 引用 jQuery 函式庫。
10. 使用 `noConflict()` 避免命名衝突。
11. `toggleClass()` 可以在新增或移除特定標籤的狀態間切換。
12. `detach()` 和 `remove()` 都可以移除選擇的元素。
13. `$(":disabled")` 會選擇所有停用的 input 元素。
14. `parent()` 可用來選擇上一層的元素。
15. `animate()` 可用在數值類型的 css 屬性。

- 參考來源：[W3Schools Quizzes](https://www.w3schools.com/quiztest/default.asp)

### jQuery 教學網頁

- [\[技術探討\] jQuery 小而精的網頁函式庫 (一) - iTe2 Blog 詮力科技](https://blog.ite2.com/2014/09/09/jquery01/)
- [\[技術探討\] jQuery 小而精的函式庫 (二) - iTe2 Blog 詮力科技](https://blog.ite2.com/2014/10/11/jquery02/)
- [\[技術探討\] jQuery實作 小而精的網頁函式庫 (三) - iTe2 Blog](https://blog.ite2.com/2015/01/13/jquery03/)
- [Day10 \[jQuery\] - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10185668)


