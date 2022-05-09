---
layout: post
title: R 語言雜記
date: 2021-01-31 12:00:00 +0800
categories: R
tags: [R Language]
--- 

這篇文章記錄 C# 如何加入由 R 語言撰寫的資料分析程式，以及 R 語言部分語法的說明。

### 將 R 語言加入至 C# 專案的心得

當初因專案需要，需要從 C# 專案呼叫團隊成員撰寫的 R 語言程式，以分析資料。因此當時調查了以下幾個程式：

- RSVT: RSVT (R Tools for Visual Studio) 和 RStudio 一樣，都是編輯和執行 R 語言的開發環境。RSVT 僅支援 2015 和 2017，不支援 VS 2019 (已停止開發)。
- RDotNet：可從外部呼叫整合 C# 專案和 R 語言的程式。可以使用 `source` 語法載入 R script。

### 參考連結

- [在 Visual Studio 中使用 R 語言進行進階資料分析 – 使用 R Tools for Visual Studio – MSDN 台灣部落格](https://blogs.msdn.microsoft.com/msdntaiwan/2016/03/24/rtvstw-overview/)
- [[C#]使用R.Net在C#內呼叫R語言 - 馬克-程式設計筆記 - 點部落](https://dotblogs.com.tw/marsxie/2019/05/27/232135)
- [R 資料輸入與輸出 - G. T. Wang](https://blog.gtwang.org/r/r-data-input-and-output/)

### 語法紀錄

- `rms()` 函式：取得均方根值，說明 : [Root Mean Square ](http://rug.mnhn.fr/seewave/HTML/MAN/rms.html)
- `sum(a, na.rm=TRUE)` ：表示移除掉 N/A 的值，並加總剩下的數值，可參考 : [R筆記–(3)套件與函式](https://rpubs.com/skydome20/R-Note3-function_and_package)