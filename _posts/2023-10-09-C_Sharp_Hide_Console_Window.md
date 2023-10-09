---
layout: post
title: C# 如何隱藏主控台視窗
date: 2023-10-09 12:00:00 +0800
categories: [C#]
---

如果今天在 C# 中撰寫一個主控台 (Console) 的專案，那麼要怎麼隱藏主控台視窗呢？

### 隱藏主控台視窗

方法 1 : 將原本的 Console Application 改成 Windows Application。

方法 2 : 使用 Windows 內建的 API。範例程式碼如下 (引用自參考資料)：

``` cs
using System.Runtime.InteropServices;

[DllImport("kernel32.dll")]
static extern IntPtr GetConsoleWindow();
[DllImport("user32.dll")]
static extern bool ShowWindow(IntPtr hWnd, int nCmdShow);

const int SW_HIDE = 0;
const int SW_SHOW = 5;

static void Main(string[] args) { 
    var handle = GetConsoleWindow();
    // Hide
    ShowWindow(handle, SW_HIDE);
    // Show
    ShowWindow(handle, SW_SHOW);
}
```

參考資料 : [Show/Hide the console window of a C# console application - Stack Overflow](https://stackoverflow.com/questions/3571627/show-hide-the-console-window-of-a-c-sharp-console-application)