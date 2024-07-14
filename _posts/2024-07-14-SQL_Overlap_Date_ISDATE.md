---
layout: post
title: SQL Server 檢查重疊日期時間、ISDATE 判斷有效日期
date: 2024-07-14 14:00:00 +0800
categories: [SQL Server]
--- 

今天要介紹兩個很實用的小技巧！一個可以檢查要插入的資料日期時間，有沒有和現有資料重複；另一個則適合用在資料欄位存各種資料時，判斷某筆資料是不是日期。

### 檢查重疊的日期、時間

假設有下列兩個時間段：

```plaintext
|xxx|---------|xxx|
|-----|ooo|-------|
```

請問如何檢查這兩個時間段有沒有重疊？

假設要檢查的日期和時間分別宣告為 `@Begin`  和 `@End` ，資料庫中的欄位為 BeginDate 和 EndDate，想要判斷不重疊 (落在已存在的兩個時間段中間) 時，可以使用以下的條件：

```sql
@Begin > EndDate AND @End < BeginDate
```

反過來說，如果要判斷有重疊的狀況，則可以列出所有的重疊狀況來檢查：

```sql
((@Begin BETWEEN BeginDate AND EndDate )
OR (@End BETWEEN BeginDate AND EndDate)
OR (BeginDate BETWEEN @Begin AND @End)
OR (EndDate BETWEEN @Begin AND @End))
```

範例如下，首先我們先加入測試資料：

```sql
-- 建立測試用資料表
CREATE TABLE TimeSlots (
    ID INT IDENTITY PRIMARY KEY,
    BeginDate DATETIME NOT NULL,
    EndDate DATETIME NOT NULL
);

-- 插入資料
INSERT INTO TimeSlots (BeginDate, EndDate)
VALUES
    ('2024-07-01 08:00:00', '2024-07-01 09:00:00'),
    ('2024-07-01 11:00:00', '2024-07-01 13:00:00'),
    ('2024-07-01 14:00:00', '2024-07-01 15:00:00');
```

接著我們定義要測試的時間，查詢重疊的時間段有哪些：

```sql
DECLARE @Begin AS DATETIME = '2024-07-01 08:30:00';
DECLARE @End AS DATETIME = '2024-07-01 12:00:00';

SELECT ID, BeginDate, EndDate
FROM TimeSlots
WHERE ((@Begin BETWEEN BeginDate AND EndDate )
    OR (@End BETWEEN BeginDate AND EndDate)
    OR (BeginDate BETWEEN @Begin AND @End)
    OR (EndDate BETWEEN @Begin AND @End))
```

其結果為：  

| ID  | BeginDate | EndDate |
| --- | --- | --- |
| 1   | 2024-07-01 08:00:00 | 2024-07-01 09:00:00 |
| 2   | 2024-07-01 11:00:00 | 2024-07-01 13:00:00 |

#### 參考資料

- [sql中检查时间是否重叠 - 杨浪 - 博客园](https://www.cnblogs.com/yanglang/p/10770385.html)  

### 使用 `ISDATE`  判斷資料是不是有效的日期

如果今天在字串類型 (ex. `VARCHAR(N)` ) 的欄位內，存放了日期和其它類型的資料，而且想取得 `DATETIME` 類型的資料，可以使用 `ISDATE`  函數檢查是不是日期。

若常常要存放日期資料，還是建議資料表要設計 `DATETIME`  欄位！

範例如下：

```sql
SELECT CASE ISDATE('2024-05-08 00:01:02') -- 會視為 VALID
	WHEN 1 THEN 'VALID' 
	ELSE 'INVALID'
END AS Result
```

#### 參考資料

- [sql - Check if value is date and convert it - Stack Overflow](https://stackoverflow.com/questions/16353411/check-if-value-is-date-and-convert-it)
- [ISDATE (Transact-SQL) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/functions/isdate-transact-sql?view=sql-server-ver16)