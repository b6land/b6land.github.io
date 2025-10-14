---
layout: post
title: SQL Server 查詢、中止長時間執行的 DDL/DML 語法
date: 2025-10-14 22:30:00 +0800
categories: [SQL Server]
--- 

如果不小心執行了需要很長時間處理的 SQL 語法，可以透過以下的語法查詢進度或終止。

### 查詢 DDL/DML 進度的語法

(DDL：Data Definition Language，資料定義語言；DML：Data Manipulation Language，資料操作語言)

```sql
SELECT 
    r.session_id,
    r.command,
    r.status,
    r.percent_complete,
    r.start_time,
    r.total_elapsed_time/1000 AS elapsed_seconds,
    r.estimated_completion_time/1000 AS est_seconds_left,
    t.text AS sql_text
FROM sys.dm_exec_requests r
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t
```

- `session_id` → ID。
- `command` → 執行的 SQL 指令類型。 
- `status`  → 執行狀態。
- `sql_text` → 正在執行的 SQL，可以確認你的 SQL 語法。

以下都只是估算值，僅能協助判斷是否要中止。

- `percent_complete` → 目前完成百分比（0~100）。
- `estimated_completion_time` → 剩餘時間（毫秒）。
- `total_elapsed_time` → 已經執行時間（毫秒）。

### 中止執行中的 DDL/DML


```sql
-- 假設要終止 session_id = 75
KILL 75;
```

要注意以下事項：

- `KILL` 不是「馬上結束」，它會觸發 rollback。
- Rollback 時間大致 ≈ 該交易已經跑的時間，例如 `ALTER TABLE`  已經跑 2 小時，rollback 也可能要 1~2 小時。
- 若原本資料表已經被鎖定，在 rollback 過程中，該資料表仍然會被鎖定。