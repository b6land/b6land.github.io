---
layout: post
title: Singleton 簡介
date: 2020-02-09 12:00:00 +0800
categories:  [C#, Design Pattern]
---

Singleton 是一種軟體的設計模式。

- 該類別在程式執行期間，只容許一個 Instance (實例) 存在。
- 須包含檢查 Instance 是否存在的程式、產生 Instance 的程式，以及提供 `Public` 方法供外部程式存取 Instance。

## 優點

- 和 Static Class 相比，保留較多的特性，如：可被封裝與繼承，可被動態產生等。

### 優點的參考資料
- [Design Pattern (G4) - Singleton Pattern](http://limitedcode.blogspot.com/2015/07/design-pattern-singleton-pattern.html)
- [JWorld@TW Java論壇 - Re:Singleton vs. 靜態類別有什麼差別](https://www.javaworld.com.tw/jute/post/view?bid=44&id=53503&sty=0&tpg=5&ppg=1&age=0)

## 缺點
- 等於是全域變數。在任何類別都能存取 Instance。當發生錯誤時，不容易找出是哪一次的存取導致的。
- 同上，無法保證每次執行同樣方法時，都取得同樣的回傳值。

### 缺點的參考資料
- [Global State 是什麼？Singleton 為何讓人抓狂？ - Yushan - Medium](https://medium.com/@yushann/6d81658fff8e)
- [為什麼不要用Singleton - YANBIN HUNG - Medium](https://medium.com/@hung_yanbin/2de2043a1dbf)

## 在 C# 中使用 `sealed` 關鍵字建立類別
- 由於建構子是 `private` 的，因此若有類別欲繼承，將會在編譯時發生錯誤。
- 若將繼承的類別放置於原有的類別內，成為一個巢狀類別，則可能導致多個 Instance 產生。
- 因此使用 `sealed` 限制該類別不能被繼承。

### 參考資料
- [Why Singleton Class Sealed in C# - Dot Net Tutorials](https://dotnettutorials.net/lesson/singleton-class-sealed/)

### 如何寫出易於維護的程式
- [為什麼要用 Dependency Injection? （for Android Developer）](https://medium.com/@hung_yanbin/e7b65704a5ac)