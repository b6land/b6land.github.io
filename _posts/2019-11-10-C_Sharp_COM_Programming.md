---
layout: post
title: C# COM 程式設計的疑難雜症
date: 2019-11-10 12:00:00 +0800
categories: C#
tags: [C#]
---

本篇介紹 C# COM 程式設計的疑難雜症，以記憶體的管理為主。

### 疑難雜症

- 800706F4 錯誤：使用 ATL Object 元件時，呼叫函式時可能出現的錯誤。這個錯誤表示傳了一個空的位址，例如傳入 NULL。
參考資料：[关于800706F4错误的问题-CSDN论坛](https://bbs.csdn.net/topics/310177338)

- C# 不會自動清除  C++ 所配置的記憶體：假如使用 Marshal 的方式呼叫 COM object 內的函式，如下方語法：

{% highlight csharp %}
void MemAlloc(ref double[] test, int membercount)
{% endhighlight %}

應該使用 `CoTaskMemAlloc()` ，從 Managed Code 配置記憶體。CLR 會在記憶體不再被使用時，釋放記憶體。也可使用 `Marshal.FreeCoTaskMem()`  釋放已配置的記憶體。

參考資料：[Does C# clean up C++ allocated memory? - Stack Overflow](https://stackoverflow.com/questions/685934/does-c-sharp-clean-up-c-allocated-memory)
[Default Marshaling Behavior - Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/framework/interop/default-marshaling-behavior?redirectedfrom=MSDN)

- 合適的釋放 COM 物件：使用 `Marshal.ReleaseComObject()` 於單一的 COM / .NET boundary crossing (如果你參考一個 COM 物件，該物件用於取得另一個 COM 參考)。不要使用 Marshal.FinalReleaseComObject() - 因為有些  COM 物件以 Singleton 模式運作，且該方法不適用於這些物件。
參考資料：[c# - Proper way of releasing COM objects? - Stack Overflow](https://stackoverflow.com/questions/15728676/proper-way-of-releasing-com-objects)

### 使用 malloc 配置記憶體和 Marshal.FreeCoTaskMem 釋放的範例

C++ Code

{% highlight cpp %}
extern "C" ABA_API void getArray(long* len, double **data)
{
    *len = delArray.size();
    auto size = (*len)*sizeof(double);
    *data = static_cast<double*>(malloc(size));
    memcpy(*data, delArray.data(), size);
}
{% endhighlight %}

C# Code

{% highlight csharp %}
[DllImport("AudioPluginSpecDelay")]
private static extern void getArray(out int length, out IntPtr array);

int theSize;
IntPtr theArrayPtr;
double[] theArray;

getArray(out theSize, out theArrayPtr);
Marshal.Copy(theArrayPtr, theArray, 0, theSize);
Marshal.FreeCoTaskMem(theArrayPtr);

// theArray is a valid managed object while the native array is already freed
{% endhighlight %}

參考資料：[Array from C++ to C# - Stack Overflow](https://stackoverflow.com/questions/36224120/array-from-c-to-c-sharp)

### 名詞解釋

- ATL: Active Template Library，可以輕鬆地建立小型、快速的元件物件模型 (COM) 物件。與 COM 開發有關，和 Marshal 應該無關。

參考資料：[ATL 簡介 - Microsoft Docs](https://docs.microsoft.com/zh-tw/cpp/atl/introduction-to-atl?view=vs-2019)

### 其它相關資料

- [Marshal.PtrToStructure Method (System.Runtime.InteropServices) - Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/api/system.runtime.interopservices.marshal.ptrtostructure?view=netframework-4.7.2)
-[C# 中的 IntPtr 類型](https://www.cnblogs.com/freeliver54/archive/2008/10/15/1311371.html)