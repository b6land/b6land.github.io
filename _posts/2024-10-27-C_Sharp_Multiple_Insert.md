---
layout: post
title: C# 有效率的插入多筆資料至 SQL Server 資料庫
date: 2024-10-27 21:00:00 +0800
categories: [C#]
--- 

有效率插入多筆資料到 SQL Server 的方法，可以分為：用 Bulk Insert 一次插入一個資料表，以及在同一個交易內使用多個 `INSERT INTO` 語法。以下提到的是如何使用多個 `INSERT INTO` 來插入資料。

### 作法

假設建立以下的資料表：

```sql
CREATE TABLE Entries (
  ID INT,
  Name NVARCHAR(100)
);
```

如果每次執行語法都只插入一筆資料，那麼會額外負擔多次連線至資料庫的時間：

```sql
第 1 次:
INSERT INTO Entries (id, name) VALUES (1, N'小王');

第 2 次:
INSERT INTO Entries (id, name) VALUES (2, N'小明');

第 3 次:
INSERT INTO Entries (id, name) VALUES (3, N'小華');
```

用以下的語法，則會有比較高的執行效率：

```sql
第 1 次:
INSERT INTO Entries (id, name) VALUES (1, N'小王');
INSERT INTO Entries (id, name) VALUES (2, N'小明');
INSERT INTO Entries (id, name) VALUES (3, N'小華');
```

還可以再簡化成：

```sql
INSERT INTO Entries (id, name) VALUES (1, N'小王'),(2, N'小明'),(3, N'小華');
```

因此我們可以使用以下的 C# 語法產生多筆 INSERT INTO 語法：

```csharp
using System;
using System.Data.SqlClient;
using System.Text;

class Program
{
    static void Main()
    {
        // 設定資料庫連線字串
        string connectionString = "your_connection_string_here"; // 自行替換

        // 準備要插入的資料
        var entries = new (int Id, string Name)[]
        {
            (1, "小王"),
            (2, "小明"),
            (3, "小華")
        };

        // 建立一次插入的 SQL 語法
        StringBuilder sql = new StringBuilder("INSERT INTO Entries (id, name) VALUES ");

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            SqlCommand command = new SqlCommand();
            command.Connection = connection;

            // 使用迴圈來組合參數化的 VALUES 子句
            for (int i = 0; i < entries.Length; i++)
            {
                var entry = entries[i];
                string paramId = $"@Id{i}";
                string paramName = $"@Name{i}";

                // 加入 VALUES 子句，處理逗號
                sql.Append($"({paramId}, {paramName})");
                if (i < entries.Length - 1)
                {
                    sql.Append(", ");
                }

                // 加入參數值
                command.Parameters.AddWithValue(paramId, entry.Id);
                command.Parameters.AddWithValue(paramName, entry.Name);
            }
            command.CommandText = sql.ToString();

            try
            {
                // 開啟連線並執行插入指令
                connection.Open();
                int rowsAffected = command.ExecuteNonQuery();
                Console.WriteLine($"{rowsAffected} rows inserted successfully.");
            }
            catch (Exception ex)
            {
                Console.WriteLine("An error occurred: " + ex.Message);
            }
        }
    }
}
```

不過要留意，組合出多個 `INSERT INTO` 語法時，要小心語法長度不要超過 SQL Server 的查詢語法長度上限，是 65,536 \* 網路封包大小 (封包大小預設是每個 4KB)。

### 參考資料

- 改善插入多筆資料效率：[c# - How should I multiple insert multiple records? - Stack Overflow](https://stackoverflow.com/questions/2972974/how-should-i-multiple-insert-multiple-records)
- 查詢語法上限：[Maximum size for a SQL Server Query? IN clause? Is there a Better Approach - Stack Overflow](https://stackoverflow.com/questions/1869753/maximum-size-for-a-sql-server-query-in-clause-is-there-a-better-approach)
- Bulk Insert 的效能改善效果：[\[Azure\]\[SQL\]改善大量資料塞入效能低弱的案例分享 - 五餅二魚工作室 - 點部落](https://dotblogs.com.tw/jamesfu/2023/02/19/DataInsertAzureSqlDatabase)