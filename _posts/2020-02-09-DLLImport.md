---
layout: post
title: 如何使用 DLLImport 呼叫 C++ 程式碼
date: 2020-02-09 12:00:00 +0800
categories: C#
tags: [C#, DLL]
---

本文介紹如何在 Visual Studio 專案和 C# 程式中，引用 C++ 編譯成的 DLL。

### DLL 專案的設定

以下引用自參考資料中的 Dot Net Perls。

```
Configuration Type:
    Dynamic Library (.dll)

Use of MFC:
    Use Standard Windows Libaries

Use of ATL:
    Not Using ATL

Common Language Runtime Support:
    No Common Language Runtime Support

Whole Program Optimization:
    Use Link Time Code Generation
```

### 在 DLL 專案下的程式碼

以下引用自參考資料中的 Dot Net Perls。
首先為標頭的宣告。

```
// This code should be put in a header file for your C++ DLL. It declares
// an extern C function that receives two parameters and is called SimulateGameDLL.
// I suggest putting it at the top of a header file.
extern "C" {
    __declspec(dllexport) void __cdecl SimulateGameDLL (int a, int b);
}
```

接著在程式本體撰寫與標頭宣告相同名稱與參數的程式碼。

```
// The keywords and parameter types must match
// ... the above extern declaration.
extern void __cdecl SimulateGameDLL (int num_games, int rand_in) {

    // This is part of the DLL, so we can call any function we want
    // in the C++. The parameters can have any names we want to give
    // them and they don't need to match the extern declaration.
}
```

### 使用 DLLImport

把編譯好的 DLL 和 C# 的執行檔放置於同一目錄下。
使用以下的程式碼呼叫 DLL 內的函式。引用自參考資料中的 Dot Net Perls。

```
/// <summary>
/// A C-Sharp interface to the DLL, written in C++.
/// </summary>
static class GameSharp
{
    /// <summary>
    /// The native methods in the DLL's unmanaged code.
    /// </summary>
    internal static class UnsafeNativeMethods
    {
        const string _dllLocation = "CoreDLL.dll";
        [DllImport(_dllLocation)]
        public static extern void SimulateGameDLL(int a, int b);
    }
}
```

### 注意事項

- 使用 Marcos 來搬移 DLL 比較省力。
- 要注意 DLL 名稱正不正確。
- 使用 `MarshalAs` 來存取 `char*` 指標。

### 參考資料

- [DllImport and ref parameters](https://social.msdn.microsoft.com/Forums/vstudio/en-US/9b1c472b-7b0c-4ccc-a4fd-d64e23a82072/dllimport-and-ref-parameters?forum=csharpgeneral) 
- [Falldog的程式戰場: [C#] Allocate structure buffer array from DLL](http://falldog7.blogspot.com/2015/01/c-sharp-allocate-structure-buffer-array-dll.html)
- [C# DllImport Attribute - Dot Net Perls](https://www.dotnetperls.com/dllimport) ：這篇文章列出了 DLL 建置、呼叫的程式碼與可能遇到的問題，可以先看。