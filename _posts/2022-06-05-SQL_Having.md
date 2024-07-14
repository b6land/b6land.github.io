---
layout: post
title: SQL Having 語法與列出重複項目
date: 2022-06-05 12:00:00 +0800
categories: [SQL Server]
---

這篇文章介紹 SQL 的 `HAVING` 函數和 `WHERE` 的差異，並試著用 `HAVING` 搭配 `COUNT` 找出重複的資料項目。

### HAVING 介紹

有使用聚合函數 (例如 `SUM`) 計算時，假如想要對計算結果做進一步的篩選，這時不能夠使用 `WHERE` 查詢，而要使用 `HAVING`：

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

`HAVING` 通常放在查詢語法的最後方。

`WHERE` 可用在一般非聚合函數的條件查詢，例如：

``` sql
SELECT EmployeeID, ItemMoney
FROM ItemValue
WHERE EmployeeID = '0001'
```

### 列出重複項目

以下語法，會使用 `COUNT` 找出人員 ID、部門的重複次數 (即相同的數量)，再透過 `HAVING` 列出重複次數大於 1 的資料。

``` sql
SELECT EmployeeID, Department 
FROM EmployeeTable
GROUP BY EmployeeID, Department 
HAVING COUNT(*) > 1
```

### 參考資料

- [SQL HAVING - 1Keydata SQL 語法教學](https://www.1keydata.com/tw/sql/sqlhaving.html)
- [SQL 列出所有重複的資料](https://lawrencetech.blogspot.com/2009/05/sql.html)