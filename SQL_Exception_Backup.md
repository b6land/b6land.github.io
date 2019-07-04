### 查詢 SQL 容量錯誤並備份資料庫

由於預期在 SQL Server Express 會遇到超過容量上限的問題，因此可以先找到對應的錯誤代碼，並從 C# 中檢查是否有該錯誤的存在。

參考資料的「Database Engine Errors - SQL Server」 列出了 SQL 的錯誤，以及相對應的描述 (和解決方法)。

從該網頁可以預先找到錯誤代碼 1105：SQL Express 容量限制達到時發出的錯誤。下一步需要能從 C# 檢查錯誤代號。

C# 執行 SQL 語法查詢，如 **SqlCommand** 類別的的 `ExecuteNonQuery()` 或 **SqlDataReader** 的 `ExecuteReader()` 時，可能會遇到產生 SqlException 的狀態。此時可以查詢 `SqlException.Number`，即對應的 SQL 錯誤代碼。

當 `SqlException.Number` 為 1105 時，表示 SQL Server Express 已超過容量限制，或是磁碟空間不足也可能造成這個問題。

此時可以 Script 備份再將資料移除掉。

```
BACKUP DATABASE test TO DISK = 'C:\test.bak'
GO
```

### Bonus: Order By 帶來的效能影響

當使用叢集索引時，使用 `Select Top Order By` 指令，包含以下的幾種情況：

- 選擇單一欄位遞增排序取 TOP 10 筆資料時不會佔用高 CPU 使用率，不會掃描完整張資料表。
- 選擇單一欄位遞減排序取 TOP 10，也不會佔用高 CPU 使用率。
- 若選擇**複數欄位排序**取得 TOP 10，將會占用高 CPU 使用率和 IO，**掃描完整張資料表**。若該資料表的異動次數不高，可建立非叢集索引，以減少 CPU 使用率和 IO 次數。

### 參考資料

[SqlException Class (System.Data.SqlClient) | Microsoft Docs](https://docs.microsoft.com/zh-tw/dotnet/api/system.data.sqlclient.sqlexception?view=netframework-4.8)

[Database Engine Errors - SQL Server | Microsoft Docs](https://docs.microsoft.com/zh-tw/sql/relational-databases/errors-events/database-engine-events-and-errors?view=sql-server-2017)

[[SQL Server]使用T-SQL來備份與還原資料庫 | F6 Team - 點部落](https://dotblogs.com.tw/puma/2009/10/14/sqlserver-tsql-backup-resotre-db)

[建立合適的索引，來降低Select Top Order By帶來的效能問題 | Rock的SQL筆記本 - 點部落](https://dotblogs.com.tw/rockchang/2016/10/02/111338)