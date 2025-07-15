---
layout: post
title: C# 從靜態類別改成動態注入的效能影響
date: 2025-07-11 23:00:00 +0800
categories: [C#]
--- 

以下是重構時，想知道將 static 類別改成一般類別，並用依賴注入模式注入資料庫存取，對效能影響的調查。

### 依賴注入的效能影響 (ASP.NET Core)

建立一個透過 new Service() 取得實例的 Controller，另一個則透過 Constructor Injection 注入同樣 Service 的 Controller。在 Release 模式下使用 K6 進行壓力測試的結果：

- 使用 new 呼叫的 Controller 平均耗時約 184.94 μs
- 使用 DI 呼叫的 Controller 平均耗時約 186.07 μs

在大部分的情況下都可以被忽略。

[design patterns - Does dependency injection in ASP.NET Core add performance overhead? - Stack Overflow](https://stackoverflow.com/questions/69708519)  

### 一般類別和 static 類別的效能差異 (C#)

靜態方法呼叫比建立實例並呼叫快 4~5 倍。不過差異通常只是「幾十奈秒」而已，除非呼叫數百萬次循環，否則不易察覺。  

更好的折衷是：對於需要重複使用的方法，可以將物件實例化一次，重複使用實例，而不是每次都 new。

[oop - Performance of using static methods vs instantiating the class containing the methods - Stack Overflow](https://stackoverflow.com/questions/202631 )  