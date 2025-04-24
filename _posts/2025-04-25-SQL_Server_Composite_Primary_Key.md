---
layout: post
title: SQL Server 複合主鍵介紹 (Composite Primary Key)
date: 2025-04-25 07:00:00 +0800
categories: [SQL Server]
--- 

通常主鍵 (Primary Key) 只有一個欄位，表示該欄位有獨一無二、用於識別的值。當使用單一欄位無法完整表示資料的唯一性時，就會使用複合主鍵（Composite Primary Key）。

### 介紹

複合主鍵是由兩個或以上的欄位組成，這些欄位的組合值必須在整個資料表中唯一，才能保證每筆資料的辨識性。例如，在「學生選課」的資料表中，可能會使用「學生編號」與「課程編號」作為複合主鍵，避免同一學生對同一課程重複選課。

在 SQL Server 中，複合主鍵的語法需於 `CREATE TABLE`  敘述中使用 `PRIMARY KEY (欄位1, 欄位2, ...)`  定義。例如：

```sql
CREATE TABLE Enrollment (
    StudentID INT,
    CourseID INT,
    EnrollDate DATE,
    PRIMARY KEY (StudentID, CourseID)
);
```

這表示 `StudentID` 與 `CourseID` 的組合需在表中唯一，系統也會自動建立一個唯一索引來強制這個限制。使用複合主鍵可提高資料完整性與查詢效率。


若要在 SQL Server Management Studio (SSMS) 裡設定複合主鍵，可以在建立資料表的設計視窗內，按住 Ctrl 鍵並選擇要建立主鍵的欄位，接著按下滑鼠右鍵選擇「設定主索引鍵」。

![SSMS 用 Ctrl 設定複合主鍵](/assets/imgs/2025-04-25/ssms_composite_primary_key.png)<br>

### 參考資料

- [Primary key on two columns sql server](https://www.pragimtech.com/blog/sql-optimization/primary-key-on-two-columns-sql-server/)
- [菜鳥工程師 肉豬: SQL Server SSMS 設定複合主鍵(compound primary key)](https://matthung0807.blogspot.com/2018/06/sql-server-ssms-compound-primary-key.html)
- ChatGPT