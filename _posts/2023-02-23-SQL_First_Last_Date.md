---
layout: post
title: SQL 取得當月第一天和最後一天
date: 2023-02-23 12:00:00 +0800
categories: [SQL]
---

以下是在 SQL 結合 `DATEPART()` 和 `DATEADD()` 方法取得當月第一天和最後一天，並去除時間。

### 作法

```sql
SELECT 
    CONVERT(DATE, -- 轉型成日期 
        DATEADD(DAY, - DATEPART(DAY, GETDATE()) + 1, GETDATE()) -- 取得目前日期，並 - 該月天數 + 1  
    ) AS FirstDay, -- 當月第一天
    CONVERT(DATE, 
        DATEADD(DAY, - DATEPART(DAY, GETDATE()), -- 減掉多餘天數
            DATEADD(MONTH, 1, GETDATE())) -- 取得目前日期，先加一個月
    ) AS LastDay -- 當月最後一天
```

結果為：

| FirstDay | LastDay |
| --- | --- |
| 2024-07-01 | 2024-07-31 |

### 相關語法

- `CONVERT`：可以將表達式轉型成特定格式，在此將 `GETDATE()` 得到的 `DATETIME` 類型資料轉為 `DATE` 類型，以去除時間。
- `DATEADD`：指定單位 (年、月、日、時、分、秒)，`DATETIME` 類型的資料增加特定整數。
- `DATEPART`：取得 `DATETIME` 資料特定單位 (年、月、日、時、分、秒) 的數值。
- `GETDATE`：取得目前的日期與時間。

以下是各語法範例：

```sql
DECLARE @Now AS DATETIME;
SET @Now = GETDATE();
SELECT @Now AS [Now],
    CONVERT(DATE, @Now) AS [Date],
    DATEADD(DAY, 5, @Now) AS [AddDate],
    DATEPART(YEAR, @Now) AS [Year]
```

執行結果如下：

| Now | Date | AddDate | Year |
| --- | --- | --- | --- |
| 2024-07-07 05:33:48.620 | 2024-07-07 | 2024-07-12 05:33:48.620 | 2024 |

### 其他人的方法

- [[SQL] 取得當月的第一天及最後一天 - Ting I 的程式碼集中營 - 點部落](https://dotblogs.com.tw/TingI/2018/01/07/020846) (使用 `DATEDIFF()` 搭配 `DATEADD()`，比較簡潔)
- [使用 getdate() 函數找出本月第一天 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10007971 "‌")