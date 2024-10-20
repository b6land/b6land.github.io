---
layout: post
title: SQL Server 使用 Print 和 Raiserror 列印訊息
date: 2024-10-20 14:00:00 +0800
categories: [SQL Server]
--- 

如果要在 SQL Server 執行 Script 時顯示處理進度或結果，可以使用 Print 和 Raiserror 列印訊息。

### 使用方式

一般來說，可以使用 `PRINT` 列印訊息：

```sql
PRINT <訊息>
```

不過該語法只會在整個 Script 執行完後顯示。如果要處理的資料很多，希望在處理到特定進度時就顯示訊息，可以改用 RAISERROR 語法：

```sql
RAISERROR (<訊息>, <嚴重度>, <狀態>) WITH NOWAIT
```

其中嚴重度由任何使用者可以設定為 0 ~ 18，系統管理員可以設定為 19 ~ 25。當嚴重度 >= 11 時，控制權會交給 `TRY ... CATCH ...` 的 `CATCH` 區塊；當嚴重度 >= 19 時，則會中斷資料庫連接。

狀態可以設為 1 ~ 255 的任意值，有助於使用者快速找出問題來源，例如以下的指令：

```sql
RAISERROR('測試錯誤', 1, 8) 
```

會顯示：

```plaintext
測試錯誤
訊息 50000，層級 1，狀態 8
```

`WITH NOWAIT` 則表示會立刻顯示訊息。

### 參考資料

- [sql server - PRINT output doesn't appear immediately in the Messages pane in SSMS - Stack Overflow](https://stackoverflow.com/questions/349358/print-output-doesnt-appear-immediately-in-the-messages-pane-in-ssms)  
- [RAISERROR (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/language-elements/raiserror-transact-sql?view=sql-server-ver16)