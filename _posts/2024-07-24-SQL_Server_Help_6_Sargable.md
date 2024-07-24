---
layout: post
title: SQL Server 效能搶救 (6) 善用索引，使用 SARGAble 改進查詢條件
date: 2024-07-24 14:00:00 +0800
categories: [SQL Server 效能搶救]
--- 

怎麼樣善用索引增加查詢效率呢？很重要的就是使用 SARGAble 的查詢語法！

### 使用索引的提示

- 索引使用到的欄位應盡量的少，可減少 Insert, Update, Delete 和 Merge 指令調整索引導致的效能影響，以及使用的磁碟空間。
- 索引適合建立在常讀取，但不常增修的大量資料表。增進讀取的效能，並減少寫入的效能影響。
- 檢視 (View) 能應用索引加速嗎 ? 如果包含資料表連接 (Join)、彙總函數 (ex. SUM)，使用索引可以增進讀取的效能。
- 盡量一次 (指令) 就完成 Insert 或 Update，減少更新整體索引的次數。
- **使用 SARGAble 的查詢語法**，以有效的利用索引。

### 使用 SARGAble 查詢條件

SARGAble 是 Search ARGument ABLE 的縮寫，意思是「可以用索引尋找」。

SARGAble 運算子包含 `=`, `>`, `<`, `>=`, `<=`, `BETWEEN`, `LIKE`, `IS \[NOT\] NULL`, `IN` 等。
另外還有效果不那麼大的運算子: `<>`, `NOT`, `NOT IN`, `NOT LIKE` ...，因可能導致掃描整個索引，導致和掃描整個資料表沒有差異太多。

使用函數、運算式和 `LIKE` 前方使用 `%` 查詢，不能滿足 SARGAble 的條件，舉例如下：

```sql
WHERE name LIKE '%Alice%'
WHERE SQRT(length) = 5.0
WHERE First + Last = Fullname
```  

查詢條件不滿足 SARGAble，會導致查詢速度變慢，因為：
1. 增加 CPU 計算量。
2. 掃描所有索引。
3. 隱性轉換。
4. 糟糕的基數預測結果。
5. 產生不合適的執行計畫。

### 改善查詢效能的原則

以下列出一些常見的原則：

1. 避免在查詢時呼叫函數 (Function)，減少計算的成本；若在 `WHERE` 條件加入函數，將無法使用索引取得資料。
2. 避免在 `WHERE` 加入 `<>`，因為需要掃描整個索引。
3. 避免在 `LIKE` 條件的一開始使用 `%` 萬用字元，導致掃描整個索引。
4. 連接表格的欄位，應該加入至索引欄位。
5. 減少不必要的排序、聚合動作，因為會增加大量的計算成本。
6. 盡可能使用預存程序，節省重新編譯查詢的時間，以及傳輸 SQL 指令的頻寬。

最後是個人的調整心得與延伸閱讀！[SQL Server 效能搶救 (7) 改進查詢效能的心得、延伸閱讀 – Lazy Coding](/SQL_Server_Help_7_Notice/)

### 參考資料

- 拙作 [SQL Not Equal 的效能影響](/SQL_Not_Equal/) 有提到 `<>` 運算子為何效果較差。
- [Sargable - Wikipedia](https://en.wikipedia.org/wiki/Sargable)
- [Non-SARGable Predicates - Brent Ozar Unlimited®](https://www.brentozar.com/blitzcache/non-sargable-predicates/)
- [Sargability: Why %string% Is Slow - Brent Ozar Unlimited®](https://www.brentozar.com/archive/2010/06/sargable-why-string-is-slow/)
- [SQL指令優化SQL Tuning](https://www.cc.ntu.edu.tw/chinese/epaper/0031/20141220_3109.html)
- [\[SQL\]\[效能調教\]在 SQL 內使用 LIKE 或 Substring 來做字串資料的效能問題 - 五餅二魚工作室 - 點部落](https://dotblogs.com.tw/jamesfu/2017/01/12/Like_and_Substring )