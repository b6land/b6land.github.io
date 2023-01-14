---
layout: post
title: C# 如何傳入選擇性 out/ref 參數
date: 2023-01-14 12:00:00 +0800
categories: [C#]
---

如果需要選擇性的傳入 `out`/`ref` 參數的話，在 C# 中沒辦法直接結合選擇性參數和 `out`/`ref`，編譯器會提示錯誤，應該怎麼做呢 ?

### 替代性作法

使用 Overload 的方式，建立不含參數的方法，然後在這個方法內，呼叫需要傳入參數的方法。

如此一來，可以達到選擇性 `out`/`ref` 參數的效果。

### 範例程式碼

原本的 `PrintText` 方法，只接受一個參數，且預設印出當時的時間：

``` cs
static void Main(string[] args)
{
    DateTime logTime = DateTime.Today;
    PrintText("Test message");
}

/// <summary> 印出文字，含目前時間 </summary>
static void PrintText(string text){
    Console.Write(DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss" + ": "));
    Console.WriteLine(text);
}

```

假設需求要增加一個選擇性參數，用來傳入要印出的時間，而且要修改時間為當時；不傳入時則預設為當時時間。

直接改成：

``` cs
void PrintText(string text, out DateTime logTime = default(DateTime))
{ //... }
```

會提示：*error CS1741: ref 或 out 參數不能有預設值*。

因此可以改成以下方式：

``` cs

static void Main(string[] args)
{
    PrintText("Test message");

    DateTime logTime = DateTime.Today;
    PrintText("Modify something... (1st)", ref logTime);
    PrintText("Modify something... (2nd)", ref logTime);
}

/// <summary> 印出文字，含目前時間 </summary>
static void PrintText(string text)
{
    DateTime nowTime = default(DateTime);
    PrintText(text, ref nowTime);
}

/// <summary> 印出文字，含傳入時間，並將傳入時間更新為目前時間 </summary>
static void PrintText(string text, ref DateTime logTime)
{
    if(logTime == default(DateTime))
    {
        Console.Write(DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss" + ": "));
    }
    else
    {
        Console.Write(logTime.ToString("yyyy-MM-dd HH:mm:ss" + ": "));
    }

    Console.WriteLine(text);
    logTime = DateTime.Now;
}

```

### 參考資料

- [c# 4.0 - C# 4.0 optional out/ref arguments - Stack Overflow](https://stackoverflow.com/questions/2870544/c-sharp-4-0-optional-out-ref-arguments)