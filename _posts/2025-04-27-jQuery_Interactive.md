---
layout: post
title: jQuery 的元件點選事件
date: 2025-04-27 22:00:00 +0800
categories:  [jQuery]
--- 

在使用 jQuery 開發網頁時，理解並掌握各種互動方法很重要。以下是三種常見的滑鼠與元件互動的事件：hover、click 與 focus。

### 說明

#### hover（滑鼠停留）

當使用者的滑鼠指標移入或移出某個元件時，會觸發 hover 事件。jQuery 提供了 `.hover()`  方法，允許開發者分別指定滑鼠進入和離開時的處理函式。例如：  

```javascript
$(selector).hover(
  function() { /* 滑鼠進入時的處理 */ },
  function() { /* 滑鼠離開時的處理 */ }
);
```

<br>

這種事件常用於提供即時的視覺回饋，如變更背景色或顯示提示訊息。

<br>

請參考 [.hover() - jQuery API Documentation](https://api.jquery.com/hover/) (現在推薦使用 [mouseenter event - jQuery API Documentation](https://api.jquery.com/mouseenter/ ) 和 [mouseleave event - jQuery API Documentation](https://api.jquery.com/mouseleave/ ) 事件)。  

#### click（滑鼠點擊）

當使用者點擊某個元件時，會觸發 click 事件。使用 jQuery 的 `.click()`  方法可以輕鬆地為元件綁定點擊事件處理函式：  

```javascript
$(selector).click(function() {
  // 點擊時的處理
});
```

這在處理按鈕點擊、表單提交或切換內容顯示等情境中特別有用。  

請參考 [.click() - jQuery API Documentation](https://api.jquery.com/click-shorthand/) (現在推薦使用 [click event - jQuery API Documentation](https://api.jquery.com/click/#on1 ) 事件)。

#### focus（獲得焦點）

當元件（如輸入框）獲得焦點時，會觸發 focus 事件。jQuery 提供了 `.focus()`  方法來處理。此外，`.focusin()`  和 `.focusout()`  方法分別對應於元件獲得和失去焦點的情況。例如：  

```javascript
$(selector).focus(function() {
  // 元件獲得焦點時的處理
});
```

可用於如在輸入框獲得焦點時強調顯示、顯示提示文字。  

請參考 [.focus() - jQuery API Documentation](https://api.jquery.com/focus-shorthand/ "https://api.jquery.com/focus-shorthand/") (現在推薦使用 [focus event - jQuery API Documentation](https://api.jquery.com/focus/#on1))，另外也可參考 [javascript - jquery how to remove ui-state-focus - Stack Overflow](https://stackoverflow.com/questions/15359665/jquery-how-to-remove-ui-state-focus)。  

綜合運用這些事件處理方法，可以大幅提升網頁的互動性和使用者體驗。

### 範例

以下是一個簡單的 HTML 範例，展示了 jQuery UI 中 hover、click 與 focus 三種事件的基本應用：

```xml
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>jQuery UI 事件範例</title>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <style>
    .box {
      width: 200px;
      height: 100px;
      background-color: lightgray;
      line-height: 100px;
      text-align: center;
      margin-bottom: 20px;
      cursor: pointer;
    }

    .hovered {
      background-color: lightblue;
    }

    .clicked {
      background-color: lightgreen;
    }

    input:focus {
      border: 2px solid orange;
    }
  </style>
</head>
<body>

  <h2>jQuery UI 互動事件範例</h2>

  <!-- hover 範例 -->
  <div id="hoverBox" class="box">滑鼠移入/移出</div>

  <!-- click 範例 -->
  <div id="clickBox" class="box">點擊我</div>

  <!-- focus 範例 -->
  <input type="text" placeholder="點擊輸入框以觸發 focus" style="padding: 8px; width: 250px;">

  <script>
    // hover 事件
    $('#hoverBox').hover(
      function() {
        $(this).addClass('hovered');
      },
      function() {
        $(this).removeClass('hovered');
      }
    );

    // click 事件
    $('#clickBox').click(function() {
      $(this).toggleClass('clicked');
    });

    // focus 事件（已用 CSS 處理）
    $('input').focus(function() {
      console.log('輸入框獲得焦點');
    });
  </script>
</body>
</html>
```

(本篇部分程式碼與文字由 ChatGPT 產生，並由人工校對)