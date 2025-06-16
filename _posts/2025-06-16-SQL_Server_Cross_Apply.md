---
layout: post
title: SQL Server 的 Cross Apply 介紹與範例
date: 2025-06-16 22:00:00 +0800
categories: [SQL Server]
--- 

可以用 `CROSS APPLY` 將表格函數或子查詢連接起來。和 `INNER JOIN` 一樣，兩邊都有資料時，才會回傳至結果。

### 基本語法

```sql
SELECT ...
FROM 資料表
CROSS APPLY (表格函數或子查詢) AS 別名
```

### 特性

- 如果需要用到子查詢，而且需要用到 (左) 資料表的資料，就適合用 `CROSS APPLY`。
- 反之，如果用 `INNER JOIN` 也能得到相同的連接結果，就應該使用 `INNER JOIN`，避免語法太過複雜。
- 也有類似 `LEFT JOIN` 的 `OUTER APPLY`，讓你保留 (左) 資料表的資料列。

### 範例

現在有員工資料表：

| EmployeeID | Name |
| --- | --- |
| 1   | Alice |
| 2   | Bob |

以及訂單資料表，`OrderID` 越大表示更新的訂單：

| OrderID | EmployeeID | Amount |
| --- | --- | --- |
| 101 | 1   | 1000 |
| 102 | 1   | 500 |
| 103 | 2   | 300 |

如何找出這兩個員工最新的訂單？

| EmployeeID | Name | OrderID | Amount |
| --- | --- | --- | --- |
| 1   | Alice | 102 | 500.00 |
| 2   | Bob | 103 | 300.00 |

先建立資料表：

```sql
-- 建立 Employees 資料表
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    Name NVARCHAR(100) NOT NULL
);

-- 建立 Orders 資料表
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    EmployeeID INT NOT NULL,
    Amount DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID)
);
```

接著建立範例資料：

```sql
-- 插入 Employees 資料
INSERT INTO Employees (EmployeeID, Name) VALUES
(1, 'Alice'),
(2, 'Bob');

-- 插入 Orders 資料
INSERT INTO Orders (OrderID, EmployeeID, Amount) VALUES
(101, 1, 1000.00),
(102, 1, 500.00),
(103, 2, 300.00);
```

使用 `CROSS APPLY` 找出每位員工的最新訂單：

```sql
SELECT 
    e.EmployeeID,
    e.Name,
    o.OrderID,
    o.Amount
FROM 
    Employees e
CROSS APPLY -- 需要做計算的子查詢
    (
        SELECT TOP 1 *
        FROM Orders o
        WHERE o.EmployeeID = e.EmployeeID -- 在這裡設定連結欄位
        ORDER BY o.OrderID DESC
    ) o
```

最後來刪除範例資料吧：

```sql
-- 先刪除 Orders 資料表（因為有參考 Employees 資料表）
DROP TABLE IF EXISTS Orders;

-- 再刪除 Employees 資料表
DROP TABLE IF EXISTS Employees;
```

### 參考資料

- [SQL Server CROSS APPLY and OUTER APPLY Helpful Examples](https://www.mssqltips.com/sqlservertip/1958/sql-server-cross-apply-and-outer-apply/ "https://www.mssqltips.com/sqlservertip/1958/sql-server-cross-apply-and-outer-apply/")
- ChatGPT 產生範例資料