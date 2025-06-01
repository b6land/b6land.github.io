---
layout: post
title: C# File 類別簡單檔案存取
date: 2025-06-01 12:00:00 +0800
categories: [C#]
--- 

在 C# 中，讀取與寫入檔案的方式有很多種，最常見的是使用 `StreamReader`  和 `StreamWriter` 類別，也可以直接使用 `File`  類別，讓程式碼更簡潔。

### StreamReader 和 File 類別的差異

一般會用 [StreamReader 類別](https://StreamReader%20%E9%A1%9E%E5%88%A5%20\(System.IO\)%20-%20Microsoft%20Learn) 讀取文字檔：

```csharp
string path = "xxx";
using (StreamReader reader = new StreamReader(path))
{
    txtEditor.Text = reader.ReadToEnd();
}
```

<br>

也可以改用 [File 類別](https://learn.microsoft.com/zh-tw/dotnet/api/system.io.file?view=net-8.0) 讀取文字檔內容：

```csharp
string path = "xxx";
txtEditor.Text = File.ReadAllText(path);
```

使用 File 類別讀取和寫入檔案內容，使用上和 `StreamReader`  / `StreamWriter`  沒有差異，但可以撰寫更少的程式碼 (連釋放資源都一起處理了)。

如果有需要自訂的讀取動作，就很適合用 `StreamReader` ，例如在讀取時減少記憶體占用，可以讓 `StreamReader`  一次只讀幾個 Byte，不用像 `File.ReadAllText`  一次讀取所有內容。

重寫自 [c# - Any difference between File.ReadAllText() and using a StreamReader to read file contents? - Stack Overflow](https://stackoverflow.com/questions/3545402/any-difference-between-file-readalltext-and-using-a-streamreader-to-read-file) 。

### 常用的 File 方法

| 方法  | 描述  |
| --- | --- |
| WriteAllText<br> | 寫入整個文字內容到檔案（會覆蓋） |
| AppendAllText<br> | 將文字加到檔案結尾 |
| ReadAllText<br> | 讀取文字檔的所有資料 |
| Exists<br> | 檢查檔案是否存在<br> |

### 範例程式碼

```csharp
using System;
using System.IO;
					
public class Program
{
	public static void Main()
	{
		string path = "sample.txt";

        // 1. 檢查檔案是否存在
        if (!File.Exists(path))
        {
            Console.WriteLine("檔案不存在，建立新檔案...");
            File.WriteAllText(path, "這是檔案的第一行。\n");
        }
        else
        {
            Console.WriteLine("檔案已存在。");
        }

        // 2. 讀取檔案內容
        Console.WriteLine("\n目前檔案內容：");
        string content = File.ReadAllText(path);
        Console.WriteLine(content);

        // 3. 覆蓋寫入新內容
        Console.WriteLine("\n覆蓋寫入新內容...");
        File.WriteAllText(path, "覆蓋後的第一行。\n");

        // 4. 附加新內容
        Console.WriteLine("附加新內容...");
        File.AppendAllText(path, "這是附加的新行。\n");

        // 5. 再次讀取檔案內容
        Console.WriteLine("\n更新後的檔案內容：");
        string updatedContent = File.ReadAllText(path);
        Console.WriteLine(updatedContent);
	}
}
```

結果為：

```plaintext
檔案不存在，建立新檔案...

目前檔案內容：
這是檔案的第一行。

覆蓋寫入新內容...
附加新內容...

更新後的檔案內容：
覆蓋後的第一行。
這是附加的新行。
```
