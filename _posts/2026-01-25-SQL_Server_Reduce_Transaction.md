---
layout: post
title: SQL Server 壓縮交易紀錄檔案容量
date: 2026-01-25 17:00:00 +0800
categories: [SQL Server]
--- 

如果很常對資料庫進行新增、修改和刪除，那麼可能會產生大量的交易紀錄，可以在 SSMS 用以下的步驟壓縮交易紀錄，減少占用磁碟的空間。

### 步驟

在資料庫上點右鍵，選擇「報表>檢視報表>磁碟使用量」，可以從這個報表檢查目前的交易紀錄檔案用量。

![查詢交易紀錄檔案用量](/assets/imgs/2026-01-25/SQLServer_Transaction_1.png)

可以看到目前交易紀錄占用的空間。

![報表中的交易紀錄檔案用量](/assets/imgs/2026-01-25/SQLServer_Transaction_2.png)

確認後，可以在資料庫上點右鍵，選擇「工作>壓縮>檔案」。

![壓縮交易紀錄](/assets/imgs/2026-01-25/SQLServer_Transaction_3.png)

接著會進入壓縮檔案的視窗，要在檔案類型選擇「記錄檔」，並選擇要壓縮的檔案名稱 (通常只有一個紀錄檔)，然後選擇壓縮動作。

這裡是選擇「釋放未使用的空間之前，先重新組織頁面」，並輸入指定的大小 (不過 SQL Server 仍會保留部分交易紀錄，不一定會壓縮到指定大小)。

![壓縮交易紀錄對話視窗](/assets/imgs/2026-01-25/SQLServer_Transaction_4.png)

按下確定後即進行壓縮作業，可以重新整理「磁碟使用量」報表檢查壓縮的效果。

### 參考資料

- [清除SQL Server Log檔 (交易紀錄) - HackMD](https://hackmd.io/@Not/H1YBRk6qw)
- [SQL Server LDF 容量大爆炸 (SQL Server ldf truncate & shrink) - The Skeptical Software Engineer](https://sdwh.dev/posts/2021/03/SQL-Server-LDF-Truncate-Shrink/)