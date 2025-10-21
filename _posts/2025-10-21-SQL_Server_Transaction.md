---
layout: post
title: SQL Server 的 Transaction 簡介
date: 2025-10-21 23:00:00 +0800
categories: [SQL Server]
--- 

這篇文章會介紹 SQL Server 裡 Transaction 的特性，以及語法的使用方式。

### 什麼是 Transaction (交易)

需要用多組語法完成一件事時，適合用 Transaction 保持完整性。

例如今天增加一個會員，需要寫入「會員資料」、「會員異動紀錄」兩個資料表，如果「會員資料」增加會員失敗，那麼「會員異動紀錄」希望不要留下紀錄。

但是執行 SQL 語法時，就算上一段失敗了，還是會繼續執行下一段語法。以上方例子來說，仍會寫入紀錄資料表。

這時可以透過 Transaction，若其中一段語法執行失敗時，強制恢復至變更前的狀態。

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

### 參考資料

- [\[筆記\]\[MSSQL\]關於SQL的交易概念 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10190252)
- [T-SQL TRANSACTION - 巢狀交易原來是要這樣思考的阿！？ - 小馬彬的部落格](https://littlehorseboy.github.io/2020/07/05/202007-t-sql-save-tran/)
- Reddit 鄉民簡單說明 Transaction 的重要性：[Database/sql 或 sqlx：我應該全部都用 transaction 嗎？還是不用？ : r/golang](https://www.reddit.com/r/golang/comments/a85ex4/databasesql_or_sqlx_should_i_use_transactions_for/?tl=zh-hant)