---
layout: post
title: 怎麼寫出好的命令列程式
date: 2023-11-14 12:00:00 +0800
categories:  [Programming]
--- 

本文介紹好的命令列程式具有那些設計上的考量，以及 REPL 模式的簡介。

### 怎麼寫出好的命令列程式

- 命令列程式架構的設計考量？
  - 最佳回答的摘錄: 
    - 使用 REPL 模式。
    - 為每個功能建立一致的介面，使其便於呼叫。
    - 加入錯誤處理。
    - 建立控制器，並將資料庫、紀錄等不同行為分隔開來。
  - 參考資料: [architecture - Architectural considerations in designing console applications? - Stack Overflow](https://stackoverflow.com/questions/817673/architectural-considerations-in-designing-console-applications)

- REPL 是 Read-Eval-Print Loop 的縮寫，是一種簡易的命令列互動環境，表示讀取使用者指令、執行計算、列印結果的循環。
  - 維基百科的定義: [Read–eval–print loop - Wikipedia](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)
  - 用 REPL 關鍵字找到的有趣專案，可以用互動模式撰寫與執行 C# 程式 (如同 Python): [GitHub - waf/CSharpRepl: A command line C# REPL with syntax highlighting – explore the language, libraries and nuget packages interactively.](https://github.com/waf/CSharpRepl)