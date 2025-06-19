---
layout: post
title: SQL Server 用 Sequence 建立流水號
date: 2025-06-19 23:00:00 +0800
categories: [SQL Server]
--- 

SQL Server 2012 (Denali) 增加了 Sequence 功能，可以用來建立流水號。

### 什麼時候適合用 Sequence 而不是 Identity

- 一個值需要被多張資料表共用，例如跨表的統一流水號。
- 需要預先取得編號的情形（不像 IDENTITY 必須插入資料才能產生）。
- 更細微地控制遞增數值範圍。

### 特性

- Sequence 屬於資料庫層級，和資料表裡的 Identity 欄位只對該資料表有效是不一樣的。
- 遞增的動作不會受到 Transcation rollback 的影響，就算交易失敗也會保持遞增結果。

### 語法

顯示目前資料庫裡的 Sequence：

```sql
SELECT * FROM sys.sequences
```

建立新的 Sequence：

```sql
CREATE SEQUENCE SequenceTest -- 建立名為 SequenceTest 的 Sequence
START WITH 1 -- 從 1 開始
INCREMENT BY 1 -- 遞增數量
```

取得 Sequence 的下一個數字：

```sql
SELECT NEXT VALUE FOR SequenceTest 
```

修改目前的 Sequence：

```sql
ALTER SEQUENCE SequenceTest -- 要修改的 Sequence
RESTART WITH 1 -- 從哪個數字開始
INCREMENT BY 1 -- 遞增數量 (要遞減可以設負數)
MINVALUE 1 -- 設定最小範圍
MAXVALUE 1000000 -- 設定最大範圍
```

此外，還有 `CYCLE` 可設定超過最大值時，從最小值重新開始。

### 參考資料


- [\[Denali 新特性探險10\]Sequence - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10076473)
- [NEXT VALUE FOR (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/functions/next-value-for-transact-sql?view=sql-server-ver17 )
- [序列號 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/sequence-numbers/sequence-numbers?view=sql-server-ver17)
- [sys.sequences (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/system-catalog-views/sys-sequences-transact-sql?view=sql-server-ver17 )
- [CREATE SEQUENCE (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/create-sequence-transact-sql?view=sql-server-ver17)
- [ALTER SEQUENCE (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/statements/alter-sequence-transact-sql?view=sql-server-ver17)