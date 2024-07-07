---
layout: post
title: 寫出好的命令列程式與 REPL 模式介紹
date: 2023-11-14 12:00:00 +0800
categories:  [Programming]
--- 

本文介紹好的命令列程式具有那些設計上的考量，以及 REPL 模式的簡介和範例程式碼。

### 怎麼寫出好的命令列程式

命令列程式架構的設計考量是什麼？
最佳回答的摘錄如下: 

- 使用 REPL 模式。
- 為每個功能建立一致的介面，使其便於呼叫。
- 加入錯誤處理。
- 建立控制器，並將資料庫、紀錄等不同行為分隔開來。

那麼，什麼是 REPL 模式呢？

### REPL 模式介紹

- REPL 是 Read-Eval-Print Loop 的縮寫，是一種簡易的命令列互動環境，表示讀取使用者指令、執行計算、列印結果的循環。

以下是一個範例 REPL 程式，使用 C# 語言：


```cs
using System;
using System.Data;

class Program
{
    static void Main()
    {
        Console.WriteLine("簡單的 C# REPL 模式");
        Console.WriteLine("輸入 'exit' 退出程式");
        
        while (true)
        {
            Console.Write("> ");
            string input = Console.ReadLine();
            
            if (input.ToLower() == "exit")
                break;
            
            try
            {
                var result = Evaluate(input);
                Console.WriteLine(result);
            }
            catch (Exception ex)
            {
                Console.WriteLine($"錯誤: {ex.Message}");
            }
        }
    }

    static object Evaluate(string expression)
    {
        DataTable table = new DataTable();
        var result = table.Compute(expression, string.Empty);
        return result;
    }
}

```

1. 輸入讀取：程式進入一個無限循環 (Loop)，等待用戶輸入 (Read)。
2. 表達式評估：利用 DataTable 的 `Compute` 方法來評估 (Evaluate) 用戶輸入的數學表達式 (例如 5+10)，並顯示結果 (Print)。
3. 退出檢查：如果用戶輸入 exit，會退出循環並結束程式。
4. 錯誤處理：如果在評估過程中出現錯誤（例如輸入無效的表達式），會顯示錯誤訊息。

這個簡單的 REPL 可以處理基本的數學運算，但不支援更複雜的 C# 語法或函數。

### 參考資料

- 怎麼寫出好的命令列程式: [architecture - Architectural considerations in designing console applications? - Stack Overflow](https://stackoverflow.com/questions/817673/architectural-considerations-in-designing-console-applications)
- 維基百科的定義: [Read–eval–print loop - Wikipedia](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)
- 用 REPL 關鍵字找到的有趣專案，可以用互動模式撰寫與執行 C# 程式 (如同 Python 的互動式介面): [GitHub - waf/CSharpRepl: A command line C# REPL with syntax highlighting – explore the language, libraries and nuget packages interactively.](https://github.com/waf/CSharpRepl)