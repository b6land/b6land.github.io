---
layout: post
title: SQL 取得當月第一天和最後一天
date: 2023-02-23 12:00:00 +0800
categories: [SQL]
---

嘗試在 SQL 中結合 `DATEPART()` 和 `DATEADD()` 方法取得當月第一天和最後一天。

### 作法

```sql
SELECT 
    CONVERT(DATE, -- 轉型成日期 
        DATEADD(DAY, - DATEPART(DAY, GETDATE()) + 1, GETDATE()) -- 取得目前日期，並 - 該月天數 + 1  
    ) AS FirstDay, -- 當月第一天
    CONVERT(DATE, 
        DATEADD(MONTH, 1, -- 加上 1 個月 
            DATEADD(DAY, - DATEPART(DAY, GETDATE()), GETDATE())) -- 取得目前日期，並 - 該月天數
    ) AS LastDay -- 當月最後一天
```

### 其他人的方法

- [[SQL] 取得當月的第一天及最後一天 - Ting I 的程式碼集中營 - 點部落](https://dotblogs.com.tw/TingI/2018/01/07/020846) (使用 `DATEDIFF()` 搭配 `DATEADD()`，比較簡潔)
- [使用 getdate() 函數找出本月第一天 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10007971 "‌")