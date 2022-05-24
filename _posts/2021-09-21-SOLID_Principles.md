---
layout: post
title: SOLID 開發原則
date: 2021-09-21 12:00:00 +0800
categories:  [Design Pattern]
--- 

本文介紹什麼是 SOLID 開發原則。盡可能地遵守這些原則，能寫出更容易維護與擴充的好程式。

### SOLID 原則介紹

SOLID 是以下五個字母的構成：

- **S - Single Responsibility Principle (單一職責)**：一個類別負責一件工作。
- **O - Open/Close Principle (開放/封閉原則)**：可以開放擴充新功能，且盡可能減少對現有程式碼的修改​。
- **L - Liskov Substitution Principle (Liskov 替換原則)**：子類別可以替換父類別，​而不會影響程式架構，子類別可以執行父類別想做的事。
- **I - Interface Segregation Principle (介面隔離原則)**：將不同功能分離至不同介面​。
- **Dependency Inversion Principle (依賴反轉原則)**：避免父類別因為子類別改變而被迫改變。
