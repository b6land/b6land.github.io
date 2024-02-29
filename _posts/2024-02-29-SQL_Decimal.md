---
layout: post
title: SQL Decimal 類型筆記
date: 2024-02-29 12:00:00 +0800
categories:  [C#]
--- 

SQL 的 decimal 可以用來儲存特定位數的整數與小數。與 float 不同的地方，在於 decimal 儲存精確的數字，而 float 則採取二進位儲存的方式，因此僅能表現出近似值。

### 類型說明

可以表示成以下形式：

decimal \[(p \[,s\])\]

- p 為精確度 (Precision) ，表示數值位數，含小數點前後的位數。
- s 為刻度 (Scale)，表示小數點後的位數。

decimal 和 numeric 在 MS-SQL 中，功能是完全相同的。

### Bonus: 轉換 numeric 到資料類型 numeric 時發生算術溢位錯誤

若在執行 SQL Script 時發生這項錯誤，可能是因為整數或小數的長度設定錯誤導致的，例如欄位定義為 decimal \[9,6\] 時：

- 123.456789 可以正常輸入
- 123456789 會發生錯誤
- 0.123456789 會儲存為 0.123456

因此要擴充欄位的長度，或檢查資料是否輸入正確，才能解決此問題。

### 參考資料

- [Understanding the SQL Decimal data type](https://www.sqlshack.com/understanding-sql-decimal-data-type/)
- [浮點數計算結果更接近正解？-黑暗執行緒](https://blog.darkthread.net/blog/float-vs-decimal-case/)