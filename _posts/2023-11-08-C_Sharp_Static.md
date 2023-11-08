---
layout: post
title: C# Static
date: 2023-11-08 12:00:00 +0800
categories: [C#]
---

C# 的 `static` 關鍵字，可以宣告類別或方法為靜態，那麼和一般的類別、方法有什麼不一樣呢？

### 關於 static

- 可以宣告靜態類別或方法，不必使用 new() 建立物件就能使用。
- 只能有一份實例 (Instance)，在應用程式啟動時即產生，直到程式結束。
- 難以套用多型物件，以及在運行中動態注入資料物件。

### Web 專案內使用 Static 的優缺點

- 可以保持只有一份實例，不需要 GC 機制。
- 但是比較適合公用程式或參數。
- 導致較難撰寫單元測試。

- 參考資料: [c# - Benefit of using static methods in web application - Stack Overflow](https://stackoverflow.com/questions/7338275/benefit-of-using-static-methods-in-web-application)