---
layout: post
title: SQL 的鎖定 (Lock) 和死結 (Deadlock)
date: 2023-02-19 12:00:00 +0800
categories: [SQL Server]
---

死結 (Deadlock) 的發生與資料庫的鎖定 (Lock) 有關，這篇文章將介紹資料庫為什麼需要鎖定、MS SQL 的鎖定擴大 (Lock Escalation) 機制，以及減少死結的方法。

### 鎖定與鎖定擴大

MS SQL 資料庫在存取時，會執行資料**鎖定**。為什麼要鎖定資料 ? 主要是避免以下情形:

1. 避免兩個更新作業同時進行，導致其中一方的更新資料遺失的問題。
2. 不要讀取到未被認可 (Commit) 的資料。
3. 避免讀取過的資料已經被修改過。
4. 當同一次交易 (Transcation) 重複多次選取同樣資料時，避免每次的選取有不一樣的情形。

鎖定的層級有許多種，例如層級較低的 Row Lock (資料列鎖定)、Page Lock (資料頁鎖定)，層級較高的 Table Lock (資料表鎖定) 等。

鎖定擴大 (Lock Escalation) 則是將多個較低層級的鎖定升級至較高的鎖定，以節省記憶體空間 (因為每個鎖定都要額外耗費記憶體)。不過由於一口氣鎖定的更多的資料，較容易造成在其他程式存取時，發生阻擋的現象。

當二或多個程式互相等待鎖定釋放時，就會造成**死結**，這時會導致程式逾時，而且程式中的 CRUD 的動作也無法完成。

### 死結的避免

要減少死結狀況，常見的做法是避免同時操作相同的資料表，特別是大量的更新/插入/刪除資料，避免發生鎖定擴大現象，可參考拙作 [SQL 的鎖定擴大 (Lock Escalation)](/SQL_Lock_Escalation/)。

此外，也有以下幾種方法可以減少死結的發生：

1. 在不同的程式中，以相同的順序存取資料。
2. 避免在交易中的使用者互動。
3. 保持批次作業中的交易簡短，避免批次同時作業時，互相阻擋其他交易的動作。
4. 使用較低的隔離層級 (Isolation Level)。
5. 使用基於資料列的隔離層級。
6. 使用 Bound Connection。

### 參考資料

- [Minimizing Deadlocks - Microsoft Learn](https://learn.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms191242(v=sql.105))
- [Deadlocks in SQL Server — part1: What the locks are? - by Alexander Solovey - ITA Labs - Medium](https://medium.com/italabs/sql-server-deadlocks-part1-b9926aec779a)
- [SQL Server 的一些眉眉角角 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10199398[MSSQL)
- [[SQL SERVER][Performance]淺談鎖定擴大 - RiCo技術農場 - 點部落](https://dotblogs.com.tw/ricochen/2012/02/17/69565)