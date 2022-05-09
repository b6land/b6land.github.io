---
layout: post
title: 觀察者模式簡介
date: 2021-03-18 12:00:00 +0800
categories: DesignPattern
tags: [Design Pattern]
--- 

本篇文章將簡單介紹觀察者模式 (Observer Pattern) 的概念。

### 摘要

- 觀察者模式將資料的接收拆解，分為**發佈者**和**訂閱者**兩個角色。可以有多個訂閱者向由一個發佈者訂閱訊息，發佈者可以發送事件通知訂閱者，訂閱者可以隨時訂閱或取消訂閱事件通知。
- 透過撰寫兩個 Interface 建立觀察者模式，**發佈者**的 Interface 內可以包含訂閱者的清單 (List) ，並接受**訂閱者** 的 Interface 存取以下方法：

1. 訂閱的方法 (Subscribe)
2. 取消訂閱的方法 (Unsubscribe)
3. 通知的方法 (Notify)

- 當類別實作以上兩個 Interface 時，即完成基礎的觀察者模式。
- C# 的 Delegate、Event Handler 機制即實踐觀察者模式。
### 參考資料

- [Design Pattern - 只要你想知道，我就告訴你 - 觀察者模式（ Observer Pattern ） feat. TypeScript - by 神Q超人 - Enjoy life enjoy coding - Medium](https://medium.com/enjoy-life-enjoy-coding/8c15dcb21622)
- [觀念 設計模式–Observer Pattern - 我，傑夫。開發人](https://jeffprogrammer.wordpress.com/2015/07/23/淺談設計模式-observer-pattern/)
- [[C#]Observer Pattern到Delegate和Event - 全端開發人員天梯 - 點部落](https://dotblogs.com.tw/wellwind/2016/05/22/csharp-observer-pattern-delegate-event)