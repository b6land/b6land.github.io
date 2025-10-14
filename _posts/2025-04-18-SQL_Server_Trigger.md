---
layout: post
title: SQL Server Trigger 操作資料
date: 2025-04-18 22:00:00 +0800
categories: [SQL Server]
--- 

操作資料 (Data Manipulation Language, DML) 指的是 `INSERT`, `UPDATE`, `DELETE` 等語法。可以用 `CREATE TRIGGER` 建立 Trigger，可以用 `AFTER` 在動作後處理資料，或是用 `INSTEAD OF` 複寫原本的語法。插入/更新、刪除資料保留字是 `inserted`, `deleted`。

### 用法

如果只要處理一筆資料，可以用宣告變數的方式，例如下列編輯資料後記錄編輯日期的例子：

```sql
CREATE TRIGGER [TriggerName] ON DataTable
AFTER UPDATE, INSERT 
AS
    DECLARE @Id AS VARCHAR(6);

    SELECT @Id FROM inserted
    UPDATE DataTable
    SET EditTime = GETDATE()
    WHERE Id = @Id
GO
```

要留意的地方是，因為每次動作只會觸發一次 Trigger，如果一次更動多筆資料，且 Trigger 用宣告變數的方法更新資料，只會更新到一筆紀錄，而沒有處理 inserted, deleted 裡的所有資料。詳細資訊請參考 [\[SQL\]Trigger 撰寫時要注意的小細節 - 五餅二魚工作室 - 點部落](https://dotblogs.com.tw/jamesfu/2014/06/20/triggersample)。

改寫後的程式如下：

```sql
CREATE TRIGGER [TriggerName] ON DataTable
AFTER UPDATE, INSERT 
AS
    UPDATE DataTable
    SET EditTime = GETDATE()
    WHERE Id IN (
        SELECT Id FROM inserted
    )
GO
```

### 暫時停用 TRIGGER

可以用以下的語法暫時停用和重新啟用 Trigger：

```sql
-- 停用 Trigger
ALTER TABLE table_name DISABLE TRIGGER tr_name
-- 啟用 Trigger
ALTER TABLE table_name ENABLE TRIGGER tr_name
```

將 table\_name 換成實際的資料表名稱，tr\_name 換成 Trigger 的名稱。

### 參考資料

- [MSSQL 的觸發程序 TRIGGER](https://waynecheng.coderbridge.io/2021/02/11/MSSQL-Trigger/)
- [VITO の 學習筆記: Trigger](https://vito-note.blogspot.com/2013/05/trigger.html)
- 官方說明：[CREATE TRIGGER (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/create-trigger-transact-sql?view=sql-server-ver16)
- [Disable Enable Trigger SQL server for a table - Stack Overflow](https://stackoverflow.com/questions/1388072)