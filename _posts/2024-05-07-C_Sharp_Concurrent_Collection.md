---
layout: post
title: C# 多執行緒用 Concurrent Collection 安全存放資料
date: 2024-05-07 21:00:00 +0800
categories:  [C#]
--- 

想避免多執行緒的資料錯誤，導致身陷除錯地獄，可以使用 Concurrent Collection。

### 什麼是 Concurrent Collection

以多個執行緒不斷的新增、移除同一個集合內的資料，可能會發生資料錯亂。

可以改用 `System.Collections.Concurrent`  內提供的集合，具備 Thread-Safe 的特性，可以避免資料在多執行緒的操作下發生錯誤。這些集合分別提供了一般常見的 List, Dictionary 和其他資料結構的 Thread-Safe 版本：[ConcurrentBag<T>](https://learn.microsoft.com/zh-tw/dotnet/api/system.collections.concurrent.concurrentbag-1?view=net-8.0)、[ConcurrentDictionary<TKey,TValue>](https://learn.microsoft.com/zh-tw/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=net-8.0)。

相關的介紹：

1. [集合物件的多執行緒存取注意事項-黑暗執行緒](https://blog.darkthread.net/blog/concurrentdictionary/)
2. [\[C#\]\[.NET\]Concurrent collections - Miles MS.HelloWorld - 點部落](https://dotblogs.com.tw/mileslin/2016/03/13/150234)


### 常用的語法

`TryAdd(TKey, TValue)`  : 嘗試加入物件，回傳加入成功或失敗，參數為要加入的物件。

`TryRemove(TKey, TValue)`  : 嘗試移除物件，回傳移除物件成功或失敗，成功時參數會傳回被移除的物件，否則回傳該類別的預設值。

其它語法可參考：[ConcurrentDictionary<TKey,TValue> 類別 (System.Collections.Concurrent) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.collections.concurrent.concurrentdictionary-2?view=net-8.0 )

### 使用 LINQ 查詢 Concurrent Collection，是 Thread-Safe 的嗎 ?

根據最佳回答提到的以下兩點，可以確認是 Thread-Safe：

- Concurrent Collection 確保其修改都是 Thread-Safe。
- LINQ 通常不會在查詢期間修改集合 (除非自己在查詢條件內修改資料)。

問題：[c# - Are linq operations on concurrent collections thread safe? - Stack Overflow](https://stackoverflow.com/questions/27569747/are-linq-operations-on-concurrent-collections-thread-safe)