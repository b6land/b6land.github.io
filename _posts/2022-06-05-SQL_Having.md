---
layout: post
title: SQL 用 HAVING 語法列出重複項目
date: 2022-06-05 12:00:00 +0800
categories: [SQL Server]
---

這篇文章介紹 SQL 如何使用 `HAVING` 搭配 `COUNT` 找出重複的資料項目，以及 `HAVING` 函數和 `WHERE` 的差異。

### 列出重複項目

以下的查詢語法，使用 `COUNT` 找出人員 ID、部門的重複次數 (即相同的數量)，依 `GROUP BY` 將相同欄位分成好幾個群組，再透過 `HAVING` 列出重複次數大於 1 的資料：

``` sql
SELECT EmployeeID, Department 
FROM EmployeeTable
GROUP BY EmployeeID, Department 
HAVING COUNT(*) > 1
```

假設我們有一個名為 EmployeeTable 的員工表格，資料如下：

| EmployeeID | Department |
| --- | --- |
| 1 | Sales |
| 2 | HR |
| 3 | IT |
| 1 | Sales |
| 4 | Marketing |
| 5 | Sales |
| 3 | IT |
| 2 | HR |
| 6 | IT |
| 7 | Marketing | 
| 1 | Sales |

執行上述的查詢語法後，其結果如下：

| EmployeeID | Department |
| --- | --- |
| 1 | Sales |
| 2 | HR | 
| 3 | IT |

因為 EmployeeID 1 在 Sales 部門出現了 3 次，2 在 HR 部門出現了 2 次，3 在 IT 部門出現了 2 次，這些都符合重複次數大於 1 的條件，所以會被列出來。

(以下是這組資料在 SQL Server 的建立語法，可以到 SQL Online IDE 等網站測試看看哦！)

```sql
-- 建立資料表
CREATE TABLE EmployeeTable (
    EmployeeID INT,
    Department NVARCHAR(50)
);

-- 產生測試資料
INSERT INTO EmployeeTable (EmployeeID, Department) VALUES
(1, 'Sales'), (2, 'HR'), (3, 'IT'),
(1, 'Sales'), (4, 'Marketing'), (5, 'Sales'),
(3, 'IT'), (2, 'HR'), (6, 'IT'),
(7, 'Marketing'), (1, 'Sales');

```

### HAVING 與 WHERE 的差異

有使用聚合函數 (例如 `SUM`, `AVG` 等等) 計算時，假如想要對計算結果做進一步的篩選，這時不能夠使用 `WHERE` 查詢，而要使用 `HAVING`。

例如以下資料：

| EmployeeID | ItemMoney |
| --- | --- |
| 1 | 500 |
| 2 | 1000 | 
| 1 | 350 |

- 錯誤的例子

``` sql
SELECT EmployeeID, SUM(ItemMoney)
FROM ItemValue
GROUP BY EmployeeID
WHERE SUM(ItemMoney) > 1500
```

- 正確的例子

``` sql
SELECT EmployeeID, SUM(ItemMoney)
FROM ItemValue
GROUP BY EmployeeID
HAVING SUM(ItemMoney) > 1500
```

`HAVING` 通常放在查詢語法的最後方；`WHERE` 可用在一般非聚合函數的條件查詢，例如：

``` sql
SELECT EmployeeID, ItemMoney
FROM ItemValue
WHERE EmployeeID = '0001'
```


### 參考資料

- [SQL HAVING - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sqlhaving.html)
- [SQL 列出所有重複的資料](https://lawrencetech.blogspot.com/2009/05/sql.html)