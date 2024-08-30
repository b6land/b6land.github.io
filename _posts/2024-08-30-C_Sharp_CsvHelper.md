---
layout: post
title: 在 C# 中讀寫中文 CSV：CsvHelper
date: 2024-08-30 22:00:00 +0800
categories: [C#]
--- 

若要在 C# 中讀寫 CSV (Comma-Separated Values)，可以從 NuGet 安裝 CsvHelper。CsvHelper 在眾多處理 CSV 檔案的函式庫內，是資源豐富、相關文件齊全的選擇。

### 安裝後的使用

我們使用收支作為範例 CSV 檔案：

```
Item,Income,Expense
零用錢,100,0
糖果,0,15
飲料,0,30
```

一開始需要先設定 CSV 的組態：

```csharp
csvConfig = new CsvConfiguration(CultureInfo.InvariantCulture) // 不考慮文化特性
{
    HasHeaderRecord = true, // CSV 包含標頭
    MissingFieldFound = null // 欄位沒有值時要如何處理，這裡指定為 null
};
```

關於 `MissingFieldFound` 屬性的使用，可以參考：[c# - How to configure CsvHelper to skip MissingFieldFound rows - Stack Overflow](https://stackoverflow.com/questions/52589868/how-to-configure-csvhelper-to-skip-missingfieldfound-rows)。

由於 .NET Core 1.0 開始沒有內建的編碼資料，而 CSV 通常以 Big5 編碼儲存，想要讀寫中文資料時，需要自行加入以下程式：

```csharp
Encoding.RegisterProvider(CodePagesEncodingProvider.Instance);
```

接著建立收支資料物件：

```csharp
public class Spending
{
     public string? Item { get; set; }
     public decimal? Income { get; set; }
     public decimal? Expense { get; set; }
}
```

資料物件可以有不同的欄位配對方式，請往下閱讀各[參考資料](#參考資料)。

可以用 `StreamReader` 讀取檔案，並指定編碼為 Big5，接著以 CsvReader 將內容轉為資料物件：

```csharp
string filename = "spending.csv";
using (var reader = new StreamReader(filename, Encoding.GetEncoding("BIG5")))
using (var csv = new CsvReader(reader, csvConfig))
{
    var records = csv.GetRecords<Spending>(); // 取得收支資料
    return records;
}
```

當我們建立或修改收支資料後，可以用 `StreamWriter` 寫入檔案、指定 Big5 編碼，寫入時使用 CsvWriter 將資料物件轉為 CSV 格式：

```csharp
string filename = "spending_edited.csv";
var records = new List<Spending>();
// ... 加入收支資料到 records 列表
using (var writer = new StreamWriter(filename, false, Encoding.GetEncoding("BIG5")))
using (var csv = new CsvWriter(writer, csvConfig))
{
    csv.WriteRecords(records);
}
```

### 參考資料

- 官方教學：[Getting Started - CsvHelper](https://joshclose.github.io/CsvHelper/getting-started/)
- 開啟中文 CSV 的範例：[使用 C# 與 CsvHelper 套件解析《臺北市政府行政機關辦公日曆表》公開資料 - The Will Will Web](https://blog.miniasp.com/post/2022/11/12/Parsing-Taipei-Gov-OpenData-Taiwan-Holiday-using-CsvHelper)
- 多種 CSV 處理函式庫比較：[再談 .NET CSV 解析 - CsvHelper 與其他 38 種程式庫選擇-黑暗執行緒](https://blog.darkthread.net/blog/csvhelper/)
- 簡易的讀寫範例：[\[C#\]\[.NET Core\] CsvHelper : 透過 C# 讀寫 csv 檔案](http://dog0416.blogspot.com/2019/11/aspnet-core-csvhelper-c-csv.html )