---
layout: post
title: 策略模式筆記
date: 2022-01-03 12:00:00 +0800
categories:  [Design Pattern]
--- 

本篇以條列式的方式，簡單描述策略模式的幾個重點。

### 特色

- 藉由定義公用的 Interface，規範不同策略類別如何實作，並達到容易替換的效果。
- 產生多個類別，減少 if-else 的長度。
- 可搭配簡單工廠模式一同使用。

### 缺點

- 會產生很多類似的類別。因此可以搭配其它模式使用，如工廠模式。
- 適用在常常需要變動的模組，若內部不太再需要變動，則不需要使用策略模式增加複雜度。

### 使用的時機

- 單元測試注入假物件時。
- 同樣行為，但使用不同的程式碼實作，例如各種繪畫的行為：實心方形、空心方形、實心圓形...。

### 參考資料

[[ Day 3 ] 初探設計模式 - 策略模式 (Strategy Pattern)](https://ithelp.ithome.com.tw/m/articles/10202506)

[[30天快速上手TDD][Day 17]Refactoring - Strategy Pattern - In 91 - 點部落](https://dotblogs.com.tw/hatelove/2013/01/02/learning-tdd-in-30-days-day17-refactoring-with-strategy-pattern)