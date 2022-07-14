---
layout: post
title: SQL Server 的全部取代函數
date: 2022-07-14 12:00:00 +0800
categories: [SQL]
---

SQL Server 沒有提供字串的「全部取代」功能，這篇文章 [A pure T-SQL replace-all function using PATINDEX :: Bart Wolff](https://www.bartwolff.com/Blog/2020/05/03/a-pure-t-sql-replace-all-function-using-patindex) 自行寫了一段程式實作。

### `PATINDEX` 介紹

`PATINDEX` 可以用來找特定 Pattern 發生在什麼位置。

例如微軟官方說明 [PATINDEX (Transact-SQL) - SQL Server - Microsoft Docs](https://docs.microsoft.com/en-us/sql/t-sql/functions/patindex-transact-sql) 的範例：

``` sql
SELECT position = PATINDEX('%ter%', 'interesting data');
```

會得到結果為 3 。

儘管這個指令有以下的限制：
1. 只支援簡單的 Pattern。
2. 只回傳第一個發生的位置。
3. 不回傳符合 Pattern 的內容。

但是 Bart Wolff 還是寫出了以下的全部取代程式，在此處附上自己的註解。

### 用 `PATINDEX` 實作全部取代函數

``` sql
CREATE FUNCTION dbo.fnReplaceAll -- 建立函數
(
    @input          VARCHAR(MAX), -- 輸入字串
    @match_pattern  VARCHAR(MAX), -- 要檢查的 Pattern
    @match_length   INT,          -- Pattern 的長度
    @replace_value  VARCHAR(MAX)  -- 要取代的字串
)
RETURNS VARCHAR(MAX) -- 回傳取代後的字串
BEGIN

    DECLARE @output     VARCHAR(MAX) = '',     -- 宣告輸入、輸出、Pattern 位置等變數
            @input_copy VARCHAR(MAX) = @input,
            @match_ix   INT;

    SET @match_ix = PATINDEX(@match_pattern, @input_copy);  -- 第一個找出符合的 Pattern
    
    WHILE @match_ix > 0 -- 有找到 Pattern 時，持續進入迴圈
    BEGIN

        SET @output = @output + SUBSTRING(@input_copy, 1, @match_ix - 1) + @replace_value; -- 將「取代的子字串 (Pattern 前方的輸入字串和取代值)」，附加到結果變數
        SET @input_copy = SUBSTRING(@input_copy, @match_ix + @match_length, LEN(@input_copy)); -- 輸入 = 「取代的子字串」後方的內容

        SET @match_ix = PATINDEX(@match_pattern, @input_copy); -- 找出符合的 Pattern
    END

    SET @output = @output + @input_copy; -- 最後附加剩下的輸入字串

    RETURN @output;

END
```

可以這樣使用：

``` sql
SELECT dbo.fnReplaceAll('Welcome to Facebook', '%o%', 1, 'oo')
```