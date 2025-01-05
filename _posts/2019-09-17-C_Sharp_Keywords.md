---
layout: post
title: C# 用 params 輸入參數、lock 確保物件獨佔
date: 2019-09-17 12:00:00 +0800
categories:  [C#]
---

本篇介紹 C# 中如何用 `params` 傳入多個參數, 如何使用 `lock` 與 `Monitor.TryEnter` 確保資源在多執行緒情形下是獨佔的，不會被同時修改。

### params 

使用 `params` 關鍵字，可以指定數量不固定的一維陣列引數作為方法參數。並且也可以直接在使用方法時用 , 分隔多個引數。使用方式可參考下方程式碼  (部分引用自 Microsoft Docs)

``` csharp
    public static void UseParams(params int[] list)
    {
        for (int i = 0; i < list.Length; i++)
        {
            Console.Write(list[i] + " ");
        }
        Console.WriteLine();
    }

    static void Main()
    {
        // You can send a comma-separated list of arguments of the 
        // specified type.
        UseParams(1, 2, 3, 4);

        // An array argument can be passed, as long as the array
        // type matches the parameter type of the method being called.
        int[] myIntArray = { 5, 6, 7, 8, 9 };
        UseParams(myIntArray);
```

### lock

`lock` 關鍵字的使用，可以避免不同執行緒同時存取同一個資源，導致資源同時被存取而引發的錯誤。

``` csharp
private object key = new object();

private void work()
{
    lock(key)
    {
        // 存取資源
    }
}
```

`key` 可使用任何型別的物件，它只會單純的當作一個鑰匙被看待。當不同執行緒在執行到 `lock` 時，會先檢查 `key` 是否被占用，若被占用，則等待到 `key` 被釋放後再繼續執行。

參考資料中的互斥鎖，透過存取變數副本的方法，可以增加多執行緒程式的效能。

### 同場加映：Monitor.TryEnter

在 [System.Therading.Monitor 類別](https://learn.microsoft.com/zh-tw/dotnet/api/system.threading.monitor?view=net-9.0)內，包含數個方法用於避免競逐狀態 (Race Condition)，其中的 `Monitor.TryEnter()`方法，可以確保物件在使用期間是被獨佔的。

和 `lock`  有什麼不一樣？

1. 用 `Monitor.TryEnter()` 可以指定等待取得鎖定物件的時間，而 `lock`  不行。
2. `lock`  語法等於用 `try...finally...`  區塊包覆 `Monitor.Enter()`  和 `Monitor.Exit()`  方法，語法較為簡潔。

### 參考資料

- [params 關鍵字 - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/params)
- [[C#] 多執行緒(3) 互斥鎖 Mutex – Program – C.Y.C](https://yuchungchuang.wordpress.com/2018/07/24/c-%E5%A4%9A%E5%9F%B7%E8%A1%8C%E7%B7%923-%E4%BA%92%E6%96%A5%E9%8E%96-mutex/)
- 官方說明
    - [Monitor.TryEnter Method (System.Threading) - Microsoft Learn](https://Monitor.TryEnter%20Method%20\(System.Threading\)%20-%20Microsoft%20Learn)
    - [Monitor.Enter Method (System.Threading) - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.threading.monitor.enter?view=net-8.0)
    - [The lock statement - synchronize access to shared resources - C# reference - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/lock)
- `lock`  和 `Monitor.TryEnter()`  的介紹：[\[筆記\]C# 鎖定-使用lock、Monitor.Enter、Monitor.TryEnter的小範例 - 遇見零壹魔王 - 點部落](https://dotblogs.com.tw/noncoder/2018/06/30/lock-Monitor)
