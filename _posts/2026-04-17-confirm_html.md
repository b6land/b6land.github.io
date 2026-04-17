---
layout: post
title: JavaScript `confirm()` 與 jQuery `.html()` 教學與範例
date: 2026-04-17 22:30:00 +0800
categories: [jQuery, JavaScript]
---  

在前端開發中，常會遇到兩種需求：

1. **需要使用者確認操作（例如刪除資料）**
2. **需要動態更新畫面內容**

這時候就會用到 JavaScript 的 `confirm()`，以及 jQuery 提供的 `.html()` 方法。以下將透過說明與範例，快速理解並實際應用。

## 使用 `confirm()` 建立確認對話框

`confirm()` 是 JavaScript Window.confirm API 的內建方法，可以顯示一個簡單的確認視窗，讓使用者選擇是否繼續操作。

### 基本用法

```javascript
var result = confirm("確定要執行此操作嗎？");

if (result) {
    console.log("使用者按下 OK");
} else {
    console.log("使用者按下 Cancel");
}
```

當對話框出現時：

* 點擊 OK → 回傳 `true`
* 點擊 Cancel → 回傳 `false`

---

### 注意事項與參考資料

- `confirm()` 會暫停 JavaScript 執行
- 對話框樣式由瀏覽器控制，無法客製化
- [Window confirm() Method](https://www.w3schools.com/jsref/met_win_confirm.asp)

## 使用 `.html()` 操作網頁內容

`.html()` 是 jQuery 中非常常用的方法，用來「取得或設定元素內的 HTML」。

### 基本語法

```javascript
$(selector).html();        // 取得內容
$(selector).html(content); // 設定內容
```

### 範例：設定 HTML

```javascript
$("#message").html("<b>操作成功！</b>");
```

畫面會變成：

```html
<div id="message"><b>操作成功！</b></div>
```

### 範例：動態更新狀態

```javascript
$("#status").html("載入中...");

setTimeout(function () {
    $("#status").html("<span style='color:green'>完成</span>");
}, 1000);
```

這種寫法常見於 AJAX 或非同步處理時，用來顯示進度。

### 範例：取得 HTML 內容

```javascript
var content = $("#message").html();
console.log(content);
```

### 注意事項與參考資料

1\. 會「完全覆蓋」原本內容。以下在 `#box` 裡的所有內容都會被取代。

```javascript
$("#box").html("<p>新內容</p>");
```

2\. 注意 XSS（跨站腳本攻擊）

如果直接將使用者輸入放入 `.html()`：

```javascript
$("#msg").html(userInput); // 不安全，會解析 HTML 標籤
```

可能會被插入惡意程式碼。建議改用：

```javascript
$("#msg").text(userInput); //  安全，只當純文字顯示 
```

3. [jQuery html() Method](https://www.w3schools.com/jquery/html_html.asp)

## 整合應用範例

將 `confirm()` 與 `.html()` 結合，可以做出簡單的互動功能：

```javascript
function deleteItem() {
    if (confirm("確定要刪除嗎？")) {
        $("#result").html("<span style='color:red'>已刪除</span>");
    } else {
        $("#result").html("已取消");
    }
}
```

使用者操作後，畫面會即時更新結果。

*(本文為 ChatGPT 從筆記大綱產生的文字，並經過人工檢查與編輯)*