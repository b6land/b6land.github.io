---
layout: post
title: C# Top Level Statement
date: 2024-03-27 12:00:00 +0800
categories:  [C#]
--- 

Top-Level Statement 是 .NET 6 和 C# 9.0 的新功能，`Program.cs` 不屬於任何類別，並取代以往的應用程式進入點。

### 介紹

在以往建立應用程式時，會產生以下程式碼：

```csharp
using System;

namespace Application
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

使用 .NET 6 建立新專案時 (例如使用 `dotnet new console` 建立主控台專案)，現在 Program.cs 只需要一行程式碼即可：

```csharp
Console.WriteLine("Hello, World!");
```

*(上述程式碼取自微軟官方文件)*

優點是程式變得更加簡潔，開發人員可以更容易去嘗試一些想法。但這項功能限制只能用在單一檔案上，沒辦法在其他檔案上使用。

### 參考資料

- 相關問答：[c# - Why is Program.cs no longer a class? - Stack Overflow](https://stackoverflow.com/questions/72022589/why-is-program-cs-no-longer-a-class)
- 微軟官方文件： [Top-level statements - C# tutorial - C# - Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/tutorials/top-level-statements)