---
layout: post
title: SQL Server 用 Rowversion 紀錄資料列版本
date: 2024-05-15 20:00:00 +0800
categories: [SQL]
--- 

在資料庫內產生可存放二進位數字的 Rowversion 欄位，資料新增或修改時會自動增加，常用來記錄資料的版本。

### 說明

每個資料表只能建立一個 Rowversion 欄位。建立或更新資料列後，該資料列的 Rowversion 欄位的值會增加，如同計數器一樣。

每個資料列的 Rowversion 數值都是唯一的，大小為 8 個 Byte。

這個欄位無法對應到實際的時間，也不適合放入索引內。

### 範例

來進行個簡單的實驗：

```sql
-- 建立含 Rowversion 的欄位
CREATE TABLE MyTest (myKey int PRIMARY KEY, myValue int, RV rowversion);

INSERT INTO MyTest (myKey, myValue) VALUES (1, 0);
INSERT INTO MyTest (myKey, myValue) VALUES (2, 0);

-- 檢查插入資料後的結果
SELECT myKey, myValue, RV FROM MyTest

UPDATE MyTest SET myValue = 1
WHERE myKey = 2

-- 檢查更新資料後的結果
SELECT myKey, myValue, RV FROM MyTest
```

插入資料後的 `SELECT`  結果 (使用 SQL Online IDE 的檢視畫面)：

![插入後第一筆資料](/assets/imgs/2024-05-15/inserted_1.png)  

第一筆，可看到 RV 欄位由 8 個 Byte 組成。

![插入後第二筆資料](/assets/imgs/2024-05-15/inserted_2.png)  

第二筆，可看到和第一筆的 RV 值不同。

接著是更新資料後的結果：

![更新後第一筆資料](/assets/imgs/2024-05-15/updated_1.png)  

第一筆，維持不變。

![更新後第二筆資料](/assets/imgs/2024-05-15/updated_2.png)  

第二筆，因為已經更新，所以最後一個 Byte 的數字增加了。

增加的數字不一定是 1，由 SQL Server 自行決定。

### 參考資料

- 官方說明：[rowversion (Transact-SQL) - Microsoft Learn](https://learn.microsoft.com/zh-tw/previous-versions/sql/sql-server-2008/ms182776(v=sql.100)?redirectedfrom=MSDN)

- 範例：[德瑞克：SQL Server 學習筆記: 認識 資料類型：rowversion，在資料庫內自動產生唯一的二進位數字(1)](http://sharedderrick.blogspot.com/2016/03/rowversion1.html)