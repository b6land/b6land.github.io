---
layout: post
title: 程式設計典範簡介
date: 2023-12-03 12:00:00 +0800
categories:  [Programming, Interview]
--- 

程式設計典範表示了程式語言如何執行，可以和無法使用的技術。雖然有部分程式語言完全遵守單一的設計典範，但也有一些程式語言混和了兩種或以上的設計典範。

## 宣告式程式設計 (Declarative programming)

不列出計算結果需要的指令，只寫出結果需要符合何種邏輯。這個分類下包含領域特定語言、函數式程式設計等典範。
和指令式程式設計通常是相對的。

### 領域特定語言 (Domain-specific languages)

針對特定的領域而撰寫的程式語言，使其更容易處理該領域的問題。
這些語言可能不包含控制流程的操作、不能和使用者互動。

例如常見的 HTML 標記語言 (網頁設計領域)、SQL 語言 (用於資料操作)。SQL 的範例如下：

``` sql
SELECT ID, Memo, Date
FROM Note
WHERE Date BETWEEN '1992-01-01' AND '1992-12-31'
```

### 函數式程式設計 (Functional programming)

函數式程式設計的程式以函數構成，呼叫函數並回傳計算結果，且避免程式執行狀態 (要做哪些事) 與變數物件 (數值被指定就不能再被改變)。通常這樣設計的程式語言適合用來處理資料。

此外，在 Java/C# 中的 Lambda 語法，概念也來自於函數式程式設計，可參考 [Java 開發者的函數式程式設計（1）初探函數式程式設計](https://openhome.cc/Gossip/CodeData/JavaLambdaTutorial/FunctionalProgramming.html)。

此典範的代表性程式語言是 HASKELL。可以透過 HASKELL 語言的教學，一覽純函數式程式設計語言的運作方式與特性：[HASKELL 趣學指南](https://learnyouahaskell.mno2.org/)。從 HASKELL 趣學指南擷取的範例如下：

```hs
circumference' :: Double -> Double  
circumference' r = 2 * pi * r

ghci> circumference' 4.0  
25.132741228718345
```

## 指令式程式設計 (Imperative programming)

程式內包含多個指令，依照指令來改變變數的狀態。這個分類下包含程序式程式設計、物件導向程式設計等典範。
和宣告式程式設計相對。

### 程序式程式設計 (Procedural programming)

程序式程式設計的概念是副程式 (subroutine 或 function) 的使用，副程式內有多個指令，並包含控制流程。
代表性的語言為 Basic 和 C 語言。

```c
bool isLeapYear(int year){
    if(year % 400 == 0){
        return true;
    }
    else if(year % 100 == 0){
        return false;
    }
    else if(year % 4 == 0){
        return true;
    }
    return false;
}
```

### 物件導向程式設計 (Object-oriented programming)

物件導向程式設計以類別為基本單位。類別將相關的資料與邏輯封裝在所產生的物件，因此更容易被重複利用，並具有模組的概念，便於擴充與替換。

知名的物件導向程式語言包含 C++、C# 和 Java 等。以下是一個 C# 類別的範例。

```cs
public class Car{
    private int speedKm;
    private string name;    

    public void Drive(){
        speedKm = 50;
    }
    
    public void Stop(){
        speedKm = 0;
    }

    public void SetName(string name){
        this.name = name;
    }

    public string GetName(){
        return name;
    }
}
```


### 維基百科的簡介

[Programming paradigm - Wikipedia](https://en.wikipedia.org/wiki/Programming_paradigm)：介紹常見的程式設計典範與其特性 (英文)。