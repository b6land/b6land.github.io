---
layout: post
title: 如何對 JavaScript 除錯
date: 2024-02-21 12:00:00 +0800
categories:  [JavaScript]
--- 

最近在學習追蹤 ASP 網頁中的 JavaScript，若想要觀察程式的運作情形，以下有幾種方式可供參考。

### 除錯 JavaScript 的幾種方式

1. 在運作時，Edge 瀏覽器 > F12 開啟「網頁開發工具」(開發人員工具)，點選其中的主控台。如果有問題時會顯示訊息在主控台中。
2. 在 JavaScript 的程式碼中，加入 console.log() 語法。
3. 在 ASPX (或 ASCX) 內加入 `debugger;` 描述， 搭配「網頁開發工具」除錯。
4. 使用 jsFiddle 可以直接測試 JavaScript 的執行結果，也可以分享給其他人。

- [出了什麼問題？JavaScript 疑難排解 - 學習該如何開發 Web - MDN](https://developer.mozilla.org/zh-TW/docs/Learn/JavaScript/First_steps/What_went_wrong)
- [How to debug the javascript in asp.net?](https://stackoverflow.com/questions/11982564/how-to-debug-the-javascript-in-asp-net)  
- [介紹好用工具：jsFiddle - Online Editor for the Web - The Will Will Web](https://blog.miniasp.com/post/2011/02/07/Useful-tool-jsFiddle-Online-Editor-for-the-Web)
- 也請參考拙作：[在 jsFiddle 測試 jQueryUI](/JQueryUI_JSFiddle/)

### 使用網頁開發工具

使用 Edge 瀏覽器作為範例。

1. 將 Debugger 關鍵字放在需要進入中斷點的位置。
2. 開啟瀏覽器，並開啟「網頁開發工具」 (通常是 F12)。
3. 切換到網頁開發工具的「來源」頁面。
4. 然後開啟想要除錯的頁面 (才會正確載入原始碼)。
5. 若為 UserControl 裡的 JavaScript ，可能抓不到原始碼，沒辦法進入中斷點。

- [c# - Break point is not hitting in javascript file - Stack Overflow](https://stackoverflow.com/questions/30890383/break-point-is-not-hitting-in-javascript-file)

### 注意快取

要小心修改 JavaScript 程式碼後，因為舊的程式被快取，而沒有載入修改後程式的問題。此問題可以在 Edge 瀏覽內，透過勾選「網頁開發工具 > 網路 > 停用快取」解決。

![網頁開發工具 > 網路 > 停用快取](/assets/imgs/2024-02-21/disable_cache.png)

- [javascript - Dynamic script file remains out of date - Stack Overflow](https://stackoverflow.com/questions/56706595/dynamic-script-file-remains-out-of-date)