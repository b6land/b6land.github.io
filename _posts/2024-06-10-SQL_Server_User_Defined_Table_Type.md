---
layout: post
title: SQL Server 利用「使用者定義資料表類型」傳遞資料和加速
date: 2024-06-10 21:00:00 +0800
categories: [SQL Server]
--- 

在 SQL Server 中，想要傳遞資料表給 SP，或是想將資料表放入記憶體加速，可以善用「使用者定義資料表類型」。

### 介紹

使用者定義資料表類型 (User-defined table type) 可以用來：

1. 作為變數，傳遞一整組資料到預存程序 (Stored Procedure, SP)。
2. 建立記憶體最佳化資料表 (Memory-optimized Table)，增進查詢效率。使用記憶體來存放暫存資料時，不必再從磁碟建立 tempdb 資料庫中的暫存表，因此可以加快速度。(在建立表格時，使用 `WITH ( MEMORY_OPTIMIZED = ON )` 關鍵字，且必須建立一組索引。) 

![使用者定義資料表類型](/assets/imgs/2024-06-10/user_defined_table_type.png)  

### 範例  

1. `CREATE TYPE` 建立使用者定義資料表類型，同時需要建立索引，而且需加入 `MEMORY_OPTIMIZED = ON`  。

```sql
-- 建立「使用者定義資料表類型」
CREATE TYPE Person_MemOptimized AS TABLE
(Id INT PRIMARY KEY NONCLUSTERED 
 , [Name] VARCHAR(100)
) WITH ( MEMORY_OPTIMIZED = ON )
```

2\. 建立 SP，此 SP 可以定義資料表類型的變數 (必須是唯讀，因此加入 `READONLY`  )，並接收資料。

```sql
-- 建立 SP
CREATE PROCEDURE Usp_InsertPersonMemOpt
@PersonData Person_MemOptimized READONLY
AS
-- SP 要執行的 SQL 語法
BEGIN
    SELECT Id, [Name] FROM @PersonData ;
END
``` 

3\. 從查詢語法建立資料，並傳遞至 SP。

```sql
-- 宣告資料表類型變數
DECLARE @VarPerson_MemOptimized AS Person_MemOptimized 
 
-- 插入資料
INSERT INTO @VarPerson_MemOptimized 
VALUES (1, 'Alice')
INSERT INTO @VarPerson_MemOptimized 
VALUES (2, 'Bob')
INSERT INTO @VarPerson_MemOptimized 
VALUES (3, 'Candy')

-- 執行 SP
EXEC Usp_InsertPersonMemOpt @VarPerson_MemOptimized 
```

### 初次使用

如果是第一次使用記憶體最佳化的相關功能，可能會遇到「資料庫的 MEMORY_OPTIMIZED_FILEGROUP 必須在線上」錯誤。此時需要執行以下指令，來建立資料庫的記憶體最佳化檔案群組和容器：

```sql
ALTER DATABASE AdventureWorks2019 ADD FILEGROUP AdventureWorks2019_mod CONTAINS MEMORY_OPTIMIZED_DATA -- 為已有的資料庫增加群組

ALTER DATABASE AdventureWorks2019 ADD FILE (name='AdventureWorks2019_mod1', filename='c:\data\AdventureWorks2019_mod1') TO FILEGROUP AdventureWorks2019_mod  -- 為群組增加容器 
```

以上的語法應該要將 AdventureWorks2019 字樣代換成實際使用的資料庫，以及想建立的檔案路徑。

### 參考資料

- [Table-Valued Parameters in SQL Server](https://www.sqlshack.com/table-valued-parameters-in-sql-server/ )
- [記憶體最佳化加快暫存資料表與資料表變數的速度 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization?view=sql-server-ver16 )
- [記憶體最佳化檔案群組 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/in-memory-oltp/the-memory-optimized-filegroup?view=sql-server-ver16 )