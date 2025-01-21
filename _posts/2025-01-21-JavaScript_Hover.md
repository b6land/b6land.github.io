---
layout: post
title: JavaScript 新增 Hover CSS 屬性
date: 2025-01-21 22:00:00 +0800
categories: [JavaScript]
---  

CSS 中有 hover 屬性，當滑鼠移動到元件上時，可以用這個屬性設定不同的效果，達到凸顯焦點的目標。如果需要用 JavaScript 控制 CSS 的 Hover 屬性時，則可設定以下的方法。

### 內容

JavaScirpt 中對應的方法如下：

- onmouseover: 移入
- onmouseout: 移出

撰寫對應的 function，例如：

```javascript
// 設定滑鼠停留按鈕時樣式
function btnMouseOver(ctl) {
    ctl.className += ' ui-state-hover';
}

// 設定滑鼠移出按鈕時樣式
function btnMouseOut(ctl) {
    ctl.className = ctl.className.replace(' ui-state-hover', '');
}
```

按鈕 HTML 語法如下：

```html
<input type='button' onmouseover='btnMouseOver(this);' onmouseout='btnMouseOut(this);' style='float:right;' class='ui-button ui-state-default ui-corner-all ' value='測試'/>
```

CSS 樣式如下：  

```css
/* 基本按鈕樣式 */
.ui-button {
    background-color: #f7f7f7;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 5px 10px;
    font-size: 14px;
    color: #333;
    cursor: pointer;
    transition: background-color 0.3s, box-shadow 0.3s;
}

/* 預設狀態 */
.ui-state-default {
    background-color: #e6e6e6;
}

/* 圓角 */
.ui-corner-all {
    border-radius: 5px;
}

/* 滑鼠停留樣式 */
.ui-state-hover {
    background-color: #d9edf7;
    border-color: #bce8f1;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
    color: #31708f;
}
```

### 註記

**優先選擇 CSS**：如果只是簡單的視覺效果變化，應盡量使用 CSS 的 `:hover` ，而非 JavaScript。這樣可以讓程式碼更簡潔，效能更佳，且容易維護。

### 參考資料

- [html - How to simulate css hover pressing a button with javascript? (no jquery) - Stack Overflow](https://stackoverflow.com/questions/23082138/how-to-simulate-css-hover-pressing-a-button-with-javascript-no-jquery)