---
layout: post
title: 字串壓縮
date: 2023-10-03 12:00:00 +0800
categories: [C#]
---

### 字串壓縮

- 將字串壓縮，將以 Bytes 陣列呈現。
[Compression/Decompression string with C# - Stack Overflow](https://stackoverflow.com/questions/7343465/compression-decompression-string-with-c-sharp)
- 將 Bytes 陣列轉為字串，以顯示在螢幕上。
[我的記憶: string和byte[]的轉換 (C#)](https://isanhsu.blogspot.com/2012/03/stringbyte-c.html)
- 將字串壓縮，並轉為 Base64 字串。
[Jax 的工作紀錄: C# 用 gzip 壓縮字串並取得 base64 字串](https://jax-work-archive.blogspot.com/2019/07/c-gzip-base64.html)

### 作法

1. 呼叫 `MemoryStream` 處理字串的輸出入。
2. 呼叫 `GZipStream` 壓縮字串。
3. 使用 `Convert.ToBase64String()` 或 `bytes[i].ToString("X2")` 將壓縮後的 bytes 陣列轉換為字串。
4. 解壓縮時，呼叫 `GZipStream` 解壓縮字串，並使用 `Encoding.ASCII.GetString` 將解壓縮的 bytes 字串轉為原本的字串。

### 缺點

- 這兩種方法都沒辦法接受中文的壓縮 / 解壓縮。
- 如果要壓縮的文字重複率不高，那麼效果就會不好。
