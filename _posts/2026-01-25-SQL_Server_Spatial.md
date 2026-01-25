---
layout: post
title: SQL Server 查詢座標是否在特定地理範圍內
date: 2026-01-25 15:00:00 +0800
categories: [SQL Server]
--- 

在 SQL Server 裡面，可以查詢座標是否在特定的地理範圍內，請看後方的介紹。

### 基礎知識

- WKB (well-known binary)：二進位儲存地理資訊，SQL Server 的 Geography 類型欄位裡存放的格式。
- WKT (well-known text)：以文字表示地理資訊，例如 `POINT (121.5 25.0)` 。

### 語法

```sql
DECLARE @g AS geography = geography::STGeomFromText('POINT(121.012345 25.541230)',4326).MakeValid(); -- 將 WKT 的座標轉為 WKB 格式，並用 MakeValid 確保座標合理
SELECT * -- 形狀相關資料欄位
FROM xxx -- 儲存形狀的資料表
WHERE [Shape].STIntersects(@g)=1 -- 是否落在 Shape 欄位的形狀內
```

#### 參考資料

- 基礎介紹：[\[番外篇\] MSSQL Spatial 地理空間資訊查詢 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10244022)
- Point 資料類型介紹：[Point - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/spatial/point?view=sql-server-ver17)
- 檢查兩個點、線、形狀是否有交錯或包含在內：[STIntersects (geometry 資料類型) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/spatial-geometry/stintersects-geometry-data-type?view=sql-server-ver17)
- 將 WKT 格式資料轉為 WKB 格式： [STGeomFromText （geography 數據類型） - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/spatial-geography/stgeomfromtext-geography-data-type?view=sql-server-ver17)

### 解決錯誤

發生以下的錯誤時，有兩個可能的原因：

```plaintext
錯誤訊息如下：
Msg 6522, Level 16, State 1, Line 2 
執行使用者自訂常式或彙總 "geography" 時，發生 .NET Framework 錯誤: System.ArgumentException: 24144: 無法完成這項作業，因為例項無效。請使用 MakeValid 將例項轉換成有效的例項。注意，MakeValid 可能導致幾何例項的點稍微偏移。System.ArgumentException:。
```

1. 查詢的 POINT 資料不正確，或沒有用 MakeValid 方法轉換。
2. 查詢的形狀資料有錯誤。可以修正查詢條件：

```sql
DECLARE @g AS geography = geography::STGeomFromText('POINT(121.012345 25.541230)',4326).MakeValid(); 
SELECT * 
FROM xxx 
WHERE Shape.STIsValid() = 1 -- 資料必須也是合理的形狀
  AND [Shape].STIntersects(@g)=1 
```

#### 參考資料

- ChatGPT
- [MakeValid (geometry 資料類型) - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/t-sql/spatial-geometry/makevalid-geometry-data-type?view=sql-server-ver17)