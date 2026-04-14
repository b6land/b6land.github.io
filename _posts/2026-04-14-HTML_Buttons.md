---
layout: post
title: HTML Button 小知識
date: 2026-04-14 21:30:00 +0800
categories: [Other]
---  

這篇文章介紹 Button 的類型與如何對齊。

### Button 的類型

HTML 按鈕的類型有三種：


| type | 功能  |
| --- | --- |
| `button` | 一般按鈕，不會自動送出表單<br> |
| `submit` | 送出表單<br> |
| `reset` | 重設表單欄位<br> |

範例：

```xml
<button type="button">普通按鈕</button>
<button type="submit">送出</button>
<button type="reset">重設</button>

```

`<button>` 的預設 type 是 `submit`，所以會送出表單。

### 與 Input 的不同

`<button type="submit">`  可以有更多的修改彈性，例如放入 HTML、放入影像，而 `<input type="submit">`  不行。

以下是影像按鈕的範例：

```xml
<button type="submit">
  <img src="send.png">
  Send
</button>

```

<br>

### 如何對齊按鈕

如何對齊按鈕呢? 以下有幾種作法，用按鈕置右為例：


1\. 使用 CSS 的  `float:right`  屬性：浮動的元素與原本的文件區塊無關，所以按鈕可以移動到最右方，缺點是會跑版。

```css
button {
  float: right;
}

```

2\. 用 `text-align` 對齊：

```xml
<div style="text-align:right">
    <button>Submit</button>
</div>

3\. 使用 CSS 的 `flex`  屬性：

```css
.container {
  display: flex;
  justify-content: flex-end;
}

```

搭配的 HTML 如下：

```csharp
<div class="container">
  <button>Submit</button>
</div>

```

因為是針對父元素去對齊，因此不會跑版，是比較推薦的做法。

### 參考資料

- [Attribute for TYPE = BUTTON - SUBMIT - RESET](https://html.com/attributes/button-type/)
- [Place a button right aligned](https://stackoverflow.com/questions/6632340/place-a-button-right-aligned)
- [按鈕類型與排版擴充](https://w3c.hexschool.com/flexbox/a5e0a54c)