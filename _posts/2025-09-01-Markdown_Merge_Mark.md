---
layout: post
title: Markdown 的特殊功能：合併表格、強調
date: 2025-09-01 23:00:00 +0800
categories: [Other]
--- 

Markdown 是很方便的標記語言，也能夠建立基本的表格。在部分支援 Markdown 的平台，可以用一些特別的語法產生更複雜的表格和畫線強調！

### 合併表格的儲存格

![水平、垂直合併儲存格](/assets/imgs/2025-09-01/table_merge.png)

我們常常會有合併儲存格的需要，如果平台有啟用 MultiMarkdown，可以透過以下方式合併。

輸入 `^^`  垂直合併儲存格：

```plaintext
| Title 1 | Title 2 |
| --- | --- |
| Row 1 | Content |
| ^^ | Content |
| Row 2 | Content |
```

將兩個 `|`  分隔符號間的空白去除，可以水平合併儲存格：

```csharp
| Title 1 | Title 2 | Title 3 |
| --- | --- | --- |
| Row 1 | Content | Note |
| Row 2 | Content ||
| Row 3 | Content | Note |
```

也可以參考這篇，採用不同的做法繪製表格：[github - Can I merge table rows in markdown - Stack Overflow](https://stackoverflow.com/questions/46621765/can-i-merge-table-rows-in-markdown )  

使用 wiki.js 時，需要在設定的轉譯裡，啟用 MultiMarkdown 表格，才能顯示合併儲存格的表格。

[Markdown tables show properly in editor, but not on live page · requarks/wiki · Discussion #5378 · GitHub](https://github.com/requarks/wiki/discussions/5378 )  

### 強調某段文字

可以在文字前後加入 `==`  ，例如 `==被強調的文字==` ，來強調文字。

這不是 Markdown 原生支援的功能，若在部分環境上無法產生效果，可以改用 HTML 標籤 `<mark>被強調的文字</mark>`  代替。

效果如下：

<mark>被強調的文字</mark>