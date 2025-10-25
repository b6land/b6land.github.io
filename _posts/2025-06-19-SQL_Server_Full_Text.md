---
layout: post
title: SQL Server 全文檢索摘要
date: 2025-06-19 23:00:00 +0800
categories: [SQL Server]
--- 

SQL Server 提供全文檢索引擎，建立對應的索引後，可以做到比 `%LIKE%`  更有效率、彈性更高的查詢。

### 查詢指令摘要

在使用全文檢索之前，需要先：

1. 建立全文檢索目錄。
2. 建立全文檢索索引。

完成後，可以用以下的指令查詢資料，摘要如下：

| 語法 | 說明 |
| --- | --- |
| CONTAINS | 用於 WHERE 子句，可以用關鍵字做模糊、精確的全文檢索。可以比對同義字、類似單字、前置詞。 |
| CONTAINSTABLE<br> | 必須用在 FROM 子句中，和 CONTAINS 相同，可以進行精確或模糊搜尋，但多了名次 (Rank) 和全文搜尋的索引鍵。<br> |
| FORMSOF(THESAURUS, '關鍵字')<br> | 表示查詢包含同義字的關鍵字<br> |
| FREETEXTTABLE<br> | 表示包含同義字的關鍵字模糊查詢<br> |

詳細的建立的方式、語句列表與用法，可參考：

- [VITO の 學習筆記: 使用全文檢索](http://vito-note.blogspot.com/2013/06/blog-post_801.html)
- [VITO の 學習筆記: 全文檢索查詢](http://vito-note.blogspot.com/2013/06/blog-post_4715.html)

### 同義字與字典

同義字指的是意思相同 (相近) 卻不同的詞語，查詢時不同的詞語應該要查出一樣 (或相近) 的結果，例如：機場、航空站。

`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\MSSQL\FTData`   內有 `tscht.xml`  的檔案。此檔案是一個 XML 檔，可以用文字編輯器設定繁體中文的同義字。 詳情可參考 [MS SQL 全文檢索搜尋-設定同義字](https://www.tpisoftware.com/tpu/articleDetails/1314)。

由於中文的全文查詢需要依賴字典分詞，如果覺得特定關鍵字的查詢效果不佳，也可以自訂字典，調整分詞以改善查詢效果：[使用自訂字典自訂文字分隔行為 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/search/customize-the-behavior-of-word-breakers-with-a-custom-dictionary?view=sql-server-ver16)

### 應該避免使用的字元

如果查詢時輸入萬用字元 \* 符號，例如 `FORMSOF(THESAURUS, "*")` ，可能會導致語法錯誤或無效的查詢。此外，? 問號和 " 引號可能也會引發語法錯誤。

### 重建索引

如果資料有變動的時候，需要重建全文索引。

語法如下 (或參考微軟 [建立及管理全文檢索目錄 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/search/create-and-manage-full-text-catalogs?view=sql-server-ver16))：

```sql
ALTER FULLTEXT CATALOG catalog_name REBUILD
```

### 官方說明

- [全文檢索搜尋 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/search/full-text-search?view=sql-server-ver16)
- [設定及管理全文檢索搜尋的同義字檔案 - SQL Server - Microsoft Learn](https://learn.microsoft.com/zh-tw/sql/relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search?view=sql-server-ver16&redirectedfrom=MSDN) 

### 其它參考資料

- [Full-text Search. 全文檢索概念 - by RiCo 技術農場 - RiCosNote - Medium](https://medium.com/ricos-note/full-text-search-ae170434907b)
- [全文檢索搜尋 - 曾智義、唐筠](https://www.mis.nsysu.edu.tw/db-book/DBProject2008Fall/dr02/SQL_Server_fulltext_finial_new.pdf )