---
layout: post
title: C# URL Encoding 編碼後傳送 Request
date: 2024-02-28 12:00:00 +0800
categories:  [C#]
--- 

如果要在 POST 的請求中使用 `application/x-www-form-urlencoded` 的 MIME 格式送出表單，並傳送英文以外的字元，需要先將非 8 位元字元的資料編碼後再傳送，以正確的傳送至伺服器端。

### 概念

這種傳送請求的方式，會將表單內容轉為 URL 編碼，而 URL 編碼只能接受標準 ASCII 字元，即 8 位元的保留字和非保留字，因此需要對資料重新編碼。

可以使用 Percent-Encoding 的編碼方式，Percent-Encoding 會將資料轉為 UTF-8，格式為一個百分號和後面的兩個 16 進位數字組成，例如台北為 %E5%8F%B0%E5%8C%97。如果需要自行改用其它的編碼，如 Big-5 的話，則百分比後方會有兩個以上的字元，例如台北會轉換成 %a5x%a5\_。

### 程式碼

在 C# 中可以使用 `HttpUtility.UrlEncoding()` 方法，以下是將繁體中文以 Big-5 編碼的範例：

```cs
string cityName = "台北";
Encoding encodingBig5 = Encoding.GetEncoding("big5"); // 查詢 Big-5 編碼

string pram = $"command=test&city={HttpUtility.UrlEncode(cityName, encodingBig5)}}"; // 如果不加入第二個編碼參數，預設是 UTF-8
byte[] bs = Encoding.ASCII.GetBytes(pram);
var httpWebRequest = (HttpWebRequest)WebRequest.Create("https://xxx.com");
httpWebRequest.ContentType = "application/x-www-form-urlencoded;charset=big5";
httpWebRequest.Method = "POST";
httpWebRequest.Timeout = 1000;
httpWebRequest.ContentLength = bs.Length;

using (Stream reqStream = httpWebRequest.GetRequestStream())
{
    reqStream.Write(bs, 0, bs.Length);
}
var httpResponse = (HttpWebResponse)httpWebRequest.GetResponse();
```

### 參考資料

- [【筆記】關於 URL Encoding - 學徒工坊 - 點部落](https://dotblogs.com.tw/apprenticeworkshop/2018/12/09/About-URL-Encoding)  
- [C#中HttpWebRequest的用法详解（转载） - PowerCoder - 博客园](https://www.cnblogs.com/OpenCoder/p/9880516.html)  
- [拾捌。淺談幾種 POST request 的格式 - iT 邦幫忙](https://ithelp.ithome.com.tw/articles/10293658)
- [HttpUtility.UrlEncode 方法 (System.Web) - Microsoft Learn](https://learn.microsoft.com/zh-tw/dotnet/api/system.web.httputility.urlencode?view=net-8.0)