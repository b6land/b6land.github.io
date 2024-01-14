---
layout: post
title: 在 jsFiddle 測試 jQueryUI
date: 2024-01-14 12:00:00 +0800
categories:  [jQuery]
--- 

jsFiddle 是一個可以測試網頁執行效果的網站，本篇記錄使用該網站的方式 (含挑選 jQueryUI 選項)。

### jsFiddle 的使用方式

![jsFiddle](/assets/imgs/2024-01-14/jsFiddle.png)

1. HTML 欄位可以填入網頁中的 Body 標籤內容。
2. CSS 欄位可以填入樣式 (通常是 Style 標籤內容)。
3. JavaScript 處填入 JavaScript (通常是 Script 標籤內容)。
4. 按下左上角的 Run 可以取得目前內容在右下角的預覽結果。

![想使用的 Framework 組合](/assets/imgs/2024-01-14/pick_framework.png)

5. JavaScript 處可以選擇想要使用的 Framework 和擴充套件。如果有應用 jQueryUI 的語法，可選擇 jQuery 1.9.1 ，並開啟下方的 jQuery UI 1.9.2。(其它版本的 jQuery 不一定可使用 jQuery UI)