---
layout: post
title: C# 用 Process 呼叫外部程式，取得輸出訊息
date: 2024-10-20 16:00:00 +0800
categories: [C#]
--- 

本文介紹如何用 `Process` 呼叫外部程式，並將外部程式的標準輸出 (`StandardOutput`) 重新導向到 C# 的應用程式內，以便捕捉和處理外部程式的訊息。
  
### 使用方式

1. 建立 `System.Diagnostics.ProcessStartInfo`  物件，並指定外部程式的檔名、參數等其它資訊。
2. 建立 `System.Diagnostics.Process`  物件，指定 `StartInfo`  為剛剛建立的 `ProcessStartInfo`  物件。
3. 接著呼叫 `Process.Start()`  方法。
4. 如果有設定 `startInfo.RedirectStandardOutput = true;`  ，可以在呼叫 Start 方法後，用 `Process.StandardOutput.ReadToEnd()`  取得外部程式顯示的訊息。
5. 如果要等待外部程式執行結束才繼續執行 C# 應用程式，可以在呼叫 Start 方法後，用 `Process.WaitForExit();`  等待。

### 範例程式

以下程式碼會啟動命令提示字元，並執行指令 (`someCommand` 和 `FileName` 可以分別代換成自己需要的參數和外部程式)。執行的結果會輸出在 C# 應用程式的 Console，並等待外部程式結束。

```csharp
string someCommand = "some_command";
System.Diagnostics.Process process = new System.Diagnostics.Process();
System.Diagnostics.ProcessStartInfo startInfo = new System.Diagnostics.ProcessStartInfo();
startInfo.WindowStyle = System.Diagnostics.ProcessWindowStyle.Hidden;
startInfo.FileName = "cmd.exe";
startInfo.Arguments = $"/C {SomeCommand}";
startInfo.RedirectStandardError = true;
startInfo.RedirectStandardOutput = true;    
process.StartInfo = startInfo;
process.Start();
Console.WriteLine(process.StandardOutput.ReadToEnd());
process.WaitForExit();
```

### 參考資料

- ChatGPT
- [ProcessStartInfo.RedirectStandardOutput 屬性 (System.Diagnostics) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.diagnostics.processstartinfo.redirectstandardoutput?view=net-8.0)