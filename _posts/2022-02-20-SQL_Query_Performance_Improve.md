---
layout: post
title: SQL 改善查詢效能
date: 2022-02-20 12:00:00 +0800
categories: SQL
---

本篇記錄當遇到查詢速度過慢時，如何檢查效能瓶頸與改善。

### 檢查的方式

1. 使用 **SQL Server Management Studio (SSMS)** 建立查詢語法。在查詢時加入實際執行計畫。檢查執行計畫中成本最高的幾個查詢語法。
2. 如果插入表格的成本過高 (如高 CPU 使用率、高 IO 次數或長執行時間)，檢查是否資料筆數過高，或資料欄位的長度過長。
3. 其他檢查的方向：
a. 純量計算：檢查是否有過度的使用 `GROUP BY` ，而且 `GROUP BY` 的欄位沒有建立索引。對實體資料表建立索引後，使用 `GROUP BY` 的效率會提高。
b. 使用 `LIKE %` ：同上，沒有建立索引的狀況下，會大幅增加查詢的時間。
4. 當執行計畫過大時 (SSMS 沒辦法完全載入時)，可以先另存計畫，並使用 **Sentry Plan Explorer** 檢查大量的執行計畫。Sentry Plan Explorer 是一項免費工具，可以快速的載入大型的執行計畫，並且提供查詢中各項語法依欄位排序的功能，可用來找出最耗費成本的幾項查詢語法。

### 效能改善的方式

- 延後 Join 的時機，只在需要時加入 Join 資料表，避免加入過量資料並進行計算（如排序等 CPU 成本）、插入資料表（增加讀寫成本）。
- 先透過條件式篩選，減少資料筆數以後，再加入關聯的資料欄位，同樣也是避免計算過量資料。

### 參考資料

- [SolarWinds(SentryOne) Plan Explorer](https://www.sentryone.com/plan-explorer)
- [[Tool]免費查看SQL PLAN的工具 - SQL Sentry Plan Explorer - 亂馬客 - 點部落](https://dotblogs.com.tw/rainmaker/2012/07/27/73659)
- [~楓花雪岳~: [SQL] 觀看執行計畫重點](http://jengting.blogspot.com/2013/12/executionplan-keypoint.html)
- [[SQL][效能調教]在 SQL 內使用 LIKE 或 Substring 來做字串資料的效能問題 - 五餅二魚工作室 - 點部落](https://dotblogs.com.tw/jamesfu/2017/01/12/Like_and_Substring)
- [SQL指令優化SQL Tuning](https://www.cc.ntu.edu.tw/chinese/epaper/0031/20141220_3109.html)