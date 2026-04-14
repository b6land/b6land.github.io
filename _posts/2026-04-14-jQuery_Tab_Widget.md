---
layout: post
title: jQuery 使用 Tab Widget
date: 2026-04-14 22:30:00 +0800
categories: [jQuery]
---  

整理數年前的 jQuery Tab Widget 筆記。

### 基本用法

```xml
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>tabs demo</title>
  <link rel="stylesheet" href="https://code.jquery.com/ui/1.13.3/themes/smoothness/jquery-ui.css">
  <script src="https://code.jquery.com/jquery-3.7.1.js"></script>
  <script src="https://code.jquery.com/ui/1.13.3/jquery-ui.js"></script>
</head>
<body>
 
<div id="tabs">
  <ul>
    <li><a href="#fragment-1"><span>One</span></a></li>
    <li><a href="#fragment-2"><span>Two</span></a></li>
    <li><a href="#fragment-3"><span>Three</span></a></li>
  </ul>
  <div id="fragment-1">
    <p>預設 active 的第一頁</p>    
  </div>
  <div id="fragment-2">
    第二頁
  </div>
  <div id="fragment-3">
    第三頁
  </div>
</div>
 
<script>
$( "#tabs" ).tabs();
</script>
 
</body>
</html>

```

以上程式碼改寫自官方說明 _(最新版)_: [Tabs Widget | jQuery UI API Documentation](https://api.jqueryui.com/tabs/#option-active)，也可以在裡面看用法。

其它的參考資料如下：

1. 從 ASP.NET 專案開始，加入 JQuery UI Tab 元件: [JQuery UI Tab With ASP.NET Web Form](https://www.c-sharpcorner.com/UploadFile/2f59d0/jquery-ui-tab-with-Asp-Net-webform/)
2. 一步一步寫出含 Tab 元件的網頁: [使用Jquery特效做出Tab頁籤切換效果](https://www.webdesigns.com.tw/jquery_tab.asp)

### 保存或指定 active 的分頁

想保留或指定當時的 active 分頁，可以根據情境分成以下兩種作法：

1. Postback 以後，可以用 ASP 的 HiddenField 設定 Postback 後要停在哪個分頁：[Staying on current jQuery tab across post back?](https://stackoverflow.com/questions/3720796/staying-on-current-jquery-tab-across-post-back)
2. 情境 2: 發送 submit (將表單資訊傳送至伺服器) 後，可用的方法：指定 URL / cookie / 模擬 click，可參考：[Stay on a current tab after after submitting a form using jQuery](https://stackoverflow.com/questions/11560613/stay-on-a-current-tab-after-after-submitting-a-form-using-jquery)

### 選擇分頁 (1.8 版)

若要用程式選擇 Tab Widget 的分頁，請留意 jQuery 1.9 和較新版本使用 active，1.9 以前使用 selected，下方是範例：

```javascript
$('selector').tabs({ selected: index });
```

參考資料：[Set default tab in jQuery UI Tabs](https://stackoverflow.com/questions/4565128/set-default-tab-in-jquery-ui-tabs)

官方說明 (1.8 版): [Tabs Widget | jQuery UI 1.8 Documentation](https://api.jqueryui.com/1.8/tabs/)