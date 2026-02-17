---
layout: post
title: SQL Server 的 Transaction 簡介
date: 2025-10-21 23:00:00 +0800
categories: [SQL Server]
--- 

這篇文章會介紹 SQL Server 裡 Transaction 的特性，以及語法的使用方式。

### 什麼是 Transaction (交易)

需要用多組語法完成一件事時，適合搭配 Transaction 語法保持完整性。

例如今天增加一個會員，需要寫入「會員資料」、「會員異動紀錄」兩個資料表，如果「會員資料」增加會員失敗，那麼「會員異動紀錄」希望不要留下紀錄。

但是沒有明確標示 Transaction 語法時，每段 SQL 語法都被視為獨立的 Transaction，就算上一段失敗了，還是會繼續執行下一段 SQL 語法。以上方例子來說，如果會員資料寫入失敗了，仍會寫入紀錄資料表。

這時可以明確標示 Transaction 語法，若其中一段語法執行失敗時，強制恢復至變更前的狀態。

### Tranaction 語法說明

以下是相關 SQL 語法：

```sql
BEGIN TRAN; -- 啟動交易
-- 要執行的 SQL 語法
COMMIT TRAN; -- 確認交易完成
-- 如果判斷 SQL 語法沒有完整執行 (例如發生錯誤)
ROLLBACK TRAN; -- 取消交易
```

### 範例

建立範例資料表：

```sql
CREATE TABLE Member (
    ID INT PRIMARY KEY,
    Name NVARCHAR(50) NOT NULL,
    Phone NVARCHAR(20) NOT NULL
);

CREATE TABLE MemberChangeLog (
    MID INT NOT NULL,
    JoinDate DATETIME NOT NULL,
    FOREIGN KEY (MID) REFERENCES Member(ID)
);
```

測試語法：

```sql
BEGIN TRY
    BEGIN TRANSACTION;

    -- 新增會員資料
    INSERT INTO Member (ID, Name, Phone)
    VALUES (1, N'王小明', '0912345678'); -- 測試正常的狀況
    -- VALUES (1, N'王小明', null); -- 測試有錯誤，恢復變更的狀況

    -- 新增會員異動紀錄
    INSERT INTO MemberChangeLog (MID, JoinDate)
    VALUES (1, GETDATE());

    -- 兩者都成功才提交
    COMMIT TRANSACTION;
END TRY
BEGIN CATCH
    -- 若任一語句失敗，恢復所有變更
    ROLLBACK TRANSACTION;

    -- 可視需求回傳錯誤訊息
    PRINT '發生錯誤：' + ERROR_MESSAGE();
END CATCH;
```

補充說明：

- `BEGIN TRY...BEGIN CATCH`：用於捕捉執行錯誤，若有錯誤會自動進入 `CATCH` 區塊。
- `ERROR_MESSAGE()`：可回傳 SQL Server 內部錯誤文字，方便除錯與紀錄。

### 相關的議題：Lock 和 Isolation Level

- 在 Transaction **修改資料**的期間，會請求一個 Exclusive Lock (互斥鎖)，避免其它 Transaction 修改該列資料；但是能不能讀取該筆資料，則取決於 Isolaction Level (隔離層級)。
- 在 Transaction **讀取資料**的期間，會請求一個 Shared Lock (共享鎖)，避免其它 Transaction 修改該列資料，多個 Transaction 可以有相同資源的 Shared Lock。
- 在 Transaction **更新資料**的期間，Update Lock 可以提供以列範圍的鎖定，比 Exclusive Lock 的鎖定範圍小，以避免大量鎖定導致死結 (Deadlock) 的發生，在同一資源下只能有一個 Transaction 的 Update Lock。若此時執行搭配 `READPAST` 提示的查詢語法，可以取得未鎖定的資料，避免讀取到未 COMMIT 的修改。

隔離層級由低到高有以下幾種：

1. READ UNCOMMITTED：可以讀取到尚未 COMMIT 的修改。
2. READ COMMITTED (預設)：要等到 COMMIT 完成後才會讀取。
3. REPEATABLE READ：多次讀取間，不能對資料做 COMMIT。
4. SERIALIZABLE：多次讀取間，避免加入未來的新資料列 COMMIT。

### Transaction 參考資料

- [SQL Server Transaction - GeeksforGeeks](https://www.geeksforgeeks.org/sql-server/sql-server-transaction/)
- [\[筆記\]\[MSSQL\]關於SQL的交易概念 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10190252)
- [T-SQL TRANSACTION - 巢狀交易原來是要這樣思考的阿！？ - 小馬彬的部落格](https://littlehorseboy.github.io/2020/07/05/202007-t-sql-save-tran/)
- Reddit 鄉民簡單說明 Transaction 的重要性：[Database/sql 或 sqlx：我應該全部都用 transaction 嗎？還是不用？ : r/golang](https://www.reddit.com/r/golang/comments/a85ex4/databasesql_or_sqlx_should_i_use_transactions_for/?tl=zh-hant)

### Lock 和 Isolation Level 參考資料

- [Transaction locking and row versioning guide - SQL Server - Microsoft Learn](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide?view=sql-server-ver17)
- [SQL Server-聚焦事务、隔离级别详解（二十九） - Jeffcky - 博客园](https://www.cnblogs.com/CreateMyself/p/6352167.html)
- [SQL Server-聚焦深入理解死锁以及避免死锁建议（三十三） - Jeffcky - 博客园](https://www.cnblogs.com/CreateMyself/p/6362904.html )
- [SQL Server-聚焦NOLOCK、UPDLOCK、HOLDLOCK、READPAST你弄懂多少？（三十四） - Jeffcky - 博客园](https://www.cnblogs.com/CreateMyself/p/6512692.html )
- [技術事件薄: SQL LOCK的情境介紹](https://joyceevent.blogspot.com/2021/01/sql-lock.html )
- [高併發系統系列-不得不了解的Isolation Level(by 錢包被扣到變負值) - 石頭的coding之路](https://isdaniel.github.io/db-isolation/ )