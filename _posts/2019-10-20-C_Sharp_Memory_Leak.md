---
layout: post
title: C# 中常見的 Memory Leak
date: 2019-10-20 12:00:00 +0800
categories:  [C#]
---

本文介紹 C# 中記憶體洩漏的來源，以及當來源是 Interop 時，要如何處理。
另外也包含常見的避免洩漏的方式。

### Memory Leak 的來源

- .Net 的事件 (Event) 機制。
- 所有的靜態 (static variable) 變數都不會被 GC 回收。集合 (Collections) 和靜態事件 (Static Event) 需特別注意。
- 任何快取的手段。
- WPF 綁定 (WPF Bindings)。
- Captured Members ：當變數被匿名方法使用時，它也會被參考。(這部分不是很清楚)
- 不會終止的執行緒：直到執行緒結束前，任何參考到執行緒 Live Stack 中的變數，都不會被 GC 機制收回。
- 使用 Unmanaged Resources：其中包含 Interop。

參考資料：

[Find, Fix, and Avoid Memory Leaks in C# .NET: 8 Best Practices](https://michaelscodingspot.com/find-fix-and-avoid-memory-leaks-in-c-net-8-best-practices/)

### 解決 Interop 的 Memory Leak

- 使用 `Marshal.ReleaseComObject()`。需注意是否有確實釋放記憶體，且須留意是否會造成例外。
``` csharp
if (pSrvLocPnt_new != null)
{
    while (Marshal.ReleaseComObject(pSrvLocPnt_new) > 0) { }
    pSrvLocPnt_new =null;
}
```
參考資料：

[C# with COM Interop memory leak - Stack Overflow](https://stackoverflow.com/questions/24659012/c-sharp-with-com-interop-memory-leak)

[c# - How do I properly clean up Excel interop objects? - Stack Overflow](https://stackoverflow.com/questions/158706/how-do-i-properly-clean-up-excel-interop-objects/158752#158752)

- 傳入物件時，不使用 obj ，而是強制使用型別。須注意是否有確實避免記憶體洩漏問題。

使用
``` csharp
object obj;

switch(...)
{
  case(...): obj = new int(); break;
  case(...): obj = new short(); break;
  case(...): obj = new byte(); break;
  ...
}
myComObj.Read(..., ref obj);
```

代替
``` csharp
object obj = new object();
myComObj.Read(..., ref obj);
```

參考資料：

[c# - Handle COM object memory leak - Stack Overflow](https://stackoverflow.com/questions/26532071/handle-com-object-memory-leak)

### 避免 C# 的記憶體洩漏

1. 記得取消訂閱事件。
2. 若事件的使用次數較少，直接在事件內部撰寫取消訂閱事件的程式碼。
3. 物件被參考 (Reference) 時不會被回收。使用 Event Aggregator，任何物件都可以訂閱事件，也可以發布事件。由於 Event Aggregator 使用 Weak Reference ，因此任何物件仍然可被回收。
4. 使用 Weak Reference 的 Event Handler。
5. 使用 Memory Profiler 追蹤記憶體洩漏部分的程式。

參考資料：
[5 Techniques to avoid Memory Leaks by Events in C# .NET you should know - Michael's Coding Spot](https://michaelscodingspot.com/5-techniques-to-avoid-memory-leaks-by-events-in-c-net-you-should-know/)

