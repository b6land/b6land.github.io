---
layout: post
title: SQL Server 插入資料時自動編號 - 新增 Identity (識別欄位值) 
date: 2024-09-25 22:00:00 +0800
categories: [SQL Server]
--- 

在 SQL Server 裡面，如何產生自動遞增的編號呢？請參考內文。

### 增加自動編號的欄位

想要產生自動編號的編號 ，可以在產生表格的 Script 內，要設定的欄位後面新增 `IDENTITY (1, 1)` ，第一個數字表示起始數值，第二個數字表示遞增的數量：

```sql
CREATE TABLE [dbo].[SomeData](
    [SN]          BIGINT          IDENTITY (1, 1) NOT NULL,
    [Name]     NVARCHAR (200) NULL,
    CONSTRAINT [SomeData] PRIMARY KEY CLUSTERED ([SN] ASC)
);
```

新增資料時插入 IDENTITY 以外的欄位即可：

```sql
INSERT INTO SomeData ([Name]) VALUES('Apple');
INSERT INTO SomeData ([Name]) VALUES('Banana');
```

插入結果如下，可看見有自動編號：

| SN | Name |
|---|---|
| 1 | Apple |
| 2 | Banana |

參考資料：[IDENTITY (屬性) (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/create-table-transact-sql-identity-property?view=sql-server-ver16)  

### 在 Identity 欄位新增特定的編號

如果想要插入自訂的編號，需要先設定特定資料表的 `IDENTITY_INSERT`  為 ON，然後再插入含 ID 欄位的資料。寫入完成後，應將 `IDENTITY_INSERT`  設為 OFF。

```sql
SET IDENTITY_INSERT SomeData ON ;
INSERT INTO SomeData (SN, [Name]) VALUES(0, 'Lazy');
SET IDENTITY_INSERT SomeData OFF;
```

參考資料：[\[MSSQL\]新增特定的識別欄位值(Identity) - 甚麼都略懂 就是不懂 - 點部落](https://dotblogs.azurewebsites.net/rexhuang/2018/06/14/113052)  

### 新增資料後取得目前的編號

假如需要取得目前的編號 (例如要在其它資料表新增資料，且會參照到該編號，以利後續用 Join 連接)，可以採用以下幾種方式：

| 語法  | 說明  |
| --- | --- |
| [`@@IDENTITY`](http://msdn.microsoft.com/en-us/library/ms187342.aspx) | 取得目前連線、任意 Scope、任意資料表最後的 Identity (請小心使用) |
| [`SCOPE_IDENTITY()`](http://msdn.microsoft.com/en-us/library/ms190315.aspx)<br> | 取得目前連線、目前 Scope、任意資料表最後的 Identity (建議使用)<br> |
| [`IDENT_CURRENT('tableName')`](http://msdn.microsoft.com/en-us/library/ms175098.aspx)<br> | 取得任何連線、任何 Scope 的特定資料表 Identity |
| `INSERT`  語法搭配 [`OUTPUT`](http://msdn.microsoft.com/en-us/library/ms177564.aspx)  語法 | 最直覺的做法，但必須先輸出至變數或表格，才能再接著利用 |

Note: Scope 表示一個 SP、Trigger、function 或 batch。

參考資料：[sql - How to get the identity of an inserted row? - Stack Overflow](https://stackoverflow.com/questions/42648/how-to-get-the-identity-of-an-inserted-row)