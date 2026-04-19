---
layout: post
title: SQL Server 刪除大量資料和資料表
date: 2026-04-19 15:30:00 +0800
categories: [SQL Server]
---

在 SQL Server 內，要刪除大量資料和資料表，可以參考下列方式，避免用到效能較差的方式刪除。

### 刪除大量資料

刪除大量資料的原則，是「減少鎖定整張資料表的機會」，其做法有下列幾種：

- 搭配 ROWLOCK 提示字，降低被升級成 Table-Lock 的機會。

```csharp
DELETE TOP (1000)
FROM YourTable WITH (ROWLOCK)
WHERE YourCondition

```

- 可以在讀取要刪除的資料時加入 READPAST，讓其它交易略過被鎖的資料。

```csharp
DELETE T
FROM (
    SELECT TOP (1000) *
    FROM YourTable WITH (READPAST)
    WHERE YourCondition
) T

```

- 將大量的欲刪除資料改成分批刪除，減少升級成 Table-Lock 的機會。

### 刪除整張資料表

- 可以用 Delete 刪除資料表的所有資料，但因為是資料操作語法 (DML)，會留下大量的交易紀錄；此外刪除期間會鎖定每一筆資料；不會重設編號。

```sql
DELETE FROM MyTable
```

- Truncate 可以刪除資料表的所有資料，只留下資料結構；不會鎖定資料；會重設編號。

```sql
TRUNCATE TABLE MyTable
```

- Drop Table 會將整個資料表的資料和資料結構都刪除掉。

```sql
DROP TABLE MyTable
```

### 參考資料

- [【SQL Server性能优化】删除大量数据的方法比较 - 小木瓜瓜瓜 - 博客园](https://www.cnblogs.com/momogua/p/8304580.html)
- [批次刪除大量資料時應搭配合適索引來降低Blocking - Rock的SQL筆記本 - 點部落](https://dotblogs.com.tw/rockchang/2018/04/30/145451)
- [刪除整個資料表，使用Delete、Truncate Table與Drop Table的差異－月神的咖啡館 - 痞客邦](https://byron0920.pixnet.net/blog/post/85759990)
- ChatGPT 產生部分 SQL 語法