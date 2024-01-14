---
layout: post
title: C# Interface, Abstract, Virtual 介紹
date: 2020-06-28 12:00:00 +0800
categories:  [C#, Interview]
--- 

本文介紹 Interface、Abstract、Virtual 的特性。

### Interface

- 用在組合 (Composition) 上面。
- 確保有特定的方法被子類別實作。

### Abstract

- 用在繼承 (Inheritance) 上面。
- 讓子類別維持和 Abstract 類別一樣的功能。
- 沒有實作程式內容。
- 一定要被 Override。

### Virtual

- 可以實作程式內容。
- 不一定要 Override，但是被 Override 後，可以呼叫 `base.method()`。
- 預先提供一部分的實作內容。
- 範例請參考 [C# 繼承 (Inheritance)](/C_Sharp_Inheritance/)

### 參考資料

- [(原創) interface和abstract class有何不同? (C/C++) (.NET) (C#) - 真 OO无双 - 博客园](https://www.cnblogs.com/oomusou/archive/2007/05/07/738311.html)