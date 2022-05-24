---
layout: post
title: C# 關鍵字 - params, lock
date: 2019-09-17 12:00:00 +0800
categories:  [C#]
---

本篇收錄 C# 關鍵字 params, lock的用法。

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

### 參考資料

[params 關鍵字 - C# 參考 - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/csharp/language-reference/keywords/params)

[[C#] 多執行緒(3) 互斥鎖 Mutex – Program – C.Y.C](https://yuchungchuang.wordpress.com/2018/07/24/c-%E5%A4%9A%E5%9F%B7%E8%A1%8C%E7%B7%923-%E4%BA%92%E6%96%A5%E9%8E%96-mutex/)