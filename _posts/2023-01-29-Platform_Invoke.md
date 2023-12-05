---
layout: post
title: 什麼是 Platform Invoke (P/Invoke)
date: 2023-01-29 12:00:00 +0800
categories:  [C#, DLL]
---

什麼是 Platform Invoke (P/Invoke，中文或可稱為平台叫用) ? 

### 介紹

- 允許從 Managed Code 存取 Unmanaged Code。例如從 .Net 的程式碼內，存取 C++ 建立的函式庫 (Library)，使用裡面的函式、結構或 Callback。
- P/Invoke API 絕大部分包含在 `System` 和 `System.Runtime.InteropServices` 命名空間。
- 最常見的例子: 

``` cs
static class AddSharp
{
    internal static class UnsafeNativeMethods
    {
        const string _dllLocation = "DllExample.dll";
        [DllImport(_dllLocation)] // 重要：告知執行期間要載入的 DLL
        public static extern int Add(int a, int b); // 關鍵：宣告相同的方法，extern 表示為外部方法 (在 DLL 內)
    }

    public static void Execute(){
        Console.WriteLine(UnsafeNativeMethods.Add(5, 10)); // 可以用 Managed 的方式 (如一般的 C# 方法) 呼叫使用
    }
}
public void Run(){
    AddSharp.Execute();
}
```

- 在 MacOS 或 Linux 中，除了外部函式庫名稱不同以外，使用方式類似，可參閱下方參考資料。
- P/Invoke 是雙向的，也可以由 Unmanaged Code 呼叫 Managed Code。以下方參考資料中的例子來說，會宣告包含委派的外部方法，並傳入 C# 的委派方法。 
- 參考資料: [Platform Invoke (P/Invoke) - Microsoft Learn](https://docs.microsoft.com/zh-tw/dotnet/standard/native-interop/pinvoke)